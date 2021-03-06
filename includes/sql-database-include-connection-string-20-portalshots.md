---
title: Uzyskaj parametry połączenia w witrynie Azure portal
description: Uzyskaj parametry połączenia w witrynie Azure portal
keywords: połączenie z SQL, ciąg połączenia
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: dab7623c86bea4e562313e618f238b9b33c0fdc5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117294"
---
### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Uzyskaj parametry połączenia w witrynie Azure portal
Użyj [witryny Azure portal](https://portal.azure.com/) uzyskać parametry połączenia, który jest konieczny, aby program kliencki do interakcji z usługą Azure SQL Database.

1. Wybierz **wszystkich usług** > **baz danych SQL**.

2. Wprowadź nazwę bazy danych w polu tekstowym filtru w pobliżu lewego górnego rogu **baz danych SQL** bloku.

3. Wybierz wiersz dla bazy danych.

4. Po bloku pojawia się dla bazy danych, wybierz pozycję visual wygody **Minimalizuj** przycisków, aby zwinąć bloki umożliwiający przeglądanie i filtrowanie bazy danych.

5. W bloku bazy danych, wybierz **Pokaż parametry połączenia bazy danych**.

6. Skopiuj parametry połączenia. czyli jeśli zamierzasz używać biblioteki połączeń ADO.NET, skopiuj odpowiedni ciąg z **ADO.NET** kartę.

    ![Skopiuj parametry połączenia ADO bazy danych][20-CopyAdoConnectionString]

7. Edytuj parametry połączenia, zgodnie z potrzebami. czyli wstawianie hasło parametrów połączenia lub usuń "@&lt;servername&gt;" z nazwy użytkownika, jeśli nazwa użytkownika lub nazwa serwera jest zbyt długa.

8. W jednym formacie lub innym informacje o parametrach połączenia należy wkleić kod klienta programu.

Aby uzyskać więcej informacji, zobacz [parametry połączenia i pliki konfiguracyjne](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
