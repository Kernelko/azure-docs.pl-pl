---
title: Konfigurowanie i zarządzanie zasadami replikacji dla odzyskiwania po awarii programu VMware na platformę Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft
description: W tym artykule opisano sposób konfigurowania ustawień replikacji dla odzyskiwania po awarii programu VMware na platformę Azure za pomocą usługi Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: fd987097c2ca7b1e7509a1a0e63905c36ec8fec8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212900"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Konfigurowanie zasad replikacji dla odzyskiwania po awarii programu VMware na platformę Azure i zarządzanie
W tym artykule opisano sposób konfigurowania zasad replikacji, Jeśli replikujesz maszyny wirtualne VMware na platformę Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Utwórz zasady

1. Wybierz pozycję **Zarządzaj** > **Infrastruktura usługi Site Recovery**.
1. W **oprogramowanie VMware i maszyny fizyczne**, wybierz opcję **zasady replikacji**. 
1. Kliknij przycisk **+ zasady replikacji**, a następnie określ nazwę zasad.
1. W polu **Wartość progowa celu punktu odzyskiwania** określ limit celu punktu odzyskiwania. Alerty są generowane, gdy ciągłej replikacji przekracza ten limit.
1. W obszarze **Przechowywanie punktów odzyskiwania** określ w godzinach, jak długie będzie okno przechowywania dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie przechowywania. W przypadku maszyn replikowanych do magazynu w warstwie Premium jest obsługiwany czas przechowywania do 24 godzin. Do 72 godzin jest obsługiwana dla magazynu w warstwie standardowa.
1. W obszarze **Częstotliwość wykonywania migawek na poziomie aplikacji** określ, jak często (w minutach) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami.
1. Kliknij przycisk **OK**. Tworzenie zasad powinno potrwać od 30 do 60 sekund.

Po utworzeniu zasad replikacji, odpowiadające im zasady powrotu po awarii replikacji jest tworzone automatycznie, wraz z sufiksem "powrotu po awarii". Po utworzeniu zasad, można go edytować, wybierając ją > **edytowanie ustawień**.

## <a name="associate-a-configuration-server"></a>Kojarzenie serwera konfiguracji 

Skojarz zasady replikacji z serwera konfiguracji w środowisku lokalnym.

1. Kliknij przycisk **skojarzyć**i wybierz serwer konfiguracji.

    ![Kojarzenie serwera konfiguracji](./media/vmware-azure-set-up-replication/associate1.png)

1. Kliknij przycisk **OK**. Kojarzenie serwera konfiguracji powinno potrwać od 1 do 2 minut.

    ![Kojarzenie serwera konfiguracji](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Usuwanie skojarzenia lub usuwanie zasad replikacji
1. Wybierz zasady replikacji.
    a. Aby usunąć skojarzenie zasad z serwera konfiguracji, upewnij się, czy nie replikowanych maszyn są korzystającej z zasad. Następnie kliknij przycisk **Usuń skojarzenie**.
    b. Aby usunąć zasady, upewnij się, że nie został skojarzony z serwerem konfiguracji. Następnie kliknij przycisk **Usuń**. Powinno to potrwać 30 – 60 sekund, aby usunąć.
1. Kliknij przycisk **OK**.
