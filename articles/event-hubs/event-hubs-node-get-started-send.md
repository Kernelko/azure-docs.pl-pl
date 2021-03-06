---
title: Wysyłanie zdarzeń do usługi Azure Event Hubs przy użyciu środowiska Node.js | Dokumentacja firmy Microsoft
description: Wprowadzenie do wysyłania zdarzeń do usługi Event Hubs przy użyciu środowiska Node.js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: 14ea98b9d31bee08b962e8b3801ed507472ba692
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49455797"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Wysyłanie zdarzeń do usługi Azure Event Hubs przy użyciu środowiska Node.js

Azure Event Hubs to platforma do pozyskiwania i strumieniowego przesyłania danych, która umożliwia odbieranie i przetwarzanie milionów zdarzeń na sekundę. Usługa Event Hubs pozwala przetwarzać i przechowywać zdarzenia, dane lub dane telemetryczne generowane przez rozproszone oprogramowanie i urządzenia. Dane wysłane do centrum zdarzeń mogą zostać przekształcone i zmagazynowane przy użyciu dowolnego dostawcy analityki czasu rzeczywistego lub adapterów przetwarzania wsadowego/magazynowania. Aby uzyskać szczegółowe omówienie usługi Event hubs, zobacz [Przegląd usługi Event Hubs](event-hubs-about.md) i [funkcji usługi Event Hubs](event-hubs-features.md).

W tym samouczku opisano sposób wysyłania zdarzeń do Centrum zdarzeń z aplikacji napisanych w języku Node.js.

> [!NOTE]
> Ten przewodnik Szybki Start można pobrać jako próbki z [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), Zastąp `EventHubConnectionString` i `EventHubName` ciągi z własnymi wartościami Centrum zdarzeń i uruchomimy ją. Alternatywnie możesz wykonać kroki w tym samouczku, aby utworzyć własny.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne jest spełnienie następujących wymagań wstępnych:

- Środowisko node.js w wersji 8.x lub nowszy. Pobierz najnowszą wersję LTS [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (zalecane) lub dowolnym środowiskiem IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Tworzenie przestrzeni nazw usługi Event Hubs i centrum zdarzeń
Pierwszym krokiem jest skorzystanie z witryny [Azure Portal](https://portal.azure.com) w celu utworzenia przestrzeni nazw typu Event Hubs i uzyskania poświadczeń zarządzania wymaganych przez aplikację do komunikacji z centrum zdarzeń. Aby utworzyć obszar nazw i Centrum zdarzeń, wykonaj procedurę opisaną w [w tym artykule](event-hubs-create.md), a następnie wykonaj następujące czynności w ramach tego samouczka.

## <a name="clone-the-sample-git-repository"></a>Sklonowania przykładowego repozytorium usługi Git
Sklonowania przykładowego repozytorium Git z [Github](https://github.com/Azure/azure-event-hubs-node) na swojej maszynie. 

## <a name="install-nodejs-package"></a>Zainstaluj pakiet Node.js
Zainstaluj pakiet Node.js dla usługi Azure Event Hubs na swojej maszynie. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Klonowanie repozytorium Git
Pobierz lub sklonuj [przykładowe](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) z usługi Github. 

## <a name="send-events"></a>Wysyłanie zdarzeń
Zestaw SDK, które zostały sklonowane zawiera kilka przykładów, które pokazują, jak do wysyłania zdarzeń do Centrum zdarzeń za pomocą środowiska node.js. W tym przewodniku Szybki Start użyjesz **simpleSender.js** przykład. Sprawdź zdarzenia odbierane, otwórz kolejny terminala i odbieranie zdarzeń za pomocą [otrzymywać przykładowe](event-hubs-node-get-started-receive.md).

1. Otwórz projekt w programie Visual Studio Code. 
2. Utwórz plik o nazwie **ENV** w obszarze **klienta** folderu. Skopiuj i wklej przykładowy zmienne środowiskowe z **sample.env** w folderze głównym.
3. Skonfiguruj parametry połączenia Centrum zdarzeń, nazwy Centrum zdarzeń i punkt końcowy magazynu. Możesz skopiować parametry połączenia Centrum zdarzeń z **podstawowe parametry połączenia** klucza w ramach **RootManageSharedAccessKey** na stronie Centrum zdarzeń w witrynie Azure portal. Aby uzyskać szczegółowe instrukcje, zobacz [pobieranie parametrów połączenia](event-hubs-create.md#create-an-event-hubs-namespace).
4. W interfejsie wiersza polecenia platformy Azure, przejdź do **klienta** ścieżki folderu. Instalowanie pakietów węzła i skompiluj projekt, uruchamiając następujące polecenia:

    ```nodejs
    npm i
    npm run build
    ```
5. Rozpocznij wysyłanie zdarzeń za pomocą następującego polecenia: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Przejrzyj przykładowy kod 
Poniżej przedstawiono przykładowy kod do wysyłania zdarzeń do Centrum zdarzeń za pomocą środowiska node.js. Można ręcznie utworzyć plik sampleSender.js i uruchom go, aby wysyłać zdarzenia do Centrum zdarzeń. 


```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

Pamiętaj, aby ustawić zmienne środowiskowe przed uruchomieniem skryptu. Możesz skonfigurować w wierszu polecenia, jak pokazano w poniższym przykładzie, lub użyj [pakietu dotenv](https://www.npmjs.com/package/dotenv#dotenv). 

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Kolejne kroki
W tym przewodniku Szybki Start zostały wysłane wiadomości do Centrum zdarzeń za pomocą środowiska Node.js. Aby dowiedzieć się, jak odbierać zdarzenia z Centrum zdarzeń za pomocą środowiska Node.js, zobacz [odbieranie zdarzeń z Centrum zdarzeń — Node.js](event-hubs-node-get-started-receive.md)

Obejrzyj inne przykłady dla platformy Node.js dla usługi Event Hubs w witrynie [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
