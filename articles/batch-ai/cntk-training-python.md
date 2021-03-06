---
title: Szkolenie modelu CNTK przy użyciu usługi Azure Batch AI — Python | Microsoft Docs
description: Szkolenie sieci neuronowej Microsoft Cognitive Toolkit (CNTK) przy użyciu usługi Azure Batch AI i zestawu SDK dla języka Python
services: batch-ai
documentationcenter: na
author: lliimsft
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: Python
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 0c805deb85a999d3c23be24b81c1d97ed5fe55eb
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057695"
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Uruchamianie zadania szkoleniowego CNTK przy użyciu zestawu Azure Python SDK

W tym artykule pokazano, jak używać zestawu Azure Python SDK do szkolenia przykładowego modelu Microsoft Cognitive Toolkit (CNTK) przy użyciu usługi Batch AI.

W tym przykładzie używa się bazy danych MNIST ręcznie zapisanych obrazów do szkolenia splotowej sieci neuronowej (CNN) w jednowęzłowym klastrze procesorów GPU.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja Azure — jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Azure Python SDK — zobacz [instrukcje dotyczące instalacji](/python/azure/python-sdk-azure-install). W tym artykule wymagany jest co najmniej pakiet azure-mgmt-batchai w wersji 2.0.0.

* Konto magazynu platformy Azure — zobacz [Tworzenie konta magazynu platformy Azure](../storage/common/storage-create-storage-account.md)

* Poświadczenia nazwy głównej usługi Azure Active Directory — zobacz [Tworzenie nazwy głównej usługi przy użyciu interfejsu wiersza polecenia](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Zarejestruj jeden raz dostawcę zasobów usługi Batch AI dla subskrypcji przy użyciu programu Azure Cloud Shell lub interfejsu wiersza polecenia platformy Azure. Rejestracja dostawcy może potrwać do 15 minut.

```azurecli
az provider register -n Microsoft.BatchAI
```

## <a name="configure-credentials"></a>Konfiguracja poświadczeń

Dodaj następujący kod do pliku skryptu i zastąp element `FILL-IN-HERE` odpowiednimi wartościami:

```Python
# credentials used for authentication
aad_client_id = 'FILL-IN-HERE'
aad_secret = 'FILL-IN-HERE'
aad_tenant = 'FILL-IN-HERE'
subscription_id = 'FILL-IN-HERE'

# credentials used for storage
storage_account_name = 'FILL-IN-HERE'
storage_account_key = 'FILL-IN-HERE'

# specify the credentials used to remote login your GPU node
admin_user_name = 'FILL-IN-HERE'
admin_user_password = 'FILL-IN-HERE'

# specify the location in which to create Batch AI resources
mylocation = 'eastus'
```

Pamiętaj, że wprowadzanie poświadczeń do kodu źródłowego nie jest dobrym rozwiązaniem i jest wykonywane tutaj w celu uproszczenia pracy z przewodnikiem Szybki start.
Rozważ użycie zamiast tego zmiennych środowiskowych lub osobnego pliku konfiguracji.

## <a name="create-batch-ai-client"></a>Tworzenie klienta usługi Batch AI

Poniższy kod tworzy obiekt poświadczeń jednostki usługi i klienta usługi Batch AI:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=aad_client_id, secret=aad_secret, tenant=aad_tenant)

batchai_client = batchai.BatchAIManagementClient(
    credentials=creds, subscription_id=subscription_id)
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Klastry i zadania usługi Batch AI to zasoby platformy Azure i muszą być umieszczone w grupie zasobów platformy Azure. Poniższy fragment kodu tworzy grupę zasobów:

```Python
from azure.mgmt.resource import ResourceManagementClient

resource_group_name = 'myresourcegroup'
resource_management_client = ResourceManagementClient(
        credentials=creds, subscription_id=subscription_id)
resource = resource_management_client.resource_groups.create_or_update(
        resource_group_name, {'location': mylocation})
```


## <a name="prepare-azure-file-share"></a>Przygotowanie udziału plików platformy Azure

W celach ilustracyjnych ten przewodnik Szybki start używa udziału plików platformy Azure do hostowania danych szkoleniowych i skryptów na potrzeby zadania szkolenia.

Utwórz udział plików o nazwie `batchaiquickstart`.

```Python
from azure.storage.file import FileService
azure_file_share_name = 'batchaiquickstart'
service = FileService(storage_account_name, storage_account_key)
service.create_share(azure_file_share_name, fail_on_exist=False)
```

Utwórz katalog w udziale o nazwie `mnistcntksample`.

```Python
mnist_dataset_directory = 'mnistcntksample'
service.create_directory(azure_file_share_name, mnist_dataset_directory, fail_on_exist=False)
```
Pobierz [przykładowy pakiet](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) i rozpakuj go do bieżącego katalogu. Poniższy kod przekazuje wymagane pliki do udziału plików platformy Azure:

```Python
for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt',
          'ConvNet_MNIST.py']:
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f)
```

## <a name="create-batch-ai-workspace"></a>Tworzenie obszaru roboczego usługi Batch AI

Obszar roboczy jest kolekcją najwyższego poziomu wszystkich typów zasobów usługi Batch AI. W obszarze roboczym można utworzyć klaster usługi Batch AI i eksperymenty.

