---
title: 'Samouczek: Integracja usługi Azure Active Directory z zbijają się | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i zbijają się.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 050a5a210ab10a557b149513a0416b188f4618ba
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885907"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Samouczek: Integracja usługi Azure Active Directory z zbijają się

W tym samouczku dowiesz się, jak zintegrować zbijają się w usłudze Azure Active Directory (Azure AD).

Integrowanie zbijają się z usługą Azure AD zapewnia następujące korzyści:

- Możesz kontrolować w usłudze Azure AD, kto ma dostęp do zbijają się
- Umożliwia użytkownikom automatyczne pobieranie zalogowanych do zbijają się (logowanie jednokrotne) przy użyciu konta usługi Azure AD
- Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD za pomocą zbijają się, potrzebne są następujące elementy:

- Subskrypcji usługi Azure AD
- Zbijają się logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko usługi Azure AD w wersji próbnej, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza

W ramach tego samouczka można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie zbijają się za pomocą galerii
2. Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne

## <a name="adding-huddle-from-the-gallery"></a>Dodawanie zbijają się za pomocą galerii

Aby skonfigurować integrację zbijają się w usłudze Azure AD, należy dodać zbijają się za pomocą galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać zbijają się z galerii, wykonaj następujące czynności:**

1. W **[witryny Azure portal](https://portal.azure.com)**, w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony.

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]

3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **zbijają się**. Wybierz **zbijają się** z panel wyników, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne

W tej sekcji konfigurowania i testowania usługi Azure AD logowanie jednokrotne za pomocą zbijają się w oparciu o użytkownika testu o nazwie "Britta Simon."

Dla logowania jednokrotnego do pracy usługi Azure AD musi znać użytkownika odpowiednika w zbijają się z użytkownikiem w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i powiązanych użytkowników w zbijają się musi zostać ustanowione.

Aby skonfigurować i testowanie usługi Azure AD logowanie jednokrotne za pomocą zbijają się, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
3. **[Tworzenie użytkownika testowego zbijają się](#creating-a-huddle-test-user)**  — aby odpowiednikiem Britta Simon w zbijają się połączonego z usługi Azure AD reprezentacja użytkownika.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Testowanie logowania jednokrotnego](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego

W tej sekcji możesz włączyć usługi Azure AD logowania jednokrotnego w witrynie Azure portal i konfigurowanie logowania jednokrotnego w aplikacji zbijają się.

**Aby skonfigurować usługę Azure AD logowanie jednokrotne za pomocą zbijają się, wykonaj następujące czynności:**

1. W witrynie Azure portal na **zbijają się** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie logowania jednokrotnego][4]

2. Kliknij przycisk **pojedynczej zmiany trybu logowania jednokrotnego** na górze ekranu, aby wybrać **SAML** trybu.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_general_300.png)

3. Na **wybierz jedną metodę logowania jednokrotnego** okno dialogowe, kliknij przycisk **wybierz** dla **SAML** trybu, aby włączyć logowanie jednokrotne.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_general_301.png)

4. Na **Ustaw się logowanie jednokrotne z SAML** kliknij **Edytuj** ikonę, aby otworzyć **podstawową konfigurację protokołu SAML** okna dialogowego.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_general_302.png)

5. Na **podstawową konfigurację protokołu SAML** sekcji należy wykonać następujące czynności, jeśli chcesz skonfigurować aplikację w **tożsamości** tryb intiated:

    > [!NOTE]
    > Wystąpienie zbijają się zostanie wykryty automatycznie z domeny wprowadzony poniżej.

    ![Zbijają się domena i adresy URL pojedynczego logowania jednokrotnego informacji](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. W **identyfikator** pole tekstowe, wpisz adres URL:

    | | |
    |--|--|
    | `https://login.huddle.net`|
    | `https://login.huddle.com`|
    | |

    b. W **adres URL odpowiedzi** pole tekstowe, wpisz adres URL:

    | | |
    |--|--|
    | `https://login.huddle.net/saml/browser-sso`|
    | `https://login.huddle.com/saml/browser-sso`|
    | `https://login.huddle.com/saml/idp-initiated-sso`|
    | |

    c. Kliknij przycisk **Ustaw dodatkowe adresy URL** i wykonać następujący krok, jeśli chcesz skonfigurować aplikację w **SP** zainicjowano tryb:

    ![Zbijają się domena i adresy URL pojedynczego logowania jednokrotnego informacji](./media/huddle-tutorial/tutorial_huddle_url1.png)

    W **adres URL logowania** polu tekstowym wpisz dowolny adres URL, za pomocą następującego wzorca:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.com`|
    | `https://us.huddle.com`|
    | |

    > [!NOTE]
    > Wartość adres URL logowania nie jest prawdziwe. Zaktualizuj tę wartość przy użyciu rzeczywisty adres URL logowania. Skontaktuj się z pomocą [zespołem pomocy technicznej klienta zbijają się](https://huddle.zendesk.com) aby zyskać tę wartość.

6. Na **Ustaw się logowania jednokrotnego przy użyciu protokołu SAML** strony w **certyfikat podpisywania SAML** , kliknij przycisk **Pobierz** można pobrać odpowiedni certyfikat zgodnie z wymagania i zapisz go na komputerze.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_huddle_certificate.png)

7. Na **Konfigurowanie zbijają się** sekcji, skopiuj odpowiednie adresy URL, zgodnie z wymaganiami.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_huddle_configure.png)

