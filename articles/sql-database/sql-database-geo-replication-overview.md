---
title: Praca awaryjna grupy i aktywnej replikacji geograficznej — usługi Azure SQL Database | Dokumentacja firmy Microsoft
description: Używanie grup automatyczny tryb failover przy użyciu aktywnej replikacji geograficznej i włączyć automatyczny tryb failover w przypadku awarii.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/29/2018
ms.openlocfilehash: 3495a923683d78446e61ff0545c7d86023c14bc0
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233858"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Przegląd: Aktywnej grupy replikacji geograficznej i automatyczny tryb failover

Aktywna replikacja geograficzna to funkcja usługi Azure SQL Database, która pozwala na tworzenie replik z możliwością odczytu bazy danych w centrum danych w tej samej lub innej (region).

![Replikacja geograficzna](./media/sql-database-geo-replication-failover-portal/geo-replication.png )

Aktywna replikacja geograficzna została zaprojektowana jako rozwiązanie ciągłości biznesowej, które umożliwia aplikacji wykonać szybkie odzyskiwanie w razie awarii skali centrum danych. Jeśli włączono replikację geograficzną, aplikacja może zainicjować trybu failover do pomocniczej bazy danych w innym regionie platformy Azure. Maksymalnie cztery pomocnicze bazy danych są obsługiwane w tej samej lub różnych regionach i pomocnicze bazy danych można także dostęp tylko do odczytu zapytań. Przełączenie w tryb failover musi być inicjowana ręcznie przez użytkownika lub aplikacji. Po zakończeniu przejścia w tryb failover nową podstawową ma punkt końcowy inne połączenie.

> [!NOTE]
> Aktywna replikacja geograficzna jest dostępna dla wszystkich baz danych we wszystkich warstwach usługi we wszystkich regionach.
> Aktywna replikacja geograficzna nie jest dostępny w wystąpieniu zarządzanym.

Automatyczny tryb failover grupy jest rozszerzeniem aktywnej replikacji geograficznej. Jest przeznaczony do zarządzanie trybem failover wielu baz replikowanej geograficznie danych, jednocześnie przy użyciu aplikacji zainicjować trybu failover lub przez delegowanie trybu failover do wykonania przez usługę SQL Database, w oparciu o użytkownika określonych kryteriów. Te ostatnie umożliwia automatyczne odzyskiwanie wielu powiązanych baz danych w regionie pomocniczym oddalonym po poważnej awarii lub innych nieplanowanego zdarzenia, które powoduje utratę pełnej lub częściowej dostępności usługi SQL Database w regionie podstawowym. Ponadto można użyć odczytu pomocniczych baz danych w celu odciążenia obciążeń związanych z zapytaniami tylko do odczytu. Ponieważ grupy automatyczny tryb failover obejmują wiele baz danych, te bazy danych musi być skonfigurowany na serwerze podstawowym. Podstawowych i pomocniczych serwerów baz danych w grupie trybu failover musi być w tej samej subskrypcji. Automatyczny tryb failover grupy obsługuje replikację wszystkich baz danych w grupie, aby tylko jeden serwer pomocniczy w innym regionie.

> [!NOTE]
> Użyj aktywnej replikacji geograficznej, jeśli wymaganych jest wiele replik pomocniczych.

Jeśli korzystają z aktywnej replikacji geograficznej i dla dowolnej przyczyny Twojej podstawowej bazy danych, zakończy się niepowodzeniem lub po prostu musi zostać przełączony do trybu offline, należy zainicjować trybu failover do dowolnego z dodatkowej bazy danych. Po aktywowaniu trybu failover do jednej z pomocniczych baz danych, wszystkie inne pomocnicze bazy danych są automatycznie łączeni ze nową podstawową. Jeśli używasz grup automatyczny tryb failover do zarządzania odzyskiwanie bazy danych i ewentualnej awarii, która ma wpływ na co najmniej kilka baz danych w grupie skutkuje automatycznej pracy awaryjnej. Można skonfigurować zasady automatyczny tryb failover, który najlepiej odpowiada Twoim potrzebom aplikacji lub zrezygnować z udziału i stosować aktywację ręcznie. Ponadto automatyczny tryb failover grupy zapewniają odczytu i zapisu oraz punktów końcowych odbiornika tylko do odczytu, które pozostają bez zmian podczas pracy w trybie Failover. Czy korzystasz z aktywacji ręcznego lub automatycznego trybu failover, pracy awaryjnej przełącza wszystkie pomocnicze bazy danych w grupie do podstawowej. Po zakończeniu pracy w trybie failover bazy danych rekord DNS jest automatycznie aktualizowana do przekierowania punkty końcowe w nowym regionie.

Możesz zarządzać replikacji i pracy w trybie failover poszczególnych baz danych lub zestaw baz danych na serwerze lub w puli elastycznej przy użyciu aktywnej replikacji geograficznej. Możesz wykonać, przy użyciu:

