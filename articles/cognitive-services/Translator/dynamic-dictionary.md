---
title: Dynamiczny słownik — interfejs API tekstu usługi Translator
titlesuffix: Azure Cognitive Services
description: Jak używać funkcji dynamiczny słownik interfejsu API tłumaczenia tekstu.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 0b96325b2d29abd230e4c389b176e97542a70282
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648251"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Jak używać funkcji dynamiczny słownik interfejs API tekstu usługi Translator

Jeśli znasz już tłumaczeń, które mają zostać zastosowane do wyrazu lub frazy, możesz podać go jako kod znaczników w żądaniu. Dynamiczny słownik jest tylko bezpieczne dla rzeczowniki złożone, np. prawidłowe nazwy i nazwy produktu.

**Składnia:**

< tłumaczenia mstrans:dictionary = "tłumaczenia frazę" > frazy < / mstrans:dictionary >

**Przykład: en-de:**

Źródło danych wejściowych: słowo < tłumaczenia mstrans:dictionary =\"wordomatic\"> wyrazu lub frazy < / mstrans:dictionary > wpis słownika.

Docelowe danych wyjściowych: brzeczki Das "wordomatic" ISTA NIP Wörterbucheintrag.

Ta funkcja działa tak samo z lub bez trybu HTML.

Funkcja powinny być używane rzadko. Odpowiednie i znacznie lepiej sposób dostosowywania tłumaczenia jest przy użyciu niestandardowych w usłudze Translator. Niestandardowe w usłudze Translator sprawia, że pełne wykorzystanie kontekstu i prawdopodobieństwa statystycznych. Jeśli masz lub można utworzyć dane szkoleniowe, które z miejsca pracy lub frazę w kontekście otrzymasz znacznie lepsze wyniki. Można znaleźć więcej informacji na temat niestandardowych w usłudze Translator w [ http://aka.ms/CustomTranslator ](http://aka.ms/CustomTranslator).
