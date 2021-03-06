---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 46fd31b54b7692700cce0bebe484c4f5de591942
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226530"
---
Podczas dodawania dysków danych do maszyny Wirtualnej z systemem Linux, mogą wystąpić błędy, jeśli nie ma dysku o numerze LUN 0. W przypadku dodawania dysku ręcznie przy użyciu `azure vm disk attach-new` polecenia i podaj numer LUN (`--lun`) zamiast co platformy Azure, aby określić odpowiednie jednostki LUN, powinien zachować ostrożność, że dysk już istnieje / będzie istnieć o numerze LUN 0. 

Rozważmy następujący przykład przedstawiający fragment danych wyjściowych z `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

Dwa dyski danych istnieje w jednostce LUN 0 i 1 jednostki LUN (w pierwszej kolumnie `lsscsi` dane wyjściowe zawierają szczegóły `[host:channel:target:lun]`). Obydwa dyski powinny być accessbile z poziomu maszyny Wirtualnej. Jeśli było ręcznie określony pierwszy dysk do dodania przy 1 jednostce LUN oraz drugi dysk numerem LUN 2, może być niewidoczna dysków poprawnie z w ramach maszyny Wirtualnej.

> [!NOTE]
> Azure `host` wartość to 5 w tych przykładach, ale to mogą się różnić w zależności od typu magazynu, możesz wybrać.
> 
> 

To zachowanie dysku nie jest problemu z platformą Azure, ale także sposób, w którym jądra systemu Linux jest zgodna ze specyfikacją SCSI. Gdy jądra systemu Linux skanuje do podłączonych urządzeń magistrali SCSI, urządzenie musi zostać znaleziony o numerze LUN 0 Aby system aby kontynuować, skanowanie w poszukiwaniu dodatkowych urządzeń. Jako takie:

* Przejrzyj dane wyjściowe `lsscsi` po dodaniu dysku danych, aby sprawdzić, czy masz dysku o numerze LUN 0.
* Jeśli dysk nie są wyświetlane poprawnie w ramach maszyny Wirtualnej, sprawdź, czy dysk, istnieje o numerze LUN 0.

