---
title: Omówienie usługi Azure Load Balancer w warstwie standardowa | Dokumentacja firmy Microsoft
description: Omówienie funkcji usługi Azure Standard Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: kumud
ms.openlocfilehash: 17b4bc68b2dc996134626b1822cfd17f0a9a7572
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161645"
---
# <a name="azure-load-balancer-standard-overview"></a>Omówienie standardowego modułu równoważenia obciążenia na platformie Azure

Usługa Azure Load Balancer umożliwia skalowanie aplikacji i zapewniać wysoką dostępność usług. Moduł równoważenia obciążenia może służyć do scenariuszy dla ruchu przychodzącego, a także ruchu wychodzącego i zapewnia małe opóźnienia i wysoką przepływność i skaluje nawet miliony przepływów dla wszystkich aplikacji TCP i UDP. 

Ten artykuł koncentruje się na standardowych modułu równoważenia obciążenia.  Aby uzyskać bardziej ogólne omówienie modułu równoważenia obciążenia Azure, przejrzyj [Omówienie usługi Load Balancer](load-balancer-overview.md) także.

## <a name="what-is-standard-load-balancer"></a>Co to jest Balancer w warstwie standardowa?

Load Balancer w warstwie standardowa to nowy produkt modułu równoważenia obciążenia dla wszystkich protokołów TCP i UDP aplikacji za pomocą funkcji rozwinięte i bardziej szczegółową ustawić przez podstawowy moduł równoważenia obciążenia.  Dostępnych jest wiele wspólnego, należy zapoznać się z różnicami, zgodnie z opisem w tym artykule.

Można użyć standardowego modułu równoważenia obciążenia jako publiczny lub wewnętrzny moduł równoważenia obciążenia. I maszynę wirtualną można połączyć z publicznego i jednego zasobu wewnętrznego modułu równoważenia obciążenia.

Funkcje zasobów modułu równoważenia obciążenia zawsze są wyrażane jako frontonu, reguły, sondy kondycji i definicji puli zaplecza.  Zasób może zawierać wiele reguł. Można umieścić maszyny wirtualne w puli zaplecza, określając puli wewnętrznej bazy danych z maszyny wirtualnej karty Sieciowej zasobu.  Ten parametr jest przekazywany za pośrednictwem profilu sieci i rozszerzona, gdy za pomocą zestawów skalowania maszyn wirtualnych.

Jednym z kluczowych aspektów jest zakres sieci wirtualnej dla zasobu.  Podstawowy moduł równoważenia obciążenia istnieje w zakresie zestawie dostępności, standardowego modułu równoważenia obciążenia jest w pełni zintegrowana z zakresu sieci wirtualnej i zastosować wszystkie koncepcji sieci wirtualnych.

Zasobów modułu równoważenia obciążenia są obiekty, w których można wyrazić jak Azure powinien program swoją infrastrukturę wielodostępnych do osiągnięcia scenariusza, który chcesz utworzyć.  Nie ma bezpośredniej relacji między zasobami usługi równoważenia obciążenia i rzeczywistej infrastruktury. Tworzenie modułu równoważenia obciążenia nie tworzy wystąpienia, jest zawsze dostępna pojemność i nie uruchamiania lub opóźnienia, które należy rozważyć skalowanie. 

