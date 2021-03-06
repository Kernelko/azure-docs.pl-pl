---
title: 'Samouczek: Konfigurowanie OnDemand podstawy dla automatycznej aprowizacji użytkowników z usługą Azure Active Directory | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować usługi Azure Active Directory do automatycznego aprowizowania lub cofania aprowizacji kont użytkowników do OnDemand podstawy.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 6a6cfb2cb1fd6b70be0437c8b6fa62f50e76e53b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345417"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie OnDemand podstawy dla automatycznej aprowizacji użytkowników


Celem tego samouczka jest pokazują kroki do wykonania w OnDemand podstawy i usługi Azure Active Directory (Azure AD), aby skonfigurować usługę Azure AD automatycznego aprowizowania lub cofania aprowizacji użytkowników i/lub grup w celu OnDemand podstawy.


> [!NOTE]
> W tym samouczku opisano łącznika, który został zbudowany na podstawie usługi aprowizacji użytkownika usługi Azure AD. Ważne szczegółowe informacje na temat tej usługi nie, jak działa i często zadawane pytania, [Automatyzowanie aprowizacji użytkowników i anulowania obsługi do aplikacji SaaS w usłudze Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku przyjęto założenie, iż już następujące wymagania wstępne:

*   Dzierżawa usługi Azure AD
*   Dzierżawy OnDemand podstawy
*   Konto użytkownika OnDemand podstawy z uprawnieniami administratora


> [!NOTE]
> Inicjowanie obsługi administracyjnej integracji usługi Azure AD opiera się na [OnDemand Webservice podstawy](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), który jest dostępny dla zespołów OnDemand podstawy.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Dodawanie OnDemand podstawy z galerii
Przed skonfigurowaniem OnDemand podstawy dla automatycznej aprowizacji użytkowników z usługą Azure AD, musisz dodać OnDemand podstawy z galerii aplikacji usługi Azure AD z listą zarządzanych aplikacji SaaS.

**Aby dodać OnDemand podstawy z galerii aplikacji usługi Azure AD, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)**, w panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje**.

    ![Aplikacje w przedsiębiorstwie sekcji][2]
    
3. Aby dodać OnDemand podstawy, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **OnDemand podstawy**.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/AppSearch.png)

5. W panelu wyników wybierz **OnDemand podstawy**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać OnDemand podstawy do listy aplikacji SaaS.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Przypisywanie użytkowników do OnDemand podstawy

Usługa Azure Active Directory używa koncepcji o nazwie "przypisania", aby określić, użytkowników, którzy otrzymają dostęp do wybranych aplikacji. W kontekście automatyczna aprowizacja użytkowników są synchronizowane tylko użytkowników i/lub grup, które "przypisano" do aplikacji w usłudze Azure AD. 

Przed Skonfiguruj i Włącz automatyczne aprowizowanie użytkowników, należy zdecydować, użytkowników i/lub grup w usłudze Azure AD muszą mieć dostęp do OnDemand podstawy. Po decyzję, możesz przypisać użytkownikom i/lub grup OnDemand podstawy zgodnie z instrukcjami w tym miejscu:

