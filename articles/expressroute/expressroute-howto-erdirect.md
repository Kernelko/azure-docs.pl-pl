---
title: Konfigurowanie platformy Azure bezpośrednio z usługi ExpressRoute | Dokumentacja firmy Microsoft
description: Ta strona służy do konfigurowania usługi ExpressRoute bezpośrednich (wersja zapoznawcza)
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: e0009791263c45e0172abcb4836aaadde26f3ace
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887193"
---
# <a name="how-to-configure-expressroute-direct-preview"></a>Konfigurowanie usługi ExpressRoute bezpośrednich (wersja zapoznawcza)

Bezpośrednio z usługi ExpressRoute zapewnia możliwość łączenia bezpośrednio do globalnej sieci firmy Microsoft w lokalizacji komunikacji równorzędnej, strategicznie rozproszonych na całym świecie. Aby uzyskać więcej informacji, zobacz [dotyczące usługi ExpressRoute bezpośrednie łączenie](expressroute-erdirect-about.md).

> [!IMPORTANT]
> Bezpośrednie z usługi ExpressRoute jest obecnie w wersji zapoznawczej.
>
> Publiczna wersja zapoznawcza nie jest objęta umową dotyczącą poziomu usług i nie należy korzystać z niej w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą nie być obsługiwane, mogą mieć ograniczone możliwości lub mogą nie być dostępne we wszystkich lokalizacjach platformy Azure. Aby uzyskać szczegółowe informacje, zobacz [Dodatkowe warunki użytkowania wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="resources"></a>1. Utwórz zasób

1. Logowanie do platformy Azure i wybierz subskrypcję. Zasób bezpośrednio z usługi ExpressRoute i obwodów usługi ExpressRoute musi być w tej samej subskrypcji.

  ```powershell
  Connect-AzureRMAccount 

  Select-AzureRMSubscription -Subscription “<SubscriptionID or SubscriptionName>”
  ```
2. Wyświetl listę wszystkich lokalizacji, w którym są obsługiwane bezpośrednio z usługi ExpressRoute.
  
  ```powershell
  Get-AzureRmExpressRoutePortsLocation
  ```

  **Przykładowe dane wyjściowe**
  
  ```powershell
  Name                : Equinix-Ashburn-DC2
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-D
                        C2
  ProvisioningState   : Succeeded
  Address             : 21715 Filigree Court, DC2, Building F, Ashburn, VA 20147
  Contact             : support@equinix.com
  AvailableBandwidths : []

  Name                : Equinix-Dallas-DA3
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA
                        3
  ProvisioningState   : Succeeded
  Address             : 1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207
  Contact             : support@equinix.com
  AvailableBandwidths : []

  Name                : Equinix-San-Jose-SV1
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
  ProvisioningState   : Succeeded
  Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
  Contact             : support@equinix.com
  AvailableBandwidths : []
  ```
3. Określa, czy lokalizacja z wymienionych powyżej ma dostępną przepustowość

  ```powershell
  Get-AzureRMExpressRoutePortsLocations -Name "Equinix-San-Jose-SV1"
  ```

  **Przykładowe dane wyjściowe**

  ```powershell
  Name                : Equinix-San-Jose-SV1
  Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
  ProvisioningState   : Succeeded
  Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
  Contact             : support@equinix.com
  AvailableBandwidths : [
                          {
                            "OfferName": "100 Gbps",
                            "ValueInGbps": 100
                          }
                        ]
  ```
4. Tworzenie zasobu usługi ExpressRoute bezpośrednio na podstawie lokalizacji wybrano tak powyżej

  Usługa ExpressRoute bezpośrednio obsługuje QinQ i Dot1Q hermetyzacji. Jeśli wybrano QinQ, każdy obwód usługi ExpressRoute zostanie dynamicznie przypisany S Tag i będą unikatowe w obrębie całej zasobów bezpośrednio z usługi ExpressRoute. Każdy Tag języka C, w ramach obwodu muszą być unikatowe w ramach obwodu, ale nie między bezpośrednio usługi ExpressRoute.  

  Jeśli Dot1Q hermetyzacji jest zaznaczone, można zarządzać unikatowości tagu języka C (VLAN), w całej zasobów bezpośrednio z usługi ExpressRoute.  

  > [!IMPORTANT]
  > Bezpośrednie usługi ExpressRoute może być tylko jeden typ hermetyzacji. Hermetyzacja nie można zmienić po utworzeniu bezpośrednio z usługi ExpressRoute.
  > 
 
  ```powershell 
  $ERDirect = New-AzureRMExpressRoutePort -Name $Name -ResourceGroupName -$RGName -PeeringLocation $PeeringLocationName -BandwidthInGbps 100.0 -Encapsulation QinQ | Dot1Q -Location $AzureRegion
  ```

  > [!NOTE]
  > Można również ustawić atrybut hermetyzacji do Dot1Q. 
  >

  **Przykładowe dane wyjściowe:**

  ```powershell
  Name                       : Contoso-Direct
  ResourceGroupName          : Contoso-Direct-rg
  Location                   : westcentralus
  Id                         : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                               ressRoutePorts/Contoso-Direct
  Etag                       : W/"<etagnumber> "
  ResourceGuid               : <number>
  ProvisioningState          : Succeeded
  PeeringLocation            : Equinix-Seattle-SE2
  BandwidthInGbps            : 100
  ProvisionedBandwidthInGbps : 0
  Encapsulation              : QinQ
  Mtu                        : 1500
  EtherType                  : 0x8100
  AllocationDate             : Saturday, September 1, 2018
  Links                      : [
                                 {
                                   "Name": "link1",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link1",
                                   "RouterName": "tst-09xgmr-cis-1",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "link2",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link2",
                                   "RouterName": "tst-09xgmr-cis-2",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
  Circuits                   : []
  ```

## <a name="state"></a>2. Zmień stan administrator łączy

Ten proces powinien służyć do przeprowadzenia testu warstwy 1, zapewnia, że każdy obejmującej wiele połączeń jest prawidłowo poprawionego do każdego routera dla podstawowego i pomocniczego.

1. Szczegółowe informacje bezpośrednio z usługi ExpressRoute.

  ```powershell
  $ERDirect = Get-AzureRmExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
  ```
2. Włącz łącze. Powtórz ten krok, aby ustawić każdy link włączony.

  Łącza [0] jest podstawowy port i łącza [1] jest dodatkowych portów.

  ```powershell
  $ERDirect.Links[0].AdminState = “Enabled”
  Set-AzureRmExpressRoutePort -ExpressRoutePort $ERDirect
  $ERDirect = Get-AzureRmExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
  $ERDirect.Links[1].AdminState = “Enabled”
  Set-AzureRmExpressRoutePort -ExpressRoutePort $ERDirect
  ```
  **Przykładowe dane wyjściowe:**

  ```powershell
  Name                       : Contoso-Direct
  ResourceGroupName          : Contoso-Direct-rg
  Location                   : westcentralus
  Id                         : /subscriptions/<number>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                             ressRoutePorts/Contoso-Direct
  Etag                       : W/"<etagnumber> "
  ResourceGuid               : <number>
  ProvisioningState          : Succeeded
  PeeringLocation            : Equinix-Seattle-SE2
  BandwidthInGbps            : 100
  ProvisionedBandwidthInGbps : 0
  Encapsulation              : QinQ
  Mtu                        : 1500
  EtherType                  : 0x8100
  AllocationDate             : Saturday, September 1, 2018
  Links                      : [
                               {
                                 "Name": "link1",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link1",
                                 "RouterName": "tst-09xgmr-cis-1",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               },
                               {
                                 "Name": "link2",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link2",
                                 "RouterName": "tst-09xgmr-cis-2",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               }
                             ]
  Circuits                   : []
  ```

Użyj tej samej procedury z `AdminState = “Disabled”` włączyć porty w dół.

## <a name="circuit"></a>3. Tworzenie obwodu

Domyślnie można utworzyć 10 obwodów w ramach subskrypcji, gdzie jest zasobów bezpośrednio z usługi ExpressRoute. Może to być zwiększana przez dział pomocy technicznej. Odpowiedzialność za śledzenia Aprowizowana wykorzystania przepustowości. Elastycznie przepustowość jest suma przepustowości wszystkich obwodów zasobu bezpośrednio z usługi ExpressRoute i wykorzystywanych przepustowość jest użycie fizycznego bazowego interfejsy fizyczne.

Brak przepustowości obwodu dodatkowe, które mogą zostać użyte na bezpośrednio ExpressRoute tylko do obsługi scenariuszy opisanych powyżej. Są to: 40Gbps i 100Gbps.

Można tworzyć obwodów standardowa lub premium. Standardowa obwodów znajdują się w kosztów, podczas obwodów premium kosztują na podstawie przepustowości wybrane. Obwody może zostać utworzony tylko jako mierzone, jako nieograniczona liczba nie jest obsługiwana na bezpośrednio z usługi ExpressRoute.

Utwórz obwód zasobu bezpośrednio z usługi ExpressRoute.

```powershell
New-AzureRmExpressRouteCircuit -Name $Name -ResourceGroupName $ResourceGroupName -ExpressRoutePort $ERDirect -BandwidthinGbps 1.0 | 2.0 | 5.0 | 10.0 | 40.0 | 100.0  -Location $AzureRegion -SkuTier Premium -SkuFamily MeteredData 
```

Inne przepustowości obejmują: 1.0, 2.0, 5.0, 10.0 i 40.0

**Przykładowe dane wyjściowe:**

```powershell
Name                             : ExpressRoute-Direct-ckt
ResourceGroupName                : Contoso-Direct-rg
Location                         : westcentralus
Id                               : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Netwo
                                   rk/expressRouteCircuits/ExpressRoute-Direct-ckt
Etag                             : W/"<etagnumber>"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Premium_MeteredData",
                                     "Tier": "Premium",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             : 
ServiceProviderProperties        : null
ExpressRoutePort                 : {
                                     "Id": "/subscriptions/<subscriptionID>n/resourceGroups/Contoso-Direct-rg/providers/Micros
                                   oft.Network/expressRoutePorts/Contoso-Direct"
                                   }
BandwidthInGbps                  : 10
Stag                             : 2
ServiceKey                       : <number>
Peerings                         : []
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag     
```

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat usługi ExpressRoute bezpośrednio zobacz [Przegląd](expressroute-erdirect-about.md).
