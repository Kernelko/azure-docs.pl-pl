---
title: Resetowanie usług pulpitu zdalnego lub jego hasło administratora na maszynie wirtualnej Windows | Dokumentacja firmy Microsoft
description: Dowiedz się, jak zresetować hasło do konta lub usług pulpitu zdalnego na maszynie Wirtualnej Windows przy użyciu witryny Azure portal lub programu Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 470834da9e9a571594789dedcbb0a67b55abd799
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953716"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Resetowanie usług pulpitu zdalnego lub jego hasło administratora na maszynie wirtualnej Windows
Jeśli nie możesz połączyć z maszyną wirtualną (VM) Windows, możesz zresetować hasło administratora lokalnego lub zresetowanie konfiguracji usług pulpitu zdalnego (nie obsługiwany na kontrolerach domeny Windows). Aby zresetować hasło, można użyć witryny Azure portal lub rozszerzenie dostępu do maszyny Wirtualnej w programie Azure PowerShell. Po zalogowaniu się do maszyny Wirtualnej, należy zresetować hasło dla tego administratora lokalnego.  
Jeśli używasz programu PowerShell, upewnij się, że masz [najnowszy moduł programu PowerShell, zainstalować i skonfigurować](/powershell/azure/overview) i zalogowano się do subskrypcji platformy Azure. Możesz również [wykonać te kroki dla maszyn wirtualnych utworzonych za pomocą klasycznego modelu wdrażania](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

Możesz zresetować usług pulpitu zdalnego i poświadczenia w następujący sposób:

- [Zresetuj przy użyciu witryny Azure portal](#reset-by-using-the-azure-portal)
- [Resetuj przy użyciu programu PowerShell i rozszerzenie VMAccess](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Zresetuj przy użyciu witryny Azure portal
Zaloguj się do witryny Azure portal, a następnie wybierz pozycję **maszyn wirtualnych** w menu po lewej stronie.

### <a name="reset-the-local-administrator-account-password"></a>**Zresetuj hasło konta administratora lokalnego**

Wybierz maszynę Wirtualną Windows, a następnie wybierz pozycję **Resetuj hasło** w obszarze **pomoc techniczna i rozwiązywanie problemów**. **Resetuj hasło** zostanie wyświetlone okno.

Wybierz **Resetuj hasło**, wprowadź nazwę użytkownika i hasło, a następnie wybierz **aktualizacji**. Spróbuj ponownie nawiązać połączenie z maszyną wirtualną.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Zresetowanie konfiguracji usług pulpitu zdalnego**

Wybierz maszynę Wirtualną Windows, a następnie wybierz pozycję **Resetuj hasło** w obszarze **pomoc techniczna i rozwiązywanie problemów**. **Resetuj hasło** zostanie wyświetlone okno. 

Wybierz **tylko konfiguracji resetowania** , a następnie wybierz **aktualizacji**. Spróbuj ponownie nawiązać połączenie z maszyną wirtualną.


## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>Resetuj przy użyciu programu PowerShell i rozszerzenie VMAccess
Upewnij się, że masz [najnowszy moduł programu PowerShell, zainstalować i skonfigurować](/powershell/azure/overview) i zalogowano się w Twojej subskrypcji platformy Azure przy użyciu [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) polecenia cmdlet.

### <a name="reset-the-local-administrator-account-password"></a>**Zresetuj hasło konta administratora lokalnego**
Resetuj nazwy użytkownika lub hasło administratora przy użyciu [AzureRmVMAccessExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmaccessextension) polecenia cmdlet programu PowerShell. `typeHandlerVersion` Ustawienie musi być 2.0 lub nowszej, ponieważ wersja 1 jest przestarzały. 

```powershell
$SubID = "<SUBSCRIPTION ID>" 
$RgName = "<RESOURCE GROUP NAME>" 
$VmName = "<VM NAME>" 
$Location = "<LOCATION>" 
 
Connect-AzureRmAccount 
Select-AzureRMSubscription -SubscriptionId $SubID 
Set-AzureRmVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
```

> [!NOTE] 
> Jeśli wprowadzasz nazwę inną niż bieżące konto administratora lokalnego na maszynie Wirtualnej, rozszerzenie VMAccess dodać konto administratora lokalnego z tą nazwą i przypisać określonego hasła do tego konta. Jeśli konto administratora lokalnego na maszynie Wirtualnej istnieje, rozszerzenie VMAccess spowoduje zresetowanie hasła. Jeśli konto jest wyłączone, spowoduje włączenie rozszerzenia VMAccess.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Zresetowanie konfiguracji usług pulpitu zdalnego**
Zresetuj dostęp zdalny do maszyny Wirtualnej za pomocą [AzureRmVMAccessExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmaccessextension) polecenia cmdlet programu PowerShell. Poniższy przykład powoduje zresetowanie rozszerzenia dostępu o nazwie `myVMAccess` na maszynie Wirtualnej o nazwie `myVM` w `myResourceGroup` grupy zasobów:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> W dowolnym momencie maszyna wirtualna może mieć tylko jednego agenta dostęp do maszyny Wirtualnej. Aby ustawić dostęp do maszyny Wirtualnej właściwości agenta, należy użyć `-ForceRerun` opcji. Kiedy używasz `-ForceRerun`, upewnij się, użyj tej samej nazwy agenta dostęp do maszyny Wirtualnej, którego użyto w poprzednie polecenia.

Jeśli nadal nie możesz połączyć zdalne z maszyną wirtualną, zobacz [Rozwiązywanie problemów z pulpitu zdalnego połączenia z systemem Windows maszyny wirtualnej platformy Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). W przypadku utraty połączenia z kontrolerem domeny Windows, należy ją przywrócić z kopii zapasowej kontrolera domeny.

## <a name="next-steps"></a>Kolejne kroki
Jeśli rozszerzenie dostępu do maszyny Wirtualnej platformy Azure nie odpowiada i nie możesz się zresetować hasło, możesz to zrobić [Resetowanie hasła lokalnego Windows w trybie offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ta metoda jest bardziej zaawansowane i wymaga podłączenia wirtualnego dysku twardego problematyczne maszyny wirtualnej do innej maszyny Wirtualnej. Wykonaj kroki opisane w tym artykule najpierw, a próba metoda resetowania hasła w trybie offline, tylko wtedy, gdy te kroki nie zadziałają.

[Dowiedz się więcej na temat funkcji i rozszerzeń maszyny Wirtualnej platformy Azure](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Nawiązać połączenie z maszyną wirtualną platformy Azure przy użyciu protokołu RDP lub SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Rozwiązywanie problemów z połączeniami pulpitu zdalnego z maszyną wirtualną na podstawie Windows Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

