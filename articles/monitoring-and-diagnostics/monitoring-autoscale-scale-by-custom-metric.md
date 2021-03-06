---
title: Automatyczne skalowanie na platformie Azure za pomocą metryk niestandardowych
description: Dowiedz się, jak skalować zasób według metryki niestandardowe na platformie Azure.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 9df587d92b9e35db496c787186ff2945db7965ce
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987819"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Rozpoczynanie pracy z usługą automatyczne skalowanie według metryki niestandardowe na platformie Azure
W tym artykule opisano sposób skalowania zasobu przez Metryka niestandardowa w witrynie Azure portal.

Skalowanie automatyczne platformy Azure Monitor ma zastosowanie tylko do [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [usług w chmurze](https://azure.microsoft.com/services/cloud-services/), [App Service — Web Apps](https://azure.microsoft.com/services/app-service/web/), i [usługi API Management](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

# <a name="lets-get-started"></a>Umożliwia rozpoczęcie pracy
W tym artykule założono, że aplikacja sieci web za pomocą usługi application insights skonfigurowane. Jeśli nie masz jeszcze, możesz to zrobić [Konfigurowanie usługi Application Insights dla witryny sieci Web ASP.NET][1]

- Otwórz [witryny Azure portal][2]
- Kliknij ikonę usługi Azure Monitor w okienku nawigacji po lewej stronie.
  ![Uruchamianie usługi Azure Monitor][3]
- Kliknij pozycję Ustawienia skalowania automatycznego, aby wyświetlić wszystkie zasoby, dla których automatycznego skalowania jest stosowana, wraz z ich bieżący stan skalowania automatycznego ![odnajdywanie automatyczne skalowanie w usłudze Azure monitor][4]
- Otwórz blok "Automatyczne skalowanie" w usłudze Azure Monitor, a następnie wybierz zasób, który chcesz skalować
> Uwaga: Poniższe kroki, użyj planu usługi app service skojarzone z aplikacji sieci web usługi app insights skonfigurowane.
- W bloku Ustawienia skalowania dla zasobu należy zauważyć, że bieżąca liczba wystąpień jest 1. Kliknij pozycję "Włączanie automatycznego skalowania".
  ![Ustawienie Skala dla nowej aplikacji sieci web][5]
- Podaj nazwę dla ustawienia skalowania, a następnie kliknij przycisk "Dodaj regułę". Zapoznaj się z opcjami reguły skalowania, które otwiera się jako okienku kontekstowym po prawej stronie. Domyślnie ustawia możliwość skalowania z liczbą wystąpień o 1, jeśli percetage Procesora zasobów przekracza 70%. Zmień źródło metryki u góry na "Application Insights", wybierz zasób usługi app insights na liście rozwijanej "Zasób", a następnie wybierz Metryka niestandardowa oparta na który chcesz skalować.
  ![Skalowanie według metryki niestandardowe][6]
- Podobnie jak powyżej, Dodaj reguły skalowania, który będzie skalowanie w pionie i zmniejszyć wartość licznika skali przez 1, jeśli Metryka niestandardowa jest poniżej wartości progowej.
  ![Skalowanie procesora cpu][7]
- Możesz ustawić limity wystąpień. Na przykład jeśli chcesz skalować między wystąpieniami 2 do 5 w zależności od tego, niestandardowe metryki wahania Ustaw minimalną, aby "2", maksymalna, "5" i "default" do "2"
> Uwaga: W przypadku, gdy występuje problem z odczytaniem metryk zasobów i bieżąca pojemność to pojemność domyślna, następnie aby zapewnić dostępność zasobów, skalowania automatycznego przeprowadzi skalowanie w poziomie do wartości domyślnej. Jeśli bieżąca pojemność jest już wyższa niż pojemność domyślna, automatycznego skalowania nie będą skalowane w.
- Kliknij przycisk "Zapisz"

Gratulacje. Masz teraz pomyślnie utworzono ustawienie automatycznego skalowania skalować aplikację sieci web, w oparciu o metryki niestandardowe.

> Uwaga: Te same kroki mają zastosowanie do Rozpoczynanie pracy z usługą roli usługi zestawu skalowania maszyn wirtualnych lub w chmurze.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
