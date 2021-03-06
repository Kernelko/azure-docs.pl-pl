---
title: Konfigurowanie monitorowania alertów dotyczących zadań usługi Azure Stream Analytics
description: W tym artykule opisano jak skonfigurować monitorowanie i alerty dla zadań usługi Azure Stream Analytics za pomocą witryny Azure portal.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: 4c676ab3039a02a4fda27ab00312133e5de8077a
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47090970"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Konfigurowanie alertów dotyczących zadań usługi Azure Stream Analytics
Alerty można skonfigurować, aby wyzwolić alert, gdy Metryka osiągnie warunek, który określisz. Na przykład skonfigurować alert dla warunku, jak pokazano poniżej:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Zasady można skonfigurować pod kątem dotyczące metryk za pośrednictwem portalu lub mogą zostać skonfigurowane [programowo](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) danych dzienników operacji.

## <a name="set-up-alerts-in-the-azure-portal"></a>Konfigurowanie alertów w witrynie Azure portal
1. W witrynie Azure portal Otwórz zadanie usługi Stream Analytics ma zostać utworzony alert w przypadku. 

2. W **zadania** bloku kliknij **monitorowanie** sekcji.  

3. W **metryki** bloku kliknij **Dodaj alert dotyczący** polecenia.

      ![Konfiguracja usługi Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Wprowadź nazwę i opis.

5. Selektorów należy używać do definiowania warunków, w ramach której zostanie wysłany alert.

6. Podaj informacje o gdzie alertu.

      ![Konfigurowanie alertu dla zadania usługi Azure Stream Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Aby uzyskać więcej informacji na temat konfigurowania alertów w witrynie Azure portal, zobacz [otrzymywanie powiadomień o alertach](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Kolejne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-get-started.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

