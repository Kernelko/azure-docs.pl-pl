---
title: Konfigurowanie urządzenia Azure Data Box | Microsoft Docs
description: Dowiedz się, jak podłączyć kable do urządzenia Azure Data Box i nawiązać z nim połączenie
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/17/2018
ms.author: alkohli
ms.openlocfilehash: c3766713c87dd721f7e4c9a90624192cdb402be9
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393834"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box"></a>Samouczek: Podłączanie kabli do urządzenia Azure Data Box i nawiązywanie z nim połączenia

W tym samouczku opisano, jak podłączyć kable do urządzenia Azure Data Box Disk, włączyć je i nawiązać z nim połączenie.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Podłączanie kabli do urządzenia Data Box
> * Nawiązywanie połączenia z urządzeniem Data Box

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że:

1. Ukończono [samouczek dotyczący zamawiania urządzenia Azure Data Box](data-box-deploy-ordered.md).
2. Urządzenie Data Box zostało do Ciebie dostarczone, a stan zamówienia w portalu to **Dostarczono**. Do urządzenia dołączono etykietę wysyłkową w przezroczystej kieszeni pod bieżącą etykietą. Przechowuj tę etykietę w bezpiecznym miejscu, ponieważ użyjesz jej do wysyłki zwrotnej.
3. Zapoznano się z [wytycznymi bezpieczeństwa dotyczącymi urządzenia Data Box](data-box-safety.md).
4. Odebrano jeden kabel zasilający z uziemieniem do korzystania z urządzenia magazynującego o pojemności 100 TB.
5. Masz komputer-host zawierający dane, które mają zostać skopiowane na urządzenie Data Box. Na komputerze hosta wymagane jest:
    - Korzystanie z [obsługiwanego systemu operacyjnego](data-box-system-requirements.md).
    - Połączenie z siecią o dużej szybkości. Zdecydowanie zaleca się posiadanie co najmniej jednego połączenia 10 GbE. Jeśli połączenie 10 GbE nie jest dostępne, można użyć połączenia danych 1 GbE, ale będzie miało to wpływ na szybkość kopiowania. 
6. Należy mieć dostęp do płaskiej powierzchni, na której można umieścić urządzenie Data Box. Jeśli chcesz umieścić urządzenie na standardowej półce na stojaku, na stojaku centrum danych potrzebne jest gniazdo 7U. Urządzenie możesz umieścić na stojaku płasko lub pionowo.
7. Należy zapewnić następujące kable w celu podłączenia urządzenia Data Box do komputera hosta.
    - Dwa kable miedziane SFP+ Twinax 10 GbE (do użytku z interfejsami sieciowymi DATA 1 i DATA 2)
    - Jeden kabel sieciowy RJ-45 CAT 6 (do użytku z interfejsem sieciowym MGMT)
    - Jeden kabel sieciowy RJ-45 CAT 6A lub RJ-45 CAT 6 (do użytku z interfejsem sieciowym DATA 3 skonfigurowanym odpowiednio dla przepływności 10 GB/s lub 1 GB/s)

## <a name="cable-your-device"></a>Podłączanie kabli do urządzenia

Wykonaj poniższe kroki, aby podłączyć kable do urządzenia.

1. Sprawdź, czy urządzenie nie ma śladów naruszenia ani innych widocznych uszkodzeń. Jeśli urządzenie jest naruszone lub poważnie uszkodzone, nie kontynuuj. Od razu skontaktuj się z pracownikiem pomocy technicznej firmy Microsoft, który pomoże Ci ocenić, czy urządzenie jest w dobrym stanie, czy konieczne jest wysłanie nowego.
2. Dostarcz urządzenie do lokalizacji, w której chcesz je włączyć. Umieść je na płaskiej powierzchni. Urządzenie można także umieścić na standardowej półce na stojaku.
3. Podłącz kabel zasilający i sieciowy. Poniżej przedstawiono płytę montażową z podłączonym urządzeniem dla typowej konfiguracji. 
    
    ![Płyta montażowa urządzenia Data Box z podłączonymi kablami](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)

    1. Podłącz kabel zasilający do oznaczonego wejścia zasilania. Drugi koniec kabla zasilającego powinien był podłączony do źródła zasilania.
    2. Użyj kabla RJ-45 CAT 6 do podłączenia portu MGMT z komputerem przenośnym.            
    3. Użyj kabla RJ-45 CAT 6A do podłączenia z jednej strony portu DATA 3. Port DATA 3 jest skonfigurowany jako port 10 GbE w przypadku podłączania za pośrednictwem kabla RJ-45 CAT 6A lub jako port 1 GbE w przypadku podłączania za pośrednictwem kabla RJ-45 CAT 6.
    4. Użyj kabli miedzianych SFP+ Twinax 10 GbE, aby podłączyć odpowiednio porty DATA 1 i 2. 
    5. Drugi koniec kabli z portów danych jest podłączony do komputera hosta za pośrednictwem przełącznika 10 GbE.

