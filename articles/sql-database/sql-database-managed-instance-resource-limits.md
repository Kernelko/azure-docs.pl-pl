---
title: Limity zasobów bazy danych SQL platformy Azure — wystąpienie zarządzane | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera omówienie limitów zasobów usługi Azure SQL Database dla wystąpienia zarządzanego.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp
manager: craigg
ms.date: 10/17/2018
ms.openlocfilehash: 97c141b6e0c071a8cea27f9a873f28a6c5113a18
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394871"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Limity zasobów wystąpienia zarządzanego Azure SQL Database — omówienie

Ten artykuł zawiera omówienie limitów zasobów wystąpienia zarządzanego Azure SQL Database i zawiera informacje, jak utworzyć żądanie, aby zwiększyć domyślne limity regionalne subskrypcji. 

> [!NOTE]
> Aby uzyskać inne ograniczenia, wystąpienia zarządzanego, zobacz [modelu zakupu opartego na rdzeniach wirtualnych](sql-database-managed-instance.md#vcore-based-purchasing-model) i [wystąpienie zarządzane usługi warstwy](sql-database-managed-instance.md#managed-instance-service-tiers). Aby poznać różnice w obsługiwanych funkcjach i T-SQL zobacz instrukcje [różnice są wyposażone w](sql-database-features.md) i [Obsługa instrukcji języka T-SQL](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Limity zasobów na poziomie wystąpienia

Wystąpienie zarządzane ma cechy i limitów zasobów, które jest zależna od podstawową infrastrukturę i architektury. Limity są zależne od sprzętu w warstwie generowania i usługi.

### <a name="hardware-generation-characteristics"></a>Właściwości generacji sprzętu

Wystąpienie usługi Azure SQL Database Managed można wdrożyć na dwa sprzętu generacji (4. generacji i 5. generacji). Generacji sprzętu mają inne cechy, które są opisane w poniższej tabeli:

|   | **4. generacji** | **5. generacji** |
| --- | --- | --- |
| Sprzęt | Intel E5-2673 v3 procesorów 2,4 GHz (Haswell), dołączony dysk SSD — rdzeń wirtualny = 1 PP (fizycznych rdzeni) | Intel E5-2673 v4 (broadwell z zegarem) 2.3 GHz procesorów, szybkie eNVM dyski SSD, — rdzeń wirtualny = LP 1 (hyper wątek) |
| Wystąpienia obliczeniowe | 8, 16, 24 rdzenie wirtualne | 8, 16, 24, 32, 40, 64, 80 rdzeni wirtualnych |
| Memory (Pamięć) | 7 GB na rdzeń wirtualny | 5.1 GB na rdzeń wirtualny |
| Maksymalny rozmiar magazynu (krytyczne dla działania firmy) | 1 TB | 1 TB, 2 TB lub 4 TB w zależności od liczby rdzeni |

### <a name="service-tier-characteristics"></a>Właściwości warstwy usług

Wystąpienia zarządzanego istnieją dwie warstwy usług - ogólnego przeznaczenia i krytyczne dla działania firmy (publiczna wersja zapoznawcza). Te warstwy zapewniają różne możliwości, zgodnie z opisem w poniższej tabeli:

| **Funkcja** | **Ogólnego przeznaczenia** | **Krytyczne dla działania firmy (wersja zapoznawcza)** |
| --- | --- | --- |
| Liczba rdzeni wirtualnych\* | 4. generacji: 8, 16, 24<br/>5. generacji: 8, 16, 24, 32, 40, 64, 80 | 4. generacji: 8, 16, 24, 32 <br/> 5. generacji: 8, 16, 24, 32, 40, 64, 80 |
| Memory (Pamięć) | 4. generacji: 56GB – 156GB<br/>5. generacji: 44GB – 440GB<br/>\*Proporcjonalnie do liczby rdzeni wirtualnych | 4. generacji: 56GB – 156GB <br/> 5. generacji: 44GB – 440GB<br/>\*Proporcjonalnie do liczby rdzeni wirtualnych |
| Maksymalny rozmiar magazynu | 8 TB | Gen 4: 1 TB <br/> 5. generacji: <br/>-1 TB, 8, 16 rdzeni wirtualnych<br/>-2 TB dla 24 rdzenie wirtualne<br/>-4 TB dla 32, 40, 64, 80 rdzeni wirtualnych |
| Maksymalny rozmiar magazynu na bazę danych | Określony przez rozmiar maksymalnego rozmiaru magazynu dla każdego wystąpienia | Określony przez rozmiar maksymalnego rozmiaru magazynu dla każdego wystąpienia |
| Maksymalna liczba baz danych dla każdego wystąpienia | 100 | 100 |
| Maksymalna liczba plików bazy danych dla każdego wystąpienia | Maksymalnie 280 | Nieograniczona liczba |
| Oczekiwano maksymalnego rozmiaru magazynu, operacje We/Wy | 500-5000 ([zależy od rozmiaru plików danych](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)). | Zależy od bazowego szybkość dysków SSD. |

## <a name="supported-regions"></a>Obsługiwane regiony

Instanced zarządzane można tworzyć tylko w [obsługiwane regiony](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Jeśli chcesz utworzyć wystąpienie zarządzane w regionie, który nie jest obecnie obsługiwane, możesz to zrobić [Wyślij żądanie pomocy technicznej za pośrednictwem witryny Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Typy obsługiwane subskrypcji

Wystąpienie zarządzane obecnie obsługuje wdrożenia tylko dla następujących typów subskrypcji:

- [Umowy Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Płatność za rzeczywiste użycie](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Dostawca usług w chmurze (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> To ograniczenie jest tymczasowe. W przyszłości będzie można włączyć nowych typów subskrypcji.

## <a name="regional-resource-limitations"></a>Ograniczenia zasobów regionalnego

Typy obsługiwane subskrypcji mogą zawierać ograniczoną liczbę zasobów na region. Wystąpienia zarządzanego istnieją dwie domyślne limity na region platformy Azure, w zależności od rodzaju typu subskrypcji:

- **Limit podsieci**: Maksymalna liczba podsieci wdrożonym wystąpienia zarządzanego w jednym regionie.
- **Limit liczby wystąpień**: Maksymalna liczba wystąpień, które mogą być wdrażane w jednym regionie.

W poniższej tabeli przedstawiono domyślne limity regionalne dla obsługiwanych subskrypcji:

|Typ subskrypcji| Maksymalna liczba podsieci wystąpienia zarządzanego | Maksymalna liczba wystąpień |Maksymalna liczba GP zarządzane wystąpienia *|Maksymalna liczba BC zarządzane wystąpienia *|
| :---| :--- | :--- |:--- |:--- |
|Płatność zgodnie z rzeczywistym użyciem|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 **|12 **|12 **|3 **|

\* 1 BC lub 4 wystąpień zasad grupy w jednej podsieci, można wdrożyć albo tak, aby łączna liczba jednostek"wystąpienie" w podsieci nigdy nie przekracza 4.

** Maksymalnej liczby wystąpień w jednej warstwie usługi ma zastosowanie, jeśli żadne wystąpienia znajduje się w innej warstwie usługi. W przypadku, gdy użytkownik chce mieszać GP i BC wystąpienia w ramach tej samej podsieci, następująca sekcja służy jako odwołanie dla dozwolonych kombinacji. Zgodnie z zasadą proste całkowita liczba podsieci nie może przekraczać 3, a łączna liczba jednostek wystąpienia nie może przekraczać 12.

Te limity można zwiększyć, tworząc specjalny [żądania pomocy technicznej w witrynie Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance) Jeśli potrzebujesz większej liczby wystąpień zarządzanych w bieżącym regionie. Jako alternatywę można utworzyć nowego wystąpienia zarządzane przez usługę w innym regionie platformy Azure, bez wysyłania żądań pomocy technicznej.

> [!IMPORTANT]
> Podczas planowania wdrożenia należy rozważyć, czy wystąpienia biznesowe krytyczne (BC) (z powodu dodane redundancy) zwykle zużywa 4 x większą pojemność niż wystąpienia ogólnego przeznaczenia (GP). Tak więc, na obliczeniach, wystąpienia zasad grupy: 1 = 1 wystąpienie jednostki oraz wystąpienia 1 BC = 4 jednostki wystąpienia. Aby uprościć analizy użycia na wartości domyślne, podsumowanie jednostki wystąpienia we wszystkich podsieciach w regionie, w których są wdrażane wystąpienia zarządzane przez usługę i należy porównać wyniki z granicami jednostki wystąpienia dla typu Twojej subskrypcji.

## <a name="strategies-for-deploying-mixed-general-purpose-and-business-critical-instances"></a>Strategie wdrażania mieszane wystąpień ogólnego przeznaczenia i krytyczne dla działania firmy

[Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) subskrypcji może mieć kombinacji GP i BC wystąpień. Istnieją jednak pewne ograniczenia dotyczące umieszczania wystąpienia w podsieci.

> [!Note] 
> [Płatność za rzeczywiste użycie](https://azure.microsoft.com/offers/ms-azr-0003p/) i [dostawca usług chmury (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources) typów subskrypcji można mieć albo jeden krytyczne dla działania firmy lub w górę do 4 wystąpień ogólnego przeznaczenia.

Poniższe przykłady obejmują przypadków wdrożenie z podsieciami niepuste i mieszane GP i BC warstwy usług.

|Liczba podsieci|Podsieć 1|Podsieć 2|Podsieci 3|
|:---|:---|:---|:---|
|1|1 BC i maksymalnie 8 zasad grupy<br>2 BC i maksymalnie 4 zasad grupy|ND| ND|
|2|BC 0, GP maksymalnie 4|BC 1, GP maksymalnie 4<br>BC 2, 0 GP|ND|
|2|BC 1, 0 ZASAD GRUPY|BC 0, GP maksymalnie 8<br>BC 1, GP maksymalnie 4|ND|
|2|BC 2, 0 GP|BC 0, GP maksymalnie 4|ND|
|3|BC 1, 0 ZASAD GRUPY|BC 1, 0 ZASAD GRUPY|BC 0, GP maksymalnie 4|
|3|BC 1, 0 ZASAD GRUPY|BC 0, GP maksymalnie 4|BC 0, GP maksymalnie 4|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Uzyskanie większego limitu przydziału dla wystąpienia zarządzanego usługi SQL

Jeśli potrzebujesz większej liczby wystąpień zarządzanych w Twojej bieżącej regionach można wysłać żądanie pomocy technicznej, aby rozszerzyć przydziału przy użyciu witryny Azure portal. Aby zainicjować proces uzyskiwania większego limitu przydziału:

1. Otwórz **Pomoc i obsługa techniczna**i kliknij przycisk **nowe żądanie obsługi**. 

   ![Pomoc i obsługa techniczna](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Na karcie podstawy nowe żądanie pomocy technicznej:
   - Aby uzyskać **typ problemu**, wybierz opcję **limity usług i subskrypcji (przydziały)**.
   - W polu **Subskrypcja** wybierz subskrypcję.
   - Aby uzyskać **typ limitu przydziału**, wybierz opcję **wystąpienie zarządzane usługi SQL Database**.
   - Aby uzyskać **plan pomocy technicznej**, wybierz plan pomocy technicznej.

     ![Problem typu przydziału](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Kliknij przycisk **Dalej**.
4. Na karcie Problem nowe żądanie pomocy technicznej:
   - Aby uzyskać **ważność**, wybierz poziom ważności problemu.
   - Aby uzyskać **szczegóły**, zawierają dodatkowe informacje o swoim problemie, w tym komunikaty o błędach.
   - Aby uzyskać **przekazywanie pliku**, Dołącz plik z dodatkowymi informacjami (maksymalnie 4 MB).

     ![Szczegóły problemu](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Prawidłowemu żądaniu powinien zawierać:
     > - Region w subskrypcję, która musi zostać zwiększone limit
     > - Wymagana liczba wystąpień na warstwę usługi w istniejących podsieci po przydział zwiększyć (Jeśli żadna z podsieci istniejących potrzebuje do wyodrębnienia
     > - Wymagana liczba nowych podsieci i łącznej liczby wystąpień na warstwę usług w ramach nowych podsieci (jeśli zajdzie potrzeba wdrożenia wystąpienia zarządzanego w nowych podsieci).
     
5. Kliknij przycisk **Dalej**.
6. Na karcie informacje kontaktowe nowe żądanie pomocy technicznej wprowadź preferowaną metodę kontaktu (adres e-mail lub telefon) i szczegóły dotyczące kontaktu.
7. Kliknij pozycję **Utwórz**.

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać więcej informacji na temat wystąpienia zarządzanego, zobacz [co to jest wystąpienie zarządzane?](sql-database-managed-instance.md). 
- Aby uzyskać informacje o cenach, zobacz [wystąpienie zarządzane usługi SQL Database, cennik](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Aby dowiedzieć się, jak utworzyć pierwsze wystąpienie zarządzane, zobacz [przewodnika Szybki start](sql-database-managed-instance-get-started.md).
