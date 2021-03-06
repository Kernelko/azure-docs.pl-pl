---
title: Dokumentacja dla deweloperów języka Java dla usługi Azure Functions | Dokumentacja firmy Microsoft
description: Zrozumienie, jak tworzyć funkcje za pomocą języka Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bezserwerowa, języka java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: 423661b8a459abf0b3028da92d6fd3ec885bb2c9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025026"
---
# <a name="azure-functions-java-developer-guide"></a>Przewodnik dla deweloperów w usłudze Azure Functions Java

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Model programowania 

Funkcji platformy Azure powinna być metoda bezstanowe klasy, która przetwarza dane wejściowe i generuje dane wyjściowe. Mimo że można napisać metody wystąpienia, funkcja nie może zależeć od wszystkie pola wystąpienia klasy. Wszystkie metody funkcji musi mieć `public` modyfikator dostępu.

## <a name="folder-structure"></a>Struktura folderów

Struktura folderów dla projektu Java wygląda podobnie do poniższego:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Brak udostępnionej [host.json](functions-host-json.md) pliku, który może służyć do konfigurowania aplikacji funkcji. Każda funkcja ma swój własny plik kodu (początku) i plik konfiguracji powiązania (function.json).

Można umieścić więcej niż jedną funkcję w projekcie. Należy unikać umieszczenie funkcji w oddzielnych plikach JAR. FunctionApp w katalogu docelowym jest o tym, co zostanie wdrożona do aplikacji funkcji na platformie Azure.

## <a name="triggers-and-annotations"></a>Wyzwalacze i adnotacje

 Usługa Azure functions są wywoływane przez wyzwalacz, takie jak żądania HTTP, czasomierz lub aktualizacji danych. Funkcja musi przetworzyć tego wyzwalacza i inne dane wejściowe do wyprodukowania jednego lub więcej danych wyjściowych.

Korzystanie z adnotacji Java objęte [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) pakietu, aby powiązać dane wejściowe i wyjściowe metody. Przykładowy kod przy użyciu adnotacji jest dostępna w [dokumenty referencyjne języka Java](/java/api/com.microsoft.azure.functions.annotation) każdej adnotacji, w dokumentacji usługi Azure Functions powiązania odniesienia, takiego jak dla [wyzwalaczy HTTP](/azure/azure-functions/functions-bindings-http-webhook).

