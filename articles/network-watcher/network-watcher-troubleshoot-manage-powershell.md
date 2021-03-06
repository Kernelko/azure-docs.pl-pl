---
title: Rozwiązywanie problemów z bramy sieci wirtualnej platformy Azure i połączenia — PowerShell | Dokumentacja firmy Microsoft
description: Tej stronie wyjaśniamy, jak używać usługi Azure Network Watcher Rozwiązywanie problemów z polecenia cmdlet programu PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 67c34156d6a397cdeb4bb50c712b1bb651c2f257
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090432"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Rozwiązywanie problemów z bramy sieci wirtualnej i połączeń przy użyciu programu PowerShell obserwatora sieci platformy Azure

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [Program PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Interfejs wiersza polecenia platformy Azure](network-watcher-troubleshoot-manage-cli.md)
> - [Interfejs API REST](network-watcher-troubleshoot-manage-rest.md)

Usługa Network Watcher udostępnia wiele możliwości, w odniesieniu do zrozumienia zasobów sieciowych na platformie Azure. Jedną z tych funkcji jest zasobem rozwiązywania problemów. Rozwiązywanie problemów z zasobu może być wywoływany za pośrednictwem portalu, programu PowerShell, interfejsu wiersza polecenia lub interfejsu API REST. Po wywołaniu usługi Network Watcher sprawdza kondycję bramy sieci wirtualnej lub połączenie i zwraca jej ustaleń.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym scenariuszu przyjęto założenie, zostały już wykonane czynności opisane w [utworzyć usługę Network Watcher](network-watcher-create.md) utworzyć usługę Network Watcher.

Aby uzyskać listę typów obsługiwanych bramy, zobacz [typy bram obsługiwane](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Przegląd

Rozwiązywanie problemów z zasobów umożliwia rozwiązywanie problemów, które wynikają z bramy sieci wirtualnej i połączenia. Po wysłaniu żądania do zasobów dotyczących rozwiązywania problemów, dzienniki są wyszukiwane i inspekcji. Po ukończeniu inspekcji wyniki są zwracane. Zasób, który żądania dotyczące rozwiązywania problemów są długotrwałe żądania, może to potrwać kilka minut zwrócony wynik. Dzienniki z rozwiązywania problemów są przechowywane w kontenerze na koncie magazynu, który jest określony.

## <a name="retrieve-network-watcher"></a>Pobieranie usługi Network Watcher

Pierwszym krokiem jest można pobrać wystąpienia usługi Network Watcher. `$networkWatcher` Zmienna jest przekazywana do `Start-AzureRmNetworkWatcherResourceTroubleshooting` polecenia cmdlet w kroku 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Pobieranie połączenia bramy sieci wirtualnej

W tym przykładzie Rozwiązywanie problemów z zasobu jest uruchomiono połączenia. Można również przekazać go bramy sieci wirtualnej.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Rozwiązywanie problemów z zasobów zwraca dane o kondycji zasobu, zapisuje dzienniki na koncie magazynu do przeglądu. W tym kroku utworzymy konto magazynu, jeśli istnieje już istniejące konto magazynu możesz użyć go.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Uruchom Rozwiązywanie problemów z zasób usługi Network Watcher

Rozwiązywanie problemów z zasobami przy użyciu `Start-AzureRmNetworkWatcherResourceTroubleshooting` polecenia cmdlet. Polecenia cmdlet przekazanie obiektu usługi Network Watcher, identyfikator połączenia lub bramy sieci wirtualnej, identyfikator konta magazynu i ścieżkę, do przechowywania wyników.

> [!NOTE]
> `Start-AzureRmNetworkWatcherResourceTroubleshooting` Polecenia cmdlet jest długotrwałe i może potrwać kilka minut.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Po uruchomieniu polecenia cmdlet usługi Network Watcher przegląda na sprawdzenie kondycji zasobu. Jego zwraca wyniki do powłoki i dzienniki wyniki są przechowywane na koncie magazynu określonym.

## <a name="understanding-the-results"></a>Opis wyników

Tekst akcji zawiera ogólne wskazówki na temat sposobu rozwiązania problemu. Jeśli dla problemu można wykonać akcji, łącze jest dostarczana z dodatkowych wskazówek. W przypadku, w przypadku, gdy nie ma żadnych dodatkowych wskazówek, odpowiedź zawiera adres url, aby otworzyć zgłoszenie do pomocy technicznej.  Aby uzyskać więcej informacji na temat właściwości odpowiedzi i co jest zawarte w odwiedzić [rozwiązywania problemów z Network Watcher — omówienie](network-watcher-troubleshoot-overview.md)

Aby uzyskać instrukcje dotyczące pobierania plików z konta usługi azure storage, zapoznaj się [wprowadzenie do usługi Azure Blob storage przy użyciu platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Inne narzędzie, które mogą być używane jest Eksploratora usługi Storage. Więcej informacji na temat Eksploratora usługi Storage można znaleźć tutaj pod następujący link: [Eksploratora usługi Storage](http://storageexplorer.com/)

## <a name="next-steps"></a>Kolejne kroki

Jeśli zmieniono ustawienia tego połączenia sieci VPN stop, zobacz [Zarządzanie sieciowymi grupami zabezpieczeń](../virtual-network/manage-network-security-group.md) ułatwiają śledzenie reguły zabezpieczeń sieci grupy i zabezpieczeń, które mogą być w danym.
