---
title: Jak zmienić, usunąć lub zarządzać grupami zarządzania na platformie Azure
description: Dowiedz się, jak utrzymanie i aktualizowanie hierarchia grup zarządzania.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rithorn
ms.openlocfilehash: a3de0df8fde3b271b7ba9bb9aab01dbcd5c3bf08
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991225"
---
# <a name="manage-your-resources-with-management-groups"></a>Zarządzanie zasobami przy użyciu grup zarządzania

Grupy zarządzania są kontenery, które ułatwiają zarządzanie dostępem, zasady i zgodność w wielu subskrypcjach. Można zmienić, usuwanie i zarządzanie tymi kontenerom uzyskanie hierarchii, które mogą być używane z [usługi Azure Policy](../policy/overview.md) i [platformy Azure na podstawie ról dostęp do formantów (RBAC)](../../role-based-access-control/overview.md). Aby dowiedzieć się więcej na temat grup zarządzania, zobacz [organizowanie zasobów przy użyciu grup zarządzania platformy Azure](overview.md).

Aby wprowadzić zmiany w grupie zarządzania, musi mieć rolę właściciela lub współautora w grupie zarządzania. Aby zobaczyć, jakie uprawnienia ma, wybierz grupę zarządzania a następnie wybierz **IAM**. Aby dowiedzieć się więcej na temat ról RBAC, zobacz [zarządzanie dostępem i uprawnieniami przy użyciu RBAC](../../role-based-access-control/overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Zmień nazwę grupy zarządzania

Możesz zmienić nazwę grupy zarządzania przy użyciu portalu, programu PowerShell lub wiersza polecenia platformy Azure.

### <a name="change-the-name-in-the-portal"></a>Zmień nazwę w portalu

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Wybierz grupę zarządzania, którą chcesz zmienić.

1. Wybierz **Zmień nazwę grupy** opcji w górnej części strony.

   ![Zmień nazwę grupy](./media/detail_action_small.png)

1. Po otwarciu menu, wprowadź nową nazwę, które Twoim zdaniem, które zostaną wyświetlone.

   ![Zmień nazwę grupy](./media/rename_context.png)

1. Wybierz pozycję **Zapisz**.

### <a name="change-the-name-in-powershell"></a>Zmień nazwę w programie PowerShell

Można zaktualizować Użyj nazwę wyświetlaną **AzureRmManagementGroup aktualizacji**. Na przykład aby zmienić nazwę grupy zarządzania z "IT firmy Contoso" do "Contoso grupy", uruchom następujące polecenie:

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Zmień nazwę w interfejsie wiersza polecenia platformy Azure

Wiersza polecenia platformy Azure użyj polecenia update.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Usuwanie grupy zarządzania

Aby usunąć grupę zarządzania, muszą być spełnione następujące wymagania:

1. Nie istnieją żadne podrzędne grupy zarządzania lub subskrypcji w ramach grupy zarządzania.

   - Aby przenieść subskrypcję z grupą zarządzania, zobacz [przenieść subskrypcję do innej grupy zarządzania](#Move-subscriptions-in-the-hierarchy).

   - Aby przenieść grupę zarządzania do innej grupy zarządzania, zobacz [przenieść grup zarządzania w hierarchii](#Move-management-groups-in-the-hierarchy).

1. Masz uprawnienia do zapisu w grupie współautora lub właściciela roli zarządzania w grupie zarządzania. Aby zobaczyć, jakie uprawnienia ma, wybierz grupę zarządzania a następnie wybierz **IAM**. Aby uzyskać więcej informacji na temat ról RBAC, zobacz [zarządzanie dostępem i uprawnieniami przy użyciu RBAC](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Usuwanie w portalu

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Wybierz grupę zarządzania, którą chcesz usunąć.

1. Wybierz **Usuń**

   - Jeśli ikona jest wyłączona, kursor nad swoje selektor myszy na ikonie dowiesz się, powód.

   ![Usuń grupę](./media/delete.png)

1. Brak okno które otwiera potwierdzenie, że chcesz usunąć grupę zarządzania.

   ![Usuń grupę](./media/delete_confirm.png)

1. Wybierz **tak**.

### <a name="delete-in-powershell"></a>Usuwanie w programie PowerShell

Użyj **AzureRmManagementGroup Usuń** polecenia w ramach programu PowerShell, można usunąć grupy zarządzania.

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Usuwanie w interfejsie wiersza polecenia platformy Azure

Za pomocą wiersza polecenia platformy Azure Użyj usunięcia grupy zarządzania polecenia az konta.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Wyświetlanie grup zarządzania

Możesz wyświetlić wszystkie grupy zarządzania już bezpośredniego lub dziedziczonego rolę RBAC z.  

### <a name="view-in-the-portal"></a>Wyświetl w portalu

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Hierarchia grup zarządzania strony obciążeń, których można eksplorować wszystkie grupy zarządzania i subskrypcje, do których masz dostęp do. Wybieranie nazwy grupy przejście w dół poziom w hierarchii. Nawigacja działa tak samo, jak Eksplorator plików.

   ![Main](./media/main.png)

1. Aby wyświetlić szczegóły grupy zarządzania, wybierz **(szczegóły)** łącze obok tytułu grupy zarządzania. Jeśli ten link nie jest dostępna, nie masz uprawnień do wyświetlania tej grupy zarządzania.  

### <a name="view-in-powershell"></a>Widok w programie PowerShell

Polecenie Get-AzureRmManagementGroup do pobrania wszystkich grup.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

Grupę zarządzania pojedynczego informacji Użyj parametru - GroupName

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="view-in-azure-cli"></a>Widok wiersza polecenia platformy Azure

Polecenie listy jest używane do pobierania wszystkich grup.  

```azurecli-interactive
az account management-group list
```

Grupę zarządzania pojedynczego informacji użyj polecenia show

```azurecli-interactive
az account management-group show --name 'Contoso'
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Przenieś subskrypcje w hierarchii

Jednym z powodów tworzenia grupy zarządzania jest powiązać razem subskrypcji. Elementy podrzędne w innej grupie zarządzania można wprowadzić tylko grupy zarządzania i subskrypcje. Subskrypcję, która przenosi do grupy zarządzania dziedziczy wszystkie dostępu użytkownika i zasad z nadrzędnej grupy zarządzania.

Aby przenieść subskrypcję, istnieje kilka uprawnienia, które musi mieć:

- Rola "Właściciel" w subskrypcji podrzędnych.
- Rola "Właściciel" lub "Współautor" w nowej grupie zarządzania nadrzędnej.
- Rola "Właściciel" lub "Współautor" na starym nadrzędna grupa zarządzania.

Aby zobaczyć, jakie uprawnienia ma, wybierz grupę zarządzania a następnie wybierz **IAM**. Aby uzyskać więcej informacji na temat ról RBAC, zobacz [zarządzanie dostępem i uprawnieniami przy użyciu RBAC](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Przeniesienie subskrypcji w portalu

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Dodać istniejącą subskrypcję do grupy zarządzania

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Wybierz grupę zarządzania, którą zamierzasz jako nadrzędną.

1. W górnej części strony wybierz **Dodaj subskrypcję**.

1. Wybierz subskrypcję z listy poprawny identyfikator.

   ![Elementy podrzędne](./media/add_context_sub.png)

1. Wybierz pozycję "Zapisz".

#### <a name="remove-a-subscription-from-a-management-group"></a>Usuń subskrypcję z grupy zarządzania

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Wybierz grupę zarządzania, planowane jest to znaczy bieżącym elementem nadrzędnym.  

1. Na liście, który chcesz przenieść, wybierz Wielokropek na końcu wiersza dla subskrypcji.

   ![Move](./media/move_small.png)

1. Wybierz **przenieść**.

1. W wyświetlonym menu wybierz **nadrzędna grupa zarządzania**.

   ![Move](./media/move_small_context.png)

1. Wybierz pozycję **Zapisz**.

### <a name="move-subscriptions-in-powershell"></a>Przenoszenie subskrypcji w programie PowerShell

Aby przenieść subskrypcję, w programie PowerShell, należy użyć polecenia Add-AzureRmManagementGroupSubscription.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Aby usunąć połączenie między i użyj polecenia Remove-AzureRmManagementGroupSubscription, subskrypcję i grupę zarządzania.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Przenieś subskrypcje w interfejsie wiersza polecenia platformy Azure

Aby przenieść subskrypcję, w interfejsie wiersza polecenia, należy użyć polecenia Dodaj.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Aby usunąć subskrypcję z grupy zarządzania, użyj polecenia remove subskrypcji.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Przeniesienie grup zarządzania w hierarchii  

Gdy użytkownik przenosi się nadrzędna grupa zarządzania, wszystkie zasoby podrzędne, które zawierają grupy zarządzania, subskrypcji, grupy zasobów i Przenieś zasoby z obiektem nadrzędnym.

### <a name="move-management-groups-in-the-portal"></a>Przeniesienie grup zarządzania w portalu

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz **wszystkich usług** > **grup zarządzania**.

1. Wybierz grupę zarządzania, którą zamierzasz jako nadrzędną.

1. W górnej części strony wybierz **Dodaj grupę zarządzania**.

1. W wyświetlonym menu wybierz nową lub użyj istniejącej grupy zarządzania.

   - Wybieranie nowego utworzy nową grupę zarządzania.
   - Wybieranie istniejącego spowoduje wyświetlenie menu rozwijane wszystkich grup zarządzania, który można przenieść do tej grupy zarządzania.  

   ![Przenieś](./media/add_context_MG.png)

1. Wybierz pozycję **Zapisz**.

### <a name="move-management-groups-in-powershell"></a>Przeniesienie grup zarządzania w programie PowerShell

Użyj polecenia AzureRmManagementGroup aktualizacji w programie PowerShell, aby przenieść grupy zarządzania do innej grupy.

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Przeniesienie grup zarządzania w interfejsie wiersza polecenia platformy Azure

Użyj polecenia aktualizacji, aby przenieść grupy do zarządzania przy użyciu wiersza polecenia platformy Azure.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat grup zarządzania, zobacz:

- [Organizowanie zasobów przy użyciu grup zarządzania platformy Azure](overview.md)
- [Tworzenie grup zarządzania w celu organizowania zasobów platformy Azure](create.md)
- [Instalowanie modułu Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Przegląd specyfikacji interfejsu API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Zainstaluj rozszerzenie wiersza polecenia platformy Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)