---
title: Obsługa języków — interfejs API mowy usługi
titleSuffix: Azure Cognitive Services
description: Lista języków naturalnych obsługiwane przez usługę rozpoznawania mowy.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: erhopf
ms.openlocfilehash: 5adc0e39c271b71d6c003eaba6cb5b8a71531bd7
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471445"
---
# <a name="language-and-region-support-for-speech-service-api"></a>Obsługa języka i regionu dla interfejsu API mowy usługi

Różne języki są obsługiwane w przypadku różnych funkcji usługi mowy. W poniższej tabeli podsumowano obsługę języka.

## <a name="speech-to-text"></a>Zamiana mowy na tekst

Interfejs API rozpoznawania mowy firmy Microsoft obsługuje następujące języki. Różne poziomy możliwości dostosowania są dostępne dla każdego języka.

  Kod | Język | [Akustyczna](how-to-customize-acoustic-models.md) | [Dostosowania języka](how-to-customize-language-model.md) | [Wymowa dostosowania](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar EG | Arabski (Egipt), standard nowoczesne | Nie | Yes | Nie
 ES urzędu certyfikacji | Kataloński | Nie | Nie | Nie
 Akcelerator deweloperski w wersji DK | Duński (Dania) | Nie | Nie | Nie
 de-DE. | Niemiecki (Niemcy) | Yes | Yes | Nie
 EN-AU | Angielski (Australia) | Nie | Yes | Yes
 EN-CA | Angielski (Kanada) | Nie | Yes | Yes
 en-GB | Angielski (Zjednoczone Królestwo) | Nie | Yes | Yes
 EN-IN | English (India) | Yes | Yes | Yes
 EN NZ | Angielski (Nowa Zelandia) | Nie | Yes | Yes  
 pl-PL | Angielski (Stany Zjednoczone) | Yes | Yes | Yes
 es-ES | Hiszpański (Hiszpania) | Nie | Yes | Nie
 es-MX | Hiszpański (Meksyk) | Nie | Yes | Nie
 fi-FI | Fiński (Finlandia) | Nie | Nie | Nie
 fr-CA | Francuski (Kanada) | Nie | Yes | Nie
 fr-FR | Francuski (Francja) | Yes | Yes | Nie
 w | Hindi (Indie) | Nie | Yes | Nie
 IT-IT | Włoski (Włochy) | Yes | Yes | Nie
 ja-JP | Japoński (Japonia) | Nie | Yes | Nie
 ko-KR | Koreański (Korea) | Nie | Yes | Nie
 nb-NO | Norweski (Bokmal) (Norwegia) | Nie | Nie | Nie
 NL-NL | Holenderski (Holandia) | Nie | Yes | Nie
 pl-PL | Polski (Polska) | Nie | Nie | Nie
 pt-BR | Portugalski (Brazylia) | Nie | Yes | Nie
 pt-PT | Portugalski (Portugalia) | Nie | Yes | Nie
 ru-RU | Rosyjski (Rosja) | Yes | Yes | Nie
 sv-SE | Szwedzki (Szwecja) | Nie | Nie | Nie
 zh-CN | Chiński (mandaryński uproszczony) | Yes | Yes | Nie
 zh-HK | Chiński (mandaryński, tradycyjny) | Nie | Yes | Nie
 zh-TW | Chiński (mandaryński tajwańskie) | Nie | Yes | Nie
 th TH | Tajski (Tajlandia) | Nie | Nie | Nie


## <a name="text-to-speech"></a>Zamiana tekstu na mowę

Interfejs API synteza mowy oferuje głosy następujące, z których każdy obsługuje określonego języka i dialektu, identyfikowane za pomocą ustawień regionalnych.

Ustawienia regionalne | Język | Płeć | Mapowanie nazwy usługi
-------|----------|---------|--------------------
ar EG\* | Arabski (Egipt) | Kobieta | "Microsoft Server mowy tekstu na głos mowy (ar np Hoda)"
ar-SA | Arabski (Arabia Saudyjska) | Mężczyzna | "Microsoft Server mowy Text na głos mowy (ar-SA Naayf)"
bg-BG | Bułgarski | Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (bg-BG Ivanowi)"
ES urzędu certyfikacji | Kataloński | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (Kanada ES, HerenaRUS)"
cs-CZ | Czeski | Mężczyzna | "Microsoft Server mowy tekstu na głos mowy (cs-CZ, Jakub)"
cs-CZ | Czeski | Mężczyzna | "Microsoft Server mowy tekstu na głos mowy (cs-CZ, Vit)"
Akcelerator deweloperski w wersji DK | Duński | Kobieta | "Microsoft Server mowy Text na głos mowy (da-DK HelleRUS)"
de-AT | Niemiecki (Austria) | Mężczyzna | "Microsoft Server mowy Text na głos mowy (de-AT, Michael)"
de-CH | Niemiecki (Szwajcaria) | Mężczyzna | "Microsoft Server mowy Text na głos mowy (de-CH, Karsten)"
de-DE. | Niemiecki (Niemcy) | Kobieta | "Microsoft Server mowy Text na głos mowy (de-DE, Hedda)"
| | | Kobieta | "Microsoft Server mowy Text na głos mowy (de-DE, HeddaRUS)"
| | | Mężczyzna | "Microsoft Server mowy Text na głos mowy (de-DE, Stefan, Apollo)"
el-GR | Grecki | Mężczyzna | "Microsoft Server mowy Text na głos mowy (el-GR Stefanos)"
EN-AU | Angielski (Australia) | Kobieta | "Microsoft Server mowy Text na głos mowy (en-AU Catherine)"
| | | Kobieta | "Microsoft Server mowy Text na głos mowy (en-AU HayleyRUS)"
EN-CA | Angielski (Kanada) | Kobieta | "Microsoft Server mowy Text na głos mowy (en-CA Anna)"
| | | Kobieta | "Microsoft Server mowy Text na głos mowy (en-CA HeatherRUS)"
en-GB | English (UK) | Kobieta | "Microsoft Server mowy Text na głos mowy (en-GB Susan, Apollo)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (en-GB HazelRUS)"
| | |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en-GB George, Apollo)"
EN-IE | Angielski (Irlandia) |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en-IE, Sean)"
EN-IE | Angielski (Irlandia) |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en-IE, Shaun)"
EN-IN | English (India) | Kobieta | "Microsoft Server mowy Text na głos mowy (en-IN, Heera, Apollo)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (en-IN PriyaRUS)"
| | |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en-IN, Ravi, Apollo)"
pl-PL | English (US) |Kobieta | "Microsoft Server mowy Text na głos mowy (en US, ZiraRUS)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (en US, JessaRUS)"
| | |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en US, BenjaminRUS)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (en US, Jessa24kRUS)"
| | |Mężczyzna | "Microsoft Server mowy Text na głos mowy (en US, Guy24kRUS)"
es-ES | Hiszpański (Hiszpania) |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (es-ES, Laura, Apollo)"
| | |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (es-ES, HelenaRUS)"
| | |Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (es-ES, urządzenia Pablo, Apollo)"
es-MX | Hiszpański (Meksyk) | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosu (es-MX, HildaRUS)"
| | | Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosu (es-MX, Raul, Apollo)"
fi-FI | Fiński | Kobieta | "Microsoft Server mowy Text na głos mowy (fi-FI, HeidiRUS)"
fr-CA | Francuski (Kanada) |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-CA Caroline)"
| | | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-CA HarmonieRUS)"
FR-CH | Francuski (Szwajcaria)|Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-CH, Guillaume)"
fr-FR | Francuski (Francja)|Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-FR, Julie, Apollo)"
| | |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-FR, HortenseRUS)"
| | |Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (fr-FR, Paul, Apollo)"
HE-IL| Hebrajski (Izrael) | Mężczyzna| "Microsoft Server mowy Text na głos mowy (he-IL Asaf)"
w | Hindi (Indie) | Kobieta | "Microsoft Server mowy Text na głos mowy (hi-IN, Kalpana, Apollo)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (cześć IN, Kalpana)"
| | | Mężczyzna | "Microsoft Server mowy Text na głos mowy (cześć IN, Hemant)"
hr-HR | Chorwacki | Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (hr-HR Matej)"
hu-HU | Węgierski | Mężczyzna | "Microsoft Server mowy Text na głos mowy (hu-HU, Szabolcs)"
id-ID | Indonezyjski| Mężczyzna | "Microsoft Server mowy Text na głos mowy (identyfikator ID, Andika)"
IT-IT | Włoski |Mężczyzna | "Microsoft Server mowy Text na głos mowy (it-IT, Cosimo, Apollo)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (it-IT, LuciaRUS)"
ja-JP | Japoński |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (ja-JP, Ayumi, Apollo)"
| | |Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (ja-JP, Ichiro, Apollo)"
| | |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (ja-JP, HarukaRUS)"
ko-KR | Koreański |Kobieta | "Microsoft Server mowy Text na głos mowy (ko-KR, HeamiRUS)"
ms-MY | Malajski | Mężczyzna | "Głosu zamiany tekstu na mowę mowy serwera firmy Microsoft (ms Moje, Rizwan)"
nb-NO | Norweski | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosu (nb — bez HuldaRUS)"
NL-NL | Holenderski | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (nl-NL, HannaRUS)"
pl-PL | Polski | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (pl-PL, PaulinaRUS)"
pt-BR | Portugalski (Brazylia) | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (pt-BR, HeloisaRUS)"
| | |Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (pt-BR, Daniel, Apollo)"
pt-PT | Portugalski (Portugalia) | Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (pt-PT, HeliaRUS)"
RO RO | Rumuński | Mężczyzna | "Microsoft Server mowy Text na głos mowy (ro-RO Andrei)"
ru-RU |Rosyjski| Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (ru-RU, Irina, Apollo)"
| | |Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (Apollo Pavel, ru-RU)"
| | |Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (ru-RU, EkaterinaRUS)"
sk-SK | Słowacki|Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (sk-SK Filip)"
sl SI | Słoweński|Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (sl-SI Lado)"
sv-SE | Szwedzki|Kobieta | "Microsoft Server mowy Text na głos mowy (sv-SE, HedvigRUS)"
Ta w | Tamilski (Indie) |Mężczyzna | "Microsoft Server mowy Text na głos mowy (ta-IN, Valluvar)"
Twórz w | Telugu (Indie) |Kobieta | "Microsoft Server mowy Text na głos mowy (t IN Chitra)"
th TH | Tajlandzki|Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosowych (th-TH Pattara)"
tr-TR |Turecki| Kobieta | "Microsoft Server mowy zamiany tekstu na mowę głosowych (tr-TR, SedaRUS)"
vi-VN | Wietnamski|Mężczyzna | "Microsoft Server mowy zamiany tekstu na mowę głosu (vi-VN)"
zh-CN | Chiński (kontynent)|Kobieta | "Microsoft Server mowy Text na głos mowy (zh-CN, HuihuiRUS)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (zh-CN, Yaoyao, Apollo)"
| | |Mężczyzna | "Microsoft Server mowy Text na głos mowy (zh-CN, Kangkang, Apollo)"
zh-HK | Chiński (SRA Hongkong)|Kobieta | "Microsoft Server mowy Text na głos mowy (zh-HK Tracy, Apollo)"
| | |Kobieta | "Microsoft Server mowy Text na głos mowy (zh-HK TracyRUS)"
| || Mężczyzna | "Microsoft Server mowy Text na głos mowy (zh-HK Danny, Apollo)"
zh-TW | Chiński (Tajwan)|Kobieta | "Microsoft Server mowy Text na głos mowy (zh-TW, Yating, Apollo)"
| || Kobieta | "Microsoft Server mowy Text na głos mowy (zh-TW HanHanRUS)"
| || Mężczyzna | "Microsoft Server mowy Text na głos mowy (zh-TW, Zhiwei, Apollo)"

