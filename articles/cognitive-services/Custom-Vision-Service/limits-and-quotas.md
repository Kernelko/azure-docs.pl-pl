---
title: Ceny i limity — Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Dowiedz się o limity i przydziały dla usługi Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: anroth
ms.openlocfilehash: d0a6b7be62afcd8e9f51b6a9387c9aa33f62d9a9
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430072"
---
# <a name="pricing-and-limits"></a>Ceny i limity

Istnieją trzy warstwy kluczy dla usługi Custom Vision. Ograniczone zasoby projektu okres próbny są dołączone do swoich danych logowania Custom Vision (oznacza to, że konto usługi Azure Active Directory lub konta MSA). Są one przeznaczone do użycia w przypadku krótkich wersji próbnej usługi. Możesz zarejestrować się F0 (wersja bezpłatna) lub subskrypcji (standardowa) S0 za pośrednictwem witryny Azure portal. Zobacz odpowiednich [stronie cennika usługi Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) szczegółowe informacje na temat cen i transakcji.

Kont utworzonych podczas wczesnych bezpłatnej wersji zapoznawczej przed wprowadzeniem wersji zapoznawczych systemu Azure (1 marca 2018), zostaną zachowane ich poprzednich przydziały w przypadku ograniczonej wersji próbnej. 

Liczba uczone obrazy na projekt i tagi dla poszczególnych projektów powinny rosną z upływem czasu dla projektów S0.

||**Ograniczona wersja próbna**|**F0**|**S0**|
|-----|-----|-----|-----|
|Projekty|2|2|100|
|Uczone obrazy na projekt i klasyfikacji|5000|5000|50,000|
|Uczone obrazy na projekt, wykrywanie obiektów|5000|5000|10 000|
|Prognozy / miesiąc|10 000 |10 000|Nieograniczona liczba|
|Tagi / project|50|50|250|
|Liczba iteracji |10|10|10|
|Co najmniej etykietą obrazy na Tag klasyfikacji (ponad 50 zalecane) |5|5|5|
|Co najmniej etykietą obrazy na Tag, wykrywanie obiektów (ponad 50 zalecane)|15|15|15|
|Jak długo są przechowywane obrazy prognoz|30 dni|30 dni|30 dni|
|[Prognozowanie](https://go.microsoft.com/fwlink/?linkid=865445) operacji przy użyciu magazynu (transakcje na sekundę)|2|2|10|
|[Prognozowanie](https://go.microsoft.com/fwlink/?linkid=865445) operacji bez użycia formatu przechowywania (transakcje na sekundę)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (liczba wywołań interfejsu API na sekundę)|2|2|10|
|[Inne wywołania interfejsu API](https://go.microsoft.com/fwlink/?linkid=865446) (transakcje na sekundę)|10|10|10|
|Maksymalny rozmiar obrazu (szkolenie przekazywania obrazów) |6 MB|6 MB|6 MB|
|Maksymalny rozmiar obrazu (Prognozowane)|4 MB|4 MB|4 MB|