*   [Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Ważne wskazówki dotyczące przypisywania użytkowników do OnDemand podstawy

*   Zalecane jest jeden użytkownik usługi Azure AD jest przypisane do OnDemand podstawy do testowania automatyczne aprowizowanie konfiguracji użytkowników. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Podczas przypisywania użytkowników do OnDemand podstawy, należy wybrać prawidłową rolą specyficzne dla aplikacji (jeśli jest dostępny) w oknie dialogowym przydział. Użytkownicy z **domyślnego dostępu** roli są wyłączone, od zainicjowania obsługi administracyjnej.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Konfigurowanie automatycznej aprowizacji użytkowników do OnDemand podstawy

Ta sekcja przeprowadzi Cię przez kroki, aby skonfigurować usługi Azure AD inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączyć użytkowników i/lub grup w OnDemand podstawy oparte na użytkownika i/lub przypisania grupy w usłudze Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Aby skonfigurować automatyczna aprowizacja użytkowników dla OnDemand podstawy w usłudze Azure AD:


1. Zaloguj się do [witryny Azure portal](https://portal.azure.com) i przejdź do **usługi Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje**.

2. Wybierz OnDemand podstawy z listy aplikacji SaaS.
 
    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/Successcenter2.png)

3. Wybierz **aprowizacji** kartę.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. W obszarze **poświadczeń administratora** sekcji danych wejściowych **nazwa użytkownika administratora**, **hasło administratora**, i **domeny** z Twojej OnDemand podstawy konto.

    *   W **nazwa użytkownika administratora** pola, wypełnij domena\nazwa_użytkownika konta administratora w Twojej dzierżawie OnDemand podstawy. Przykład: contoso\admin.

    *   W **hasło administratora** pola, wypełnij hasło odpowiadający nazwa użytkownika administratora.

    *   W **domeny** pola, wypełnij dzierżawy OnDemand podstawy adres URL usługi sieci Web. Przykład: Usługa znajduje się w `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`dla domeny firmy Contoso jest `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Aby uzyskać więcej informacji na temat sposobu pobierania adresu URL usługi sieci Web, zobacz [tutaj](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Podczas wypełniania pola wyświetlane w kroku 5, kliknij przycisk **Testuj połączenie** aby zapewnić usłudze Azure AD można połączyć się z OnDemand podstawy. Jeśli połączenie nie powiedzie się, upewnij się, że Twoje konto OnDemand podstawy ma uprawnienia administratora i spróbuj ponownie.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. W **wiadomość E-mail z powiadomieniem** wprowadź adres e-mail osoby lub grupy, który powinien otrzymywać powiadomienia błąd inicjowania obsługi administracyjnej i zaznacz pole wyboru - **Wyślij wiadomość e-mail z powiadomieniem, gdy wystąpi awaria**.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Kliknij pozycję **Zapisz**.

9. W obszarze **mapowania** zaznacz **synchronizacji Azure użytkownicy usługi Active Directory do OnDemand podstawy**.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD OnDemand podstawy w **mapowanie atrybutu** sekcji. Atrybuty wybrany jako **zgodne** właściwości są używane do dopasowania kont użytkowników w OnDemand podstawy dla operacji aktualizacji. Wybierz **Zapisz** przycisk, aby zatwierdzić zmiany.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Aby skonfigurować filtrów określania zakresu, można znaleźć w następujących instrukcjach podanych w [samouczek filtru Scoping](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Aby włączyć usługi Azure AD, usługi dla OnDemand podstawy aprowizacji, zmień **stanie aprowizacji** do **na** w **ustawienia** sekcji.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Definiowanie użytkowników i/lub grup, które chcesz, aby obsługiwać je na OnDemand podstawy, wybierając odpowiednie wartości w **zakres** w **ustawienia** sekcji.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Gdy wszystko jest gotowe do aprowizowania, kliknij przycisk **Zapisz**.

    ![Inicjowanie obsługi administracyjnej OnDemand podstawy](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)


Ta operacja uruchamia początkowa synchronizacja wszystkich użytkowników i/lub grup zdefiniowanych w **zakres** w **ustawienia** sekcji. Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które występują co około 40 minut, tak długo, jak działa usługa aprowizacji usługi Azure AD. Możesz użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do raportu działań w tym artykule opisano wszystkie akcje wykonywane przez usługę Azure AD, inicjowania obsługi usługi na OnDemand podstawy aprowizacji.

Aby uzyskać więcej informacji na temat sposobu odczytywania aprowizacji dzienniki usługi Azure AD, zobacz [raportowanie na inicjowanie obsługi administracyjnej konta użytkownika automatyczne](../manage-apps/check-status-user-account-provisioning.md).
## <a name="connector-limitations"></a>Ograniczenia łącznika

* OnDemand podstawy **pozycji** atrybut oczekuje, że wartość, która odnosi się do ról w portalu OnDemand podstawy. Lista prawidłowe **pozycji** będzie można uzyskać, przechodząc do wartości **edytowanie rekordu użytkownika > struktury organizacji > pozycja** w portalu OnDemand podstawy.
    ![Inicjowanie obsługi administracyjnej OnDemand podstawy Edytowanie użytkownika](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![OnDemand podstawy aprowizacji pozycji](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![OnDemand podstawy aprowizacji pozycji listy](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)
    
## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie aprowizacją konta użytkownika dla aplikacji przedsiębiorstwa](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



## <a name="next-steps"></a>Kolejne kroki

* [Dowiedz się, jak przeglądać dzienniki i Uzyskaj raporty dotyczące inicjowania obsługi administracyjnej działania](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
