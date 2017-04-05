---
title: "Różne sposoby tworzenia maszyny wirtualnej z systemem Linux na platformie Azure | Microsoft Azure"
description: "Informacje na temat różnych sposobów tworzenia maszyny wirtualnej z systemem Linux na platformie Azure oraz linki do narzędzi i samouczków dotyczących poszczególnych metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d7ff1317cdf6ccfe6b4e5035878fc4e096fcc0f9
ms.lasthandoff: 04/03/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Różne sposoby tworzenia maszyny wirtualnej z systemem Linux
Platforma Azure umożliwia elastyczne tworzenie maszyn wirtualnych z systemem Linux przy użyciu dowolnych narzędzi i przepływów pracy. Ten artykuł zawiera podsumowanie różnych sposobów oraz przykłady tworzenia maszyn wirtualnych z systemem Linux, m.in. przy użyciu interfejsu wiersza polecenia platformy Azure 2.0. Możesz też wyświetlić informacje o opcjach tworzenia z użyciem [interfejsu wiersza polecenia platformy Azure 1.0](creation-choices-nodejs.md).

[Interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-az-cli2) jest dostępny na wielu platformach przy użyciu pakietów menedżera npm, pakietów dostępnych dla określonych dystrybucji lub kontenera Docker. Zainstaluj kompilację najbardziej odpowiednią dla danego środowiska i zaloguj się na koncie platformy Azure przy użyciu [identyfikatora logowania az](/cli/azure/#login)

W poniższych przykładach używany jest interfejs wiersza polecenia platformy Azure 2.0. Zapoznaj się z poszczególnymi artykułami, aby uzyskać więcej informacji na temat przedstawionych poleceń. Możesz także znaleźć przykłady dotyczące opcji tworzenia dla systemu Linux przy użyciu [interfejsu wiersza polecenia platformy Azure w wersji 1.0](creation-choices-nodejs.md).

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W tym przykładzie użyto polecenia [az group create](/cli/azure/group#create) do utworzenia grupy zasobów o nazwie `myResourceGroup`: 
-    
    ```azurecli
    az group create --name myResourceGroup --location westus
    ```
    
  * W tym przykładzie użyto polecenia [az vm create](/cli/azure/vm#create) do utworzenia maszyny wirtualnej o nazwie `myVM` przy użyciu najnowszego obrazu systemu Debian z usługą Azure Managed Disks i kluczem publicznym o nazwie `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
     --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  –-new --size-gb 5    --public-ip-address-dns-name myPublicDNS \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

    * Jeśli chcesz korzystać z dysków niezarządzanych, dodaj flagę `--use-unmanaged-disks` do powyższego polecenia. Zostanie utworzone konto magazynu. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Managed Disks](../../storage/storage-managed-disks-overview.md).

* [Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W poniższym przykładzie użyto polecenia [az group deployment create](/cli/azure/group/deployment#create) do utworzenia maszyny wirtualnej przy użyciu szablonu przechowywanego w witrynie GitHub:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Tworzenie kompletnego środowiska systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Obejmuje tworzenie modułu równoważenia obciążenia i wielu maszyn wirtualnych w zestawie dostępności.

* [Dodawanie dysku do maszyny wirtualnej z systemem Linux](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * W poniższym przykładzie przy użyciu polecenia [az vm disk attach-new](/cli/azure/vm/disk#attach-new) do istniejącej maszyny wirtualnej o nazwie `myVM` jest dodawany dysk zarządzany o rozmiarze 50 GB:
  
    ```azurecli
    az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  \
    –-new --size-gb 50
    ```

## <a name="azure-portal"></a>Azure Portal
Witryna [Azure Portal](https://portal.azure.com) umożliwia szybkie tworzenie maszyn wirtualnych, ponieważ nie wymaga instalacji żadnych składników w systemie. Użyj witryny Azure Portal, aby utworzyć maszynę wirtualną:

* [Tworzenie maszyny wirtualnej z systemem Linux przy użyciu witryny Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Dołączanie dysku przy użyciu witryny Azure Portal](../windows/attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Wybór systemu operacyjnego i obrazu
Podczas tworzenia maszyny wirtualnej możesz wybrać obraz w oparciu o system operacyjny, który chcesz uruchomić. Platforma Azure i jej partnerzy oferują wiele obrazów, z których część zawiera wstępnie zainstalowane aplikacje i narzędzia. Możesz również przekazać własny obraz (więcej informacji można znaleźć w [poniższej sekcji](#use-your-own-image)).

### <a name="azure-images"></a>Obrazy platformy Azure
Użyj polecenia [az vm image](/cli/azure/vm/image), aby wyświetlić dostępne obrazy według wydawcy, wersji dystrybucji i kompilacji.

Aby wyświetlić dostępnych wydawców:

```azurecli
az vm image list-publishers --location WestUS
```

Aby wyświetlić dostępne produkty (oferty) dla wybranego wydawcy:

```azurecli
az vm image list-offers --publisher Canonical --location WestUS
```

Aby wyświetlić dostępne jednostki SKU (wersje dystrybucji) dla danej oferty:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location WestUS
```

Aby wyświetlić dostępne obrazy dla danego wydania:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location WestUS
```

Aby uzyskać więcej przykładów związanych z przeglądaniem i wykorzystywaniem dostępnych obrazów, zobacz [Navigate and select Azure virtual machine images with the Azure CLI](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure przy użyciu interfejsu wiersza polecenia platformy Azure).

Polecenie **az vm create** ma aliasy umożliwiające szybki dostęp do najpopularniejszych dystrybucji i ich najnowszych wersji. Użycie aliasów jest zwykle łatwiejsze niż określanie wydawcy, oferty, jednostki SKU i wersji za każdym razem podczas tworzenia maszyny wirtualnej:

| Alias | Wydawca | Oferta | SKU | Wersja |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |najnowsza |
| CoreOS |CoreOS |CoreOS |Stable |najnowsza |
| Debian |credativ |Debian |8 |najnowsza |
| openSUSE |SUSE |openSUSE |13.2 |najnowsza |
| RHEL |Redhat |RHEL |7.2 |najnowsza |
| SLES |SLES |SLES |12-SP1 |najnowsza |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |najnowsza |

### <a name="use-your-own-image"></a>Użycie własnego obrazu
Jeśli potrzebujesz specjalnego dostosowania, możesz użyć obrazu opartego na istniejącej maszynie wirtualnej platformy Azure poprzez *przechwycenie* tej maszyny wirtualnej. Możesz również przekazać obraz utworzony lokalnie. Aby uzyskać więcej informacji o obsługiwanych dystrybucjach i sposobach wykorzystania własnych obrazów, zobacz następujące artykuły:

* [Dystrybucje zatwierdzone na platformie Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Informacje dotyczące niezatwierdzonych dystrybucji](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [How to capture a Linux virtual machine as a Resource Manager template](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Jak przechwycić maszynę wirtualną systemu Linux jako szablon usługi Resource Manager).
  
  * Przykładowe polecenia **az vm** Szybki start umożliwiające przechwycenie istniejącej maszyny wirtualnej przy użyciu dysków niezarządzanych:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm capture --resource-group myResourceGroup --name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Następne kroki
* Utwórz maszynę wirtualną z systemem Linux przy użyciu [portalu](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), interfejsu [wiersza polecenia platformy Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub [szablonu](../windows/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) usługi Azure Resource Manager.
* Po utworzeniu maszyny wirtualnej z systemem Linux [dodaj dysk danych](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Szybkie kroki umożliwiające [zresetowanie hasła lub kluczy SSH i zarządzanie użytkownikami](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

