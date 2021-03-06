---
title: Najlepsze rozwiązania dotyczące dostępu warunkowego w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
description: Więcej informacji na temat czynności, które należy znać i co jest to należy unikać podczas konfigurowania zasad dostępu warunkowego.
services: active-directory
keywords: dostęp warunkowy do aplikacji, dostęp warunkowy w usłudze Azure AD, zabezpieczenia dostępu do zasobów firmy, zasady dostępu warunkowego
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
ms.date: 08/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4e9f5a9318db813b1a0f16d3599f74fd98e53ffc
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818061"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Najlepsze rozwiązania w zakresie dostępu warunkowego w usłudze Azure Active Directory

Za pomocą [dostępu warunkowego usługi Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), jak autoryzowanego dostępu użytkowników można kontrolować swoje aplikacje w chmurze. Ten artykuł zawiera informacje dotyczące:

- Czynności, które należy znać 
- Co to jest należy unikać wykonywania podczas konfigurowania zasad dostępu warunkowego. 

W tym artykule założono, że korzystały pojęcia i terminologia opisane w temacie [co to jest dostęp warunkowy w usłudze Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>Co to są wymagane w celu zmiany celu zasad działają?

Podczas tworzenia nowych zasad są nie użytkowników, grup, aplikacji lub kontroli dostępu, zaznaczone.

![Aplikacje w chmurze](./media/best-practices/02.png)


Aby ułatwić zasad usługi działa, należy skonfigurować:


|Elementy           | Jak                                  | Dlaczego|
|:--            | :--                                  | :-- |
|**Aplikacje w chmurze** |Musisz wybrać co najmniej jedną aplikację.  | Celem zasad dostępu warunkowego jest umożliwiają kontrolowanie sposobu autoryzowani użytkownicy mogą uzyskiwać dostęp do aplikacji w chmurze.|
| **Użytkownicy i grupy** | Musisz wybrać co najmniej jednego użytkownika lub grupę, która dysponuje autoryzacją do dostępu do aplikacji w wybranej chmurze. | Zasady dostępu warunkowego, która nie ma użytkowników i grup przypisanych, nigdy nie zostanie wywołany. |
| **Kontrola dostępu** | Musisz wybrać kontroli dostępu co najmniej jeden. | Jeśli warunki są spełnione, procesor zasad musi wiedzieć, co należy zrobić.|




## <a name="what-you-should-know"></a>Co należy wiedzieć

### <a name="how-are-assignments-evaluated"></a>Jak są obliczane przypisania

Wszystkie przypisania są logicznie **wykonywana**. Jeśli masz więcej niż jedno przypisanie, skonfigurowane, wszystkie przypisania muszą być spełnione do wyzwolenia zasad.  

Jeśli musisz skonfigurować warunek lokalizacji, która ma zastosowanie do wszystkich połączeń z spoza sieci organizacji:

- Obejmują **wszystkie lokalizacje**
- Wyklucz **wszystkie zaufane adresy IP**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Co należy zrobić, jeśli są zablokowane z portalu administratora usługi Azure AD?

Jeśli użytkownik z zablokowanym dostępem do portalu usługi Azure AD z powodu niepoprawnego ustawienia zasad dostępu warunkowego:

- Sprawdź, czy są inni administratorzy w Twojej organizacji, które jeszcze nie są blokowane. Administrator z uprawnieniami do witryny Azure portal można wyłączyć zasady, które ma wpływ na logowanie. 

- Jeśli żaden z administratorami w organizacji można zaktualizować zasad, należy przesłać żądanie pomocy technicznej. Pomoc techniczna firmy Microsoft można przeglądać i aktualizować zasady dostępu warunkowego, które uniemożliwiają dostęp.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Co się stanie, jeśli masz zasady w klasycznym portalu Azure i skonfigurowany z witryny Azure portal?  

Obie zasady są wymuszane przez usługę Azure Active Directory, a użytkownik uzyskuje dostęp tylko wtedy, gdy są spełnione wszystkie wymagania.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Co się stanie, jeśli masz zasady, w portalu usługi Intune z programem Silverlight i witryny Azure portal?

Obie zasady są wymuszane przez usługę Azure Active Directory, a użytkownik uzyskuje dostęp tylko wtedy, gdy są spełnione wszystkie wymagania.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Co się stanie, jeśli mam wiele zasad dla tego samego użytkownika skonfigurowane?  

Podczas każdego logowania usługi Azure Active Directory ocenia wszystkie zasady i gwarantuje, że spełnione są wszystkie wymagania, przed udzielony dostęp do użytkownika. Blokuj dostęp trumps wszystkich innych ustawień konfiguracji. 


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Dostęp warunkowy działa z programem Exchange ActiveSync?

Tak, można użyć programu Exchange ActiveSync w zasadach dostępu warunkowego.






## <a name="what-you-should-avoid-doing"></a>Co należy zrobić

Framework dostęp warunkowy zapewnia elastyczność konfigurację wspaniałe. Jednak elastyczność oznacza także, należy dokładnie przejrzeć wszystkie zasady konfiguracji przed zwolnieniem, aby uniknąć niepożądane wyniki. W tym kontekście, należy zwrócić szczególną uwagę na przydziały wpływające na zestawy, takie jak **wszyscy użytkownicy / grupy / aplikacje w chmurze**.

W danym środowisku należy unikać następujące konfiguracje:


**Dla wszystkich użytkowników wszystkie aplikacje w chmurze:**

- **Zablokuj dostęp** — ta konfiguracja zablokuje całej organizacji, które ostatecznie nie jest dobrym pomysłem.

- **Wymagają zgodnego urządzenia** — dla użytkowników, które nie zarejestrowali swoich urządzeń, ale te zasady blokują dostęp wszystkie, łącznie z dostępem do portalu usługi Intune. Jeśli jesteś administratorem bez zarejestrowanego urządzenia, ta zasada blokuje powrót do portalu Azure do zmiany zasad.

- **Wymagane było przyłączenie do domeny** — ten blok zasad dostępu również może potencjalnie zablokować dostęp dla wszystkich użytkowników w Twojej organizacji, jeśli nie masz jeszcze urządzenia przyłączone do domeny.


**Dla wszystkich użytkowników, wszystkie aplikacje w chmurze, wszystkie platformy urządzeń:**

- **Zablokuj dostęp** — ta konfiguracja zablokuje całej organizacji, które ostatecznie nie jest dobrym pomysłem.


## <a name="how-should-you-deploy-a-new-policy"></a>Jak możesz wdrożyć nowe zasady

Pierwszym krokiem należy ocenić przy użyciu zasad [narzędzie analizy warunkowej](what-if-tool.md).

Gdy wszystko jest gotowe do wdrożenia nowych zasad w środowisku, można to zrobić w fazach:

1. Zastosuj zasady wśród małej grupy użytkowników i sprawdź, czy działa zgodnie z oczekiwaniami. 

2.  Po rozwinięciu zasady w celu uwzględnienia większej liczby użytkowników, w dalszym ciągu wykluczenia wszystkich administratorów z zasad. Dzięki temu administratorzy nadal mają dostęp, a można zaktualizować zasad, jeśli jest wymagane zmiany.

3. Zastosuj zasady do wszystkich użytkowników tylko wtedy, gdy jest to naprawdę konieczne. 

Najlepszym rozwiązaniem jest tworzenie konta użytkownika, który jest:

- Dedykowane administrowanie zasadami 
- Wykluczone ze wszystkich zasad


## <a name="policy-migration"></a>Migracja zasad

Należy rozważyć Migrowanie zasad, które nie zostały utworzone w witrynie Azure portal ponieważ:

- Teraz można rozwiązać scenariusze, które nie może obsłużyć przed.

- Można zmniejszyć liczbę zasad, którą trzeba zarządzać konsolidując je.   

- Możesz zarządzać wszystkie zasady dostępu warunkowego w jednej centralnej lokalizacji.

- Klasyczny portal Azure został wycofany.   


Aby uzyskać więcej informacji, zobacz [Migrowanie zasad klasycznych w witrynie Azure portal](policy-migration.md).


## <a name="next-steps"></a>Kolejne kroki

Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wymagają usługi MFA dla określonych aplikacji przy użyciu dostępu warunkowego usługi Azure Active Directory](app-based-mfa.md).