8. Aby skonfigurować logowanie jednokrotne na **zbijają się** stronie, musisz wysłać certyfikat, który został pobrany i adresów URL, które zostały skopiowane z **Konfigurowanie** **zbijają się** sekcji z witryny Azure portal do [zespołem pomocy technicznej klienta zbijają się](https://huddle.zendesk.com). Ustawiają to ustawienie, aby były prawidłowo po obu stronach połączenia logowania jednokrotnego SAML.

    >[!NOTE]
    > Logowania jednokrotnego musi być włączona przez zespół pomocy technicznej zbijają się. Otrzymasz powiadomienie po zakończeniu konfiguracji.

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w witrynie Azure portal, o nazwie Britta Simon.

1. W witrynie Azure portal w okienku po lewej stronie wybierz **usługi Azure Active Directory**, wybierz opcję **użytkowników**, a następnie wybierz pozycję **wszyscy użytkownicy**.

    ![Utwórz użytkownika usługi Azure AD][100]

2. Wybierz **nowego użytkownika** w górnej części ekranu.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/huddle-tutorial/create_aaduser_01.png) 

3. We właściwościach użytkownika wykonaj następujące czynności.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/huddle-tutorial/create_aaduser_02.png)

    a. W **nazwa** pola wprowadź **BrittaSimon**.
  
    b. W **nazwa_użytkownika** typ pola **brittasimon@yourcompanydomain.extension**  
    Na przykład: BrittaSimon@contoso.com

    c. Wybierz **właściwości**, wybierz opcję **hasło Show** pole wyboru, a następnie zapisz wartość, która jest wyświetlana w polu hasło.

    d. Wybierz pozycję **Utwórz**.

### <a name="creating-a-huddle-test-user"></a>Tworzenie użytkownika testowego zbijają się

Aby umożliwić użytkownikom usługi Azure AD, zaloguj się do zbijają się, musi być obsługiwana w zbijają się. W przypadku zbijają się inicjowanie obsługi administracyjnej jest zadanie ręczne.

**Aby skonfigurować aprowizację użytkowników, wykonaj następujące czynności:**

1. Zaloguj się do Twojej **zbijają się** witryny firmy jako administrator.

2. Kliknij przycisk **obszaru roboczego**.

3. Kliknij przycisk **osób \> zapraszać inne osoby**.

    ![Osoby](./media/huddle-tutorial/IC787838.png "osoby")

4. W **Tworzenie nowego zaproszenia** sekcji, wykonaj następujące czynności:
  
    ![Nowe zaproszenie](./media/huddle-tutorial/IC787839.png "nowe zaproszenie")
  
    a. W **wybierz zespół, aby zapraszać inne osoby do dołączenia do** listy wybierz **zespołu**.

    b. Typ **adres E-mail** prawidłowe platformy Azure konto usługi AD, w którym chcesz udostępnić w celu **wprowadź adres e-mail osoby, które chcesz zaprosić** pola tekstowego.

    c. Kliknij przycisk **zaprosić**.

    >[!NOTE]
    > Właściciel konta usługi Azure AD zostanie wysłana wiadomość e-mail, w tym link do potwierdzenia konta, zanim stanie się aktywny.

>[!NOTE]
>Aprowizuj konta użytkownika usługi Azure AD, można użyć innych zbijają się narzędzi tworzenia konta użytkownika lub interfejsów API dostarczonych przez zbijają się.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji możesz włączyć Britta Simon do używania platformy Azure logowanie jednokrotne za udzielanie dostępu do zbijają się.

1. W witrynie Azure portal wybierz **aplikacje dla przedsiębiorstw**, wybierz opcję **wszystkie aplikacje**.

    ![Przypisz użytkownika][201]

2. Na liście aplikacji wybierz **zbijają się**.

    ![Konfigurowanie logowania jednokrotnego](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz pozycję **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. W **użytkowników i grup** okna dialogowego wybierz **Britta Simon** na liście użytkowników, następnie kliknij przycisk **wybierz** znajdujący się u dołu ekranu.

6. W **Dodaj przydziału** okna dialogowego wybierz **przypisać** przycisku.

### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka zbijają się w panelu dostępu, powinna pojawić się automatycznie strony logowania zbijają się w aplikacji.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących integrowania aplikacji SaaS w usłudze Azure Active Directory](tutorial-list.md)
* [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
