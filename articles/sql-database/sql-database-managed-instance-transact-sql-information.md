---
title: Usługa Azure SQL Database Managed różnice języka T-SQL w wystąpieniu | Dokumentacja firmy Microsoft
description: W tym artykule omówiono różnice języka T-SQL wystąpienia zarządzanego Azure SQL Database i programu SQL Server
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: 6ffa09343560e4ac55b1fd62325fd4e3bd370848
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242181"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Różnice w usługi Azure SQL Database zarządzane wystąpienia języka T-SQL z programu SQL Server

Wystąpienie usługi Azure SQL Database Managed zapewnia wysoką zgodność z aparatem bazy danych serwera SQL w środowisku lokalnym. Większość funkcji aparatu bazy danych programu SQL Server są obsługiwane w wystąpieniu zarządzanym. Ponieważ nadal istnieją pewne różnice w składnią i zachowaniem, ten artykuł zawiera podsumowanie i opisano te różnice.

- [Różnice w języku T-SQL i nieobsługiwane funkcje](#Differences)
- [Funkcje, które mają różne zachowanie w wystąpieniu zarządzanym](#Changes)
- [Tymczasowe ograniczenia i znane problemy](#Issues)

## <a name="Differences"></a> Różnice języka T-SQL z programu SQL Server

Ta sekcja zawiera podsumowanie podstawowych różnic w składni języka T-SQL i zachowanie między wystąpienia zarządzanego i aparatu bazy danych serwera SQL w środowisku lokalnym, a także nieobsługiwanych funkcji.

### <a name="always-on-availability"></a>Dostępność zawsze włączona

[Wysoka dostępność](sql-database-high-availability.md) jest wbudowana w wystąpieniu zarządzanym i nie mogą być kontrolowane przez użytkowników. Poniższe instrukcje nie są obsługiwane:

- [TWORZENIE PUNKTU KOŃCOWEGO... ABY UZYSKAĆ DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [UTWÓRZ GRUPĘ DOSTĘPNOŚCI](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [POLECENIA ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [GRUPA DOSTĘPNOŚCI LISTY](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) klauzuli [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) — instrukcja

### <a name="auditing"></a>Inspekcja

Podstawowe różnice między inspekcji SQL w wystąpieniu zarządzanym, Azure SQL Database i SQL Server w środowisku lokalnym są:

- W wystąpieniu zarządzanym inspekcji SQL działa na poziomie serwera i magazyny `.xel` konta magazynu obiektów blob plików na platformie Azure.  
- W usłudze Azure SQL Database inspekcji SQL, działa na poziomie bazy danych.
- W programie SQL Server w środowisku lokalnym / maszyny wirtualnej, inspekcji SQL, który działa na poziomie serwera, ale przechowuje zdarzenia w plikach systemu/dzienniki zdarzeń systemu windows.  
  
Inspekcji systemu XEvent, w wystąpieniu zarządzanym obsługuje obiekty docelowe magazynu obiektów blob platformy Azure. Dzienniki plików i systemu windows nie są obsługiwane.

Klucz różnice w `CREATE AUDIT` składnia dla inspekcji w usłudze Azure blob storage są:

- Nowa składnia `TO URL` jest dostarczany i umożliwia określenie adresu URL kontenera magazynu obiektów blob platformy Azure gdzie `.xel` zostaną umieszczone pliki
- Składnia `TO FILE` nie jest obsługiwana, ponieważ wystąpienia zarządzanego nie można uzyskać dostępu do udziałów plików w Windows.

Aby uzyskać więcej informacji, zobacz:  

- [TWORZENIE INSPEKCJI SERWERA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [INSTRUKCJA ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Inspekcja](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="backup"></a>Backup

Wystąpienie zarządzane jest automatyczne tworzenie kopii zapasowych i pozwala użytkownikom na tworzenie pełnej bazy danych `COPY_ONLY` kopii zapasowych. Różnicowe kopie zapasowe, log oraz kopii zapasowych migawki pliku nie są obsługiwane.

- Wystąpienie zarządzane kopi zapasowej bazy danych tylko do konta usługi Azure Blob Storage:
  - Tylko `BACKUP TO URL` jest obsługiwana
  - `FILE`, `TAPE`, a urządzenia kopii zapasowej nie są obsługiwane.  
- Większość ogólnych `WITH` opcje są obsługiwane
  - `COPY_ONLY` jest wymagany
  - `FILE_SNAPSHOT` Nie jest obsługiwany
  - Opcje taśmy: `REWIND`, `NOREWIND`, `UNLOAD`, i `NOUNLOAD` nie są obsługiwane
  - Opcje dziennika: `NORECOVERY`, `STANDBY`, i `NO_TRUNCATE` nie są obsługiwane

Ograniczenia:  

- Wystąpienie zarządzane można utworzyć kopię zapasową bazy danych w kopii zapasowej z maksymalnie 32 rozkłada, który jest wystarczająca dla baz danych do 4 TB, jeśli kompresja kopii zapasowej jest używany.
- Maksymalny rozmiar kopii zapasowej usługi stripe to 195 GB (rozmiar maksymalny obiektów blob). Zwiększ liczbę paski w kopii zapasowej polecenie, aby zmniejszyć rozmiar woluminu rozłożonego indywidualnych i w ramach tego limitu.

> [!TIP]
> Aby obejść to ograniczenie w środowisku lokalnym, wykonywanie kopii zapasowej na `DISK` zamiast wykonywania kopii zapasowej na `URL`, przekazywanie pliku kopii zapasowej do obiektów blob, a następnie przywrócić. Ponieważ typ inny obiektu blob jest używany, należy przywrócić pliki większe obsługuje.  

Aby uzyskać informacji na temat kopii zapasowych przy użyciu języka T-SQL, zobacz [kopii zapasowej](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Rozszerzenie puli buforów

- [Rozszerzenie puli bufora](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) nie jest obsługiwane.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` nie jest obsługiwane. Zobacz [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="bulk-insert--openrowset"></a>Wstawianie zbiorcze / openrowset

Wystąpienia zarządzanego nie można uzyskać dostępu udziałów plików i folderów Windows, więc należy importować pliki z usługi Azure blob storage:

- `DATASOURCE` jest wymagany w `BULK INSERT` polecenia podczas importowania plików z magazynu obiektów blob platformy Azure. Zobacz [WSTAWIANIA ZBIORCZEGO](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` jest wymagany w `OPENROWSET` działają w przypadku odczytać zawartość pliku z usługi Azure blob storage. Zobacz [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="certificates"></a>Certyfikaty

Wystąpienie zarządzane nie może uzyskiwać dostępu do udziałów plików i folderów systemu Windows, dlatego obowiązują następujące ograniczenia:

- `CREATE FROM`/`BACKUP TO` plik nie jest obsługiwany w przypadku certyfikatów
- `CREATE`/`BACKUP` certyfikat od `FILE` / `ASSEMBLY` nie jest obsługiwane. Nie można użyć plików kluczy prywatnych.  

Zobacz [Utwórz certyfikat](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) i [kopii zapasowej certyfikatu](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Obejście**: klucz prywatny certyfikatu/script, zapisać jako plik SQL i tworzenie na podstawie pliku binarnego:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="clr"></a>CLR

Wystąpienie zarządzane nie może uzyskiwać dostępu do udziałów plików i folderów systemu Windows, dlatego obowiązują następujące ograniczenia:

- Tylko `CREATE ASSEMBLY FROM BINARY` jest obsługiwana. Zobacz [tworzenia zestawu z danych](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` nie jest obsługiwane. Zobacz [tworzenie zestawów z pliku](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` Nie można odwoływać się do plików. Zobacz [zmiana zestawu](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="compatibility-levels"></a>Poziomy zgodności

- Poziomy zgodności obsługiwane są: 100, 110, 120, 130, 140  
- Poziomy zgodności poniżej 100 nie są obsługiwane.
- Domyślny poziom zgodności dla nowych baz danych jest 140. W przypadku przywróconych baz danych poziom zgodności pozostanie niezmieniona Jeśli był 100 lub nowszym.

Zobacz [poziom zgodności bazy danych ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="credential"></a>Poświadczenie

Tylko usługi Azure Key Vault i `SHARED ACCESS SIGNATURE` tożsamości są obsługiwane. Windows, użytkownicy nie są obsługiwane.

Zobacz [Utwórz POŚWIADCZENIA](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) i [POŚWIADCZEŃ ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Dostawcy usług kryptograficznych

Wystąpienia zarządzanego nie można uzyskać dostęp do plików, więc nie można utworzyć dostawcy usług kryptograficznych:

- `CREATE CRYPTOGRAPHIC PROVIDER` nie jest obsługiwane. Zobacz [dostawcy usług KRYPTOGRAFICZNYCH Utwórz](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` nie jest obsługiwane. Zobacz [dostawcy usług KRYPTOGRAFICZNYCH ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="collation"></a>Sortowanie

Opcja sortowania serwera jest `SQL_Latin1_General_CP1_CI_AS` i nie można zmienić. Zobacz [sortowania](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="database-options"></a>Opcje bazy danych

- Wiele plików dziennika nie są obsługiwane.
- Obiekty w pamięci nie są obsługiwane w przypadku warstwy usług ogólnego przeznaczenia.  
- Ma limitu 280 plików dla każdego wystąpienia obszaru max 280 plików na bazę danych. Plików dziennika i danych są wliczane do tego limitu.  
- Baza danych nie mogą zawierać grup plików zawierających dane filestream.  Przywracania zakończy się niepowodzeniem, jeśli zawiera .bak `FILESTREAM` danych.  
- Każdy plik jest umieszczany w usłudze Azure Premium storage. We/Wy i przepływność na plik zależeć od rozmiaru każdego pliku, w taki sam sposób jak w przypadku dysków usługi Premium Storage dla platformy Azure. Zobacz [wydajności dysków Azure w wersji Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  

#### <a name="create-database-statement"></a>Instrukcja CREATE DATABASE

Poniżej przedstawiono `CREATE DATABASE` ograniczenia:

- Nie można zdefiniować pliki i grupy plików.  
- `CONTAINMENT` opcja nie jest obsługiwana.  
- `WITH`Opcje nie są obsługiwane.  
   > [!TIP]
   > Jako obejście tego problemu należy użyć `ALTER DATABASE` po `CREATE DATABASE` można ustawić opcji bazy danych, aby dodać pliki lub ustawić zawierania.  

- `FOR ATTACH` opcja nie jest obsługiwana.
- `AS SNAPSHOT OF` opcja nie jest obsługiwana.

Aby uzyskać więcej informacji, zobacz [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instrukcja ALTER DATABASE

Niektóre właściwości pliku nie można ustawić lub zmienić:

- Nie można określić ścieżkę pliku w `ALTER DATABASE ADD FILE (FILENAME='path')` instrukcję języka T-SQL. Usuń `FILENAME` ze skryptu, ponieważ zarządzane wystąpienie automatycznie umieszcza pliki.  
- Nie można zmienić nazwy pliku, przy użyciu `ALTER DATABASE` instrukcji.

Poniższe opcje są domyślnie i nie można zmienić:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

Nie można zmodyfikować następujące opcje:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Modyfikowanie nazwa nie jest obsługiwana.

Aby uzyskać więcej informacji, zobacz [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Dublowanie bazy danych

Funkcja dublowania bazy danych nie jest obsługiwane.

- `ALTER DATABASE SET PARTNER` i `SET WITNESS` opcje nie są obsługiwane.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` nie jest obsługiwane.

Aby uzyskać więcej informacji, zobacz [ALTER DATABASE SET PARTNER i SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) i [CREATE ENDPOINT... Aby UZYSKAĆ DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC

Nieudokumentowany instrukcji DBCC, które są włączone w programie SQL Server nie są obsługiwane w wystąpieniu zarządzanym.

- `Trace Flags` nie są obsługiwane. Zobacz [flagi śledzenia](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` nie jest obsługiwane. Zobacz [polecenia DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` nie jest obsługiwane. Zobacz [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Transakcje rozproszone

Żadna usługa MSDTC ani [transakcje elastyczne](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) są obecnie obsługiwane w wystąpieniu zarządzanym.

### <a name="extended-events"></a>Rozszerzone zdarzenia

Niektóre cele Windows specyficznych dla systemu XEvents nie są obsługiwane:

- `etw_classic_sync target` nie jest obsługiwane. Store `.xel` magazynu obiektów blob plików na platformie Azure. Zobacz [docelowej etw_classic_sync](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target).
- `event_file target`nie jest obsługiwane. Store `.xel` magazynu obiektów blob plików na platformie Azure. Zobacz [docelowej event_file](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Zewnętrzne biblioteki

W bazie danych języków R i Python w zewnętrznych bibliotekach nie są jeszcze obsługiwane. Zobacz [programu SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM i Filetable

- danych FILESTREAM nie jest obsługiwana.
- Baza danych nie mogą zawierać grup plików przy użyciu `FILESTREAM` danych
- `FILETABLE` nie jest obsługiwane
- Tabele nie mogą mieć `FILESTREAM` typów
- Następujące funkcje nie są obsługiwane:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Aby uzyskać więcej informacji, zobacz [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) i [Filetable](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantyczne wyszukiwanie pełnotekstowe

[Wyszukiwanie semantyczne](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) nie jest obsługiwane.

### <a name="linked-servers"></a>Serwery połączone

Połączone serwery w wystąpieniu zarządzanym obsługują ograniczoną liczbę elementów docelowych:

- Obsługiwane obiekty docelowe: SQL Server i bazy danych SQL
- Nieobsługiwane elementy docelowe: pliki, usługi Analysis Services i inne RDBMS.

Operacje

- Transakcje zapisu dla wielu wystąpień nie są obsługiwane.
- `sp_dropserver` jest obsługiwana dla porzucenie połączonego serwera. Zobacz [serwera sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` funkcja może służyć do wykonywania zapytań tylko w wystąpieniach programu SQL Server (albo zarządzane lokalnie lub na maszynach wirtualnych). Zobacz [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` funkcja może służyć do wykonywania zapytań tylko w wystąpieniach programu SQL Server (albo zarządzane lokalnie lub na maszynach wirtualnych). Tylko `SQLNCLI`, `SQLNCLI11`, i `SQLOLEDB` wartości są obsługiwane jako dostawca. Na przykład: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Zobacz [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="logins--users"></a>Identyfikatory logowania / użytkownicy

- Utworzone nazw logowania SQL `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, i `FROM SID` są obsługiwane. Zobacz [logowania Utwórz](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Logowania Windows utworzonych za pomocą `CREATE LOGIN ... FROM WINDOWS` składni nie są obsługiwane.
- Azure użytkownik usługi Active Directory (Azure AD), który utworzył wystąpienie ma [nieograniczone uprawnienia administratora](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Użytkownicy poziomu bazy danych usługi Azure Active Directory (Azure AD) niebędący administratorami można tworzyć przy użyciu `CREATE USER ... FROM EXTERNAL PROVIDER` składni. Zobacz [Utwórz użytkownika... Z ZEWNĘTRZNEGO DOSTAWCY](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)

### <a name="polybase"></a>Program Polybase

Tabele zewnętrzne odwołujące się do plików w systemie plików HDFS lub Azure blob storage nie są obsługiwane. Aby uzyskać informacji na temat technologii Polybase, zobacz [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replikacja

Replikacja jest dostępna w publicznej wersji zapoznawczej na wystąpieniu zarządzanym. Aby uzyskać informacji o replikacji, zobacz [replikacji programu SQL Server](http://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>Przywracanie — instrukcja

- Obsługiwanej składni
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Nieobsługiwana składnia
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Element źródłowy  
  - `FROM URL` (Magazyn obiektów blob platformy azure) jest tylko obsługiwaną opcją.
  - `FROM DISK`/`TAPE`/ urządzenie kopii zapasowej nie jest obsługiwana.
  - Zestawy kopii zapasowych nie są obsługiwane.
- `WITH` Opcje nie są obsługiwane (Brak `DIFFERENTIAL`, `STATS`itp.)
- `ASYNC RESTORE` -Restore będzie kontynuowane, nawet w przypadku zerwania połączenia klienta. Jeśli połączenie zostanie porzucone, możesz sprawdzić `sys.dm_operation_status` widok stanu operacji przywracania (a także tworzenie i UPUSZCZANIE bazy danych). Zobacz [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

Następujące opcje bazy danych są zastąpione/zestawu i nie można zmienić później:  

- `NEW_BROKER` (jeśli broker nie jest włączona w pliku z rozszerzeniem bak)  
- `ENABLE_BROKER` (jeśli broker nie jest włączona w pliku z rozszerzeniem bak)  
- `AUTO_CLOSE=OFF` (jeśli ma bazy danych w pliku bak `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (jeśli ma bazy danych w pliku bak `SIMPLE` lub `BULK_LOGGED` tryb odzyskiwania)
- Grupa plików zoptymalizowana pod kątem pamięci zostaną dodane i wywoływane XTP, jeśli nie znajdowała się w pliku bak źródła  
- Wszelkie istniejące grupy plików zoptymalizowanej pod kątem pamięci została zmieniona na XTP  
- `SINGLE_USER` i `RESTRICTED_USER` opcje są konwertowane na `MULTI_USER`

Ograniczenia:  

- `.BAK` Nie można przywrócić pliki zawierające wiele zestawów kopii zapasowych.
- `.BAK` Nie można przywrócić plików zawierających wiele plików dziennika.
- Przywracania zakończy się niepowodzeniem, jeśli zawiera .bak `FILESTREAM` danych.
- Tworzenie kopii zapasowych zawierający bazy danych, które obiekty były aktywne w pamięci obecnie nie można przywrócić.  
- Tworzenie kopii zapasowych zawierający bazy danych, gdzie w pewnym momencie obiektów w pamięci istniały obecnie nie można przywrócić.
- Tworzenie kopii zapasowych baz danych w trybie tylko do odczytu zawierające obecnie nie można przywrócić. To ograniczenie zostanie wkrótce usunięty.

Aby uzyskać informacje na temat instrukcji Restore, zobacz [PRZYWRÓCIĆ instrukcji](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Usługa Service broker

Broker usług dla wielu wystąpień nie jest obsługiwana:

- `sys.routes` — Wymagania wstępne: Wybierz adres z sys.routes. Adres musi być lokalny dla każdej ścieżki. Zobacz [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE` — nie jest możliwe `CREATE ROUTE` z `ADDRESS` innych niż `LOCAL`. Zobacz [Utwórz trasy](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE` Nie można `ALTER ROUTE` z `ADDRESS` innych niż `LOCAL`. Zobacz [trasy ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="service-key-and-service-master-key"></a>Usługa klucza głównego klucza i usługi

- [Kopię zapasową klucza głównego](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) nie jest obsługiwane (zarządzane przez usługę SQL Database)
- [Przywróć klucz główny](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) nie jest obsługiwane (zarządzane przez usługę SQL Database)
- [Kopię zapasową klucza głównego usługi](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) nie jest obsługiwane (zarządzane przez usługę SQL Database)
- [Przywróć klucz główny usługi](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) nie jest obsługiwane (zarządzane przez usługę SQL Database)

### <a name="stored-procedures-functions-triggers"></a>Procedury składowane, funkcje, wyzwalaczy

- `NATIVE_COMPILATION` obecnie nie jest obsługiwana.
- Następujące [procedury składowanej sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) opcje nie są obsługiwane:
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `max text repl size`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` nie jest obsługiwane. Zobacz [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` nie jest obsługiwane. Zobacz [procedury xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` nie są obsługiwane, w tym `sp_addextendedproc`  i `sp_dropextendedproc`. Zobacz [rozszerzonych procedur składowanych](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, i `sp_detach_db` nie są obsługiwane. Zobacz [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), i [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` nie jest obsługiwane. Zobacz [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>Program SQL Server Agent

- Ustawienia agenta SQL są tylko do odczytu. Procedura `sp_set_agent_properties` nie jest obsługiwana w wystąpieniu zarządzanym.  
- Stanowiska
  - Kroki w zadaniu języka T-SQL są obsługiwane.
  - Obsługiwane są następujące zadania replikacji:
    - Czytnik dziennika transakcji.  
    - Migawki.
    - Dystrybutor
  - Kroki zadania SSIS są obsługiwane.
  - Inne rodzaje kroków zadania nie są obecnie obsługiwane, w tym:
    - Krok zadania replikacji scalającej nie jest obsługiwane.  
    - Czytnik kolejki nie jest obsługiwane.  
    - Powłoka poleceń nie jest jeszcze obsługiwana.
  - Wystąpienia zarządzanego nie można uzyskać dostępu zewnętrznych zasobów (np. udziały sieciowe za pośrednictwem robocopy).  
  - Program PowerShell nie jest jeszcze obsługiwana.
  - Usługi Analysis Services nie są obsługiwane.
- Powiadomienia są obsługiwane częściowo
- Powiadomienie e-mail jest obsługiwane, wymagane jest skonfigurowanie profil poczty bazy danych. Może istnieć tylko jedna baza danych profilu poczty i musi zostać wywołana `AzureManagedInstance_dbmail_profile` w publicznej wersji zapoznawczej (tymczasowe ograniczenie).  
  - Pagera nie jest obsługiwane.  
  - NetSend nie jest obsługiwane.
  - Alerty nie są jeszcze nie obsługiwane.
  - Serwery proxy nie są obsługiwane.  
- Dziennik zdarzeń nie jest obsługiwane.

Następujące funkcje nie są obecnie obsługiwane, ale zostaną włączone w przyszłości:

- Serwery proxy
- Planowanie zadań w stanie bezczynności procesora CPU
- Włączanie/wyłączanie agenta
- Alerty

Aby uzyskać informacji na temat programu SQL Server Agent, zobacz [programu SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tabele

Obsługiwane są następujące funkcje nie:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Aby uzyskać informacji na temat tworzenia i modyfikowania tabel, zobacz [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) i [instrukcji ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="Changes"></a> Zmiany zachowania

Następujące zmienne, funkcje i widoki zwracają różne wyniki:

- `SERVERPROPERTY('EngineEdition')` Zwraca wartość 8. Ta właściwość jest jednoznacznie identyfikuje wystąpienie zarządzane. Zobacz [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Zwraca wartość NULL, ponieważ koncepcji wystąpienia, ponieważ nie istnieje dla programu SQL Server nie ma zastosowania do wystąpienia zarządzanego. Zobacz [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` Zwraca pełną "składnika" nazwą DNS, na przykład Moje instance.wcus17662feb9ce98.database.windows.net zarządzane. Zobacz [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -Zwraca pełną "składnika" nazwy DNS, takich jak `myinstance.domain.database.windows.net` dla właściwości "name" i "data_source". Zobacz [SYS. SERWERY](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` Zwraca wartość NULL, ponieważ koncepcję usług, ponieważ nie istnieje dla programu SQL Server nie ma zastosowania do wystąpienia zarządzanego. Zobacz [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` jest obsługiwany. Zwraca wartość NULL, jeśli logowanie do usługi AAD nie jest sys.syslogins. Zobacz [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` nie jest obsługiwane. Zwraca nieprawidłowe dane (znany problem tymczasowy). Zobacz [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` i inne funkcje wbudowane daty/godziny zawsze zwraca czas w strefie czasowej UTC. Zobacz [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Znane problemy i ograniczenia

### <a name="tempdb-size"></a>Rozmiar bazy danych TEMPDB

`tempdb` zostanie podzielona na 12 plików za pomocą maksymalny rozmiar 14 GB na plik. Nie można zmienić ten maksymalny rozmiar każdego pliku i nie można dodać nowe pliki do `tempdb`. To ograniczenie zostanie wkrótce usunięty. Niektóre zapytania może zwrócić błąd, gdy potrzebują więcej niż 168 GB `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Przekroczenia miejsca do magazynowania z plikami małej bazy danych

Każde wystąpienie zarządzane musi 35 TB pamięci masowej zarezerwowane dla miejsca na dysku Premium platformy Azure, a każdego pliku bazy danych znajduje się na innym dysku fizycznym. Rozmiary dysków może być 128 GB, 256 GB, 512 GB, 1 TB lub 4 TB. Nieużywane miejsce na dysku nie jest rozliczany, ale suma rozmiarów dysków w warstwie Premium platformy Azure nie może przekraczać 35 TB. W niektórych przypadkach wystąpienia zarządzanego, które nie wymagają 8 TB w sumie może przekraczać 35 TB Azure limit rozmiaru magazynu, z powodu wewnętrznego fragmentacji.

Na przykład wystąpienie zarządzane mogą mieć jeden plik 1,2 TB, rozmiar, który jest umieszczony na dysku 4 TB i rozmiar ins 1 GB 248 pliki są umieszczone na oddzielnych dyskach 128 GB. W tym przykładzie:

- rozmiar magazynu całkowitego miejsca na dysku jest 1-4 TB + 248 × 128 GB = 35 TB.
- łączne miejsce zarezerwowane dla baz danych w wystąpieniu jest 1 x 1,2 TB + 248 x 1 GB = 1,4 TB pojemności.

Obrazuje to, że w pewnych okolicznościach, ze względu na bardzo szczegółowych dystrybucji plików, wystąpienie zarządzane mogą dotrzeć do 35 TB zarezerwowane dla dołączonego dysku w warstwie Premium usługi Azure, gdy być może nie oczekujesz.

W tym przykładzie istniejących baz danych będą nadal działać i można rozwijać bez żadnych przeszkód, tak długo, jak nowe pliki nie zostaną dodane. Jednak nowe bazy danych może nie można utworzyć ani przywrócić, ponieważ nie ma wystarczającej ilości miejsca dla nowych dysków twardych, nawet wtedy, gdy łączny rozmiar wszystkich baz danych nie osiąga limit rozmiaru wystąpienia. Błąd, który jest zwracany nie jest w takim przypadku usuń zaznaczenie.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Przywróć niepoprawnej konfiguracji klucza sygnatury dostępu Współdzielonego podczas bazy danych

`RESTORE DATABASE` który odczytuje plik bak może być stale ponawianie próby można odczytać pliku bak i zwróciła błąd po długim okresie czasu, jeśli sygnatura dostępu współdzielonego w `CREDENTIAL` jest niepoprawny. Wykonaj Przywracanie HEADERONLY przed przywróceniem bazy danych należy upewnić się, że klucz sygnatury dostępu Współdzielonego jest poprawna.
Upewnij się, że usuwa wiodące `?` z klucza sygnatury dostępu Współdzielonego, który jest generowany przy użyciu witryny Azure portal.

### <a name="tooling"></a>Narzędzia

SQL Server Management Studio i SQL Server Data Tools, może być pewne problemy podczas uzyskiwania dostępu do wystąpienia zarządzanego. Wszystkie problemy narzędzia zostaną rozwiązane przed ogólnie dostępne.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Nieprawidłowa baza danych nazw w niektórych widoków, dzienników i komunikatów

Kilka widoków systemowych, liczniki wydajności, komunikaty o błędach, XEvents i wpisów dziennika błędów Wyświetl identyfikator GUID identyfikatory bazy danych zamiast nazwy bazy danych. Nie należy polegać na tych identyfikatorów GUID, ponieważ ich zostanie zamienione nazwy bazy danych w przyszłości.

### <a name="database-mail-profile"></a>Profil poczty bazy danych

Może istnieć tylko jedna baza danych profilu poczty i musi zostać wywołana `AzureManagedInstance_dbmail_profile`. To tymczasowe ograniczenie, które zostaną wkrótce usunięte.

### <a name="error-logs-are-not-persisted"></a>Dzienniki błędów są utrwalane nie

Dzienniki błędów, które są dostępne w przypadku wystąpienia zarządzanego nie są zachowywane, a ich rozmiar jest niedostępna w limit maksymalnego rozmiaru magazynu. Dzienniki błędów może automatycznie usuwane w przypadku pracy awaryjnej.

### <a name="error-logs-are-verbose"></a>Dzienniki błędów są pełne

Wystąpienie zarządzane umieszcza pełne informacje w dziennikach błędów i wiele z nich nie są istotne. W przyszłości będzie można zmniejszyć ilość informacji w dziennikach błędów.

**Obejście**: niestandardowe procedury do odczytywania dzienników błędów, które filtru w poziomie niektóre-odpowiednie wpisy. Aby uzyskać więcej informacji, zobacz [DB wystąpienia zarządzanego Azure SQL — sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-is-not-supported"></a>Zakres transakcji na dwie bazy danych w ramach tego samego wystąpienia nie jest obsługiwana.

`TransactionScope` Klasa na platformie .net nie działa, jeśli dwa zapytania są wysyłane do dwóch baz danych w ramach tego samego wystąpienia w ramach tego samego zakresu transakcji:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Mimo że ten kod działa z danymi w ramach tego samego wystąpienia wymagane usługi MSDTC.

**Obejście**: Użyj [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) używać innej bazy danych w kontekście połączenia, zamiast korzystać z dwóch połączeń.

### <a name="clr-modules-and-linked-servers-sometime-cannot-reference-local-ip-address"></a>Moduły środowiska CLR i połączone serwery jakiś czas nie mogą odwoływać się lokalny adres IP

Moduły środowiska CLR, znajduje się w wystąpieniu zarządzanym i połączonych serwerów/rozproszonych zapytań, które odwołują się do pewnego czasu bieżącego wystąpienia nie można rozpoznać adresu IP lokalnego wystąpienia. Jest to błąd przejściowy.

**Obejście**: Użyj połączenia kontekstu w module środowiska CLR, jeśli to możliwe.

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać szczegółowe informacje o wystąpieniu zarządzanym, zobacz [co to jest wystąpienie zarządzane?](sql-database-managed-instance.md)
- Dla funkcji i listy porównanie, zobacz [typowe funkcje SQL](sql-database-features.md).
- Aby uzyskać szybki start omawiający Tworzenie nowego wystąpienia zarządzanego, zobacz [tworzenia wystąpienia zarządzanego](sql-database-managed-instance-get-started.md).
