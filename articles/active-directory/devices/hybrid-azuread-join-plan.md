---
title: Jak skonfigurować urządzenia dołączone hybrydowo do usługi Azure Active Directory | Microsoft Docs
description: Dowiedz się, jak skonfigurować urządzenia dołączone hybrydowo do usługi Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 28344ac7c50b48b472ba6f907b116b3b202de454
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238801"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Jak planowanie implementacji hybrydowej usługi Azure Active Directory join

Podobnie jak w przypadku użytkownika, urządzenie staje się kolejną tożsamością, którą należy chronić oraz używać do zabezpieczania zasobów w dowolnym czasie i miejscu. W tym celu można przenieść tożsamości urządzeń do usługi Azure AD przy użyciu jednej z następujących metod:

- Dołączenie do usługi Azure AD
- Dołączenie hybrydowe do usługi Azure AD
- Rejestracja w usłudze Azure AD

Przenosząc urządzenia do usługi Azure AD, można zmaksymalizować wydajność użytkowników dzięki zastosowaniu logowania jednokrotnego (SSO) w zasobach chmury i zasobach lokalnych. Jednocześnie można zapewnić bezpieczny dostęp do zasobów chmury i zasobów lokalnych przy użyciu [dostępu warunkowego](../active-directory-conditional-access-azure-portal.md).

Jeśli masz lokalne środowisko usługi Active Directory i chcesz dołączyć do usługi Azure AD urządzenia dołączone do domeny, możesz to zrobić przez skonfigurowanie urządzeń dołączonych hybrydowo do usługi Azure AD. Ten artykuł zawiera o powiązanych kroków dotyczących implementacji hybrydowej usługi Azure AD join w danym środowisku. 


## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że czytelnik zna [wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Planowanie implementacji

Aby zaplanować hybrydowego wdrożenia usługi Azure AD, należy się zapoznać z:

|   |   |
|---|---|
|![Zaznacz][1]|Przegląd obsługiwanych urządzeń|
|![Zaznacz][1]|Czynności przeglądu, które należy znać|
|![Zaznacz][1]|Wybierz scenariusz|


 


## <a name="review-supported-devices"></a>Przegląd obsługiwanych urządzeń 

Dołączenie do hybrydowej usługi Azure AD obsługuje urządzenia z szerokiego zakresu Windows. Ponieważ konfiguracji dla urządzeń ze starszymi wersjami systemu Windows wymaga wykonania czynności dodatkowym lub innym, obsługiwanych urządzeń są pogrupowane na dwie kategorie:

**Bieżące urządzenia Windows**

- Windows 10
    
- Windows Server 2016


Dla urządzeń z systemem Windows pulpitu systemu operacyjnego, obsługiwana wersja to Windows Update rozliczenia 10 (wersja 1607) lub nowszej. Najlepszym rozwiązaniem jest uaktualnienie do najnowszej wersji systemu Windows 10.



 **Windows niższego poziomu urządzeń**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


Pierwszym krokiem planowania należy przejrzeć środowiska i ustalić, czy potrzebujesz do obsługi systemu Windows niższego poziomu urządzeń.



## <a name="review-things-you-should-know"></a>Czynności przeglądu, które należy znać

Nie można użyć dołączenie do hybrydowej usługi Azure AD, jeśli środowiska składa się z pojedynczego lasu, który synchronizowania danych tożsamości do więcej niż jednej dzierżawy usługi Azure AD.

Jeśli użytkownik korzysta z narzędzia przygotowywania systemu (Sysprep), upewnij się, że tworzenie obrazów z instalacji systemu Windows, który nie został skonfigurowany na dołączenie do hybrydowej usługi Azure AD.

Jeśli używasz migawkę maszyny wirtualnej (VM), aby utworzyć dodatkowe maszyny wirtualne, upewnij się, że używasz migawki maszyny Wirtualnej, która nie została skonfigurowana dla dołączenie do hybrydowej usługi Azure AD.

Dołączenie do hybrydowej usługi Azure AD Windows niższego poziomu urządzeń:

- **Jest** obsługiwane w środowiskach inne niż federacyjne za pośrednictwem [usługi Azure Active Directory bezproblemowe logowanie jednokrotne](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 

- **Nie jest** obsługiwane w przypadku korzystania z uwierzytelniania usługi Azure AD przekazywanego bez bezproblemowego logowania jednokrotnego.

- **Nie jest** obsługiwane podczas korzystania z roamingu poświadczeń lub profilu użytkownika mobilnego lub w przypadku używania infrastruktury pulpitu wirtualnego (VDI).


Rejestracja systemu Windows Server działającego w roli kontrolera domeny (DC) nie jest obsługiwana.

Jeśli organizacja wymaga dostępu do Internetu za pośrednictwem uwierzytelnionego serwera proxy ruchu wychodzącego, musisz upewnić się, że komputery z systemem Windows 10 mogą pomyślnie uwierzytelnić się na serwerze proxy ruchu wychodzącego. Ponieważ komputery z systemem Windows 10 uruchamiają rejestrację urządzenia przy użyciu kontekstu maszyny, konfigurowanie uwierzytelniania serwera proxy ruchu wychodzącego należy wykonać właśnie przy użyciu kontekstu maszyny.


Dołączenie do hybrydowej usługi Azure AD jest proces automatycznego rejestrowania urządzeń przyłączonych do domeny lokalnej za pomocą usługi Azure AD. Istnieją przypadki, w których nie chcesz wszystkich urządzeń do automatycznej rejestracji. Jeśli to PRAWDA dla Ciebie, zobacz [sposób kontrolowania dołączenie do hybrydowej usługi Azure AD urządzeń](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Wybierz scenariusz

Można skonfigurować hybrydowych usługi Azure AD join w następujących scenariuszach:

- Domeny zarządzane
- Domeny federacyjne  



Jeśli środowisko zawiera zarządzane domeny, dołączenie do hybrydowej usługi Azure AD obsługuje:

- Przekazać za pośrednictwem uwierzytelniania (PTA) za pomocą bezproblemowego logowania jednokrotnego (SSO) 

- Synchronizacja skrótów haseł (wersji) za pomocą bezproblemowego logowania jednokrotnego (SSO) 

Począwszy od wersji 1.1.819.0, program Azure AD Connect zapewnia kreator umożliwiający konfigurowanie dołączania hybrydowego do usługi Azure AD. Kreator pozwala znacznie uprościć proces konfiguracji. Aby uzyskać więcej informacji, zobacz:

- [Konfigurowanie hybrydowego dołączania do usługi Azure Active Directory dla domen federacyjnych](hybrid-azuread-join-federated-domains.md)


- [Konfigurowanie hybrydowego dołączania do usługi Azure Active Directory dla domen zarządzanych](hybrid-azuread-join-managed-domains.md)


 Jeśli zainstalowanie wymaganej wersji programu Azure AD Connect nie jest dostępną opcją w, zobacz [jak ręcznie skonfigurować rejestrację urządzeń](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Konfiguruj dołączenie do usługi Azure Active Directory hybrydowej przez domeny federacyjne](hybrid-azuread-join-federated-domains.md)
> [Konfiguruj hybrydowych usługi Azure Active Directory join dla domeny zarządzanej](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