\* *ar np obsługuje nowoczesnych standardowa arabski (MSA).*

### <a name="customization"></a>Dostosowywanie

Dostosowanie głos jest dostępna dla US English (en US), kontynent, chiński (zh-CN) i włoski (it-IT).

> [!NOTE]
> Szkolenia włoskojęzycznego rozpoczyna się od zestawu danych z ponad 2000 wypowiedzi. Chiński — angielski dwujęzyczny modele również są obsługiwane za pomocą ponad 2000 wypowiedzi początkowego zestawu danych.

## <a name="speech-translation"></a>Tłumaczenie mowy

**Tłumaczenia mowy** API obsługę innych języków tłumaczenia mowy do rozpoznawania mowy i rozpoznawania mowy na tekst. Język źródłowy zawsze musi być z poniższej tabeli języka mowy. Dostępne języki docelowej zależą od tego, czy element docelowy tłumaczenia jest mowy lub tekstu.

### <a name="speech-languages"></a>Języki mowy

| Język mowy   | Kod języka |
|:----------- |-|
| Arabski (nowoczesnych standardowy)      | `ar` |
| Chiński (mandaryński)      | `zh` |
| Polski      | `en` |
| Francuski      | `fr` |
| Niemiecki      | `de` |
| Włoski      | `it` |
| Japoński      | `jp` |
| Portugalski (Brazylia)     | `pt` |
| Rosyjski      | `ru` |
| Hiszpański      |  `es` |

