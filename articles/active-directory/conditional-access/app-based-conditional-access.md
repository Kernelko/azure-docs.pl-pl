---
title: Usługa Azure Active Directory na podstawie aplikacji dostępu warunkowego | Dokumentacja firmy Microsoft
description: Dowiedz się, jak działa dostęp warunkowy oparty na aplikacji usługi Azure Active Directory.
services: active-directory
keywords: dostęp warunkowy do aplikacji, dostęp warunkowy w usłudze Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: f34fc4c41094292db9bed1294ee7b26ec04c96c6
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630606"
---
# <a name="azure-active-directory-app-based-conditional-access"></a>Usługa Azure Active Directory na podstawie aplikacji dostępu warunkowego  

Pracownicy używają urządzeń przenośnych zarówno osobistych i służbowych. Upewnij się, że Twoi pracownicy mogą być produktywność, ma zapobiegać utracie danych. Przy użyciu usługi Azure Active Directory (Azure AD) na podstawie aplikacji dostępu warunkowego można ograniczyć dostęp do aplikacji w chmurze do aplikacji klienckich, które może chronić dane firmowe.  

W tym temacie opisano sposób konfigurowania dostępu warunkowego opartego na aplikacji usługi Azure AD.

## <a name="overview"></a>Przegląd

Za pomocą [dostępu warunkowego usługi Azure AD](overview.md), można dostosować sposób autoryzowani użytkownicy mogą dostęp do zasobów. Na przykład można ograniczyć dostęp do aplikacji w chmurze do zaufanych urządzeń.

Możesz użyć [zasady ochrony aplikacji usługi Intune](https://docs.microsoft.com/intune/app-protection-policy) w celu ochrony danych firmy. Zasady ochrony aplikacji usługi Intune nie wymagają rozwiązania do zarządzania urządzeniami przenośnymi (MDM), które umożliwia ochronę danych firmy bez rejestrowania urządzeń w rozwiązaniu do zarządzania urządzeniami.

Ograniczanie dostępu do aplikacji w chmurze do aplikacji klienckich, które obsługują zasady ochrony aplikacji usługi Intune Azure umożliwia dostęp warunkowy oparty na aplikacji usługi Active Directory. Na przykład można ograniczyć dostęp do usługi Exchange Online do aplikacji Outlook.

W terminologii dostępu warunkowego, te aplikacje klienckie są znane jako **zatwierdzonych aplikacji klienckich**.  


![Dostęp warunkowy](./media/app-based-conditional-access/05.png)


Aby uzyskać listę zatwierdzonych aplikacji klienckich, zobacz [zatwierdzone klienta aplikacji wymaganie](technical-reference.md#approved-client-app-requirement).


Można łączyć zasady dostępu warunkowego opartego na aplikacji z innymi zasadami, takich jak [zasady dostępu warunkowego opartego na urządzeniach](require-managed-devices.md) aby zapewnić większą elastyczność w sposobie ochrony danych dla urządzeń osobistych i firmowych.

 


## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym temacie założono, że znasz:

- [Zatwierdzone klienta aplikacji wymaganie](technical-reference.md#approved-client-app-requirement) techniczne.


- Podstawowe pojęcia związane z [dostępu warunkowego w usłudze Azure Active Directory](overview.md).

- Jak [konfigurowania zasad dostępu warunkowego](app-based-mfa.md).

- [Migrację zasad dostępu warunkowego](best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Wymagania wstępne

Aby utworzyć zasady dostępu warunkowego opartego na aplikacji, konieczne jest posiadanie pakietu Enterprise Mobility + Security lub subskrypcję usługi Azure Active Directory premium, a użytkownicy muszą mieć licencję usług EMS lub Azure AD. 


## <a name="exchange-online-policy"></a>Zasady usługi Exchange Online 

Ten scenariusz składa się z zasad dostępu warunkowego opartego na aplikacji, aby uzyskać dostęp do usługi Exchange Online.


### <a name="scenario-playbook"></a>Podręcznik dotyczący scenariusza

W tym scenariuszu założono, że użytkownik:

- Konfiguruje poczty e-mail przy użyciu aplikacji natywnego programu pocztowego w systemie iOS lub Android nawiązać połączenia z programem Exchange

- Otrzymuje wiadomość e-mail, która wskazuje, że dostęp jest możliwy wyłącznie przy użyciu aplikacji Outlook

- Pliki do pobrania aplikacji z linkiem

- Otwiera aplikację Outlook i loguje się przy użyciu poświadczeń usługi Azure AD

- Jest monit o zainstalowanie aplikacji Authenticator (iOS) lub aplikacji Portal firmy (Android), aby kontynuować

- Instalacje aplikacji i możesz powrócić do aplikacji Outlook, aby kontynuować

- Jest monitowany o zarejestrowanie urządzenia

- Umożliwia dostęp do poczty e-mail

Wszystkie zasady ochrony aplikacji usługi Intune są aktywowane w momencie dostęp do danych firmowych i może monitować użytkownika o ponowne uruchomienie aplikacji, należy użyć dodatkowych itp numeru PIN (jeśli jest skonfigurowane dla aplikacji i platformy).

### <a name="configuration"></a>Konfigurowanie 

**Krok 1 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/01.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/07.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **platform urządzeń** i **aplikacje klienckie**:

    a. Jako **platform urządzeń**, wybierz opcję **Android** i **iOS**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/03.png)

    b. Jako **aplikacje klienckie**, wybierz opcję **aplikacje mobilne i aplikacje komputerowe**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/04.png)

5. Jako **kontrole dostępu**, musisz mieć **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)** wybrane.

    ![Dostęp warunkowy](./media/app-based-conditional-access/05.png)
 

**Krok 2 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online za pomocą Active Sync (EAS)**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/06.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.


3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/07.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **aplikacje klienckie**. 

    a. Jako **aplikacje klienckie**, wybierz opcję **programu Exchange Active Sync**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/08.png)

    b. Jako **kontrole dostępu**, musisz mieć **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)** wybrane.

    ![Dostęp warunkowy](./media/app-based-conditional-access/05.png)


