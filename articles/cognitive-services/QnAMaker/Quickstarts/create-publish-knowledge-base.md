---
title: Tworzenie, uczenie i publikowanie bazy wiedzy — QnA Maker
titleSuffix: Azure Cognitive Services
description: Na podstawie własnej zawartości, takiej jak często zadawane pytania lub podręczniki produktów, możesz utworzyć bazę wiedzy usługi QnA Maker. Baza wiedzy usługi QnA Maker w tym przykładzie jest tworzona na podstawie prostej strony internetowej często zadawanych pytań w celu udzielania odpowiedzi na pytania dotyczące klucza odzyskiwania funkcji BitLocker.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: c57edd897797d4352706283072aa19444948436b
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49644789"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Tworzenie, szkolenie i publikowanie bazy wiedzy usługi QnA Maker

Na podstawie własnej zawartości, takiej jak często zadawane pytania lub podręczniki produktów, możesz utworzyć bazę wiedzy usługi QnA Maker. Baza wiedzy usługi QnA Maker w tym przykładzie jest tworzona na podstawie prostej strony internetowej często zadawanych pytań w celu udzielania odpowiedzi na pytania dotyczące klucza odzyskiwania funkcji BitLocker.

## <a name="prerequisite"></a>Wymagania wstępne

> [!div class="checklist"]
> * Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-qna-maker-knowledge-base"></a>Tworzenie bazy wiedzy usługi QnA Maker

1. Zaloguj się do witryny QnAMaker.ai przy użyciu swoich poświadczeń platformy Azure.

2. W witrynie usługi QnA Maker wybierz pozycję **Utwórz bazę wiedzy**.

   ![Tworzenie nowej bazy wiedzy](../media/qna-maker-create-kb.png)

3. Na stronie **Tworzenie** w kroku 1 wybierz pozycję **Utwórz usługę QnA**. Nastąpi przekierowanie do witryny [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) w celu skonfigurowania usługi QnA Maker w ramach subskrypcji. Jeśli upłynie limit czasu witryny Azure Portal, wybierz w witrynie pozycję **Ponów próbę**. Po nawiązaniu połączenia zostanie wyświetlony pulpit nawigacyjny platformy Azure.

4. Po pomyślnym utworzeniu nowej usługi QnA Maker na platformie Azure wróć do strony qnamaker.ai/create. Wybierz swoją usługę QnA z listy rozwijanej w kroku 2. Jeśli została utworzona nowa usługa QnA, pamiętaj aby odświeżyć stronę.

   ![Wybieranie bazy wiedzy usługi QnA](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. W kroku 3 nadaj bazie wiedzy nazwę **My Sample QnA KB**.

6. Aby dodać zawartość do swojej bazy wiedzy, wybierz trzy rodzaje źródeł danych. W kroku 4, w obszarze **Wypełnij bazę wiedzy** dodaj adres URL [Często zadawane pytania dotyczące odzyskiwania funkcji BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) w polu **adresu URL**.

   ![Wybieranie bazy wiedzy usługi QnA](../media/qnamaker-quickstart-kb/add-datasources.png)

7. W kroku 5 wybierz pozycję **Tworzenie bazy wiedzy**.

8. Gdy baza wiedzy jest tworzona, pojawi się okno podręczne. Proces wyodrębniania zajmuje kilka minut, możesz więc przeczytać stronę HTML i zidentyfikować pytania i odpowiedzi.

9. Po pomyślnym utworzeniu bazy wiedzy zostanie otwarta strona **Baza wiedzy**. Na tej stronie możesz edytować zawartość bazy wiedzy.

10. W prawym górnym rogu wybierz pozycję **Dodaj parę pytań i odpowiedzi**, aby dodać nowy wiersz w sekcji **Redakcja** bazy wiedzy. W obszarze **Pytanie** wprowadź **Hi.** (Cześć). W obszarze **Odpowiedź** wprowadź **Hello. Ask me bitlocker questions.** (Witam. Zadaj mi pytanie dotyczące funkcji BitLocker).

   ![Dodawanie pary pytań i odpowiedzi](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. W prawym górnym rogu wybierz pozycję **Zapisz i przeszkol**, aby zapisać wprowadzone zmiany i przeszkolić model usługi QnA Maker. Zmiany nie są przechowywane, o ile nie zostaną zapisane.

   ![Zapisywanie i szkolenie](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. W prawym górnym rogu wybierz pozycję **Test**, aby przetestować, czy wprowadzone zmiany weszły w życie. Wprowadź w polu **hi there** (Hej), a następnie naciśnij klawisz Enter. Powinna zostać wyświetlona odpowiedź utworzona jako reakcja.

13. Wybierz pozycję **Zbadaj**, aby bardziej szczegółowo sprawdzić odpowiedź. Okno testów służy do testowania Twoich zmian w bazie wiedzy zanim zostaną opublikowane.

   ![Panel testu](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Wybierz ponownie pozycję **Test**, aby zamknąć wyskakujące okienko **Testowanie**.

15. W menu obok pozycji **Edycja** wybierz pozycję **Opublikuj**. Aby potwierdzić, wybierz na stronie pozycję **Opublikuj**.

16. Usługa QnA Maker została teraz pomyślnie opublikowana. Możesz użyć punktu końcowego w swojej aplikacji lub kodu bota.

   ![Publikowanie](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie bazy wiedzy](../How-To/create-knowledge-base.md)
