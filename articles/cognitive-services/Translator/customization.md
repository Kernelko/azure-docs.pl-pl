---
title: Dostosowanie tłumaczenia — interfejs API tekstu usługi Translator
titlesuffix: Azure Cognitive Services
description: Użyj Centrum w usłudze Translator firmy Microsoft, aby utworzyć własny system tłumaczenia maszynowego, korzystając z preferowanych terminologii i stylu.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: d01a5a8a9a60bef315327721b9f55345bc3d1361
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645064"
---
# <a name="customize-your-text-translations"></a>Dostosuj tłumaczenia tekstu

Microsoft niestandardowe w usłudze Translator w wersji zapoznawczej to funkcja usługi Microsoft Translator, która umożliwia użytkownikom dostosowywanie zaawansowane neuronowego tłumaczenia maszynowego Microsoft Translator podczas tłumaczenia tekstu przy użyciu interfejsu API tłumaczenia tekstu (tylko wersja 3).

Tej funkcji można również dostosować tłumaczenia mowy, gdy jest używane z [mowy w usłudze Cognitive Services w wersji zapoznawczej](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Niestandardowe w usłudze Translator

Translator niestandardowe umożliwiają tworzenie tłumaczenie neuronowe systemami, zapoznaj się z terminologią używane we własnych firm i branży. System tłumaczeń dostosowany zostanie następnie integrowanie istniejących aplikacji, przepływów pracy i witryn sieci Web.

### <a name="how-does-it-work"></a>Jak to działa?

Użyj wcześniej przetłumaczone dokumentów (ulotki, stron sieci Web, dokumentacji, itp.), system tłumaczeń, odzwierciedlający Twoje specyficzne dla domeny terminologii i stylu, lepiej niż system ogólnego tłumaczenia kompilacji. Użytkownicy mogą przesłać dokumenty TMX XLIFF, TXT, DOCX i XLSX.  

System akceptuje także danych, który jest równolegle na poziomie dokumentu, ale jeszcze nie jest wyrównany na poziomie pojedynczych zdań. Jeśli użytkownicy mają dostęp do wersji tej samej zawartości, w wielu językach, ale w oddzielnych dokumentów niestandardowe w usłudze Translator będzie można automatycznie dopasowywać zdań między dokumentów.  System umożliwia również jednojęzyczne danych w językach jednego lub obu tych uzupełniają dane równoległe szkolenie i ulepszać tłumaczenia.

Dostosowany system jest dostępna za pośrednictwem regularnych wywołania Microsoft interfejsu API tłumaczenia tekstu przy użyciu parametru kategorii.

Biorąc pod uwagę odpowiedniego typu i ilość danych szkoleniowych nie jest niczym niezwykłym, można oczekiwać zyski od 5 do 10 lub BELEU jeszcze więcej punktów jakości tłumaczenia przy użyciu niestandardowych w usłudze Translator.

Więcej informacji na temat różnych poziomów dostosowywania oparte na dostępnych danych można znaleźć w [podręcznika użytkownika w usłudze Translator niestandardowe](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Centrum usługi Microsoft Translator

Starsze Centrum w usłudze Translator firmy Microsoft może służyć do translacji statystycznego tłumaczenia maszynowego. [Dowiedz się więcej](https://www.microsoft.com/en-us/translator/hub.aspx)

## <a name="custom-translator-versus-hub"></a>Niestandardowe w usłudze Translator w stosunku do Centrum

|   | **Centrum** | **Niestandardowe w usłudze Translator**|
|:-----|:----:|:----:|
|Stan funkcji dostosowywania   | Ogólna dostępność  | Podgląd |
| Interfejs API tłumaczenia tekstu w wersji  | Tylko w wersji 2   | Tylko w wersji 3 |
| Dostosowywanie SMT | Tak   | Nie |
| Dostosowywanie NMT | Nie    | Tak |
| Nowe ujednolicone dostosowywanie usług mowy | Nie    | Tak |
| [Bez śledzenia](http://www.aka.ms/notrace) | Tak   | Tak |

## <a name="collaborative-translations-framework"></a>Framework współpracy tłumaczenia

> [!NOTE]
> Od 1 lutego 2018 r. AddTranslation() i AddTranslationArray() nie są już dostępne do użytku z programem V2.0 interfejs API tekstu usługi Translator. Te metody zakończy się niepowodzeniem i nic nie zostanie zapisany. V3.0 interfejs API tekstu usługi Translator nie obsługuje tych metod.

>Podobne funkcje są dostępne w interfejsie API usługi Translator koncentratora. Zobacz [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger).

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Skonfiguruj system dostosowane języka, przy użyciu niestandardowych w usłudze Translator](http://aka.ms/CustomTranslatorDocs)
