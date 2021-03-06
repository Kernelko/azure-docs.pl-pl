---
title: 'Szybki start: pozyskiwanie danych z centrum zdarzeń do usługi Azure Data Explorer'
description: Z tego przewodnika Szybki start dowiesz się, jak pozyskiwać (ładować) dane do usługi Azure Data Explorer z centrum zdarzeń.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: efaf551d134d339205d40966cb84f41b408559bd
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394182"
---
# <a name="quickstart-ingest-data-from-event-hub-into-azure-data-explorer"></a>Szybki start: pozyskiwanie danych z centrum zdarzeń do usługi Azure Data Explorer

Azure Data Explorer to szybka i wysoce skalowalna usługa eksploracji danych na potrzeby danych dziennika i telemetrycznych. Usługa Azure Data Explorer umożliwia pozyskiwanie (ładowanie) danych z usługi Event Hubs — platformy do strumieniowego przesyłania dużych ilości danych i usługi pozyskiwania zdarzeń. Usługa Event Hubs może przetwarzać miliony zdarzeń na sekundę niemal w czasie rzeczywistym. W tym przewodniku Szybki start utworzysz centrum zdarzeń, nawiążesz z nim połączenie z usługi Azure Data Explorer i sprawdzisz przepływ danych w systemie.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego przewodnika Szybki start, oprócz subskrypcji platformy Azure, potrzebne są następujące elementy:

* [Klaster testowy i baza danych](create-cluster-database-portal.md)

* [Przykładowa aplikacja](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) generująca dane

