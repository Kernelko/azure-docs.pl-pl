---
title: Interfejs Academic Graph atrybutów jednostki - interfejsu Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Więcej informacji na temat atrybutów jednostki, których można używać z Academic Graph w interfejsu Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a203fdf6562dabb1b9d6e8ab5bb8f46ff6d5dc27
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902794"
---
# <a name="entity-attributes"></a>Atrybuty obiektu:

Interfejs academic graph składa się z 7 typów jednostek. Wszystkie jednostki będą mieć identyfikator jednostki i typu jednostki.

## <a name="common-entity-attributes"></a>Wspólne atrybuty jednostki
Name (Nazwa)    |Opis                |Typ       | Operacje
------- | ------------------------- | --------- | ----------------------------
Identyfikator      |Identyfikator jednostki                  |Int64      |Równa się
Ty      |Typ jednostki                |Wyliczenia   |Równa się

## <a name="entity-type-enum"></a>Jednostka typu enum
Name (Nazwa)                                                            |wartość
----------------------------------------------------------------|-----
[Dokument](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Dziennik](JournalEntityAttributes.md)                           |2
[Seria konferencji](JournalEntityAttributes.md)                 |3
[Wystąpienie konferencji](ConferenceInstanceEntityAttributes.md)    |4
[Przynależność](AffiliationEntityAttributes.md)                   |5
[Zakres badań](FieldsOfStudyEntityAttributes.md)                      |6