```Python
workspace_name='myworkspace'
batchai_client.workspaces.create(resource_group_name, workspace_name, mylocation)
```

## <a name="create-gpu-cluster"></a>Tworzenie klastra procesora GPU

Utwórz klaster usługi Batch AI. W tym przykładzie klaster zawiera jeden węzeł maszyny wirtualnej STANDARD_NC6. Rozmiar maszyny wirtualnej ma jeden procesor GPU NVIDIA K80. Zainstaluj udział plików w folderze o nazwie `azurefileshare`. Pełna ścieżka tego folderu w węźle obliczeniowym procesora GPU to `$AZ_BATCHAI_MOUNT_ROOT/azurefileshare`.

```Python
cluster_name = 'mycluster'

relative_mount_point = 'azurefileshare'

parameters = models.ClusterCreateParameters(
    # VM size. Use N-series for GPU
    vm_size='STANDARD_NC6',
    # Configure the ssh users
    user_account_settings=models.UserAccountSettings(
        admin_user_name=admin_user_name,
        admin_user_password=admin_user_password),
    # Number of VMs in the cluster
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=1)
    ),
    # Configure each node in the cluster
    node_setup=models.NodeSetup(
        # Mount shared volumes to the host
        mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=storage_account_key),
                    azure_file_url='https://{0}/{1}'.format(
                        service.primary_endpoint, azure_file_share_name),
                    relative_mount_path=relative_mount_point)],
        ),
    ),
)
batchai_client.clusters.create(resource_group_name, workspace_name, cluster_name,
                               parameters).result()
```

## <a name="get-cluster-status"></a>Pobieranie stanu klastra

Monitoruj stan klastra przy użyciu następującego polecenia:

```Python
cluster = batchai_client.clusters.get(resource_group_name, workspace_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; Leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count))
```

Powyższy kod wyświetla podstawowe informacje dotyczące alokacji klastra, takie jak w poniższym przykładzie:

```
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving: 0
```

Klaster będzie gotowy, gdy węzły zostaną przydzielone i zostanie zakończone przygotowanie (patrz atrybut `nodeStateCounts`). Jeśli wystąpił błąd, atrybut `errors` zawiera opis błędu.

## <a name="create-experiment-and-training-job"></a>Tworzenie eksperymentu i zadania szkolenia

Po utworzeniu klastra utwórz eksperyment (kontener logiczny dla grupy powiązanych zadań). Następnie skonfiguruj i prześlij zadanie edukacyjne w eksperymencie:

```Python
experiment_name='myexperiment'

batchai_client.experiments.create(resource_group_name, workspace_name, experiment_name)

job_name = 'myjob'

parameters = models.JobCreateParameters(
    # The cluster this job will run on
    cluster=models.ResourceId(id=cluster.id),
    # The number of VMs in the cluster to use
    node_count=1,
    # Write job's standard output and execution log to Azure File Share
    std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(
        relative_mount_point),
    # Configure location of the training script and MNIST dataset
    input_directories=[models.InputDirectory(
        id='SAMPLE',
        path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(
            relative_mount_point, mnist_dataset_directory))],
    # Specify location where generated model will be stored
    output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")],
    # Container configuration
    container_settings=models.ContainerSettings(
        image_source_registry=models.ImageSourceRegistry(
            image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')),
    # Toolkit specific settings
    cntk_settings=models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
)

# Create the job
batchai_client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name, parameters).result()
```

## <a name="monitor-job"></a>Monitorowanie zadania

Możesz sprawdzić stan zadania przy użyciu następującego kodu:

```Python
job = batchai_client.jobs.get(resource_group_name, workspace_name, experiment_name, job_name)

print('Job state: {0} '.format(job.execution_state))
```

Dane wyjściowe są podobne do następujących: `Job state: running`.

Atrybut `executionState` zawiera bieżący stan wykonania zadania:
* `queued`: zadanie oczekuje na dostępność węzłów klastra
* `running`: zadanie zostało uruchomione
* `succeeded` (lub `failed`): zadanie zostało zakończone; atrybut `executionInfo` zawiera szczegóły dotyczące wyniku

## <a name="list-stdout-and-stderr-output"></a>Wyświetlanie danych wyjściowych dzienników stdout i stderr

Użyj następującego kodu w celu wyświetlenia listy wygenerowanych plików stdout, stderr i dzienników:

```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, workspace_name, experiment_name, job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="stdouterr"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="list-generated-model-files"></a>Wyświetlanie listy wygenerowanych plików modelu
Użyj następującego kodu, aby wyświetlić listę wygenerowanych plików modelu:
```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, workspace_name, experiment_name,job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="MODEL"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="delete-resources"></a>Usuwanie zasobów

Użyj następującego kodu, aby usunąć zadanie:
```Python
batchai_client.jobs.delete(resource_group_name, workspace_name, experiment_name, job_name)
```

Użyj następującego kodu, aby usunąć klaster:
```Python
batchai_client.clusters.delete(resource_group_name, workspace_name, cluster_name)
```

Użyj następującego kodu, aby usunąć wszystkie przydzielone zasoby:
```Python
resource_management_client.resource_groups.delete('myresourcegroup')
```
## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o używaniu usługi Batch AI z różnymi strukturami, zobacz [przepisy na szkolenie](https://github.com/Azure/BatchAI).