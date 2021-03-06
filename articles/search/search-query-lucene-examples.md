---
title: Przykłady zapytań Lucene dla usługi Azure Search | Dokumentacja firmy Microsoft
description: Składnia zapytań Lucene dla Wyszukiwanie rozmyte, wyszukiwanie w sąsiedztwie, promowanie terminów, wyszukiwanie wyrażenia regularnego i wyszukiwania symboli wieloznacznych w usłudze Azure Search.
author: HeidiSteen
manager: cgronlun
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/09/2018
ms.author: heidist
ms.openlocfilehash: b5a3e2eac218ba2aa6958ffc56bd59f5b513cf48
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42061492"
---
# <a name="lucene-syntax-query-examples-for-building-advanced-queries-in-azure-search"></a>Przykłady zapytań składni Lucene do tworzenia zaawansowanych zapytań w usłudze Azure Search
Podczas tworzenia zapytań do usługi Azure Search, możesz zastąpić domyślną [prosty analizator zapytań](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) z udogodniona [analizator składni zapytań Lucene w usłudze Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) sformułowanie specjalistyczne i zaawansowane zapytania definicje. 

Analizator składni zapytań Lucene obsługuje konstrukcje złożonego zapytania, np. zapytania należące do pola, rozmyte i wyszukiwanie symbol wieloznaczny prefiksu, wyszukiwanie w sąsiedztwie, promowanie terminów i wyszukiwanie wyrażenia regularnego. Dodatkową moc jest dostarczany z wymagania dodatkowe przetwarzanie, dzięki czemu możesz spodziewać się nieco więcej czasu wykonywania. W tym artykule możesz przejrzeć przykłady pokazujące dostępne operacje zapytań, korzystając z pełnej składni.

> [!Note]
> Wiele konstrukcje specjalne zapytania włączane przy użyciu pełnej składni zapytań Lucene nie są [przeanalizowany tekst](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), które mogą być Zaskakujące, jeśli oczekujesz, wynikające lub Lematyzacja. Poddawać analizie leksykalnej jest realizowane wyłącznie na warunkach pełne (zapytanie termin lub frazę zapytania). Typy zapytań z warunkami niekompletne (prefiks zapytania zapytanie symboli wieloznacznych, zapytania wyrażenia regularnego, rozmyte zapytania) są dodawane bezpośrednio w drzewie zapytań, z pominięciem na etapie analizy. West jest tylko przekształcenie wykonywane na warunkach niekompletne zapytanie. 
>

## <a name="formulate-requests-in-postman"></a>Sformułowania żądań w narzędziu Postman

