---
title: Skalowanie przepływności w usłudze Azure Cosmos DB
description: W tym artykule opisano, jak usługi Azure Cosmos DB jest skalowana przepływność elastycznie
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: adbac964e6654e16f6c405b9a5b8669326583f3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244157"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Skalowanie przepływności w usłudze Azure Cosmos DB

W usłudze Azure Cosmos DB, aprowizowana przepływność jest reprezentowany jako żądania jednostek na sekundę (RU/s, liczba mnoga: jednostek zarezerwowanych). Pomiar koszt odczytu i zapisu dla kontenera usługi Cosmos.

![Jednostki żądania](./media/scale-throughput/figure1.png)

Możesz aprowizować jednostek żądań na kontener Cosmos lub bazy danych Cosmos. Jednostek ru zaprowizowanych w kontenerze jest dostępna wyłącznie dla operacji wykonywanych w tym kontenerze. Jednostek ru zaprowizowanych w bazie danych są współużytkowane przez wszystkie kontenery w ramach tej bazy danych (z wyjątkiem wszystkie kontenery z wyłącznie przypisanych RUs).

Elastyczne skalowanie przepływności w górę lub w dół, aby uzyskać można zwiększyć lub zmniejszyć aprowizowanych jednostek RU/s w dowolnym momencie. Aby uzyskać więcej informacji zobacz instrukcje aprowizowanie przepływności i elastycznie Skaluj kontenery Cosmos i baz danych. Globalnie skalowania przepływności, można dodać lub usunąć regiony na Twoim koncie Cosmos w dowolnym momencie. Aby uzyskać więcej informacji zobacz instrukcje Dodawanie lub usuwanie regionów do konta usługi Cosmos. Kojarzenie wielu regionów za pomocą konta usługi Cosmos jest ważne w wielu scenariuszach uzyskanie małych opóźnień i [wysokiej dostępności](high-availability.md) na świecie.

## <a name="throughput-scaling-details"></a>Szczegóły skalowania przepływności

Zainicjowanie obsługi języka R (RUS) w kontenerze Cosmos (lub bazy danych), usługi Cosmos DB gwarantuje, że R (RUS) są dostępne w *każdego* regionów skojarzonych z Twoim kontem Cosmos. Po dodaniu każdego nowego regionu do konta usługi Cosmos usługi Cosmos DB automatycznie aprowizuje RUs języka R w nowo dodanym regionie. Operacje wykonywane wbrew kontenera usługi Cosmos jest gwarantowana można pobrać jednostek ru języka R w każdym z regionów skojarzonych z Twoim kontem Cosmos. Nie można selektywnie przypisywać jednostki zarezerwowane w określonym regionie — aprowizowanych jednostek ru aprowizowanych dla kontenera Cosmos (lub bazy danych) dla wszystkich regionów skojarzonych z Twoim kontem Cosmos.

Przy założeniu, że kontener Cosmos jest skonfigurowany przy użyciu języka R (RUS) i istnieją N regiony skojarzony następnie przy użyciu konta Cosmos:

- Jeśli konto usługi Cosmos jest skonfigurowany z jednym regionie zapisu łączna liczba jednostek ru dostępna globalnie w kontenerze = R x N.
- Jeśli konto usługi Cosmos jest skonfigurowany z wieloma regionami zapisu, łączna liczba jednostek ru dostępna globalnie w kontenerze = R x (N + 1). Dodatkowe jednostki zarezerwowane języka R są automatycznie konfigurowani konfliktów aktualizacji procesu i ruch przeciw entropii między regionami.

Wybór [modelu spójności](consistency-levels.md) wpływa również na przepływności. Możesz uzyskać około 2 x przepływność odczytu, sesja, spójny prefiks i spójność na powiązana nieaktualność lub wysoki poziom spójności w porównaniu.

## <a name="next-steps"></a>Kolejne kroki

Następnie możesz dowiedzieć się, jak skonfigurować przepływność za pomocą następującego artykułu:

* [Pobieranie i ustawianie przepływności baz danych i kontenerów](set-throughput.md) 

