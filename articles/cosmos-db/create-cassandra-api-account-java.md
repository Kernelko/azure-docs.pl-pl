---
title: Tworzenie konta, bazy danych i tabeli interfejsu API Cassandra usługi Azure Cosmos DB przy użyciu aplikacji języka Java
description: W tym artykule przedstawiono sposób tworzenia konta interfejsu API Cassandra oraz dodawania bazy danych (określanej również jako przestrzeń kluczy) i tabeli do tego konta przy użyciu aplikacji języka Java.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: b5e3d87e026b65a602b7bdf2e52365d13b21f62f
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166835"
---
# <a name="create-an-azure-cosmos-db-cassandra-api-account-database-and-a-table-by-using-a-java-application"></a>Tworzenie konta, bazy danych i tabeli interfejsu API Cassandra usługi Azure Cosmos DB przy użyciu aplikacji języka Java

W tym samouczku przedstawiono sposób użycia aplikacji języka Java do tworzenia konta interfejsu API Cassandra w usłudze Azure Cosmos DB, dodawania bazy danych (określanej również jako przestrzeń kluczy) oraz dodawania tabeli. Aplikacja języka Java używa [sterownika Java](https://github.com/datastax/java-driver) do tworzenia bazy danych użytkowników, która zawiera szczegółowe informacje, takie jak identyfikator użytkownika, nazwa użytkownika i miasto użytkownika.  

Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Tworzenie konta bazy danych Cassandra
> * Pobieranie parametrów połączenia konta
> * Tworzenie projektu Maven i zależności
> * Dodawanie bazy danych i tabeli
> * Uruchamianie aplikacji

## <a name="prerequisites"></a>Wymagania wstępne 

* Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz  [bezpłatne konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) . Alternatywnie można  [bezpłatnie wypróbować usługę Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)  bez subskrypcji platformy Azure — nie wymaga to opłat ani zobowiązań. 

* Pobierz najnowszą wersję zestawu [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 

* [Pobierz](http://maven.apache.org/download.cgi) i [zainstaluj](http://maven.apache.org/install.html) archiwum plików binarnych [Maven](http://maven.apache.org/) 
  - W systemie Ubuntu możesz uruchomić polecenie  `apt-get install maven` , aby zainstalować narzędzie Maven. 

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych 

1. Zaloguj się do witryny  [Azure Portal](https://portal.azure.com/). 

2. Wybierz kolejno pozycje  **Utwórz zasób** > **Bazy danych** > **Azure Cosmos DB**. 

3. W okienku  **Nowe konto**  wprowadź ustawienia nowego konta usługi Azure Cosmos DB. 

   |Ustawienie   |Sugerowana wartość  |Opis  |
   |---------|---------|---------|
   |ID   |   Wprowadź unikatową nazwę    | Wprowadź unikatową nazwę do identyfikacji tego konta usługi Azure Cosmos DB. <br/><br/>Ponieważ adres cassandra.cosmosdb.azure.com jest dołączany do podanego identyfikatora w celu utworzenia punktu kontaktowego, użyty identyfikator powinien być unikatowy, ale rozpoznawalny.         |
   |Interfejs API    |  Cassandra   |  Interfejs API określa typ konta do utworzenia. <br/> Wybierz pozycję **Cassandra**, ponieważ w tym artykule utworzysz bazę danych z wieloma kolumnami, dla których można tworzyć zapytania przy użyciu składni języka CQL.  |
   |Subskrypcja    |  Twoja subskrypcja        |  Wybierz subskrypcję platformy Azure, która ma być używana dla tego konta usługi Azure Cosmos DB.        |
   |Grupa zasobów   | Wprowadź nazwę    |  Wybierz pozycję  **Utwórz nową**, a następnie wprowadź nazwę nowej grupy zasobów dla konta. Dla uproszczenia można użyć takiej samej nazwy jak identyfikator.    |
   |Lokalizacja    |  Wybierz region najbliżej Twoich użytkowników    |  Wybierz lokalizację geograficzną, w której będzie hostowane Twoje konto usługi Azure Cosmos DB. Użyj lokalizacji znajdującej się najbliżej Twoich użytkowników, aby zapewnić im najszybszy dostęp do danych.    |

   ![Tworzenie konta za pomocą portalu](./media/create-cassandra-api-account-java/create-account.png)

4. Następnie wybierz pozycję  **Utwórz**. <br/>Tworzenie konta potrwa kilka minut. Po utworzeniu zasobu możesz zobaczyć powiadomienie **Wdrażanie zakończyło się pomyślnie** w prawym rogu portalu.

## <a name="get-the-connection-details-of-your-account"></a>Pobieranie szczegółów połączenia konta  

Pobierz informacje o parametrach połączenia z witryny Azure Portal i skopiuj je do pliku konfiguracji środowiska Java. Umożliwia to aplikacji komunikację z hostowaną bazą danych. 

1. W witrynie  [Azure Portal](http://portal.azure.com/) przejdź do swojego konta usługi Azure Cosmos DB. 

2. Otwórz okienko  **Parametry połączenia**.  

3. Skopiuj wartości **PUNKT KONTAKTOWY**, **PORT**, **NAZWA UŻYTKOWNIKA** i **HASŁO PODSTAWOWE** do użycia w następnych krokach.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Tworzenie projektu Maven, zależności i klas narzędzi 

Przykładowy projekt języka Java używany w tym artykule jest hostowany w witrynie GitHub. Możesz pobrać go z repozytorium [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started). 

Po pobraniu plików zaktualizuj informacje o parametrach połączenia w pliku `java-examples\src\main\resources\config.properties` i uruchom go.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_port = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Możesz również skompilować przykład od początku.  

1. W terminalu lub wierszu polecenia utwórz nowy projekt Maven o nazwie Cassandra-demo. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Znajdź folder `cassandra-demo`. Za pomocą edytora tekstów otwórz wygenerowany plik `pom.xml`. 

   Dodaj zależności bazy danych Cassandra i skompiluj wtyczki wymagane przez projekt, jak pokazano w pliku [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml).  

3. W folderze `cassandra-demo\src\main` utwórz nowy folder o nazwie `resources`.  W folderze zasobów dodaj pliki config.properties i log4j.properties:

   - W pliku [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) są przechowywane wartości kluczy i punktu końcowego połączenia interfejsu API Cassandra w usłudze Azure Cosmos DB. 
   
   - Plik [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) definiuje poziom rejestrowania wymagany podczas interakcji z interfejsem API Cassandra.  

4. Następnie przejdź do folderu `src/main/java/com/azure/cosmosdb/cassandra/`. W folderze cassandra utwórz inny folder o nazwie `utils`. W nowym folderze są przechowywane klasy narzędzi wymagane do połączenia z kontem interfejsu API Cassandra. 

   Dodaj klasę [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) w celu utworzenia klastra oraz otwarcia i zamknięcia sesji bazy danych Cassandra. Klaster łączy się z interfejsem API bazy danych Cassandra w usłudze Azure Cosmos DB i zwraca sesję umożliwiającą uzyskanie dostępu. Użyj klasy [Configurations](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) w celu odczytania informacji o parametrach połączenia z pliku config.properties. 

5. Przykład języka Java tworzy bazę danych z informacjami o użytkownikach, takimi jak nazwa użytkownika, identyfikator użytkownika i miasto użytkownika. Musisz zdefiniować metody get i set, aby uzyskiwać dostęp do szczegółów użytkowników w funkcji głównej.
 
   Utwórz klasę [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) w folderze `src/main/java/com/azure/cosmosdb/cassandra/` przy użyciu metod get i set. 

## <a name="add-a-database-and-a-table"></a>Dodawanie bazy danych i tabeli  

Ta sekcja zawiera opis sposobu dodawania bazy danych (przestrzeni kluczy) oraz tabeli za pomocą języka zapytań bazy danych Cassandra (CQL, Cassandra Query Language). Aby uzyskać informacje dotyczące składni języka CQL dla tych poleceń, zapoznaj się ze składnią zapytań [create keyspace](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) i [create table](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable). 

1. W folderze `src\main\java\com\azure\cosmosdb\cassandra` utwórz nowy folder o nazwie `repository`. 

2. Następnie utwórz klasę języka Java `UserRepository` i dodaj do niej następujący kod: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Znajdź folder `src\main\java\com\azure\cosmosdb\cassandra` i utwórz nowy podfolder o nazwie `examples`.

4. Następnie utwórz klasę języka Java `UserProfile`. Ta klasa zawiera metodę główną, która wywołuje zdefiniowane wcześniej metody createKeyspace i createTable: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Uruchamianie aplikacji 

1. Otwórz wiersz polecenia lub okno terminala. Wklej poniższy blok kodu. 

   Ten kod zmienia katalog (polecenie cd) na ścieżkę folderu, w której utworzono projekt. Następnie jest uruchamiane polecenie `mvn clean install` w celu wygenerowania pliku `cosmosdb-cassandra-examples.jar` w folderze docelowym. Na koniec kod uruchamia aplikację języka Java.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   W oknie terminalu zostaną wyświetlone powiadomienia o utworzeniu przestrzeni kluczy i tabeli. 
   
2. Teraz w witrynie Azure Portal otwórz **Eksplorator danych**, aby potwierdzić utworzenie przestrzeni kluczy i tabeli.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób tworzenia konta, bazy danych i tabeli interfejsu API Cassandra usługi Azure Cosmos DB przy użyciu aplikacji języka Java. Teraz możesz przejść do następnego artykułu:

> [!div class="nextstepaction"]
> [Ładowanie przykładowych danych do tabeli interfejsu API Cassandra](cassandra-api-load-data.md).
