---
title: Twórz aplikacje bezserwerowe za pomocą programu Visual Studio | Dokumentacja firmy Microsoft
description: Tworzenie, wdrażanie i zarządzanie nimi swoją pierwszą aplikację bez użycia serwera za pomocą usługi Azure Logic Apps i Azure Functions w programie Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.openlocfilehash: f5555d9a60934529bf8fed6db6a18dd783f46075
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297472"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Utwórz swoją pierwszą aplikację bez użycia serwera za pomocą usługi Azure Logic Apps i Azure Functions — Visual Studio

Możesz szybko tworzyć i wdrażać aplikacje w chmurze przy użyciu narzędzia niewymagające użycia serwera i funkcji na platformie Azure takie jak [usługi Azure Logic Apps](../logic-apps/logic-apps-overview.md) i [usługi Azure Functions](../azure-functions/functions-overview.md). W tym artykule pokazano, jak rozpocząć Kompilowanie aplikacji bez użycia serwera, który korzysta z aplikacji logiki, która wywołuje funkcję platformy Azure w programie Visual Studio. Aby dowiedzieć się więcej o rozwiązaniach nieużywających serwera na platformie Azure, zobacz [platformy Azure bez użycia serwera za pomocą Functions i Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

Tworzenie aplikacji bezserwerowej w programie Visual Studio, potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, [zarejestruj się w celu założenia bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/).

* [Program Visual Studio 2017](https://www.visualstudio.com/vs/) lub Visual Studio 2015 — Community, Professional lub Enterprise

* [Microsoft Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 lub nowszy)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* Narzędzia [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) lub program [Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  Możesz pobrać i zainstalować narzędzia Azure Logic Apps Tools bezpośrednio z poziomu programu Visual Studio Marketplace lub [Dowiedz się, jak zainstalować to rozszerzenie z poziomu programu Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
  Upewnij się, że ponownie program Visual Studio po zakończeniu instalacji. 

* [Podstawowe narzędzia usługi Azure Functions](https://www.npmjs.com/package/azure-functions-core-tools) lokalne debugowanie funkcji

* Dostęp do Internetu podczas korzystania z projektanta aplikacji logiki osadzone w programie Visual Studio

  Projektant wymaga połączenia internetowego do tworzenia zasobów na platformie Azure oraz odczytywania właściwości i danych z łączników w aplikacji logiki. 
  Jeśli na przykład używany jest łącznik usługi Dynamics CRM Online, projektant wyszukuje dostępne właściwości domyślne i niestandardowe w wystąpieniu CRM.

## <a name="create-resource-group-project"></a>Tworzenie projektu grupy zasobów

Aby rozpocząć, Utwórz [projekt grupy zasobów platformy Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) aplikacji bez użycia serwera. Na platformie Azure tworzysz zasoby w grupie zasobów, czyli logicznej kolekcji, używanych w przypadku organizowanie, zarządzanie i wdrażanie zasobów dla całej aplikacji jako pojedynczy zasób. Dla aplikacji bez użycia serwera na platformie Azure grupa zasobów zawiera zasoby dla usługi Azure Functions i Azure Logic Apps. Dowiedz się więcej o [zasobach i grupach zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md).

1. Uruchom program Visual Studio, a następnie zaloguj się przy użyciu konta platformy Azure. 

1. W menu **Plik** wybierz pozycję **Nowy** > **Projekt**. 

   ![Utwórz nowy projekt w programie Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. W obszarze **Zainstalowane** wybierz pozycję **Visual C#** lub **Visual Basic**. Wybierz pozycję **Chmura** > **Grupa zasobów platformy Azure**.

   Jeśli **chmury** kategorii lub **grupy zasobów platformy Azure** projektu nie istnieje, upewnij się, że zainstalowano zestaw Azure SDK dla programu Visual Studio.

1. Nadaj projektowi nazwę i lokalizację, a następnie wybierz **OK**. 

   Program Visual Studio zostanie wyświetlony monit o wybranie szablonu. 
   Można zacząć od pustego, aplikacja logiki lub innego szablonu, ale w tym przykładzie użyto szablonu szybkiego startu platformy Azure w celu skompilowania aplikacji bez użycia serwera, który zawiera aplikację logiki i wywołanie funkcji platformy Azure.

   Aby utworzyć aplikację logiki w programie Visual Studio, wybierz **aplikacji logiki** szablonu. Ten szablon umożliwia utworzenie pustej aplikacji logiki, zostanie otwarty w Projektancie aplikacji logiki bez konieczności wstępnego wdrażania rozwiązania w grupie zasobów platformy Azure.

1. W obszarze **Pokaż szablony z tej lokalizacji**, wybierz opcję **szybkiego startu platformy Azure (github/Azure/azure — szablony szybkiego startu —)**. 

1. W polu wyszukiwania wpisz "aplikacja logiki" jako filtr, wybierz ten szablon szybkiego startu bez użycia serwera i wybierz **OK**: **101-logic-app-and-function-app**

   ![Wybierz szablon szybkiego startu platformy Azure](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Program Visual Studio tworzy i otwiera rozwiązanie dla Twojego projektu grupy zasobów. 
   Szablon szybkiego startu wybrano utworzenie szablonu wdrożenia o nazwie `azuredeploy.json` wewnątrz projekt grupy zasobów. 
   Szablon wdrożenia zawiera definicję to prosta aplikacja logiki wyzwala na żądanie HTTP, który wywołuje funkcję platformy Azure i zwraca wynik w postaci odpowiedź HTTP. 
   
   ![Nowe rozwiązanie niewymagające użycia serwera](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Następnie należy wdrożyć swoje rozwiązanie na platformie Azure, aby można było otworzyć szablon wdrażania i przejrzyj zasoby dla aplikacji bez użycia serwera. 

## <a name="deploy-your-solution"></a>Zacznij wdrażać swoje rozwiązanie

Zanim będzie można otworzyć aplikację logiki przy użyciu projektanta aplikacji logiki w programie Visual Studio, musi mieć grupę zasobów platformy Azure, która została już wdrożona na platformie Azure. Projektant następnie można utworzyć połączenia z zasobami i usługami w aplikacji logiki. W tym celu wdrożenia rozwiązania w programie Visual Studio do witryny Azure portal.

1. W Eksploratorze rozwiązań Otwórz menu skrótów projektu zasobów, a następnie wybierz **Wdróż** > **New**.

   ![Utwórz nowe wdrożenie dla grupy zasobów](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Jeśli jeszcze nie jest wybrana, wybierz swoją subskrypcję platformy Azure i grupę zasobów, do której chcesz wdrożyć. Wybierz **wdrażanie**.

   ![Ustawienia wdrożenia](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Jeśli **Edytuj parametry** pojawi się okno, podaj nazwę zasobu do użycia w aplikacji logiki i aplikacji funkcji platformy Azure we wdrożeniu, a następnie Zapisz ustawienia. Upewnij się, że używasz globalnie unikatową nazwę aplikacji funkcji.

   ![Podaj nazwy dla aplikacji logiki i aplikacji funkcji](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Po uruchomieniu programu Visual Studio wdrożenia w grupie zasobów określonej stan wdrożenia tego rozwiązania jest wyświetlany w programie Visual Studio **dane wyjściowe** okna. 
   Po zakończeniu wdrożenia aplikacja logiki jest aktywna w witrynie Azure portal.

## <a name="edit-logic-app-in-visual-studio"></a>Edytuj aplikację logiki w programie Visual Studio

Teraz, gdy rozwiązanie jest wdrażana do grupy zasobów, należy otworzyć aplikację logiki przy użyciu projektanta aplikacji logiki, aby można było edytować i zmienić aplikację logiki.

1. W Eksploratorze rozwiązań Otwórz `azuredeploy.json` pliku menu skrótów, a następnie wybierz **Otwórz za pomocą projektanta aplikacji logiki**.

   ![Otwórz "azuredeploy.json" w Projektancie aplikacji logiki](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Po **właściwości aplikacji logiki** pojawi się okno i nie jest jeszcze wybrana, w obszarze **subskrypcji**, wybierz swoją subskrypcję platformy Azure. W obszarze **grupy zasobów**, wybierz grupę zasobów i lokalizacji, w którym wdrożono rozwiązanie, a następnie wybierz **OK**.

   ![Właściwości aplikacji logiki](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Po otwarciu Projektant aplikacji logiki możesz kontynuować dodawanie kroków lub zmiana przepływu pracy i zapisać aktualizacje.

   ![Aplikacja logiki otwarty w Projektancie aplikacji logiki](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Tworzenie projektu usługi Azure Functions

Do utworzenia Twojego projektu funkcji i funkcji przy użyciu języka JavaScript, Python, F #, programu PowerShell, usługi Batch lub powłoki Bash, postępuj zgodnie z instrukcjami w artykule [pracy przy użyciu podstawowych narzędzi usługi Azure Functions](../azure-functions/functions-run-local.md). Tworzenie funkcji platformy Azure przy użyciu języka C# w rozwiązaniu, umożliwia biblioteki klas C#, wykonując czynności opisane w artykule [publikowania bibliotekę klas platformy .NET, co aplikacja funkcji](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>Wdrażanie funkcji w programie Visual Studio

Szablon wdrożenia służy do wdrażania usługi Azure functions, które masz w swoim rozwiązaniu z repozytorium Git, który jest określony przez zmienne w `azuredeploy.json` pliku. Jeśli utworzysz i tworzenie projektu usługi Functions w rozwiązaniu można sprawdzić ten projekt do kontroli źródła Git, na przykład, GitHub lub DevOps platformy Azure i następnie zaktualizuj `repo` zmiennej, tak aby ten szablon służy do wdrażania funkcji platformy Azure.

## <a name="manage-logic-apps-and-view-run-history"></a>Zarządzanie aplikacjami logiki i Widok historii uruchamiania

Dla usługi logic apps już wdrożonych na platformie Azure można nadal edytować, zarządzanie, wyświetlanie historii uruchamiania i wyłączyć te aplikacje z programu Visual Studio. 

1. Z **widoku** w programie Visual Studio, otwórz menu **programu Cloud Explorer**. 

1. W obszarze **wszystkie subskrypcje**, wybierz subskrypcję platformy Azure skojarzone z usługą logic apps, aby zarządzać, a następnie wybierz **Zastosuj**.

1. W obszarze **Logic Apps**, wybierz swoją aplikację logiki. Wybierz z menu skrótów dla tej aplikacji, **Otwórz za pomocą edytora aplikacji logiki**. 

Możesz teraz pobrać aplikacji logiki już opublikowane w projekcie grupy zasobów. Więc mimo że być może rozpoczęto aplikacji logiki w witrynie Azure portal, można nadal importować i zarządzanie tej aplikacji w programie Visual Studio. Aby uzyskać więcej informacji, zobacz [Zarządzanie aplikacjami logiki w programie Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Kolejne kroki

* [Twórz bezserwerowe społecznościowego pulpitu nawigacyjnego](logic-apps-scenario-social-serverless.md)
* [Zarządzanie aplikacjami logiki w programie Visual Studio](manage-logic-apps-with-visual-studio.md)
* [Język definicji przepływów pracy aplikacji logiki](logic-apps-workflow-definition-language.md)