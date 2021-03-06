---
title: Uaktualnianie do stosu kopii zapasowej maszyny Wirtualnej platformy Azure w wersji 2
description: Uaktualnienie procesu i często zadawane pytania dla stosu kopii zapasowej maszyny Wirtualnej, modelu wdrażania usługi Resource Manager
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 10/3/2018
ms.author: trinadhk
ms.openlocfilehash: 9152733e189aec25a5c024de7f9a3582c29218a3
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406538"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Uaktualnianie do stosu kopii zapasowych maszyn wirtualnych platformy Azure w wersji 2

Model wdrażania usługi Resource Manager dla uaktualnienie do stosu kopii zapasowej maszyny wirtualnej (VM) zapewnia następujące ulepszenia funkcji:

* Możliwość, aby wyświetlić migawki podjęte w ramach zadania tworzenia kopii zapasowej, który jest dostępny dla odzyskiwania bez oczekiwania na transfer danych zakończyć. Zmniejsza to czas oczekiwania dla migawek skopiować do magazynu przed wyzwoleniem przywracania. Ponadto dzięki temu nie wymaganie dodatkowego miejsca do magazynowania kopie zapasowe maszyn wirtualnych usługi premium, z wyjątkiem pierwszej kopii zapasowej.  

* Skraca okresy i przywracania kopii zapasowych, przy zachowaniu migawek lokalnie, przez siedem dni.

* Obsługa dysków o rozmiarach do 4 TB.

* Możliwość używania niezarządzanej maszyny Wirtualnej w oryginalnych kont magazynu, podczas przywracania. Ta możliwość istnieje, nawet wtedy, gdy maszyna wirtualna ma dyski, które są dystrybuowane na kontach magazynu. Przyspiesza operacje przywracania dla różnych konfiguracji maszyny Wirtualnej.
    > [!NOTE]
    > Ta możliwość nie jest taka sama jak zastępowanie dysków maszyny Wirtualnej przy użyciu danych z punktu odzyskiwania. 
    

## <a name="whats-changing-in-the-new-stack"></a>Co ulega zmianie na nowy stos?
Obecnie zadanie tworzenia kopii zapasowej obejmuje dwa etapy:
1.  Wykonanie migawki maszyny Wirtualnej.
2.  Przenoszenie migawki maszyny Wirtualnej do magazynu kopii zapasowych Azure.

Punkt odzyskiwania jest uważany za tworzone tylko wtedy, gdy są wykonywane fazy 1 i 2. Jako część nowego stosu punkt odzyskiwania jest tworzony, zaraz po zakończeniu migawki. Można także przywrócić z tego punktu odzyskiwania przy użyciu tego samego przepływu przywracania. Tego punktu odzyskiwania w witrynie Azure portal można zidentyfikować za pomocą "snapshot" jako typ punktu odzyskiwania. Po przekazaniu migawki do magazynu typ punktu odzyskiwania zmienia się na "migawka i magazyn."

![Zadanie tworzenia kopii zapasowej w maszynę Wirtualną stosu kopii zapasowej modelu Resource Manager deployment — magazynu i Magazyn](./media/backup-azure-vms/instant-rp-flow.jpg)

Domyślnie migawki są przechowywane przez 7 dni. Ta funkcja umożliwia przywracanie szybsze zakończenie z migawek. Zmniejsza to czas, które są wymagane w celu kopiowania danych z magazynu do konta magazynu klienta.

## <a name="considerations-before-upgrade"></a>Zagadnienia dotyczące przed uaktualnieniem

* Uaktualnienie stosu kopii zapasowej maszyny Wirtualnej jest jednym kierunkowe, tworzenie wszystkich kopii zapasowych przejdź do tego przepływu. Ponieważ zmiana odbywa się na poziomie subskrypcji, wszystkie maszyny wirtualne przechodzą w ten przepływ. Nowe funkcje są oparte na tym samym stosie. Nie można obecnie kontrolować stosu na poziomie zasad.

* Migawki są przechowywane lokalnie, aby zwiększyć możliwości tworzenia punktu odzyskiwania, a także aby przyspieszyć operacje przywracania. W rezultacie zobaczysz koszty magazynowania, które odpowiadają migawek podczas 7 dniowy okres.

* Przyrostowe migawki są przechowywane jako stronicowe obiekty BLOB. Wszyscy klienci, którzy korzystają z dysków niezarządzanych są naliczane przez siedem dni, które migawki są przechowywane na koncie magazynu lokalnego klienta. Ponieważ kolekcje punktów przywracania używana przez kopie zapasowe zarządzanych maszyn wirtualnych używa migawek obiektów blob na poziomie magazynu podstawowego, za dyski zarządzane zobaczysz koszty odpowiadający [migawki cen usługi blob](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) i są one przyrostowe.

* Maszyna wirtualna w warstwie premium w przypadku przywracania z migawki punktu odzyskiwania, tymczasową lokalizację magazynu jest używany podczas tworzenia maszyny Wirtualnej.

