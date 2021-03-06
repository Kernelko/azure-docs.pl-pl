---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8e6db54853efcba4d648c1d3bc793a9d1ce57441
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165374"
---
<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Aby Podłączanie kabli do urządzenia zasilania
> [!NOTE]
> Zarówno obudów w urządzeniu StorSimple obejmują PCMs nadmiarowe. Dla każdej obudowie PCMs musi być zainstalowane i połączone z różnymi źródłami zasilania aby zapewnić wysoką dostępność.
> 
> 

1. Upewnij się, że przełączniki zasilania na wszystkich PCMs znajdują się w pozycji wył.
2. Podstawowy obudowy Połącz się zarówno PCMs przewodów zasilania. Kable są oznaczone na czerwono na diagramie okablowania power poniżej.
3. Należy upewnić się czy dwa PCMs na podstawowym obudowy źródłami zasilania oddzielne.
4. Dołącz do włączania stojak jednostki dystrybucji zasilania przewodów zasilania, jak pokazano w usłudze Power BI okablowania diagramu.
5. Powtórz kroki od 2 do 4 dla obudowy EBOD.
6. Włącz obudowy EBOD przestawiając przycisk zasilania na każdy PCM pozycji dalej.
7. Upewnij się, że obudowa EBOD jest włączona, sprawdzając, czy diod LED na odwrocie podkładki kontrolera EBOD są włączone.
8. Włącz głównej obudowy, przestawiając każdego PCM przełącznika w pozycji włączone.
9. Sprawdź, czy system jest włączony, zapewniając z kontrolerem urządzenia, których włączono diod LED.
10. Upewnij się, że połączenie między kontrolera EBOD i z kontrolerem urządzenia jest aktywny, upewniając się, że cztery diod LED obok portów SAS kontrolera EBOD są zielone.
    
    > [!IMPORTANT]
    > Aby zapewnić wysoką dostępność w systemie, firma Microsoft zaleca ściśle przestrzegać możliwości okablowania schemat pokazano na poniższym diagramie.
    > 
    > 
    
    ![Podłączanie kabli do urządzenia 4U zasilania](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Przewody zasilania**
    
    | Etykieta | Opis |
    |:--- |:--- |
    | 1 |Podstawowy obudowy |
    | 2 |MODULE PCM 0 |
    | 3 |MODULE PCM 1 |
    | 4 |Kontrolera 0 |
    | 5 |Kontrolera 1 |
    | 6 |Kontrolera EBOD 0 |
    | 7 |Kontrolera EBOD 1 |
    | 8 |Obudowa EBOD |
    | 9 |PDU |

