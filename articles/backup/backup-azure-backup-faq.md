
---
title: "Usługa Azure Backup — często zadawane pytania | Microsoft Docs"
description: "Odpowiedzi na często zadawane pytania dotyczące sposobu działania usługi, agenta usługi Azure Backup, magazynu usługi Recovery Services i limitów wykonywania kopii zapasowych oraz przechowywania."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "tworzenie kopii zapasowej i odzyskiwanie po awarii; usługa kopii zapasowej"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/10/2017
ms.author: markgal;giridham;arunak;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 87d500d886feb2dbd61d15d5a980ab2c4018c2f2
ms.lasthandoff: 04/03/2017


---
# <a name="questions-about-the-azure-backup-service"></a>Pytania dotyczące usługi Azure Backup
W tym artykule znajdują się sekcje często zadawanych pytań (wraz z odpowiedziami) ułatwiające szybkie zapoznanie się ze składnikami usługi Azure Backup. W niektórych odpowiedziach znajdują się linki do artykułów zawierających szczegółowe informacje. Aby zadać pytanie dotyczące usługi Azure Backup, kliknij pozycję **Komentarze** (po prawej stronie). Komentarze są wyświetlane na dole tego artykułu. Aby komentować, musisz mieć konto Livefyre. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Aby szybko przeskanować sekcje znajdujące się w tym artykule, użyj linków z prawej strony w obszarze **W tym artykule**.


## <a name="recovery-services-vault"></a>Magazyn usługi Recovery Services

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Czy istnieje ograniczenie liczby magazynów, które można utworzyć w poszczególnych subskrypcjach platformy Azure? <br/>
Tak. Od września 2016 r. w każdej subskrypcji można utworzyć 25 magazynów usługi Recovery Services lub magazynów kopii zapasowych. Maksymalnie można utworzyć 25 magazynów usługi Recovery Services na obsługiwany region usługi Azure Backup dla każdej subskrypcji. Jeśli potrzebna jest większa liczba magazynów, należy utworzyć dodatkową subskrypcję.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Czy istnieją ograniczenia dotyczące liczby serwerów/maszyn, które można zarejestrować w każdym magazynie? <br/>
Tak, w magazynie można zarejestrować maksymalnie 50 maszyn. W przypadku maszyny wirtualnej IaaS platformy Azure ograniczenie wynosi 200 maszyn wirtualnych w magazynie. Jeśli trzeba zarejestrować więcej maszyn, należy utworzyć kolejny magazyn.

