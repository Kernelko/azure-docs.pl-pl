---
title: Prześlij certyfikat usługi Azure Service Management | Dokumentacja firmy Microsoft
description: Dowiedz się, jak można przekazać certyfikatu zarządzania usługami dla witryny Azure portal.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeconnoc
ms.openlocfilehash: 788ce1f2543b9ed76d718cf01d28292f49594414
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037358"
---
# <a name="upload-an-azure-service-management-certificate"></a>Prześlij certyfikat usługi Azure Service Management
Certyfikaty zarządzania umożliwiają uwierzytelnianie przy użyciu klasycznego modelu wdrażania udostępnianych przez platformę Azure. Wiele programów i narzędzi (takich jak Visual Studio lub zestawu SDK platformy Azure) Automatyzuj Konfigurowanie i wdrażanie różnych usług platformy Azure przy użyciu funkcji tych certyfikatów. 

> [!WARNING]
> Ostrożnie! Zezwalaj na tych typów certyfikatów, każdy, kto uwierzytelnia się za pomocą ich do zarządzania subskrypcją, które są skojarzone.
>
>

Jeśli chcesz dowiedzieć się więcej o certyfikatach platformy Azure (takie jak tworzenie certyfikatu z podpisem własnym), zobacz [Omówienie certyfikatów usług Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Można również użyć [usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/) do uwierzytelniania kod klienta do celów usługi automation.

**Uwaga:** musi być administrator współpracujący dla subskrypcji można wykonywać dowolne operacje w obszarze certyfikaty zarządzania. [Dowiedz się więcej](https://go.microsoft.com/fwlink/?linkid=849300) na temat sposobu apletu Dodaj lub usuń Współadministratorzy w nowej witrynie Azure Portal 

## <a name="upload-a-management-certificate"></a>Przekazywanie certyfikatu zarządzania
Po utworzeniu utworzono certyfikat zarządzania, (plik cer z kluczem publicznym) można przesłać go do portalu. Jeśli certyfikat jest dostępny w portalu, każda osoba mająca zgodnego certyfikatu (klucz prywatny) łączenie się za pośrednictwem interfejsu API zarządzania i dostępu do zasobów dla skojarzonej subskrypcji.

1. Zaloguj się do witryny [Azure Portal](http://portal.azure.com).
2. Kliknij przycisk **wszystkich usług** na dolną listę usług platformy Azure, następnie wybierz pozycję **subskrypcje** w _ogólne_ grupy usługi.

    ![Menu subskrypcja](./media/azure-api-management-certs/subscriptions_menu.png)

3. Upewnij się wybrać poprawną subskrypcję, którą chcesz skojarzyć z certyfikatem.     
4. Po wybraniu poprawną subskrypcję, naciśnij klawisz **certyfikaty zarządzania** w _ustawienia_ grupy.

    ![Ustawienia](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Naciśnij klawisz **przekazywanie** przycisku.

    ![Przekaż na stronie certyfikatów](./media/azure-api-management-certs/certificates_page.png)
6. Wypełnij informacje w oknie dialogowym, a następnie naciśnij klawisz **przekazywanie**.

    ![Ustawienia](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Kolejne kroki
Teraz, gdy certyfikat zarządzania skojarzony z subskrypcją, można (po zainstalowaniu zgodnego certyfikatu lokalnie) programowo nawiązać [klasycznego modelu wdrażania interfejsu API REST](https://msdn.microsoft.com/library/azure/mt420159.aspx) i automatyzacja różne zasoby platformy Azure, które również są skojarzone z tą subskrypcją.
