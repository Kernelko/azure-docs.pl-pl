---
title: Integrowanie usługi Azure Automation z kontrolą źródła usługom DevOps platformy Azure
description: Scenariusz przeprowadzi Cię przez skonfigurowanie integracji przy użyciu konta usługi Azure Automation i usługom DevOps platformy Azure kontroli źródła.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: Program Azure powershell, usługi DevOps platformy Azure, kontrolę źródła, automatyzacji
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: b06e315cc12856976dce87791b423d10f002c6df
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801441"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Scenariusz automatyzacji platformy Azure — Integracja kontroli źródła usługi Automation przy użyciu infrastruktury DevOps platformy Azure

> [!NOTE]
> Istnieje nowe środowisko do kontroli źródła. Aby dowiedzieć się więcej na temat nowego środowiska, zobacz [kontroli źródła (wersja zapoznawcza)](source-control-integration.md).

W tym scenariuszu należy projektu DevOps platformy Azure, którego używasz do zarządzania elementami runbook usługi Azure Automation lub konfiguracji DSC pod kontrolą źródła.

W tym artykule opisano sposób integracji DevOps platformy Azure ze środowiskiem usługi Azure Automation, umożliwiając ciągłą integrację się dzieje w przypadku każdego ewidencjonowania.

## <a name="getting-the-scenario"></a>Uzyskiwanie scenariusza

