---
title: Usługa SQL Database (PaaS) a program SQL Server w chmurze na maszynach wirtualnych (IaaS) | Microsoft Docs
description: 'Dowiedz się, która opcja programu SQL Server w chmurze jest odpowiednia do używanej aplikacji: usługa Azure SQL Database (PaaS) czy program SQL Server w chmurze na maszynach wirtualnych platformy Azure.'
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: chmura usługi SQL Server, SQL Server w chmurze, baza danych PaaS, chmura SQL Server, DBaaS
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: ff6b535b67608d9331e134ff3b3d943601e73a48
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364493"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Wybieranie opcji programu SQL Server w chmurze: usługa Azure SQL Database (PaaS) lub program SQL Server na maszynach wirtualnych Azure (IaaS)

Na platformie Azure może mieć obciążeń programu SQL Server, działające w infrastrukturze hostowanej (IaaS) lub są uruchamiane jako usługa hostowana ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

- [Usługa Azure SQL Database](https://azure.microsoft.com/services/sql-database/): aparatu bazy danych SQL, w oparciu o Enterprise programu SQL Server, który jest zoptymalizowany pod kątem tworzenia nowoczesnych aplikacji. Usługa Azure SQL Database oferuje kilka opcji wdrażania:

  - Możesz wdrożyć pojedynczą bazę danych do [serwer logiczny](sql-database-logical-servers.md).
  - Można wdrożyć w [puli elastycznej](sql-database-elastic-pool.md) na [serwer logiczny](sql-database-logical-servers.md) udostępnianie zasobów i obniżyć koszty.
  - Można wdrożyć na [wystąpieniach zarządzanych bazy danych SQL Azure](sql-database-managed-instance.md).

   Poniższa ilustracja przedstawia te opcje wdrażania:

     ![deployment-options](./media/sql-database-technical-overview/deployment-options.png)

     > [!NOTE]
     > Za pomocą wszystkich trzech wersjach bazy danych SQL Azure dodaje dodatkowe funkcje, które nie są dostępne w programie SQL Server, takie jak wbudowana funkcja analizy i zarządzania. Serwer logiczny, zawierający pojedynczy i puli baz danych oferuje większość funkcji o zakresie bazy danych programu SQL Server. W usłudze Azure SQL Database Managed Instance Azure SQL Database oferuje zasoby udostępnione dla baz danych i dodatkowych funkcji należących do zakresu wystąpienia. Wystąpienie usługi Azure SQL Database Managed obsługuje migrację bazy danych, korzystając z minimalnym bez zmian w bazie danych.

- [Program SQL Server na maszynach wirtualnych Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server zainstalowany i hostowany w chmurze w systemie Windows Server lub Linux maszyn wirtualnych (VM) na platformie Azure, znany również jako infrastruktura jako usługa (IaaS). Program SQL Server na maszynach wirtualnych platformy Azure to dobra opcja w przypadku migracji lokalnych baz danych programu SQL Server i aplikacje bez zmiany bazy danych. Wszystkie najnowsze wersje i wydania programu SQL Server są dostępne do zainstalowania na maszynie wirtualnej IaaS. Najbardziej znaczący różnica z bazy danych SQL polega na tym, że maszyny wirtualne SQL Server umożliwia pełną kontrolę nad aparatem bazy danych. Można wybrać podczas konserwacji/poprawianie rozpocznie się, aby zmienić model odzyskiwania prostego lub niepełnym dziennikiem umożliwia szybsze ładowanie mniej dziennika, aby wstrzymać lub uruchomić aparatu w razie potrzeby, i można w pełni dostosować aparatu bazy danych programu SQL Server. Dołączono dodano odpowiedzialność za zarządzanie maszynami wirtualnymi za pomocą tej kontrolki dodatkowe.

Dowiedz się, jak każdej opcji wdrażania dopasowuje się do platformy danych firmy Microsoft i Uzyskaj pomoc przy dopasowaniu odpowiedniej opcji do wymagań biznesowych. Niezależnie od tego, czy priorytetem jest oszczędność, czy ograniczenie administracji do minimum, artykuł ten może pomóc zdecydować, które rozwiązanie pozwoli spełnić wymagania związane z działalnością biznesową, na których zależy Ci najbardziej.

## <a name="microsofts-sql-data-platform"></a>Platforma danych SQL firmy Microsoft

Podczas każdej dyskusji dotyczącej baz danych Azure w zestawieniu z lokalnymi bazami danych programu SQL Server należy przede wszystkim pamiętać o tym, że można używać ich wszystkich. Platforma danych firmy Microsoft korzysta z technologii programu SQL Server i udostępnia ją na fizycznych komputerach lokalnych, w prywatnych środowiskach chmury, prywatnych środowiskach chmury udostępnianych przez inne firmy i w chmurze publicznej. Program SQL Server w usłudze Azure Virtual Machines umożliwia spełnienie unikatowych i zróżnicowanych potrzeb związanych z prowadzeniem działalności biznesowej poprzez kombinację wdrożeń lokalnych i hostowanych w chmurze, z równoczesnym użyciem w tych środowiskach tego samego zestawu produktów serwerowych, narzędzi projektowych i zakresu wiedzy.

   ![Opcje programu SQL Server w chmurze: program SQL Server w usłudze IaaS lub baza danych SQL w usłudze SaaS w chmurze.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak widać to na diagramie, każde rozwiązanie można scharakteryzować według posiadanego poziomu administracji względem infrastruktury (na osi X) oraz według stopnia opłacalności wynikającej z konsolidacji poziomu bazy danych i automatyzacji (na osi Y).

Podczas projektowania aplikacji są dostępne cztery podstawowe opcje obsługi części aplikacji programu SQL Server:

- program SQL Server na niezwirtualizowanych komputerach fizycznych,
- program SQL Server na lokalnych komputerach zwirtualizowanych (w chmurze prywatnej),
- program SQL Server na maszynie wirtualnej platformy Azure (w chmurze publicznej firmy Microsoft),
- usługa Azure SQL Database (w chmurze publicznej firmy Microsoft).

Poniższe sekcje zawierają informacje na temat programu SQL Server w chmurze publicznej firmy Microsoft: usługi Azure SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure. Ponadto przeanalizujemy typowe czynniki motywujące związane z działalnością biznesową, aby ustalić, która opcja jest najlepsza dla aplikacji.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Bliższe spojrzenie na usługę Azure SQL Database i program SQL Server na maszynach wirtualnych platformy Azure

- **Azure SQL Database**

Relacyjne bazy danych jako — usługa (DBaaS) hostowanych w chmurze platformy Azure, ją zaliczyć do kategorii branżowych *Platform-as-a-Service (PaaS)*. [Baza danych SQL](sql-database-technical-overview.md) działa na ustandaryzowanym sprzęcie i oprogramowaniu, które stanowi własność firmy Microsoft, jest przez nią hostowane i konserwowane. Usługa SQL Database można użyć wbudowanych funkcji i funkcjonalności, które wymaga rozbudowane konfiguracji w programie SQL Server. Płatność za korzystanie z usługi SQL Database odbywa się zgodnie z rzeczywistym użyciem, a opcje pozwalają na skalowanie w górę lub w poziomie, dzięki czemu klienci mają do dyspozycji większe możliwości bez najmniejszych zakłóceń. Usługa Azure SQL Database jest idealne środowisko do tworzenia nowych aplikacji w chmurze. Ponadto za pomocą [wystąpienia zarządzanego Azure SQL Database](sql-database-managed-instance.md), można dostarczyć własną licencję. Ponadto ta opcja udostępnia wszystkie korzyści PaaS usługi Azure SQL Database, ale dodaje możliwości, które były wcześniej dostępne tylko na maszynach wirtualnych programu SQL. Obejmuje natywne sieci wirtualnej (VNet) i prawie 100% zgodności z lokalnym serwerem SQL. [Wystąpienie zarządzane](sql-database-managed-instance.md) doskonale nadaje się do lokalnej bazy danych migracji na platformę Azure przy minimalnych zmianach wymagane.

- **Program SQL Server na maszynach wirtualnych platformy Azure (maszyny wirtualne)**

Należy do kategorii branżowych *Infrastructure-as-a-Service (IaaS)* i pozwala na uruchamianie programu SQL Server na maszynie wirtualnej w chmurze. [Maszyny wirtualne programu SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) również uruchomić na ustandaryzowanym sprzęcie, będące własnością, hostowane i konserwowane przez firmę Microsoft. Korzystając z programu SQL Server na maszynie Wirtualnej, można płacić za — tak jak w rzeczywistym licencji programu SQL Server dołączoną do obrazu programu SQL Server lub łatwe użycie istniejącej licencji. Możesz również zatrzymać lub wznowienia maszyny Wirtualnej, zgodnie z potrzebami.

Ogólnie rzecz biorąc, te dwie opcje są zoptymalizowane do różnych celów:

- **Azure SQL Database**

Zoptymalizowana pod kątem ograniczenia ogólne koszty zarządzania do minimum dla inicjowania obsługi administracyjnej i zarządzanie wieloma bazami danych. Obniża bieżące koszty administracyjne, ponieważ nie jest konieczne zarządzanie maszynami wirtualnymi, systemem operacyjnym ani oprogramowaniem bazy danych. Nie wymaga zarządzania uaktualnieniami, wysoką dostępnością ani [kopiami zapasowymi](sql-database-automated-backups.md). Ogólnie rzecz biorąc, usługa Azure SQL Database może znacząco zwiększyć liczbę baz danych zarządzanych przez pojedynczy zasób informatyczny lub projektowy. [Pule elastyczne](sql-database-elastic-pool.md) za pomocą funkcji izolacji dzierżawcy i możliwość skalowania, aby ograniczyć koszty dzięki udostępnieniu zasobów w bazach danych obsługują także architektury wielodostępnej aplikacji SaaS. [Wystąpienie usługi Azure SQL Database Managed](sql-database-managed-instance.md) zapewnia obsługę funkcji należących do zakresu wystąpień, umożliwiając łatwą migrację istniejących aplikacji, jak również udostępnianie zasobów między bazami danych.

- **SQL Server uruchomiony na maszynach wirtualnych platformy Azure**

Zoptymalizowane pod kątem migrowania istniejących aplikacji na platformie Azure lub rozszerzania istniejących aplikacji lokalnych do chmury we wdrożeniach hybrydowych. Ponadto przy użyciu programu SQL Server na maszynie wirtualnej można tworzyć i testować tradycyjne aplikacje programu SQL Server. W przypadku programu SQL Server na maszynach wirtualnych platformy Azure masz pełne uprawnienia administracyjne do dedykowanego wystąpienia programu SQL Server oraz maszyny wirtualnej w chmurze. To doskonałe rozwiązanie, gdy firma ma już dostępne zasoby informatyczne do obsługi maszyn wirtualnych. Te funkcje umożliwiają stworzenie doskonale dopasowanego systemu, który spełni określone wymagania aplikacji związane z wydajnością i dostępnością.

Poniższa tabela zawiera podsumowanie głównych cech usługi SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure:

| | Azure SQL Database<br>Serwery logiczne, pule elastyczne i pojedynczych baz danych | Azure SQL Database<br>Wystąpienie zarządzane |Maszyna wirtualna systemu Azure<br>Oprogramowanie SQL Server |
| --- | --- | --- |---|
| **Najlepsze dla:** |Nowe aplikacje zaprojektowane dla chmury, które chcesz korzystać z najnowszych funkcji programu SQL Server stabilne i mają ograniczenia czasowe w zakresie projektowania i marketingu. | Nowe aplikacje lub istniejących aplikacji lokalnych, które ma być używana najnowsza stabilna funkcje programu SQL Server i które są migrowane do chmury przy minimalnych zmianach.  | Istniejące aplikacje, które wymagają szybkiej migracji do chmury przy minimalnych zmianach lub jedynie minimalnych zmianach. Scenariusze szybkiego tworzenia i testowania, gdy nie chcesz kupować lokalnego nieprodukcyjnego sprzętu dla programu SQL Server. |
|  | Zespoły, które wymagają wbudowanej wysokiej dostępności, odzyskiwania po awarii i uaktualniania bazy danych. | Taka sama jak baza danych SQL. | Zespoły, które można skonfigurować prawidłowo dostrajania, dostosowywanie i zarządzanie wysokiej dostępności, odzyskiwania po awarii i stosowanie poprawek dla programu SQL Server. Niektóre dostępne funkcje automatyczne znacznie to upraszczają. | |
|  | Zespoły, które nie chcą zarządzać podstawowymi ustawieniami systemu operacyjnego i konfiguracji. | Taka sama jak baza danych SQL. | Potrzebne jest dostosowane środowisko z pełnymi prawami administracyjnymi. | |
|  | Bazy danych o rozmiarze do 100 TB. | Taka sama jak baza danych SQL. | Wystąpienia programu SQL Server z maksymalnie 64 TB przestrzeni dyskowej. Wystąpienie może obsługiwać dowolną liczbę baz danych zależnie od potrzeb. |
| **Zgodność** | Obsługuje większość funkcji poziomu bazy danych w środowisku lokalnym. | Obsługuje prawie wszystkie lokalne funkcje na poziomie wystąpienia i poziomu bazy danych. | Obsługuje wszystkie funkcje w środowisku lokalnym. |
| **Zasoby:** | Czy chcesz używać zasobów IT do konfiguracji i zarządzania podstawową infrastrukturą, ale chcesz skupić się na warstwie aplikacji. | Taka sama jak baza danych SQL. | Masz niektóre zasoby informatyczne do konfiguracji i zarządzania. Niektóre dostępne funkcje automatyczne znacznie to upraszczają. |
| **Całkowity koszt posiadania:** | Eliminuje koszty sprzętu i ogranicza koszty administracyjne. | Taka sama jak baza danych SQL. | Eliminuje koszty sprzętu. |
| **Ciągłość działalności biznesowej:** |Oprócz [wbudowaną odporność na uszkodzenia infrastruktury możliwości](sql-database-high-availability.md), Azure SQL Database oferuje funkcje, takie jak [automatyczne kopie zapasowe](sql-database-automated-backups.md), [punktu w czasie przywracania](sql-database-recovery-using-backups.md#point-in-time-restore), [geoprzywracanie](sql-database-recovery-using-backups.md#geo-restore), i [trybu failover grupy i aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md) Aby zwiększyć ciągłość prowadzenia działalności biznesowej. Więcej informacji znajduje się w temacie [Omówienie ciągłości działalności biznesowej usługi SQL Database](sql-database-business-continuity.md). | Dostępne są takie same jak bazy danych SQL, a także inicjowane przez użytkownika, tylko do kopiowania kopii zapasowych. | Program SQL Server na maszynach wirtualnych platformy Azure umożliwia skonfigurowanie rozwiązania wysokiej dostępności i odzyskiwania po awarii w zależności od określonych potrzeb bazy danych. Można mieć zatem system w znacznym stopniu zoptymalizowany dla aplikacji. Jeśli wystąpi taka potrzeba, tryb failover można przetestować i uruchomić samodzielnie. Więcej informacji znajduje się w temacie [Wysoka dostępność i odzyskiwanie po awarii dla programu SQL Server w usłudze Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Chmura hybrydowa:** |Aplikacja lokalna może uzyskiwać dostęp do danych w usłudze Azure SQL Database. | [Implementacja natywnych sieci wirtualnej](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration) i łączności do środowiska lokalnego przy użyciu usługi Azure Expressroute lub bram sieci VPN. | Program SQL Server na maszynach wirtualnych platformy Azure może obejmować aplikacje, które działają częściowo w chmurze i częściowo lokalnie. Można na przykład rozszerzyć sieć lokalną i domenę usługi Active Directory do chmury za pośrednictwem usługi [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Ponadto można przechowywać lokalne pliki danych w usłudze Azure Storage przy użyciu [plików danych programu SQL Server na platformie Azure](http://msdn.microsoft.com/library/dn385720.aspx). Więcej informacji znajduje się w temacie [Wprowadzenie do chmury hybrydowej programu SQL Server 2014](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Obsługuje [replikację transakcyjną programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako subskrybent replikowanych danych. | Replikacja nie jest obsługiwana dla wystąpienia zarządzanego Azure SQL Database. | W pełni obsługuje [replikację transakcyjną programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [zawsze włączonych grup dostępności](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), usługi integracji i wysyłanie dzienników do replikacji danych. Ponadto w pełni obsługiwane są tradycyjne kopie zapasowe programu SQL Server. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Względy biznesowe przemawiające za wyborem usługi Azure SQL Database lub programu SQL Server na maszynach wirtualnych Azure

### <a name="cost"></a>Koszty

Ograniczone fundusze są często podstawowym czynnikiem decydującym o wyborze sposobu hostowania baz danych — niezależnie od tego, czy Twoja firma to start-up o ograniczonych środkach finansowych, czy należysz do zespołu firmowego, który musi zmieścić się w zaplanowanym budżecie. W tej sekcji poznasz informacje dotyczące rozliczeń i licencjonowania platformy Azure w odniesieniu do dwóch opcji obsługi relacyjnych baz danych: usługi SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure. Poznasz również sposób obliczania całkowitego kosztu aplikacji.

#### <a name="billing-and-licensing-basics"></a>Podstawowe informacje dotyczące rozliczeń i licencjonowania

Obecnie **bazy danych SQL** jest oferowana jako usługa i jest dostępna w kilku warstwach usług z cenami różnych zasobów, z których wszystkie są naliczane godzinowo według stałej stawki ustalanej na podstawie warstwy usługi i wybranego rozmiaru obliczeń. Wystąpienie zarządzane SQL Database możesz także dostarczyć własną licencję. Więcej informacji na temat przenoszenia własnej licencji można znaleźć w temacie [Przenośność licencji za pośrednictwem programu Software Assurance w systemie Azure](https://azure.microsoft.com/pricing/license-mobility/). Rozliczany jest również internetowy ruch wychodzący po zwykłych [stawkach transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/). Możesz dynamicznie dostosować warstwy usług i obliczenia rozmiarów odpowiednio do potrzeb przepływności zależeć od Twojej aplikacji. Aby uzyskać najnowsze informacje dotyczące bieżących obsługiwanych warstw usług można wyświetlić [modelu zakupu opartego na jednostkach DTU](sql-database-service-tiers-dtu.md) i [modelu zakupu opartego na rdzeniach wirtualnych](sql-database-service-tiers-vcore.md). Można również utworzyć [pul elastycznych](sql-database-elastic-pool.md) współużytkowanie zasobów między wystąpieniami bazy danych do zmniejszenia kosztów i obsłużyć obciążenie nagłego zapotrzebowania.

W przypadku usługi **SQL Database** oprogramowanie bazy danych jest automatycznie konfigurowane, poprawiane i uaktualniane przez firmę Microsoft, co obniża koszty administracyjne. Ponadto [wbudowana funkcja tworzenia kopii zapasowych](sql-database-automated-backups.md) pozwala osiągnąć znaczne oszczędności kosztów, zwłaszcza w przypadku dużej liczby baz danych.

Dzięki **programowi SQL Server na maszynach wirtualnych platformy Azure** można używać udostępnianego na platformie obrazu programu SQL Server (który obejmuje licencję) lub przenieść swoją licencję programu SQL Server. Dostępne są wszystkie obsługiwane wersje programu SQL Server (2008R2, 2012, 2014, 2016) i edycje (Developer, Express, Web, Standard, Enterprise). Ponadto są dostępne wersje bring-your-own-license (BYOL) obrazów. W przypadku użycia obrazów dostarczanych przez platformę Azure koszty operacyjne zależą od rozmiaru maszyny wirtualnej, a także od wybranej wersji programu SQL Server. Niezależnie od rozmiaru maszyny Wirtualnej lub wersji programu SQL Server naliczana jest opłata za minutę licencjonowania programu SQL Server i Windows lub Linux Server wraz z kosztami usługi Azure Storage dla dysków maszyny Wirtualnej. Opcja rozliczania co minutę pozwala używać programu SQL Server tak długo, jak jest to konieczne bez wykupywania dodatkowych licencji programu SQL Server. W przypadku przeniesienia licencji programu SQL Server na platformie Azure, opłaty są naliczane dla serwera i tylko koszty magazynowania. Więcej informacji na temat przenoszenia własnej licencji można znaleźć w temacie [Przenośność licencji za pośrednictwem programu Software Assurance w systemie Azure](https://azure.microsoft.com/pricing/license-mobility/). Rozliczany jest również internetowy ruch wychodzący po zwykłych [stawkach transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/).

#### <a name="calculating-the-total-application-cost"></a>Obliczanie całkowitego kosztu aplikacji

Po uruchomieniu przy użyciu platformy w chmurze, koszt działania aplikacji obejmuje koszty w przypadku nowych wdrożeń i bieżące koszty administracyjne, a także koszty usługi chmury publicznej platformy.

**W przypadku korzystania z usługi Azure SQL Database:**

- Znacznie ograniczone koszty administracyjne
- Koszty rozwoju ograniczone dla migrowanych aplikacji
- Koszty usługi SQL Database
- Żadnych kosztów zakupu sprzętu

**W przypadku korzystania z programu SQL Server na maszynach wirtualnych platformy Azure:**

- Wyższe koszty administracyjne
- Ograniczone do żadnych kosztów programowania dla migrowanych aplikacji
- Koszty usługi maszyny wirtualnej
- Koszty licencji programu SQL Server
- Żadnych kosztów zakupu sprzętu

Więcej informacji na temat cen zawierają następujące zasoby:

- [Cennik usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
- [Maszyny wirtualne — cennik](https://azure.microsoft.com/pricing/details/virtual-machines/) ([SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) i [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows))
- [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Administracja

Dla wielu firm decyzja o przeniesieniu usług do usługi w chmurze wiąże się zarówno z ograniczeniem złożoności w zakresie administracji, jak i obniżeniem kosztów. Przy użyciu rozwiązań IaaS i PaaS Microsoft administruje podstawowej infrastruktury i automatycznie replikuje wszystkie dane w celu zapewniania odzyskiwania po awarii, konfiguruje i uaktualnia oprogramowanie bazy danych, zarządza równoważeniem obciążenia i obsługuje tryb failover, jeśli istnieje Błąd serwera w centrum danych.

- Za pomocą **usługi Azure SQL Database**, możesz administrować swoją bazą danych, ale nie trzeba zarządzać aparatem bazy danych, system operacyjny serwera lub sprzętu.  Przykładowe elementy, którymi można dalej zarządzać to bazy danych, identyfikatory logowania, dostrajanie indeksów i zapytań oraz inspekcja i zabezpieczenia. Ponadto skonfigurowanie wysokiej dostępności do innego centrum danych wymaga minimalnej konfiguracji i administrowania.
- **Program SQL Server na maszynach wirtualnych platformy Azure** daje pełną kontrolę nad konfiguracją systemu operacyjnego i wystąpienia programu SQL Server. W przypadku korzystania z maszyny wirtualnej decydujesz o tym, kiedy należy zaktualizować/uaktualnić system operacyjny i oprogramowanie bazy danych i kiedy zainstalować dodatkowe oprogramowanie, np. oprogramowanie antywirusowe. Dostępnych jest kilka funkcji automatycznych, które znacznie upraszczają instalowanie poprawek, tworzenie kopii zapasowej i zapewnianie wysokiej dostępności. Ponadto możesz kontrolować rozmiar maszyny wirtualnej, liczbę dysków oraz ich konfiguracje magazynu. Platforma Azure umożliwia zmianę rozmiaru maszyny wirtualnej zgodnie z zapotrzebowaniem. Więcej informacji można znaleźć w temacie [Virtual Machine and Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md) (Rozmiary maszyn wirtualnych i usług w chmurze na platformie Azure).

### <a name="service-level-agreement-sla"></a>Umowa dotycząca poziomu usług (SLA)

Dla wielu działów IT wypełnienie zobowiązań wynikających z umowy dotyczącej poziomu usług (SLA) ma najwyższy priorytet. W tej sekcji opisano, jakie warunki umowy SLA stosuje się do poszczególnych opcji obsługi bazy danych.

Aby uzyskać **bazy danych SQL**, firma Microsoft oferuje umowę SLA ZAPEWNIAJĄCĄ dostępność przez 99,99% czasu. Najnowsze informacje można znaleźć w artykule [SQL Database — umowa SLA](https://azure.microsoft.com/support/legal/sla/sql-database/).

W przypadku **programu SQL Server uruchomionego na maszynach wirtualnych platformy Azure** firma Microsoft zapewnia dostępność na poziomie 99,95%, która obejmuje tylko maszyny wirtualne. Umowa SLA nie obejmuje procesów (np. programu SQL Server) uruchomionych na maszynie wirtualnej i wymaga obsługi przynajmniej dwóch wystąpień maszyny wirtualnej w zbiorze dostępności. Najnowsze informacje znajdują się w artykule [Maszyny wirtualne — umowa SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Dla bazy danych o wysokiej dostępności (HA) w ramach maszyn wirtualnych, należy skonfigurować jedną z obsługiwanych opcji wysokiej dostępności w programie SQL Server, takich jak [zawsze włączonych grup dostępności](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Użycie obsługiwanej opcji wysokiej dostępności nie zapewnia dodatkowej umowy SLA, ale umożliwia osiągnięcie dostępności bazy danych na poziomie > 99,99%.

### <a name="market"></a>Czas na przeniesienie na platformę Azure

**Serwery logiczne usługi SQL Database, pul elastycznych i pojedynczych baz danych** to właściwe rozwiązanie dla aplikacji projektowanych w chmurze, gdy wydajność deweloperów i krótki czas — na rynek nowe rozwiązania mają kluczowe znaczenie. Dzięki funkcjonalności przypominającej model DBA jest doskonała dla architektów i deweloperów chmury, ponieważ zmniejsza potrzebę zarządzania bazowym systemem operacyjnym i bazą danych.

**Wystąpienie zarządzane usługi SQL Database** znacznie upraszcza migrację istniejących aplikacji do usługi Azure SQL Database, co umożliwia przenoszenie aplikacji zmigrowana baza danych, aby szybko wprowadzać na rynek na platformie Azure.

**Program SQL Server działający na maszynach wirtualnych Azure** jest doskonałym rozwiązaniem, jeśli istniejące lub nowe aplikacje wymagają dużych baz danych lub uzyskać dostęp do wszystkich funkcji programu SQL Server lub Windows/Linux i chcesz uniknąć czasu i pieniędzy uzyskiwanie nowego sprzętu lokalnego. Jest również odpowiedni, jeśli chcesz przeprowadzić migrację istniejących lokalnych aplikacji i baz danych na platformie Azure jako-to - w przypadkach, gdy wystąpienia zarządzanego Azure SQL Database jest dobrym rozwiązaniem. Ponieważ nie trzeba zmieniać prezentacji, aplikacji i warstwy danych, oszczędność czasu i budżetu na ponownego projektowania istniejącego rozwiązania. Zamiast tego możesz skoncentrować się na migracji wszystkich rozwiązań do platformy Azure i przeprowadzeniu optymalizacji wydajności, których może wymagać platforma Azure. Więcej informacje zawiera artykuł [Performance Best Practices for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md) (Najlepsze praktyki dotyczące wydajności dla programu SQL Server w usłudze Azure Virtual Machines).

## <a name="next-steps"></a>Kolejne kroki

- Aby rozpocząć pracę z usługą SQL Database, zobacz [Your first Azure SQL Database](sql-database-get-started-portal.md) (Twoja pierwsza baza danych Azure SQL Database).
- Zobacz [Cennik usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Aby rozpocząć pracę z programem SQL Server na maszynach wirtualnych platformy Azure, zobacz temat [Aprowizowanie maszyny wirtualnej programu SQL Server w Portalu Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