* [Program Visual Studio 2017 w wersji 15.3.2 lub nowszej](https://www.visualstudio.com/vs/) do uruchomienia przykładowej aplikacji

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń

W tym przewodniku Szybki start wygenerujesz przykładowe dane i wyślesz je do centrum zdarzeń. Pierwszym krokiem jest utworzenie centrum zdarzeń. Możesz to zrobić, używając szablonu usługi Azure Resource Manager (ARM) w witrynie Azure Portal.

1. Naciśnij poniższy przycisk, aby rozpocząć wdrażanie.

    [![Wdrażanie na platformie Azure](media/ingest-data-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

    Przycisk **Wdróż na platformie Azure** powoduje przejście do witryny Azure Portal w celu wypełnienia formularza wdrożenia.

    ![Wdrażanie na platformie Azure](media/ingest-data-event-hub/deploy-to-azure.png)

1. Wybierz subskrypcję, w której chcesz utworzyć centrum zdarzeń, i utwórz grupę zasobów o nazwie *test-hub-rg*.

    ![Tworzenie grupy zasobów](media/ingest-data-event-hub/create-resource-group.png)

1. Wypełnij formularz, używając poniższych informacji.

    ![Formularz wdrożenia](media/ingest-data-event-hub/deployment-form.png)

    W przypadku wszystkich ustawień, które nie są wymienione w poniższej tabeli, użyj ustawień domyślnych.

    **Ustawienie** | **Sugerowana wartość** | **Opis pola**
    |---|---|---|
    | Subskrypcja | Twoja subskrypcja | Wybierz subskrypcję platformy Azure, która ma być używana dla centrum zdarzeń.|
    | Grupa zasobów | *test-hub-rg* | Utwórz nową grupę zasobów. |
    | Lokalizacja | *Zachodnie stany USA* | Na potrzeby tego przewodnika Szybki start wybierz wartość *Zachodnie stany USA*. W przypadku systemu produkcyjnego wybierz region, który najlepiej odpowiada Twoim potrzebom.
    | Nazwa przestrzeni nazw | Unikatowa nazwa przestrzeni nazw | Wybierz unikatową nazwę, która identyfikuje Twoją przestrzeń nazw. Na przykład *mytestnamespace*. Do podanej nazwy jest dołączana nazwa domeny *servicebus.windows.net*. Nazwa może zawierać tylko litery, cyfry i łączniki. Nazwa musi zaczynać się literą i kończyć literą lub cyfrą. Nazwa musi mieć długość od 6 do 50 znaków.
    | Nazwa centrum zdarzeń | *test-hub* | Centrum zdarzeń znajduje się w przestrzeni nazw, która zapewnia unikatowy kontener określania zakresu. Nazwa centrum zdarzeń musi być unikatowa w obrębie przestrzeni nazw. |
    | Nazwa grupy konsumentów | *test-group* | Dzięki grupom konsumentów każda z wielu aplikacji korzystających z danych może mieć osobny widok strumienia zdarzeń. |
    | | |

1. Wybierz pozycję **Zakup**, która potwierdza, że tworzysz zasoby w ramach swojej subskrypcji.

1. Wybierz pozycję **Powiadomienia** (ikonę dzwonka) na pasku narzędzi, aby monitorować proces aprowizacji. Pomyślne zakończenie wdrożenia może zająć kilka minut, ale możesz teraz przejść do następnego kroku.

## <a name="create-a-target-table-in-azure-data-explorer"></a>Tworzenie tabeli docelowej w usłudze Azure Data Explorer

Teraz w usłudze Azure Data Explorer utworzysz tabelę, do której będą wysyłane dane z usługi Event Hubs. Tabela zostanie utworzona w klastrze i bazie danych, które były aprowizowane w sekcji **Wymagania wstępne**.

1. W witrynie Azure Portal w obszarze klastra wybierz pozycję **Zapytanie**.

    ![Link do aplikacji Zapytanie](media/ingest-data-event-hub/query-explorer-link.png)

1. Skopiuj poniższe polecenie do okna i wybierz pozycję **Uruchom**.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```

    ![Uruchamianie zapytania create](media/ingest-data-event-hub/run-create-query.png)

1. Skopiuj poniższe polecenie do okna i wybierz pozycję **Uruchom**.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```
    To polecenie mapuje przychodzące dane JSON na nazwy kolumn i typy danych używane podczas tworzenia tabeli.

## <a name="connect-to-the-event-hub"></a>Łączenie z centrum zdarzeń

Teraz połączysz się z centrum zdarzeń z usługi Azure Data Explorer, aby dane trafiające do centrum zdarzeń były przesyłane strumieniowo do tabeli testowej.

1. Wybierz pozycję **Powiadomienia** na pasku narzędzi, aby sprawdzić, czy wdrożenie centrum zdarzeń zakończyło się pomyślnie.

1. W obszarze utworzonego klastra wybierz pozycję **Bazy danych**, a następnie pozycję **TestDatabase**.

    ![Wybieranie testowej bazy danych](media/ingest-data-event-hub/select-test-database.png)

1. Wybierz pozycję **Pozyskiwanie danych**, a następnie pozycję **Dodaj połączenie danych**.

    ![Wprowadzanie danych](media/ingest-data-event-hub/data-ingestion-create.png)

1. Wypełnij formularz, używając poniższych informacji, a następnie wybierz pozycję **Utwórz**.

    ![Połączenie centrum zdarzeń](media/ingest-data-event-hub/event-hub-connection.png)

    **Ustawienie** | **Sugerowana wartość** | **Opis pola**
    |---|---|---|
    | Nazwa połączenia danych | *test-hub-connection* | Nazwa połączenia, które chcesz utworzyć w usłudze Azure Data Explorer.|
    | Przestrzeń nazw centrum zdarzeń | Unikatowa nazwa przestrzeni nazw | Wybrana wcześniej nazwa, która identyfikuje Twoją przestrzeń nazw. |
    | Centrum zdarzeń | *test-hub* | Utworzone przez Ciebie centrum zdarzeń. |
    | Grupa konsumentów | *test-group* | Grupa konsumentów zdefiniowana w utworzonym przez Ciebie centrum zdarzeń. |
    | Tabela | *TestTable* | Tabela utworzona przez Ciebie w obszarze **TestDatabase**. |
    | Format danych | *JSON* | Obsługiwane są formaty JSON i CSV. |
    | Mapowanie kolumn | *TestMapping* | Mapowanie utworzone przez Ciebie w obszarze **TestDatabase**. |

    W tym przewodniku Szybki start będziesz używać *routingu statycznego* z centrum zdarzeń, w którym określisz nazwę tabeli, format pliku i mapowanie. Możesz również używać routingu dynamicznego, w którym te właściwości będą ustawiane przez aplikację.

## <a name="copy-the-connection-string"></a>Kopiowanie parametrów połączenia

Gdy uruchamiasz aplikację w celu wygenerowania przykładowych danych, potrzebujesz parametrów połączenia dla przestrzeni nazw centrum zdarzeń.

1. W obszarze utworzonej przez Ciebie przestrzeni nazw centrum zdarzeń wybierz pozycję **Zasady dostępu współużytkowanego**, a następnie pozycję **RootManageSharedAccessKey**.

    ![Zasady dostępu współużytkowanego](media/ingest-data-event-hub/shared-access-policies.png)

1. Skopiuj zawartość pola **Parametry połączenia — klucz podstawowy**.

    ![Parametry połączenia](media/ingest-data-event-hub/connection-string.png)

## <a name="generate-sample-data"></a>Generowanie danych przykładowych

Po nawiązaniu połączenia między usługą Azure Data Explorer i centrum zdarzeń wygenerujesz dane za pomocą przykładowej aplikacji, która została pobrana.

1. Otwórz przykładowe rozwiązanie aplikacji w programie Visual Studio.

1. W pliku *program.cs* zaktualizuj stałą `connectionString` tak, aby zawierała parametry połączenia skopiowane z przestrzeni nazw centrum zdarzeń.

    ```csharp
    const string eventHubName = "test-hub";
    // Copy the connection string ("Connection string-primary key") from your Event Hub namespace.
    const string connectionString = @"<YourConnectionString>";
    ```

1. Skompiluj i uruchom aplikację. Aplikacja wysyła komunikaty do centrum zdarzeń i co dziesięć sekund wyświetla stan.

1. Po wysłaniu przez aplikację kilku komunikatów przejdź do następnego kroku: przeglądania przepływu danych do centrum zdarzeń i tabeli testowej.

## <a name="review-the-data-flow"></a>Przeglądanie przepływu danych

1. W witrynie Azure Portal w obszarze centrum zdarzeń zobaczysz wzrost aktywności, gdy aplikacja jest uruchomiona.

    ![Wykres centrum zdarzeń](media/ingest-data-event-hub/event-hub-graph.png)

1. Wróć do aplikacji i zatrzymaj ją, gdy osiągnie 99 komunikatów.

1. W testowej bazie danych uruchom poniższe zapytanie, aby sprawdzić, ile komunikatów zostało przekazanych do tej pory do bazy danych.

    ```Kusto
    TestTable
    | count
    ```

1. Uruchom poniższe zapytanie, aby sprawdzić zawartość komunikatów.

    ```Kusto
    TestTable
    ```

    Zestaw wyników powinien wyglądać tak jak na poniższej ilustracji.

    ![Zestaw wyników komunikatów](media/ingest-data-event-hub/message-result-set.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz ponownie używać centrum zdarzeń, wyczyść grupę zasobów **test-hub-rg**, aby uniknąć ponoszenia kosztów.

1. W witrynie Azure Portal wybierz **grupy zasobów** daleko po lewej stronie, a następnie wybierz utworzoną grupę zasobów.  

    Jeśli menu po lewej stronie jest zwinięte, wybierz ![przycisk Rozwiń,](media/ingest-data-event-hub/expand.png) aby je rozwinąć.

   ![Wybieranie grupy zasobów do usunięcia](media/ingest-data-event-hub/delete-resources-select.png)

1. W obszarze **test-resource-group** wybierz pozycję **Usuń grupę zasobów**.

1. W nowym oknie wpisz nazwę grupy zasobów do usunięcia (*test-hub-rg*), a następnie wybierz pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Szybki start: wykonywanie zapytań o dane w usłudze Azure Data Explorer](web-query-data.md)
