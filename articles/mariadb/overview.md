---
title: Omówienie usługi relacyjnej bazy danych Azure Database for MariaDB
description: Omówienie usługi relacyjnej bazy danych Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 696ba68894b4ac5f73c234ea49883fd0aa3db4d5
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320921"
---
# <a name="what-is-azure-database-for-mariadb"></a>Czym jest usługa Azure Database for MariaDB?

Azure Database for MariaDB to usługa relacyjnej bazy danych w chmurze firmy Microsoft. Usługa Azure Database for MariaDB jest oparta na aparacie bazy danych [MariaDB Community Edition](https://mariadb.org/download/). Ta usługa jest dostępna w publicznej wersji zapoznawczej. 

Usługa Azure Database for MariaDB udostępnia następujące funkcje:

- Wbudowana wysoka dostępność bez dodatkowych kosztów
- Przewidywalna wydajność dzięki płatnościom zgodnym z rzeczywistym użyciem
- Skalowanie zgodnie z potrzebami w ciągu kilku sekund
- Zabezpieczenia zapewniające ochronę przesyłanych oraz magazynowanych poufnych danych
- Automatyczne tworzenie kopii zapasowych i funkcja przywracania do punktu w czasie do 35 dni
- Mechanizmy zabezpieczeń i zgodności klasy korporacyjnej

Te funkcje nie wymagają niemal żadnej administracji. Są one udostępniane bez dodatkowych opłat. Usługa Azure Database for MariaDB ułatwia szybkie tworzenie aplikacji i pozwala skrócić czas wprowadzania produktów na rynek. Nie musisz przydzielać cennego czasu i ważnych zasobów na potrzeby zarządzania maszynami wirtualnymi i infrastrukturą. Możesz też tworzyć aplikacje przy użyciu dowolnych narzędzi i platform typu open source. Osiągaj zadowalające wyniki z szybkością i efektywnością zgodną z Twoimi wymaganiami biznesowymi bez konieczności uczenia się nowych umiejętności.

Aby uzyskać informacje o podstawowych pojęciach i funkcjach w usłudze Azure Database for MariaDB, łącznie z wydajnością, skalowalnością i możliwością zarządzania, zobacz te przewodniki Szybki start:

- [Tworzenie serwera usługi Azure Database for MariaDB za pomocą witryny Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MariaDB za pomocą interfejsu wiersza polecenia platformy Azure](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Dostosowanie wydajności i skalowania w kilka sekund

W wersji zapoznawczej usługa Azure Database for MariaDB oferuje kilka warstw usług: Podstawowa, Ogólnego przeznaczenia i Zoptymalizowana pod kątem pamięci. Każda z warstw oferuje inną wydajność i różne możliwości w celu obsługi różnych obciążeń bazy danych, zarówno niewielkich, jak i ogromnych. Możesz utworzyć swoją pierwszą aplikację na podstawie małej bazy danych za jedynie kilka dolarów miesięcznie, a następnie dostosować skalowanie do potrzeb rozwiązania. Dynamiczna skalowalność umożliwia bazie danych przezroczyste odpowiadanie na gwałtownie zmieniające się wymagania dotyczące zasobów. Zapłacisz tylko za potrzebne zasoby i tylko wtedy, gdy będą używane. Aby uzyskać szczegółowe informacje, zobacz [Warstwy cenowe](concepts-pricing-tiers.md).

## <a name="monitoring-and-alerting"></a>Monitorowanie i zgłaszanie alertów

Jak podjąć decyzję o tym, kiedy należy skalować w górę lub w dół? Możesz użyć wbudowanych funkcji monitorowania wydajności i alertów usługi Azure Database for MariaDB w połączeniu z ocenami wydajności opartymi na rdzeniach wirtualnych. Za pomocą tych narzędzi możesz szybko ocenić efekt skalowania rdzeni wirtualnych w górę lub w dół na podstawie bieżących lub przewidywanych wymagań dotyczących wydajności. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Zapewnienie działania aplikacji i firmy

Umowa SLA o czołowej w branży dostępności 99,99% dla platformy Azure (nieoferowana w publicznej wersji zapoznawczej) jest obsługiwana przez globalną sieć centrów danych zarządzanych przez firmę Microsoft. Ta sieć pomaga zapewnić działanie aplikacji codziennie przez całą dobę. Możesz korzystać z wbudowanych zabezpieczeń, odporności na uszkodzenia i ochrony danych w usłudze Azure Database for MariaDB. Dzięki usłudze Azure Database for MariaDB możesz użyć przywracania do punktu w czasie, aby odzyskać wcześniejszy stan serwera — nawet sprzed 35 dni.

## <a name="secure-your-data"></a>Zabezpieczanie danych

Usługi baz danych platformy Azure mają tradycje zabezpieczeń danych, które usługa Azure Database for MariaDB podtrzymuje. Usługa Azure Database for MariaDB udostępnia funkcje ograniczania dostępu, ochrony przesyłanych i magazynowanych danych oraz monitorowania aktywności. Odwiedź [Centrum zaufania Azure](https://www.microsoft.com/en-us/trustcenter/security), aby uzyskać informacje o zabezpieczeniach platformy Azure.

Usługa Azure Database for MariaDB używa szyfrowania magazynu dla magazynowanych danych. Dane, w tym dane kopii zapasowej, są szyfrowane na dysku. Pliki tymczasowe tworzone przez aparat podczas uruchamiania zapytań nie są szyfrowane na dysku. Usługa korzysta z 256-bitowego szyfru AES zawartego w szyfrowaniu usługi Azure Storage. Klucze są zarządzane przez system. Szyfrowanie magazynu jest zawsze włączone i nie można go wyłączyć.

Domyślnie usługa Azure Database for MariaDB wymaga [zabezpieczeń połączenia SSL](./concepts-ssl-connection-security.md) dla danych przesyłanych przez sieć. Wymuszanie połączeń SSL między serwerem bazy danych a aplikacją kliencką ułatwia ochronę przed atakami typu man-in-the-middle dzięki szyfrowaniu strumienia danych między serwerem a aplikacją. Opcjonalnie możesz wyłączyć wymaganie protokołu SSL dla połączeń z usługą bazy danych, jeśli Twoja aplikacja kliencka nie obsługuje łączności SSL.

## <a name="contacts"></a>Kontakty

Możesz wysłać pytania lub sugestie dotyczące pracy z usługą Azure Database for MariaDB do [zespołu ds. usługi Azure Database for MariaDB](mailto:AskAzureDBforMariaDB@service.microsoft.com) (nie jest to alias pomocy technicznej).

Możesz też użyć następujących punktów kontaktowych:
- Aby skontaktować się z pomocą techniczną platformy Azure, [otwórz żądanie obsługi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) w witrynie Azure Portal.
- Aby rozwiązać problem z Twoim kontem, [otwórz żądanie obsługi](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) w portalu Azure Portal.
- Aby przekazać opinię lub poprosić o nowe funkcje, utwórz wpis na [forach z opiniami dotyczącymi platformy Azure](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Następne kroki

Po przeczytaniu wprowadzenia do usługi Azure Database for MariaDB możesz wykonać następujące czynności:
- Odwiedzenie strony [cennika](https://azure.microsoft.com/pricing/details/mariadb/) zawierającej porównania kosztów i kalkulatory. 
- Rozpoczęcie pracy przez [utworzenie pierwszego serwera](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