Dane wejściowe wyzwalacza i dane wyjściowe można także definiować w [function.json](/azure/azure-functions/functions-reference#function-code) funkcji zamiast za pomocą adnotacji. Za pomocą `function.json` zamiast adnotacje w ten sposób nie jest zalecane.

> [!IMPORTANT] 
> Należy skonfigurować konto usługi Azure Storage w swojej [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) lokalnie uruchomić wyzwalacze usługi Azure Storage Blob, kolejki lub tabeli.

Przykład korzystanie z adnotacji:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Tę samą funkcję zapisywane bez adnotacji:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

za pomocą odpowiednich `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>Zestaw JDK środowiska uruchomieniowego dostępność i pomoc techniczna 

Pobieranie i używanie [Azul Zulu dla platformy Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) JDK z [firmy Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) potrzeby lokalnego programowania aplikacji funkcji w języku Java. JDK są dostępne dla Windows, Linux i macOS oraz [pomocy technicznej platformy Azure](https://support.microsoft.com/en-us/help/4026305/sql-contact-microsoft-azure-support) jest dostępna dla problemów napotykanych podczas programowania przy użyciu [plan pomocy technicznej kwalifikowaną](https://azure.microsoft.com/support/plans/).

## <a name="third-party-libraries"></a>Bibliotek innych firm 

Usługa Azure Functions obsługuje korzystanie z bibliotek innych firm. Domyślnie wszystkie zależności są określone w projekcie `pom.xml` pliku zostaną automatycznie dołączone podczas `mvn package` cel. W przypadku bibliotek, które nie jest określony jako zależności w `pom.xml` pliku, umieść je w `lib` katalogu w katalogu głównym funkcji. Zależności są umieszczane w `lib` katalog zostanie dodany do modułu ładującego klasę systemu w czasie wykonywania.

`com.microsoft.azure.functions:azure-functions-java-library` Zależności znajduje się na ścieżce klasy domyślnie i nie musi być objęte `lib` katalogu.

## <a name="data-type-support"></a>Obsługa typu danych

Wszystkie typy danych w języku Java można użyć dla danych wejściowych i wyjściowych, w tym natywnych typów; dostosowane typy języka Java i wyspecjalizowane Azure typów zdefiniowanych w `azure-functions-java-library` pakietu. Usługi Azure Functions, który próbuje środowiska uruchomieniowego skonwertować danych wejściowych, odebranych na typ żądanego przez kod.

### <a name="strings"></a>Ciągi

Wartości przekazane do funkcji metod będzie rzutowane na ciągi, jeśli odpowiedni typ parametru wejściowego dla tej funkcji jest typu `String`. 

### <a name="plain-old-java-objects-pojos"></a>Zwykłych starych obiektów Java (Pojo)

Ciągi sformatowane przy użyciu formatu JSON będzie być rzutowane na typy języka Java, jeśli podpis wejściowy funkcji oczekuje typu języka Java. Ta konwersja umożliwia przekazywanie w formacie JSON i pracują z typami środowiska Java.

Obiektu typu POJO typy używane jako dane wejściowe do funkcji musi takie same `public` modyfikator dostępu metody funkcji, są one używane w. Nie trzeba zadeklarować obiektu typu POJO pola klasy `public`. Na przykład ciąg JSON `{ "x": 3 }` jest w stanie ma zostać przekonwertowane na następujący typ obiektu typu POJO:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Dane binarne

Dane binarne, jest przedstawiana jako `byte[]` w kodzie funkcji platformy Azure. Powiązywanie binarnych danych wejściowych lub wyjściowych funkcji, ustawiając `dataType` w swojej function.json do `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Następnie użyj go w kodzie funkcji:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Może być puste wartości wejściowe `null` jako argument funkcji, ale zalecany sposób radzenia sobie z potencjał jest użycie wartości puste `Optional<T>`.


## <a name="function-method-overloading"></a>Przeciążenie metody — funkcja

Możesz przeciążyć metody funkcji o tej samej nazwie, ale z różnymi typami. Na przykład, może mieć jednocześnie `String echo(String s)` i `String echo(MyType s)` w klasie. Usługa Azure Functions decyduje, jakiej metody do wywołania na podstawie danych wejściowych typu (dla danych wejściowych, typ MIME HTTP `text/plain` prowadzi do `String` podczas `application/json` reprezentuje `MyType`).

## <a name="inputs"></a>Dane wejściowe

Dane wejściowe są podzielone na dwie kategorie w usłudze Azure Functions: jeden z nich to dane wejściowe wyzwalacza, a drugi to dodatkowe dane wejściowe. Mimo że są inne w przypadku `function.json`, użycie jest identyczna w kodzie języka Java. Jako przykład Weźmy poniższy fragment kodu:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Gdy ta funkcja jest wyzwalana, żądanie HTTP jest przekazywany do funkcji przez `String in`. Wpis zostanie pobrany z usługi Azure Table Storage, na podstawie Identyfikatora w adresie URL trasy i wprowadziliśmy dostępne jako `obj` w treści funkcji.

## <a name="outputs"></a>Dane wyjściowe

Dane wyjściowe mogą być wyrażone zarówno wartości zwracane lub parametry wyjściowe. Jeśli istnieje tylko jedno wyjście, są zalecane do użycia wartości zwracanej. Wiele wyjść należy użyć parametrów wyjściowych.

Wartość zwracana jest najprostsza forma dane wyjściowe, po prostu zwraca wartość dowolnego typu i środowisko uruchomieniowe usługi Azure Functions będzie próbował kierować je do rzeczywistego typu (na przykład odpowiedź HTTP).  Można zastosować adnotacji w danych wyjściowych metody — funkcja (właściwości name obiektu adnotacji musi być $return) do definiowania danych wyjściowych wartość zwracaną.

Aby utworzyć wiele wartości danych wyjściowych, należy użyć `OutputBinding<T>` typ zdefiniowany w elemencie `azure-functions-java-library` pakietu. Jeśli musisz wprowadzić odpowiedź HTTP i wypychania komunikat do kolejki, a także można napisać mniej więcej tak:

Na przykład zawartość obiektu blob, kopiowanie funkcji można zdefiniować jako następujący kod. `@StorageAccount` Adnotacja służy tutaj, aby uniknąć duplikowania właściwości połączenia dla obu `@BlobTrigger` i `@BlobOutput`.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Użyj `OutputBinding<byte[]`> Aby dane binarne dane wyjściowe wartość (parametry); wartości zwracane, wystarczy użyć `byte[]`.

## <a name="specialized-types"></a>Typy specjalne

Czasami funkcję muszą szczegółowe kontrolę nad dane wejściowe i wyjściowe. Wyspecjalizowane typów w `azure-functions-java-core` pakietu są dostarczane do manipulowania informacje dotyczące żądania i dostosować zwracany stan wyzwalacza HTTP:

| Specjalistyczną odmianą      |       Środowisko docelowe        | Typowy                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Wyzwalacz HTTP     | Metoda, nagłówków lub zapytania |
| `HttpResponseMessage<T>` | Powiązanie danych wyjściowych HTTP | Status powrotu niż 200   |

> [!NOTE] 
> Można również użyć `@BindingName` adnotacji można pobrać nagłówków HTTP i zapytań. Na przykład `@BindingName("name") String query` dokonuje iteracji nagłówków żądań HTTP i zapytania i przekaż tę wartość do metody. Na przykład `query` będzie `"test"` Jeśli adres URL żądania jest `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metadane

Metadane pochodzą z różnych źródeł, takich jak nagłówki HTTP, kwerendy HTTP i [wyzwolić metadanych](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Użyj `@BindingName` adnotacji wraz z nazwą metadanych w celu uzyskania wartości.

Na przykład `queryValue` w poniższym kodzie będzie fragment `"test"` Jeśli żądany adres URL jest `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="execution-context"></a>Kontekst wykonywania

Wchodzić w interakcje ze środowiskiem wykonywania usługi Azure Functions za pomocą `ExecutionContext` obiektu zdefiniowany w `azure-functions-java-library` pakietu. Użyj `ExecutionContext` obiekt, aby użyć informacji o wywołania i środowisko uruchomieniowe funkcji w kodzie.

### <a name="custom-logging"></a>Funkcja Rejestrowanie niestandardowe

Dostęp do funkcji rejestrowania środowiska uruchomieniowego jest dostępna za pośrednictwem `ExecutionContext` obiektu. Tego rejestratora jest powiązany z usługi Azure monitor i pozwala flagi ostrzeżeń i błędów napotkanych podczas wykonywania funkcji.

Poniższy przykład kodu rejestruje komunikat ostrzegawczy, gdy Odebrano treść żądania jest pusta.

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Wyświetlanie dzienników i śledzenia

Można użyć wiersza polecenia platformy Azure do strumienia Java standard out i rejestrowania błędów, a także rejestrowanie innych aplikacji. Najpierw należy skonfigurować aplikację funkcji do zapisania rejestrowania aplikacji przy użyciu wiersza polecenia platformy Azure:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Przesyłanie strumieniowe danych wyjściowych rejestrowania dla aplikacji funkcji przy użyciu wiersza polecenia platformy Azure, otwórz nowy wiersz polecenia, Bash lub sesji terminalowej i wprowadź następujące polecenie:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
[Az webapp log tail](/cli/azure/webapp/log) polecenie ma funkcje umożliwiające filtrowanie danych wyjściowych przy użyciu `--provider` opcji. 

Aby pobrać pliki dziennika jako pojedynczy plik ZIP, przy użyciu wiersza polecenia platformy Azure, otwórz nowy wiersz polecenia, Bash lub sesji terminalowej i wprowadź następujące polecenie:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Musisz włączyć systemu plików, logowania w witrynie Azure Portal lub interfejsu wiersza polecenia platformy Azure, przed uruchomieniem tego polecenia.

## <a name="environment-variables"></a>Zmienne środowiskowe

Zachowaj informacje poufne, takie jak kluczy lub tokenów z kodu źródłowego ze względów bezpieczeństwa. Użyj klawiszy i tokenów w kodzie funkcji, odczytując je ze zmiennych środowiskowych.

Do ustawiania zmiennych środowiskowych podczas uruchamiania usługi Azure Functions lokalnie, można dodać te zmienne do pliku local.settings.json. Jeśli nie jest obecny w katalogu głównym projektu funkcji, można go utworzyć. Oto jak powinien wyglądać plik:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Każdy klucz-wartość mapowanie w `values` mapy zostaną udostępnione w czasie wykonywania jako zmienną środowiskową, dostępna poprzez wywołanie `System.getenv("<keyname>")`, na przykład `System.getenv("AzureWebJobsStorage")`. Dodawanie dodatkowy klucz / wartość pary jest akceptowane i zalecana praktyka.

> [!NOTE]
> Jeśli takie podejście jest zajęta, można się, że można dodać local.settings.json pliku do repozytorium zignorować pliku, tak, aby nie jest zatwierdzona.

W kodzie teraz w zależności od tych zmiennych środowiskowych można logowaniu do portalu Azure, aby ustawić ten sam klucz / wartość pary w ustawieniach aplikacji funkcji, aby kod ekwiwalentnie podczas testowania lokalnie, jak i podczas wdrażania na platformie Azure.

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat tworzenia funkcji platformy Azure w języku Java zobacz następujące zasoby:

* [Najlepsze rozwiązania dotyczące usługi Azure Functions](functions-best-practices.md)
* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)
* [Wyzwalacze w usłudze Azure Functions i powiązania](functions-triggers-bindings.md)
- Lokalne programowanie i debugowanie za pomocą [programu Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md), i [Eclipse](functions-create-maven-eclipse.md). 
* [Zdalne debugowanie środowiska Java w usłudze Azure Functions przy użyciu programu Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Wtyczka maven plugin for Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) -Usprawnij Tworzenie funkcji przy użyciu `azure-functions:add` cel i przygotowywanie katalogu przemieszczania dla [wdrażanie plików ZIP](deployment-zip-push.md).
