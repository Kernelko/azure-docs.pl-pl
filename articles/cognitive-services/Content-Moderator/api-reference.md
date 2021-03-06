---
title: Dokumentacja interfejsu API — Content Moderator
titlesuffix: Azure Cognitive Services
description: Dowiedz się więcej o moderowania zawartości i przejrzyj interfejsów API Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: reference
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: f065310e3afeaf95af602e513421da6770c9583f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222408"
---
# <a name="content-moderator-api-reference"></a>Dokumentacja interfejsu API usługi Content Moderator

Rozpoczynanie pracy z interfejsami API usługi Azure Content Moderator, w następujący sposób: (Zobacz też [zarządzania poświadczeniami](review-tool-user-guide/credentials.md).)

- W witrynie Azure portal [subskrybować zawartości interfejsów API Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Zaloguj się w celu [narzędzie do przeglądu usługi Content Moderator](http://contentmoderator.cognitive.microsoft.com/). Zobacz [Szybki Start](quick-start.md).

## <a name="moderation-apis"></a>Interfejsy API moderowania

Następujące interfejsy API Content Moderator umożliwia konfigurowanie przepływów pracy po moderowania.

| Opis | Informacje ogólne |
| -------------------- |-------------|
| **Interfejs API moderowania obrazów**<br /><br />Skanowanie obrazów i wykrywania potencjalnych wyszukania zawartości erotycznej i przeznaczonej dla osób dorosłych za pomocą tagów, oceny zaufania i inne informacje wyodrębnione. <br /><br />Te informacje służą do publikowania, odrzuć lub przejrzyj zawartość w przepływie pracy po moderowania. <br /><br />| [Dokumentacja interfejsu API moderowania obrazów](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "dokumentacja interfejsu API moderowania obrazów")   |
| **Interfejs API moderowania tekstu**<br /><br />Skanowanie zawartości tekstowej. Warunki wulgaryzmów i identyfikowalne dane osobowe (PII) są zwracane. <br /><br />Te informacje służą do publikowania, odrzuć lub przejrzyj zawartość w przepływie pracy po moderowania.<br /><br /> | [Dokumentacja interfejsu API moderowania tekstu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "dokumentacja interfejsu API moderowania tekstu")   |
| **Moderowanie filmów wideo interfejsu API**<br /><br />Skanuj pliki wideo i wykrywanie potencjalnych zawartości dla dorosłych. <br /><br />Te informacje służą do publikowania, odrzuć lub przejrzyj zawartość w przepływie pracy po moderowania.<br /><br /> | [Interfejs API moderowania klip wideo z omówieniem](video-moderation-api.md "omówienie interfejsu API moderowania wideo")   |
| **Interfejs API zarządzania listy**<br /><br />Tworzenie i zarządzanie nimi niestandardowej listy wykluczeń lub dołączeń obrazów i tekstu. Jeśli włączona, **obrazu — dopasowania** i **tekst — ekranu** operacje czy dopasowywania rozmytego przesłanych treści z list niestandardowych. <br /><br />W celu zwiększenia wydajności możesz pominąć krok Moderowanie tekstu w uczeniu maszynowym komputera.<br /><br /> | [Dokumentacja interfejsu API zarządzania listy](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "dokumentacja interfejsu API zarządzania listy")   |

## <a name="review-api"></a>Przegląd interfejsu API

Przegląd interfejsu API zawiera następujące składniki:

| Opis | Informacje ogólne |
| -------------------- |-------------|
| **Zadania**<br /><br /> Zainicjuj Moderowanie skanowania i Przejrzyj przepływy pracy na potrzeby zawartości tekstowych i obrazów. Zadanie Moderowanie skanowanie zawartości przy użyciu interfejsu API moderowania obrazów i interfejs API moderowania tekstu. Moderowanie zadań użyj zdefiniowane i domyślnych przepływów pracy w celu wygenerowania przeglądów. <br /><br />Po ludzi moderator przejrzał automatycznie przypisane tagi i dane prognozowania i przesłane decyzji moderowanie zawartości, interfejs API przeglądu przesyła wszystkie informacje do punktu końcowego interfejsu API.<br /><br /> | [Zadania, odwołanie](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "zadania, odwołanie")   |
| **Przeglądy**<br /><br />Narzędzie do przeglądu umożliwiają bezpośrednio utworzyć przeglądy image lub text dla moderatorów ludzi.<br /><br /> Po ludzi moderator przejrzał automatycznie przypisane tagi i dane prognozowania i przesłane decyzji moderowanie zawartości, interfejs API przeglądu przesyła wszystkie informacje do punktu końcowego interfejsu API.<br /><br /> | [Przejrzyj odwołanie](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Przejrzyj odwołania")   |
| **Przepływy pracy**<br /><br />Tworzenie, aktualizowanie i uzyskać szczegółowe informacje o niestandardowych przepływów pracy tworzonych przez zespół. Przepływy pracy są definiowane za pomocą narzędzia do przeglądu. <br /> <br />Przepływy pracy zwykle używać pakietu Content Moderator, ale można również użyć niektórych innych interfejsów API, które są dostępne jako łączniki w narzędzie do przeglądu.<br /><br /> | [Odwołanie do przepływu pracy](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "odwołanie do przepływu pracy")   |


