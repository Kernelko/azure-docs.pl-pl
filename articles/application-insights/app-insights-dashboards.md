---
title: Pulpity nawigacyjne i nawigacja w usłudze Azure Application Insights | Dokumentacja firmy Microsoft
description: Utwórz widoki kluczowe wykresy APM i zapytań.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 8161fda80b5fa498f9321371c9ad2c8a2d97441a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962423"
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Nawigacja i pulpity nawigacyjne w portalu Application Insights
Po utworzeniu [Konfigurowanie usługi Application Insights w projekcie](app-insights-overview.md), dane telemetryczne dotyczące użycia i wydajności aplikacji będą wyświetlane w zasobie usługi Application Insights projektu w [witryny Azure portal](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Znajdź telemetrii
Zaloguj się do [witryny Azure portal](https://portal.azure.com) i przejdź do zasobu usługi Application Insights, który został utworzony dla aplikacji.

![Kliknij przycisk Przeglądaj, wybierz usługę Application Insights, a następnie aplikację.](./media/app-insights-dashboards/00-start.png)

Blok przeglądu (strona) dla aplikacji zawiera podsumowanie informacji o kluczowych metrykach diagnostycznych aplikacji i jest bramą do innych funkcji w portalu.

![Główne trasy, aby wyświetlić dane telemetryczne](./media/app-insights-dashboards/010-oview.png)

Można dostosowywać każdy siatki i wykresy i przypiąć je do pulpitu nawigacyjnego. W ten sposób możesz zebrać kluczowe dane telemetryczne z różnych aplikacji na centralnym pulpicie nawigacyjnym.

## <a name="dashboards"></a>Pulpity nawigacyjne
Pierwszą rzeczą, zostanie wyświetlony po zalogowaniu się do [portalu Microsoft Azure](https://portal.azure.com) jest pulpit nawigacyjny. W tym miejscu możesz zebrać wykresów, które są dla Ciebie najważniejsze we wszystkich zasobach platformy Azure, w tym dane telemetryczne z [usługi Azure Application Insights](app-insights-overview.md).

![Dostosowanego pulpitu nawigacyjnego.](./media/app-insights-dashboards/31.png)

1. **Przejdź do określonych zasobów** np. aplikacji w usłudze Application Insights: Użyj pasku po lewej stronie.
2. **Wróć do bieżącego pulpitu nawigacyjnego**, lub przełącz się z innymi widokami ostatnie: Użyj menu rozwijane w lewym górnym rogu.
3. **Przełącz pulpity nawigacyjne**: używając menu rozwijanego tytuł pulpitu nawigacyjnego
4. **Tworzenie, edytowanie i udostępnianie pulpitów nawigacyjnych** na pasku nawigacyjnym.
5. **Edytuj pulpit nawigacyjny**: Umieść kursor na kafelku, a następnie użyć jej w górnym pasku do przenoszenia, dostosowywania lub usuń go.

## <a name="add-to-a-dashboard"></a>Dodaj do pulpitu nawigacyjnego
Podczas wyszukiwania w bloku lub zestaw wykresy, który jest szczególnie interesujące, możesz przypiąć jej kopię do pulpitu nawigacyjnego. Zobaczysz go następnym razem zwracanych przez.

![Aby przypiąć wykres, umieść kursor nad nim, a następnie kliknij przycisk "..." w nagłówku.](./media/app-insights-dashboards/33.png)

1. Przypnij wykres do pulpitu nawigacyjnego. Kopiuj wykresu pojawi się na pulpicie nawigacyjnym.
2. Przypnij cały blok do pulpitu nawigacyjnego — będzie ono wyświetlane na pulpicie nawigacyjnym jako Kafelek, który można kliknąć, za pośrednictwem.
3. Kliknij lewym górnym rogu, aby powrócić do bieżącego pulpitu nawigacyjnego. Następnie można użyć menu rozwijanego, aby powrócić do bieżącego widoku.

Należy zauważyć, że wykresy są pogrupowane na fragmenty: kafelka może zawierać więcej niż jednego wykresu. Możesz przypiąć całą kafelka do pulpitu nawigacyjnego.

Wykres są automatycznie odświeżane z częstotliwością zależną od zakresu czasu wykresu:

* Zakres czasu do 1 godziny: odświeżania co 5 minut
* Zakres czasu 1-24 godzin: odświeżanie co 15 minut
* Zakres powyżej 24 godzin czasu: (zakres czasu) / 60.

### <a name="pin-any-query-in-analytics"></a>Przypnij wszystkie zapytania w usłudze Analytics
Możesz również [przypiąć Analytics](../log-analytics/query-language/get-started-analytics-portal.md) wykresy do [udostępnionego](#share-dashboards-with-your-team) pulpitu nawigacyjnego. Dzięki temu można dodać wykresy dowolnego dowolnego zapytania równolegle standardowych metryk. 

Wyniki są automatycznie ponownie obliczane co godzinę. Kliknij ikonę odświeżania na wykresie, aby ponownie obliczyć natychmiast. (Odśwież w przeglądarce nie Oblicz ponownie.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Dostosuj kafelka na pulpicie nawigacyjnym
Po zakończeniu kafelka na pulpicie nawigacyjnym, możesz je dostosować.

![Umieść kursor nad wykres, aby go edytować.](./media/app-insights-dashboards/36.png)

1. Dodawanie wykresu do kafelka.
2. Ustaw metryki i grupowania według wymiarów stylu (tabeli, wykres) wykresu.
3. Przeciągnij kursor nad diagramem powiększania. Kliknij przycisk Cofnij, aby zresetować timespan; Ustawianie właściwości filtru dla wykresów na kafelku.
4. Ustaw tytuł kafelka.

Kafelki przypięte z bloków Eksplorator metryk ma więcej opcji edytowania niż kafelków przypiętych z poziomu bloku Przegląd.

Zmiany nie ma wpływu na oryginalny Kafelek, który został przypięty.

## <a name="switch-between-dashboards"></a>Przełącz między pulpitami nawigacyjnymi
Można zapisać więcej niż jeden pulpit nawigacyjny i przełączać się między nimi. Po przypięciu wykres lub bloku, są one dodawane do bieżącego pulpitu nawigacyjnego.

![Aby przełączyć się między pulpitami nawigacyjnymi, kliknij pulpit nawigacyjny, a następnie wybierz zapisanych pulpit nawigacyjny. Aby utworzyć i zapisać nowy pulpit nawigacyjny, kliknij przycisk Nowa. Aby zmienić kolejność, kliknij przycisk Edytuj.](./media/app-insights-dashboards/32.png)

Na przykład może mieć jeden pulpit nawigacyjny do wyświetlania pełnoekranowego w pokoju zespołu i inny wpis dla ogólne ustawienia projektowania.

Na pulpicie nawigacyjnym zostanie wyświetlony blok, jako Kafelek: kliknij go, aby przejść do bloku. Wykres replikuje wykresu w jej oryginalnej lokalizacji.

![Kliknij Kafelek, aby otworzyć blok, który reprezentuje](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Udostępnianie pulpitów nawigacyjnych
Po utworzeniu pulpitu nawigacyjnego, możesz go udostępnić innym użytkownikom.

![W nagłówku pulpitu nawigacyjnego kliknij udział](./media/app-insights-dashboards/41.png)

Dowiedz się więcej o [role i kontrola dostępu](app-insights-resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Programowe tworzenie pulpitów nawigacyjnych
Można zautomatyzować tworzenie pulpitu nawigacyjnego za pomocą [usługi Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) i prostego edytora JSON.

## <a name="app-navigation"></a>Aplikacja nawigacji
Blok przeglądu jest bramy Aby dowiedzieć się więcej o aplikacji.

* **Dowolnego wykresu lub kafelka** — kliknij dowolny Kafelek lub wykres, aby wyświetlić więcej szczegółów na temat zostanie wyświetlony.

### <a name="overview-blade-buttons"></a>Przyciski bloku przeglądu
![Omówienie bloku górnym pasku nawigacyjnym](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Eksplorator metryk** ](app-insights-metrics-explorer.md) — Utwórz wykresy wydajności i użycia.
* [**Wyszukiwanie** ](app-insights-diagnostic-search.md) — zbadać konkretne wystąpienia zdarzenia, takie jak żądania, wyjątki, lub ślady dzienników.
* [**Analiza** ](app-insights-analytics.md) -zaawansowanych zapytań w ramach telemetrii.
* **Zakres czasu** — Dostosuj zakres wyświetlanych przez wszystkich wykresach w bloku.
* **Usuń** — Usuwanie zasobu usługi Application Insights dla tej aplikacji. Należy również albo usunąć pakiety usługi Application Insights w kodzie aplikacji, lub edytować [klucz Instrumentacji](app-insights-create-new-resource.md#copy-the-instrumentation-key) w swojej aplikacji, aby kierować dane telemetryczne do innego zasobu usługi Application Insights.

### <a name="essentials-tab"></a>Karcie danych podstawowych
* [Klucz Instrumentacji](app-insights-create-new-resource.md#copy-the-instrumentation-key) — identyfikuje ten zasób aplikacji.

### <a name="app-navigation-bar"></a>Pasek nawigacyjny aplikacji
![Lewy pasek nawigacyjny](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Omówienie** -powrócić do bloku przeglądu aplikacji.
* **Dziennik aktywności** — alerty i zdarzenia administracyjne platformy Azure.
* [**Kontrola dostępu** ](app-insights-resources-roles-access-control.md) — zapewniają dostęp do członków zespołu i innym osobom.
* [**Tagi** ](../azure-resource-manager/resource-group-using-tags.md) -grupa aplikacji z innymi osobami za pomocą tagów.

ZBADAJ

* [**Mapa aplikacji** ](app-insights-app-map.md) -Active mapie składniki aplikacji, pochodzące z informacji o zależnościach.
* [**Wykrywanie inteligentne** ](app-insights-proactive-diagnostics.md) — ostatnie alerty wydajności można przeglądać.
* [**Live Stream** ](app-insights-live-stream.md) — A ustalony zestaw metryk niemal natychmiastowe przydatne podczas wdrażania nowej kompilacji lub debugowania.
* [**Dostępność / testy sieci Web** ](app-insights-monitor-web-app-availability.md) -wysyłania żądań regularne do aplikacji sieci web z całym world.*
* [**Błędów, wydajności** ](app-insights-web-monitor-performance.md) — wyjątki, częstotliwości awarii i czasów odpowiedzi dla żądań kierowanych do aplikacji i dla żądań z aplikacji, aby [zależności](app-insights-asp-net-dependencies.md).
* [**Wydajność** ](app-insights-web-monitor-performance.md) — czas reakcji, czasy reakcji zależności.
* [Serwery](app-insights-web-monitor-performance.md) -liczniki wydajności. Jeśli dostępne możesz [Zainstaluj Monitor stanu](app-insights-monitor-performance-live-website-now.md).
* **Przeglądarka** — strony widoku i wydajności AJAX. Jeśli dostępne możesz [Instrumentacja stron sieci web](app-insights-javascript.md).
* **Użycie** -stronie liczby widoku, użytkowników i sesji. Jeśli dostępne możesz [Instrumentacja stron sieci web](app-insights-javascript.md).

KONFIGURUJ

* **Wprowadzenie do** — samouczek wbudowanego.
* **Właściwości** -klucz instrumentacji, subskrypcji i identyfikator zasobu.
* [Alerty](app-insights-alerts.md) — Konfigurowanie alertu metryki.
* [Eksport ciągły](app-insights-export-telemetry.md) — skonfiguruj Eksport danych telemetrycznych do usługi Azure storage.
* [Testowanie wydajności](app-insights-monitor-web-app-availability.md#performance-tests) — Konfigurowanie syntetycznego obciążenia w witrynie sieci Web.
* [Przydział i cennik](app-insights-pricing.md) i [próbkowanie fragmentaryczne](app-insights-sampling.md).
* **Dostęp do interfejsu API** — tworzenie [adnotacje dotyczące wersji](app-insights-annotations.md) i interfejsu API usługi Data Access.
* [**Elementy robocze** ](app-insights-diagnostic-search.md#create-work-item) -nawiązać połączenie z pracy, w systemie śledzenia, dzięki czemu można utworzyć błędy podczas sprawdzania danych telemetrycznych.

USTAWIENIA

* [**Blokuje** ](../azure-resource-manager/resource-group-lock-resources.md) — blokowanie zasobów platformy Azure
* [**Skrypt automatyzacji** ](app-insights-powershell.md) — Eksportuj definicję zasobu platformy Azure, dzięki czemu można użyć jako szablon do tworzenia nowych zasobów.


## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Kolejne kroki

|  |  |
| --- | --- |
| [Eksplorator metryk](app-insights-metrics-explorer.md)<br/>Metryki filtrowania i dzielenia |![Przykład wyszukiwania](./media/app-insights-dashboards/64.png) |
| [Wyszukiwanie diagnostyczne](app-insights-diagnostic-search.md)<br/>Znajdowanie i zbadaj zdarzenia, zdarzenia powiązane oraz tworzyć usterki |![Przykład wyszukiwania](./media/app-insights-dashboards/61.png) |
| [Analiza](app-insights-analytics.md)<br/>Zaawansowany język zapytań |![Przykład wyszukiwania](./media/app-insights-dashboards/63.png) |
