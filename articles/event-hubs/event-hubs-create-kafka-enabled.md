---
title: Tworzenie klastra Apache Kafka włączone usługi Azure Event Hubs | Dokumentacja firmy Microsoft
description: Tworzenie platformy Kafka włączona przestrzeń nazw usługi Azure Event Hubs przy użyciu witryny Azure portal
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: 7ce12f9dcaa15ade95274419f99c13d5915dbaaa
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056211"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Tworzenie usługi event hubs platformy Apache Kafka włączone

Azure Event Hubs to platforma do strumieniowego przesyłania jako usługa (PaaS), która pozyskuje miliony zdarzeń na sekundę i zapewnia małe opóźnienia i wysoką przepływność do analizy w czasie rzeczywistym i wizualizacji danych Big Data.

Usługa Azure Event Hubs zapewnia punkt końcowy platformy Kafka. Ten punkt końcowy umożliwia przestrzeni nazw usługi Event Hubs zrozumieć, natywnie [platformy Apache Kafka](https://kafka.apache.org/intro) komunikatu protokołu i interfejsów API. Dzięki tej możliwości możesz mogą komunikować się za pomocą usługi event hubs, co w przypadku tematów platformy Kafka bez zmieniania klientów protokołu lub działające własne klastry. Usługa Event Hubs obsługuje [platformy Apache Kafka w wersji 1.0](https://kafka.apache.org/10/documentation.html) i nowszych.

W tym artykule opisano sposób tworzenia przestrzeni nazw usługi Event Hubs i pobieranie parametrów połączenia wymagane do łączenia aplikacji platformy Kafka z centrów zdarzeń z obsługą platformy Kafka.

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Tworzenie przestrzeni nazw usługi Event Hubs z obsługą platformy Kafka

1. Zaloguj się do [witryny Azure portal][Azure portal]i kliknij przycisk **Utwórz zasób** w lewym górnym rogu ekranu.

2. Wyszukaj usługę Event Hubs i wybierz pokazane tutaj opcje:
    
    ![Wyszukiwanie usługi Event Hubs w portalu](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Podaj unikatową nazwę i włącz platformę Kafka w przestrzeni nazw. Kliknij pozycję **Utwórz**.
    
    ![Tworzenie przestrzeni nazw](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Po utworzeniu przestrzeni nazw na karcie **Ustawienia** kliknij pozycję **Zasady dostępu współdzielonego** w celu pobrania parametrów połączenia.

    ![Klikanie pozycji Zasady dostępu współużytkowanego](./media/event-hubs-create/create-event-hub7.png)

5. Można wybrać domyślną wartość **RootManageSharedAccessKey** lub dodać nowe zasady. Kliknij nazwę zasad, a następnie skopiuj parametry połączenia. 
    
    ![Wybieranie zasad](./media/event-hubs-create/create-event-hub8.png)
 
6. Dodaj te parametry połączenia do konfiguracji aplikacji platformy Kafka.

Teraz możesz przesyłać strumieniowo zdarzenia z aplikacji, które używają protokołu platformy Kafka, do usługi Event Hubs.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat usługi Event Hubs, skorzystaj z następujących linków:

* [Stream do usługi Event Hubs z poziomu aplikacji platformy Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Dowiedz się więcej o usłudze Event Hubs dla platformy Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Dowiedz się więcej na temat usługi Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
