---
title: Używanie interfejsu wiersza polecenia platformy Azure do tworzenia przypisania zasad w celu zidentyfikowania niezgodnych zasobów w środowisku platformy Azure
description: Używając programu PowerShell, utwórz przypisanie usługi Azure Policy, aby zidentyfikować niezgodne zasoby.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 4954ca42af1755ea62e7142048d48805397b6a0a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968493"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Tworzenie przypisania zasad w celu zidentyfikowania niezgodnych zasobów w środowisku Azure za pomocą interfejsu wiersza polecenia platformy Azure

Pierwszym krokiem do zrozumienia pojęcia zgodności na platformie Azure jest określenie obecnej sytuacji dotyczącej Twoich zasobów.
Ten przewodnik Szybki start przeprowadzi Cię przez proces tworzenia przypisania zasad w celu zidentyfikowania maszyn wirtualnych, które nie korzystają z dysków zarządzanych.

Po zakończeniu tego procesu pomyślnie zidentyfikujesz maszyny wirtualne, które nie korzystają z dysków zarządzanych. Są one *niezgodne* z przypisaniem zasad.

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku interfejs wiersza polecenia platformy Azure używany jest do utworzenia przypisania zasad i zidentyfikowania niezgodnych zasobów w środowisku Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli chcesz zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik Szybki start będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Wymagania wstępne

Zarejestruj dostawcę zasobów usługi Policy Insights przy użyciu interfejsu wiersza polecenia platformy Azure. Dzięki zarejestrowaniu dostawcy zasobów masz pewność, że subskrypcja będzie z nim współpracować. Aby zarejestrować dostawcę zasobów, musisz mieć uprawnienia do wykonywania operacji rejestrowania dostawcy zasobów. Ta operacja jest uwzględniona w rolach Współautor i Właściciel. Uruchom następujące polecenie, aby zarejestrować dostawcę zasobów:

```azurecli-interactive
az provider register --namespace 'Microsoft.PolicyInsights'
```

Aby uzyskać więcej informacji na temat rejestrowania i przeglądania dostawców zasobów, zobacz [Dostawcy zasobów i ich typy](../../azure-resource-manager/resource-manager-supported-services.md)

Jeśli ta czynność nie została jeszcze wykonana, zainstaluj klienta [ARMClient](https://github.com/projectkudu/ARMClient). Jest to narzędzie, które wysyła żądania HTTP do interfejsów API opartych na usłudze Azure Resource Manager.

## <a name="create-a-policy-assignment"></a>Tworzenie przypisania zasad

W tym przewodniku Szybki start utworzysz przypisanie zasad i przypiszesz definicję **Audit VMs that do not use managed disks** (Przeprowadź inspekcję maszyn wirtualnych, które nie korzystają z dysków zarządzanych). Ta definicja zasad identyfikuje zasoby, które nie spełniają warunków określonych w definicji zasad.

Uruchom następujące polecenie, aby utworzyć przypisanie zasad:

```azurecli-interactive
az policy assignment create --name 'audit-vm-manageddisks' --display-name 'Audit Virtual Machines without Managed Disks Assignment' --scope '<scope>' --policy '<policy definition ID>'
```

Poprzednie polecenie korzysta z następujących informacji:

- **Nazwa** — rzeczywista nazwa przypisania.  W tym przykładzie użyto nazwy *audit-vm-manageddisks*.
- **DisplayName** — nazwa wyświetlana przypisania zasad. W tym przypadku użyj nazwy przypisania *Audit Virtual Machines without Managed Disks Assignment* (Przeprowadź inspekcję maszyn wirtualnych bez dysków zarządzanych).
- **Zasady** — identyfikator definicji zasad, na podstawie której tworzysz przypisanie. W tym przypadku jest to identyfikator definicji zasad *Audit VMs that do not use managed disks* (Przeprowadź inspekcję maszyn wirtualnych, które nie korzystają z dysków zarządzanych). Aby uzyskać identyfikator definicji zasad, uruchom następujące polecenie: `az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks']"`
- **Zakres** — zakres określa, jakie zasoby lub grupy zasobów są wymuszane w ramach przypisania zasad. Może obejmować zarówno subskrypcje, jak i grupy zasobów. Pamiętaj, aby zastąpić fragment &lt;zakres&gt; nazwą grupy zasobów.

## <a name="identify-non-compliant-resources"></a>Identyfikowanie niezgodnych zasobów

Aby wyświetlić zasoby, które są niezgodne z tym nowym przypisaniem, uzyskaj identyfikator przypisania zasad, uruchamiając następujące polecenia:

```azurepowershell-interactive
$policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit Virtual Machines without Managed Disks Assignment' }
$policyAssignment.PolicyAssignmentId
```

Aby uzyskać więcej informacji na temat identyfikatorów przypisań zasad, zobacz [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Następnie uruchom poniższe polecenie, aby uzyskać identyfikatory niezgodnych zasobów, które są danymi wyjściowymi przekazywanymi do pliku JSON:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Wyniki powinny wyglądać podobnie do następujących:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

Wyniki są porównywalne z zawartością zwykle wyświetlaną w obszarze **Niezgodne zasoby** w widoku witryny Azure Portal.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki w tej kolekcji bazują na tym przewodniku Szybki start. Jeśli planujesz kontynuować pracę z kolejnymi samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie zamierzasz kontynuować, usuń utworzone przypisanie, uruchamiając następujące polecenie:

```azurecli-interactive
az policy assignment delete --name 'audit-vm-manageddisks' --scope '/subscriptions/<subscriptionID>/<resourceGroupName>'
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start została przypisana definicja zasad mających na celu zidentyfikowanie niezgodnych zasobów w środowisku platformy Azure.

Aby dowiedzieć się więcej na temat przypisywania zasad i zapewnić zgodność zasobów tworzonych w **przyszłości**, przejdź do samouczka:

> [!div class="nextstepaction"]
> [Tworzenie zasad i zarządzanie nimi](./tutorials/create-and-manage.md)