**Krok 3 — Konfigurowanie zasad ochrony aplikacji usługi Intune dla systemów iOS i aplikacji klienckich dla systemu Android**


![Dostęp warunkowy](./media/app-based-conditional-access/09.png)

Zobacz [ochrona aplikacji i danych w usłudze Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) Aby uzyskać więcej informacji.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Zasady usługi Exchange Online i SharePoint Online

Ten scenariusz składa się z dostępu warunkowego przy użyciu zasad zarządzania aplikacjami mobilnymi dla dostępu do usługi Exchange Online i SharePoint Online przy użyciu zatwierdzonych aplikacji.

### <a name="scenario-playbook"></a>Podręcznik dotyczący scenariusza

W tym scenariuszu założono, że użytkownik:

- Próbuje użyć aplikacji programu SharePoint, łączenie i wyświetlanie ich firmowych witryn

- Próby logowania przy użyciu tych samych poświadczeń, poświadczenia aplikacji Outlook

- Nie trzeba ponownie zarejestrować i uzyskać dostęp do zasobów


### <a name="configuration"></a>Konfigurowanie

**Krok 1 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online i SharePoint Online**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/71.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.


3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online** i **Office 365 SharePoint Online**. 

    ![Dostęp warunkowy](./media/app-based-conditional-access/02.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **platform urządzeń** i **aplikacje klienckie**:

    a. Jako **platform urządzeń**, wybierz opcję **Android** i **iOS**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/03.png)

    b. Jako **aplikacje klienckie**, wybierz opcję **aplikacje mobilne i aplikacje komputerowe**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/04.png)

5. Jako **kontrole dostępu**, musisz mieć **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)** wybrane.

    ![Dostęp warunkowy](./media/app-based-conditional-access/05.png)