>[!NOTE]
> Platforma Azure udostępnia mechanizm równoważenia w pełni zarządzanych rozwiązań dla swoich scenariuszy.  Jeśli szukasz, kończenie żądań protokołu TLS ("odciążanie protokołu SSL") lub na przetwarzanie warstwy aplikacji żądania HTTP/HTTPS, zapoznaj się z [Application Gateway](../application-gateway/application-gateway-introduction.md).  Jeśli szukasz globalnego DNS równoważenia obciążenia, przejrzyj [usługi Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Scenariuszy end-to-end mogą korzystać z łączenia tych rozwiązań, zgodnie z potrzebami.

## <a name="why-use-standard-load-balancer"></a>Dlaczego warto używać standardowego modułu równoważenia obciążenia?

Usługa Load Balancer w warstwie Standardowa pozwala skalować aplikacje i zapewniać wysoką dostępność zarówno wdrożeniom o małej skali, jak i dużym oraz złożonym architekturom obejmującym wiele stref.

Przejrzeć tabelę poniżej omówienie różnic między standardowego modułu równoważenia obciążenia i podstawowego modułu równoważenia obciążenia:

>[!NOTE]
> Nowe projekty powinna przyjąć standardowego modułu równoważenia obciążenia. 

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Przegląd [limitów usług dla usługi równoważenia obciążenia](https://aka.ms/lblimits), jak i [ceny](https://aka.ms/lbpricing), i [umowy SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Pula zaplecza

Pule zaplecza modułu równoważenia obciążenia w warstwie standardowa rozszerza się na żaden zasób maszynę wirtualną w sieci wirtualnej.  Może zawierać maksymalnie 1000 wewnętrznej bazy danych wystąpień.  Wystąpienie wewnętrznej bazy danych jest konfiguracji IP, czyli własności zasobów kart Sieciowych.

Pula zaplecza może zawierać autonomicznych maszyn wirtualnych, zestawów dostępności lub zestawach skalowania maszyn wirtualnych.  Można tworzyć zasoby w puli zaplecza. Można połączyć maksymalnie 150 zasobów w puli zaplecza dla każdego zasobu modułu równoważenia obciążenia.

Podczas wybierania sposobu projektowania puli zaplecza, można zaprojektować do najmniejszej liczby zasobów w puli zaplecza poszczególnych dodatkowo zoptymalizować czas trwania operacji zarządzania.  Nie ma różnic w wydajności płaszczyzny danych lub skali.

### <a name="probes"></a>Sondy kondycji
  
Load Balancer w warstwie standardowa dodaje obsługę [sondy kondycji HTTPS](load-balancer-custom-probe-overview.md#httpprobe) (sondy HTTP z otoką zabezpieczeń TLS (Transport Layer)) do dokładnie Monitoruj swoje aplikacje protokołu HTTPS.  

Ponadto, jeśli pula zaplecza całego [sondy w dół](load-balancer-custom-probe-overview.md#probedown), standardowego modułu równoważenia obciążenia zezwala na wszystkie ustanowionych połączeń TCP kontynuować. (Podstawowy moduł równoważenia obciążenia będzie zakończenie wszystkich połączeń TCP dla wszystkich wystąpień).

Przegląd [sondy kondycji modułu równoważenia obciążenia](load-balancer-custom-probe-overview.md) Aby uzyskać szczegółowe informacje.

### <a name="az"></a>Strefy dostępności

Load Balancer w warstwie standardowa obsługuje dodatkowe możliwości w regionach, w której strefy dostępności są dostępne.  Te funkcje są dodawane do wszystkich Balancer w warstwie standardowa zapewnia.  Konfiguracje strefy dostępności są dostępne dla publicznych i wewnętrznych standardowego modułu równoważenia obciążenia.

Inne niż strefowych frontonów stają się strefowo nadmiarowe domyślnie podczas wdrażania w regionie Dzięki strefom dostępności.   Strefowo nadmiarowe frontonu przeżyje błąd stref i jest jednocześnie obsługiwany przez dedykowana infrastruktura we wszystkich strefach. 

Ponadto można zagwarantować frontonu do określonej strefy. Strefowy frontonu udostępni LOS odpowiedniej strefy i jest obsługiwany tylko przez dedykowana infrastruktura w jednej strefie.

Równoważenie obciążenia między strefami jest dostępna dla puli zaplecza, a żaden zasób maszynę wirtualną w sieci wirtualnej mogą być częścią puli zaplecza.

Przegląd [szczegółowe omówienie stref dostępności, powiązane możliwości](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Diagnostyka

Standardowy moduł równoważenia obciążenia zapewnia metryk wielowymiarowych za pomocą usługi Azure Monitor.  Tych metryk można filtrować, pogrupowane i podzielone dla danego wymiaru.  Zapewniają one bieżące i historyczne szczegółowych informacji o wydajności i kondycji usługi.  Usługa Resource Health jest również obsługiwany.  Poniżej przedstawiono krótkie omówienie obsługiwanych diagnostyki:

| Metryka | Opis |
| --- | --- |
| Dostępność adresu VIP | Standardowy moduł równoważenia obciążenia nieustannie korzysta ze ścieżki danych z w obrębie regionu frontonu do stosu SDN, który obsługuje Twoja maszyna wirtualna modułu równoważenia obciążenia. Tak długo, jak utrzymać dobrej kondycji wystąpień, pomiar następuje taką samą ścieżkę ruchu ze zrównoważonym obciążeniem w danej aplikacji. Ścieżka danych, który jest używany przez klientów jest również sprawdzany. Miara jest niewidoczny dla aplikacji i nie kolidują z innymi operacjami.|
| Dostępność DIP | Standardowy moduł równoważenia obciążenia używa kondycji rozproszonej, sondowanie usługi, która monitoruje kondycję swojej aplikacji punktu końcowego zgodnie z ustawieniami konfiguracji. Ta metryka zapewnia agregacji lub na punkt końcowy filtrowania widoku każdego punktu końcowego poszczególnych wystąpień, w module równoważenia obciążenia w puli.  Aby zobaczyć, jak moduł równoważenia obciążenia widoków kondycji aplikacji wskazane przez konfigurację sondy kondycji.
| SYN pakietów | Standardowy moduł równoważenia obciążenia nie zakończyć połączenia TCP lub korzystać z protokołu TCP lub UDP przepływów pakietów. Przepływów i ich uzgodnienia są zawsze między źródłem i wystąpienie maszyny Wirtualnej. Aby lepiej rozwiązywać scenariuszy protokołu TCP, można skorzystać z SYN liczniki pakietów, aby zrozumieć, jak wiele połączeń TCP prób. Metryka zgłasza liczbę pakietów TCP SYN, które zostały odebrane.|
| Połączeń SNAT | Standardowy moduł równoważenia obciążenia zgłasza liczbę przepływy wychodzące, które są masqueraded na publiczny adres IP frontonu. Porty SNAT to zasób wyczerpującymi. Ta metryka może stanowić wskazówkę stopnia aplikacji powołuje się na SNAT dla ruchu wychodzącego przepływów.  Liczniki dla udane i nieudane przepływy wychodzące SNAT są zgłaszane i może służyć do rozwiązywania oraz zrozumieć kondycję swoich przepływów ruchu wychodzącego.|
| Liczniki bajtów | Standardowy moduł równoważenia obciążenia raporty danych przetworzonych w ramach frontonu.|
| Liczniki pakietów | Standardowy moduł równoważenia obciążenia raporty pakietów przetworzonych w ramach frontonu.|

Przegląd [szczegółowe omówienie standardowa diagnostykę modułu równoważenia obciążenia](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>Zaświadczanie o kondycji portów

Load Balancer w warstwie standardowa obsługuje nowy typ reguły.  

Można skonfigurować reguły skalowania aplikacji i jest wysoce niezawodne równoważenia obciążenia. Kiedy używasz porty wysokiej dostępności reguły równoważenia obciążenia standardowego modułu równoważenia obciążenia zapewni na przepływ równoważenia obciążenia na każdym portów efemerycznych adresu IP frontonu dla wewnętrznego standardowego modułu równoważenia obciążenia.  Ta funkcja jest przydatne w przypadku innych scenariuszach, gdzie jest to niepraktyczne lub niepożądane, aby określić poszczególnych portów.

Reguły równoważenia obciążenia porty wysokiej dostępności umożliwia tworzenie aktywny / pasywny lub aktywny aktywny n + 1 scenariusze dla wirtualnych urządzeń sieciowych oraz dowolnej aplikacji, która wymaga duże zakresy portów przychodzących.  Sonda kondycji może służyć do określenia, które zaplecza powinni otrzymywać nowych przepływów.  Sieciowa grupa zabezpieczeń służy do emulowania scenariusz zakresu portów.

>[!IMPORTANT]
> Jeśli planujesz używać wirtualnego urządzenia sieciowego, skontaktuj się z dostawcą, aby uzyskać wskazówki dotyczące tego, czy ich produkt był testowany z porty wysokiej dostępności, a następnie postępuj zgodnie z ich dokładne wskazówki dotyczące implementacji. 

Przegląd [szczegółowe omówienie portów wysokiej dostępności](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Zabezpieczanie domyślnie

Load Balancer w warstwie standardowa jest w pełni dołączona do sieci wirtualnej.  Sieć wirtualna jest sieci prywatne i zamknięte.  Ponieważ standardowych modułów równoważenia obciążenia i standardowe publiczne adresy IP pozwalają tej sieci wirtualnej jako dostępne spoza sieci wirtualnej, te zasoby teraz domyślnie chyba że je otworzyć. Oznacza to, aby jawnie zezwolić na teraz są używane grupy zabezpieczeń sieci (NSG) i dozwolonych dopuszczonego ruchu.  Można tworzyć swoje całego wirtualnego centrum danych i określić za pomocą sieciowej grupy zabezpieczeń, co i kiedy powinny być dostępne.  Jeśli nie masz, sieciowej grupy zabezpieczeń w podsieci lub karty Sieciowej zasobu maszyny wirtualnej, ruch nie jest dozwolony do osiągnięcia tego zasobu.

Aby dowiedzieć się więcej na temat sieciowych grup zabezpieczeń i jak stosować je do danego scenariusza, zobacz [sieciowych grup zabezpieczeń](../virtual-network/security-overview.md).

### <a name="outbound"></a> Połączenia wychodzące

Moduł równoważenia obciążenia obsługuje scenariusze ruchu przychodzącego i wychodzącego.  Standardowy moduł równoważenia obciążenia różni się znacznie podstawowego modułu równoważenia obciążenia w odniesieniu do połączeń wychodzących.

Źródło sieci adresu tłumaczenia (SNAT) są używane do mapowania prywatne, wewnętrzne adresy IP w sieci wirtualnej do publicznych adresów IP na frontonów modułu równoważenia obciążenia.

Load Balancer w warstwie standardowa wprowadzono nowy algorytm [niezawodne, skalowalne i przewidywalne algorytm SNAT](load-balancer-outbound-connections.md#snat) i umożliwia nowych możliwości, usuwa niejednoznaczności i wymusza jawnej konfiguracji zamiast po stronie efekty. Zmiany te są niezbędne w celu umożliwienia o nowych funkcjach wyłaniać się. 

Oto kluczowe założenia pamiętać podczas pracy z usługą Load Balancer w warstwie standardowa:

- uzupełnianie reguły zależy od zasobu modułu równoważenia obciążenia.  wszystkie programowania platformy Azure jest pochodną od jego konfiguracji.
- Jeśli dostępnych jest wiele frontonów, używane są wszystkie frontonów i każdym frontonem mnoży liczbę dostępnych portów SNAT
- można wybrać i kontroli, jeśli nie chcesz dla określonego serwera sieci Web, w której ma być używany dla połączeń wychodzących.
- scenariusze dotyczące ruchu wychodzącego mają jawne i łączności wychodzącej nie istnieje, dopóki nie został określony.
- reguły równoważenia obciążenia wnioskować o tym, jak użyć funkcji SNAT względem jest programowane. Reguły równoważenia obciążenia jest specyficzna dla protokołu. SNAT to protokół określonych i konfiguracji należy odzwierciedlenie tej sytuacji zamiast tworzyć efekt uboczny.

#### <a name="multiple-frontends"></a>Wiele frontonów
Jeśli chcesz więcej portów SNAT, ponieważ są oczekiwane lub już występuje duże zapotrzebowanie dla połączeń wychodzących, można również dodać przyrostowe spisu portu SNAT, przez skonfigurowanie dodatkowych frontonów, reguł i pule zaplecza do tej samej maszyny wirtualnej zasoby.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Frontonu, która jest używana do kontroli ruchu wychodzącego
Jeśli chcesz ograniczyć połączenia wychodzące tylko tak, jakby pochodził z adresu IP określonego serwera sieci Web, opcjonalnie można wyłączyć SNAT wychodzących w regule, który wyrazi mapowania ruchu wychodzącego.

#### <a name="control-outbound-connectivity"></a>Łączność wychodząca kontroli
Standardowy moduł równoważenia obciążenia istnieje w ramach sieci wirtualnej.  Sieć wirtualna jest izolowane sieci prywatnej.  Łączności publicznej nie jest dozwolona, chyba że istnieje skojarzenie z publicznym adresem IP.  Możesz docierać do [punkty końcowe usługi sieci wirtualnej](../virtual-network/virtual-network-service-endpoints-overview.md) ponieważ są one oraz lokalnych z siecią wirtualną.  Jeśli chcesz nawiązać łączności wychodzącej do miejsca docelowego spoza sieci wirtualnej, masz dwie opcje:
- Przypisz standardowej jednostki SKU publicznego adresu IP jako adres publiczny adres IP na poziomie wystąpienia na zasób maszynę wirtualną lub
- Umieść zasobu maszyny wirtualnej w puli zaplecza publicznych standardowego modułu równoważenia obciążenia.

Oba pozwoli łączności wychodzącej z sieci wirtualnej, aby poza siecią wirtualną. 

Jeśli użytkownik _tylko_ ma wewnętrznego standardowego modułu równoważenia obciążenia skojarzone z pulą zaplecza, w którym znajduje się zasób maszynę wirtualną, maszynę wirtualną można tylko dostęp do zasobów w sieci wirtualnej i [Usługa sieci wirtualnej Punkty końcowe](../virtual-network/virtual-network-service-endpoints-overview.md).  Możesz wykonać czynności opisane w poprzednim akapicie, aby utworzyć łączności wychodzącej.

Łączność wychodząca zasobu maszyny wirtualnej nie jest skojarzona z pozostaje standardowej jednostki SKU jako przed.

Przegląd [szczegółowe omówienie połączeń wychodzących](load-balancer-outbound-connections.md).

### <a name="multife"></a>Wiele frontonów
Usługa Load Balancer obsługuje wiele reguł z wielu frontonów.  Load Balancer w warstwie standardowa rozszerza to scenariusze ruchu wychodzącego.  Scenariusze wychodzące są zasadniczo odwrotność reguły równoważenia obciążenia dla ruchu przychodzącego.  Obciążenia dla ruchu przychodzącego, reguły równoważenia również tworzy skojarzenia dla połączeń wychodzących. Standard Load Balancer, która używa wszystkich frontonów skojarzony zasób maszynę wirtualną za pomocą reguły równoważenia obciążenia.  Ponadto parametru na temat równoważenia obciążenia regułę i umożliwia pomijanie równoważenia reguł na potrzeby łączności wychodzącej, który umożliwia wybranie określonych frontonów, w tym none.

Dla porównania podstawowego modułu równoważenia obciążenia wybiera losowo pojedynczego serwera sieci Web, i nie ma możliwość kontrolowania, który z nich został wybrany.

Przegląd [szczegółowe omówienie połączeń wychodzących](load-balancer-outbound-connections.md).

### <a name="operations"></a> Operacje zarządzania

Standardowe zasoby modułu równoważenia obciążenia istnieje na platformę infrastruktury w zupełnie nowy.  Pozwala to znacznie szybsze operacje zarządzania dla standardowej jednostki SKU i czasów zakończenia są zazwyczaj mniej niż 30 sekund dla każdego zasobu standardowej jednostki SKU.  Należy pamiętać, że pule zaplecza zwiększenia rozmiaru, czas trwania wymaganych do wewnętrznej bazy danych puli zmienia się również wzrost.

Można modyfikować zasobów standardowego modułu równoważenia obciążenia i przenieść standardowy publiczny adres IP z jednej maszyny wirtualnej do innego znacznie szybciej.

## <a name="migration-between-skus"></a>Migracja między jednostkami SKU

Jednostki SKU nie jest modyfikowalna. Wykonaj kroki opisane w tej sekcji, aby przenieść z jednego zasobu jednostki SKU.

>[!IMPORTANT]
>Przeczytaj ten dokument w całości, aby zrozumieć różnice między jednostkami SKU i dokładnie zbadaniu danego scenariusza.  Konieczne może wprowadzić dodatkowe zmiany, aby wyrównać danego scenariusza.

### <a name="migrate-from-basic-to-standard-sku"></a>Migracja z podstawowych do standardowej jednostki SKU

1. Utwórz nowy zasób Standard (modułu równoważenia obciążenia i publiczne adresy IP, zgodnie z potrzebami). Ponownie utworzyć reguły i sondowania definicje.  Jeśli używano wcześniej sondę TCP 443/TCP wcześniej, należy rozważyć zmianę ten protokół sondy sondy protokołu HTTPS i Dodaj ścieżkę.

2. Tworzenie nowego elementu lub aktualizacji istniejącej sieciowej grupie zabezpieczeń, kart Sieciowych lub podsieci do listy dozwolonych ruch ze zrównoważonym sondowania, jak również cały ruch, który chcesz zezwolić.

3. Usuń zasoby podstawowej jednostki SKU (modułu równoważenia obciążenia i publicznych adresów IP, jeśli ma to zastosowanie), ze wszystkich wystąpień maszyny Wirtualnej. Pamiętaj usunęła również wszystkie wystąpienia maszyn wirtualnych zestawu dostępności.

4. Dołącz wszystkie wystąpienia maszyn wirtualnych do nowych zasobów standardowej jednostki SKU.

### <a name="migrate-from-standard-to-basic-sku"></a>Migrowanie od planu Standard do podstawowej jednostki SKU

1. Utwórz nowy zasób podstawowe (modułu równoważenia obciążenia i publiczne adresy IP, zgodnie z potrzebami). Ponownie utworzyć reguły i sondowania definicje.  Zmień sondy protokołu HTTPS na sondę TCP 443/TCP. 

2. Usuń zasoby standardowej jednostki SKU (modułu równoważenia obciążenia i publicznych adresów IP, jeśli ma to zastosowanie), ze wszystkich wystąpień maszyny Wirtualnej. Pamiętaj usunęła również wszystkie wystąpienia maszyn wirtualnych zestawu dostępności.

3. Dołącz wszystkie wystąpienia maszyn wirtualnych do nowych zasobów w ramach podstawowej jednostki SKU.

>[!IMPORTANT]
>
>Istnieją pewne ograniczenia dotyczące użycia, Basic i standardowej jednostki SKU.
>
>Zaświadczanie o kondycji portów i Diagnostyka standardowej jednostki SKU są dostępne tylko w standardowej jednostki SKU. Nie można migrować z standardowej jednostki SKU do podstawowej jednostki SKU i również zachować te funkcje.
>
>Podstawowa i standardowa jednostka SKU ma szereg różnic, co zostało opisane w tym artykule.  Upewnij się, zrozumieć i przygotować się do nich.
>
>Dopasowywanie jednostki SKU musi być używany dla zasobów modułu równoważenia obciążenia i publicznego adresu IP. Nie może mieć kombinację zasobów podstawowej jednostki SKU i standardowej jednostki SKU. Nie można dołączyć autonomicznych maszyn wirtualnych, maszyn wirtualnych w zasobie zestawu dostępności lub zasobów zestawu skalowania maszyn wirtualnych jednocześnie do obu jednostek SKU.

## <a name="region-availability"></a>Dostępność w danym regionie

Standardowy moduł równoważenia obciążenia jest obecnie dostępna we wszystkich regionach chmury publicznej.

## <a name="sla"></a>Umowa SLA

Standardowe moduły równoważenia obciążenia dostępnych z SLA na poziomie 99,99%.  Przegląd [standardowych umów SLA usługi równoważenia obciążenia](https://aka.ms/lbsla) Aby uzyskać szczegółowe informacje.

## <a name="pricing"></a>Cennik

Standard Load Balancer, która jest produktem płatne na podstawie liczby reguł równoważenia obciążenia skonfigurowany i wszystkie dane przychodzące i wychodzące przetworzone. Dla Load Balancer w warstwie standardowa informacje o cenach, odwiedź stronę [cennik usługi Równoważenie obciążenia](https://aka.ms/lbpricing) strony.

## <a name="limitations"></a>Ograniczenia

- Jednostki SKU nie jest modyfikowalna. Nie można zmienić jednostki SKU dla istniejącego zasobu.
- Zasób maszynę wirtualną autonomiczny zestaw dostępności zasobów lub zasobu zestawu skalowania maszyny wirtualnej może odwoływać się jedną jednostką SKU, nigdy nie oba.
- Reguła modułu równoważenia obciążenia nie mogą rozciągać się dwie sieci wirtualne.  Frontony oraz ich wystąpień powiązanych wewnętrznej bazy danych musi znajdować się w tej samej sieci wirtualnej.  
- Frontonów modułu równoważenia obciążenia nie są dostępne w globalne wirtualne sieci równorzędne.
- [Subskrypcja operacje są przenoszone z](../azure-resource-manager/resource-group-move-resources.md) nie są obsługiwane w przypadku standardowych jednostek SKU równoważenia obciążenia i narzędzie PIP zasobów.
- Role procesów roboczych w sieci Web bez sieci wirtualnej i innych usług platformy Microsoft może być dostępny, gdy tylko wewnętrznego standardowego modułu równoważenia obciążenia jest używana z powodu efekt uboczny z jak pre-sieć wirtualna usługi i innych platform services — funkcja. Należy nie polegać na tych odpowiednio obsługi autonomiczną usługą Intune a bazowego platformy mogą ulec zmianie bez powiadomienia. Użytkownik musi zawsze zakładaj, że należy utworzyć [łączności wychodzącej](load-balancer-outbound-connections.md) jawnie, jeśli jest to konieczne, korzystając z wewnętrznego standardowego modułu równoważenia obciążenia tylko.
- Moduł równoważenia obciążenia jest produkt TCP lub UDP do przekierowania portów dla tych określonych protokołów IP i równoważenia obciążenia.  Reguły równoważenia obciążenia i reguł translatora adresów Sieciowych dla ruchu przychodzącego są obsługiwane w przypadku protokołów TCP i UDP i nie jest obsługiwane dla innych protokołów IP, w tym protokołu ICMP. Moduł równoważenia obciążenia nie kończy, odpowiadać lub inny sposób interakcji z ładunku przepływ protokołu UDP lub TCP. Nie jest serwer proxy. Pomyślnej weryfikacji łączności z frontonu musi mieć miejsce wewnątrzpasmowe przy użyciu tego samego protokołu, które są używane w obciążenia równoważenia ruchu przychodzącego translatora adresów Sieciowych regułę lub (TCP lub UDP) _i_ co najmniej jedną z maszyn wirtualnych należy wygenerować odpowiedzi dla klienta Aby zobaczyć odpowiedź z frontonu.  Nie odbiera odpowiedź wewnątrzpasmowe z frontonu modułu równoważenia obciążenia oznacza, że żadne maszyny wirtualne zostały przygotowane.  Nie jest możliwe do interakcji z usługą Load Balancer frontonu nie może odpowiadać maszyny wirtualnej.  Dotyczy to również połączenia wychodzące gdzie [maskująca portu SNAT](load-balancer-outbound-connections.md#snat) jest obsługiwana tylko w przypadku protokołów TCP i UDP; żadnych innych protokołów IP, ICMP w tym również zakończy się niepowodzeniem.  Przypisz adres publiczny adres IP na poziomie wystąpienia, aby uniknąć.
- Inaczej niż w przypadku publicznych modułów równoważenia obciążenia, które zawierają [połączeń wychodzących](load-balancer-outbound-connections.md) podczas przenoszenia z prywatnych adresów IP w sieci wirtualnej do publicznych adresów IP, wewnętrznego równoważenia obciążenia nie tłumaczenia wychodzącego pochodzi połączenia frontonu dla wewnętrznego modułu równoważenia obciążenia jako znajdują się w prywatnej przestrzeni adresowej IP.  Umożliwia to uniknięcie ryzyko wyczerpania SNAT wewnątrz unikatowy wewnętrznej przestrzeni adresów IP gdzie tłumaczenia nie jest wymagana.  Efekt uboczny jest to, że jeśli przepływu wychodzącego z maszyny Wirtualnej w puli zaplecza prób przepływ frontonu dla wewnętrznego modułu równoważenia obciążenia w puli, która znajduje się _i_ jest mapowany do siebie, zarówno nogi przepływu nie są zgodne i przepływ zakończy się niepowodzeniem. .  Jeśli przepływ nie zamapować powrót do tej samej maszyny Wirtualnej w puli zaplecza, która utworzyła przepływ do frontonu, przepływ zostanie wykonane pomyślnie.   Gdy przepływ mapuje do samego przepływu wychodzącego jest wyświetlana tak, jakby pochodził z maszyny Wirtualnej do frontonu i odpowiednie przepływu ruchu przychodzącego, który pojawia się tak, jakby pochodził z maszyny Wirtualnej do samego siebie. Z punktu widzenia systemu operacyjnego gościa dla ruchu przychodzącego i wychodzącego części takim samym przepływie nie są zgodne na maszynie wirtualnej. Dotyczącą stosu TCP nie rozpozna tych części tego samego przepływu jako część tego samego przepływu jako źródłowe i docelowe nie są zgodne.  Gdy przepływ jest mapowany do innej maszyny Wirtualnej w puli zaplecza, będzie zgodna części przepływu i maszyny Wirtualnej pomyślnie mogą odpowiadać na przepływ.  Objaw w tym scenariuszu jest połączenie przekroczeń limitu czasu. Kilka typowych obejściach, niezawodne realizacji tego scenariusza (pochodzące przepływy z puli zaplecza na serwer zaplecza pul odpowiednich wewnętrznych frontonu modułu równoważenia obciążenia) które obejmują albo wstawiania serwer proxy innych firm za obciążenia wewnętrznego Moduł równoważenia lub [przy użyciu reguł stylu DSR](load-balancer-multivip-overview.md).  Podczas korzystania z publicznej usługi Load Balancer, można rozwiązać, wynikowy scenariusz jest podatna na [wyczerpania SNAT](load-balancer-outbound-connections.md#snat) i należy ich unikać, chyba że uważnie zarządzane.

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o korzystaniu z [Balancer w warstwie standardowa i strefy dostępności](load-balancer-standard-availability-zones.md).
- Dowiedz się więcej o [sond kondycji](load-balancer-custom-probe-overview.md).
- Dowiedz się więcej o [strefy dostępności](../availability-zones/az-overview.md).
- Dowiedz się więcej o [diagnostykę modułu równoważenia obciążenia standardowy](load-balancer-standard-diagnostics.md).
- Dowiedz się więcej o [obsługiwane metryk wielowymiarowych](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) dotyczące diagnostyki w [usługi Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Dowiedz się więcej o korzystaniu z [modułu równoważenia obciążenia dla połączeń wychodzących](load-balancer-outbound-connections.md).
- Dowiedz się więcej o [reguł dla ruchu wychodzącego](load-balancer-outbound-rules-overview.md).
- Dowiedz się więcej o [TCP Resetowanie bezczynności](load-balancer-tcp-reset.md).
- Dowiedz się więcej o [standardowego modułu równoważenia obciążenia za pomocą reguł równoważenia obciążenia na porty wysokiej dostępności](load-balancer-ha-ports-overview.md).
- Dowiedz się więcej o korzystaniu z [równoważenia obciążenia za pomocą wielu frontonów](load-balancer-multivip-overview.md).
- Dowiedz się więcej o [sieci wirtualnych](../virtual-network/virtual-networks-overview.md).
- Dowiedz się więcej o [sieciowych grup zabezpieczeń](../virtual-network/security-overview.md).
- Dowiedz się więcej o [punkty końcowe usługi sieci wirtualnej](../virtual-network/virtual-network-service-endpoints-overview.md).
- Poznaj inne kluczowe [możliwości sieciowe](../networking/networking-overview.md) na platformie Azure.
- Dowiedz się więcej o [modułu równoważenia obciążenia](load-balancer-overview.md).
