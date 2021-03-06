---
title: Rozwiązywanie problemów z usługi Azure Application Insights Profiler | Dokumentacja firmy Microsoft
description: Ta strona zawiera informacje pomagające deweloperom, którzy mają problemy włączania lub za pomocą usługi Application Insights profiler i instrukcje rozwiązywania problemów.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 28de0f8bdcaa730c5beea0c630d4e86e15642809
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142566"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Rozwiązywanie problemów, włączanie i wyświetlanie Application Insights Profiler

## <a id="troubleshooting"></a>Rozwiązywanie ogólnych

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>Profile są przekazywane tylko, jeśli żądań do aplikacji po uruchomieniu profilera
Application Insights Profiler zbiera dane profilera dla dwie minuty, co godzinę lub gdy [ **profilu teraz** ](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) naciśnięciu przycisku **skonfigurować Application Insights Profiler**strony. Ale profilowania dane są przekazywane tylko, gdy może być dołączane do żądania, które wystąpiły podczas był uruchomiony program profilujący. 

Profiler zapisuje komunikaty śledzenia i zdarzenia niestandardowe do zasobu usługi application insights. Można użyć tych zdarzeń, aby zobaczyć, jak działa program profilujący. Jeśli program profilujący powinien być uruchomiony i przechwytywanie śladów, ale nie widać je na stronie wydajność, można sprawdzić, jak działa program profilujący:

1. Wyszukiwanie komunikatów śledzenia i zdarzenia niestandardowe, wysłanych przez program profilujący do zasobu usługi Application Insights. Ten ciąg wyszukiwania umożliwia wyszukiwanie istotnych danych:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Oto przykład z dwóch wyszukiwania z dwóch różnych zasobów sztucznej Inteligencji na poniższym zrzucie ekranu. 
    
    * Po lewej stronie znajduje się z aplikacji, która nie jest wprowadzenie żądań, po uruchomieniu profilera. Zobaczysz komunikat, że przekazywanie zostało anulowane ze względu na Brak działania. 

    * W tym przykładzie po prawej stronie widać, że profiler pracę i wysyłane zdarzenia niestandardowe, po jego wykryciu żądań, które miały miejsce, gdy program profilujący było uruchomione. Jeśli widzisz ServiceProfilerSample niestandardowe zdarzenie, oznacza to, program profilujący jest dołączony śledzenia na żądanie i możesz wyświetlić ślad ze strony wydajność usługi Application Insights.

    * Jeśli nie widzisz żadnych danych telemetrycznych w ogóle, program profilujący nie jest uruchomiona. Może być konieczne odczytu sekcje dotyczące rozwiązywania problemów dla danego typu specyficzne dla aplikacji, w tym dokumencie poniżej.  

     ![Wyszukaj Telemetrię Profiler][profiler-search-telemetry]

1. W przypadku żądań w czasie uruchomienia profilera, upewnij się, że żądania są obsługiwane przez część aplikacji, która ma profiler włączone. Czasami aplikacje składają się z wielu składników, ale Profiler jest dostępna tylko w niektórych przypadkach nie wszystkich składników. Strona Konfigurowanie Application Insights Profiler będzie przedstawiać składniki, które zostały przekazane dane śledzenia.

### <a name="net-core-21-bug"></a>**.Net Core 2.1 usterki** 
Istnieje błąd w agencie programu profiler, który uniemożliwia przekazywanie ślady z aplikacji uruchomionych na platformy ASP.NET Core 2.1. Firma Microsoft pracuje nad poprawką, zostanie on gotowy wkrótce. Poprawka dla tej usterki zostanie wdrożony do końca października.

### <a name="other-things-to-check"></a>**Inne czynności do wykonania:**
* Twoja aplikacja działa na platformie .NET Framework 4.6.
* Jeśli aplikacja sieci web znajduje się aplikacja ASP.NET Core, musi być uruchomiony co najmniej ASP.NET Core 2.0.
* Jeśli dane, które chcesz wyświetlić jest starsza niż kilka tygodni, spróbuj ograniczyć czas filtru i spróbuj ponownie. Ślady są usuwane po upływie siedmiu dni.
* Upewnij się, nie ma serwerów proxy lub Zapora zablokowała dostęp do https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>**Zliczanie w równoległych wątków podwójnej precyzji**

