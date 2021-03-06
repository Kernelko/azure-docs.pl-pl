---
title: 'Samouczek: rozpoznawanie emocji na twarzy na obrazie — interfejs API rozpoznawania emocji, Python'
titlesuffix: Azure Cognitive Services
description: Aby dowiedzieć się, jak używać interfejsu API rozpoznawania emocji z językiem Python, użyj notesu Jupyter. Wyniki wizualizuj przy użyciu popularnych bibliotek.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 31e346cd9a3f43f8181ebee4474ae6c9ee2cc6fc
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237861"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Samouczek: używaj interfejsu API rozpoznawania emocji z notesem Jupyter i językiem Python.

> [!IMPORTANT]
> Interfejs API rozpoznawania emocji zostanie wycofany w dniu 15 lutego 2019 r. Rozpoznawanie emocji jest teraz ogólnie dostępne jako część [interfejsu API rozpoznawania twarzy](https://docs.microsoft.com/azure/cognitive-services/face/). 

Aby rozpoczęcie korzystania z interfejsu API rozpoznawania emocji było łatwiejsze, notes Jupyter, do którego link znajduje się poniżej, pokazuje, jak korzystać z interfejsu API w języku Python oraz jak wizualizować wyniki za pomocą pewnych popularnych bibliotek.

[Link do notesu w usłudze GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Korzystanie z notesu Jupyter

Aby używać notesu interakcyjnie, będzie trzeba go sklonować i uruchomić w programie Jupyter. Aby dowiedzieć się, jak rozpocząć pracę z interaktywnymi notesami Jupyter, postępuj zgodnie z instrukcjami na stronie http://jupyter.readthedocs.org/en/latest/install.html.

Aby użyć tego notesu, konieczny będzie klucz subskrypcji dla interfejsu API rozpoznawania emocji. Odwiedź [stronę subskrypcji](https://azure.microsoft.com/try/cognitive-services/), aby się zarejestrować. Na stronie „Logowanie” użyj swojego konta Microsoft, aby zalogować się. Następnie aktywuj subskrypcję i uzyskaj bezpłatne klucze. Po zakończeniu procesu rejestracji wklej swój klucz w sekcji zmiennych pokazanej poniżej. Działają oba klucze, podstawowy i pomocniczy.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