### <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Jak można zarejestrować serwer w innym centrum danych?<br/>
Dane kopii zapasowej są wysyłane do centrum danych magazynu, w którym jest ona zarejestrowana. Najprostszym sposobem zmiany centrum danych jest odinstalowanie agenta oraz ponowne zainstalowanie i zarejestrowanie go w nowym magazynie należącym do odpowiedniego centrum danych.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Jeśli organizacja ma jeden magazyn, to w jaki sposób można odizolować dane z jednego serwera od danych z innego serwera podczas przywracania danych?<br/>
Wszystkie serwery, które są zarejestrowane w tym samym magazynie, mogą odzyskać dane umieszczone w kopii zapasowej przez inne serwery, *które korzystają z tego samego hasła*. Jeśli dane kopii zapasowej pewnych serwerów mają zostać odizolowane od innych serwerów w organizacji, należy użyć w przypadku tych serwerów innego hasła. Na przykład serwery zarządzania zasobami ludzkimi mogą korzystać z jednego hasła szyfrowania, serwery księgowości z drugiego, a serwery pamięci masowej z trzeciego.

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Jaki jest minimalny wymagany rozmiar folderu pamięci podręcznej? <br/>
Rozmiar folderu pamięci podręcznej określa ilość danych, które można umieścić w kopii zapasowej. Folder pamięci podręcznej powinien udostępniać 5% miejsca wymaganego do przechowywania danych.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Czy można „migrować” magazyn lub dane kopii zapasowej między subskrypcjami? <br/>
Nie. Magazyn jest tworzony na poziomie subskrypcji i po utworzeniu nie można go ponownie przypisać do innej subskrypcji.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Magazyny usług Recovery Services są oparte na usłudze Resource Manager. Czy magazyny usługi Backup (tryb klasyczny) są nadal obsługiwane? <br/>
Wszystkie magazyny usługi Backup istniejące w [klasycznym portalu](https://manage.windowsazure.com) nadal są obsługiwane. Za pomocą klasycznego portalu nie można już jednak wdrażać nowych magazynów usługi Backup. Firma Microsoft zaleca używanie magazynów usługi Recovery Services dla wszystkich wdrożeń, ponieważ przyszłe rozszerzenia będą miały zastosowanie tylko do magazynów tej usługi. Przy próbie utworzenia magazynu usługi Backup w portalu klasycznym nastąpi przekierowanie do witryny [Azure Portal](https://portal.azure.com).

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Czy mogę przeprowadzić migrację magazynu usługi Backup do magazynu usługi Recovery Services? <br/>
Niestety nie. Nie ma możliwości przeprowadzania migracji zawartości magazynu usługi Backup do magazynu usługi Recovery Services. Pracujemy nad dodaniem tej funkcji, ale nie jest ona obecnie dostępna.

### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Czy magazyny usługi Recovery Services obsługują klasyczne maszyny wirtualne, czy maszyny wirtualne oparte na usłudze Resource Manager? <br/>
Magazyny usługi Recovery Services obsługują oba modele.  Możliwe jest utworzenie kopii zapasowej klasycznej maszyny wirtualnej (utworzonej w portalu klasycznym) lub maszyny wirtualnej usługi Resource Manager (utworzonej w witrynie Azure Portal) w magazynie usługi Recovery Services.

### <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Mam utworzone kopie zapasowe moich klasycznych maszyn wirtualnych w magazynie usługi Backup. Czy mogę migrować maszyny wirtualne z trybu klasycznego do trybu usługi Resource Manager i chronić je w magazynie usługi Recovery Services?
Punkty odzyskiwania klasycznych maszyn wirtualnych w magazynie usługi Backup nie są migrowane automatycznie do magazynu usługi Recovery Services podczas przenoszenia maszyn wirtualnych z trybu klasycznego do trybu usługi Resource Manager. Wykonaj te kroki, aby przenieść kopie zapasowe maszyn wirtualnych:

1. W magazynie usługi Backup przejdź do karty **Elementy chronione** i wybierz maszynę wirtualną. Kliknij pozycję [Zatrzymaj ochronę](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Pozostaw opcję *Usuń powiązane dane kopii zapasowych* **niezaznaczoną**.
2. Przeprowadź migrację maszyny wirtualnej z trybu klasycznego do trybu usługi Resource Manager. Upewnij się, że informacje o magazynie i sieci odpowiadające maszynie wirtualnej również zostały zmigrowane do trybu usługi Resource Manager.
3. Utwórz magazyn usługi Recovery Services i skonfiguruj kopię zapasową w zmigrowanej maszynie wirtualnej przy użyciu akcji **Kopia zapasowa** w górnej części pulpitu nawigacyjnego magazynu. Aby uzyskać szczegółowe informacje na temat tworzenia kopii zapasowych maszyn wirtualnych w magazynie usługi Recovery Services, zobacz artykuł [Ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md).



## <a name="azure-backup-agent"></a>Agent usługi Azure Backup

### <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Gdzie można pobrać najnowszą wersję agenta usługi Azure Backup? <br/>
Najnowszą wersję agenta tworzenia kopii zapasowej systemu Windows Server, System Center DPM lub klienta systemu Windows można pobrać [tutaj](http://aka.ms/azurebackup_agent). Aby utworzyć kopię zapasową maszyny wirtualnej, należy skorzystać z agenta maszyny wirtualnej (który automatycznie instaluje właściwe rozszerzenie). Agent maszyny wirtualnej znajduje się już na maszynie wirtualnej, która została utworzona z poziomu galerii Azure.

### <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Podczas konfigurowania agenta usługi Azure Backup pojawia się monit o wprowadzenie poświadczeń magazynu. Czy poświadczenia magazynu wygasają?
Tak, poświadczenia magazynu wygasają po upływie 48 godzin. Jeśli plik wygaśnie, należy zalogować się do witryny Azure Portal i pobrać z magazynu pliki poświadczeń magazynu.

### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Co się stanie, jeśli zostanie zmieniona nazwa serwera Windows, który wykonuje kopie zapasowe danych na platformie Azure?<br/>
Po zmianie nazwy serwera wszystkie aktualnie skonfigurowane kopie zapasowe są zatrzymywane.
Zarejestruj nową nazwę serwera w magazynie usługi Backup. Po zarejestrowaniu nowej nazwy w magazynie pierwszą operacją kopii zapasowej będzie *pełna* kopia zapasowa. Dane, których kopia zapasowa wykonana przy użyciu starej nazwy serwera znajduje się w magazynie, można odzyskać przy użyciu opcji [**Inny serwer**](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) w kreatorze **Odtwarzanie danych**.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Z jakich typów dysków można tworzyć kopie zapasowe plików i folderów? <br/>
Nie można utworzyć kopii zapasowych następujących dysków/woluminów:

* Nośniki wymienne: wszystkie źródła elementów kopii zapasowych muszą być stałe.
* Woluminy tylko do odczytu: wolumin musi mieć możliwość zapisu ze względu na działanie usługi kopiowania woluminów w tle (VSS, volume shadow copy).
* Woluminy offline: wolumin musi być w trybie online, aby mogła działać usługa VSS.
* Udział sieciowy: wolumin musi być lokalny dla serwera, aby możliwe było wykonanie kopii zapasowej za pomocą usługi kopii zapasowej online.
* Woluminy chronione przez funkcję BitLocker: wolumin musi zostać odblokowany przed próbą wykonania kopii zapasowej.
* Identyfikacja systemu plików: jedyny obsługiwany system plików to system NTFS.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Jakie typy plików i folderów z serwera można umieszczać w kopiach zapasowych?<br/>
Obsługiwane są następujące typy:

* Zaszyfrowane
* Skompresowane
* Rozrzedzone
* Skompresowane i rozrzedzone
* Twarde linki: nieobsługiwane, pomijane
* Punkt ponownej analizy: nieobsługiwane, pomijane
* Zaszyfrowane i rozrzedzone: nieobsługiwane, pomijane
* Skompresowany strumień: nieobsługiwane, pomijane
* Rozrzedzony strumień: nieobsługiwane, pomijane

### <a name="what-is-the-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Jaka jest maksymalna długość ścieżki pliku, którą można określić w ramach zasad usługi Backup przy użyciu agenta usługi Azure Backup? <br/>
Agent usługi Azure Backup bazuje na systemie plików NTFS. [Specyfikacja długości ścieżki pliku jest ograniczona przez interfejs API systemu Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Jeśli długość ścieżki pliku plików, które chcesz chronić, jest większa niż dozwolona przez interfejs API systemu Windows, utwórz kopię zapasową folderu nadrzędnego lub napędu dyskowego.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Jakie znaki są dozwolone w ścieżce pliku zasad usługi Azure Backup przy użyciu agenta usługi Azure Backup? <br>
 Agent usługi Azure Backup bazuje na systemie plików NTFS. Dopuszcza [znaki obsługiwane w systemie NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) w ramach specyfikacji pliku.  

### <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>W jaki sposób można zmienić lokalizację pamięci podręcznej określoną dla agenta usługi Azure Backup?<br/>
Użyj następującej listy, aby zmienić lokalizację pamięci podręcznej.

* Należy zatrzymać aparat kopii zapasowej za pomocą następującego polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:

```PS C:\> Net stop obengine```
* Nie należy przenosić plików. Zamiast tego należy skopiować folder z obszarem pamięci podręcznej na inny dysk z wystarczającą ilością miejsca. Pierwotny obszar pamięci podręcznej można usunąć po potwierdzeniu, że kopie zapasowe działają z nowym obszarem pamięci podręcznej.
* Należy zaktualizować następujące wpisy rejestru ścieżką do nowego folderu obszaru pamięci podręcznej.<br/>

| Ścieżka rejestru | Klucz rejestru | Wartość |
| --- | --- | --- |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nowa lokalizacja folderu pamięci podręcznej* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nowa lokalizacja folderu pamięci podręcznej* |

* Należy ponownie uruchomić aparat usługi Backup, wykonując następujące polecenie w wierszu polecenia z podwyższonym poziomem uprawnień:

```PS C:\> Net start obengine```

Po pomyślnym zakończeniu tworzenia kopii zapasowej w nowej lokalizacji pamięci podręcznej można usunąć pierwotny folder pamięci podręcznej.

### <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Gdzie można umieścić folder pamięci podręcznej, aby agent usługi Azure Backup działał zgodnie z oczekiwaniami?<br/>
Nie zaleca się używać następujących lokalizacji dla folderu pamięci podręcznej:

* Udział sieciowy lub nośniki wymienne: folder pamięci podręcznej musi być lokalny dla serwera, który wymaga wykonywania kopii zapasowych przy użyciu usługi kopii zapasowej online. Lokalizacje sieciowe ani nośniki wymienne, takie jak dyski USB, nie są obsługiwane.
* Woluminy offline: folder pamięci podręcznej musi być w trybie online dla oczekiwanej kopii zapasowej wykonywanej za pomocą agenta usługi Azure Backup.

### <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Czy jakiekolwiek atrybuty folderu pamięci podręcznej nie są obsługiwane?<br/>
Następujące atrybuty folderu pamięci podręcznej ani ich kombinacje nie są obsługiwane:

* Zaszyfrowane
* Deduplikowane
* Skompresowane
* Rozrzedzone
* Punkt ponownej analizy

Folder pamięci podręcznej i dysk VHD metadanych nie mają wymaganych atrybutów dla agenta usługi Azure Backup.


## <a name="virtual-machines"></a>Maszyny wirtualne

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Czy można zainstalować agenta usługi Azure Backup na maszynie wirtualnej, której kopia zapasowa została już utworzona przez usługę Azure Backup przy użyciu rozszerzenia maszyny wirtualnej? <br/>
Naturalnie. Usługa Azure Backup udostępnia funkcję tworzenia kopii zapasowych na poziomie maszyny wirtualnej dla maszyn wirtualnych platformy Azure przy użyciu rozszerzenia maszyny wirtualnej. Aby chronić pliki i foldery znajdujące się w systemie operacyjnym Windows gościa, zainstaluj w tym systemie agenta usługi Azure Backup.

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Czy można zainstalować agenta usługi Azure Backup na maszynie wirtualnej Azure, aby wykonywać kopie zapasowe plików i folderów znajdujących się w magazynie tymczasowym dostarczanym przez maszynę wirtualną platformy Azure? <br/>
Tak. Zainstaluj agenta usługi Azure Backup w systemie operacyjnym Windows gościa i wykonuj kopie zapasowe plików i folderów do magazynu tymczasowego. Zadania tworzenia kopii zapasowych zakończą się niepowodzeniem, gdy dane magazynu tymczasowego zostaną wyczyszczone. Ponadto usunięcie danych magazynu tymczasowego powoduje, że można je przywracać tylko do trwałego magazynu.


## <a name="azure-backup-server-and-data-protection-manager"></a>Program Azure Backup Server i Data Protection Manager

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Czy można używać programu Azure Backup Server do tworzenia kopii zapasowej z odzyskiwaniem systemu od zera (BMR) serwera fizycznego? <br/>
Tak.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Która wersja programu System Center Data Protection Manager jest obsługiwana? <br/>
Zaleca się zainstalowanie [najnowszego](http://aka.ms/azurebackup_agent) agenta usługi Azure Backup z najnowszego pakietu zbiorczego aktualizacji (UR) dla programu System Center Data Protection Manager (DPM). Począwszy od sierpnia 2016 r. pakiet zbiorczy aktualizacji 11 jest najnowszą aktualizacją.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-system-center-dpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Agent usługi Azure Backup został zainstalowany w celu ochrony plików i folderów. Czy można teraz zainstalować program System Center DPM do pracy z agentem usługi Azure Backup, aby chronić obciążenia aplikacji lokalnych i maszyn wirtualnych na platformie Azure? <br/>
Aby użyć usługi Azure Backup z programem System Center Data Protection Manager (DPM), najpierw zainstaluj program DPM, a następnie agenta usługi Azure Backup. Instalowanie składników usługi Azure Backup w tej kolejności zapewnia współdziałanie agenta usługi Azure Backup z programem DPM. Nie zaleca się instalowania agenta usługi Azure Backup przed instalacją programu DPM. Takie rozwiązanie nie jest obsługiwane.


## <a name="how-azure-backup-works"></a>Jak działa usługa Azure Backup

### <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Czy agent usługi Azure Backup działa na serwerze, który korzysta z deduplikacji systemu Windows Server 2012? <br/>
Tak. Podczas przygotowywania do wykonania kopii zapasowej usługa agenta konwertuje deduplikowane dane do zwykłej postaci. Następnie optymalizuje dane pod kątem utworzenia kopii zapasowej, szyfruje je, po czym wysyła zaszyfrowane dane do usługi kopii zapasowej online.

### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Czy w przypadku anulowania uruchomionego zadania tworzenia kopii zapasowej przesyłane dane zostaną usunięte? <br/>
Nie. Wszystkie dane przesłane do magazynu przed momentem anulowania zadania utworzenia kopii zapasowej pozostaną w magazynie. Usługa Azure Backup używa mechanizmu, który podczas tworzenia kopii zapasowej co pewien czas dodaje punkty kontrolne do danych kopii zapasowej. Ponieważ w danych kopii zapasowej umieszczone są punkty kontrolne, następny proces kopii zapasowej może sprawdzić integralność plików. Następnym zadaniem kopii zapasowej będzie przyrostowa kopia zapasowa tworzona w oparciu o wcześniej utworzoną kopię zapasową danych. Przyrostowe kopie zapasowe przesyłają tylko nowe lub zmienione dane, dzięki czemu zapewnia się lepsze wykorzystanie przepustowości.

Jeśli anulujesz zadanie kopii zapasowej dla maszyny wirtualnej platformy Azure, wszelkie przesłane dane zostaną zignorowane. Następne zadanie kopii zapasowej przesyła przyrostowe dane z ostatniego wykonanego zadania kopii zapasowej.

### <a name="if-a-backup-job-fails-can-i-configure-the-backup-service-to-send-e-mail-br"></a>Czy jeśli zadanie tworzenia kopii zapasowej nie powiedzie się, będzie można skonfigurować usługę Backup w taki sposób, aby wysyłała wiadomości e-mail? <br/>
Tak, usługa Backup ma kilka alertów opartych na zdarzeniach, z których można korzystać za pomocą skryptu programu PowerShell. Pełny opis można znaleźć w temacie [Configure notifications](backup-azure-monitor-vms.md#configure-notifications) (Konfigurowanie powiadomień).

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Czy istnieją ograniczenia dotyczące czasu wykonywania zadań tworzenia kopii zapasowej i ich liczby?<br/>
Tak. Zadania tworzenia kopii zapasowej na serwerze lub stacjach roboczych z systemem Windows można uruchamiać maksymalnie trzy razy w ciągu dnia. Zadania tworzenia kopii zapasowej w programie System Center DPM można uruchamiać maksymalnie dwa razy dziennie. Zadanie tworzenia kopii zapasowej maszyn wirtualnych IaaS można uruchamiać jeden raz w ciągu dnia. Korzystając z zasad planowania dla systemu Windows Server lub stacji roboczej z systemem Windows, można określić dzienne lub tygodniowe harmonogramy. Za pomocą programu System Center DPM można określić dzienne, tygodniowe, miesięczne i roczne harmonogramy.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Dlaczego rozmiar danych przesyłanych do magazynu usługi Recovery Services jest mniejszy niż rozmiar danych, dla których utworzono kopię zapasową?<br/>
 Wszystkie kopie zapasowe wykonywane przy użyciu agenta usługi Azure Backup, programu SCDPM lub serwera usługi Azure Backup są kompresowane i szyfrowane, zanim zostaną przesłane. Po zastosowaniu kompresji i szyfrowania dane w magazynie kopii zapasowych są mniejsze o 30–40%.

 ### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Czy istnieje sposób dostosowania wielkości przepustowości, która będzie używana przez usługę Backup?<br/>
  Tak, aby ustawić przepustowość, należy skorzystać z opcji **Zmień właściwości** agenta kopii zapasowej. Możesz dostosować wielkość przepustowości oraz czas, w którym będzie ona używana. Aby uzyskać instrukcje krok po kroku, zobacz **[Enable network throttling](backup-configure-vault.md#enable-network-throttling)** (Włączanie ograniczania użycia sieci).



## <a name="what-can-i-back-up"></a>Dla jakich danych mogę utworzyć kopię zapasową

### <a name="which-operating-systems-do-azure-backup-support-br"></a>Jakie systemy operacyjne są obsługiwane przez usługę Azure Backup? <br/>
Usługa Azure Backup obsługuje następujące systemy operacyjne w przypadku zabezpieczonego tworzenia kopii zapasowych: plików i folderów oraz kopii zapasowych aplikacji obciążeń przy użyciu programów Azure Backup Server i System Center Data Protection Manager (DPM).

| System operacyjny | Platforma | SKU |
|:--- | --- |:--- |
| Windows 8 i najnowsze dodatki Service Pack |64-bitowa |Enterprise, Pro |
| Windows 7 i najnowsze dodatki Service Pack |64-bitowa |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 i najnowsze dodatki Service Pack |64-bitowa |Enterprise, Pro |
| Windows 10 |64-bitowa |Enterprise, Pro, Home |
| Windows Server 2016 |64-bitowa |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 i najnowsze dodatki Service Pack |64-bitowa |Standard, Datacenter, Foundation |
| Windows Server 2012 i najnowsze dodatki Service Pack |64-bitowa |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 i najnowsze dodatki Service Pack |64-bitowa |Standard, Workgroup |
| Windows Storage Server 2012 i najnowsze dodatki Service Pack |64-bitowa |Standard, Workgroup |
| Windows Server 2012 R2 i najnowsze dodatki Service Pack |64-bitowa |Essential |
| Windows Server 2008 R2 SP1 |64-bitowa |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64-bitowa |Standard, Enterprise, Datacenter, Foundation |

**W przypadku usługi kopii zapasowych maszyny wirtualnej platformy Azure:**

* **Linux**: usługa Azure Backup obsługuje [dystrybucje zalecane dla platformy Azure](../virtual-machines/linux/endorsed-distros.md) z wyjątkiem systemu operacyjnego Linux Core.  W przypadku pozostałych dystrybucji opartych na modelu „Bring Your Own Linux” usługa Azure Backup powinna działać, jeśli na maszynie wirtualnej jest dostępny agent maszyny wirtualnej oraz jest obsługiwany język Python.
* **Windows Server**: wersje starsze niż Windows Server 2008 R2 nie są obsługiwane.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Czy istnieje ograniczenie rozmiaru poszczególnych źródeł danych, dla których tworzona jest kopia zapasowa? <br/>
Nie ma żadnego limitu ilości danych, dla których można utworzyć kopie zapasowe w magazynie. Usługa Azure Backup ogranicza maksymalny rozmiar źródła danych, niemniej te limity są duże. Według stanu na sierpień 2015 r. maksymalny rozmiar źródła danych dla obsługiwanych systemów operacyjnych wynosi:

| L.p. | System operacyjny | Maksymalny rozmiar źródła danych |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 lub nowszy |54 400 GB |
| 2 |Windows 8 lub nowszy |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

W poniższej tabeli opisano sposób ustalania rozmiaru dla każdego źródła danych.

| Źródło danych | Szczegóły |
|:---:|:--- |
| Wolumin |Ilość danych kopii zapasowej z jednego woluminu komputera serwera lub klienta |
| Maszyna wirtualna z funkcją Hyper-V |Suma danych wszystkich dysków VHD maszyny wirtualnej, której kopia zapasowa jest wykonywana |
| Baza danych Microsoft SQL Server |Rozmiar pojedynczej bazy danych SQL, której kopia zapasowa jest wykonywana |
| Microsoft SharePoint |Suma baz danych z zawartością i danymi konfiguracyjnymi w farmie programu SharePoint, której kopia zapasowa jest wykonywana |
| Microsoft Exchange |Suma wszystkich baz danych programu Exchange w serwerze Exchange, którego kopia zapasowa jest wykonywana |
| Stan systemu/BMR |Każda pojedyncza kopia BMR lub stanu systemu komputera, którego kopia zapasowa jest wykonywana |



## <a name="retention-policy-and-recovery-points"></a>Zasady przechowywania i punkty odzyskiwania

### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Czy istnieje różnica między zasadami przechowywania w programie DPM i systemie Windows Server/kliencie systemu Windows (tj. w systemie Windows Server bez programu DPM)?<br/>
Nie, zarówno program DPM, jak i system Windows Server/klient systemu Windows mają dzienne, tygodniowe, miesięczne i roczne zasady przechowywania.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Czy można skonfigurować zasady przechowywania selektywnie, na przykład skonfigurować zasady tygodniowe i dziennie, ale nie roczne i miesięczne?<br/>
Tak, struktura przechowywania usługi Azure Backup umożliwia w pełni elastyczne definiowanie zasad przechowywania zgodnie z wymaganiami użytkownika.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Czy można „zaplanować utworzenie kopii zapasowej” o godzinie 18:00 i określić zasady przechowywania na inną godzinę?<br/>
Nie. Zasady przechowywania mogą być stosowane wyłącznie w punktach kopii zapasowej. Na poniższej ilustracji określono zasady przechowywania dla kopii zapasowych wykonanych o godzinie 12:00 i 18:00. <br/>

![Harmonogram tworzenia i przechowywania kopii zapasowych](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Czy przyrostowa kopia jest przesyłana zgodnie z zaplanowanymi zasadami przechowywania? <br/>
Nie, przyrostowa kopia jest wysyłana na podstawie czasu podanego na stronie harmonogramu tworzenia kopii zapasowych. Punkty, które mogą być przechowywane, są określane na podstawie zasad przechowywania.

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Czy w przypadku, gdy kopia zapasowa przechowywana jest przez długi czas, odzyskanie danych z wcześniejszego punktu danych trwa dłużej? <br/>
Nie — czas odzyskania najstarszego i najnowszego punktu jest taki sam. Każdy punkt odzyskiwania zachowuje się jak pełny punkt.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Skoro każdy punkt odzyskiwania jest traktowany jak pełny punkt, czy ma to wpływ na całkowitą fakturowaną przestrzeń dyskową dla kopii zapasowych?<br/>
Typowe produkty punktów długoterminowego przechowywania przechowują dane kopii zapasowych jako pełne punkty. Pełne punkty są *nieefektywne* z punktu widzenia magazynu, ale są łatwiejsze i szybsze do przywrócenia. Przyrostowe kopie są *efektywne* z punktu widzenia magazynu, ale wymagają przywrócenia łańcucha danych, co wpływa na czas odzyskiwania. Architektura magazynu usługi Azure Backup oferuje zalety obu rozwiązań dzięki optymalnemu przechowywaniu danych, umożliwiającemu szybkie ich przywrócenie, oraz niskim kosztom magazynowania. Takie podejście do magazynu danych zapewnia efektywne wykorzystanie przepustowości ruchu przychodzącego i wychodzącego. Zarówno ilość pamięci masowej, jak i czas potrzebny do odzyskania danych są ograniczone do minimum. Dowiedz się więcej o tym, dlaczego zapisywanie [przyrostowych kopii zapasowych](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) jest wydajne.

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Czy istnieje ograniczenie liczby punktów odzyskiwania, które można utworzyć?<br/>
Możesz utworzyć maksymalnie 9999 punktów odzyskiwania na każde pojedyncze chronione wystąpienie. Chronione wystąpienie to komputer, serwer (fizyczny lub wirtualny) albo obciążenie, które skonfigurowano do tworzenia kopii zapasowych na platformie Azure. Nie ma żadnego limitu liczby chronionych wystąpień na magazyn kopii zapasowych. Aby uzyskać więcej informacji, zobacz objaśnienia w sekcjach [Tworzenie kopii zapasowej i przechowywanie](./backup-introduction-to-azure-backup.md#backup-and-retention) i [Co to jest chronione wystąpienie?](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance).

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Ile zadań odzyskiwania można wykonać na danych, których kopie zapasowe utworzono na platformie Azure?<br/>
Nie ma ograniczenia liczby operacji odzyskiwania z usługi Azure Backup.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Czy podczas przywracania danych należy zapłacić za ruch wychodzący z platformy Azure? <br/>
Nie. Operacje odzyskiwania są bezpłatne i nie są naliczane opłaty za ruch wyjściowy.

### <a name="i-receive-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Nawet po skonfigurowaniu zasad kopii zapasowych wyświetlany jest komunikat ostrzegawczy „Proces tworzenia kopii zapasowych Azure nie został skonfigurowany dla tego serwera” <br/>
Ostrzeżenie to występuje, gdy ustawienia harmonogramu tworzenia kopii zapasowych przechowywane na serwerze lokalnym nie są takie same jak ustawienia przechowywane w magazynie kopii zapasowych. Gdy serwer lub ustawienia zostały odzyskane do znanego, dobrego stanu, harmonogramy tworzenia kopii zapasowych mogą utracić synchronizację. W przypadku wystąpienia tego ostrzeżenia należy [zmienić konfigurację zasad tworzenia kopii zapasowej](backup-azure-manage-windows-server.md), a następnie wybrać opcję **Wykonaj kopię zapasową**, aby ponownie zsynchronizować lokalny serwer z platformą Azure.



## <a name="azure-backup-encryption"></a>Szyfrowanie w usłudze Azure Backup

### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Czy dane wysyłane do platformy Azure są szyfrowane? <br/>
Tak. Dane są szyfrowane na lokalnym serwerze/kliencie/komputerze z programem SCDPM za pomocą algorytmu szyfrowania AES-256, a następnie przesyłane za pośrednictwem bezpiecznego łącza HTTPS.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Czy dane kopii zapasowej na platformie Azure są również szyfrowane?<br/>
Tak. Dane wysyłane do platformy Azure pozostają zaszyfrowane (nieaktywne). Firma Microsoft nie odszyfrowuje danych kopii zapasowej w żadnym punkcie. Podczas wykonywania kopii zapasowej maszyny wirtualnej platformy Azure usługa Azure Backup opiera się na szyfrowaniu maszyny wirtualnej. Jeśli na przykład maszyna wirtualna została zaszyfrowana przy użyciu usługi Azure Disk Encryption lub innej technologii szyfrowania, usługa Azure Backup używa tego szyfrowania do zabezpieczenia danych.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Jaka jest minimalna długość klucza szyfrowania używanego do szyfrowania danych w kopii zapasowej? <br/>
Klucz szyfrowania powinien mieć co najmniej 16 znaków.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Co się stanie, jeśli klucz szyfrowania zostanie zgubiony przez użytkownika? Czy użytkownik lub firma Microsoft może odzyskać dane? <br/>
Klucz używany do szyfrowania danych kopii zapasowej jest przechowywany tylko lokalnie przez klienta. Firma Microsoft nie przechowuje kopii klucza na platformie Azure i nie ma do niego dostępu. Jeśli klient zgubi klucz, firma Microsoft nie może odzyskać danych kopii zapasowej.