- [Azure Portal](sql-database-geo-replication-portal.md)
- [Programu PowerShell: Pojedynczą bazę danych](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [Programu PowerShell: Pula elastyczna](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [Programu PowerShell: Grupy trybu Failover](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Języka Transact-SQL: Pojedynczą bazę danych lub elastycznej puli](/sql/t-sql/statements/alter-database-azure-sql-database)
- [Interfejs API REST: Pojedynczej bazy danych](https://docs.microsoft.com/rest/api/sql/replicationlinks)
- [Interfejs API REST: Grupy trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups).

Po przejściu w tryb failover upewnij się, że wymagania dotyczące uwierzytelniania dla serwera i bazy danych są skonfigurowane na nową podstawową. Aby uzyskać więcej informacji, zobacz [zabezpieczeń bazy danych SQL Database po awarii](sql-database-geo-replication-security-config.md). Dotyczy to zarówno do aktywnej grupy replikacji geograficznej i automatyczny tryb failover.

Korzysta z aktywnej replikacji geograficznej [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technologii SQL Server, aby informacje o asynchronicznym replikowaniu przekazane transakcje na podstawowej bazy danych do pomocniczej bazy danych przy użyciu izolacji migawki. Automatyczny tryb failover grupy zapewniają semantykę grupy na podstawie aktywnej replikacji geograficznej, ale jest używany ten sam mechanizm replikacji asynchronicznej. Znajduje się na dowolnym etapie, pomocniczej bazy danych może być nieco za podstawowej bazy danych, danych pomocniczych jest gwarantowane, nigdy nie miała transakcji częściowych. Nadmiarowość między regionami umożliwia aplikacjom szybko odzyskać z trwałą utratę całego centrum danych lub ich części centrum danych spowodowane klęski żywiołowe, krytycznego błędami ludzkimi lub złośliwych działań. Określone dane w celu punktu odzyskiwania znajduje się w temacie [omówienie ciągłości](sql-database-business-continuity.md).

Poniższej ilustracji przedstawiono przykład aktywnej replikacji geograficznej skonfigurowaną główną w regionie północno-środkowe stany USA i dodatkowych w regionie południowo-środkowe stany USA.

![relacji replikacji geograficznej](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Ponieważ pomocniczych baz danych można odczytać, używać celu odciążenia obciążeń tylko do odczytu, takich jak zadania raportowania. Korzystając z aktywnej replikacji geograficznej, jest możliwe utworzenie pomocniczej bazy danych w tym samym regionie przy użyciu podstawowego, ale nie zwiększa odporność aplikacji na katastrofalnych awarii. Jeśli używasz grup automatyczny tryb failover pomocniczej bazy danych zawsze jest tworzony w innym regionie.

Oprócz awarii aktywnej replikacji geograficznej odzyskiwania może służyć w następujących scenariuszach:

- **Migracja bazy danych**: aktywną replikację geograficzną można użyć do migracji bazy danych z jednego serwera do innej na online, przy minimalnych przestojach.
- **Uaktualnienia aplikacji**: możesz utworzyć dodatkowe pomocniczy jako kopia wstecz podczas uaktualniania aplikacji.

Aby osiągnąć rzeczywistych ciągłości działania, dodawanie nadmiarowość bazy danych między centrami danych jest tylko część rozwiązania. Odzyskiwanie aplikacji (usługa) end-to-end, po poważnej awarii wymaga odzyskiwania wszystkich składników wchodzących w skład usługi i usług zależnych. Przykładami tych składników oprogramowania klienta (na przykład przeglądarka za pomocą niestandardowych skryptów JavaScript), frontonów sieci web, magazynu i DNS. Należy koniecznie wszystkie składniki są odporne na awarie tych samych i stają się dostępne w ramach cel czasu odzyskiwania (RTO) aplikacji. W związku z tym należy zidentyfikować wszystkich zależnych usług i zrozumieć gwarancje i możliwości, jakie oferują. Następnie należy wykonać odpowiednie kroki w celu zapewnienia, że funkcje usługi podczas pracy awaryjnej usługi, od których zależy. Aby uzyskać więcej informacji na temat projektowania rozwiązań do odzyskiwania po awarii, zobacz [projektowania rozwiązań w chmurze dla za pomocą odzyskiwania po awarii aktywnej replikacji geograficznej](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Funkcje aktywnej replikacji geograficznej

Funkcja aktywna replikacja geograficzna udostępnia następujące podstawowe funkcje:

- **Automatyczną replikację asynchroniczną**

 Można tylko utworzyć pomocniczą bazę danych, dodając do istniejącej bazy danych. Pomocniczy można utworzyć w dowolnym serwerem Azure SQL Database. Po utworzeniu pomocniczej bazy danych jest wypełniana dane skopiowane z podstawowej bazy danych. Ten proces jest nazywany rozmieszczania. Po utworzeniu pomocniczej bazy danych i zasilany aktualizacje do podstawowej bazy danych są asynchronicznie replikowane do dodatkowej bazy danych automatycznie. Replikacja asynchroniczna oznacza, że transakcje są zatwierdzane w podstawowej bazie danych, zanim będą one replikowane do dodatkowej bazy danych.

- **Odczytu pomocniczych baz danych**

  Aplikacja może uzyskiwać dostęp do dodatkowej bazy danych dla operacji tylko do odczytu przy użyciu podmiotów zabezpieczeń tego samego lub innego, używane do uzyskiwania dostępu do podstawowej bazy danych. Pomocnicze bazy danych działa w trybie izolacji migawki, aby upewnić się, że replikacja aktualizacji podstawową (dziennik powtarzania) nie będzie opóźniony o zapytania wykonywane na serwerze pomocniczym.

  > [!NOTE]
  > Powtarzanie dziennika jest opóźnione w pomocniczej bazie danych, czy są aktualizacje schematu na serwerze podstawowym. Te ostatnie wymaga blokadę schematu w pomocniczej bazie danych.

- **Wiele repliki możliwe do odczytu**

  Co najmniej dwóch dodatkowych baz danych zwiększenia nadmiarowości i poziom ochrony podstawowej bazy danych i aplikacji. Jeśli istnieje wiele pomocniczych baz danych, aplikacji pozostaje chroniony nawet w przypadku awarii jednej z pomocniczych baz danych. Jeśli istnieje tylko jeden pomocniczej bazy danych, a zakończy się niepowodzeniem, aplikacja jest narażony na większe ryzyko, dopiero po utworzeniu nowej pomocniczej bazy danych.

  > [!NOTE]
  > Korzystania z aktywnej replikacji geograficznej tworzenie globalnie rozproszonych aplikacji i musi zapewniać dostęp tylko do odczytu do danych z więcej niż czterech regionów można utworzyć pomocniczego pomocniczej (proces znany jako łańcucha). Dzięki temu mogą osiągnąć niemal nieograniczoną skalę replikacji bazy danych. Dodatkowo łańcuch zmniejsza obciążenie replikacji z podstawowej bazy danych. Jest to kompromis lag zwiększoną replikacji w pomocniczych bazach danych większość typu liść.

- **Obsługa elastycznej puli baz danych**

  Każdej repliki można oddzielnie uczestniczyć w puli elastycznej lub nie być w dowolnej elastycznej puli w ogóle. Wybór puli dla każdej repliki jest oddzielony i nie zależą od konfiguracji dowolnej innej repliki (czy podstawowym lub pomocniczym). Każda pula elastyczna jest zawarty w jednym regionie, w związku z tym wiele replik w tej samej topologii nigdy nie mogą udostępniać puli elastycznej.

- **Można konfigurować obliczeń rozmiaru pomocniczej bazy danych**

  Podstawowych i pomocniczych baz danych muszą mieć taką samą warstwę usług. Również zdecydowanie zaleca się że tej dodatkowej bazy danych jest tworzony przy użyciu tych samych obliczeń rozmiaru (jednostki Dtu lub rdzeni wirtualnych) jako podstawowy. Pomocniczego z niższym rozmiaru obliczeń jest narażony na opóźnienie replikacji zwiększona, potencjalne niedostępności lokacji dodatkowej, a w konsekwencji na ryzyko utraty danych znacznej po przejściu w tryb failover. Dzięki temu usługa opublikowane cel punktu odzyskiwania = nie można zagwarantować 5 s. Inne ryzyko to, że po włączeniu trybu failover aplikacji będzie mieć wpływ na wydajność ze względu na moc obliczeniową niewystarczające nową podstawową, dopóki nie zostanie uaktualniony na większy rozmiar obliczeń. Czas uaktualniania zależy od rozmiaru bazy danych. Ponadto obecnie takie uaktualnienia wymaga podstawowych i pomocniczych baz danych są w trybie online i, w związku z tym, nie można ukończyć zminimalizowaniu wpływu awarii. Jeśli zdecydujesz się utworzyć pomocniczej z niższym rozmiaru obliczeń, wykres wartość procentową operacji We/Wy dziennika w witrynie Azure portal oferuje dobry sposób, aby oszacować rozmiar minimalny obliczeń pomocniczy, który jest wymagany do obsługi obciążenia replikacji. Na przykład, jeśli podstawowej bazy danych jest P6 (1000 jednostek DTU) i jego procent we/wy dziennika to 50% pomocnicza musi wynosić co najmniej P4 (500 jednostek DTU). Możesz również pobrać dane we/wy dziennika przy użyciu [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) lub [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) bazy danych widoków.  Aby uzyskać więcej informacji na temat rozmiarów wystąpień obliczeniowych bazy danych SQL, zobacz [co to są warstwy usługi bazy danych SQL](sql-database-service-tiers.md).

- **Kontrolowane przez użytkownika trybu failover i powrotu po awarii**

  Pomocnicza baza danych jawnie można dostosować do roli głównej w dowolnym momencie przez użytkownika lub aplikacji. Podczas awarii rzeczywistych opcji "nieplanowane" należy który promuje natychmiast pomocniczego jako podstawowy. Gdy nie powiodło się podstawowe odzyskuje i będzie znowu dostępna, system automatycznie oznacza odzyskane podstawowej jako pomocniczego i przełączyć aktualne z nową podstawową. Ze względu na charakter asynchronicznej replikacji niewielką ilość danych może być utracone podczas nieplanowane tryby Failover podstawowego zakończy się niepowodzeniem, przed rozpoczęciem replikacji najnowszych zmian do regionu pomocniczego. Po przejściu podstawowej oraz możliwość definiowania wielu tryb failover, system automatycznie ponownie konfiguruje relacje replikacji i łączy pozostałe pomocnicze bazy danych z nowo wypromowaną podstawową bez konieczności żadnej interwencji użytkownika. Po zminimalizowaniu wpływu awarii, które spowodowało przełączenie w tryb failover może być pożądane, przywrócenie aplikacji do regionu podstawowego. Aby to zrobić, polecenie pracy awaryjnej należy można wywołać z opcją "planowane".

- **Synchronizacja poświadczeń i reguł zapory**

  Firma Microsoft zaleca używanie [bazy danych reguły zapory](sql-database-firewall-configure.md) replikowanej geograficznie baz danych, dzięki czemu te reguły mogą być replikowane z bazą danych, aby upewnić się, wszystkie pomocnicze bazy danych ma te same reguły zapory jako podstawowy. To podejście eliminuje potrzebę stosowania klientów ręcznego konfigurowania i konserwacji reguły zapory na serwerach hostujących zarówno podstawowych i pomocniczych baz danych. Podobnie za pomocą [zawartych użytkowników bazy danych](sql-database-manage-logins.md) danych zapewnia podstawowych i pomocniczych baz danych zawsze mają taki sam dostęp do poświadczeń użytkownika, więc podczas pracy w trybie failover nie ma żadnych przerw w działaniu z powodu niezgodności z logowania i hasła. Z dodatkiem [usługi Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), klienci mogą zarządzać dostępem użytkowników do podstawowych i pomocniczych baz danych i całkowicie eliminując potrzebę zarządzania poświadczeniami w bazach danych.

## <a name="auto-failover-group-capabilities"></a>Funkcje grupy automatyczny tryb failover

Funkcja grup automatyczny tryb failover zapewnia zaawansowane abstrakcji aktywnej replikacji geograficznej, dzięki obsłudze replikacji na poziomie grupy i automatycznej pracy awaryjnej. Ponadto usuwa konieczność, aby zmienić parametry połączenia SQL po włączeniu trybu failover, zapewniając punktów końcowych odbiornika dodatkowe.

> [!NOTE]
> Automatyczny tryb failover nie jest dostępny w wystąpieniu zarządzanym.
>  

- **Grupy trybu failover**

  Można utworzyć jeden lub wiele grup trybu failover między dwoma serwerami w różnych regionach (podstawowych i pomocniczych serwerów). Każda grupa może zawierać jeden lub kilka baz danych, które mogą zostać odzyskane jako jednostka, w przypadku wszystkich lub niektórych podstawowych baz danych staną się niedostępne z powodu awarii w regionie podstawowym.  

- **Serwer podstawowy**

  Serwer, który jest hostem podstawowych baz danych w grupie trybu failover.

- **Serwer pomocniczy**

  Serwer, który jest hostem pomocniczych baz danych w grupie trybu failover. Serwer pomocniczy nie może być w tym samym regionie co serwer podstawowy.

- **Dodawanie baz danych do grupy trybu failover**

  Kilka baz danych na serwerze lub w ramach puli elastycznej można umieścić w tej samej grupy trybu failover. Jeśli dodasz pojedynczej bazy danych do grupy, automatycznie tworzy pomocniczą bazę danych przy użyciu tego samego rozmiaru edition i mocy obliczeniowej. Jeśli podstawowa baza danych znajduje się w puli elastycznej, pomocnicza jest automatycznie tworzony w elastycznej puli o takiej samej nazwie. Po dodaniu bazy danych, która ma już pomocniczej bazy danych serwera pomocniczego, że replikacja geograficzna jest dziedziczona przez grupę.

  > [!NOTE]
  > Podczas dodawania bazy danych, która ma już pomocniczej bazy danych na serwerze, który nie jest częścią grupy pracy awaryjnej, nowym serwerem pomocniczym jest tworzony w serwerze pomocniczym.

- **Odbiornik odczytu i zapisu grupy trybu failover**

  Rekord CNAME systemu DNS utworzonych jako  **&lt;nazwę grupy trybu failover&gt;. database.windows.net** wskazuje bieżący adres URL serwera podstawowego. Umożliwia aplikacji SQL odczytu i zapisu przezroczyste ponownie nawiązać połączenie z podstawowej bazy danych podstawowego zmianie po włączeniu trybu failover.

- **Odbiornik grupy trybu failover tylko do odczytu**

  Rekord CNAME systemu DNS utworzonych jako  **&lt;nazwę grupy trybu failover&gt;. secondary.database.windows.net** wskazującego na adres URL serwera pomocniczego. Umożliwia aplikacji SQL tylko do odczytu do jawnego połączenia do pomocniczej bazy danych, za pomocą określonych reguł równoważenia obciążenia.

- **Zasady automatycznej pracy awaryjnej**

  Domyślnie grupy trybu failover skonfigurowano zasady automatycznej pracy awaryjnej. System wyzwala trybu failover, po wykryciu awarii i upłynął okres prolongaty. System należy zweryfikować, że awaria nie da się ograniczyć przez infrastrukturę wbudowaną wysoką dostępność usługi SQL Database ze względu na skalę wpływ. Jeśli chcesz kontrolować przepływ pracy trybu failover z aplikacji, można wyłączyć automatyczną pracę awaryjną.

- **Zasady trybu failover tylko do odczytu**

  Domyślnie pracy awaryjnej odbiornika tylko do odczytu jest wyłączona. Zapewnia, że wydajność podstawowej nie ulega zmianie gdy pomocnicza jest w trybie offline. Jednak również oznacza, że sesji tylko do odczytu nie będzie można połączyć, dopóki pomocnicza została odzyskana. Jeśli nieakceptowalne przestojów dla sesji tylko do odczytu i OK, aby tymczasowo używać podstawowej dla ruchu tylko do odczytu i odczytu / zapisu, kosztem potencjalnego obniżenia wydajności podstawowych, można włączyć trybu failover dla odbiornika tylko do odczytu. W takiej sytuacji ruch tylko do odczytu zostanie automatycznie przekierowywane do serwera podstawowego Jeśli pomocniczy serwer jest niedostępny.  

- **Ręczna praca awaryjna**

  Należy ręcznie zainicjować trybu failover, w dowolnym momencie, niezależnie od konfiguracji automatycznej pracy awaryjnej. Jeśli nie skonfigurowano zasady automatycznej pracy awaryjnej, ręczna praca awaryjna jest wymagany do odzyskiwania baz danych w grupie trybu failover. Możesz zainicjować wymuszonego lub przyjazna trybu failover (z pełnej synchronizacji danych). One może służyć do przeniesienia aktywnego serwera do regionu podstawowego. Po zakończeniu trybu failover rekordy DNS są automatycznie aktualizowane tak, aby zapewnić łączność z właściwym serwerem.

- **Okres prolongaty z utratą danych**

  Ponieważ podstawowych i pomocniczych baz danych są synchronizowane przy użyciu replikacji asynchronicznej, przełączenie w tryb failover może spowodować utratę danych. Można dostosować zasady automatycznej pracy awaryjnej, aby odzwierciedlić aplikacji tolerancję utraty danych. Konfigurując **GracePeriodWithDataLossHours**, można kontrolować, jak długo system czeka przed przejściem w tryb failover, która prawdopodobnie się wynik utraty danych.

  > [!NOTE]
  > Gdy system wykryje, że bazy danych w grupie są nadal w trybie online (na przykład awaria jedynie wpływ na płaszczyźnie kontroli usługi), natychmiast aktywuje tryb failover przy użyciu pełnej synchronizacji danych (tryb failover przyjazna) niezależnie od wartości ustawionej przez  **GracePeriodWithDataLossHours**. Takie zachowanie gwarantuje, że istnieje bez utraty danych podczas odzyskiwania. Okres prolongaty działa tylko wtedy, gdy jest to przyjazna przejścia w tryb failover nie jest możliwe. Jeśli awaria jest zmniejszany przed upływem okresu prolongaty, pracy w trybie failover nie jest aktywna.

- **Wiele grup trybu failover**

  Można skonfigurować wiele grup trybu failover dla tej samej pary serwerów do kontrolowania skali przejścia w tryb failover. Każda grupa nie powiedzie się za pośrednictwem niezależnie. Jeśli aplikacja wielodostępna używa pul elastycznych, umożliwia ta funkcja mieszać podstawowych i pomocniczych baz danych w każdej puli. W ten sposób można zmniejszyć wpływ awarii połowa dzierżawcy.

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>Najlepsze rozwiązania przy użyciu grupy trybu failover dla ciągłości działania

Podczas projektowania usługi za pomocą ciągłość prowadzenia działalności biznesowej należy pamiętać, należy przestrzegać następujących wytycznych:

- **Użyj jednej lub kilku grup trybu failover można zarządzać trybem failover wielu baz danych**

  Można utworzyć jeden lub wiele grup trybu failover między dwoma serwerami w różnych regionach (podstawowych i pomocniczych serwerów). Każda grupa może zawierać jeden lub kilka baz danych, które mogą zostać odzyskane jako jednostka, w przypadku wszystkich lub niektórych podstawowych baz danych staną się niedostępne z powodu awarii w regionie podstawowym. Grupy trybu failover tworzy pomocniczej geograficznej bazy danych, w tym samym celu usługi jako podstawowy. Jeśli dodasz istniejącej relacji replikacji geograficznej do grupy trybu failover upewnij się, że pomocniczej geograficznej jest skonfigurowany z tej samej warstwy usług i rozmiaru obliczeń jako podstawowy.

- **Użyj odbiornika odczytu i zapisu dla obciążenia OLTP**

  Podczas wykonywania operacji OLTP, użyj  **&lt;nazwę grupy trybu failover&gt;. database.windows.net** jako serwer połączenia i adres URL automatycznie są kierowane do podstawowego. Ten adres URL nie zmienia się po pracy w trybie failover. Należy pamiętać, że przełączenie w tryb failover wymaga aktualizacji, który rekord DNS, dzięki czemu połączenia klienckie są przekierowywane do nowej podstawowej tylko wtedy, gdy klienta pamięci podręcznej DNS są odświeżane.

- **Użyj odbiornika tylko do odczytu dla obciążenia tylko do odczytu**

  Jeśli masz obciążenie logicznie izolowanej tylko do odczytu, która jest odporny na niektórych nieaktualność danych, można użyć pomocniczej bazy danych w aplikacji. W przypadku sesji tylko do odczytu, użyj  **&lt;nazwę grupy trybu failover&gt;. secondary.database.windows.net** jako serwer adresu URL i połączenia automatycznie zostanie skierowany do regionu pomocniczego. Zaleca się wskazanie w parametrach połączenia odczytać intencji za pomocą **ApplicationIntent = tylko do odczytu**.

- **Przygotowanie do obniżenia wydajności**

  Decyzja trybu failover programu SQL jest niezależna od pozostałej części lub innych usług używanych aplikacji. Aplikacja może być wartość "mixed" za pomocą niektóre składniki na jednym regionie, a niektóre w innym. Aby uniknąć obniżenia wydajności, upewnij się, wdrożenie aplikacji nadmiarowy w regionie odzyskiwania po awarii, a następnie postępuj zgodnie z wytycznymi podanymi w tym artykule. Uwaga do aplikacji w regionie odzyskiwania po awarii nie ma używać ciągu innego połączenia.  

- **Przygotowanie do utraty danych**

  W przypadku wykrycia awarii SQL automatyczne wyzwolenie odczytu i zapisu w tryb failover ma zerową utratę danych, najlepiej naszej wiedzy. W przeciwnym razie oczekiwania przez czas określony przez **GracePeriodWithDataLossHours**. Jeśli określono **GracePeriodWithDataLossHours**, można przygotować utraty danych. Ogólnie rzecz biorąc podczas awarii, Azure preferuje dostępności. Jeśli nie możesz zakupić utraty danych, upewnij się, że ustawiony **GracePeriodWithDataLossHours** wystarczająco dużą liczbę, takich jak 24 godziny.

> [!IMPORTANT]
> Elastycznymi pulami za pomocą 800 lub mniejszej liczby jednostek Dtu i ponad 250 baz danych, przy użyciu replikacji geograficznej mogą wystąpić problemy, w tym już planowanego przejścia w tryb failover i pogorszenie wydajności.  Te problemy są najbardziej prawdopodobne w przypadku obciążeń intensywnie korzystających z zapisu podczas replikacji geograficznej punkty końcowe są znacznie oddalonych od siebie według lokalizacji geograficznej lub wiele dodatkowych punktów końcowych są używane dla każdej bazy danych.  Objawy te problemy są wskazane, gdy opóźnienia replikacji geograficznej zwiększa się wraz z upływem czasu.  To opóźnienie można monitorować za pomocą [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Jeśli te problemy występują, środki zaradcze obejmują zwiększenie liczby jednostek Dtu puli lub zmniejszenie liczby replikacji geograficznej bazy danych w tej samej puli.

## <a name="failover-groups-and-network-security"></a>Grupy trybu failover i bezpieczeństwo sieci

W przypadku niektórych aplikacji, które reguły zabezpieczeń wymagają, że dostęp do sieci do warstwy danych jest ograniczony do określonego składnika lub składniki, takie jak maszyny Wirtualnej z systemem usługi sieci web itp. To wymaganie przedstawiono niektóre wyzwania dotyczące projektowania ciągłości biznesowej i sposób korzystania z grup trybu failover. Należy rozważyć następujące opcje podczas implementowania takich ograniczony dostęp.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Korzystanie z reguł sieci wirtualnej i grupy trybu failover

Jeśli używasz [punkty końcowe usługi sieci wirtualnej i reguł](sql-database-vnet-service-endpoint-rule-overview.md) ograniczyć dostęp do bazy danych SQL, należy pamiętać, ten punkt końcowy usługi każdej sieci wirtualnej dotyczy tylko jednego regionu platformy Azure. Punkt końcowy nie uwzględnia innych regionach akceptował komunikację z podsieci. Więc tylko aplikacje klienckie wdrożone w tym samym regionie mogą łączyć się z podstawowej bazy danych. Ponieważ trybu failover zakończyło się z sesji klienta SQL są przekierowane do serwera w innym regionie (informacje pomocnicze), te sesje zakończy się niepowodzeniem, jeśli pochodzi z klienta poza tym regionem. Z tego powodu zasady automatycznej pracy awaryjnej nie można włączyć w przypadku serwerów znajdują się w reguł sieci wirtualnej. Aby zapewnić obsługę ręcznej pracy awaryjnej, wykonaj następujące kroki:

1. Aprowizowanie nadmiarowe kopie składniki frontonu aplikacji (usługa sieci web, maszyny wirtualne itp.) w regionie pomocniczym
2. Konfigurowanie [reguł sieci wirtualnej](sql-database-vnet-service-endpoint-rule-overview.md) osobno dla podstawowego i pomocniczego serwera
3. Włącz [frontonu trybu failover przy użyciu Konfiguracja usługi Traffic manager](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Zainicjuj ręcznego przełączania trybu failover po wykryciu awarii

Ta opcja jest zoptymalizowany pod kątem aplikacji, które wymagają spójne opóźnienie między frontonu i warstwy danych i obsługuje funkcję odzyskiwania, gdy frontonu, warstwy danych lub obu wpływ awarii.

> [!NOTE]
> Jeśli używasz **odbiornika tylko do odczytu** aby zrównoważyć obciążenie tylko do odczytu, upewnij się, że to obciążenie jest wykonywane na maszynie Wirtualnej lub innych zasobów w regionie pomocniczym go do połączenia się z dodatkowej bazy danych.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Korzystanie z grup trybu failover i reguł zapory usługi SQL database

Jeśli planem ciągłości biznesowej wymaga trybu failover przy użyciu grup automatycznego przejścia w tryb failover, można ograniczyć dostęp do usługi SQL database przy użyciu reguł zapory tradycyjnych.  Aby zapewnić obsługę automatycznej pracy awaryjnej, wykonaj następujące kroki:

1. [Tworzenie publicznego adresu IP](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Tworzenie publicznego modułu równoważenia obciążenia](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) i przypisać publiczny adres IP.
3. [Tworzenie sieci wirtualnej i maszynach wirtualnych](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) składników frontonu
4. [Utwórz sieciową grupę zabezpieczeń](../virtual-network/security-overview.md) i skonfigurowanie połączeń przychodzących.
5. Upewnij się, czy połączenia wychodzące są otwarte dla danych Azure SQL database przy użyciu "Sql" [tag usługi](../virtual-network/security-overview.md#service-tags).
6. Tworzenie [reguły zapory bazy danych SQL](sql-database-firewall-configure.md) zezwalająca na ruch przychodzący z publicznego adresu IP, zostanie utworzony w kroku 1.

Aby uzyskać więcej informacji na temat na konfigurowanie wychodzącego dostępu i jakie adres IP do użycia w regułach zapory zobacz [połączeń wychodzących usługi równoważenia obciążenia](../load-balancer/load-balancer-outbound-connections.md).

Powyższa konfiguracja będzie upewnij się, że automatycznej pracy awaryjnej nie blokuje połączenia z składników frontonu i przyjęto założenie, że aplikacja może tolerować dłuższe opóźnienie między frontonem a warstwą danych.

> [!IMPORTANT]
> Aby zagwarantować ciągłość działania w regionalnych przestojów musi zapewnić nadmiarowość geograficzna zarówno składników frontonu, jak i bazy danych.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Uaktualnianie lub zmiany na starszą wersję podstawową bazą danych

Można uaktualnić lub obniżyć podstawowej bazy danych do rozmiaru obliczeniowej (w ramach tej samej wersji usługi) bez odłączania wszystkie pomocnicze bazy danych. Podczas uaktualniania, zalecamy najpierw uaktualnić pomocnicze bazy danych, a następnie uaktualnić podstawowy. Przed obniżeniem, odwracanie kolejności: najpierw obniżenia poziomu podstawowego, a następnie obniżyć wersję usługi dodatkowej. Po uaktualnieniu lub starszą wersję bazy danych do warstwy usług różnych tego zalecenia zostanie wymuszona.

> [!NOTE]
> Jeśli utworzono pomocniczej bazy danych jako część konfiguracji grupy trybu failover, który nie jest zalecane w przypadku obniżania pomocniczej bazy danych. To jest, aby upewnić się, że w warstwie danych ma dostatecznie dużą pojemność przetwarzanie regularne obciążenie, po aktywowaniu trybu failover.

## <a name="preventing-the-loss-of-critical-data"></a>Zapobieganie utracie danych o kluczowym znaczeniu

Ze względu na duże opóźnienia sieci rozległej ciągłych kopii używa mechanizmu replikacji asynchronicznej. Replikacja asynchroniczna sprawia, że utrata danych nieuniknione Jeśli wystąpi awaria. Jednak niektóre aplikacje mogą wymagać bez utraty danych. Aby chronić te aktualizacje krytyczne, Twórca aplikacji może wywołać [operacja sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) procedury system natychmiast po zatwierdzanie transakcji. Wywoływanie **operacja sp_wait_for_database_copy_sync** blokuje wątek wywołujący, aż ostatniej transakcji zatwierdzone została przesłana do pomocniczej bazy danych. Jednak go nie czeka na przesyłane transakcji, które mają być powtórzone i zatwierdzona na serwerze pomocniczym. **Operacja sp_wait_for_database_copy_sync** obejmuje łącze określonych ciągłych kopii. Każdy użytkownik z uprawnieniami połączenia podstawowej bazy danych można wywołać tej procedury.

> [!NOTE]
> **Operacja sp_wait_for_database_copy_sync** zapobiega utracie danych po pracy awaryjnej, ale nie gwarantuje pełną synchronizację, aby uzyskać dostęp do odczytu. Opóźnienie spowodowane **operacja sp_wait_for_database_copy_sync** wywołanie procedury mogą być znaczące i zależy od rozmiaru dziennika transakcji w momencie wywołania.

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Programowe zarządzanie grupy trybu failover i aktywna replikacja geograficzna

Zgodnie z opisem wcześniej grupy automatyczny tryb failover i aktywna replikacja geograficzna można również zarządzać programowo przy użyciu programu Azure PowerShell i interfejsu API REST. W poniższych tabelach opisano zestaw poleceń dostępnych. Aktywna replikacja geograficzna zawiera zestaw interfejsów API usługi Azure Resource Manager do zarządzania, w tym [interfejs API REST usługi Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) i [poleceń cmdlet programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). Te interfejsy API korzystają z grup zasobów i obsługuje zabezpieczenia oparte na rolach (RBAC). Aby uzyskać więcej informacji o tym, jak można zaimplementować ról dostępu, zobacz [kontroli dostępu](../role-based-access-control/overview.md).

### <a name="manage-sql-database-failover-using-transact-sql"></a>Zarządzaj trybem failover bazy danych SQL za pomocą instrukcji języka Transact-SQL

| Polecenie | Opis |
| --- | --- |
| [Instrukcja ALTER DATABASE (baza danych SQL platformy Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Aby utworzyć pomocniczą bazę danych dla istniejącej bazy danych i rozpoczyna replikację danych należy użyć argumentu dodawania serwera POMOCNICZEGO w |
| [Instrukcja ALTER DATABASE (baza danych SQL platformy Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Użyj trybu FAILOVER lub FORCE_FAILOVER_ALLOW_DATA_LOSS, aby przełączyć pomocniczej bazy danych jako główną w celu zainicjowania trybu failover |
| [Instrukcja ALTER DATABASE (baza danych SQL platformy Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Usuń serwer POMOCNICZY ON umożliwia zakończenie replikacji danych między bazą danych SQL i wybraną pomocniczą bazą danych. |
| [sys.geo_replication_links (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Zwraca informacje o wszystkich istniejących linków replikacji dla każdej bazy danych na serwerze logicznym Azure SQL Database. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Pobiera czas ostatniej replikacji, ostatnim opóźnienie replikacji i inne informacje na temat łącze replikacji dla danej bazy danych SQL. |
| [sys.dm_operation_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Wyświetla stan dla wszystkich operacji bazy danych, w tym stan łącza replikacji. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |powoduje, że aplikacja poczekaj, aż wszystkie zatwierdzone transakcje są replikowane i potwierdzone przez aktywnej pomocniczej bazy danych. |
|  | |

### <a name="manage-sql-database-failover-using-powershell"></a>Zarządzaj trybem failover bazy danych SQL przy użyciu programu PowerShell

| Polecenie cmdlet | Opis |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Pobiera co najmniej jedną bazę danych. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Tworzy pomocniczą bazę danych dla istniejącej bazy danych i rozpoczyna replikację danych. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Przełącza pomocniczą bazę danych jako główną w celu zainicjowania trybu failover. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Przerywa replikację danych między bazą danych SQL Database i wybraną pomocniczą bazą danych. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Pobiera linki replikacji geograficznej między bazą danych Azure SQL Database i grupą zasobów lub programem SQL Server. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |To polecenie tworzy grupę trybu failover i rejestruje je na serwerach podstawowych i pomocniczych|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Usuwa grupę trybu failover z serwera i usuwa wszystkie pomocnicze bazy danych są uwzględnione grupy |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Pobiera konfigurację grupy trybu failover |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Modyfikuje konfigurację grupy trybu failover |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Wyzwalacze pracy w trybie failover grupy trybu failover na serwer pomocniczy |
|  | |

> [!IMPORTANT]
> Przykładowe skrypty można zobaczyć [Konfiguruj i pracy awaryjnej pojedynczej bazy danych przy użyciu aktywnej replikacji geograficznej](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [Konfiguruj i pracy awaryjnej bazy danych w puli przy użyciu aktywnej replikacji geograficznej](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), i [ Konfigurowanie i trybu failover przejścia w tryb failover grupy dla pojedynczej bazy danych](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

### <a name="manage-sql-database-failover-using-the-rest-api"></a>Zarządzaj trybem failover bazy danych SQL przy użyciu interfejsu API REST

| Interfejs API | Opis |
| --- | --- |
| [Tworzenie lub aktualizacja bazy danych (createMode = przywracanie)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Tworzy, aktualizuje lub przywrócenie podstawowej lub pomocniczej bazy danych. |
| [Pobierz Utwórz lub zaktualizuj stan bazy danych](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Zwraca stan podczas operacji tworzenia. |
| [Ustaw pomocniczej bazy danych jako podstawowy (planowanego trybu Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Zestawy, które z repliką baz danych jest kluczem podstawowym, przechodzenie w tryb failover z bieżącej bazy danych repliki podstawowej. |
| [Ustaw pomocniczej bazy danych jako podstawowy (nieplanowany tryb Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Zestawy, które z repliką baz danych jest kluczem podstawowym, przechodzenie w tryb failover z bieżącej bazy danych repliki podstawowej. Ta operacja może spowodować utratę danych. |
| [Uzyskaj Link replikacji](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Pobiera łącze replikacji dla danej bazy danych SQL w partnerstwie replikacji geograficznej. Pobiera informacje widoczne w widoku wykazu sys.geo_replication_links. |
| [Łącza replikacji — lista przez bazę danych](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Pobiera wszystkie łącza replikacji dla danej bazy danych SQL w partnerstwie replikacji geograficznej. Pobiera informacje widoczne w widoku wykazu sys.geo_replication_links. |
| [Usuń Link replikacji](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Usuwa link replikacji bazy danych. Nie można wykonać podczas pracy awaryjnej. |
| [Utwórz lub zaktualizuj grupę trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Tworzy lub aktualizuje grupę trybu failover |
| [Usuwanie grupy trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Usuwa grupę trybu failover z serwera |
| [Praca awaryjna (planowana)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Nie powiedzie się za pośrednictwem z bieżącego serwera podstawowego do tego serwera. |
| [Zezwalaj na utratę danych wymuszenie trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails za pośrednictwem z bieżącego serwera podstawowego do tego serwera. Ta operacja może spowodować utratę danych. |
| [Pobierz grupę trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Pobiera grupy trybu failover. |
| [Wyświetlanie listy grup trybu Failover przez serwer](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Wyświetla listę grup trybu failover na serwerze. |
| [Aktualizacja grupy trybu Failover](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Aktualizuje grupę trybu failover. |
|  | |

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać przykładowe skrypty Zobacz:
  - [Konfigurowanie pojedynczej bazy danych i wprowadzanie jej w tryb failover przy użyciu funkcji aktywnej replikacji geograficznej](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Konfigurowanie bazy danych w puli i wprowadzanie jej w tryb failover przy użyciu funkcji aktywnej replikacji geograficznej](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Konfigurowanie grupy trybu failover i wprowadzanie jej w tryb failover dla jednej bazy danych](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- Omówienie ciągłości działania i scenariuszach można znaleźć [omówienie ciągłości działania](sql-database-business-continuity.md)
- Aby dowiedzieć się więcej na temat usługi Azure SQL Database automatyczne kopie zapasowe, zobacz [bazy danych SQL, automatyczne kopie zapasowe](sql-database-automated-backups.md).
- Aby dowiedzieć się więcej o korzystaniu z automatycznych kopii zapasowych do odzyskania, zobacz [przywrócić bazę danych z kopii zapasowych zainicjowanych przez usługę](sql-database-recovery-using-backups.md).
- Aby dowiedzieć się o wymagania dotyczące uwierzytelniania dla nowym serwerem podstawowym i bazy danych, zobacz [zabezpieczeń bazy danych SQL Database po awarii](sql-database-geo-replication-security-config.md).
