---
title: Dzienniki usług IIS, w usłudze Azure Log Analytics | Dokumentacja firmy Microsoft
description: Internet Information Services (IIS) aktywności użytkownika są przechowywane w plikach dziennika, które mogą być zbierane przez usługę Log Analytics.  W tym artykule opisano sposób konfigurowania zbierania dzienników usług IIS i szczegóły rekordów, które tworzą w obszarze roboczym usługi Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: bwren
ms.comopnent: ''
ms.openlocfilehash: 7b44c0712c4d88ec0bbb7a94f574c2a12faf3550
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040680"
---
# <a name="iis-logs-in-log-analytics"></a>Dzienniki usług IIS, w usłudze Log Analytics
Internet Information Services (IIS) aktywności użytkownika są przechowywane w plikach dziennika, które mogą być zbierane przez usługę Log Analytics.  

![Dzienniki usług IIS](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurowanie usług IIS dzienników
Log Analytics zbiera wpisy z pliki dziennika utworzone przez usługi IIS, więc musisz [skonfigurowania usług IIS do rejestrowania](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics tylko obsługuje przechowywane w formacie W3C plików dziennika usług IIS i nie obsługuje niestandardowych pól lub Zaawansowane rejestrowanie programu IIS.  
Usługa log Analytics zbiera dzienniki w formacie natywnym NCSA lub IIS.

Skonfiguruj dzienniki usług IIS w usłudze Log Analytics z [danych menu Ustawienia usługi Analiza dzienników](log-analytics-data-sources.md#configuring-data-sources).  Jest wymagana żadna konfiguracja, inne niż wybieranie **pliki dziennika usług IIS w formacie W3C zbieranie**.


## <a name="data-collection"></a>Zbieranie danych
Log Analytics zbiera wpisów dziennika IIS z każdego agenta w każdym razem, gdy dziennik jest zamknięty i nowego jest tworzony. Tę częstotliwość jest kontrolowana przez **harmonogram Przerzucanie pliku dziennika** ustawienia witryny usług IIS, która jest raz dziennie domyślnie. Na przykład, jeśli to ustawienie **godzinowe**, a następnie usługi Log Analytics będzie zbierać dziennika co godzinę.  Jeśli to ustawienie ma **codzienne**, a następnie usługi Log Analytics będzie zbierać dziennika co 24 godziny.


## <a name="iis-log-record-properties"></a>Właściwości rekordu dziennika usług IIS
Rekordy dziennika usług IIS mają typ **W3CIISLog** i mają właściwości podane w poniższej tabeli:

| Właściwość | Opis |
|:--- |:--- |
| Computer (Komputer) |Nazwa komputera, na którym zostały zebrane zdarzenia. |
| Przelew |Adres IP klienta. |
| csMethod |Metoda żądania, takich jak GET lub POST. |
| csReferer |Witryna, że użytkownik, a następnie łącze z do bieżącej lokacji. |
| csUserAgent |Typ przeglądarki klienta. |
| csUserName |Nazwa uwierzytelnionego użytkownika, który uzyskał dostęp do serwera. Użytkownicy anonimowi są wskazywani przez łącznik. |
| csUriStem |Obiekt docelowy żądaniem, takiego jak strony sieci web. |
| csUriQuery |Zapytania, jeśli istnieje, czy klient próbował wykonać. |
| ManagementGroupName |Nazwa grupy zarządzania agentów programu Operations Manager.  Dla innych agentów jest to AOI -\<identyfikator obszaru roboczego\> |
| RemoteIPCountry |Kraj adres IP klienta. |
| RemoteIPLatitude |Szerokość adresu IP klienta. |
| RemoteIPLongitude |Długość geograficzna adresu IP klienta. |
| scStatus |Kod stanu HTTP. |
| scSubStatus |Kod błędu: Substatus. |
| scWin32Status |Kod stanu Windows. |
| sIP |Adres IP serwera sieci web. |
| SourceSystem |Program Operations Manager |
| sPort |Port, na serwerze klient połączony. |
| sSiteName |Nazwa witryny usług IIS. |
| TimeGenerated |Data i godzina zarejestrowania wpis. |
| Właściwość timeTaken |Długość czasu na przetworzenie żądania w milisekundach. |

## <a name="log-searches-with-iis-logs"></a>Wyszukiwanie w dzienniku z dziennikami usług IIS
Poniższa tabela zawiera przykłady różnych zapytań dziennika, które pobierają rekordy dziennika usług IIS.

| Zapytanie | Opis |
|:--- |:--- |
| W3CIISLog |Wszystkie rekordy dziennika usług IIS. |
| W3CIISLog &#124; gdzie scStatus == 500 |Wszystkie rekordy dziennika usług IIS przy użyciu status powrotu 500. |
| W3CIISLog &#124; podsumowania count() przez cIP |Liczba usług IIS wpisy Zaloguj się przy użyciu adresu IP klienta. |
| W3CIISLog &#124; gdzie csHost == "www.contoso.com" &#124; podsumowania count() przez csUriStem |Liczba usług IIS wpisy dziennika przez adres URL dla www.contoso.com hosta. |
| W3CIISLog &#124; Podsumuj sum(csBytes) przez komputer &#124; zająć 500000 |Suma bajtów odebranych przez każdy komputer IIS. |

## <a name="next-steps"></a>Kolejne kroki
* Skonfiguruj usługę Log Analytics do gromadzenia innych [źródeł danych](log-analytics-data-sources.md) do analizy.
* Dowiedz się więcej o [dziennikach](log-analytics-log-searches.md) analizować dane zbierane z innych źródeł danych i rozwiązań.
* Konfigurowanie alertów w usłudze Log Analytics w celu proaktywnego powiadamiania o ważne warunki, które można odnaleźć w dziennikach usług IIS.