W niektórych przypadkach metryki całkowity czas, w podglądzie stosu jest większa niż czas trwania żądania.

Taka sytuacja może wystąpić, gdy dwa lub więcej wątków są skojarzone z żądaniem i działają równolegle. W takim przypadku czas łączna liczba wątków jest większa niż czas, jaki upłynął. Jeden wątek może być oczekiwanie na inne, należy wykonać. Podgląd próbuje wykryć tę sytuację i pomija postrzegać czas oczekiwania, ale jej błędami boku wyświetlania zbyt dużej ilości informacji, a nie pominięto, co może być krytyczne informacje.

Wyświetlenie równoległych wątków w Twoje ślady określają, które wątki oczekują, dzięki czemu można ustalić ścieżkę krytyczną dla żądania. W większości przypadków wątek, który szybko przechodzi w stan oczekiwania, po prostu oczekuje na inne wątki. Koncentrować się na inne wątki i Ignoruj czasu wątków oczekujących.

### <a name="error-report-in-the-profiling-viewer"></a>**Raport o błędach w podglądzie profilowania**
Prześlij zgłoszenie do pomocy technicznej w portalu. Pamiętaj uwzględniać identyfikator korelacji z komunikatu o błędzie.

## <a name="troubleshooting-profiler-on-app-services"></a>Rozwiązywanie problemów z Profiler od aplikacji usług
### <a name="for-the-profiler-to-work-properly"></a>**Programu Profiler zapewnić prawidłowe działanie:**
* Twój plan usługi sieci web app service musi być w warstwie podstawowa lub wyższej.
* Twoja aplikacja sieci web musi mieć rozszerzenie usługi Application Insights dla usług App Service (2.6.5) zainstalowane.
* Twoja aplikacja internetowa musi odebrać **APPINSIGHTS_INSTRUMENTATIONKEY** ustawienia aplikacji skonfigurowano ten sam klucz instrumentacji, który jest używany przez zestaw SDK usługi Application Insights.
* Twoja aplikacja internetowa musi odebrać **APPINSIGHTS_PROFILERFEATURE_VERSION** ustawienia aplikacji zdefiniowane i ustawić 1.0.0.
* **ApplicationInsightsProfiler2** zadanie sieci web musi być uruchomione. Zadanie sieci web można sprawdzić, przechodząc do [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/)i otwieranie **pulpitu nawigacyjnego zadań Webjob** w menu Narzędzia. Jak widać na zrzutach ekranu poniżej, klikając łącze ApplicationInsightsProfiler2, możesz zobaczyć szczegóły zadania webjob, w tym dzienniku.

    Oto link, który należy kliknąć przycisk, aby wyświetlić szczegóły zadania webjob: 

    ![zadania webjob profilera]

    Oto strona która zawiera szczegółowe informacje. Możesz pobrać dziennik i wysyłać je do naszego zespołu, jeśli nie można ustalić dlaczego program profilujący nie działa dla Ciebie.
    
    ![Program profilujący zadania webjob dziennika]

### <a name="manual-installation"></a>**Instalacja ręczna**

Po skonfigurowaniu Profiler, aktualizacje są wprowadzane do ustawień aplikacji sieci web. Możesz ręcznie zastosować aktualizacje, jeśli wymaga środowiska. Przykładem może być, że aplikacja jest uruchomiona w środowisku aplikacji sieci Web w usłudze PowerApps.

1. W **kontroli aplikacji sieci Web** otwartym okienkiem **ustawienia**.
1. Ustaw **.Net Framework w wersji** do **wersje 4.6**.
1. Ustaw **zawsze włączone** do **na**.
1. Dodaj **APPINSIGHTS_INSTRUMENTATIONKEY** aplikacji ustawienia, a następnie ustaw wartość na ten sam klucz instrumentacji, który jest używany przez zestaw SDK.
1. Dodaj **APPINSIGHTS_PROFILERFEATURE_VERSION** ustawienia aplikacji, a następnie ustaw wartość 1.0.0.
1. Otwórz **zaawansowane narzędzia**.
1. Wybierz **Przejdź** otworzyć Kudu witryny sieci Web.
1. W witrynie sieci Web programu Kudu, wybierz **rozszerzeń witryny**.
1. Zainstaluj **usługi Application Insights** w galerii aplikacji sieci Web platformy Azure.
1. Uruchom ponownie aplikację internetową.

