---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 127ade3b37380c50461c5b648265b431a2f4467e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227046"
---
*Niestandardowa* maszyna wirtualna to po prostu maszyna wirtualna utworzona za pomocą **aplikacji prezentowanej** w witrynie **Marketplace**, w przypadku której większość koniecznej pracy już wykonano. Mimo to można wybrać pewne opcje konfiguracji, w tym następujące:

* Połączenie maszyny wirtualnej z siecią wirtualną.
* Zainstalowanie agenta maszyny wirtualnej platformy Azure i rozszerzeń maszyny wirtualnej platformy Azure, takich jak chroniące przed złośliwym oprogramowaniem.
* Dodanie maszyny wirtualnej do istniejących usług w chmurze.
* Dodanie maszyny wirtualnej do istniejącego konta magazynu.
* Dodanie maszyny wirtualnej do zestawu dostępności.

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> Jeśli maszyna wirtualna ma używać sieci wirtualnej, należy określić tę sieć podczas tworzenia maszyny wirtualnej.

> * Dwie zalety używania sieci wirtualnej to łączenie się bezpośrednio z maszyną wirtualną i możliwość konfigurowania połączeń między różnymi lokalizacjami.

> * Maszynę wirtualną można skonfigurować w celu dołączenia do sieci wirtualnej tylko na etapie jej tworzenia. Aby uzyskać szczegółowe informacje na temat sieci wirtualnych, zobacz [Omówienie usługi Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md).
>
>

## <a name="to-create-the-virtual-machine"></a>Aby utworzyć maszynę wirtualną
