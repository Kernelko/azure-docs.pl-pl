---
title: Usługi Azure Event Hubs dla platformy Apache Kafka | Dokumentacja firmy Microsoft
description: Omówienie i wprowadzenie do platformy Kafka włączone usługi Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: b1a046fdb62926d32c05bab5a1616b250911865b
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352577"
---
# <a name="azure-event-hubs-for-apache-kafka-preview"></a>Usługa Azure Event Hubs dla platformy Apache Kafka (wersja zapoznawcza)

Usługa Event Hubs zapewnia punktu końcowego platformy Kafka, który może być używany przez istniejącą platformy Kafka na podstawie aplikacji zamiast uruchamiania klastra Kafka. Usługa Event Hubs obsługuje [protokołu platformy Apache Kafka w wersji 1.0 lub nowszy](https://kafka.apache.org/documentation/)i współpracuje z istniejącymi aplikacjami platformy Kafka, w tym narzędzia MirrorMaker. 

## <a name="what-does-event-hubs-for-kafka-provide"></a>Co oferuje usługi Event Hubs dla platformy Kafka

Usługa Event Hubs dla platformy Kafka funkcji zawiera head protokołu, na podstawie usługi Azure Event Hubs jest binarny zgodny z platformą Kafka w wersji 1.0 i nowszym w przypadku zarówno odczytywanie z oraz zapisywanie do tematów platformy Kafka. Punkt końcowy platformy Kafka z poziomu aplikacji może korzystać z żadne zmiany kodu, ale zmiana minimalnej konfiguracji. Należy zaktualizować parametry połączenia w konfiguracji w celu wskazania punktu końcowego platformy Kafka, udostępnianych przez Centrum zdarzeń zamiast wskazujący klastra Kafka. Następnie należy uruchomić, przesyłanie strumieniowe zdarzeń z poziomu aplikacji, które używają protokołu Kafka do usługi Event Hubs. 

Koncepcyjnie Kafka i usługi Event Hubs są niemal identyczne: są obydwa dzienniki podzielonym na partycje, stworzona z myślą o danych przesyłanych strumieniowo. Poniższa tabela zawiera mapowanie pojęcia między platformy Kafka i centrów zdarzeń.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Usługi Kafka i Centrum zdarzeń koncepcyjne mapowanie

| Koncepcja platformy Kafka | Koncepcja centrów zdarzeń|
| --- | --- |
| Klaster | Przestrzeń nazw |
| Temat | Event Hubs |
| Partycja | Partycja|
| Grupa konsumentów | Grupa konsumentów |
| Przesunięcie | Przesunięcie|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Podstawowe różnice między Kafka i usługi Event Hubs

Gdy [platformy Apache Kafka](https://kafka.apache.org/) to oprogramowanie, które można uruchomić wszędzie tam, gdzie możesz wybrać, usługa Event Hubs to usługa w chmurze podobne do usługi Azure Blob Storage. Brak serwerów lub sieci do zarządzania ani nie brokerów, aby skonfigurować. Tworzenie przestrzeni nazw, czyli nazwę FQDN, w którym będzie funkcjonować tematów, a następnie utworzyć usługi Event Hubs lub tematów w ramach tej przestrzeni nazw. Aby uzyskać więcej informacji na temat usługi Event Hubs i przestrzenie nazw, zobacz [funkcji usługi Event Hubs](event-hubs-features.md#namespace). Jako usługa w chmurze usługa Event Hubs używa pojedynczego stabilne wirtualnego adresu IP jako punkt końcowy, dzięki czemu klienci nie muszą wiedzieć o brokerów lub maszyn w ramach klastra. 

Skalowanie w usłudze Event Hubs jest kontrolowana przez liczbę jednostek przepływności można kupić, każda jednostka przepływności uprawniających należy do 1 MB na sekundę lub 1000 zdarzeń na sekundę transferu danych przychodzących. Domyślnie usługa Event Hubs jest skalowany w górę jednostek przepływności w przypadku osiągnięcia limitu z [automatyczne rozszerzanie](event-hubs-auto-inflate.md) funkcji; to także funkcji współpracuje z usługą Event Hubs dla platformy Kafka funkcji. 

### <a name="security-and-authentication"></a>Zabezpieczenia i uwierzytelnianie

Usługa Azure Event Hubs wymaga protokołu SSL lub TLS dla całej komunikacji i używa sygnatur dostępu współdzielonego (SAS) do uwierzytelniania. To wymaganie obowiązuje również punkt końcowy platformy Kafka w usłudze Event Hubs. Zgodność z platformą Kafka usługa Event Hubs używa zwykły SASL dla uwierzytelniania oraz protokołu SSL SASL zabezpieczeń transportu. Aby uzyskać więcej informacji na temat zabezpieczeń w usłudze Event Hubs, zobacz [uwierzytelniania usługi Event Hubs i zabezpieczeń](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Inne funkcje usługi Event Hubs jest dostępna dla platformy Kafka

Centra zdarzeń dla funkcji platformy Kafka umożliwia jeden protokół do pisania i odczytać za pomocą innego, tak, aby kontynuować publikowanie za pomocą platformy Kafka w Twojej bieżącej producentów platformy Kafka i dodawać czytelników przy użyciu usługi Event Hubs, takich jak Azure Stream Analytics lub Azure Functions. Ponadto usługa Event Hubs funkcje, takie jak [przechwytywania](event-hubs-capture-overview.md) i [odzyskiwania po awarii replikacji geograficznej](event-hubs-geo-dr.md) również działać z usługą Event Hubs dla platformy Kafka funkcji.

## <a name="features-that-are-not-supported-in-the-preview"></a>Funkcje, które nie są obsługiwane w wersji zapoznawczej

W publicznej wersji zapoznawczej usługi Event Hubs dla platformy Kafka integracji nie są obsługiwane następujące funkcje platformy Kafka:

*   Producent Idempotentne
*   Transakcji
*   Kompresja
*   Przechowywania na podstawie rozmiaru
*   Kompaktowanie dziennika
*   Dodawanie partycji istniejącemu tematowi
*   Obsługa protokołu HTTP platformy Kafka interfejsu API
*   Platforma Kafka Connect
*   Kafka Streams

## <a name="next-steps"></a>Kolejne kroki

W tym artykule podano zapoznać się z wprowadzeniem do usługi Event Hubs dla platformy Kafka. Aby dowiedzieć się więcej, zobacz następujące linki:

* [Jak utworzyć Kafka włączone usługi Event Hubs](event-hubs-create-kafka-enabled.md)
* [Stream do usługi Event Hubs z poziomu aplikacji platformy Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Poznaj więcej przykładów w usłudze GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
* Rozpocznij pracę od [samouczka dotyczącego usługi Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

 
 

