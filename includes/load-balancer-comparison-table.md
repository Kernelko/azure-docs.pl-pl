---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 10/29/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 5517cd236c503edce88ab45edbeff5604ef9322b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50254632"
---
| | Standardowy SKU | Podstawowy SKU |
| --- | --- | --- |
| Rozmiar puli zaplecza | Obsługuje maksymalnie 1000 wystąpień | Obsługuje maksymalnie 100 wystąpień |
| Punkty końcowe puli zaplecza | Każda maszyna wirtualna w jednej sieci wirtualnej, program blend maszyn wirtualnych i zestawów dostępności w tym zestawy skalowania maszyn wirtualnych. | Ustaw maszyn wirtualnych w skali dostępności pojedynczy zestaw lub maszyny wirtualnej. |
| [Sondy kondycji](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP I HTTP, HTTPS | TCP I HTTP |
| [Sonda kondycji zachowanie w dół](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | Połączenia TCP pozostaną aktywne na wystąpienie sondy w dół __i__ na wszystkich sondy w dół. | TCP połączenia pozostaną aktywne na wystąpienie sondy w dół. Wszystkie zakończenia połączenia TCP na wszystkich sondy nie działają. |
| Strefy dostępności | W standardowej jednostki SKU strefowo nadmiarowy i strefowych frontonów dla ruchu przychodzącego i wychodzącego, mapowania przepływy wychodzące nie są unieważniane strefy uszkodzenia, równoważenie obciążenia między strefami. | Niedostępne |
| Diagnostyka | Usługa Azure Monitor metryk wielowymiarowych łącznie bajtów i pakietów liczników, kondycja sondowania stanu, próby nawiązania połączenia (TCP SYN), kondycja połączenia wychodzące (SNAT udane i nieudane przepływami), pomiarów płaszczyzny danych active | Usługa Azure Log Analytics dla publicznej usługi Load Balancer tylko alert wyczerpania SNAT, liczba kondycji puli zaplecza. |
| Zaświadczanie o kondycji portów | Wewnętrzny moduł równoważenia obciążenia | Niedostępne |
| Zabezpieczanie domyślnie | Użyj domyślnej dla ruchu przychodzącego zamknięte dla publicznych punktów końcowych adresów IP i równoważenia obciążenia i sieciowej grupy zabezpieczeń do jawnie listy dozwolonych na przepływ ruchu. | Otwórz domyślna sieciowa grupa zabezpieczeń jest opcjonalne. |
| [Połączenia wychodzące](../articles/load-balancer/load-balancer-outbound-connections.md) | Można jawnie zdefiniować oparte na pulę NAT dla ruchu wychodzącego z [reguł dla ruchu wychodzącego](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Można użyć wielu frontonów przy użyciu na obciążenia równoważenia reguły zrezygnować. Scenariusz wychodzącego _musi_ jawnie tworzyć dla maszyny wirtualnej do użycia łączności wychodzącej.  Punkty końcowe usługi sieci wirtualnej bez łączność wychodząca jest osiągalna i nie są uwzględniane w kierunku przetworzonych danych.  Wszystkie publiczne adresy IP, łącznie z usługami PaaS platformy Azure nie jest dostępna jako punkty końcowe usługi sieci wirtualnej, musi być dostępny za pośrednictwem połączenia wychodzącego i liczba kierunku przetworzonych danych. Gdy tylko wewnętrznego modułu równoważenia obciążenia działa jako maszyna wirtualna, połączenia wychodzące przy użyciu domyślnego SNAT nie są dostępne; Użyj [reguł dla ruchu wychodzącego](../articles/load-balancer/load-balancer-outbound-rules-overview.md) zamiast tego. Wychodzące programowania SNAT jest protokół transportowy określonych oparta na protokole reguły równoważenia obciążenia dla ruchu przychodzącego. | Fronton pojedynczego wybranych losowo, jeśli podano wiele frontonów.  Gdy tylko wewnętrznego modułu równoważenia obciążenia działa jako maszyna wirtualna, używana jest domyślna SNAT. |
| [Reguły ruchu wychodzącego](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Deklaratywne wychodzącego konfiguracji translatora adresów Sieciowych, w tym których publiczny adres IP lub publiczny prefiks adresu IP, można skonfigurować limit czasu bezczynności ruchu wychodzącego, niestandardowe alokacją portów SNAT | Niedostępne |
|  [Resetowanie TCP w stanie bezczynności](../articles/load-balancer/load-balancer-tcp-reset.md) | Włączanie protokołu TCP (TCP RST) zresetowanie limitu czasu bezczynności na żadną regułę | Niedostępne |
| [Wiele frontonów](../articles/load-balancer/load-balancer-multivip-overview.md) | Dla ruchu przychodzącego i [ruchu wychodzącego](../articles/load-balancer/load-balancer-outbound-connections.md) | Tylko transfer przychodzący |
| Operacje zarządzania | Większość operacji < 30 sekund | 60-ponad 90 sekund typowy. |
| Umowa SLA | dostępność przez 99,99% dla ścieżki danych za pomocą dwóch dobrej kondycji maszyn wirtualnych. | [Niejawne w umowie SLA maszyn wirtualnych](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). | 
| Cennik | Naliczane na podstawie liczby reguł, przetworzone dane przychodzące lub wychodzące skojarzonych zasobów.  | Brak opłat |
|  |  |  |
