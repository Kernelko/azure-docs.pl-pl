---
title: Tworzenie zadań przesyłania strumieniowego platformy Spark z dokładnie — raz zdarzeń przetwarzanie — Azure HDInsight
description: Jak skonfigurować przesyłania strumieniowego platformy Spark do przetwarzania zdarzeń, jeden raz i tylko jeden raz.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/26/2018
ms.openlocfilehash: ae170e90cede26bd6a43fcc10b93fcd7490d838f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618825"
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Tworzenie zadań przesyłania strumieniowego platformy Spark z dokładnie — raz zdarzenia przetwarzania

Aplikacji do przetwarzania Stream wykonać różne podejścia do sposobu obsługi ponownego przetwarzania wiadomości po niektórych awarii w systemie:

* Co najmniej raz: każdy komunikat jest gwarantowane do przetworzenia, ale może są przetwarzane w więcej niż jeden raz.
* Co najwyżej raz: każdy komunikat może lub nie mogą być przetwarzane. Jeśli komunikat jest przetwarzany, jego jest przetwarzany tylko raz.
* Dokładnie jeden raz: każdy komunikat jest gwarantowane do przetworzenia jeden raz i tylko jeden raz.

W tym artykule przedstawiono sposób konfigurowania, przesyłania strumieniowego platformy Spark w celu osiągnięcia dokładnie — po zakończeniu przetwarzania.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Dokładnie-jednokrotnej semantyki z przesyłaniem strumieniowym Spark

Najpierw należy wziąć pod uwagę jak wszystkie punkty awarii systemu, uruchom ponownie po wystąpił problem i jak można uniknąć utraty danych. Aplikacja usługi przesyłania strumieniowego platformy Spark ma:

* Źródło danych wejściowych
* Jeden lub więcej procesów odbiorcy, które pobierają dane ze źródła danych wejściowych
* Zadania, które przetwarzają dane
* Ujścia danych wyjściowych
* Proces sterownika, który zarządza długotrwałe zadania

Dokładnie — po semantyki wymagają, że żadne dane nie są tracone w dowolnym momencie i przetworzenia komunikatu jest ponownego uruchamiania, niezależnie od tego, gdzie występuje błąd.

### <a name="replayable-sources"></a>Przechwytywaniem źródeł

Źródłowy odczytuje zdarzenia z aplikacji Spark Streaming musi być *przechwytywaniem*. Oznacza to, że w przypadku, gdy komunikat został pobrany, ale następnie system nie powiodła się, zanim może być utrwalone lub przetwarzania wiadomości, źródła należy podać tę samą wiadomość ponownie.

Na platformie Azure Azure Event Hubs i Kafka w HDInsight zapewniają przechwytywaniem źródeł. Innym przykładem przechwytywaniem źródła jest systemem odpornej na uszkodzenia plików, takich jak system plików HDFS, obiekty BLOB usługi Azure Storage, lub Azure Data Lake Store, gdzie wszystkie dane są przechowywane w nieskończoność i w dowolnym momencie możesz ponownie może odczytywać dane, które w całości.

### <a name="reliable-receivers"></a>Niezawodne odbiorcy

W przesyłanie strumieniowe Spark, źródeł, takich jak Event Hubs i Kafka mają *niezawodne odbiorniki*, gdzie każdy odbiorca przechowuje informacje o postępach źródło odczytu. Niezawodne odbiorcy utrzymuje jego stan w odpornej na uszkodzenia magazynu, w ramach dozorcy lub punktów kontrolnych przesyłania strumieniowego platformy Spark, zapisywane do systemu plików HDFS. Jeśli takie odbiornik kończy się niepowodzeniem i jest późniejsza ponownym jego możliwe będzie kontynuowanie pracy tam, gdzie ją przerwaliśmy.

### <a name="use-the-write-ahead-log"></a>Korzystanie z dziennika zapisu z wyprzedzeniem

Przesyłanie strumieniowe Spark obsługuje dziennika zapisu z wyprzedzeniem, gdzie każdego odebranego zdarzenia jest najpierw zapisywane w katalogu punktu kontrolnego platforma Spark w odpornej na uszkodzenia pamięci masowej, a następnie zapisywane w odporne rozproszone zestawu danych (RDD). Na platformie Azure odpornej na uszkodzenia magazyn jest wspierana przez usługę Azure Storage lub Azure Data Lake Store w systemie HDFS. W aplikacji Spark Streaming zapisu z wyprzedzeniem dziennik jest włączony dla wszystkich odbiorców, ustawiając `spark.streaming.receiver.writeAheadLog.enable` ustawienia konfiguracji do `true`. Dziennik zapisu z wyprzedzeniem zapewnia odporność na uszkodzenia na wypadek awarii sterownik i executors.

