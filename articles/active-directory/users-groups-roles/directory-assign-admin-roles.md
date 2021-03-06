---
title: Uprawnienia roli administratora w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
description: Roli administratora można dodać użytkowników, przypisywać role administracyjne, resetować hasła użytkowników, zarządzać licencjami użytkowników lub zarządzać domenami.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 28f06efdd990e46eaa84b1fe26ed5d8944971505
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156922"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Uprawnienia roli administratora w usłudze Azure Active Directory

Za pomocą usługi Azure Active Directory (Azure AD), można wyznaczyć oddzielny administratorom różne funkcje. Administratorzy mogą umieszczoną w portalu usługi Azure AD do wykonania zadania, takie jak dodanie lub zmiana użytkowników, przypisywanie ról administracyjnych, resetowanie haseł użytkowników, zarządzanie licencjami użytkowników i zarządzanie nazwami domen.

Administrator globalny ma dostęp do wszystkich funkcji administracyjnych. Domyślnie osoba, która zarejestruje się w subskrypcji platformy Azure przypisano rolę administratora globalnego dla katalogu. Tylko administratorzy globalni mogą delegować ról administratora.

## <a name="assign-or-remove-administrator-roles"></a>Przypisywanie lub usuwanie ról administratora

Aby dowiedzieć się, jak przypisywać role administracyjne dla użytkownika w usłudze Azure Active Directory, zobacz [widoku i przypisywanie ról administratorów w usłudze Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Dostępne role

Dostępne są następujące role administratora:

* **[Administrator aplikacji](#application-administrator)**: użytkownicy w tej roli można tworzyć i zarządzać wszystkimi aspektami aplikacje dla przedsiębiorstw i rejestracje aplikacji, ustawienia serwera proxy aplikacji. Ta rola daje również możliwość wyrazić zgodę na delegowane uprawnienia i uprawnienia aplikacji, z wyjątkiem programu Microsoft Graph i Azure AD Graph. Członkowie tej roli nie są dodawane jako właścicieli, podczas tworzenia nowej rejestracji aplikacji lub aplikacji dla przedsiębiorstw.

  <b>Ważne</b>: Ta rola umożliwia zarządzanie poświadczeniami aplikacji. Użytkownicy przypisani do tej roli można dodać poświadczeń do aplikacji i spersonifikować tożsamości aplikacji przy użyciu tych poświadczeń. Jeśli tożsamości aplikacji udzielono dostępu do usługi Azure Active Directory, takie jak możliwość utworzenia lub zaktualizowania użytkownika lub inne obiekty, przypisani do tej roli użytkownika może wykonać te czynności podczas personifikacji aplikacji. Ta możliwość spersonifikować tożsamości aplikacji może być podniesienie uprawnień za pośrednictwem co użytkownik może wykonać za pomocą swoje przypisania roli w usłudze Azure AD. Jest ważne dowiedzieć się, że przypisanie użytkownika do roli administratora aplikacji daje im możliwość personifikacji tożsamość aplikacji.

* **[Deweloper aplikacji](#application-developer)**: użytkownicy w tej roli mogą tworzyć rejestracje aplikacji po "Użytkownicy mogą rejestrować aplikacje" został ustawiony na nie. Ta rola pozwala również elementy członkowskie do wyrażenia zgody we własnym imieniu po "Użytkownicy mogą zezwalać aplikacjom uzyskiwanie dostępu do danych firmy w ich imieniu" został ustawiony na nie. Członkowie tej roli są dodawane jako właścicieli, podczas tworzenia nowej rejestracji aplikacji lub aplikacji dla przedsiębiorstw.

* **[Administrator rozliczeń](#billing-administrator)**: dokonuje zakupów, zarządza subskrypcjami, zarządza biletami pomocy technicznej i monitoruje kondycję usługi.

* **[Administrator aplikacji w chmurze](#cloud-application-administrator)**: użytkownicy w tej roli mają takie same uprawnienia, jak roli administratora aplikacji z wyjątkiem możliwości zarządzania serwera proxy aplikacji. Ta rola umożliwia tworzenie i zarządzanie nimi wszystkie aspekty aplikacji dla przedsiębiorstw i rejestracje aplikacji. Ta rola daje również możliwość wyrazić zgodę na delegowane uprawnienia i uprawnienia aplikacji, z wyjątkiem programu Microsoft Graph i Azure AD Graph. Członkowie tej roli nie są dodawane jako właścicieli, podczas tworzenia nowej rejestracji aplikacji lub aplikacji dla przedsiębiorstw.

  <b>Ważne</b>: Ta rola umożliwia zarządzanie poświadczeniami aplikacji. Użytkownicy przypisani do tej roli można dodać poświadczeń do aplikacji i spersonifikować tożsamości aplikacji przy użyciu tych poświadczeń. Jeśli tożsamości aplikacji udzielono dostępu do usługi Azure Active Directory, takie jak możliwość utworzenia lub zaktualizowania użytkownika lub inne obiekty, przypisani do tej roli użytkownika może wykonać te czynności podczas personifikacji aplikacji. Ta możliwość spersonifikować tożsamości aplikacji może być podniesienie uprawnień za pośrednictwem co użytkownik może wykonać za pomocą swoje przypisania roli w usłudze Azure AD. Jest ważne dowiedzieć się, że przypisanie użytkownika do roli Administrator aplikacji w chmurze daje im możliwość personifikacji tożsamość aplikacji.

* **[Administrator urządzenia w chmurze](#cloud-device-administrator)**: użytkownicy w tej roli można włączyć, wyłączyć, usuwać urządzenia w usłudze Azure AD i klucze do odczytu funkcją BitLocker systemu Windows 10 (jeśli istnieje) w witrynie Azure portal. Rola nie powoduje przyznania uprawnień do zarządzania innych właściwości, na urządzeniu.

* **[Administrator do spraw zgodności](#compliance-administrator)**: użytkownicy z tą rolą mają uprawnienia do zarządzania w ramach zabezpieczeń usługi Office 365 i Centrum zgodności i Centrum administracyjnego programu Exchange. Więcej informacji o [ról administratora o usługi Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrator dostępu warunkowego](#conditional-access-administrator)**: użytkownicy z tą rolą mają możliwość zarządzania ustawieniami dostępu warunkowego usługi Azure Active Directory.
  > [!NOTE]
  > Aby wdrożyć zasady dostępu warunkowego programu Exchange ActiveSync na platformie Azure, użytkownik musi być administratorem globalnym.
  
* **[Administratorzy urządzenia](#device-administrators)**: Ta rola jest dostępne do przypisania tylko jako dodatkowego administratora lokalnego w [ustawienia urządzenia](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Użytkownicy z tą rolą stają się administratorami maszyny lokalnej na wszystkich urządzeniach z systemem Windows 10 dołączonych do usługi Azure Active Directory. Nie mają możliwość zarządzania obiektami urządzeń w usłudze Azure Active Directory. 

* **[Odczytywanie katalogów](#directory-readers)**: jest to rola starszej wersji, która ma być przypisana do aplikacji, które nie obsługują [zgody Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Nie powinien zostać przypisany do żadnych użytkowników.

* **[Konta synchronizacji katalogu](#directory-synchronization-accounts)**: nie używaj. Ta rola jest przypisywany do usługi Azure AD Connect i jest nie przeznaczone lub obsługiwane w przypadku innych celów.

* **[Zapisywanie katalogów](#directory-writers)**: jest to rola starszej wersji, która ma być przypisana do aplikacji, które nie obsługują [zgody Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Nie powinien zostać przypisany do żadnych użytkowników.

* **[Dynamics 365 administratora / administratora CRM](#dynamics-365-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft Dynamics 365 Online, jeśli usługa została zainstalowana, a także możliwość zarządzania biletami pomocy technicznej i monitorowania usługi kondycję. Więcej informacji o [używać roli administratora usługi do zarządzania dzierżawą](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Dynamics 365". "Administrator Dynamics 365" znajduje się w witrynie Azure portal.

* **[Administrator programu Exchange](#exchange-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft CRM Online, gdy ta usługa została zainstalowana. a także możliwość tworzenia i zarządzania wszystkich grup usługi Office 365, zarządzanie biletami pomocy technicznej i monitorowania kondycji usługi. Więcej informacji o [ról administratora o usługi Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Exchange". "Administrator programu Exchange" znajduje się w witrynie Azure portal.

* **[Administrator globalny / Administrator firmy](#company-administrator)**: użytkownicy z tą rolą mają dostęp do wszystkich funkcji administracyjnych w usłudze Azure Active Directory, a także usług korzystających z tożsamości usługi Azure Active Directory, takich jak usługa Exchange Online SharePoint Online i Skype dla firm Online. Osoba, która zarejestruje się dla dzierżawy usługi Azure Active Directory staje się administratorem globalnym. Tylko administratorzy globalni mogą przypisywać pozostałe role administratorów. Może istnieć więcej niż jednego administratora globalnego w Twojej firmie. Administratorzy globalni mogą resetować hasła dla wszystkich użytkowników oraz wszystkich innych administratorów.

  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator firmy". Jest on "Administrator globalny" [witryny Azure portal](https://portal.azure.com).
  >
  >

* **[Osoba zapraszająca gości](#guest-inviter)**: użytkownicy w tej roli mogą zarządzać zaproszeniami użytkowników gości B2B usługi Azure Active Directory po **członkowie mogą zapraszać** użytkownika został ustawiony na nie. Więcej informacji na temat współpracy B2B w [współpracy usługi Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Nie ma żadnych innych uprawnień.

* **[Administrator usługi Information Protection](#information-protection-administrator)**: użytkownicy z tą rolą mają wszystkie uprawnienia w usłudze Azure Information Protection. Ta rola umożliwia konfigurowanie etykiet w zasadach usługi Azure Information Protection, Zarządzanie szablonami ochrony i aktywacja ochrony. Ta rola nie przyznaje wszystkie uprawnienia w Centrum usługi Identity Protection, Privileged Identity Management, kondycji usługi Monitor Office 365, lub Office 365 Centrum zabezpieczeń i zgodności.

* **[Administrator usługi Intune](#intune-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft CRM Online, gdy ta usługa została zainstalowana. Ponadto ta rola oferuje możliwość zarządzania użytkownikami i urządzeniami w celu kojarzenia zasad, a także tworzenie grup i zarządzanie nimi. Więcej informacji o [kontrola administracji opartej na rolach (RBAC) w usłudze Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Intune". "Administrator usługi Intune" znajduje się w witrynie Azure portal.

* **[Administrator licencji](#license-administrator)**: użytkownicy w tej roli mogą dodawania, usuwania i aktualizacji przypisań licencji dla użytkowników i grup (z wykorzystaniem Licencjonowanie na podstawie grupy) i zarządzanie lokalizacji użytkowania — na użytkownikach. Rola nie powoduje przyznania możliwość zakupu lub Zarządzaj subskrypcjami Tworzenie lub zarządzać grupami, lub Utwórz lub Zarządzanie użytkownikami poza lokalizacji użytkowania.

* **[Komunikat czytnika Centrum](#message-center-reader)**: użytkownicy w tej roli można monitorować powiadomienia i aktualizacje porad dotyczących kondycji w [Centrum wiadomości usługi Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) dla swojej organizacji w skonfigurowanych usług, takich jak Exchange i usługi Intune i Microsoft Teams. Czytelnicy Centrum wiadomości otrzymasz tygodniowy skróty e-mail wpisów i aktualizacji i mogą udostępniać wpisy Centrum wiadomości w usłudze Office 365. W usłudze Azure AD użytkownicy przypisani do tej roli tylko mają dostęp tylko do odczytu w usługach Azure AD, takich jak użytkownicy i grupy. 

* **[Partner obsługi 1](#partner-tier1-support)**: nie używaj. Ta rola jest przestarzała i zostanie usunięty z usługi Azure AD w przyszłości. Ta rola jest przeznaczony do użytku przez małą liczbę partnerów firmy Microsoft w odsprzedaży i nie jest przeznaczona do użytku ogólnego.

* **[Partner pomocy technicznej w warstwie 2](#partner-tier2-support)**: nie używaj. Ta rola jest przestarzała i zostanie usunięty z usługi Azure AD w przyszłości. Ta rola jest przeznaczony do użytku przez małą liczbę partnerów firmy Microsoft w odsprzedaży i nie jest przeznaczona do użytku ogólnego.

* **[Administrator haseł / Administrator pomocy technicznej](#helpdesk-administrator)**: użytkownicy z tą rolą można zmienić hasła, unieważnienie tokeny odświeżania, zarządzanie żądaniami obsługi i monitorowania kondycji usługi. Unieważnienie token odświeżania wymusza na użytkowniku, aby zalogować się ponownie. Administratorzy pomocy technicznej można resetować hasła i unieważnić tokenów odświeżania innych użytkowników, którzy są użytkowników niebędących administratorami lub członków z następujących ról:
  * Odczytywanie katalogów
  * Osoba zapraszająca gościa
  * Administrator pomocy technicznej
  * Czytelnik Centrum wiadomości
  * Czytnik raportów
  
  <b>Ważne</b>: użytkownicy z tą rolą mogą zmieniać hasła dla osób, które mogą uzyskiwać dostęp do informacje poufne lub prywatne lub krytyczne konfiguracji i spoza usługi Azure Active Directory. Zmiana hasła użytkownika może oznaczać, że możliwość przyjęcia tożsamości i uprawnienia tego użytkownika. Na przykład:
  * Rejestrowanie aplikacji i aplikacji przedsiębiorstwa właścicieli, którzy mogą zarządzać poświadczeniami aplikacje, które są właścicielami. Te aplikacje mogą mieć uprzywilejowany uprawnień w usłudze Azure AD i gdzie indziej nie zostały przyznane dla administratorów pomocy technicznej. Za pomocą tej ścieżki, które Administrator pomocy technicznej może przyjąć tożsamość właściciela aplikacji, a następnie dalej przyjąć tożsamość uprzywilejowanych aplikacji przez zaktualizowanie poświadczeń dla aplikacji.
  * Właścicieli subskrypcji platformy Azure, którzy mogą uzyskiwać dostęp do informacje poufne lub prywatne lub krytyczne konfiguracji na platformie Azure.
  * Grupy zabezpieczeń i grupy usługi Office 365 właścicieli, którzy mogą zarządzać członkostwem. Te grupy mogą udzielać dostępu do informacje poufne lub prywatne lub krytyczne konfiguracji w usłudze Azure AD i w innych miejscach.
  * Administratorzy w innych usługach poza usługi Azure AD, takich jak Exchange Online, zabezpieczenia pakietu Office i Centrum zgodności i zarządzania zasobami ludzkimi systemów.
  * Kierownicy doradcą prawnym i pracownicy działu kadr, którzy mogą uzyskiwać dostęp do poufne lub prywatne informacje, takie jak użytkownicy niebędący administratorami.

  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator pomocy technicznej". Jest on "Administrator haseł" [witryny Azure portal](https://portal.azure.com/).
  >
  
* **[Power BI Administrator](#power-bi-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft Power BI, gdy usługa jest obecna, a także możliwość zarządzania biletami pomocy technicznej i monitorowania kondycji usługi. Więcej informacji o [opis roli administratora usługi Power BI](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Power BI". "Administrator usługi Power BI" znajduje się w witrynie Azure portal.

* **[Administrator ról uprzywilejowanych](#privileged-role-administrator)**: użytkownicy z tą rolą mogą zarządzać przypisaniami ról w usłudze Azure Active Directory, a także w ramach usługi Azure AD Privileged Identity Management. Ponadto ta rola umożliwia zarządzanie wszystkimi aspektami Privileged Identity Management.

  <b>Ważne</b>: Ta rola umożliwia zarządzanie członkostwem we wszystkich rolach usługi Azure AD, łącznie z rolą administratora globalnego. Ta rola nie ma innych możliwości uprzywilejowanego w usłudze Azure AD, takie jak tworzenie lub aktualizowanie użytkowników. Użytkownicy przypisani do tej roli można przyznać samych lub innych dodatkowych uprawnień, przypisując dodatkowych ról.

* **[Czytnik raportów](#reports-reader)**: użytkownicy z tą rolą mogą wyświetlać użycia raportowania danych i raporty pulpitu nawigacyjnego w Centrum administracyjnym usługi Office 365 i kontekstu przyjęcia pakietu w usłudze Power BI. Ponadto rola zapewnia dostęp do logowania w raportach i działań w usłudze Azure AD i interfejsu API raportowania danych zwróconych przez program Microsoft Graph. Użytkownik przypisany do roli czytelnika raportów dostęp tylko odpowiednie użycia i metryk przyjęcia. Nie mają żadnych uprawnień administratora do skonfigurowania ustawień lub dostępu do Centrum administracyjnego konkretnych produktów, takich jak program Exchange. 

* **[Administrator zabezpieczeń](#security-administrator)**: użytkownicy z tą rolą mają wszystkie uprawnienia tylko do odczytu roli czytelnika zabezpieczeń oraz możliwość zarządzania konfiguracją usług związanych z zabezpieczeniami: Azure Active Directory Identity Protection Usługa Azure Information Protection i usługi Office 365 Centrum zabezpieczeń i zgodności. Więcej informacji na temat usługi Office 365 uprawnień znajduje się w temacie [uprawnienia w Centrum zgodności i zabezpieczeń usługi Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | W | Można zrobić |
  | --- | --- |
  | Centrum usługi Identity Protection |<ul><li>Wszystkie uprawnienia roli Czytelnik zabezpieczeń.<li>Ponadto możliwość wykonywania wszystkich operacji IPC z wyjątkiem resetowania haseł. |
  | Privileged Identity Management |<ul><li>Wszystkie uprawnienia roli Czytelnik zabezpieczeń.<li>**Nie można** zarządzania członkostwa w roli usługi Azure AD lub ustawienia. |
  | <p>Monitorowanie kondycji usługi Office 365</p><p>Centrum zabezpieczeń i zgodności usługi Office 365 |<ul><li>Wszystkie uprawnienia roli Czytelnik zabezpieczeń.<li>Można skonfigurować wszystkie ustawienia w funkcji zaawansowanej ochrony przed zagrożeniami (ochrony przed złośliwym oprogramowaniem i wirusami, złośliwy adres URL konfiguracji, adres URL śledzenia itp.). |
  
* **[Czytelnik zabezpieczeń](#security-reader)**: użytkownicy z tą rolą mają globalny dostęp tylko do odczytu, w tym wszystkie informacje w usłudze Azure Active Directory, Identity Protection, Privileged Identity Management, a także możliwość odczytu usługi Azure Active Directory Raporty logowania i dzienników inspekcji. Rola również przyznaje uprawnienia tylko do odczytu w Centrum zgodności i zabezpieczeń usługi Office 365. Więcej informacji na temat usługi Office 365 uprawnień znajduje się w temacie [uprawnienia w Centrum zgodności i zabezpieczeń usługi Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | W | Można zrobić |
  | --- | --- |
  | Centrum usługi Identity Protection |Odczyt wszystkich raporty dotyczące zabezpieczeń i informacje o ustawieniach dla funkcji zabezpieczeń<ul><li>Antyspamowy<li>Szyfrowanie<li>Ochrona przed utratą danych<li>Chroniące przed złośliwym kodem<li>Zaawansowana ochrona przed zagrożeniami<li>Przed wyłudzaniem<li>Reguły Mailflow |
  | Privileged Identity Management |<p>Ma dostęp tylko do odczytu, aby wszystkie informacje są prezentowane z użyciem usługi Azure AD PIM: zasady i raportów dotyczących przypisania roli usługi Azure AD, zabezpieczenia przeglądów i w przyszłości dostęp do odczytu do zasad, dane i raporty dla scenariuszy, oprócz przypisania roli usługi Azure AD.<p>**Nie można** Zarejestruj się w usłudze Azure AD PIM lub wprowadzać żadnych zmian. W portalu firmy usługi PIM lub za pośrednictwem programu PowerShell ktoś jest w tej roli Aktywacja dodatkowe role (na przykład administrator globalny lub Administrator ról uprzywilejowanych), jeśli użytkownik jest kandydatem do nich. |
  | <p>Monitorowanie kondycji usługi Office 365</p><p>Centrum zabezpieczeń i zgodności usługi Office 365</p> |<ul><li>Przeczytaj alerty i zarządzaj nimi<li>Przeczytaj zasady zabezpieczeń<li>Przeczytaj analizy zagrożeń, odnajdywania aplikacji w chmurze i kwarantanny w wyszukiwaniu i Zbadaj<li>Odczytuj wszystkie raporty |

* **[Administrator pomocy technicznej usługi](#service-support-administrator)**: użytkownicy z tą rolą mogą otwierać żądania pomocy technicznej firmy Microsoft dla usług platformy Azure i usługi Office 365 i widoki pulpitu nawigacyjnego usług i Centrum komunikatów w witrynie Azure portal i portalu administracyjnego usługi Office 365. Więcej informacji o [ról administratora o usługi Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrator programu SharePoint](#sharepoint-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft SharePoint Online, gdy usługa jest obecna, a także możliwość tworzenia i zarządzania nimi wszystkich grup usługi Office 365, zarządzanie biletami pomocy technicznej i Monitorowanie kondycji usługi. Więcej informacji o [ról administratora o usługi Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi SharePoint." "Administrator programu SharePoint" znajduje się w witrynie Azure portal.

* **[Skype dla firm / Administrator usługi Lync](#skype-for-business-administrator)**: użytkownicy z tą rolą mają uprawnienia globalne w usłudze Microsoft Skype dla firm, gdy usługa jest obecna, a także zarządzać specyficzne dla programu Skype atrybutów użytkownika w usłudze Azure Active Directory. Ponadto ta rola daje możliwość zarządzania biletami pomocy technicznej i monitorowania kondycji usługi i uzyskać dostęp do zespołów i Skype dla firm Centrum administracyjnego. Konto musi mieć również licencję dla zespołów lub nie można uruchomić polecenia cmdlet programu PowerShell zespołów. Więcej informacji o [dotyczące programu Skype dla firm przypisaną rolę administratora](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) i zespołów, informacje o licencjonowaniu na [Skype dla firm i Microsoft Teams dodatek licencjonowania](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Lync". ""Skype dla firm Administrator znajduje się w [witryny Azure portal](https://portal.azure.com/).

* **[Zespoły administratora komunikacji](#teams-communications-administrator)**: użytkownicy w tej roli mogą zarządzać aspektów Microsoft Teams obciążenie związane z głosu & telefonii. W tym narzędzia do zarządzania dla przypisywania numerów telefonów, głos i spotkania zasady i pełny dostęp do narzędzi analizy wywołania.

* **[Zespoły ze specjalistą pomocy technicznej komunikacji](#teams-communications-support-engineer)**: użytkownicy w tej roli można rozwiązywać problemy z komunikacją w ramach Microsoft Teams i Skype dla firm przy użyciu użytkownika wywołać narzędzia do rozwiązywania problemów w Microsoft Teams i Skype dla Centrum administratora firmy. Użytkownicy w tej roli mogą wyświetlać pełną wywołanie rejestrowanie informacji dla wszystkich uczestników związane.

* **[Zespoły, specjalista ds. pomocy technicznej komunikacji](#teams-communications-support-specialist)**: użytkownicy w tej roli można rozwiązywać problemy z komunikacją w ramach Microsoft Teams i Skype dla firm przy użyciu użytkownika wywołać narzędzia do rozwiązywania problemów w Microsoft Teams i Skype dla Centrum administratora firmy. Użytkownicy w tej roli mogą wyświetlać szczegóły użytkownika tylko w wywołaniu dla określonego użytkownika, że ma wyszukiwać.

* **[Zespoły administratora](#teams-administrator)**: użytkownicy w tej roli mogą zarządzać wszystkimi aspektami obciążenie Microsoft Teams przy użyciu programu Microsoft Teams & Skype Centrum administracyjnego usługi biznesowe i odpowiednie moduły programu PowerShell. W tym, wśród innych obszarów, wszystkie narzędzia do zarządzania związane z telefoniczne, wiadomości, spotkań i same zespoły dbają. Ta rola również przyznaje możliwość tworzenia i zarządzania wszystkich grup usługi Office 365, zarządzanie biletami pomocy technicznej i monitorowania kondycji usługi.
  > [!NOTE]
  > W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "zespoły Administrator usługi". "Administrator zespoły" znajduje się w witrynie Azure portal.

* **[Administrator kont użytkowników](#user-account-administrator)**: użytkownicy z tą rolą mogą tworzyć użytkowników i zarządzać wszystkimi aspektami użytkowników z pewnymi ograniczeniami (patrz poniżej). Ponadto użytkownicy posiadający tę rolę można tworzyć i zarządzać wszystkich grup. Ta rola obejmuje również możliwość tworzenia i zarządzania widoki użytkowników, zarządzanie biletami pomocy technicznej i monitorowania kondycji usługi.

  | | |
  | --- | --- |
  |Uprawnieniami ogólnymi|<p>Tworzenie użytkowników i grup</p><p>Tworzenie i zarządzanie nimi widoki użytkowników</p><p>Zarządzanie biletami pomocy technicznej pakietu Office|
  |<p>Na wszystkich użytkowników w tym wszystkich administratorów</p>|<p>Zarządzanie licencjami</p><p>Zarządzanie wszystkich właściwości użytkownika, z wyjątkiem główna nazwa użytkownika</p>
  |Tylko na użytkowników, którzy są administratorami bez lub w jednym z następujących ograniczonych ról administratora:<ul><li>Odczytywanie katalogów<li>Osoba zapraszająca gościa<li>Administrator pomocy technicznej<li>Czytelnik Centrum wiadomości<li>Czytnik raportów<li>Administrator kont użytkowników|<p>Usuń i przywracania</p><p>Wyłącz i Włącz</p><p>Unieważnienie tokenów odświeżania</p><p>Zarządzanie wszystkich właściwości użytkownika, w tym nazwa główna użytkownika</p><p>Resetowanie hasła</p><p>Aktualizuj klucze urządzenia (FIDO)</p>
  
  <b>Ważne</b>: użytkownicy z tą rolą mogą zmieniać hasła dla osób, które mogą uzyskiwać dostęp do informacje poufne lub prywatne lub krytyczne konfiguracji i spoza usługi Azure Active Directory. Zmiana hasła użytkownika może oznaczać, że możliwość przyjęcia tożsamości i uprawnienia tego użytkownika. Na przykład:
  * Rejestrowanie aplikacji i aplikacji przedsiębiorstwa właścicieli, którzy mogą zarządzać poświadczeniami aplikacje, które są właścicielami. Te aplikacje mogą mają uprzywilejowany uprawnień w usłudze Azure AD i gdzie indziej nie przyznawać Administratorzy użytkowników. Za pomocą tej ścieżki, które Administrator użytkowników może przyjąć tożsamość właściciela aplikacji, a następnie dalej przyjąć tożsamość uprzywilejowanych aplikacji przez zaktualizowanie poświadczeń dla aplikacji.
  * Właścicieli subskrypcji platformy Azure, którzy mogą uzyskiwać dostęp do informacje poufne lub prywatne lub krytyczne konfiguracji na platformie Azure.
  * Grupy zabezpieczeń i grupy usługi Office 365 właścicieli, którzy mogą zarządzać członkostwem. Te grupy mogą udzielać dostępu do informacje poufne lub prywatne lub krytyczne konfiguracji w usłudze Azure AD i w innych miejscach.
  * Administratorzy w innych usługach poza usługi Azure AD, takich jak Exchange Online, zabezpieczenia pakietu Office i Centrum zgodności i zarządzania zasobami ludzkimi systemów.
  * Kierownicy doradcą prawnym i pracownicy działu kadr, którzy mogą uzyskiwać dostęp do poufne lub prywatne informacje, takie jak użytkownicy niebędący administratorami.

W poniższych tabelach opisano określone uprawnienia w usłudze Azure Active Directory do każdej roli. Niektóre role mogą mieć dodatkowe uprawnienia w usługach firmy Microsoft poza usługą Azure Active Directory.

### <a name="application-administrator"></a>Administrator aplikacji
Może tworzyć wszystkie aspekty rejestracji aplikacji i aplikacji przedsiębiorstwa oraz zarządzać nimi.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/Audience/Update | Aktualizowanie właściwości applications.audience w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Authentication/Update | Aktualizowanie właściwości applications.authentication w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Basic/Update | Aktualizowanie właściwości podstawowych w aplikacjach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Applications/Create | Tworzenie aplikacji w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Credentials/Update | Aktualizowanie właściwości applications.credentials w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/DELETE | Usuwanie aplikacji w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/owners/Update | Aktualizowanie właściwości applications.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/permissions/Update | Aktualizowanie właściwości applications.permissions w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/policies/Update | Aktualizowanie właściwości applications.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Tworzenie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Odczytywanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Aktualizowanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Usuwanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Aktualizowanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Tworzenie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Usuwanie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualizowanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualizowanie właściwości podstawowych elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Tworzenie elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Usuwanie elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualizowanie właściwości servicePrincipals.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualizowanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Odczytywanie raportów usługi Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="application-developer"></a>Deweloper aplikacji
Rejestracje aplikacji można tworzyć niezależnie od "Użytkownicy mogą rejestrować aplikacje" ustawienie.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Tworzenie aplikacji w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Tworzenie elementu appRoleAssignments w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Tworzenie elementu oAuth2PermissionGrants w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Tworzenie elementu servicePrincipals w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |

### <a name="billing-administrator"></a>Administrator rozliczeń
Może wykonywać typowe zadania związane z rozliczeniami, takie jak aktualizowanie informacji o płatności.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Organization/Basic/Update | Aktualizowanie właściwości podstawowych w organizacji w usłudze Active Directory Azure. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualizowanie właściwości organization.trustedCAsForPasswordlessAuth w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| microsoft.commerce.billing/allEntities/allTasks | Zarządzaj wszystkimi aspektami rozliczenia usługi Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="desktop-analytics-administrator"></a>Administrator usługi Analytics pulpitu
Można uzyskać dostęp i zarządzać nimi, Zarządzanie pulpitem narzędzi i usług, w tym w usłudze Intune.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.desktopAnalytics/allEntities/allTasks | Zarządzaj wszystkimi aspektami Analytics pulpitu. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="cloud-application-administrator"></a>Administrator aplikacji w chmurze
Może tworzyć wszystkie aspekty rejestracji aplikacji i aplikacji przedsiębiorstwa (z wyjątkiem serwera proxy aplikacji) oraz zarządzać nimi.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/Audience/Update | Aktualizowanie właściwości applications.audience w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Authentication/Update | Aktualizowanie właściwości applications.authentication w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Basic/Update | Aktualizowanie właściwości podstawowych w aplikacjach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Applications/Create | Tworzenie aplikacji w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Credentials/Update | Aktualizowanie właściwości applications.credentials w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/DELETE | Usuwanie aplikacji w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/owners/Update | Aktualizowanie właściwości applications.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/permissions/Update | Aktualizowanie właściwości applications.permissions w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/policies/Update | Aktualizowanie właściwości applications.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Tworzenie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Aktualizowanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Usuwanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Tworzenie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Aktualizowanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Usuwanie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Aktualizowanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Odczytywanie właściwości policies.applicationConfiguration w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualizowanie właściwości podstawowych elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Tworzenie elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Usuwanie elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualizowanie właściwości servicePrincipals.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualizowanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Odczytywanie raportów usługi Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="cloud-device-administrator"></a>Administrator urządzenia w chmurze
Pełny dostęp do zarządzania urządzeniami w usłudze Azure AD.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Devices/DELETE | Usuwanie urządzeń w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/disable | Wyłączanie urządzeń w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/enable | Włącz urządzeń w usłudze Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Odczytywanie raportów usługi Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="company-administrator"></a>Administrator firmy
Może zarządzać wszystkimi aspektami usług Azure AD i Microsoft korzystających z tożsamości usługi Azure AD.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia z roli.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Tworzenie i usuwanie elementu administrativeUnits oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Tworzenie i usuwanie aplikacji oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Tworzenie i usuwanie elementu appRoleAssignments oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Tworzenie i usuwanie kontaktów oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Tworzenie i usuwanie umów oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/allProperties/allTasks | Tworzenie i usuwanie urządzeń oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Tworzenie i usuwanie elementu directoryRoles oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Tworzenie i usuwanie elementu directoryRoleTemplates oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/domains/allProperties/allTasks | Tworzenie i usuwanie domen oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/allProperties/allTasks | Tworzenie i usuwanie grup oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Tworzenie i usuwanie elementu groupSettings oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Tworzenie i usuwanie elementu groupSettingTemplates oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Tworzenie i usuwanie elementu loginTenantBranding oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Tworzenie i usuwanie elementu OAuth2PermissionGrants oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/organization/allProperties/allTasks | Tworzenie i usuwanie organizacji oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/allProperties/allTasks | Tworzenie i usuwanie zasad oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Tworzenie i usuwanie elementu roleAssignments oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Tworzenie i usuwanie elementu roleDefinitions oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Tworzenie i usuwanie elementu ScopedRoleMemberships oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/serviceAction/activateService | Może wykonać akcję usługi Activateservice w usłudze Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Może wykonać akcję usługi Disabledirectoryfeature w usłudze Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Może wykonać akcję usługi Enabledirectoryfeature w usłudze Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Może wykonać akcję usługi Getavailableextentionproperties w usłudze Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Tworzenie i usuwanie elementu servicePrincipals oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Tworzenie i usuwanie elementu subscribedSkus oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Tworzenie i usuwanie użytkowników oraz odczytywanie i aktualizowanie wszystkich właściwości w usłudze Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Wykonywanie wszystkich akcji w programie Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Tworzenie i usuwanie wszystkich zasobów oraz odczytywanie i aktualizowanie właściwości standardowych w elemencie microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Odczytywanie wszystkich zasobów w elemencie microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/allEntities/allTasks | Odczytywanie i konfigurowanie raportów usługi Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.informationProtection/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| microsoft.commerce.billing/allEntities/allTasks | Zarządzaj wszystkimi aspektami rozliczenia usługi Office 365. |
| microsoft.intune/allEntities/allTasks | Zarządzaj wszystkimi aspektami usługi Intune. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Zarządzanie wszystkimi aspektami Menedżera zgodności usługi Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Zarządzaj wszystkimi aspektami usługi Exchange Online. |
| Microsoft.office365.lockbox/allEntities/allTasks | Zarządzanie wszystkimi aspektami Skrytki klienta usługi Office 365 |
| Microsoft.office365.messageCenter/messages/Read | Przeczytaj komunikaty w microsoft.office365.messageCenter. |
| Microsoft.office365.messageCenter/securityMessages/Read | Przeczytaj securityMessages w microsoft.office365.messageCenter. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Power BI. |
| Microsoft.office365.protectionCenter/allEntities/allTasks | Zarządzanie wszystkimi aspektami Centrum ochrony usługi Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Tworzenie i usuwanie wszystkich zasobów oraz odczytywanie i aktualizowanie właściwości standardowych w elemencie microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Skype dla firm Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Dynamics 365. |

### <a name="compliance-administrator"></a>Administrator zgodności
Może odczytywać konfigurację i raporty zgodności oraz zarządzać nimi w usługach Azure AD i Office 365.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Zarządzanie wszystkimi aspektami Menedżera zgodności usługi Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Zarządzaj wszystkimi aspektami usługi Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Tworzenie i usuwanie wszystkich zasobów oraz odczytywanie i aktualizowanie właściwości standardowych w elemencie microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Skype dla firm Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="conditional-access-administrator"></a>Administrator dostępu warunkowego
Może zarządzać możliwościami dostępu warunkowego.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Odczytywanie właściwości policies.conditionalAccess w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Aktualizowanie właściwości policies.conditionalAccess w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Tworzenie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Usuwanie zasad w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Odczytywanie właściwości policies.conditionalAccess w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Aktualizowanie właściwości policies.conditionalAccess w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Odczytywanie właściwości policies.conditionalAccess w usłudze Azure Active Directory. |

### <a name="customer-lockbox-access-approver"></a>Osoba zatwierdzająca dostęp do skrytki klienta
Może zatwierdzać żądania pomocy technicznej firmy Microsoft dotyczące uzyskania dostępu do danych organizacyjnych klienta.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| Microsoft.office365.lockbox/allEntities/allTasks | Zarządzanie wszystkimi aspektami Skrytki klienta usługi Office 365 |

### <a name="device-administrators"></a>Administratorzy urządzenia
Członkowie tej roli są dodawane do grupy Administratorzy lokalni na urządzeniach przyłączonych do usługi AD systemu Azure.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Odczytywanie właściwości podstawowych w elemencie groupSettings w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Odczytywanie właściwości podstawowych w elemencie groupSettingTemplates w usłudze Azure Active Directory. |

### <a name="directory-readers"></a>Odczytywanie katalogów
Może odczytywać informacje o katalogu podstawowego. Przyznawania dostępu do aplikacji, nie przeznaczone dla użytkowników.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia z roli.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Odczytywanie właściwości podstawowych w elemencie administrativeUnit w usłudze Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Odczytywanie właściwości administrativeUnits.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Audience/Read | Odczytywanie właściwości applications.audience w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Authentication/Read | Odczytywanie właściwości Users.OwnedObjects w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Basic/Read | Odczytywanie właściwości podstawowych w aplikacjach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Credentials/Read | Odczytywanie właściwości applications.credentials w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/owners/Read | Odczytywanie właściwości applications.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/permissions/Read | Odczytywanie właściwości applications.permissions w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/policies/Read | Odczytywanie właściwości applications.policies w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Basic/Read | Odczytywanie właściwości podstawowych w kontaktach w usłudze Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Odczytywanie właściwości contacts.memberOf w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/contracts/Basic/Read | Odczytywanie właściwości podstawowych w umowach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/Basic/Read | Odczytywanie właściwości podstawowych w urządzeniach w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Odczytywanie właściwości devices.memberOf w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Odczytywanie właściwości devices.registeredOwners w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Odczytywanie właściwości devices.registeredUsers w usłudze Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/basic/read | Odczytywanie właściwości podstawowych w obiektach directoryRoles w usłudze Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Odczytywanie właściwości directoryRoles.eligibleMembers w usłudze Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Odczytywanie właściwości directoryRoles.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Domains/Basic/Read | Odczytywanie właściwości podstawowych w domenach w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Odczytywanie właściwości groups.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Basic/Read | Odczytywanie właściwości podstawowych w grupach w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Odczytywanie właściwości groups.memberOf w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/members/Read | Odczytywanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/owners/Read | Odczytywanie właściwości groups.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Settings/Read | Odczytywanie właściwości groups.settings w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/read | Odczytywanie właściwości podstawowych w elemencie groupSettings w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Odczytywanie właściwości podstawowych w elemencie groupSettingTemplates w usłudze Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Odczytywanie właściwości standardowych w elemencie oAuth2PermissionGrants w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Organization/Basic/Read | Odczytywanie właściwości podstawowych w organizacji w usłudze Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Odczytywanie właściwości organization.trustedCAsForPasswordlessAuth w usłudze Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/basic/read | Przeczytaj podstawowe właściwości w tym usługi Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/basic/read | Przeczytaj podstawowe właściwości roleDefinitions w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Odczytywanie właściwości servicePrincipals.appRoleAssignedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Odczytywanie właściwości servicePrincipals.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Odczytywanie właściwości podstawowych elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Odczytywanie właściwości servicePrincipals.memberOf w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Odczytywanie właściwości servicePrincipals.oAuth2PermissionGrants w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Odczytywanie właściwości servicePrincipals.ownedObjects w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Odczytywanie właściwości servicePrincipals.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Odczytywanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/basic/read | Odczytywanie właściwości podstawowych w elemencie subscribedSkus w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Odczytywanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Read | Odczytywanie właściwości podstawowych użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Odczytywanie właściwości users.directReports w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Odczytywanie właściwości users.invitedBy w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Odczytywanie właściwości users.invitedUsers w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Odczytywanie właściwości users.manager w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Odczytywanie właściwości users.memberOf w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Odczytywanie właściwości users.oAuth2PermissionGrants w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Odczytywanie właściwości users.ownedDevices w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Odczytywanie właściwości users.ownedObjects w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Odczytywanie właściwości users.registeredDevices w usłudze Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Konta synchronizacji katalogu
Używane tylko przez usługę Azure AD Connect.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia z roli.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Aktualizowanie właściwości organization.dirSync w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Create | Tworzenie zasad w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/DELETE | Usuwanie zasad w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Basic/Read | Odczytywanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Basic/Update | Aktualizowanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/owners/Read | Odczytywanie właściwości policies.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/owners/Update | Aktualizowanie właściwości policies.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Odczytywanie właściwości policies.policiesAppliedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Odczytywanie właściwości servicePrincipals.appRoleAssignedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignedTo w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Odczytywanie właściwości servicePrincipals.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Aktualizowanie właściwości servicePrincipals.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Odczytywanie właściwości podstawowych elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Aktualizowanie właściwości podstawowych elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Tworzenie elementu servicePrincipals w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Odczytywanie właściwości servicePrincipals.memberOf w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Odczytywanie właściwości servicePrincipals.oAuth2PermissionGrants w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Odczytywanie właściwości servicePrincipals.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Aktualizowanie właściwości servicePrincipals.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Odczytywanie właściwości servicePrincipals.ownedObjects w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Odczytywanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualizowanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Wykonywanie wszystkich akcji w programie Azure AD Connect. |

### <a name="directory-writers"></a>Zapisywanie katalogów
Może odczytywać i zapisywać informacje katalogu podstawowego. Przyznawania dostępu do aplikacji, nie przeznaczone dla użytkowników.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/groups/Create | Tworzenie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Tworzenie grup w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualizowanie właściwości groups.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Basic/Update | Aktualizowanie właściwości podstawowych w grupach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/groups/members/Update | Aktualizowanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/owners/Update | Aktualizowanie właściwości groups.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Settings/Update | Aktualizowanie właściwości groups.settings w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/update | Aktualizowanie właściwości podstawowych w elemencie groupSettings w usłudze Active Directory Azure. |
| microsoft.aad.directory/groupSettings/create | Tworzenie elementu groupSettings w usłudze Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Usuwanie elementu groupSettings w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualizowanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Update | Aktualizowanie właściwości podstawowych użytkowników w usłudze Active Directory Azure. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Unieważnianie wszystkich tokenów odświeżania użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | Aktualizowanie właściwości users.manager w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualizowanie właściwości users.userPrincipalName w usłudze Azure Active Directory. |

### <a name="dynamics-365-administrator"></a>Dynamics 365 administratora
Może zarządzać wszystkimi aspektami produktu Dynamics 365. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Dynamics 365 administratora usługi." "Administrator Dynamics 365" znajduje się w witrynie Azure portal.


  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  > Ta rola ma również uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Dynamics 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="exchange-administrator"></a>Administrator programu Exchange
Może zarządzać wszystkimi aspektami produktu Exchange. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Exchange." "Administrator programu Exchange" znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.AAD.Directory/groups/Unified/Create | Utwórz grupy usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/DELETE | Usuń grupy usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/Basic/Update | Zaktualizuj podstawowe właściwości grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/members/Update | Zaktualizuj członkostwo grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/owners/Update | Zaktualizuj własności grup usługi Office 365. |
| Microsoft.office365.Exchange/allEntities/allTasks | Zarządzaj wszystkimi aspektami usługi Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="guest-inviter"></a>Osoba zapraszająca gościa
Może zapraszać użytkowników-gości niezależnie od ustawienia „członkowie mogą zapraszać gości”.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia z roli.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Odczytywanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Read | Odczytywanie właściwości podstawowych użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Odczytywanie właściwości users.directReports w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Odczytywanie właściwości users.invitedBy w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Zapraszanie użytkowników-gości w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Odczytywanie właściwości users.invitedUsers w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Odczytywanie właściwości users.manager w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Odczytywanie właściwości users.memberOf w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Odczytywanie właściwości users.oAuth2PermissionGrants w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Odczytywanie właściwości users.ownedDevices w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Odczytywanie właściwości users.ownedObjects w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Odczytywanie właściwości users.registeredDevices w usłudze Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Administrator pomocy technicznej
Może resetować hasła dla użytkowników niebędących administratorami i administratorów pomocy.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Unieważnianie wszystkich tokenów odświeżania użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/Update | Zaktualizuj hasła dla wszystkich użytkowników w usłudze Azure Active Directory. Zobacz dokumentację online, aby uzyskać więcej szczegółów. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="information-protection-administrator"></a>Administrator usługi Information Protection
Może zarządzać wszystkimi aspektami produktu Azure Information Protection.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="intune-administrator"></a>Administrator usługi Intune
Może zarządzać wszystkimi aspektami produktu Intune. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Intune." "Administrator usługi Intune" znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/Basic/Update | Aktualizowanie właściwości podstawowych w kontaktach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Create | Tworzenie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Usuwanie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/Basic/Update | Aktualizowanie właściwości podstawowych w urządzeniach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Devices/Create | Tworzenie urządzeń w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/DELETE | Usuwanie urządzeń w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Aktualizowanie właściwości devices.registeredOwners w usłudze Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Aktualizowanie właściwości devices.registeredUsers w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualizowanie właściwości groups.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Basic/Update | Aktualizowanie właściwości podstawowych w grupach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/groups/Create | Tworzenie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Tworzenie grup w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| Microsoft.AAD.Directory/groups/DELETE | Usuwanie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Odczytywanie właściwości groups.hiddenMembers w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/members/Update | Aktualizowanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/owners/Update | Aktualizowanie właściwości groups.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Restore | Przywracanie grup w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Settings/Update | Aktualizowanie właściwości groups.settings w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualizowanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Update | Aktualizowanie właściwości podstawowych użytkowników w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Users/Manager/Update | Aktualizowanie właściwości users.manager w usłudze Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| microsoft.intune/allEntities/allTasks | Zarządzaj wszystkimi aspektami usługi Intune. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="license-administrator"></a>Administrator licencji
Można zarządzać licencje produktów dla użytkowników i grup.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Aktualizowanie właściwości users.usageLocation w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="skype-for-business-administrator"></a>Administrator programu Skype dla firm
Może zarządzać wszystkimi aspektami produktu Skype dla firm. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Skype dla administratora usługi biznesowej." ""Skype dla firm Administrator znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Skype dla firm Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="message-center-reader"></a>Czytelnik Centrum wiadomości
Może czytać wiadomości i aktualizacje dla swojej organizacji tylko w Centrum wiadomości usługi Office 365. 

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| Microsoft.office365.messageCenter/messages/Read | Przeczytaj komunikaty w microsoft.office365.messageCenter. |

### <a name="partner-tier1-support"></a>Pomoc techniczna dla partnerów (warstwa 1)
Nie używaj — nie są przeznaczone do użytku ogólnego.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/Basic/Update | Aktualizowanie właściwości podstawowych w kontaktach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Create | Tworzenie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Usuwanie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Create | Tworzenie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Tworzenie grup w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| Microsoft.AAD.Directory/groups/members/Update | Aktualizowanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/owners/Update | Aktualizowanie właściwości groups.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualizowanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Update | Aktualizowanie właściwości podstawowych użytkowników w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Users/DELETE | Usuwanie użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Unieważnianie wszystkich tokenów odświeżania użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | Aktualizowanie właściwości users.manager w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/Update | Zaktualizuj hasła dla wszystkich użytkowników w usłudze Azure Active Directory. Zobacz dokumentację online, aby uzyskać więcej szczegółów. |
| Microsoft.AAD.Directory/Users/Restore | Przywracanie usuniętych użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualizowanie właściwości users.userPrincipalName w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="partner-tier2-support"></a>Pomoc techniczna dla partnerów (warstwa 2)
Nie używaj — nie są przeznaczone do użytku ogólnego.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/Basic/Update | Aktualizowanie właściwości podstawowych w kontaktach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Create | Tworzenie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Usuwanie kontaktów w usłudze Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Tworzenie i usuwanie domen oraz odczytywanie i aktualizowanie właściwości standardowych w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Create | Tworzenie grup w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/DELETE | Usuwanie grup w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/members/Update | Aktualizowanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Restore | Przywracanie grup w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Organization/Basic/Update | Aktualizowanie właściwości podstawowych w organizacji w usłudze Active Directory Azure. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Aktualizowanie właściwości organization.trustedCAsForPasswordlessAuth w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualizowanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Update | Aktualizowanie właściwości podstawowych użytkowników w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Users/DELETE | Usuwanie użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Unieważnianie wszystkich tokenów odświeżania użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | Aktualizowanie właściwości users.manager w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/Update | Zaktualizuj hasła dla wszystkich użytkowników w usłudze Azure Active Directory. Zobacz dokumentację online, aby uzyskać więcej szczegółów. |
| Microsoft.AAD.Directory/Users/Restore | Przywracanie usuniętych użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualizowanie właściwości users.userPrincipalName w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="power-bi-administrator"></a>Power BI Administrator
Może zarządzać wszystkimi aspektami produktu Power BI. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi Power BI." "Administrator usługi Power BI" znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Power BI. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="privileged-role-administrator"></a>Administrator ról uprzywilejowanych
Mogą zarządzać przypisaniami ról w usłudze Azure AD i wszystkimi aspektami Privileged Identity Management.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Aktualizowanie elementu directoryRoles w usłudze Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Tworzenie i usuwanie wszystkich zasobów oraz odczytywanie i aktualizowanie właściwości standardowych w elemencie microsoft.aad.privilegedIdentityManagement. |

### <a name="reports-reader"></a>Czytnik raportów
Może odczytywać raporty logowania i inspekcji.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | Odczytywanie raportów usługi Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Odczytywanie raportów dotyczących użycia usługi Office 365. |

### <a name="security-administrator"></a>Administrator zabezpieczeń
Może odczytywać informacje o zabezpieczeniach i raporty oraz zarządzać konfiguracją w usłudze Azure AD i Office 365.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/policies/Update | Aktualizowanie właściwości applications.policies w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Basic/Update | Aktualizowanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Create | Tworzenie zasad w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/DELETE | Usuwanie zasad w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/owners/Update | Aktualizowanie właściwości policies.owners w usłudze Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Aktualizowanie właściwości servicePrincipals.policies w usłudze Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Odczytywanie wszystkich zasobów w elemencie microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Aktualizowanie wszystkich zasobów w elemencie microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Odczytywanie wszystkich zasobów w elemencie microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Odczytywanie wszystkich aspektów Centrum ochrony usługi Office 365. |
| Microsoft.office365.protectionCenter/allEntities/Update | Aktualizowanie wszystkich zasobów w elemencie microsoft.office365.protectionCenter. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="security-reader"></a>Odczytywanie zabezpieczeń
Może odczytywać informacje zabezpieczające i raporty o zabezpieczeniach w usługach Azure AD i Office 365.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Odczytywanie wszystkich zasobów w elemencie microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Odczytywanie wszystkich zasobów w elemencie microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Odczytywanie wszystkich aspektów Centrum ochrony usługi Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="service-support-administrator"></a>Administrator pomocy technicznej dotyczącej usług
Może odczytywać informacje o kondycji usług i zarządzać biletami pomocy technicznej.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="sharepoint-administrator"></a>Administrator programu SharePoint
Może zarządzać wszystkimi aspektami usługi SharePoint. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "Administrator usługi SharePoint." "Administrator programu SharePoint" znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.AAD.Directory/groups/Unified/DELETE | Usuń grupy usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/Basic/Update | Zaktualizuj podstawowe właściwości grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/members/Update | Zaktualizuj członkostwo grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/owners/Update | Zaktualizuj własności grup usługi Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Tworzenie i usuwanie wszystkich zasobów oraz odczytywanie i aktualizowanie właściwości standardowych w elemencie microsoft.office365.sharepoint. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

### <a name="teams-communications-administrator"></a>Administrator komunikacji w usłudze Teams
Może zarządzać funkcjami rozmów telefonicznych i spotkań w ramach usługi Microsoft Teams.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/policies/Basic/Read | Odczytywanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Odczytywanie raportów dotyczących użycia usługi Office 365. |

### <a name="teams-communications-support-engineer"></a>Inżynier pomocy technicznej komunikacji w usłudze Teams
Może rozwiązywać problemy z komunikacją w usłudze Teams przy użyciu zaawansowanych narzędzi.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/policies/Basic/Read | Odczytywanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="teams-communications-support-specialist"></a>Specjalista pomocy technicznej komunikacji w usłudze Teams
Może rozwiązywać problemy z komunikacją w usłudze Teams przy użyciu podstawowych narzędzi.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| Microsoft.AAD.Directory/policies/Basic/Read | Odczytywanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |

### <a name="teams-administrator"></a>Zespoły administratora
Może zarządzać usługą Microsoft Teams. W interfejsu API Microsoft Graph, interfejs API usługi Azure AD Graph i Azure AD PowerShell ta rola jest rozpoznawana jako "zespoły administratora usługi." "Administrator zespoły" znajduje się w witrynie Azure portal.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

  > [!NOTE]
  > Ta rola ma uprawnienia dodatkowy poza usługą Azure Active Directory. Aby uzyskać więcej informacji zobacz opis roli powyżej.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Odczytywanie właściwości groups.hiddenMembers w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/policies/Basic/Read | Odczytywanie właściwości podstawowych w zasadach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Unified/DELETE | Usuń grupy usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/Basic/Update | Zaktualizuj podstawowe właściwości grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/members/Update | Zaktualizuj członkostwo grup usługi Office 365. |
| Microsoft.AAD.Directory/groups/Unified/owners/Update | Zaktualizuj własności grup usługi Office 365. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Odczytywanie raportów dotyczących użycia usługi Office 365. |

### <a name="user-account-administrator"></a>Administrator kont użytkowników
Może zarządzać wszystkimi aspektami użytkowników i grup, w tym resetowaniem haseł dla administratorów z ograniczonymi uprawnieniami.

  > [!NOTE]
  > Ta rola dziedziczy dodatkowe uprawnienia roli czytelników katalogu.
  >
  >

| **Akcje** | **Opis** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Tworzenie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Usuwanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Aktualizowanie elementu appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Basic/Update | Aktualizowanie właściwości podstawowych w kontaktach w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/Create | Tworzenie kontaktów w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Usuwanie kontaktów w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Aktualizowanie właściwości groups.appRoleAssignments w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Basic/Update | Aktualizowanie właściwości podstawowych w grupach w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/groups/Create | Tworzenie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Tworzenie grup w usłudze Azure Active Directory. Twórca nie zostanie dodany jako pierwszy właściciela, a utworzony obiekt zmniejsza limit przydziału 250 utworzonych obiektów twórcy. |
| Microsoft.AAD.Directory/groups/DELETE | Usuwanie grup w usłudze Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Odczytywanie właściwości groups.hiddenMembers w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/members/Update | Aktualizowanie właściwości groups.members w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/owners/Update | Aktualizowanie właściwości groups.owners w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Restore | Przywracanie grup w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/groups/Settings/Update | Aktualizowanie właściwości groups.settings w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Aktualizowanie właściwości users.appRoleAssignments w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Zarządzanie licencjami użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Basic/Update | Aktualizowanie właściwości podstawowych użytkowników w usłudze Active Directory Azure. |
| Microsoft.AAD.Directory/Users/Create | Tworzenie użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/DELETE | Usuwanie użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Unieważnianie wszystkich tokenów odświeżania użytkowników w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | Aktualizowanie właściwości users.manager w usłudze Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/Update | Zaktualizuj hasła dla wszystkich użytkowników w usłudze Azure Active Directory. Zobacz dokumentację online, aby uzyskać więcej szczegółów. |
| Microsoft.AAD.Directory/Users/Restore | Przywracanie usuniętych użytkowników w usłudze Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Aktualizowanie właściwości users.userPrincipalName w usłudze Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Zarządzanie wszystkimi aspektami usługi Azure Access. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie usługi Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Tworzenie biletów pomocy technicznej platformy Azure i zarządzanie nimi. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Odczytywanie i konfigurowanie kondycji usługi Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Tworzenie i zarządzanie biletami pomocy technicznej usługi Office 365. |

## <a name="deprecated-roles"></a>Przestarzałe ról

Nie można używać następujących ról. One są przestarzałe i zostaną usunięte z usługi Azure AD w przyszłości.

* Administrator licencji ad hoc
* Dołączanie urządzeń
* Menedżerowie urządzenia
* Użytkownicy urządzeń
* Tworzenie użytkowników zweryfikowanych za pośrednictwem poczty e-mail
* Administrator skrzynki pocztowej
* Dołączanie urządzeń w miejscu pracy

## <a name="next-steps"></a>Kolejne kroki

* Aby dowiedzieć się więcej na temat sposobu przypisywania użytkownika jako administratora subskrypcji platformy Azure, zobacz [zarządzanie dostępem przy użyciu RBAC i witryny Azure portal](../../role-based-access-control/role-assignments-portal.md)
* Aby dowiedzieć się więcej o kontrolowaniu dostępu do zasobów na platformie Microsoft Azure, zobacz [Understanding resource access in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md) (Opis dostępu do zasobów na platformie Azure).
* Aby uzyskać więcej informacji dotyczących relacji między usługą Azure Active Directory i subskrypcją platformy Azure, zobacz [Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
