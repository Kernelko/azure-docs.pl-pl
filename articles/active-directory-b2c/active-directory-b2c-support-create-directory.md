---
title: Rozwiązywanie problemów z tworzeniem dzierżaw w usłudze Azure Active Directory B2C | Dokumentacja firmy Microsoft
description: Problemy i rozwiązania do tworzenia dzierżawy usługi Azure Active Directory lub Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 009f7ac2f7e614b7e07623e41888973f1a2b254d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440985"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Rozwiązywanie problemów podczas tworzenia dzierżawy usługi Azure Active Directory lub Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Tworzenie dzierżawy usługi Azure AD
Jeśli nie można utworzyć dzierżawę usługi Azure Active Directory (Azure AD) przy pierwszej próbie, spróbuj ponownie. Jeśli problem będzie się powtarzać, skontaktuj się z działem pomocy technicznej systemu Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure AD B2C
Jeśli wystąpią problemy po użytkownik [Tworzenie usługi Azure Active Directory B2C dzierżawie (Azure AD B2C)](active-directory-b2c-get-started.md), wypróbuj następujące opcje:

* Jeśli dzierżawa usługi Azure AD B2C nie jest wyświetlane na liście dzierżaw, spróbuj ponownie utworzyć dzierżawcę.
* Jeśli dzierżawa usługi Azure AD B2C jest wyświetlany na liście dzierżaw i zostanie wyświetlony następujący komunikat o błędzie, należy usunąć dzierżawę i utworzyć ją ponownie:

    "Nie można ukończyć tworzenia dzierżawy B2C"contosob2c". Skorzystaj z tego [łącze](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) Aby uzyskać więcej wskazówek. "
* Istnieją znane problemy podczas usuwania istniejącej dzierżawy usługi Azure AD B2C i utwórz go ponownie przy użyciu tej samej nazwy domeny. Podczas tworzenia nowej dzierżawy usługi Azure AD B2C, należy użyć nazwy innej domeny.
* Jeśli te rozwiązania nie działają, skontaktuj się z działem pomocy technicznej systemu Azure. Aby uzyskać więcej informacji, zobacz [pliku żądania pomocy technicznej dla usługi Azure AD B2C](active-directory-b2c-support.md).