Ten scenariusz składa się z dwóch elementy runbook programu PowerShell, które można importować bezpośrednio z [galerii elementów Runbook](automation-runbook-gallery.md) w witrynie Azure portal lub Pobierz go z [galerii programu PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Elementy Runbook

Element Runbook | Opis|
--------|------------|
Sync-VSTS | Podczas ewidencjonowania odbywa się zaimportować elementy runbook i konfiguracje z kontroli źródła DevOps platformy Azure. Jeśli uruchomiony ręcznie, importuje i publikuje wszystkie elementy runbook i konfiguracje, na konto usługi Automation.| 
Sync-VSTSGit | Importuj elementy runbook i konfiguracje z DevOps platformy Azure w ramach kontroli źródła Git podczas ewidencjonowania odbywa się. Jeśli uruchomiony ręcznie, importuje i publikuje wszystkie elementy runbook i konfiguracje, na konto usługi Automation.|

### <a name="variables"></a>Zmienne

Zmienna | Opis|
-----------|------------|
VSToken | Bezpieczne zasób zmiennej możesz utworzyć, które zawiera token pat DevOps platformy Azure. Możesz dowiedzieć się, jak utworzyć DevOps platformy Azure osobisty token dostępu na [strony uwierzytelniania usługi Azure DevOps](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

## <a name="installing-and-configuring-this-scenario"></a>Instalowanie i konfigurowanie scenariusza

Tworzenie [osobisty token dostępu](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) w DevOps platformy Azure, która umożliwia synchronizowanie elementów runbook lub konfiguracji do konta usługi automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Tworzenie [bezpiecznego zmiennej](automation-variables.md) na koncie usługi automation, aby pomieścić osobisty token dostępu, aby element runbook mógł uwierzytelniać się DevOps platformy Azure i synchronizacji elementy runbook i konfiguracje, na konto usługi Automation. Możesz nazwać ten VSToken.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Zaimportuj element runbook, który przeprowadza synchronizację z elementów runbook lub konfiguracji do konta usługi automation. Możesz użyć [DevOps platformy Azure przykładowego elementu runbook](https://www.powershellgallery.com/packages/Sync-VSTS) lub [DevOps platformy Azure za pomocą narzędzia Git przykładowego elementu runbook](https://www.powershellgallery.com/packages/Sync-VSTSGit) z [galerii programu PowerShell](https://www.powershellgallery.com) zależności od tego, jeśli używasz źródła DevOps platformy Azure formant lub DevOps platformy Azure przy użyciu narzędzia Git i wdrażania do konta usługi automation.

![Galeria programu PowerShell](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Możesz teraz [publikowania](automation-creating-importing-runbook.md#publishing-a-runbook) ten element runbook, aby można było utworzyć element webhook.

![Pulish elementu runbook](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Tworzenie [elementu webhook](automation-webhooks.md) dla tego elementu runbook w usłudze VSTS synchronizacji i wypełnij parametrów, jak pokazano poniżej. Upewnij się, że skopiuj adres url elementu webhook, odpowiednio do potrzeb dla zaczepienia usług w infrastrukturze DevOps platformy Azure. VSAccessTokenVariableName jest nazwą (VSToken), bezpieczne zmiennej, która utworzoną wcześniej do przechowywania osobisty token dostępu.

Integracja z usługą Azure DevOps (synchronizacji VSTS.ps1) przyjmuje następujące parametry:

### <a name="sync-vsts-parameters"></a>Parametry usługi VSTS synchronizacji

Parametr | Opis|
--------|------------|
WebhookData | Zawiera informacje zaewidencjonowania wysyłane z wpięcie usługi DevOps platformy Azure. Ten parametr należy pozostawić pusty.| 
ResourceGroup | Jest to nazwa grupy zasobów, w której znajduje się konto usługi automation.|
AutomationAccountName | Nazwa konta usługi automation, synchronizowanej z DevOps platformy Azure.|
VSFolder | Nazwa folderu w DevOps platformy Azure, w którym istnieje elementów runbook i konfiguracji.|
VSAccount | Nazwa organizacji DevOps platformy Azure.|
VSAccessTokenVariableName | Nazwa zmiennej bezpieczne (VSToken), która przechowuje DevOps platformy Azure osobisty token dostępu.|

![Webhook](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Jeśli używasz DevOps platformy Azure przy użyciu narzędzia GIT (synchronizacji VSTSGit.ps1) potrwa następujące parametry.

Parametr | Opis|
--------|------------|
WebhookData | To będzie zawierać informacje zaewidencjonowania wysyłane z wpięcie usługi DevOps platformy Azure. Ten parametr należy pozostawić pusty.|
ResourceGroup | Ta nazwa grupy zasobów, w której znajduje się konto usługi automation.|
AutomationAccountName | Nazwa konta usługi automation, synchronizowanej z DevOps platformy Azure.|
VSAccount | Nazwa organizacji DevOps platformy Azure.|
VSProject | Nazwa projektu w DevOps platformy Azure, w którym istnieje elementów runbook i konfiguracji.|
GitRepo | Nazwa repozytorium Git.|
GitBranch | Nazwa gałęzi w repozytorium DevOps Git platformy Azure.|
Folder | Nazwa folderu w gałęzi DevOps Git platformy Azure.|
VSAccessTokenVariableName | Nazwa zmiennej bezpieczne (VSToken), która przechowuje DevOps platformy Azure osobisty token dostępu.|

![Element Webhook usługi GIT](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Utwórz wpięcie usługi w DevOps platformy Azure do zaewidencjonowania do folderu, który wywołuje ten element webhook w zaewidencjonowaniu kodu. Wybierz **techniki Web Hooks** jako usługę, aby zintegrować z usługą podczas tworzenia nowej subskrypcji. Temat można znaleźć więcej informacji o punktach wpięcia usług na [dokumentację punktach Wpięcia usług DevOps platformy Azure](https://www.visualstudio.com/docs/marketplace/integrate/service-hooks/get-started).
![Wpięcie usługi](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Teraz powinno być możliwe w celu zaewidencjonowania wszystkich elementów runbook i konfiguracje do DevOps platformy Azure i mają one automatycznie zsynchronizowane na koncie usługi automation.

![Wynik uruchomienia elementu runbook synchronizacji](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Jeśli ten element runbook zostanie uruchomiony ręcznie bez wyzwalane przez DevOps platformy Azure, można pozostawić puste, parametr webhookdata i wykonuje pełną synchronizację z podanym folderze DevOps platformy Azure.

Jeśli chcesz odinstalować scenariusza, należy usunąć wpięcie usługi z DevOps platformy Azure, Usuń element runbook, a zmienna VSToken.
