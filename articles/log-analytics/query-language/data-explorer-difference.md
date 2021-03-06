---
title: Dokumentacja języka w usłudze Azure Monitor Log Analytics | Dokumentacja firmy Microsoft
description: Informacje referencyjne dotyczące języka zapytań Eksploratora danych używane przez usługę Log Analytics. Zawiera dodatkowe elementy specyficzne dla usługi Log Analytics i elementy nieobsługiwane w zapytaniach usługi Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e6d097749dae49cf6f1d710bcf01cf99dcd98a4c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244122"
---
# <a name="log-analytics-query-language-differences"></a>Zaloguj się różnice języka zapytań usługi Analytics

Gdy [usługi Log Analytics](../log-analytics-queries.md) jest oparta na [Eksploratora danych usługi Azure](/azure//data-explorer) i używa [sam język zapytań](/azure/kusto/query), wersję języka ma pewne różnice. W tym artykule identyfikuje elementy, które różnią się między wersją język używany do Eksploratora danych i wersja używana do zapytań usługi Log Analytics.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Elementy Eksploratora danych nie jest obsługiwane w usłudze Log Analytics
W poniższych sekcjach opisano elementy języka zapytań Eksploratora danych, które nie są obsługiwane przez usługę Log Analytics.

### <a name="statements-not-supported-in-log-analytics"></a>Instrukcje nie są obsługiwane w usłudze Log Analytics

* [Alias](/kusto/query/aliasstatement)
* [Parametry zapytania](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Funkcje nieobsługiwane w usłudze Log Analytics

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Operatory nie obsługiwane w usłudze Log Analytics

* [Sprzężenie krzyżowe klastra](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Dodatki nie są obsługiwane w usłudze Log Analytics

* [Wtyczka sql_request](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Dodatkowe operatory w usłudze Log Analytics
Następujące operatory obsługi określonych funkcji usługi Log Analytics i nie są dostępne poza usługą Log Analytics.

* [metodzie App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Kolejne kroki

<<<<<<< HEAD:articles/log-analytics/query-language/data-explorer-difference.md
- Pobieranie odwołań do różnych [zasoby dotyczące pisania zapytań usługi Log Analytics](kusto.md).
- Dostęp do pełnego [odwoływać się do dokumentacji języka zapytań w Eksploratorze danych](/azure/kusto/query/).
=======
- Przeczytaj o zapytaniach w [usługi Log Analytics](../log-analytics-queries.md).
- Przeprowadzenie lekcji na zapis na [zapytanie usługi Log Analytics](/log-analytics/query-language/get-started-queries.md).
- Dostęp do pełnego [dokumentacji dla Kusto](/azure/kusto/query/).
>>>>>>> 4bccab5ecb17c887658a4d2ed1bab6b22bf29ffd:articles/log-Analytics/Query-Language/kusto.MD
