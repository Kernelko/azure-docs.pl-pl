---
title: Ze wstępnie utworzonych domen Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Usługa LUIS obejmuje zestaw wstępnie utworzonych domen do szybkiego dodawania scenariuszy użytkowników typowe i konwersacyjny.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: b1c7ced4a934ea5d094e0c54a295870986f09933
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49651878"
---
# <a name="add-prebuilt-domains-for-common-usage-scenarios"></a>Dodaj ze wstępnie utworzonych domen dla typowych scenariuszy użycia 

Usługa LUIS zawiera zestaw wbudowanych intencji ze wstępnie utworzonych domen do szybkiego dodawania typowych intencje i wypowiedzi. Jest to szybki i łatwy sposób dodać możliwości do aplikacji klienckiej konwersacji, bez konieczności projektowania modeli dla tych możliwości. 

## <a name="add-a-prebuilt-domain"></a>Dodawanie wstępnie utworzonej domeny

1. Na **Moje aplikacje** wybierz aplikację. Spowoduje to otwarcie aplikacji **kompilacji** części aplikacji. 

1. Na **intencji** wybierz opcję **Dodaj ze wstępnie utworzonych domen** dolnej, lewej paska narzędzi. 

1. Wybierz **kalendarza** intencji, a następnie wybierz opcję **Dodaj domenę** przycisku.

    ![Dodawanie domeny wbudowanych kalendarza](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Wybierz **intencji** w nawigacji po lewej stronie, aby wyświetlić intencji kalendarza. Każdy intencji z tej domeny jest poprzedzony znakiem `Calendar.`. Wraz z wypowiedzi, dwie jednostki dla tej domeny są dodawane do aplikacji: `Calendar.Location` i `Calendar.Subject`. 

## <a name="train-and-publish"></a>Uczenie i publikowanie

1. Po dodaniu domeny uczenie aplikacji, wybierając **szkolenie** w górnym rogu, kliknij prawym przyciskiem myszy pasek narzędzi. 

1. Na górnym pasku narzędzi wybierz **Publikuj**. Publikowanie **produkcji**. 

1. Gdy pojawi się powiadomienie o powodzeniu zielony, wybierz **można znaleźć na liście punktów końcowych** linku umożliwia wyświetlenie listy punktów końcowych.

1. Wybierz punkt końcowy. Punkt końcowy zostanie otwarta nowa karta przeglądarki. Nie zamykaj kartę przeglądarki i w dalszym ciągu **testu** sekcji.

## <a name="test"></a>Testuj

Test nowe opcje w punkcie końcowym dodany przez wartość **q** parametru: `Schedule a meeting with John Smith in Seattle next week`.

Usługa LUIS zwraca poprawne intencji i przedmiot spotkania:

```JSON
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="next-steps"></a>Kolejne kroki
> [!div class="nextstepaction"]
> [Dokumentacja ze wstępnie utworzonych domen](./luis-reference-prebuilt-domains.md)
