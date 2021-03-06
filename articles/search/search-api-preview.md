---
title: 2017 wersja interfejsu API REST usługi Azure Search-11-11-Preview | Dokumentacja firmy Microsoft
description: Azure Search usługi REST 2017 wersja interfejsu API-11-11-Preview obejmuje eksperymentalne funkcje, takie jak synonimy i moreLikeThis wyszukiwania.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 06/28/2018
ms.author: HeidiSteen
ms.openlocfilehash: b5cb60bf16a4c904c9a6060113eba8b4d3a671ef
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112606"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Wersja interfejsu api REST usługi Azure Search 2017-11-11-Preview
W tym artykule opisano `api-version=2017-11-11-Preview` wersji usługi Azure Search interfejsu API REST, oferty eksperymentalną nie jeszcze ogólnie dostępna.

> [!NOTE]
> Funkcje w wersji zapoznawczej są dostępne do testowania i doświadczenia z celem zbierania opinii i mogą ulec zmianie. Zdecydowanie odradzamy przy użyciu podglądu interfejsów API w aplikacji produkcyjnych.


## <a name="new-in-2017-11-11-preview"></a>Nowość w 2017-11-11-Preview

[**Funkcja automatycznego uzupełniania** ](search-autocomplete-tutorial.md) łączy istniejące [API sugestie](https://docs.microsoft.com/rest/api/searchservice/suggestions) można dodać uzupełniające wpisywaniu napotyka na pasku wyszukiwania. Funkcja automatycznego uzupełniania zwraca candidate terminów zapytania, które użytkownik może wybrać jako ciąg zapytania dla następnego przeszukiwania. Sugestie zwraca rzeczywiste dokumenty w odpowiedzi na dane wejściowe częściowe: wyniki wyszukiwania są natychmiastowe i zmienia się dynamicznie wraz z rozwojem wprowadzania terminu wyszukiwania w długości i szczegółowością.

[**Wyszukiwanie kognitywnych**](cognitive-search-concept-intro.md), nowa funkcja wzbogacenia w usłudze Azure Search znajduje ukryty informacje w źródłach nietekstowych i niesortowalne tekstu, przekształcania go w zawartości wyszukiwanie pełnotekstowe w usłudze Azure Search. Następujące zasoby są wprowadzone lub modyfikowane w wersji zapoznawczej interfejsu API REST. Wszystkie inne interfejsy API REST są takie same, czy wywołać ogólnie dostępna lub podejrzeć wersji.

+ [Skillset operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [Utwórz indeksator (interfejs api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Wstępnie zdefiniowane umiejętności](cognitive-search-predefined-skills.md)

Wszystkie inne interfejsy API REST są takie same, niezależnie od tego, jak ustawić parametr api-version. Na przykład `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` i `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (bez `Preview`) działają tak samo.

## <a name="other-preview-features"></a>Inne funkcje w wersji zapoznawczej

Funkcje ogłaszane w wcześniejszych wersji zapoznawczych są nadal w publicznej wersji zapoznawczej. W przypadku wywoływania interfejsu API za pomocą starszej wersji zapoznawczej interfejsu api wersji, można w tej wersji lub przełącz się do `2017-11-11-Preview` się bez wprowadzania zmian do oczekiwane zachowanie.

+ [Pliki CSV w indeksowanie obiektów Blob Azure](search-howto-index-csv-blobs.md), wprowadzonych w programie `api-version=2015-02-28-Preview`, pozostaje w wersji zapoznawczej. Ta funkcja jest częścią indeksowanie obiektów Blob platformy Azure i jest wywoływana przez ustawienie parametru. Każdy wiersz w pliku CSV jest indeksowany jako osobny dokument.

+ [Tablice notacji JSON w indeksowanie obiektów Blob Azure](search-howto-index-json-blobs.md), wprowadzonych w programie `api-version=2015-02-28-Preview`, pozostaje w wersji zapoznawczej. Ta funkcja jest częścią indeksowanie obiektów Blob platformy Azure i jest wywoływana przez ustawienie parametru. gdzie każdy element tablicy jest indeksowany jako osobny dokument.

+ [parametr zapytania moreLikeThis](search-more-like-this.md) znajduje dokumenty, które mają zastosowanie do określonego dokumentu. Ta funkcja została w wcześniejszych wersji zapoznawczych. 


## <a name="how-to-call-a-preview-api"></a>Wywoływanie interfejsu API w wersji zapoznawczej

Starszej wersji zapoznawczych są nadal działa, ale nieodświeżone wraz z upływem czasu. Jeśli kod wywołuje `api-version=2016-09-01-Preview` lub `api-version=2015-02-28-Preview`, wywołań są nadal ważne. Jednak tylko najnowsza wersja preview jest odświeżona ulepszenia. 

Następująca składnia przykład ilustruje wywołanie wersja interfejsu API.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Usługa Azure Search jest dostępna w różnych wersjach. Aby uzyskać więcej informacji, zobacz [wersji interfejsu API](search-api-versions.md).
