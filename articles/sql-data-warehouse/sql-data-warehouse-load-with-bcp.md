---
title: "Ładowanie danych do usługi SQL Data Warehouse przy użyciu programu bcp | Microsoft Docs"
description: "Dowiedz się, czym jest program bcp i jak z niego korzystać w scenariuszach dotyczących magazynów danych."
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: e368ae8b249fe3c33371794160440e472b0f35e3
ms.lasthandoff: 01/30/2017



---
# <a name="load-data-with-bcp"></a>Ładowanie danych za pomocą narzędzia BCP
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

**[bcp][bcp]** to narzędzie wiersza polecenia do ładowania zbiorczego, które umożliwia kopiowanie danych między programem SQL Server, plikami danych i usługą SQL Data Warehouse. Narzędzie bcp służy do importowania dużej liczby wierszy do tabel usługi SQL Data Warehouse lub do eksportowania danych z tabel programu SQL Server do plików danych. Z wyjątkiem przypadków obejmujących użycie opcji queryout program bcp nie wymaga żadnej znajomości języka Transact-SQL.

Za pomocą programu bcp można szybko i łatwo przenosić mniejsze zestawy danych do i z bazy danych usługi SQL Data Warehouse. Dokładne zalecenia dotyczące ilości danych do ładowania/wyodrębniania za pomocą narzędzia bcp będą zależeć od połączenia sieciowego z centrum danych Azure.  Ogólnie rzecz biorąc, przy użyciu narzędzia bcp można łatwo ładować tabele wymiarów i wyodrębniać z nich dane, jednak narzędzie to nie jest zalecane dla ładowania lub wyodrębniania dużych ilości danych.  Zalecanym narzędziem do ładowania i wyodrębnianie dużych ilości danych jest program Polybase, który lepiej wykonuje to zadanie, wykorzystując architekturę masowego przetwarzania równoległego usługi SQL Data Warehouse.

Narzędzie bcp umożliwia:

* Ładowanie danych do usługi SQL Data Warehouse przy użyciu prostego narzędzia wiersza polecenia.
* Wyodrębnianie danych z usługi SQL Data Warehouse przy użyciu prostego narzędzia wiersza polecenia.

Ten samouczek przedstawia sposób wykonania następujących czynności:

* Importowanie danych do tabeli za pomocą polecenia bcp in
* Eksportowanie danych do tabeli za pomocą polecenia bcp out

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse
* Zainstalowane narzędzie wiersza polecenia bcp
* Zainstalowane narzędzie wiersza polecenia SQLCMD

> [!NOTE]
> Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Importowanie danych do usługi SQL Data Warehouse
W tym samouczku utworzysz tabelę w usłudze Azure SQL Data Warehouse i zaimportujesz dane do tej tabeli.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Krok 1: tworzenie tabeli w usłudze Azure SQL Data Warehouse
W wierszu polecenia użyj polecenia sqlcmd, aby uruchomić następujące zapytanie w celu utworzenia tabeli w wystąpieniu:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

> [!NOTE]
> Zobacz artykuł [Omówienie tabel][Table Overview] lub [Składnia polecenia CREATE TABLE][CREATE TABLE syntax], aby uzyskać więcej informacji dotyczących tworzenia tabel w usłudze SQL Data Warehouse oraz dostępnych opcji klauzuli WITH.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Krok 2: tworzenie źródłowego pliku danych
Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku tekstowego, a następnie zapisz ten plik w lokalnym katalogu tymczasowym: C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Należy pamiętać, że program bcp.exe nie obsługuje kodowania pliku w formacie UTF-8. Podczas korzystania z programu bcp.exe należy używać plików kodowanych w formacie ASCII lub UTF-16.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Krok 3: nawiązywanie połączenia i importowanie danych
Przy użyciu narzędzia bcp można nawiązać połączenie i zaimportować dane, stosując następujące polecenie z odpowiednio zastąpionymi wartościami:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Poprawność załadowania danych można sprawdzić za pomocą następującego zapytania uruchomionego przy użyciu narzędzia sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Powinny zostać zwrócone następujące wyniki:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: tworzenie statystyk na podstawie nowo załadowanych danych
Usługa Azure SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk. W celu uzyskania najlepszej wydajności zapytań należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie. Szczegółowy opis statystyk znajduje się w temacie [Statystyki][Statistics] w grupie artykułów dla deweloperów. Poniżej przedstawiono prosty przykład tworzenia statystyk dotyczących tabeli załadowanej w tym przykładzie.

W wierszu polecenia sqlcmd wykonaj następującą instrukcję CREATE STATISTICS:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Eksportowanie danych z usługi SQL Data Warehouse
W tym samouczku utworzysz plik danych z tabeli w usłudze SQL Data Warehouse. Wyeksportujemy dane utworzone powyżej do nowego pliku danych o nazwie DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Krok 1: eksportowanie danych
Przy użyciu narzędzia bcp można nawiązać połączenie i wyeksportować dane za pomocą następującego polecenia z odpowiednio zastąpionymi wartościami:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Możesz sprawdzić, czy dane zostały poprawnie wyeksportowane, otwierając nowy plik. Dane w pliku powinny zawierać poniższy tekst:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Ze względu na specyfikę systemów rozproszonych kolejność danych w bazach danych usługi SQL Data Warehouse może się różnić. Innym rozwiązaniem jest użycie funkcji **queryout** programu bcp, aby napisać zapytanie wyodrębniające, zamiast eksportowania całej tabeli.
> 
> 

## <a name="next-steps"></a>Następne kroki
Omówienie ładowania można znaleźć w artykule [Ładowanie danych do usługi SQL Data Warehouse][Load data into SQL Data Warehouse].
Więcej porad dla deweloperów znajduje się w artykule [Omówienie programowania w usłudze SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433

