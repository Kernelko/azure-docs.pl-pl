---
title: Jak dodać lub usunąć właścicieli grupy usługi Azure Active Directory | Dokumentacja firmy Microsoft
description: Dowiedz się, jak dodać lub usunąć właścicieli grupy za pomocą usługi Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: fae68bccbeaa54ca1bab9d77510fe6baecd11fcc
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139724"
---
# <a name="how-to-add-or-remove-group-owners-in-azure-active-directory"></a>Porady: Dodawanie lub usuwanie właściciele grupy w usłudze Azure Active Directory
Grupy usługi Azure Active Directory (Azure AD), są własnością i zarządzane przez właścicieli grupy. Właściciele grupy są przypisywane do zarządzania grupą i jej elementów członkowskich według właściciela zasobu (administrator). Właściciele grupy nie muszą być członkami grupy. Po przypisaniu właściciela grupy właściciela zasobu, można dodać lub usunąć właścicieli.

W niektórych przypadkach jako administrator może nie chcesz przypisać właściciela grupy. W tym przypadku staje się właścicielem grupy. Ponadto właścicieli można przypisać właścicieli do ich grupy, o ile nie zostało to ograniczona w ustawieniach grupy.

## <a name="add-an-owner-to-a-group"></a>Dodawanie właściciela do grupy
Dodaj grupę dodatkowych właścicieli do grupy przy użyciu usługi Azure AD.

### <a name="to-add-a-group-owner"></a>Aby dodać właściciela grupy
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com) przy użyciu konta administratora globalnego dla katalogu.

2. Wybierz **usługi Azure Active Directory**, wybierz opcję **grup**, a następnie wybierz grupę, dla którego chcesz dodać jako właściciela (w tym przykładzie *zasad zarządzania urządzeniami Przenośnymi - Zachodnia*).

3. Na **zasady zarządzania urządzeniami Przenośnymi — omówienie zachodnie** wybierz opcję **właścicieli**.

    ![Zasady zarządzania urządzeniami Przenośnymi — strona przeglądu zachodnie z podświetloną opcją właścicieli](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. Na **właścicieli - Zachodnia — zasady zarządzania urządzeniami Przenośnymi** wybierz opcję **Dodawanie właścicieli**, a następnie wyszukaj i wybierz użytkownika, który będzie nowego właściciela grupy, a następnie wybierz **wybierz**.

    ![Zarządzania urządzeniami Przenośnymi zasad - zachód - właścicieli strony z podświetloną opcją właścicieli Dodaj](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Po zaznaczeniu nowego właściciela, możesz odświeżyć **właścicieli** strony, a nazwa dodane do listy właścicieli jest wyświetlana.

## <a name="remove-an-owner-from-a-group"></a>Usuwanie właściciela z grupy
Usuwanie właściciela z grupy przy użyciu usługi Azure AD.

### <a name="to-remove-an-owner"></a>Można usunąć właściciela
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com) przy użyciu konta administratora globalnego dla katalogu.

2. Wybierz **usługi Azure Active Directory**, wybierz opcję **grup**, a następnie wybierz grupę, dla którego chcesz usunąć właściciela (w tym przykładzie *zasad zarządzania urządzeniami Przenośnymi - Zachodnia*).

3. Na **zasady zarządzania urządzeniami Przenośnymi — omówienie zachodnie** wybierz opcję **właścicieli**.

    ![Zasady zarządzania urządzeniami Przenośnymi — strona przeglądu zachodnie z podświetloną opcją właścicieli](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. Na **właścicieli - Zachodnia — zasady zarządzania urządzeniami Przenośnymi** wybierz użytkownika, aby Usuń jako właściciela grupy, wybierz **Usuń** z strona informacji o użytkownika i wybierz przycisk **tak** o potwierdzenie Twoją decyzję.

    ![Strona informacji użytkownika z podświetloną opcją Remove](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Po usunięciu właściciela możesz wrócić do **właścicieli** strony i zobacz, nazwa została usunięta z listy właścicieli.

## <a name="next-steps"></a>Kolejne kroki
- [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)

- [Polecenia cmdlet usługi Azure Active Directory służące do konfigurowania ustawień grupy](../users-groups-roles/groups-settings-cmdlets.md)

- [Używanie grup do udzielania dostępu do zintegrowanych aplikacji SaaS](../users-groups-roles/groups-saasapps.md)

- [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

- [Polecenia cmdlet usługi Azure Active Directory służące do konfigurowania ustawień grupy](../users-groups-roles/groups-settings-v2-cmdlets.md)
