---
title: Tworzenie indeksu usługi Azure Search w portalu | Dokumentacja firmy Microsoft
description: Dowiedz się, jak utworzyć indeks usługi Azure Search przy użyciu wbudowanych indeks portalu projektantów.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: bb1ba5e860dab237b3f6e16205b5e4cbad45e6e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990850"
---
# <a name="how-to-create-an-azure-search-index-using-the-azure-portal"></a>Tworzenie indeksu usługi Azure Search przy użyciu witryny Azure portal

Usługa Azure Search zawiera indeks wbudowanego projektanta w portalu jest przydatne w przypadku prototypy lub tworzenia [indeksu wyszukiwania](search-what-is-an-index.md) hostowanych w usłudze Azure Search. To narzędzie służy do tworzenia schematów. Po zapisaniu definicji pusty indeks staje się całkowicie wyrażone w usłudze Azure Search. Jak załadować je przy użyciu danych z możliwością wyszukiwania zależy od użytkownika.

Projektant indeksu jest tylko jedno podejście do tworzenia indeksu. Programowe tworzenie indeksu przy użyciu [.NET](search-create-index-dotnet.md) lub [REST](search-create-index-rest-api.md) interfejsów API.

## <a name="prerequisites"></a>Wymagania wstępne

Ten artykuł zakłada, że użytkownik posiada [subskrypcję platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) i [usługę Azure Search](search-create-service-portal.md).

## <a name="open-index-designer-and-name-an-index"></a>Otwórz projektanta indeksów i nadaj nazwę indeksu

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) i otwórz pulpit nawigacyjny usługi. Możesz kliknąć pozycję **Wszystkie usługi** na pasku dostępu, aby poszukać istniejących usług wyszukiwania w bieżącej subskrypcji. 

2.  Kliknij przycisk **Dodaj indeks** na pasku poleceń u góry strony.

3. Nadaj nazwę indeksowi usługi Azure Search. Operacje indeksowania i zapytanie odwołuje się ich nazw. Nazwa indeksu staje się częścią adresu URL punktu końcowego używanego w przypadku połączeń z indeksem i wysyłania żądań HTTP za pomocą interfejsu API REST usługi Azure Search.

   * Wpisywanie zacznij od litery.
   * Użyj tylko małych liter, cyfr i łączników („-”).
   * Ogranicz długość nazwy do 60 znaków.

## <a name="define-the-fields-of-your-index"></a>Definiowanie pól indeksu

Kompozycja indeksu obejmuje *kolekcję pól*, która definiuje dane z możliwością wyszukiwania w indeksie. Cała kolekcji pól określa strukturę dokumentów, które są przekazywane oddzielnie. Kolekcja pól zawiera wymagane i opcjonalne pola nazwane i wpisane z atrybutami indeksu, które określają, jak można używać pola.

1. W bloku **Dodawanie indeksu** kliknij pozycję **Pola**, aby wysunąć blok definicji pola. 

