---
title: Uaktywnij Moje role katalogu usługi Azure AD w usłudze PIM | Dokumentacja firmy Microsoft
description: Dowiedz się, jak aktywować ról katalogu usługi Azure AD w usłudze Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59dab4651366c3ad6579e0da660baee0c653d1a3
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666006"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Uaktywnij Moje role katalogu usługi Azure AD w usłudze PIM

Usługa Azure Active Directory (Azure AD) Privileged Identity Management (PIM) upraszcza sposób przedsiębiorstw Zarządzanie uprzywilejowanego dostępu do zasobów w usłudze Azure AD i innych usług online firmy Microsoft, takich jak usługi Office 365 lub Microsoft Intune.  

Jeśli użytkownik wprowadzono kwalifikuje się do roli administracyjnej, oznacza to, że możesz aktywować tej roli, gdy potrzebne do wykonania uprzywilejowanych akcji. Na przykład jeśli zarządzasz od czasu do czasu funkcje usługi Office 365, Administratorzy ról uprzywilejowanych w organizacji może nie mieć możesz stałe Administrator globalny, ponieważ ta rola ma wpływ na inne usługi, za. Zamiast tego są każą kwalifikuje się do ról usługi Azure AD, takich jak Exchange Online Administrator. Możesz poprosić o aktywowanie tej roli, gdy wymagane jego uprawnienia, a następnie będziesz mieć kontroli administratora w okresie wyznaczonym czasie.

Ten artykuł jest przeznaczony dla administratorów, którzy muszą aktywować swoją rolę katalogu usługi Azure AD w usłudze PIM.

## <a name="activate-a-role"></a>Uaktywnij rolę

Gdy trzeba przełączyć w roli katalogu usługi Azure AD, możesz poprosić aktywacji przy użyciu **Moje role** opcji nawigacji w usłudze PIM.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

1. Otwórz **usługi Azure AD Privileged Identity Management**. Aby uzyskać informacje dotyczące sposobu dodawania kafelka PIM do pulpitu nawigacyjnego, zobacz [rozpocząć korzystanie z usługi PIM](pim-getting-started.md).

1. Kliknij przycisk **ról katalogu usługi Azure AD**.

1. Kliknij przycisk **Moje role** umożliwia wyświetlenie listy z kwalifikujących się ról katalogu usługi Azure AD.

    ![Role katalogu usługi AD systemu Azure — Moje role](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Wyszukuje rolę, którą chcesz aktywować.

    ![Role katalogu usługi AD systemu Azure — Moja lista ról](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Kliknij przycisk **Aktywuj** aby otworzyć okienko szczegółów aktywacji roli.

1. Jeśli Twoja rola wymaga uwierzytelniania wieloskładnikowego (MFA), kliknij **zweryfikować swoją tożsamość przed kontynuowaniem**. Musisz uwierzytelnić się jeden raz na sesję.

    ![Przed aktywację roli sprawdzić za pomocą usługi MFA](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Kliknij przycisk **Zweryfikuj moją tożsamość** i postępuj zgodnie z instrukcjami w celu zapewnienia dodatkowej weryfikacji zabezpieczeń.

    ![Dodatkowa weryfikacja zabezpieczeń](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Kliknij przycisk **Aktywuj** aby otworzyć okienko aktywacji.

    ![W okienku aktywacji](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Jeśli to konieczne, należy określić czas rozpoczęcia aktywacji niestandardowych.

1. Określ czas trwania aktywacji.

1. W **Przyczyna aktywacji** Wprowadź przyczynę żądania aktywacji. Niektóre role wymagają podania numeru biletu problemy.

    ![Ukończone okienko aktywacji](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Kliknij przycisk **aktywować**.

    Jeśli rola wymaga zatwierdzenia, jest już aktywna, a rola jest wyświetlana na liście aktywnych ról. Jeśli [rola wymaga zatwierdzenia](./azure-ad-pim-approval-workflow.md) można aktywować, powiadomienie pojawi się w prawym górnym rogu przeglądarki informujące o tym, żądanie oczekuje na zatwierdzenie.

    ![Żądania oczekujące powiadomienia](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Wyświetl stan swoich żądań

Można wyświetlić stan Oczekujące żądania aktywowania.

1. Otwórz program Azure AD Privileged Identity Management.

1. Kliknij przycisk **ról katalogu usługi Azure AD**.

1. Kliknij przycisk **Moje żądania** umożliwia wyświetlenie listy żądań.

    ![Role katalogu usługi AD systemu Azure — Moje żądania](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="use-a-role-immediately-after-activation"></a>Użyj roli od razu po aktywacji

Z powodu buforowania, aktywacji nie występują bezpośrednio w witrynie Azure portal, bez jego odświeżania. Jeśli musisz ograniczyć możliwość opóźnienia po aktywowaniu rolę, możesz użyć **dostęp do aplikacji** strony w portalu. Aplikacje dostępne na tej stronie sprawdź, czy nowego przypisania roli natychmiast.

1. Otwórz program Azure AD Privileged Identity Management.

1. Kliknij przycisk **dostęp do aplikacji** strony.

    ![Dostęp do aplikacji PIM](./media/pim-how-to-activate-role/pim-application-access.png)

1. Kliknij przycisk **usługi Azure Active Directory** ponownie otworzyć portalu na **wszyscy użytkownicy** strony.

    Po kliknięciu tego łącza, wymusić odświeżenie wyboru dla nowej usługi Azure AD przypisań ról.

## <a name="deactivate-a-role"></a>Dezaktywacja roli

Po uaktywnieniu roli automatycznie dezaktywuje to, gdy zostanie osiągnięty limit czasu (czas trwania zakwalifikowania).

Jeśli zadania administratora zostanie ukończone przedwcześnie, można również dezaktywować rolę ręcznie w usłudze Azure AD Privileged Identity Management.

1. Otwórz program Azure AD Privileged Identity Management.

1. Kliknij przycisk **ról katalogu usługi Azure AD**.

1. Kliknij przycisk **Moje role**.

1. Kliknij przycisk **aktywnych ról** Aby wyświetlić listę aktywnych ról.

1. Znajdź roli, wszystko będzie gotowe przy użyciu, a następnie kliknij przycisk **Dezaktywuj**.

## <a name="cancel-a-pending-request"></a>Anulowanie oczekującego żądania

Jeśli nie jest wymagane uaktywnienie roli, która wymaga zatwierdzenia, możesz anulować w dowolnym momencie oczekującego żądania.

1. Otwórz program Azure AD Privileged Identity Management.

1. Kliknij przycisk **ról katalogu usługi Azure AD**.

1. Kliknij przycisk **Moje żądania**.

1. Dla tej roli, którą chcesz anulować, kliknij przycisk **anulować** przycisku.

    Gdy klikniesz przycisk Anuluj, żądanie zostanie anulowane. Aby aktywować rolę ponownie, trzeba będzie przesłać nowe żądanie aktywacji.

   ![Anulowanie oczekującego żądania](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="next-steps"></a>Kolejne kroki

- [Uaktywnij Moje role zasobów platformy Azure w usłudze PIM](pim-resource-roles-activate-your-roles.md)