Poniższe przykłady korzystać z indeksu wyszukiwania Pokazowa składający się z dostępnych zadań na podstawie zestawu danych, dostarczone przez [miasta w Nowym Jorku OpenData](https://opendata.cityofnewyork.us/) inicjatywy. Te dane nie uważa się bieżących lub ukończone. Indeks znajduje się w piaskownicy usługi obsługiwane przez firmę Microsoft, co oznacza, że nie ma potrzeby subskrypcji platformy Azure lub usługi Azure Search, aby wypróbować te zapytania.

Konieczne jest Postman lub równoważne narzędzie do wystawiania żądań HTTP na GET. Aby uzyskać więcej informacji, zobacz [Eksploruj za pomocą klientów REST](search-fiddler.md).

### <a name="set-the-request-header"></a>Ustaw nagłówek żądania

1. W nagłówku żądania ustaw **Content-Type** do `application/json`.

2. Dodaj **klucz interfejsu api**i ustaw go na następujący ciąg: `252044BE3886FE4A8E3BAA4F595114BB`. Jest to klucz zapytania usługi wyszukiwania piaskownicy hostingu indeksu Pokazowa.

Po określeniu nagłówek żądania, można użyć ponownie go dla wszystkich zapytań w tym artykule wymienić tylko **wyszukiwania =** ciągu. 

  ![Nagłówek żądania narzędzia Postman](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>Ustaw adres URL żądania

Żądanie jest sparowana z adres URL usługi Azure Search punktu końcowego i wyszukaj ciąg zawierający polecenie GET.

  ![Nagłówek żądania narzędzia Postman](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

Kompozycja adresu URL zawiera następujące elementy:

+ **`https://azs-playground.search.windows.net/`** jest to usługa wyszukiwania w piaskownicy jest obsługiwana przez zespół usługi Azure Search. 
+ **`indexes/nycjobs/`** jest indeksem Pokazowa w kolekcji indeksów tej usługi. Nazwa usługi i indeksu są wymagane dla żądania.
+ **`docs`** to kolekcji documents zawierający całą zawartość można wyszukiwać. Klucz interfejsu api zapytań podany w nagłówku żądania działa tylko na operacje odczytu, wybieranie kolekcji dokumentów.
+ **`api-version=2017-11-11`** Ustawia wartość api-version, czyli wymaganego parametru na każde żądanie.
+ **`search=*`** jest ciągiem zapytania, które początkowego zapytania ma wartość null, zwraca 50 pierwszych wyników (domyślnie).

## <a name="send-your-first-query"></a>Wyślij pierwszego zapytania

Jako kroku weryfikacji, wklej następujące żądanie GET, a następnie kliknij przycisk **wysyłania**. Wyniki są zwracane jako pełne dokumenty JSON. Użytkownik może kopiowanie i wklejanie tego adresu URL w pierwszym przykładzie poniżej.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=*
  ```

Ciąg zapytania **`search=*`**, odpowiada nieokreślonego wyszukiwania wyszukiwania o wartości null ani być pusta. Nie jest to szczególnie przydatne, ale jest najprostszym wyszukiwania, które można wykonać.

Opcjonalnie można dodać **`$count=true`** do adresu URL, aby zwrócić liczbę dokumentów spełniających kryteria wyszukiwania. W ciągu wyszukiwania puste to wszystkie dokumenty w indeksie (około 2800 w przypadku Pokazowa).

## <a name="how-to-invoke-full-lucene-parsing"></a>Jak wywołać pełnej analizy Lucene

Dodaj **queryType = full** do wywołania składni pełne zapytanie w języku, Zastępowanie domyślnego prosta składnia zapytań. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&search=*
```

Wszystkie przykłady w niniejszym artykule określ **queryType = full** wyszukiwania parametr, wskazujący, że pełnej składni jest obsługiwane przez analizator składni zapytań Lucene. 

## <a name="example-1-field-scoped-query"></a>Przykład 1: Zakres pola zapytania

W pierwszym przykładzie nie jest specyficzne dla analizatora składni, ale możemy prowadzić z nim wprowadzenie pierwszego zapytanie podstawowe pojęcia: zawierania. W tym przykładzie zakresów, wykonywania zapytań i odpowiedzi na kilka konkretnych pól. Ważne jest wiedza, jak i struktury można odczytać odpowiedź w formacie JSON, po narzędzie Postman lub wyszukiwania Eksploratora. 

Celu skrócenia programu, zapytanie jest przeznaczona tylko *business_title* pola, a następnie określa tylko tytuły biznesowe są zwracane. Składnia jest **searchFields** ograniczyć wykonywanie zapytania do tylko pola business_title i **wybierz** można określić pola, które mają zostać uwzględnione w odpowiedzi.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

Odpowiedź dla tego zapytania powinien wyglądać podobnie do poniższej zrzut ekranu.

  ![Postman przykładowa odpowiedź](media/search-query-lucene-examples/postman-sample-results.png)

Być może Zauważyłeś, wynik wyszukiwania w odpowiedzi. Jednolite wyniki 1 wystąpić, gdy jest nie rangę, albo ponieważ wyszukiwanie pełnotekstowe nie jest wyszukiwanie lub ponieważ żadne kryteria nie została zastosowana. Dla wartości null wyszukiwania przy użyciu kryteriów wiersze wrócić w dowolnej kolejności. Umieszczonego rzeczywiste kryteria zobaczysz wyniki ewoluować w istotne wartości wyszukiwania.

## <a name="example-2-intra-field-filtering"></a>Przykład 2: Filtrowanie w obrębie pola

Pełna składnia Lucene obsługuje wyrażenia w obrębie pola. To zapytanie wyszukuje tytuły firm z starszy termin w nich, ale nie młodszych:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:senior+NOT+junior
```

  ![Postman przykładowa odpowiedź](media/search-query-lucene-examples/intrafieldfilter.png)

Określając **fieldname:searchterm** konstrukcji, można zdefiniować operacji fielded zapytania, gdzie pole jest pojedynczego wyrazu, a termin wyszukiwania jest również pojedynczego wyrazu lub frazy, opcjonalnie wraz z operatorami logicznymi. Oto kilka przykładów:

* business_title:(senior NOT junior)
* Stan: ("New York" i "Nowe Jersey")

Pamiętaj umieścić wielu ciągów w cudzysłowach, jeśli chcesz, aby oba ciągi, które ma zostać obliczone jako pojedynczą jednostkę, jak w tym przypadku wyszukiwania dla dwóch różnych miast w polu lokalizacji. Upewnij się również, operator jest wielką literą, jak widać, z użyciem NOT i AND.

Pole określone w **fieldname:searchterm** musi być polu możliwym do przeszukania. Zobacz [Create Index (Azure Search Service interfejs API REST)](https://docs.microsoft.com/rest/api/searchservice/create-index) szczegółowe informacje na temat używania atrybuty indeksu w definicji pola.

## <a name="example-3-fuzzy-search"></a>Przykład 3: Wyszukiwanie rozmyte

Pełna składnia Lucene obsługuje również Wyszukiwanie rozmyte dopasowywanie na zasadach, które mają podobne konstrukcji. Aby wykonać wyszukiwanie rozmyte, należy dołączyć tylda `~` symbolu na końcu pojedynczego wyrazu z opcjonalnym parametrem, wartość z zakresu od 0 do 2, określającą odległość edycji. Na przykład `blue~` lub `blue~1` zwróci niebieski, blues i przyklej.

To zapytanie wyszukuje zadania za pomocą termin "Skojarz" (celowo błędna):

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:asosiate~
```
  ![Wyszukiwanie rozmyte odpowiedzi](media/search-query-lucene-examples/fuzzysearch.png)

Na [dokumentacji Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), Wyszukiwanie rozmyte opierają się na [odległość Damerau Levenshtein](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

> [!Note]
> Rozmyte zapytania nie są [analizowane](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Typy zapytań z warunkami niekompletne (prefiks zapytania zapytanie symboli wieloznacznych, zapytania wyrażenia regularnego, rozmyte zapytania) są dodawane bezpośrednio w drzewie zapytań, z pominięciem na etapie analizy. West jest tylko przekształcenie wykonywane na warunkach niekompletne zapytanie.
>

## <a name="example-4-proximity-search"></a>Przykład 4: Wyszukiwanie w sąsiedztwie
Wyszukiwanie w sąsiedztwie służą do znajdowania warunki znajdujących się blisko siebie w dokumencie. Wstaw tyldy "~" symbolu na końcu frazy następuje liczbę słów, które Utwórz granicę odległości między elementami. Na przykład, "port lotniczy hotel" ~ 5 znajdzie hotelu warunki i port lotniczy w ciągu 5 słów od siebie w dokumencie.

W tym zapytaniu dla zadań z terminem "analityka starszy" gdzie rozdzielone nie więcej niż jeden wyraz:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~1
```
  ![Zapytanie odległości między elementami](media/search-query-lucene-examples/proximity-before.png)

Wypróbuj ją ponownie usuwanie wyrazów między termin "starszy analityka". Należy zauważyć, że dokumenty 8 są zwracane dla tego zapytania, w przeciwieństwie do 10 poprzednie zapytanie.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~0
```

## <a name="example-5-term-boosting"></a>Przykład 5: Promowanie terminów
Promowanie terminów odnosi się do dokumentu większe, jeśli zawiera on wzmocnione termin względem dokumentów, które nie zawierają termin klasyfikacji. Zwiększ termin, użyj daszek, "^", symbol ze współczynnikiem boost (liczba) na końcu okresu, w przypadku wyszukiwania. 

W tym "przed" zapytania wyszukiwania zadań z terminem *analityka komputera* i zwróć uwagę, Brak wyników z obu wyrazów *komputera* i *analityka*, jeszcze  *komputer* zadań znajdują się na początku wyników.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst
```
  ![Promowanie przed](media/search-query-lucene-examples/termboostingbefore.png)

W zapytaniu "po", powtórz wyszukiwanie, tym razem zwiększania wyniku wyniki z terminem *analityka* przez okres *komputera* oba te wyrazy nie istnieją. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst%5e2
```
Bardziej ludzki można odczytać wersji powyższym zapytaniu jest `search=business_title:computer analyst^2`. Dla zapytania wymagającego `^2` zakodowanymi w formacie `%5E2`, które jest trudniejsze, aby zobaczyć.

  ![Promowanie po](media/search-query-lucene-examples/termboostingafter.png)

Promowanie terminów różni się od profile oceniania, w tym profile oceniania zwiększania niektóre pola, a nie konkretne terminy. Poniższy przykład pomoże zilustrować różnice.

Należy wziąć pod uwagę profil oceniania, co zwiększa przywiązanie podanej w niektóre pola, takie jak **gatunku** w przykładzie musicstoreindex. Promowanie terminów może służyć do dalszych Zwiększ niektórych wyszukiwania wyższe niż inne postanowienia. Na przykład "rock ^ 2 elektronicznych" spowoduje zwiększenie dokumentów, które zawierają terminy wyszukiwania w **gatunku** pola wyższa niż inne pola z możliwością wyszukiwania w indeksie. Ponadto dokumenty zawierające frazę "rock" zostanie wyznaczona ranga wyższe niż inne wyszukiwany termin "elektronicznego" w wyniku wartość boost termin (2).

Po ustawienie poziomie współczynnik wyższa współczynnika zwiększenie wydajności, tym większe znaczenie termin będzie względem innych terminy wyszukiwania. Domyślnie współczynnik boost wynosi 1. Chociaż współczynnik boost musi być dodatnią, może być mniejsza niż 1 (na przykład 0.2).


## <a name="example-6-regex"></a>Przykład 6: wyrażenie regularne

Wyszukiwanie wyrażenia regularnego znalezienia dopasowania na podstawie zawartości między ukośnikami "/", zgodnie z opisem w [klasy RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

W tym zapytaniu wyszukiwania zadań z termin starszy lub inny poziom: "search = business_title:/(Sen| Ior cze) / ".

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:/(Sen|Jun)ior/
```

  ![Wyrażenie regularne zapytania](media/search-query-lucene-examples/regex.png)

> [!Note]
> Wyrażenie regularne zapytania nie są [analizowane](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). West jest tylko przekształcenie wykonywane na warunkach niekompletne zapytanie.
>

## <a name="example-7-wildcard-search"></a>Przykład 7: Wyszukiwanie symboli wieloznacznych
Możesz użyć składni powszechnie wielu (\*) lub pojedynczego wyszukiwania symboli wieloznacznych znaku (?). Należy pamiętać, że analizator składni zapytań Lucene obsługuje korzystanie z tych symboli za pomocą pojedynczego terminu i nie frazę.

W tym zapytaniu wyszukiwania zadań, które zawierają prefiks programu, który zamieści tytuły firm z warunkami programowania programisty w nim. Nie można użyć * lub? symbol jako pierwszego znaku wyszukiwania.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:prog*
```
  ![Symbol wieloznaczny zapytania](media/search-query-lucene-examples/wildcard.png)

> [!Note]
> Symbol wieloznaczny zapytania nie są [analizowane](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). West jest tylko przekształcenie wykonywane na warunkach niekompletne zapytanie.
>

## <a name="next-steps"></a>Kolejne kroki
Spróbuj określić analizatora zapytań Lucene w kodzie. Poniższe linki wyjaśniono, jak skonfigurować zapytania wyszukiwania dla środowiska .NET i interfejsu API REST. Łączy należy użyć składni proste domyślnego, konieczne będzie Zastosuj zdobytą wiedzę z tego artykułu, aby określić **queryType**.

* [Zapytanie indeksu usługi Azure Search przy użyciu zestawu .NET SDK](search-query-dotnet.md)
* [Zapytanie indeksu usługi Azure Search przy użyciu interfejsu API REST](search-query-rest-api.md)

Odwołanie do dodatkowej składni, architektura zapytania i przykłady można znaleźć w następujących łączy:

+ [Przykłady zapytań składni prosty](search-query-simple-examples.md)
+ [Jak działa wyszukiwanie pełnotekstowe w usłudze Azure Search](search-lucene-query-architecture.md)
+ [Prosta składnia zapytań](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Pełna składnia zapytań Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)