4. Znajdź przycisk zasilania na przednim panelu operacyjnym urządzenia. Włącz urządzenie.

    ![Przycisk zasilania urządzenia Data Box](media/data-box-deploy-set-up/data-box-powered-door-open.png)

## <a name="connect-to-your-device"></a>Podłączanie urządzenia

Wykonaj poniższe kroki, aby skonfigurować urządzenie przy użyciu lokalnego internetowego interfejsu użytkownika i interfejsu użytkownika portalu.

1. Skonfiguruj kartę Ethernet na komputerze przenośnym używanym do nawiązywania połączenia z urządzeniem za pomocą statycznego adresu IP 192.168.100.5 i podsieci 255.255.255.0. 
2. Nawiąż połączenie z portem MGMT urządzenia i uzyskaj dostęp do jego lokalnego internetowego interfejsu użytkownika pod adresem https://192.168.100.10. Może to potrwać do 5 minut po włączeniu urządzenia.
3. Kliknij pozycję **Szczegóły**, a następnie kliknij pozycję **Przejdź do strony internetowej**.

   ![Nawiązywanie połączenia z lokalnym internetowym interfejsem użytkownika](media/data-box-deploy-set-up/data-box-connect-local-web-ui.png) 

4. Zostanie wyświetlona strona **Logowanie** dla lokalnego internetowego interfejsu użytkownika. Sprawdź, czy numer seryjny urządzenia jest zgodny zarówno w interfejsie użytkownika portalu, jak i w lokalnym internetowym interfejsie użytkownika. Urządzenie jest w tym momencie zablokowane.
5. Zaloguj się do [Azure Portal](https://portal.azure.com).
6. Pobierz poświadczenia urządzenia z portalu. Przejdź do obszaru **Ogólne > Szczegóły urządzenia**. Skopiuj wartość z pola **Hasło urządzenia**. Hasło urządzenia jest powiązane z określonym zamówieniem w portalu. 

    ![Uzyskiwanie poświadczeń urządzenia](media/data-box-deploy-set-up/data-box-device-credentials.png)
    
    
7. Podaj hasło urządzenia uzyskane w witrynie Azure Portal w poprzednim kroku, aby zalogować się do lokalnego internetowego interfejsu użytkownika urządzenia. Kliknij pozycję **Zaloguj się**.
8. Na **pulpicie nawigacyjnym** sprawdź, czy interfejsy sieciowe zostały skonfigurowane. 
    - Jeżeli w Twoim środowisku włączony jest protokół DHCP, interfejsy sieciowe są automatycznie konfigurowane. 
    - Jeśli protokół DHCP nie jest włączony, przejdź do obszaru **Ustawianie interfejsów sieciowych** i przypisz w razie potrzeby statyczne adresy IP.

    ![Pulpit nawigacyjny urządzenia](media/data-box-deploy-set-up/data-box-dashboard-1.png)

Po skonfigurowaniu sieciowych interfejsów danych można też uzyskać dostęp do lokalnego internetowego interfejsu użytkownika pod adresem `https://<IP address of a data network interface>` przy użyciu adresu IP dowolnego interfejsu DATA 1–DATA 3. 

Po zakończeniu konfiguracji urządzenia możesz połączyć się z udziałami na urządzeniu i skopiować dane z komputera na urządzenie. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono zagadnienia dotyczące usługi Azure Data Box, takie jak:

> [!div class="checklist"]
> * Podłączanie kabli do urządzenia Data Box
> * Nawiązywanie połączenia z urządzeniem Data Box

Przejdź do następnego samouczka, aby dowiedzieć się, jak skopiować dane na urządzenie Data Box.

> [!div class="nextstepaction"]
> [Kopiowanie danych na urządzenie Azure Data Box](./data-box-deploy-copy-data.md)