### <a name="text-languages"></a>Tekstu/wszystkie języki

| Język tekstu    | Kod języka |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabski       | `ar`          |
| Bengalski      | `bn`          |
| Bośniacki (łaciński)      | `bs`          |
| Bułgarski      | `bg`          |
| Kantoński (tradycyjny)      | `yue`          |
| Kataloński      | `ca`          |
| Chiński uproszczony      | `zh-Hans`          |
| Chiński tradycyjny      | `zh-Hant`          |
| Chorwacki      | `hr`          |
| Czeski      | `cs`          |
| Duński      | `da`          |
| Holenderski      | `nl`          |
| Polski      | `en`          |
| Estoński      | `et`          |
| Fidżi      | `fj`          |
| Filipino      | `fil`          |
| Fiński      | `fi`          |
| Francuski      | `fr`          |
| Niemiecki      | `de`          |
| Grecki      | `el`          |
| Haitański      | `ht`          |
| Hebrajski      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Węgierski      | `hu`          |
| Indonezyjski      | `id`          |
| Włoski      | `it`          |
| Japoński      | `ja`          |
| Swahili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Koreański      | `ko`          |
| Łotewski      | `lv`          |
| Litewski      | `lt`          |
| Malgaski      | `mg`          |
| Malajski      | `ms`          |
| Maltański      | `mt`          |
| Norweski      | `nb`          |
| Perski      | `fa`          |
| Polski      | `pl`          |
| Portugalski      | `pt`          |
| Queretaro Otomi      | `otq`          |
| Rumuński      | `ro`          |
| Rosyjski      | `ru`          |
| (Samoa Zachodnie)      | `sm`          |
| Serbski (cyrylica)      | `sr-Cyrl`          |
| Serbski (łaciński)      | `sr-Latn`          |
| Słowacki     | `sk`          |
| Słoweński      | `sl`          |
| Hiszpański      | `es`          |
| Szwedzki      | `sv`          |
| Tahitian      | `ty`          |
| Tamilski      | `ta`          |
| Tajlandzki      | `th`          |
| Pa'anga      | `to`          |
| Turecki      | `tr`          |
| Ukraiński      | `uk`          |
| Urdu      | `ur`          |
| Wietnamski      | `vi`          |
| Walijski      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Kolejne kroki

* [Pobierz subskrypcję usługi mowy w wersji próbnej](https://azure.microsoft.com/try/cognitive-services/)
* [Zobacz, jak rozpoznawanie mowy w języku C#](quickstart-csharp-dotnet-windows.md)
