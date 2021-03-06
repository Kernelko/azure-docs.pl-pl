---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: d7b78519f1e81fed1f74b6f3a2fd2720cd28cce4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843172"
---
## <a name="setting-up-your-web-server-or-project"></a>Konfigurowanie serwera sieci web lub projektu

> Chcesz zamiast tego Pobierz ten przykładowy projekt?
> - [Pobierz pliki projektu](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) dla lokalnego serwera internetowego, takie jak węzeł
>
> lub
> - [Pobieranie projektu programu Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> A następnie przejdź do [kroku konfiguracji](#register-your-application) do skonfigurowania przykładowego kodu przed jej wykonanie.

## <a name="prerequisites"></a>Wymagania wstępne
Serwer sieci web w lokalnych, takich jak [Node.js](https://nodejs.org/en/download/), [platformy .NET Core](https://www.microsoft.com/net/core), lub usług IIS Express integracji z usługą [programu Visual Studio 2017](https://www.visualstudio.com/downloads/) jest wymagany do uruchomienia w tym samouczku.

Instrukcje w tym przewodniku są oparte na technologii Node.js i programu Visual Studio 2017, ale możesz użyć dowolnego środowiska programowania lub serwer sieci Web.

## <a name="create-your-project"></a>Utwórz projekt

> ### <a name="option-1-node-other-web-servers"></a>Opcja 1: Węzeł / inne serwery sieci web
> Upewnij się, że zainstalowano [Node.js](https://nodejs.org/en/download/), następnie wykonaj kroki opisane poniżej:
> - Utwórz folder do hostowania aplikacji.

<p/><!-- -->

> ### <a name="option-2-visual-studio"></a>Opcja 2: Visual Studio
> Jeśli używasz programu Visual Studio i tworzenia nowego projektu, wykonaj poniższe kroki, aby utworzyć nowe rozwiązanie programu Visual Studio:
> 1.    W programie Visual Studio: **Plik > Nowy > Projekt**
> 2.    W obszarze **Visual C# \Web**, wybierz opcję **aplikacji sieci Web platformy ASP.NET (.NET Framework)**
> 3.    Wprowadź nazwę aplikacji i wybierz **OK**
> 4.    W obszarze **Nowa aplikacja internetowa ASP.NET**, wybierz opcję **pusty**


## <a name="create-your-single-page-applications-ui"></a>Tworzenie interfejsu użytkownika aplikacji jednostronicowej
1.  Utwórz `index.html` swoje SPA JavaScript w pliku. Jeśli używasz programu Visual Studio wybierz projekt (folder główny projektu), kliknij prawym przyciskiem myszy i wybierz pozycję: **Dodaj > Nowy element > stronę HTML** i nadaj mu nazwę index.html.

2.  Dodaj następujący kod do strony:
```html
<!DOCTYPE html>
<html>
<head>
        <title>Quickstart for MSAL JS</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
        <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.js"></script>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
</head>
<body>
        <h2>Welcome to MSAL.js Quickstart</h2><br/>
        <h4 id="WelcomeMessage"></h4>
        <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
        <pre id="json"></pre>
        <script>
            //JS code
        </script>
</body>
</html>
```
