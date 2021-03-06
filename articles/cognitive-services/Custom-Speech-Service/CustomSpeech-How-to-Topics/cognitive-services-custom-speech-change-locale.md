---
title: Obsługiwane ustawienia regionalne i strony języków w usłudze Custom Speech Service na platformie Azure | Dokumentacja firmy Microsoft
description: Omówienie obsługiwanych języków Custom Speech Service w usługach Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 8493aeb3c1c2436900ae626ad0f34cbe8b060163
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342172"
---
# <a name="supported-locales-in-custom-speech-service"></a>Obsługiwane ustawienia regionalne w usłudze Custom Speech Service

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Custom Speech Service obecnie obsługuje dostosowywania modeli w następujących lokalizacjach:

| Typ modelu | Obsługa języków |
|----|-----|
| Modeli akustycznych | Angielski (US) (en US) |
| Modele językowe | Angielski (US) (en US), chiński (zh-CN) |

Dostosowanie modelu akustycznego jest tylko obsługiwana w US English, importowanie chińskich dane akustyczne jest obsługiwane na potrzeby testowania niestandardowych modeli języka chińskiego w trybie offline.

Przed podjęciem jakiegokolwiek działania należy wybrać odpowiednie ustawienia regionalne. Bieżące ustawienia regionalne są wskazywane w tytule tabeli na wszystkich stronach danych, modelu i wdrożenia. Aby zmienić ustawienia regionalne, kliknij przycisk "Zmień ustawienia regionalne" znajdujący się pod tytułem tabeli. Spowoduje to przejście do strony potwierdzenia ustawień regionalnych. Kliknij przycisk OK, aby powrócić do tabeli.

Należy stosować do następnych kroków
* Dowiedz się, [jak utworzyć niestandardowy model akustyczny](cognitive-services-custom-speech-create-acoustic-model.md) w celu zwiększenia dokładności rozpoznawania
* Dowiedz się, [sposobu tworzenia modelu języka niestandardowego](cognitive-services-custom-speech-create-language-model.md) można zwiększyć szybkość rozpoznawania
* Postępuj zgodnie z [wytycznych transkrypcji](cognitive-services-custom-speech-transcription-guidelines.md) do przygotowania danych
