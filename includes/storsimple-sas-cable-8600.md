---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 792589f4aa2a80c05378224ffe4e4d1dad2f935c
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165304"
---
<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>Aby dołączyć kabli SAS
1. Zidentyfikuj podstawowe i obudowy EBOD. Dwie obudowy, można zidentyfikować, analizując ich odpowiednich płaszczyznach Wstecz. Zobacz poniższy obraz, aby uzyskać wskazówki. 
   
    ![Tworzenie kopii płaszczyzny podstawowej i obudów EBOD](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Tworzenie kopii widoku podstawowych i obudowy EBOD**
   
   | Etykieta | Opis |
   |:--- |:--- |
   | 1 |Podstawowy obudowy |
   | 2 |Obudowa EBOD |
2. Znajdź numery seryjne podstawowej i obudowy EBOD. Nalepki numer seryjny jest dołączone do tyłu Wyczyść z każdej obudowie. Numery seryjne musi być taka sama na obu obudowy. [Skontaktuj się z Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) natychmiast, jeśli numery seryjne nie są zgodne. Widać na ilustracji, aby zlokalizować numerów seryjnych.
   
    ![Widok tyłu przedstawiający numer seryjny obudowy](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Lokalizacja nalepkę numer seryjny**
   
   | Etykieta | Opis |
   |:--- |:--- |
   | 1 |Wyczyść obudowa |
3. Za pomocą podanego kabli SAS obudowy EBOD połączyć się z podstawowym obudowy w następujący sposób:
   
   1. Zidentyfikuj cztery porty SAS w obudowie podstawowego i obudowy EBOD. Porty sygnatury dostępu Współdzielonego są oznaczone jako EBOD na podstawowym obudowy i odpowiadają A port na obudowę EBOD, jak pokazano na sygnatury dostępu Współdzielonego okablowania na ilustracji poniżej.
   2. Użyj podana kabli SAS, tak aby połączyć z A. porty EBOD
   3. Powinny być połączone portu EBOD na kontrolerze 0, A port kontrolera EBOD 0. Port EBOD na kontrolerze 1 powinien być połączony element port kontrolera EBOD 1. Widać na ilustracji, aby uzyskać wskazówki. 
      
      ![Sygnatury dostępu Współdzielonego okablowania dla Twojego urządzenia](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **Sygnatury dostępu Współdzielonego okablowania**
      
      | Etykieta | Opis |
      |:--- |:--- |
      | A |Podstawowy obudowy |
      | B |Obudowa EBOD |
      | 1 |Kontrolera 0 |
      | 2 |Kontrolera 1 |
      | 3 |Kontrolera EBOD 0 |
      | 4 |Kontrolera EBOD 1 |
      | 5, 6 |Portów SAS na podstawowym obudowa (EBOD etykietami) |
      | 7, 8 |Portów SAS w obudowie EBOD (Port A) |