Dla procesów roboczych uruchomionych zadań przed dane zdarzenia każdy RDD jest zgodnie z definicją zarówno replikowane i rozproszone na wielu procesów roboczych. Jeśli zadanie nie powiedzie się, ponieważ proces roboczy, uruchamiając go wystąpiła awaria, zadanie zostanie ponownie uruchomiona w ramach innego procesu roboczego, z repliką dane zdarzenia, więc zdarzenia nie zostaną utracone.

### <a name="use-checkpoints-for-drivers"></a>Używać punktów kontrolnych dla sterowników

Sterowniki zadania trzeba ponownego uruchamiania. Sterownik Spark Streaming aplikacja uruchamiana ulegnie awarii, dopiero po z nim wszystkie uruchomione odbiorników, zadania i wszelkich danych przechowywania danych zdarzenia. W takim przypadku konieczne będzie można zapisać postęp zadania, dzięki czemu można wznowić ją później. Jest to osiągane przez tworzenie punktów kontrolnych kierowane acykliczne wykresu (DAG) programu DStream okresowo w magazynie odpornej na uszkodzenia. Metadane DAG obejmują konfiguracji używane do tworzenia aplikacji przesyłania strumieniowego, operacje, które definiują aplikację i wszystkie instancje, które są umieszczane w kolejce, ale nie została jeszcze ukończona. Te metadane umożliwia zakończonych niepowodzeniem sterowników, należy ponownie uruchomić z informacji dotyczących punktu kontrolnego. Po ponownym uruchomieniu sterownika, uruchomi nowych odbiorców sami odzyskiwać dane zdarzenia do zestawów danych z dziennika zapisu z wyprzedzeniem.

Punkty kontrolne są włączone w przesyłania strumieniowego platformy Spark w dwóch krokach. 

1. W obiekcie StreamingContext należy skonfigurować ścieżkę magazynu na ich złączenie:

    sterujący Val = nowe StreamingContext (spark, Seconds(1)) ssc.checkpoint("/path/to/checkpoints")

    HDInsight należy zapisać te punkty kontrolne do domyślnego magazynu dołączone do klastra, usługa Azure Storage lub Azure Data Lake Store.

2. Następnie określ interwał punktu kontrolnego (w sekundach) na DStream. Po każdym odstępie dane o stanie pochodną dane wejściowe zdarzenia są utrwalane w magazynie. Dane stanu utrwalonego może zmniejszyć obliczeń potrzebne podczas odbudowywania stanu ze źródła zdarzenia.

    Wiersze Val = ssc.socketTextStream ("Nazwa hosta", 9999) lines.checkpoint(30) ssc.start() ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Użyj ujścia idempotentne

Docelowego ujścia, do którego zadanie zapisuje wyniki musi być w stanie obsługiwać sytuację, w której otrzymuje ten sam wynik więcej niż jeden raz. Ujścia musi być w stanie wykryć takie zduplikowane wyniki i je ignorować. *Idempotentne* ujścia można wywołać wiele razy z tymi samymi danymi bez zmian stanu.

Implementowanie logiki, która najpierw sprawdza istnienie przychodzących wynik w magazynu danych można utworzyć ujścia idempotentne. Jeśli wynik już istnieje, zapis powinna zostać wyświetlona zakończyło się sukcesem z punktu widzenia zadanie platformy Spark, ale w rzeczywistości swoim magazynem danych ignorowane zduplikowanych danych. Jeśli wynik nie istnieje, obiekt sink należy wstawić nowy wynik do jego magazynu. 

Na przykład można użyć procedury składowanej przy użyciu usługi Azure SQL Database, która wstawia zdarzenia do tabeli. Tę procedurę składowaną najpierw wyszukuje zdarzenia według pól kluczy i tylko wtedy, gdy nie pasującego zdarzenia można odnaleźć rekordu wstawione do tabeli.

Innym przykładem jest używać partycjonowanych systemu plików, takich jak obiekty BLOB usługi Azure Storage lub Azure Data Lake store. W tym przypadku logikę ujścia nie trzeba sprawdzić, czy istnieje plik. Jeśli istnieje plik reprezentujący zdarzenia, po prostu jest zastępowany przy użyciu tych samych danych. W przeciwnym razie tworzony jest nowy plik w ścieżce obliczanej.

## <a name="next-steps"></a>Kolejne kroki

* [Omówienie przesyłania strumieniowego platformy Spark](apache-spark-streaming-overview.md)
* [Tworzenie zadań przesyłania strumieniowego platformy Spark o wysokiej dostępności w YARN](apache-spark-streaming-high-availability.md)