### <a name="too-many-active-profiling-sessions"></a>**Zbyt wiele aktywnych sesji profilowania**

Obecnie można włączyć Profiler na maksymalnie cztery Azure web apps i miejsc wdrożenia, które działają w ramach jednego planu usług. Jeśli zadanie sieci web Profiler zgłoszenie zbyt wiele aktywnych sesji profilowania, przenieść niektórych aplikacji sieci web do planu w innej usługi.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Błąd wdrożenia: katalog nie jest pusty "D:\\macierzystego\\witryny\\wwwroot\\App_Data\\zadania

W przypadku ponownego wdrażania aplikacji sieci web do zasobu aplikacji sieci Web za pomocą Profiler włączone, może zostać wyświetlony następujący komunikat:

*Katalog nie jest pusty "D:\\macierzystego\\witryny\\wwwroot\\App_Data\\zadania*

Ten błąd występuje, jeśli należy uruchomić narzędzie Web Deploy ze skryptów lub w potoku wdrożenia metodyki DevOps platformy Azure. Rozwiązanie to można dodać następujące parametry dodatkowe wdrożenia do zadań narzędzia Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Te parametry, usuń folder, który jest używany przez program Application Insights Profiler i odblokować proces ponownego wdrażania. Nie wpływają one na wystąpienie Profiler, które jest aktualnie uruchomione.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Jak ustalić, czy Application Insights Profiler działa?

Profiler jest uruchamiany jako zadanie web ciągły w aplikacji sieci web. Możesz otworzyć zasób aplikacji sieci web w [witryny Azure portal](https://portal.azure.com). W **WebJobs** okienko, sprawdź stan **ApplicationInsightsProfiler**. Jeśli nie jest uruchomiony, otwórz **dzienniki** Aby uzyskać więcej informacji.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Rozwiązywanie problemów z Profiler i WAD

Istnieją trzy czynności, aby sprawdzić, czy program profilujący jest poprawnie skonfigurowana za pomocą funkcji WAD. Najpierw należy sprawdzić, czy zawartość konfiguracji WAD, które są wdrażane są, czego oczekiwać. Po drugie należy sprawdzić, czy WAD przekazuje właściwego klucza iKey w wierszu polecenia programu profilującego. Po trzecie można sprawdzić plik dziennika profilera, aby zobaczyć, jeśli program profilujący został uruchomiony, ale występują błąd. 

Aby sprawdzić ustawienia, które były używane do konfigurowania funkcji WAD, musisz zalogować się do maszyny Wirtualnej, a następnie otwórz plik dziennika w następującej lokalizacji: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
W tym pliku możesz wyszukać ciąg "WadCfg" i można znaleźć ustawienia, które zostały przekazane do maszyny Wirtualnej, aby skonfigurować narzędzie diagnostyczne. Można sprawdzić, czy ten klucz instrumentacji, używane przez obiekt sink profiler jest prawidłowa.

Po drugie można sprawdzić wiersza polecenia, które jest używane do uruchamiania profilera. Następujący plik zawiera argumenty używane do uruchomienia profilera.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Sprawdź, czy klucz Instrumentacji w wierszu polecenia profiler jest poprawna. 

Po trzecie przy użyciu ścieżki znajdującą się w pliku config.json powyżej, sprawdź plik dziennika profilera. Informacje o debugowaniu, wskazujący stan i komunikaty o błędach od profilera i ustawień, których używa program profilujący będzie wyświetlany. Jeśli program profilujący jest uruchomiona, gdy aplikacja odbiera żądania, zostanie wyświetlony ten komunikat: działanie wykryte z klucza iKey. Po przekazaniu śledzenia zobaczysz tego komunikatu: Rozpocznij przekazywanie śledzenia. 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[zadania webjob profilera]:./media/app-insights-profiler/Profiler-webjob.png
[Program profilujący zadania webjob dziennika]:./media/app-insights-profiler/Profiler-webjob-log.png








