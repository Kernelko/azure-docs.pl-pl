---
title: Usługa Azure IoT Hub Migrowanie to ustawień diagnostyki | Dokumentacja firmy Microsoft
description: Jak zaktualizować usługi Azure IoT Hub, aby użyć ustawień diagnostyki platformy Azure zamiast operacji monitoring do monitorowania stanu operacji w Centrum IoT w czasie rzeczywistym.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 3cb0f91f3143e6a4828548f3a15678b3814cba17
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154865"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migrowanie usługi IoT Hub z działania monitorowania to ustawień diagnostyki

Klienci korzystający z [monitorowanie operacji](iot-hub-operations-monitoring.md) umożliwia śledzenie stanu operacji w usłudze IoT Hub można migrować na ten przepływ pracy do [ustawień diagnostyki Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), funkcja usługi Azure Monitor. Ustawienia diagnostyczne podać poziom zasobów informacje diagnostyczne dotyczące wielu usług platformy Azure.

Operacje monitorowania funkcji usługi IoT Hub jest przestarzały i zostanie usunięte w przyszłości. Ten artykuł zawiera kroki umożliwiające przeniesienie obciążeń od działań monitorowania to ustawień diagnostyki. Aby uzyskać więcej informacji na temat osi czasu wycofywania, zobacz [Monitoruj rozwiązania Azure IoT za pomocą usługi Azure Monitor i Azure Resource Health](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/).

## <a name="update-iot-hub"></a>Aktualizowanie usługi IoT Hub

Aby zaktualizować Centrum IoT Hub w witrynie Azure portal, najpierw należy włączyć ustawienia diagnostyki, a następnie wyłącz monitorowanie operacji.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Wyłącz monitorowanie operacji

Po przetestowaniu nowego ustawienia diagnostyki w przepływie pracy, można wyłączyć monitorowanie funkcji operacji. 

1. W menu Centrum IoT Hub, wybierz **monitorowanie operacji**.

2. W ramach każdej kategorii monitorowania, wybierz **Brak**.

3. Zapisz operacje monitorowania zmian.

## <a name="update-applications-that-use-operations-monitoring"></a>Aktualizowanie aplikacji, które używają monitorowanie operacji

Schematów na potrzeby monitorowania operacji i ustawień diagnostyki się nieco różnić. Jest ważne, aktualizację aplikacji, które umożliwia monitorowanie dzisiaj operacji mapowania na schemat używany przez ustawienia diagnostyczne. 

Ponadto diagnostyki ustawienia oferty śledzenia dla pięciu nowych kategorii. Po zaktualizowaniu aplikacji dla istniejącego schematu, Dodaj również nowe kategorie:

* Operacje bliźniaczej reprezentacji chmury do urządzenia
* Operacje bliźniaczej reprezentacji urządzenia do chmury
* Zapytania dotyczące bliźniaczych reprezentacji
* Operacje zadań
* Metody bezpośrednie

W strukturach określonego schematu, zobacz [zrozumieć schematu dla ustawień diagnostycznych](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="next-steps"></a>Kolejne kroki

* [Monitorowanie kondycji usługi Azure IoT Hub i szybkie diagnozowanie problemów](iot-hub-monitor-resource-health.md)