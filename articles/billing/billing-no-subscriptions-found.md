---
title: Nie znaleziono żadnych subskrypcji błąd podczas próby Zaloguj się do witryny Azure portal lub Centrum konta platformy Azure | Dokumentacja firmy Microsoft
description: Zawiera rozwiązanie problemu, w którym subskrypcji nie można odnaleźć wystąpienia błędu podczas Zaloguj się do witryny Azure portal lub Centrum konta platformy Azure.
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: a1e90f946508f1ffc0a1ee812dde46ee733d715a
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392443"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Nie znaleziono żadnych subskrypcji błąd w witrynie Azure portal lub w Centrum konta platformy Azure

Można otrzymać komunikat o błędzie "Nie znaleziono żadnych subskrypcji", gdy próbuje zalogować się do [witryny Azure portal](https://portal.azure.com/) lub [Centrum konta platformy Azure](https://account.windowsazure.com/Subscriptions). Ten artykuł zawiera rozwiązanie tego problemu.

## <a name="symptom"></a>Objaw

Podczas logowania się w [witryny Azure portal](https://portal.azure.com/) lub [Centrum konta platformy Azure](https://account.windowsazure.com/Subscriptions), pojawi się następujący komunikat o błędzie: "Nie znaleziono żadnych subskrypcji".

## <a name="cause"></a>Przyczyna

Ten problem występuje w przypadku wybrania niewłaściwego katalogu, czy Twoje konto nie ma wystarczających uprawnień. 

## <a name="solution"></a>Rozwiązanie

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Scenariusz 1: Odebrano komunikat o błędzie w [witryny Azure portal](https://portal.azure.com)

Aby rozwiązać ten problem:

* Upewnij się, że wybrano prawidłowy katalog usługi Azure, klikając swoje konto w prawym górnym rogu.

  ![Wybierz katalog, w prawym górnym rogu witryny Azure portal](./media/billing-no-subscriptions-found/directory-switch.png)
* Jeśli wybrano katalogu bezpośrednio do platformy Azure, ale nadal otrzymywać komunikat o błędzie [przypisanie roli właściciela do swojego konta](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Scenariusz 2: Odebrano komunikat o błędzie w [Centrum konta platformy Azure](https://account.windowsazure.com/Subscriptions)

Sprawdź, czy konta, którego użyto jest administratora konta. Aby sprawdzić, kim jest Administrator konta, wykonaj następujące kroki:

1. Zaloguj się do [subskrypcje wyświetlić w witrynie Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wybierz subskrypcję, aby sprawdzić, a następnie sprawdź w obszarze **ustawienia**.
1. Wybierz **właściwości**. Administrator konta subskrypcji są wyświetlane w **administrator konta** pole.  

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.

Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) można szybko rozwiązać swój problem. 
