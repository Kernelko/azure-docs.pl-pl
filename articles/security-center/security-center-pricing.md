---
title: Przeprowadź uaktualnienie do warstwy standardowa Security Center w celu uzyskania zwiększonych zabezpieczeń | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera informacje o cenach usługi Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2018
ms.author: rkarlin
ms.openlocfilehash: a6bfc5cc577a60b4f1a89b05089af201e024f2dd
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297676"
---
# <a name="upgrade-to-security-centers-standard-tier-for-enhanced-security"></a>Uaktualnienie do warstwy standardowa Security Center w celu uzyskania zwiększonych zabezpieczeń
Usługa Azure Security Center zapewnia ujednolicone zarządzanie zabezpieczeniami i zaawansowaną ochronę przed zagrożeniami na potrzeby obciążeń uruchamianych na platformie Azure, lokalnie i w innych chmurach. System ten zapewnia widoczność i kontrolę nad obciążeniach chmury hybrydowej, aktywne mechanizmów obronnych pozwalających ograniczyć narażenie na zagrożenia i wykrywanie inteligentne, które ułatwią Ci zmieniającego się szybko ewoluującymi cyberatakami.

## <a name="pricing-tiers"></a>Warstwy cenowe
Usługa Security Center jest oferowana w dwóch warstwach:

- **Bezpłatna** warstwy jest automatycznie włączona na wszystkich subskrypcji platformy Azure i zawiera zasady zabezpieczeń, ciągła ocena zabezpieczeń i zalecenia dotyczące zabezpieczeń informacje z możliwością działania, aby pomóc w ochronie Twoich zasobów platformy Azure.
- **Standardowa** warstwa rozszerza możliwości w warstwie bezpłatna obciążeń w prywatnych i innych chmur publicznych, zapewniając ujednolicone ochronę zarządzania i zagrożenia zabezpieczeń różnych obciążeń chmury hybrydowej. Warstwa standardowa obejmuje również zaawansowane zagrożenia możliwości wykrywania, która używa wbudowaną analizę behawioralną i uczenie maszynowe do identyfikowania ataków i zero day luki w zabezpieczeniach, kontroli dostępu i aplikacji, aby zmniejszyć narażenie na ataki sieciowe i złośliwym oprogramowaniem, a więcej. Korzystanie z warstwy Standardowa jest bezpłatne przez pierwszych 60 dni.

Aby uzyskać więcej informacji, zobacz Centrum zabezpieczeń [stronę z cennikiem](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="try-standard-free-for-60-days"></a>Wypróbuj bezpłatnie przez 60 dni Standard
Warstwa Standardowa jest oferowana za darmo przez pierwsze 60 dni. Na końcu 60 dni należy wybrać nadal korzystać z usługi będzie automatycznie Zaczniemy naliczać opłaty za użycie.

Cały subskrypcji platformy Azure można uaktualnić do warstwy standardowa, która jest dziedziczona przez wszystkie zasoby w ramach subskrypcji.

Aby wyświetlić warstwy standardowa:

1. Wybierz **zasady zabezpieczeń** na **usługi Security Center** menu głównego.
2. Wybierz subskrypcję, której chcesz przejść na warstwę standardowa.
3. Na **zasady zabezpieczeń** bloku wybierz **warstwa cenowa**.
4. Wybierz **standardowa** do uaktualnienia.
5. Kliknij pozycję **Zapisz**.

![Zdarzenie naruszenia zabezpieczeń][1]

> [!NOTE]
> Aby włączyć wszystkie funkcje usługi Security Center, należy najpierw zastosować standardowej warstwy cenowej na subskrypcję lub grupę zasobów zawierającą odpowiednie maszyn wirtualnych. Konfigurowanie ceny dla obszaru roboczego nie włączać tylko w przypadku dostępu do maszyny Wirtualnej w czasie, funkcje adaptacyjnego sterowania aplikacjami i rozwiązaniami do wykrywania sieci dla zasobów platformy Azure.
>
>

## <a name="why-upgrade-to-standard"></a>Dlaczego należy przejść na warstwę standardowa?
Usługa Security Center oferuje lepsze zabezpieczenia i ochrona przed zagrożeniami dla obciążeń chmury hybrydowej, w tym:

- **Zabezpieczenia hybrydowe** — Uzyskaj ujednolicony widok zabezpieczeń dla wszystkich lokalnych obciążeń i w chmurze. Stosowanie zasad zabezpieczeń i nieustannie oceniaj zabezpieczenia obciążeń chmury hybrydowej w celu zapewnienia zgodności ze standardami zabezpieczeń. Zbieranie, wyszukiwanie i analizowanie danych zabezpieczeń z różnych źródeł, w tym zapór i innych rozwiązań partnerskich.
- **Zaawansowane wykrywanie zagrożeń** — zaawansowane analizy i usługi Microsoft Intelligent Security Graph, aby uzyskać przewagę nad ewoluującymi cyberatakami.  Wykorzystaj wbudowaną analizę behawioralną i uczenie maszynowe do identyfikowania ataków i luk typu zero day. Monitoruj sieci, maszyny i usługi w chmurze pod kątem przychodzących ataków i działań po naruszeniu zabezpieczeń. Usprawnij badanie dzięki interaktywnym narzędziom i kontekstowej analizie zagrożeń.
- **Kontroli dostępu i aplikacji** — blokuj złośliwe oprogramowanie i inne niechciane aplikacje, stosując zalecenia dotyczące listy dozwolonych dostosowane do konkretnych obciążeń i obsługiwane przez usługi machine learning. Zmniejsz narażony na ataki sieciowe dzięki just-in-time kontrolowany dostęp do portów zarządzania na maszynach wirtualnych platformy Azure, znacząco ograniczyć narażenie na ataki siłowe i inne ataki sieciowe.


## <a name="next-steps"></a>Kolejne kroki
W tym artykule zostały wprowadzone z użyciem usługi Security Center. Aby dowiedzieć się więcej o zwiększonych zabezpieczeń i zaawansowaną ochronę przed zagrożeniami warstwy standardowa, zobacz:

- [Zaawansowane wykrywanie zagrożeń](security-center-threat-report.md)
- [Po prostu w kontroli dostępu na maszynie Wirtualnej czas](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-pricing/get-standard.png