2. Zaakceptuj wygenerowane pole *klucza* typu Edm.String. Domyślnie pole nosi nazwę *id*, ale można ją zmienić, o ile ciąg spełnia [reguły nazewnictwa](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Pole klucza jest wymagane dla każdego indeksu usługi Azure Search i musi mieć postać ciągu.

3. Dodaj pola, aby w pełni określić dokumenty, które chcesz przekazać. Jeśli dokumenty składają się z pól *id*, *nazwa hotelu*, *adres*, *miasto* i *region*, utwórz odpowiednie pole dla każdego z nich w indeksie. Zapoznaj się ze [wskazówkami projektowania zawartymi w poniższej sekcji](#design), aby uzyskać pomoc podczas ustawiania atrybutów.

4. Opcjonalnie dodaj dowolne pola, które są używane wewnętrznie w wyrażeniach filtru. Atrybuty pola można ustawić w taki sposób, aby wykluczały pola z operacji wyszukiwania.

5. Po zakończeniu kliknij przycisk **OK**, aby zapisać i utworzyć indeks.

## <a name="tips-for-adding-fields"></a>Porady dotyczące dodawania pól

Tworzenie indeksu w portalu jest czasochłonne. Aby maksymalnie ograniczyć liczbę wykonywanych kroków, postępuj według następującego przepływu pracy:

1. Najpierw utwórz listę pól, wprowadzając nazwy i ustawienia typów danych.

2. Użyj pól wyboru w górnej części każdego atrybutu w celu zbiorczego włączenia ustawień dla wszystkich pól, a następnie wyczyść kilka wybranych pól wyboru, które tego nie wymagają. Na przykład pola ciągów zazwyczaj mogą być wyszukiwane. W związku z tym możesz kliknąć pozycje **Możliwość pobierania** i **Możliwość wyszukiwania**, aby zwrócić wartości pól w wynikach wyszukiwania, a także umożliwić wyszukiwanie pełnotekstowe pola. 

<a name="design"></a>

## <a name="design-guidance-for-setting-attributes"></a>Wskazówki dotyczące projektowania ustawień atrybutów

Chociaż możesz w dowolnym momencie dodać nowe pola, istniejące definicje pól są zablokowane przez cały czas istnienia indeksu. Z tego powodu deweloperzy zazwyczaj używają portalu do tworzenia prostych indeksów, testowania pomysłów lub używania stron portalu w celu wyszukania ustawień. Częsta iteracja po projekcie indeksu jest bardziej wydajna, jeśli stosujesz podejście oparte na kodzie, które pozwala na odbudowanie indeksu w prosty sposób.

Analizatory i sugestory są kojarzone z polami przed zapisaniem indeksu. Aby dodać analizatory lub sugestory języka do definicji indeksu, należy kliknąć każdą stronę z kartami.

Pola ciągów są często oznaczone jako **Możliwość sortowania** i **Możliwość pobierania**.

Pola używane w celu zawężenia wyników wyszukiwania zawierają oznaczenia **Możliwość sortowania**, **Możliwość filtrowania** i **Możliwość tworzenia aspektów**.

Atrybuty pól określają, jak używane jest dane pole, np. czy jest używane w wyszukiwaniu pełnotekstowym, nawigacji aspektowej, operacjach sortowania i tak dalej. Następująca tabela zawiera opis każdego atrybutu.

|Atrybut|Opis|  
|---------------|-----------------|  
|**Wyszukiwanie**|Wyszukiwanie pełnotekstowe, które można poddawać analizie leksykalnej, takiej jak dzielenie wyrazów podczas indeksowania. Ustawienie wartości „sunny day” dla pola z możliwością wyszukiwania spowoduje podział na dwa oddzielne tokeny „sunny” i „day”. Aby uzyskać więcej informacji, zobacz [Jak działa wyszukiwanie pełnotekstowe](search-lucene-query-architecture.md).|  
|**Filtrowanie**|Odwołania do tego atrybutu znajdują się w zapytaniach **$filter**. Pola z możliwością filtrowania typu `Edm.String` lub `Collection(Edm.String)` nie są poddawane dzieleniu wyrazów, więc porównania dotyczą tylko dokładnych dopasowań. Ustawienie pola filtrowania na wartość „sunny day” spowoduje, że pole typu `$filter=f eq 'sunny'` nie wyświetli żadnych wyników, ale `$filter=f eq 'sunny day'` już tak. |  
|**Sortowanie**|Domyślnie system sortuje pozycje według wyników, ale można również ustawić sortowanie według poszczególnych pól w dokumentach. Pola typu `Collection(Edm.String)` nie mogą być **sortowane**. |  
|**Tworzenie aspektów**|Zwykle używany w prezentacji wyników wyszukiwania, która zawiera liczbę trafień według kategorii (na przykład hotele znajdujące się w określonym mieście). Nie można używać tej opcji z polami typu `Edm.GeographyPoint`. Pola typu `Edm.String` z możliwością **filtrowania**, **sortowania** lub **tworzenia aspektów** mogą mieć maksymalnie 32 kilobajty długości. Aby uzyskać więcej informacji, zobacz [Create Index (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Tworzenie indeksu [interfejs REST API]).|  
|**Klucz**|Unikatowy identyfikator dokumentów w indeksie. Można wybrać tylko jedno pole klucza i musi ono być typu `Edm.String`.|  
|**Pobieranie**|Określa, czy pole może być zwracane w wynikach wyszukiwania. Jest przydatny, jeśli chcesz użyć pola (np. *marża zysku*) z możliwością filtrowania, sortowania lub oceniania, ale nie chcesz, aby to pole było widoczne dla użytkownika końcowego. Ten atrybut musi przyjmować wartość `true` dla pól typu `key`.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Tworzenie indeksu „hotele” używanego w przykładowych sekcjach dotyczących interfejsu API

Dokumentacja interfejsu API usługi Azure Search zawiera przykłady kodu korzystającego z prostego indeksu *hotele*. Na poniższych zrzutach ekranu przedstawiono definicję indeksu, w tym analizator języka francuskiego określony podczas definiowania indeksu, który można odtworzyć jako ćwiczenie praktyczne w portalu.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Kolejne kroki

Po utworzeniu indeksu usługi Azure Search można przejść do następnego kroku: [przekazywanie danych z możliwością wyszukiwania do indeksu](search-what-is-data-import.md).

Można również dowiedzieć się więcej na temat indeksów. Oprócz kolekcji pól indeks określa również analizatory, sugestory, profile oceniania i ustawienia specyfikacji CORS. Portal zawiera strony z kartami umożliwiające definiowanie najczęściej występujących elementów: pól, analizatorów i sugestorów. Aby utworzyć lub zmodyfikować inne elementy, można użyć interfejsu API REST lub zestawu .NET SDK.

## <a name="see-also"></a>Zobacz także

 [Jak działa wyszukiwanie pełnotekstowe](search-lucene-query-architecture.md)  
 [Interfejs API REST usługi Search](https://docs.microsoft.com/rest/api/searchservice/) [Zestaw .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

