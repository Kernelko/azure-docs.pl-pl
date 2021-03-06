---
title: Konfigurowanie BFD za pośrednictwem usługi ExpressRoute | Dokumentacja firmy Microsoft
description: Niniejszy dokument zawiera instrukcje dotyczące sposobu konfigurowania BFD za pośrednictwem prywatnej komunikacji równorzędnej w obwodu usługi ExpressRoute.
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: ''
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 8/17/2018
ms.author: rambala
ms.openlocfilehash: 97ac49ff5cc50621fd902f77c2543a428c7e8bae
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49384756"
---
# <a name="configure-bfd-over-expressroute"></a>Konfigurowanie BFD za pośrednictwem usługi ExpressRoute

Usługa ExpressRoute obsługuje dwukierunkowe przesyłanie dalej wykrywania (BFD) za pośrednictwem prywatnej komunikacji równorzędnej. Po włączeniu BFD za pośrednictwem usługi ExpressRoute może usprawnić wykrywanie awarii łącza między Microsoft Enterprise na urządzenia brzegowe (MSEE) i routery, na których zakończyć obwód usługi ExpressRoute (PE). Możesz zakończyć działanie usługi ExpressRoute za pośrednictwem routingu urządzenia brzegowe klienta lub routingu urządzenia brzegowe partnera (jeśli zakończył się za pomocą zarządzanej usługi połączenia warstwy 3). Ten dokument przeprowadzi Cię przez potrzebę BFD oraz jak włączyć BFD za pośrednictwem usługi ExpressRoute.

## <a name="need-for-bfd"></a>Potrzebę BFD

Na poniższym diagramie przedstawiono korzyści włączania BFD za pośrednictwem obwodu usługi ExpressRoute: [ ![1]][1]

Można włączyć obwód usługi ExpressRoute za połączenia w warstwie 2 lub zarządzane połączenia w warstwie 3. W obu przypadkach Jeśli co najmniej jedno urządzenie warstwy 2 znajduje się w ścieżce połączenia usługi ExpressRoute, odpowiedzialność wykrywania błędów łącze w ścieżce spoczywa inne protokołu BGP.

Na urządzeniach MSEE keepalive protokołu BGP i czas wstrzymania zwykle są skonfigurowane jako 60 i 180 sekund odpowiednio. W związku z tym po awarii łącza, jaki zajmuje maksymalnie trzy minuty do wykrywania dowolny link błędu i przełączać ruch do alternatywne połączenie.

Czasomierze protokołu BGP można kontrolować, konfigurując niższe keepalive protokołu BGP i czasu blokady na urządzeniu komunikacji równorzędnej krawędź klienta. Jeśli niezgodność czasomierzy protokołu BGP między dwoma urządzeniami komunikacji równorzędnej, sesji protokołu BGP między komputerami Użyj niższą wartość czasomierza. Keepalive protokołu BGP można ustawić najmniejsza trzy sekundy, a czas wstrzymania zgodnie z kolejnością dziesiątki sekund. Jednak ustawienie dla protokołu BGP czasomierzy agresywnie mniej preferowane ponieważ proces intensywnie korzystających z protokołu.

W tym scenariuszu BFD może pomóc. BFD zapewnia wykrywanie awarii łącza małym obciążeniem w odstępie czasu subsecond. 


## <a name="enabling-bfd"></a>Włączanie BFD

BFD jest domyślnie skonfigurowane, w ramach wszystkich nowo utworzonej usługi ExpressRoute prywatnej komunikacji równorzędnej interfejsów w rozwiązaniami Msee. W związku z tym aby włączyć BFD, należy po prostu skonfigurować BFD na Twoje PEs. Konfigurowanie BFD to dwuetapowy proces: należy skonfigurować BFD na interfejsie, a następnie połącz go sesji protokołu BGP.

Poniżej przedstawiono przykładową konfigurację PE (przy użyciu Cisco IOS XE). 

    interface TenGigabitEthernet2/0/0.150
      description private peering to Azure
      encapsulation dot1Q 15 second-dot1q 150
      ip vrf forwarding 15
      ip address 192.168.15.17 255.255.255.252
      bfd interval 300 min_rx 300 multiplier 3


    router bgp 65020
      address-family ipv4 vrf 15
        network 10.1.15.0 mask 255.255.255.128
        neighbor 192.168.15.18 remote-as 12076
        neighbor 192.168.15.18 fall-over bfd
        neighbor 192.168.15.18 activate
        neighbor 192.168.15.18 soft-reconfiguration inbound
      exit-address-family

>[!NOTE]
>Aby włączyć BFD w ramach istniejącego prywatnej komunikacji równorzędnej; należy zresetować komunikacji równorzędnej. Zobacz [komunikacje równorzędne usługi ExpressRoute z resetowania][ResetPeering]
>

## <a name="bfd-timer-negotiation"></a>Negocjowanie czasomierza BFD

Między komputerami równorzędnymi BFD wolniejsze z dwoma komputerami określają szybkość transmisji. 300 MS można ustawić Msee BFD transmisji/odbierania w odstępach czasu. Konfigurując wyższych wartości, możesz wymusić interwałami dłuższe; jednak nie krótszy.

>[!NOTE]
>Jeśli skonfigurowano obwodów komunikacji równorzędnej prywatnej magazynu geograficznie nadmiarowego usługi ExpressRoute lub użyć protokołu IPSec lokacja-lokacja sieci VPN, łączności przechowywania kopii zapasowych dla prywatnej komunikacji równorzędnej usługi ExpressRoute; Włączanie BFD za pośrednictwem prywatnej komunikacji równorzędnej może pomóc trybu failover szybciej, zgodnie z błędem łączności usługi ExpressRoute. 
>

## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe informacje lub pomoc zapoznaj się z następujących linków:

- [Tworzenie i modyfikowanie obwodu usługi ExpressRoute][CreateCircuit]
- [Tworzenie i modyfikowanie routingu dla obwodu usługi ExpressRoute][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD skracają czas dedukcji awarii łącza"

<!--Link References-->
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ResetPeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering






