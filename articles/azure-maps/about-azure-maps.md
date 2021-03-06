---
title: Omówienie usługi Azure Maps | Microsoft Docs
description: Wprowadzenie do usługi Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 09/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 4db494de87993a6434eaf5dbf441094578d72ea4
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165968"
---
# <a name="what-is-azure-maps"></a>Co to jest usługa Azure Maps?

Azure Maps to zbiór usług geoprzestrzennych opartych na aktualnych danych mapowych, zapewniający dokładny kontekst geograficzny do użycia w aplikacjach internetowych i mobilnych. Obejmuje on interfejsy API REST do renderowania map i wyszukiwania punktów orientacyjnych. Interfejsy API umożliwiają także znalezienie tras do punktów orientacyjnych, określenie warunków ruchu drogowego, stref czasowych i lokalizacji na podstawie adresu IP. Interfejsy API działają ze znanymi narzędziami, co umożliwia szybkie tworzenie i skalowanie rozwiązań, które integrują informacje o lokalizacji w rozwiązaniach platformy Azure. Oprócz interfejsów API REST jest dostępna internetowa [kontrolka mapy dla języka JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control), która sprawia, że opracowywanie jest łatwe, elastyczne i przenośne między wieloma nośnikami.

W poniższym filmie wideo wyjaśniono dokładnie działanie usługi Azure Maps:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kontrolki mapy

### <a name="web-control"></a>Kontrolka internetowa

Kontrolka internetowa usługi Azure Maps umożliwia dostosowywanie interaktywnych map przy użyciu własnych treści i obrazów na potrzeby wyświetlania w aplikacjach internetowych i mobilnych. Kontrolka korzysta z technologii WebGL, co umożliwia renderowanie dużych zestawów danych z wysoką wydajnością. Do programowania z użyciem tej kontrolki można użyć języka JavaScript lub TypeScript.

![Kontrolka internetowa usługi Azure Maps](media/about-azure-maps/Introduction_WebMapControl.png)

## <a name="services-in-azure-maps"></a>Usługi Azure Maps

Usługa Azure Maps składa się z następujących sześciu usług zapewniających kontekst geograficzny aplikacjom platformy Azure.

### <a name="render-service"></a>Render Service

Usługa Render Service umożliwia deweloperom tworzenie aplikacji internetowych i mobilnych związanych z mapowaniem. Usługa używa rastrowych obrazów graficznych o wysokiej jakości — dostępnych z 19 poziomami powiększenia — lub w pełni konfigurowalnych obrazów map w formacie wektorowym.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