**Krok 2 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online za pomocą Active Sync (EAS)**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/06.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online**. Online 

    ![Dostęp warunkowy](./media/app-based-conditional-access/07.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **aplikacje klienckie**:

    a. Jako **aplikacje klienckie**, wybierz opcję **programu Exchange Active Sync**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/08.png)

    b. Jako **kontrole dostępu**, musisz mieć **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)** wybrane.

    ![Dostęp warunkowy](./media/app-based-conditional-access/05.png)




**Krok 3 — Konfigurowanie zasad ochrony aplikacji usługi Intune dla systemów iOS i aplikacji klienckich dla systemu Android**


![Dostęp warunkowy](./media/app-based-conditional-access/09.png)

Zobacz [ochrona aplikacji i danych w usłudze Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) Aby uzyskać więcej informacji.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Zasady aplikacji lub być zgodne urządzenia dla usługi Exchange Online i SharePoint Online

Ten scenariusz składa się z zasad dostępu warunkowego opartego na aplikacji lub być zgodne urządzenie do uzyskiwania dostępu do usługi Exchange Online.


### <a name="scenario-playbook"></a>Podręcznik dotyczący scenariusza

W tym scenariuszu założono, że:
 
- Niektóre użytkownika są już zarejestrowane (z lub bez urządzenia firmowe)

- Użytkownicy, którzy nie są zarejestrowane i zarejestrowane w usłudze Azure AD przy użyciu aplikacji chronionych wymagania aplikacji, aby zarejestrować urządzenia uzyskują dostęp do zasobów

- Zarejestrowani użytkownicy za pomocą aplikacji chronionej aplikacji, nie trzeba ponownie zarejestrować urządzenie


### <a name="configuration"></a>Konfigurowanie

**Krok 1 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online i SharePoint Online**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/62.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online** i **Office 365 SharePoint Online**. 

     ![Dostęp warunkowy](./media/app-based-conditional-access/02.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **platform urządzeń** i **aplikacje klienckie**. 
 
    a. Jako **platform urządzeń**, wybierz opcję **Android** i **iOS**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/03.png)

    b. Jako **aplikacje klienckie**, wybierz opcję **aplikacje mobilne i aplikacje komputerowe**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/04.png)

5. Jako **kontrole dostępu**, musisz mieć następujące wybrane:

    - **Wymagaj, aby urządzenie było oznaczone jako zgodne**

    - **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)**

    - **Wymagaj jednej z wybranych kontrolek**   
 
    ![Dostęp warunkowy](./media/app-based-conditional-access/11.png)



**Krok 2 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online za pomocą Active Sync (EAS)**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/61.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online**. 

    ![Dostęp warunkowy](./media/app-based-conditional-access/07.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **aplikacje klienckie**. 

    Jako **aplikacje klienckie*, wybierz opcję **programu Exchange Active Sync**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/08.png)

5. Jako **kontrole dostępu**, musisz mieć **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)** wybrane.
 
    ![Dostęp warunkowy](./media/app-based-conditional-access/11.png)




**Krok 3 — Konfigurowanie zasad ochrony aplikacji usługi Intune dla systemów iOS i aplikacji klienckich dla systemu Android**


![Dostęp warunkowy](./media/app-based-conditional-access/09.png)

Zobacz [ochrona aplikacji i danych w usłudze Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) Aby uzyskać więcej informacji.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Zasady aplikacji i zgodnego urządzenia dla usługi Exchange Online i SharePoint Online

Ten scenariusz składa się z zasad dostępu warunkowego opartego na aplikacji i zgodnego urządzenia do uzyskiwania dostępu do usługi Exchange Online.


### <a name="scenario-playbook"></a>Podręcznik dotyczący scenariusza

W tym scenariuszu założono, że użytkownik:
 
