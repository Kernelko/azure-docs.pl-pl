---
title: 'Samouczek Azure Analysis Services: lekcja 12 — analiza w programie Excel | Microsoft Docs'
description: Opisuje sposób użycia funkcji analizy w programie Excel w projekcie samouczka usług Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 425c3a351c75c63cb07df66d6122f4d900b4932e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429766"
---
# <a name="analyze-in-excel"></a>Analizuj w programie Excel

W tej lekcji użyjesz funkcji analizy w programie Excel, aby otworzyć program Microsoft Excel, automatycznie utworzyć połączenie z obszarem roboczym modelu oraz automatycznie dodać tabelę przestawną do arkusza. Funkcja analizy w programie Excel to szybki i łatwy sposób na sprawdzenie wydajności projektu modelu przed jego wdrożeniem. Podczas tej lekcji nie wykonasz żadnej analizy danych. Ta lekcja ma umożliwić Tobie jako autorowi modelu zapoznanie się z narzędziami, których możesz użyć do testowania projektu modelu.   
  
Aby ukończyć tę lekcję, musisz dysponować programem Excel zainstalowanym na tym samym komputerze, co program Visual Studio.
  
Szacowany czas trwania lekcji: **5 minut**  
  
## <a name="prerequisites"></a>Wymagania wstępne  
Ten temat stanowi część samouczka modelowania tabelarycznego, który należy wykonać w podanej kolejności. Przed przystąpieniem do wykonywania zadań w tej lekcji należy ukończyć lekcję poprzednią: [Lekcja 11. Tworzenie ról](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Przeglądanie przy użyciu perspektywy domyślnej i perspektywy sprzedaży internetowej  
W pierwszych zadaniach przejrzysz swój model przy użyciu zarówno perspektywy domyślnej, która zawiera wszystkie obiekty modelu, jak i utworzonej wcześniej perspektywy sprzedaży internetowej. Perspektywa sprzedaży internetowej nie zawiera obiektu tabeli Klient.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>Przeglądanie przy użyciu perspektywy domyślnej  
  
1.  Kliknij menu **Model** > **Analizuj w programie Excel**.  
  
2.  W oknie dialogowym **Analiza w programie Excel** kliknij przycisk **OK**.  
  
    Zostanie otwarty nowy skoroszyt w programie Excel. Zostanie utworzone połączenie ze źródłem danych przy użyciu bieżącego konta użytkownika. Do zdefiniowania pól możliwych do wyświetlenia zostanie użyta perspektywa domyślna. Do arkusza zostanie automatycznie dodana tabela przestawna.  
  
3.  W programie Excel zwróć uwagę na grupy miar **DimDate** i **FactInternetSales** wyświetlane w obszarze **Lista pól tabeli przestawnej**. Widoczne będą także tabele **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** i **FactInternetSales** wraz z odpowiednimi kolumnami.  
  
4.  Zamknij program Excel bez zapisywania skoroszytu.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>Przeglądanie przy użyciu perspektywy sprzedaży internetowej  
  
1.  Kliknij menu **Model**, a następnie kliknij pozycję **Analizuj w programie Excel**.  
  
2.  W oknie dialogowym **Analiza w programie Excel** pozostaw pole **Bieżący użytkownik systemu Windows** zaznaczone, a następnie wybierz z listy rozwijanej **Perspektywa** pozycję **Internet Sales** (Sprzedaż internetowa) i kliknij przycisk **OK**. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  W programie Excel zwróć uwagę na to, że **Lista pól tabeli przestawnej** nie zawiera tabeli DimCustomer.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Zamknij program Excel bez zapisywania skoroszytu.  
  
## <a name="browse-by-using-roles"></a>Przeglądanie przy użyciu ról  
Role są ważnym elementem każdego modelu tabelarycznego. Bez co najmniej jednej roli, do której użytkownicy są dodawani jako członkowie, nie mogą oni uzyskać dostępu do danych ani analizować ich przy użyciu modelu. Funkcja analizy w programie Excel umożliwia testowanie zdefiniowanych ról.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>Przeglądanie przy użyciu roli użytkownika Sales Manager  
  
1.  W programie SSDT kliknij menu **Model**, a następnie kliknij opcję **Analizuj w programie Excel**.  
  
2.  W obszarze **Określ nazwę lub rolę użytkownika, która będzie używana do nawiązywania połączenia z modelem** wybierz opcję **Rola**, a następnie z listy rozwijanej wybierz pozycję **Sales Manager** (Menedżer sprzedaży) i kliknij przycisk **OK**.  
  
    Zostanie otwarty nowy skoroszyt w programie Excel. Automatycznie utworzona zostanie tabela przestawna. Lista pól tabeli przestawnej zawiera wszystkie pola danych dostępne w nowym modelu.  
      
3.  Zamknij program Excel bez zapisywania skoroszytu.  
  
## <a name="whats-next"></a>Co dalej?
Przejdź do następnej lekcji: [Lekcja 13. Wdrażanie](../tutorials/aas-lesson-13-deploy.md).

  
  
  