Usługa Render Service oferuje teraz interfejsy API w wersji zapoznawczej umożliwiające deweloperom pracę z obrazami satelitarnymi. Aby uzyskać więcej informacji, przeczytaj artykuł [Azure Maps Render APIs (Interfejsy API usługi Render w usłudze Azure Maps)](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Route Service

Usługa Route Service zawiera niezawodne obliczenia geometryczne rzeczywistej infrastruktury oraz tras dla wielu środków transportu. Usługa pozwala deweloperom obliczać trasy dla wielu sposobów przemieszczania się, takich jak samochód osobowy, ciężarówka, rower czy wędrówka. Usługa może również brać pod uwagę dane wejściowe, takie jak warunki na drogach, ograniczenia co do masy pojazdu lub transport materiałów niebezpiecznych.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Usługa Route Service oferuje teraz zaawansowane funkcje w wersji zapoznawczej, takie jak przetwarzanie wsadowe wielu żądań tras, macierzy czasu podróży i odległości między zestawem punktów początkowych i końcowych, a także odnajdowanie tras lub odległości, które można pokonać w określonym czasie lub z określoną ilością paliwa. Aby uzyskać szczegółowe informacje dotyczące możliwości wyznaczania trasy, przeczytaj [Azure Maps Route APIs (Interfejsy API usługi Route w usłudze Azure Maps)](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Search Service

Usługa Search Service umożliwia deweloperom wyszukiwanie adresów, miejsc, firm według nazwy lub kategorii, a także innych informacji geograficznych. Usługa Search Service umożliwia też [odwracanie adresów geokodowania](https://en.wikipedia.org/wiki/Reverse_geocoding) i przechodzenie przez ulice na podstawie długości i szerokości geograficznej.

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Usługa Search Service umożliwia również dostęp do zaawansowanych funkcji, takich jak wyszukiwanie wzdłuż trasy, wyszukiwanie w ramach większego obszaru, grupowanie żądań wyszukiwania oraz wyszukiwanie większego obszaru zamiast konkretnego punktu. Interfejsy API do wyszukiwania grupowego i wyszukiwania obszaru są obecnie dostępne w wersji zapoznawczej. Aby uzyskać szczegółowe informacje na temat możliwości wyszukiwania, przeczytaj artykuł [Azure Maps Search APIs (Interfejsy API usługi Search w usłudze Azure Maps)](https://docs.microsoft.com/rest/api/maps/search).

### <a name="time-zone-service"></a>Time Zone Service

Usługa Time Zone Service umożliwia wysyłanie zapytań o bieżące, historyczne i przyszłe informacje o strefie czasowej przy użyciu par szerokość-długość geograficzna lub identyfikatora [IANA ID](http://www.iana.org/). Usługa Time Zone ponadto umożliwia konwertowanie identyfikatorów strefy czasowej systemu Microsoft Windows na strefy czasowe IANA, pobieranie przesunięcia strefy czasowej względem czasu UTC oraz pobieranie bieżącego czasu w odpowiedniej strefie czasowej. Typowa odpowiedź JSON na zapytanie wysyłane do usługi Time Zone Service wygląda jak w następującym przykładzie:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Aby uzyskać szczegółowe informacje na temat tej usługi, odwiedź stronę [Azure Maps Timezone APIs (Interfejsy API usługi Timezone w usłudze Azure Maps)](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Traffic Service

Usługa Traffic Service to zbiór usług internetowych umożliwiających deweloperom tworzenie aplikacji internetowych i mobilnych wymagających informacji o ruchu. Usługa udostępnia dwa typy danych:

* Traffic Flow — zarejestrowane prędkości i czasy podróży w czasie rzeczywistym dla wszystkich kluczowych dróg w sieci.
* Traffic Incidents — dokładny wgląd w korki na drogach i zdarzenia w sieci dróg.

![Ruch w usłudze Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

Odwiedź stronę [Azure Maps Traffic APIs (Interfejsy API usługi Traffic w usłudze Azure Maps)](https://docs.microsoft.com/rest/api/maps/traffic), aby uzyskać więcej informacji.

### <a name="ip-to-location"></a>IP to Location

Usługa IP to Location umożliwia wyświetlenie podglądu dwuliterowego kodu kraju pobranego dla danego adresu IP. Ta usługa może ułatwić Ci dopasowanie i ulepszenie środowiska użytkownika przez dostarczanie dostosowanej zawartości w aplikacji na podstawie lokalizacji geograficznej.

Aby uzyskać informacje dotyczące interfejsów API REST dla usługi IP to Location, odwiedź stronę [interfejsów API geolokalizacji usługi Azure Maps](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Model programowania

Usługę Azure Maps zaprojektowano pod kątem mobilności i obsługi aplikacji na wielu platformach. Korzysta ona z modelu programowania niezależnego od języka i obsługuje format danych wyjściowych JSON za pośrednictwem interfejsów [API REST](https://docs.microsoft.com/rest/api/maps/).

Ponadto usługa Azure Maps oferuje wygodną [kontrolkę mapy JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) z prostym modelem programowania, która umożliwia szybkie i łatwe tworzenie aplikacji internetowych i mobilnych.

## <a name="usage"></a>Sposób użycia

Aby uzyskać dostęp do usługi Azure Maps, wystarczy przejść do witryny [Azure Portal](http://portal.azure.com) i utworzyć konto usługi Azure Maps.

Usługa Azure Maps korzysta ze schematu uwierzytelniania opartego na kluczach. Konto będzie zawierać dwa wstępnie wygenerowane klucze do użycia. Rozpocznij integrowanie tych możliwości obsługi lokalizacji w Twojej aplikacji przez użycie dowolnego z kluczy w żądaniu wysłanym do usługi Azure Maps.

## <a name="supported-regions"></a>Obsługiwane regiony

Interfejs API usługi Azure Maps jest obecnie dostępny we wszystkich krajach oprócz następujących:

* Argentyna
* Chiny
* Indie
* Maroko
* Pakistan
* Korea Południowa

Zweryfikuj, czy lokalizacja Twojego bieżącego adresu IP nie znajduje się w jednym z nieobsługiwanych krajów wymienionych powyżej.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat nowych funkcji usługi Azure Maps, zobacz:

> [!div class="nextstepaction"]
> [Route Matrix, Isochrones, IP lookup, and more (Macierze tras, izochrony, wyszukiwanie adresów IP i nie tylko)](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/)

Wypróbuj teraz aplikację przykładową pokazującą możliwości usługi:

> [!div class="nextstepaction"]
> [Uruchom demonstracyjną mapę z interaktywnym wyszukiwaniem](quick-demo-map-app.md)
