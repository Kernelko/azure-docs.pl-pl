---
title: SQL Server na maszynach wirtualnych Windows Azure — często zadawane pytania | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera odpowiedzi na często zadawane pytania na temat uruchamiania programu SQL Server na maszynach wirtualnych platformy Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 906cec35c30c277ac1e8bc641d12fe37c29413dd
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427182"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Często zadawane pytania dotyczące programu SQL Server uruchomionego na maszynach wirtualnych Windows Azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania na temat uruchamiania [programu SQL Server na maszynach wirtualnych Windows na platformie Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Ten artykuł koncentruje się na kwestie związane z programu SQL Server na maszynach wirtualnych Windows. Jeśli używasz programu SQL Server na maszynach wirtualnych z systemem Linux, zobacz [Linux — często zadawane pytania](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Obrazy

1. **Jakie obrazów w galerii maszyn wirtualnych programu SQL Server są dostępne?**

   Azure obsługuje obrazy maszyn wirtualnych dla wszystkich obsługiwanych wersji głównych programu SQL Server we wszystkich wersjach dla systemów Windows i Linux. Aby uzyskać więcej informacji, zobacz pełną listę [obrazów maszyn wirtualnych Windows](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) i [obrazów maszyn wirtualnych z systemem Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Obrazy galerii w istniejącej maszyny wirtualnej programu SQL Server są aktualizowane?**

   Co dwa miesiące obrazów programu SQL Server w galerii maszyn wirtualnych są aktualizowane przy użyciu najnowszych Windows i Linux aktualizacji. W przypadku obrazów Windows w tym wszelkie aktualizacje, które są oznaczone jako ważne w aktualizacji Windows, w tym ważne aktualizacje zabezpieczeń programu SQL Server i dodatków service pack. Dla obrazów systemu Linux w tym najnowsze aktualizacje systemu. Aktualizacje zbiorcze programu SQL Server są obsługiwane inaczej dla systemów Linux i Windows. Dla systemu Linux aktualizacje zbiorcze programu SQL Server znajdują się również w odświeżania. Ale w tej chwili Windows maszyny wirtualne nie zostaną zaktualizowane o aktualizacje zbiorcze programu SQL Server lub Windows Server.

1. **Można usuwane z galerii obrazów maszyn wirtualnych programu SQL Server?**

   Tak. Platforma Azure utrzymuje tylko jeden obraz na głównej wersji i wydania. Na przykład po udostępnieniu nowego dodatku service pack programu SQL Server, platforma Azure dodaje nowy obraz w galerii dla tego dodatku service pack. Obraz programu SQL Server dla poprzedniego dodatku service pack jest usuwane natychmiast w witrynie Azure portal. Jednak jest wciąż dostępna na potrzeby aprowizacji za pomocą programu PowerShell dla następnych trzech miesięcy. Po upływie trzech miesięcy na poprzedniej ilustracji pakiet usługi nie jest już dostępna. Te zasady usuwania czy mają zastosowanie również w wersji programu SQL Server staje się nieobsługiwany po osiągnięciu końca swojego cyklu życia.

1. **Czy mogę utworzyć obraz wirtualnego dysku twardego z maszyny Wirtualnej programu SQL Server?**

   Tak, ale jest kilka istotnych kwestii. Jeśli wdrożono ten wirtualny dysk twardy do nowej maszyny Wirtualnej na platformie Azure, możesz zrobić ge nie w sekcji Konfiguracja programu SQL Server w portalu. Następnie można zarządzać opcje konfiguracji programu SQL Server za pomocą programu PowerShell. Ponadto możesz opłata zgodnie ze stawką maszyn wirtualnych SQL, obraz został pierwotnie oparty na. Ta zasada obowiązuje, nawet w przypadku usunięcia programu SQL Server z dysku VHD, przed wdrożeniem. 

1. **Czy jest możliwe do skonfigurowania konfiguracje nie są wyświetlane w galerii maszyn wirtualnych (na przykład Windows 2008 R2 i SQL Server 2012)?**

   Nie. Dla obrazów galerii maszyn wirtualnych, które obejmują program SQL Server należy wybrać jeden z podanych obrazów.

## <a name="creation"></a>Tworzenie

1. **Jak utworzyć maszynę wirtualną platformy Azure z programem SQL Server?**

   Najprostszym rozwiązaniem jest do utworzenia maszyny wirtualnej, który zawiera program SQL Server. Samouczek dotyczący rejestracji na platformie Azure i tworzenie maszyny Wirtualnej SQL z portalu, zobacz [Aprowizowanie maszyny wirtualnej programu SQL Server w witrynie Azure portal](virtual-machines-windows-portal-sql-server-provision.md). Można wybrać obraz maszyny wirtualnej, który używa licencjonowania programu SQL Server płatności na sekundę lub użyć obrazu, który pozwala na model dostarczania własnej licencji programu SQL Server. Istnieje również opcja ręcznego instalowania programu SQL Server na maszynie Wirtualnej z oboma swobodnie licencjonowaną (Developer i Express) lub dzięki ponownemu wykorzystaniu licencji w środowisku lokalnym. W przypadku przeniesienia własnej licencji, konieczne jest posiadanie [przenośności licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility/). Aby uzyskać więcej informacji, zobacz [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Wskazówki dotyczące cen maszyn wirtualnych platformy Azure z programem SQL Server).

1. **Jak migrować mojej lokalnej bazy danych programu SQL Server do chmury**

   Najpierw należy utworzyć maszynę wirtualną platformy Azure przy użyciu wystąpienia programu SQL Server. Następnie należy przeprowadzić migrację lokalnych baz danych do tego wystąpienia. Aby Strategie migracji danych, zobacz [migracji bazy danych programu SQL Server do programu SQL Server na Maszynie wirtualnej platformy Azure](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licencjonowanie

1. **Jak zainstalować Moje licencjonowaną kopię programu SQL Server na Maszynie wirtualnej platformy Azure?**

   Istnieją dwa sposoby, aby to zrobić. Można obsługiwać jeden z [obrazów maszyn wirtualnych, które obsługuje licencji](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), który jest również znany jako bring-your-own-license (BYOL). Innym rozwiązaniem jest skopiowanie nośnika instalacyjnego programu SQL Server do maszyny Wirtualnej z systemem Windows Server, a następnie zainstaluj program SQL Server na maszynie Wirtualnej. Jednak jeśli ręcznie zainstalować program SQL Server, jest nie integrację portalu i rozszerzenie agenta IaaS programu SQL Server nie jest obsługiwany, dlatego funkcje takie jak automatyczne kopie zapasowe i automatyczne stosowanie poprawek nie będzie działać w tym scenariuszu. Z tego powodu zaleca się skorzystanie z galerii obrazów BYOL. Aby użyć BYOL i multimediów programu SQL Server na Maszynie wirtualnej platformy Azure, konieczne jest posiadanie [przenośności licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility/). Aby uzyskać więcej informacji, zobacz [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Wskazówki dotyczące cen maszyn wirtualnych platformy Azure z programem SQL Server).

1. **Czy mogę zmienić Maszynę wirtualną do używania licencję programu SQL Server, jeśli został utworzony z jednego z obrazów w galerii zgodnie z rzeczywistym użyciem**

   Nie. Nie można przełączyć się z płatności na sekundę licencji do korzystania z własnych licencji. Tworzenie nowej maszyny wirtualnej platformy Azure przy użyciu jednej z [obrazy BYOL](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), a następnie przeprowadzić migrację baz danych na nowy serwer przy użyciu standardu [technik migracji danych](virtual-machines-windows-migrate-sql.md).

1. **Czy muszę płacić za licencję programu SQL Server na Maszynie wirtualnej platformy Azure, jeśli jest on używany tylko w stanie wstrzymania/tryb failover?**

   Jeśli masz pakiet Software Assurance i używają opcji przenoszenia licencji zgodnie z opisem w [często zadawane pytania dotyczące licencjonowania maszyny wirtualnej](http://azure.microsoft.com/pricing/licensing-faq/) nie trzeba płacić za licencję na jeden udział jako replika pomocnicza pasywnym we wdrożeniu o wysokiej dostępności programu SQL Server. W przeciwnym razie trzeba płacić za jej licencji.


## <a name="administration"></a>Administracja

1. **W tej samej maszyny Wirtualnej można zainstalować drugie wystąpienie programu SQL Server? Czy można zmienić zainstalowane funkcje domyślne wystąpienie?**

   Tak. Nośnik instalacyjny programu SQL Server znajduje się w folderze na **C** dysku. Uruchom **Setup.exe** z lokalizacji, aby dodać nowe wystąpienia programu SQL Server lub do zmiany innych zainstalowane funkcje programu SQL Server na maszynie. Należy pamiętać, że niektóre funkcje, takie jak automatyczne kopie zapasowe, automatyczne stosowanie poprawek i integracji magazynu kluczy Azure, będą działać tylko względem wystąpienia domyślnego.

1. **Można odinstalować domyślnego wystąpienia programu SQL Server?**

   Tak, ale są pewne zagadnienia. Jak wspomniano w poprzedniej odpowiedzi na pytanie, funkcje, które polegają na [rozszerzenie agenta IaaS programu SQL Server](virtual-machines-windows-sql-server-agent-extension.md) działać tylko w domyślnym wystąpieniu. Po odinstalowaniu wystąpienie domyślne rozszerzenie jej szukać w dalszym ciągu i może generować błędy dziennika zdarzeń. Te błędy są z dwóch następujących źródeł: **poświadczeń programu Microsoft SQL Server Management** i **agenta IaaS programu Microsoft SQL Server**. Błędy mogą być podobne do następujących:

      Wystąpił błąd związany z siecią lub wystąpieniem podczas nawiązywania połączenia z programem SQL Server. Serwer nie został znaleziony lub jest on niedostępny.

   Jeśli zdecydujesz się odinstalować domyślnego wystąpienia, również odinstalować [rozszerzenie agenta IaaS programu SQL Server](virtual-machines-windows-sql-server-agent-extension.md) także.

1. **Czy można usunąć programu SQL Server całkowicie z maszyny Wirtualnej SQL?**

   Tak, ale będą nadal naliczane za maszynę Wirtualną SQL, zgodnie z opisem w [ceny wskazówki dotyczące maszyn wirtualnych programu SQL Server Azure](virtual-machines-windows-sql-server-pricing-guidance.md). Jeśli program SQL Server nie są już potrzebne, można wdrożyć nową maszynę wirtualną i migrować dane i aplikacje do nowej maszyny wirtualnej. Następnie można usunąć maszyny wirtualnej programu SQL Server.
   
## <a name="updating-and-patching"></a>Stosowanie poprawek i aktualizacji

1. **Jak uaktualnić do nowej wersji/wydania programu SQL Server w Maszynie wirtualnej platformy Azure?**

   Obecnie nie ma żadnych uaktualnienia w miejscu programu SQL Server uruchomionego na Maszynie wirtualnej platformy Azure. Utwórz nową maszynę wirtualną platformy Azure przy użyciu wersji/wydania żądanego programu SQL Server, a następnie Migruj bazy danych na nowy serwer przy użyciu standardu [technik migracji danych](virtual-machines-windows-migrate-sql.md).

1. **Sposób aktualizacji i dodatków service pack stosowania na maszynę Wirtualną programu SQL Server?**

   Maszyny wirtualne zapewniają kontrolę nad maszyną hosta, w tym, kiedy i jak należy zastosować aktualizacje. Dla systemu operacyjnego możesz ręcznie zastosować aktualizacje systemu windows, można również włączyć usługę harmonogramu o nazwie [automatyczne stosowanie poprawek](virtual-machines-windows-sql-automated-patching.md). Usługa Automatyczne stosowanie poprawek instaluje wszelkie aktualizacje, które są oznaczone jako ważne, w tym aktualizacje programu SQL Server w tej kategorii. Inne, opcjonalne aktualizacje dla programu SQL Server należy zainstalować ręcznie.

## <a name="general"></a>Ogólne

1. **Czy wystąpienia klastra trybu Failover (FCI) programu SQL Server są obsługiwane na maszynach wirtualnych platformy Azure?**

   Tak. Możesz [utworzyć klaster pracy awaryjnej Windows w systemie Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) i korzystania z bezpośrednimi miejscami do magazynowania (S2D) jako magazynu klastra. Alternatywnie, można użyć klastrowania lub magazynu rozwiązania innych firm zgodnie z opisem w [wysokiej dostępności i odzyskiwania po awarii dla programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > W tej chwili [rozszerzenie agenta IaaS programu SQL Server](virtual-machines-windows-sql-server-agent-extension.md) nie jest obsługiwana dla infrastruktury klasyfikacji plików programu SQL Server na platformie Azure. Firma Microsoft zaleca, odinstaluj rozszerzenie z maszyny wirtualne, które uczestniczą w trybu. To rozszerzenie obsługuje funkcje, takie jak automatyczne kopie zapasowe i stosowanie poprawek i niektórych funkcji portalu do języka SQL. Te funkcje nie będą działać dla maszyn wirtualnych SQL, po odinstalowaniu agenta.

1. **Jaka jest różnica między maszynami wirtualnymi SQL i usługi SQL Database?**

   Koncepcyjnie programem SQL Server na maszynie wirtualnej platformy Azure jest to, że nie różni się od uruchamiania programu SQL Server w zdalnym centrum danych. Z kolei [bazy danych SQL](../../../sql-database/sql-database-technical-overview.md) oferuje bazy danych jako usługę. Usługa SQL Database nie masz dostępu do maszyn, na których są hostowane bazy danych. Dla pełnego porównania, zobacz [Wybieranie opcji programu SQL Server w chmurze: Azure SQL Database (PaaS) lub SQL Server na maszynach wirtualnych Azure (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Jak zainstalować narzędzia danych serwera SQL na maszynie Wirtualnej platformy Azure?**

    Pobieranie i instalowanie narzędzi danych programu SQL z [programu Microsoft SQL Server Data Tools — Business Intelligence programu Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Zasoby

**Maszyny wirtualne Windows**:

* [Omówienie programu SQL Server na maszynie Wirtualnej Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Aprowizowanie Windows serwer SQL maszyny Wirtualnej](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrowanie bazy danych do programu SQL Server na maszynie Wirtualnej platformy Azure](virtual-machines-windows-migrate-sql.md)
* [Wysoka dostępność i odzyskiwanie po awarii programu SQL Server na maszynach wirtualnych platformy Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Najlepsze rozwiązania w zakresie wydajności dla programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Wzorce aplikacji i strategie programowania dla programu SQL Server na maszynach wirtualnych platformy Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Maszyny wirtualne systemu Linux**:

* [Omówienie programu SQL Server na maszynie Wirtualnej systemu Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Aprowizowanie maszyny Wirtualnej systemu Linux z programu SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Często zadawane pytania (system Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Dokumentacja programu SQL Server w systemie Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
