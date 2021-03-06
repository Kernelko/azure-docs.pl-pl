---
title: Użyj bazy danych MySQL jako PaaS w usłudze Azure Stack | Dokumentacja firmy Microsoft
description: Dowiedz się, jak wdrażanie dostawcy zasobów MySQL i podaj baz danych MySQL jako usługi w usłudze Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 635948c28ffe5d5eaece372976e58d26e17214e3
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50084234"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Użyj bazy danych MySQL w usłudze Microsoft Azure Stack

MySQL są wspólne w witrynach sieci Web, a także obsługuje wiele platform witryny sieci Web. Na przykład można utworzyć witryny sieci Web systemu WordPress przy użyciu platformy aplikacji sieci Web jako dodatek usługi (PaaS).

Po wdrożeniu dostawcy zasobów, możesz wykonywać następujące czynności:

* Tworzenie serwerów MySQL i baz danych przy użyciu szablonów wdrażania usługi Azure Resource Manager.
* Podaj baz danych MySQL jako usługi.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Architektura karty dostawcy zasobów MySQL

Dostawca zasobów ma następujące składniki:

* **MySQL zasobów dostawcy karty maszyny wirtualnej (VM)**, który jest maszyną Wirtualną serwera systemu Windows, która działa usług dostawcy.
* **Dostawca zasobów**, która przetwarza żądania i uzyskuje dostęp do bazy danych zasobów.
* **Serwery obsługujące serwer MySQL**, które zapewniają pojemność dla bazy danych, które są nazywane serwerami hostingu. Można utworzyć wystąpienia MySQL, samodzielnie lub zapewniają dostęp do zewnętrznych wystąpień MySQL. [Galerii Szybki Start usługi Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) zawiera przykładowy szablon, który służy do:

  * Tworzy serwer MySQL.
  * Pobrać i wdrożyć serwer MySQL w witrynie Azure Marketplace.

> [!NOTE]
> Hostowania serwerów, które są zainstalowane w usłudze Azure Stack zintegrowanych systemów musi zostać utworzona z subskrypcji dzierżawcy. Nie można ich tworzyć subskrypcji dostawcy domyślnego. Musi zostać utworzona z poziomu portalu dzierżawcy lub w sesji programu PowerShell przy użyciu odpowiedniej logowania. Wszystkie serwery hostingu są płatne maszyn wirtualnych i musi mieć licencje. Administrator usługi może być właścicielem subskrypcji dzierżawcy.

### <a name="required-privileges"></a>Wymagane uprawnienia

Konto system musi mieć następujące uprawnienia:

* **Baza danych:** utworzyć, porzucić
* **Zaloguj się:** tworzenie, ustaw, porzucić, przyznać, odwoływanie  

## <a name="next-steps"></a>Kolejne kroki

[Wdrażanie dostawcy zasobów MySQL](azure-stack-mysql-resource-provider-deploy.md)
