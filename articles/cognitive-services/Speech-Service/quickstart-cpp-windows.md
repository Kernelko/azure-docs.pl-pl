---
title: 'Przewodnik Szybki start: Rozpoznawanie mowy w języku C++ w systemie Windows przy użyciu zestawu SDK usługi Mowa'
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak rozpoznawać mowę w języku C++ na komputerze z systemem Windows przy użyciu zestawu SDK usługi Mowa
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: wolfma
ms.openlocfilehash: ddac12c6414365f8d20c42d324695be9855774dc
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469871"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Przewodnik Szybki start: Rozpoznawanie mowy w języku C++ w systemie Windows przy użyciu zestawu SDK usługi Mowa

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

W tym artykule opisano tworzenie aplikacji konsolowej w języku C++ dla systemu Windows. Użyjesz [zestawu SDK usługi Mowa](speech-sdk.md) z usługi Cognitive Services do transkrypcji na tekst w czasie rzeczywistym mowy z mikrofonu komputera. Aplikacja będzie kompilowana przy użyciu [pakietu NuGet zestawu SDK usługi Mowa](https://aka.ms/csspeech/nuget) i programu Microsoft Visual Studio 2017 (w dowolnej wersji).

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia kroków tego przewodnika Szybki start potrzebujesz klucza subskrypcji usługi Mowa. Możesz go uzyskać bezpłatnie. Aby uzyskać szczegółowe informacje, zobacz temat [Try the Speech service for free](get-started.md) (Wypróbuj bezpłatnie usługę Mowa).

## <a name="create-a-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

1. Uruchom program Visual Studio 2017.

1. Upewnij się, że jest dostępny pakiet roboczy **Programowanie aplikacji klasycznych w języku C++**. Wybierz kolejno pozycje **Narzędzia** > **Pobierz narzędzia i funkcje** na pasku menu programu Visual Studio, aby otworzyć instalator programu Visual Studio. Jeśli ten pakiet roboczy jest już włączony, pomiń ten krok i przejdź do kolejnego. 

    ![Zrzut ekranu karty Pakiety robocze programu Visual Studio](media/sdk/vs-enable-cpp-workload.png)

    W przeciwnym razie zaznacz pole wyboru obok pozycji **Programowanie aplikacji klasycznych w języku C++**. 

1. Upewnij się, że jest dostępny składnik **Menedżer pakietów NuGet**. Przejdź do karty **Poszczególne składniki** w oknie dialogowym instalatora programu Visual Studio i wybierz pozycję **Menedżer pakietów NuGet**, jeśli nie została jeszcze wybrana.

      ![Zrzut ekranu karty Poszczególne składniki programu Visual Studio](media/sdk/vs-enable-nuget-package-manager.png)

1. Jeśli musisz włączyć pakiet roboczy języka C++ lub program NuGet, wybierz pozycję **Modyfikuj** (w prawym dolnym rogu okna dialogowego). Instalowanie nowej funkcji chwilę potrwa. Jeśli obie funkcje zostały już włączone, należy zamknąć okno dialogowe.

1. Utwórz nową aplikację konsolową dla systemu Windows Desktop, korzystając z zestawu narzędzi Visual C++. Najpierw wybierz w menu pozycje **Plik** > **Nowy** > **Projekt**. W oknie dialogowym **Nowy projekt** w lewym okienku rozwiń pozycje **Zainstalowane** > **Visual C++** > **Windows Desktop**. Następnie wybierz pozycję **Aplikacja konsolowa systemu Windows**. Jako nazwę projektu podaj *helloworld*.

    ![Zrzut ekranu przedstawiający okno dialogowe nowego projektu](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Jeśli korzystasz z 64-bitowego systemu Windows, możesz przełączyć platformę kompilacji na `x64` za pomocą menu rozwijanego na pasku narzędzi programu Visual Studio. (64-bitowe wersje systemu Windows mogą obsługiwać aplikacje 32-bitowe, więc nie jest to wymagane.)

    ![Zrzut ekranu paska narzędzi programu Visual Studio z wyróżnioną opcją x64](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.

    ![Zrzut ekranu Eksploratora rozwiązań z wyróżnioną opcją Zarządzaj pakietami NuGet rozwiązania](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. W prawym górnym rogu wybierz w polu **Źródło pakietu** wartość **nuget.org**. Wyszukaj pakiet `Microsoft.CognitiveServices.Speech` i zainstaluj go w projekcie **helloworld**.

    ![Zrzut ekranu okna dialogowego Zarządzaj pakietami dla rozwiązania](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > Bieżąca wersja zestawu SDK usługi Mowa z usługi Cognitive Services to `1.0.1`.

1. Zaakceptuj wyświetloną licencję, aby rozpocząć instalowanie pakietu NuGet.

    ![Zrzut ekranu okna dialogowego Akceptacja licencji](media/sdk/qs-cpp-windows-05-nuget-license.png)

Po zainstalowaniu pakietu w konsoli Menedżera pakietów zostanie wyświetlone potwierdzenie.

## <a name="add-sample-code"></a>Dodawanie przykładowego kodu

1. Otwórz plik źródłowy *helloworld.cpp*. Zastąp cały kod poniżej początkowej instrukcji include (`#include "stdafx.h"` lub `#include "pch.h"`) następującymi instrukcjami:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. W tym samym pliku zastąp ciąg `YourSubscriptionKey` kluczem subskrypcji.

1. Zastąp ciąg `YourServiceRegion` [regionem](regions.md) skojarzonym z subskrypcją (na przykład `westus` w przypadku subskrypcji bezpłatnej wersji próbnej).

1. Zapisz zmiany w projekcie.

## <a name="build-and-run-the-app"></a>Kompilowanie i uruchamianie aplikacji

1. Skompiluj aplikację. Na pasku menu wybierz kolejno pozycje **Kompiluj** > **Kompiluj rozwiązanie**. Kod powinien zostać skompilowany bez błędów.

   ![Zrzut ekranu aplikacji Visual Studio z wyróżnioną opcją Kompiluj rozwiązanie](media/sdk/qs-cpp-windows-06-build.png)

1. Uruchom aplikację. Na pasku menu wybierz kolejno pozycje **Debuguj** > **Rozpocznij debugowanie** lub naciśnij klawisz **F5**.

   ![Zrzut ekranu aplikacji Visual Studio z wyróżnioną opcją Rozpocznij debugowanie](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Zostanie wyświetlone okno konsoli z monitem o wypowiedź ustną. Wypowiedz zwrot lub zdanie w języku angielskim. Wypowiedź zostanie przesłana do usługi Mowa i nastąpi jest transkrypcja na tekst, który zostanie wyświetlony w tym samym oknie.

   ![Zrzut ekranu przedstawiający dane wyjściowe konsoli po pomyślnym ukończeniu rozpoznawania](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Poszukaj tego przykładu w folderze `quickstart/cpp-windows`.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Recognize intents from speech by using the Speech SDK for C++](how-to-recognize-intents-from-speech-cpp.md) (Rozpoznawanie intencji z mowy przy użyciu zestawu SDK usługi Mowa dla języka C++)

## <a name="see-also"></a>Zobacz też

- [Translate speech with the Cognitive Services Speech SDK for C#](how-to-translate-speech-csharp.md) (Tłumaczenie mowy za pomocą zestawu SDK usługi Mowa z usługi Cognitive Services dla języka C#)
- [Samouczek: tworzenie niestandardowego modelu akustycznego](how-to-customize-acoustic-models.md)
- [Samouczek: tworzenie niestandardowego modelu językowego](how-to-customize-language-model.md)