* Dla kont usługi premium storage miejsca przydzielonego do migawek wykonywanych liczby punktów natychmiastowe odzyskiwanie kierunku limit 10 TB.

> [!NOTE]
> Uaktualnianie do stosu kopii zapasowych maszyn wirtualnych platformy Azure w wersji 2 można pobrać obsługę usługi Azure Backup [dysków zarządzanych dysków SSD w warstwie standardowa](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/) i maszyny wirtualne z maksymalnie 32 dyski danych.

## <a name="upgrade"></a>Uaktualnienie
### <a name="the-azure-portal"></a>Witryna Azure Portal
Jeśli używasz witryny Azure portal, zostanie wyświetlone powiadomienie na pulpicie nawigacyjnym magazynu. To powiadomienie odnosi się do obsługi dużych dysków oraz ulepszenia dotyczące prędkości i przywracania kopii zapasowych. Alternatywnie możesz przejść do strony właściwości magazynu można pobrać opcji uaktualniania.

![Zadanie tworzenia kopii zapasowej w modelu wdrażania usługi Resource Manager stosu kopii zapasowej maszyny Wirtualnej — zgłoszenia pomocy technicznej](./media/backup-azure-vms/instant-rp-banner.png)

Aby otworzyć ekran dla uaktualnienie do nowego stosu, wybierz baner.

![Zadanie tworzenia kopii zapasowej w stos kopii zapasowej maszyny Wirtualnej modelu wdrażania usługi Resource Manager — uaktualnianie](./media/backup-azure-vms/instant-rp.png)

### <a name="powershell"></a>PowerShell
Uruchom następujące polecenia cmdlet z terminala PowerShell z podwyższonym poziomem uprawnień:
1.  Zaloguj się do konta platformy Azure:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Wybierz subskrypcję, której chcesz zarejestrować:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Zarejestruj tej subskrypcji:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Sprawdź, czy zakończeniu uaktualniania
Z poziomu terminalu programu PowerShell z podwyższonym poziomem uprawnień uruchom następujące polecenie cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Jeśli wyświetlany jest tekst "Zarejestrowane", Twoja subskrypcja jest uaktualniany do modelu wdrażania usługi Resource Manager stosu kopii zapasowej maszyny Wirtualnej.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

Zestaw poniższych pytań i odpowiedzi zostały połączone z forów i pytania klientów.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>Uaktualnienie do wersji 2 wpływają na bieżące kopie zapasowe?
Po uaktualnieniu do wersji 2, istnieje bez wpływu na Twoje bieżące kopie zapasowe i nie konieczności konfigurowania środowiska. Uaktualnianie i tworzenia kopii zapasowej środowiska w dalszym ciągu działać, ponieważ zawiera ona.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>Ile kosztuje uaktualnienie do kopii zapasowych maszyn wirtualnych usługi Azure stack w wersji 2?
Nie ma żadnych kosztów, aby uaktualnić stosu do wersji 2. Migawki są przechowywane lokalnie, aby przyspieszyć tworzenie punktu odzyskiwania i operacji przywracania. W rezultacie zobaczysz koszty magazynowania, które odpowiadają migawki wykonane w ciągu 7 dniowy okres.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>Uaktualnianie do stosu v2 wzrostu limit migawek konta magazynu premium przez 10 TB?
Nie, limitu całkowitej migawki na konto magazynu nadal pozostaje na 10TB.

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>W ramach kont usługi Premium Storage migawek wykonanych dla punktu odzyskiwania błyskawicznych zajmować limit migawek 10 TB?
Tak, dla kont usługi premium storage, migawki wykonane w punkcie natychmiastowe odzyskiwanie, zajmują przydzielone 10 TB miejsca.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>Jak działa migawki w okresie siedmiu dni?
Każdego dnia nowe migawki. Istnieje siedem poszczególnych migawki. Usługą **nie** wykonaj kopię pierwszego dnia, a następnie dodaj zmian dla ciągu najbliższych sześciu dni.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Jest migawką v2, przyrostową migawką lub Pełna migawka?
Migawek przyrostowych są używane w przypadku dysków niezarządzanych. W przypadku dysków zarządzanych punkt, Kolekcja utworzona przez usługę Azure Backup używa migawek obiektów blob i dlatego są dodawane.

### <a name="how-to-get-standard-ssd-managed-disk-support-for-a-virtual-machine"></a>Jak uzyskać SSD w warstwie standardowa zarządzany za pomocą techniczną dysku dla maszyny wirtualnej
Uaktualnianie do stosu kopii zapasowych maszyn wirtualnych platformy Azure w wersji 2 można pobrać obsługę usługi Azure Backup [dysków zarządzanych dysków SSD w warstwie standardowa](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). Po uaktualnieniu można również tworzyć kopie zapasowe maszyn wirtualnych z maksymalnie 32 dyski danych.
