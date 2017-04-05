# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Regiony i dostępność maszyn wirtualnych na platformie Azure
Ważne jest, aby zrozumieć, jak i gdzie maszyny wirtualne działają na platformie Azure, a także poznać opcje maksymalizowania wydajności, dostępności i nadmiarowości. Platforma Azure działa w wielu centrach danych na całym świecie. Te centra danych są grupowane w regiony geograficzne, dzięki czemu można elastycznie wybierać miejsca do kompilowania aplikacji. Ten artykuł zawiera omówienie funkcji dostępności i nadmiarowości platformy Azure.

## <a name="what-are-azure-regions"></a>Co to są regiony platformy Azure?
Platforma Azure umożliwia tworzenie zasobów, takich jak maszyny wirtualne, w określonych regionach geograficznych, takich jak „Zachodnie stany USA”, „Europa Północna” lub „Azja Południowo-Wschodnia”. Obecnie na świecie jest 30 regionów platformy Azure. Możesz zapoznać się z [listą regionów i ich lokalizacji](https://azure.microsoft.com/regions/). W każdym regionie istnieje wiele centrów danych, co zapewnia nadmiarowość i dostępność. To podejście zapewnia elastyczność podczas kompilowania aplikacji w celu utworzenia maszyn wirtualnych najbliżej użytkowników, a także spełnia wszelkie wymagania prawne oraz dotyczące zgodności i podatków.

## <a name="special-azure-regions"></a>Specjalne regiony platformy Azure
Istnieją specjalne regiony platformy Azure przeznaczone do celów związanych ze zgodnością lub prawem, których możesz użyć podczas kompilowania aplikacji. Te regiony specjalne to:

* **Administracja USA — Wirginia** i **Administracja USA — Iowa**
  * Wystąpienie platformy Azure odizolowane fizycznie i na poziomie sieci logicznej dla instytucji rządowych oraz obsługiwane przez sprawdzony pod kątem bezpieczeństwa personel z obywatelstwem Stanów Zjednoczonych. Uwzględnia dodatkowe certyfikaty zgodności, takie jak [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) i [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Dowiedz się więcej o [platformie Azure Government](https://azure.microsoft.com/features/gov/).
* **Indie Środkowe**, **Indie Zachodnie** i **Indie Południowe**
  * Te regiony są obecnie dostępne dla klientów korzystających z licencjonowania zbiorowego i partnerów zarejestrowanych lokalnie w Indiach. Od 2016 r. użytkownicy mogą uzyskiwać do nich dostęp, jeśli zakupili bezpośrednie subskrypcje online.
* **Chiny Północne** i **Chiny Wschodnie**
  * Te regiony są dostępne dzięki unikatowemu partnerstwu firm Microsoft i 21Vianet, w ramach którego firma Microsoft nie zarządza bezpośrednio centrami danych. Dowiedz się więcej na temat [platformy Microsoft Azure w Chinach](http://www.windowsazure.cn/).
* **Niemcy Środkowe** i **Niemcy Północno-Wschodnie**
  * Te regiony są obecnie dostępne za pośrednictwem modelu powiernika danych, w którym dane klientów pozostają pod kontrolą T-Systems, niemieckiej firmy telekomunikacyjnej działającej jako niemiecki powiernik danych.

## <a name="region-pairs"></a>Pary regionów
Każdy region platformy Azure jest powiązany z innym regionem w obrębie tego samego obszaru geograficznego (takiego jak Stany Zjednoczone, Europa i Azja). To podejście umożliwia replikację zasobów, takich jak maszyny wirtualne, między obszarami geograficznymi, co powinno zmniejszyć prawdopodobieństwo tego, że klęski żywiołowe, niepokoje społeczne, przerwy w dostawie prądu lub awarie sieci fizycznych będą wpływać na obydwa regiony na raz. Dodatkowe korzyści wynikające z tworzenia par regionów:

* W przypadku awarii platformy Azure o większym zasięgu jeden region z każdej pary ma przydzielany wyższy priorytet, co pomaga zredukować czas wymagany do przywrócenia aplikacji. 
* Zaplanowane aktualizacje platformy Azure są wdrażane w powiązanych regionach pojedynczo, aby zminimalizować przestoje i ryzyko awarii aplikacji.
* Dla celów związanych z podatkami i egzekwowaniem prawa dane przez cały czas znajdują się w tym samym obszarze geograficznym, co para (z wyjątkiem regionu Brazylia Południowa).

Przykłady par regionów:

| Podstawowy | Pomocniczy |
|:--- |:--- |
| Zachodnie stany USA |Wschodnie stany USA |
| Europa Północna |Europa Zachodnia |
| Azja Południowo-Wschodnia |Azja Wschodnia |

Zobacz pełną [listę par regionów tutaj](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Dostępność funkcji
Niektóre usługi lub funkcje maszyn wirtualnych, takie jak określone rozmiary maszyn wirtualnych lub typy magazynu, są dostępne tylko w określonych regionach. Niektóre globalne usługi platformy Azure, takie jak [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) lub [Azure DNS](../articles/dns/dns-overview.md), nie wymagają wybrania określonego regionu. Aby ułatwić sobie projektowanie środowiska aplikacji, sprawdź [dostępność usług Azure w poszczególnych regionach](https://azure.microsoft.com/regions/#services). 

## <a name="storage-availability"></a>Dostępność magazynu
Zrozumienie sposobu działania regionów i obszarów geograficznych platformy Azure staje się ważne, jeśli chcesz skorzystać z dostępnych opcji replikacji magazynu. W zależności od typu magazynu masz do wyboru różne opcje replikacji.

**Azure Managed Disks**
* Magazyn lokalnie nadmiarowy (LRS)
  * Umożliwia trzykrotne replikowanie danych w regionie, w którym utworzono konto magazynu.

**Dyski oparte na koncie magazynu**
* Magazyn lokalnie nadmiarowy (LRS)
  * Umożliwia trzykrotne replikowanie danych w regionie, w którym utworzono konto magazynu.
* Magazyn strefowo nadmiarowy (ZRS)
  * Umożliwia trzykrotne replikowanie danych w ramach dwóch lub trzech obiektów, w jednym lub dwóch regionach.
* Magazyn geograficznie nadmiarowy (GRS)
  * Umożliwia replikowanie danych do regionu pomocniczego, który jest oddalony od regionu podstawowego o setki kilometrów.
* Magazyn geograficznie nadmiarowy dostępny do odczytu (RA-GRS)
  * Umożliwia replikowanie danych do regionu pomocniczego, podobnie jak w przypadku magazynu GRS, ale oferuje także dostęp tylko do odczytu do danych w lokalizacji pomocniczej.

W poniższej tabeli przedstawiono krótkie podsumowanie różnic między typami replikacji magazynu:

| Strategia replikacji | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Dane są replikowane między wieloma obiektami. |Nie |Tak |Tak |Tak |
| Dane mogą być odczytywane z lokalizacji pomocniczej i z lokalizacji podstawowej. |Nie |Nie |Nie |Tak |
| Liczba kopii danych obsługiwanych w osobnych węzłach. |3 |3 |6 |6 |

Aby uzyskać więcej informacji, zobacz [opcje replikacji magazynu Azure Storage tutaj](../articles/storage/storage-redundancy.md). Aby uzyskać więcej informacji o dyskach zarządzanych, zobacz [Omówienie usługi Azure Managed Disks](../articles/storage/storage-managed-disks-overview.md).

### <a name="storage-costs"></a>Koszty magazynowania
Ceny różnią się w zależności od wybranego typu magazynu i dostępności.

**Azure Managed Disks**
* Dyski Managed Disks w warstwie Premium opierają się na dyskach półprzewodnikowych (SSD), a dyski Managed Disks w warstwie Standardowa — na zwykłych dyskach obrotowych. Opłaty za dyski Managed Disks w warstwach Premium i Standardowa są naliczane zgodnie z aprowizowaną pojemnością dysku.

**Dyski niezarządzane**
* Magazyn w warstwie Premium opiera się na dyskach SSD, a opłaty są naliczane na podstawie pojemności dysku.
* Magazyn w warstwie Standardowa opiera się na zwykłych dyskach obrotowych, a opłaty są naliczane na podstawie używanej pojemności i żądanej dostępności magazynu.
  * W przypadku magazynów RA-GRS istnieje dodatkowa opłata za transfer danych replikacji geograficznej związana z przepustowością wykorzystywaną do replikowania tych danych do innego regionu platformy Azure.

Zobacz [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/), aby uzyskać informacje na temat różnych typów magazynów i opcji dostępności.

## <a name="azure-images"></a>Obrazy platformy Azure
Na platformie Azure maszyny wirtualne są tworzone na podstawie obrazu. Zazwyczaj obrazy pochodzą z witryny [Azure Marketplace](https://azure.microsoft.com/marketplace/), w której partnerzy mogą oferować wstępnie skonfigurowane obrazy kompletnego systemu operacyjnego lub aplikacji.

W przypadku tworzenia maszyny wirtualnej na podstawie obrazu w witrynie Azure Marketplace w rzeczywistości pracujesz z szablonami. Szablony usługi Azure Resource Manager to deklaratywne pliki JSON (JavaScript Object Notation), które mogą służyć do tworzenia złożonych środowisk aplikacji obejmujących maszyny wirtualne, magazyn, sieci wirtualne itd. Zapoznaj się z dodatkowymi informacjami o [szablonach usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), w tym czynnościami wymaganymi do [skompilowania własnych szablonów](../articles/resource-group-authoring-templates.md).

Możesz również tworzyć własne niestandardowe obrazy i przekazywać je za pomocą [interfejsu wiersza polecenia platformy Azure](../articles/virtual-machines/linux/upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub [programu Azure PowerShell](../articles/virtual-machines/windows/upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), aby szybko tworzyć niestandardowe maszyny wirtualne spełniające określone wymagania dotyczące kompilacji.

## <a name="availability-sets"></a>Zestawy dostępności
Zestaw dostępności to logiczne grupowanie maszyn wirtualnych, które umożliwia platformie Azure zrozumienie sposobu kompilacji aplikacji w celu zapewnienia nadmiarowości i dostępności. Zalecane jest, aby w celu zapewnienia wysokiej dostępności aplikacji i spełnienia warunków [umowy SLA platformy Azure gwarantującej dostępność przez 99,95% czasu](https://azure.microsoft.com/support/legal/sla/virtual-machines/) utworzyć w zestawie dostępności co najmniej dwie maszyny wirtualne. Jeśli pojedyncza maszyna wirtualna korzysta z usługi [Azure Premium Storage](../articles/storage/storage-premium-storage.md), w przypadku zdarzeń nieplanowanej konserwacji obowiązuje umowa SLA platformy Azure. Zestaw dostępności zawiera dwa dodatkowe grupowania, które chronią przed awariami sprzętu i umożliwiają bezpieczne stosowanie aktualizacji — domeny błędów i domeny aktualizacji.

![Rysunek koncepcyjny przedstawiający konfigurację domeny aktualizacji i domeny błędów](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

Zapoznaj się z dodatkowymi informacjami na temat zarządzania dostępnością [maszyn wirtualnych z systemem Linux](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub [maszyn wirtualnych z systemem Windows](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="fault-domains"></a>Domeny błędów
Domena błędów to logiczne grupowanie odpowiednich elementów sprzętu, które współdzielą źródła zasilania i przełącznik sieciowy, podobnie jak w przypadku regału w lokalnym centrum danych. Podczas tworzenia maszyn wirtualnych w zestawie dostępności platforma Azure automatycznie rozdziela maszyny wirtualne między domeny błędów. To podejście ogranicza wpływ potencjalnych awarii sprzętu fizycznego, przestojów sieci lub przerw w dostawie prądu.

#### <a name="managed-disk-fault-domains-and-availability-sets"></a>Zestawy dostępności i domeny błędów usługi Managed Disks
Maszyny wirtualne korzystające z usługi [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md) są przydzielane do domen błędów dysków zarządzanych w przypadku korzystania z zarządzanego zestawu dostępności. Dzięki takiemu dopasowaniu wszystkie dyski zarządzane dołączone do maszyny wirtualnej działają w tej samej domenie błędów dysku zarządzanego. W zarządzanym zestawie dostępności można tworzyć tylko maszyny wirtualne z użyciem dysków zarządzanych. Liczba domen błędów dysku zarządzanego zależy od regionu — dwie lub trzy domeny błędów dysku zarządzanego na region.

### <a name="update-domains"></a>Domeny aktualizacji
Domena aktualizacji to logiczne grupowanie odpowiednich elementów sprzętu, które mogą być w tym samym czasie poddawane konserwacji lub ponownie uruchamiane. Podczas tworzenia maszyn wirtualnych w zestawie dostępności platforma Azure automatycznie rozdziela maszyny wirtualne między domeny aktualizacji. To podejście zapewnia, że zawsze działa co najmniej jedno wystąpienie aplikacji, gdy platforma Azure jest poddawana okresowej konserwacji. Podczas planowanej konserwacji domeny aktualizacji mogą nie być ponownie uruchamiane kolejno, ale w danym momencie tylko jedna domena aktualizacji jest uruchamiana ponownie.

## <a name="next-steps"></a>Następne kroki
Możesz teraz rozpocząć korzystanie z tych funkcji dostępności i nadmiarowości podczas kompilowania środowiska platformy Azure. Aby uzyskać informacje o najlepszych rozwiązaniach, zobacz [Azure availability best practices](../articles/best-practices-availability-checklist.md) (Najlepsze rozwiązania dotyczące dostępności platformy Azure).