-   Konfiguruje poczty e-mail przy użyciu aplikacji natywnego programu pocztowego w systemie iOS lub Android nawiązać połączenia z programem Exchange
-   Otrzymuje wiadomość e-mail, który oznacza, że dostępu wymaga urządzenia do zarejestrowania
-   Pliki do pobrania aplikacji portal firmy oraz loguje się do portalu firmy
-   Sprawdza, czy wiadomości e-mail i zostanie poproszony o korzystali z aplikacji Outlook
-   Pobieranie aplikacji Outlook
-   Zostanie otwarty w aplikacji Outlook i wprowadza poświadczenia używane w rejestracji
-   Użytkownik ma dostęp do poczty e-mail

Żadne zasady ochrony aplikacji usługi Intune są aktywowane w momencie dostęp do danych firmowych i może monitować użytkownika o ponowne uruchomienie aplikacji, należy użyć dodatkowych itp numeru PIN (jeśli jest skonfigurowane dla aplikacji i platformy)


### <a name="configuration"></a>Konfigurowanie

**Krok 1 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online i SharePoint Online**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/62.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online** i **Office 365 SharePoint Online**. 

     ![Dostęp warunkowy](./media/app-based-conditional-access/02.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **platform urządzeń** i **aplikacje klienckie**. 
 
    a. Jako **platform urządzeń**, wybierz opcję **Android** i **iOS**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/03.png)

    b. Jako **aplikacje klienckie**, wybierz opcję **aplikacje mobilne i aplikacje komputerowe**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/04.png)

5. Jako **kontrole dostępu**, musisz mieć następujące wybrane:

    - **Wymagaj, aby urządzenie było oznaczone jako zgodne**

    - **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)**

    - **Wymagaj wszystkich wybranych kontrolek**   
 
    ![Dostęp warunkowy](./media/app-based-conditional-access/13.png)



**Krok 2 — Konfigurowanie zasad dostępu warunkowego usługi Azure AD dla usługi Exchange Online za pomocą Active Sync (EAS)**

Zasady dostępu warunkowego w tym kroku należy skonfigurować następujące składniki:

![Dostęp warunkowy](./media/app-based-conditional-access/61.png)

1. **Nazwa** zasad dostępu warunkowego.

2. **Użytkownicy i grupy**: wszystkie zasady dostępu warunkowego musi mieć co najmniej jednego użytkownika lub wybranej grupy.

3. **Aplikacje w chmurze:** jako aplikacje w chmurze, musisz wybrać **Office 365 Exchange Online**. 

    ![Dostęp warunkowy](./media/app-based-conditional-access/07.png)

4. **Warunki:** jako **warunki**, należy skonfigurować **aplikacje klienckie**. 

    Jako **aplikacje klienckie**, wybierz opcję **programu Exchange Active Sync**.

    ![Dostęp warunkowy](./media/app-based-conditional-access/08.png)

5. Jako **kontrole dostępu**, musisz mieć następujące wybrane:

    - **Wymagaj, aby urządzenie było oznaczone jako zgodne**

    - **Wymagaj zatwierdzonej aplikacji klienckiej (wersja zapoznawcza)**

    - **Wymagaj wszystkich wybranych kontrolek**   
 
    ![Dostęp warunkowy](./media/app-based-conditional-access/64.png)




**Krok 3 — Konfigurowanie zasad ochrony aplikacji usługi Intune dla systemów iOS i aplikacji klienckich dla systemu Android**


![Dostęp warunkowy](./media/app-based-conditional-access/09.png)

Zobacz [ochrona aplikacji i danych w usłudze Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) Aby uzyskać więcej informacji.






## <a name="next-steps"></a>Kolejne kroki

Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wymagają usługi MFA dla określonych aplikacji przy użyciu dostępu warunkowego usługi Azure Active Directory](app-based-mfa.md).

Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego dla danego środowiska, zobacz [najlepsze rozwiązania dotyczące dostępu warunkowego w usłudze Azure Active Directory](best-practices.md). 
