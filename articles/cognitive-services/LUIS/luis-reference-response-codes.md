---
title: Kody odpowiedzi HTTP interfejsu API Understanding (LUIS) języka — Azure | Dokumentacja firmy Microsoft
titleSuffix: Azure
description: Zrozumienie, jakie kody odpowiedzi HTTP są zwracane z tworzenia usługi LUIS i interfejsów API punktu końcowego
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: f005c7f13cd05ce3ca6ce494c4a2670106cb3637
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900985"
---
# <a name="luis-api-http-response-codes"></a>Kody odpowiedzi HTTP interfejsu API usługi LUIS
[Tworzenia](https://aka.ms/luis-authoring-apis) i [punktu końcowego](https://aka.ms/luis-endpoint-apis) interfejsy API zwracają kody odpowiedzi HTTP. Chociaż komunikatach odpowiedzi zawiera informacje specyficzne dla żądania, kod stanu odpowiedzi HTTP jest ogólny. 

## <a name="common-status-codes"></a>Typowe kody stanu
W poniższej tabeli wymieniono niektóre najbardziej typowe kody stanu odpowiedzi HTTP dla [tworzenia](https://aka.ms/luis-authoring-apis) i [punktu końcowego](https://aka.ms/luis-endpoint-apis) interfejsów API:

|Kod|Interfejs API|Wyjaśnienie|
|:--|--|--|
|400|Tworzenie punktu końcowego|Parametry żądania są nieprawidłowe, co oznacza, że wymagane parametry są brakujące, źle sformułowane lub zbyt duży|
|400|Tworzenie punktu końcowego|Treść żądania jest nieprawidłowa, co oznacza, że za pomocą pliku JSON jest Brak, źle sformułowane lub zbyt duży|
|401|Tworzenie|używany klucz subskrypcji dla punktu końcowego, zamiast tworzenia klucza|
|401|Tworzenie punktu końcowego|nieprawidłowy, źle sformułowane lub pusty klucz|
|401|Tworzenie punktu końcowego| klucz nie jest zgodna regionu|
|401|Tworzenie|nie jesteś właścicielem lub współpracownika|
|401|Tworzenie|Nieprawidłowa kolejność wywołań interfejsu API|
|403|Tworzenie punktu końcowego|Przekroczono Całkowity miesięczny limit przydziału klucza|
|409|Endpoint|nadal trwa ładowanie aplikacji|
|410|Endpoint|Aplikacja musi być retrained i ponowne opublikowanie|
|414|Endpoint|kwerendy przekracza maksymalną dozwoloną długość|
|429|Tworzenie punktu końcowego|Przekroczono limit szybkości (żądań na sekundę)|