---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227447"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problem: Niestandardowego obrazu. błędy inicjowania obsługi administracyjnej
Błędy inicjowania obsługi administracyjnej wystąpić, jeśli przekazywanie lub przechwytywania obrazu uogólnionej maszyny Wirtualnej jako obrazu wyspecjalizowanej maszyny Wirtualnej lub na odwrót. Pierwsza spowoduje błąd limitu czasu inicjowania obsługi administracyjnej i jego spowoduje błąd inicjowania obsługi administracyjnej. Aby wdrożyć niestandardowy obraz bez błędów, upewnij się, że typ obrazu nie zmienia się podczas procesu przechwytywania.

Poniższa tabela zawiera listę możliwych kombinacji uogólniony, wyspecjalizowana obrazów i typ błędu, które można napotkać, co należy zrobić, aby naprawić błędy.

