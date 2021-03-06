---
title: Przywróć klucz usługi Key Vault i klucz tajny dla szyfrowanych maszyn wirtualnych przy użyciu usługi Azure Backup
description: Dowiedz się, jak przywrócić klucz usługi Key Vault i klucz tajny w usłudze Azure Backup przy użyciu programu PowerShell
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ac3c3d8f2a5ae37f1d32f9781f0cdbec0b293e8
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42055373"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Przywróć klucz usługi Key Vault i klucz tajny dla szyfrowanych maszyn wirtualnych przy użyciu usługi Azure Backup
Ten artykuł zawiera informacje o przy użyciu kopii zapasowych maszyn wirtualnych platformy Azure przeprowadzić przywracanie zaszyfrowanych maszyn wirtualnych platformy Azure, z kluczem i wpisem tajnym nie istnieją w magazynie kluczy. Te kroki można również Jeśli chcesz zachować oddzielna kopia key (klucz szyfrowania klucza) i klucz tajny (klucz szyfrowania funkcją BitLocker) dla przywróconej maszyny Wirtualnej.

## <a name="prerequisites"></a>Wymagania wstępne
* **Kopii zapasowej zaszyfrowanych maszyn wirtualnych** — zaszyfrowanych maszyn wirtualnych platformy Azure utworzone kopie zapasowe przy użyciu usługi Azure Backup. Zobacz artykuł [Zarządzanie kopia zapasowa i przywracanie maszyn wirtualnych platformy Azure przy użyciu programu PowerShell](backup-azure-vms-automation.md) szczegółowe informacje na temat sposobu tworzenia kopii zapasowych zaszyfrowanych maszyn wirtualnych platformy Azure.
* **Konfigurowanie usługi Azure Key Vault** — upewnij się, w tym magazynie kluczy, do którego mają być przywracane kluczy i wpisów tajnych jest już obecny. Zobacz artykuł [Rozpoczynanie pracy z usługą Azure Key Vault](../key-vault/key-vault-get-started.md) szczegółowe informacje na temat Zarządzanie usługą key vault.
* **Przywracanie dysku** — upewnij się, że zostało wyzwolone zadanie przywracania przywracania dysków dla zaszyfrowanej maszyny Wirtualnej przy użyciu [— kroki programu PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm). Jest to spowodowane to zadanie generuje plik w formacie JSON na koncie magazynu, zawierający klucze i wpisy tajne zaszyfrowane maszyny wirtualnej do przywrócenia.

## <a name="get-key-and-secret-from-azure-backup"></a>Pobieranie klucza i wpisu tajnego z usługi Azure Backup

> [!NOTE]
> Gdy dysk został przywrócony do zaszyfrowanej maszyny Wirtualnej, upewnij się, że:
> 1. $details jest wypełniana szczegóły zadania przywracania dysku, zgodnie z opisem w [PowerShell kroki przywracania sekcji dysków](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. Należy utworzyć maszyny Wirtualnej z przywróconych dysków tylko **po przywróceniu klucza i wpisu tajnego usługi key vault**.
>
>

Tworzenie zapytań o właściwości przywróconego dysku, aby uzyskać szczegóły zadania.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Ustaw kontekst magazynu platformy Azure i przywrócić pliku konfiguracji JSON zawierający klucza i wpisu tajnego szczegóły dla zaszyfrowanej maszyny Wirtualnej.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Przywracanie klucza
Po wygenerowaniu pliku JSON w ścieżce docelowej w wymienionych powyżej wygenerować plik obiektu blob klucza z danych JSON i źródła danych do przywrócenia klucza polecenia cmdlet, aby przełączyć klucza (KEK), wróć do usługi key vault.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Przywróć klucz tajny
Użyj pliku JSON, wygenerowany powyżej, nazwa wpisu tajnego i wartości do niego, aby ustawić wpisu tajnego polecenia cmdlet, aby umieścić wpis tajny (klucz szyfrowania bloków) ponownie w usłudze key vault. **Użyj tych poleceń cmdlet, jeśli maszyna wirtualna jest zaszyfrowana przy użyciu BEK i KEK.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Jeśli maszyna wirtualna jest **szyfrowany tylko przy użyciu klucza szyfrowania bloków**, generowanie pliku obiektu blob klucza tajnego z kodu JSON i źródła danych do przywrócenia klucza tajnego polecenia cmdlet, aby umieścić wpis tajny (klucz szyfrowania bloków) w magazynie kluczy.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Wartość $secretname można uzyskać przez odwołujący się do danych wyjściowych $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl i przy użyciu tekstu po wpisów tajnych / np. adres URL wpisu tajnego danych wyjściowych jest https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 i nazwa wpisu tajnego jest B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Wartość tagu DiskEncryptionKeyFileName jest taka sama jak nazwa wpisu tajnego.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Tworzenie maszyny wirtualnej na podstawie przywróconego dysku
Jeśli utworzono kopię zapasową zaszyfrowanej maszyny Wirtualnej przy użyciu kopii zapasowych maszyn wirtualnych platformy Azure, poleceń cmdlet programu PowerShell wymienionej powyżej pozwalają Przywracanie klucza i wpisu tajnego wstecz do magazynu kluczy. Po przywróceniu ich, zobacz artykuł [Zarządzanie kopia zapasowa i przywracanie maszyn wirtualnych platformy Azure przy użyciu programu PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) Aby utworzyć zaszyfrowanych maszyn wirtualnych na podstawie przywróconego dysku, klucza i wpisu tajnego.

## <a name="legacy-approach"></a>Podejście starszej wersji
Podejście, o których wspomniano powyżej, będzie działać dla wszystkich punktów odzyskiwania. Jednak starsze metody pobierania klucza i wpisu tajnego informacje z punktu odzyskiwania będzie obowiązywać punkty odzyskiwania starsze niż 11 lipca 2017 r. za maszyny wirtualne szyfrowane przy użyciu BEK i KEK. Po zakończeniu przywracania dysku zadania dla zaszyfrowanej maszyny Wirtualnej przy użyciu [— kroki programu PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm), upewnij się, $rp jest wypełniana prawidłową wartość.

### <a name="restore-key"></a>Przywracanie klucza
Użyj następujących poleceń cmdlet, aby uzyskać informacje o kluczu (KEK) z punktu odzyskiwania i źródła danych do przywrócenia klucza polecenia cmdlet, aby umieścić go w magazynie kluczy.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Przywróć klucz tajny
Użyj następujących poleceń cmdlet pobrać informacji o secret (klucz szyfrowania bloków) z punktu odzyskiwania do niego można ustawić klucza tajnego polecenia cmdlet, aby umieścić go w magazynie kluczy.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. Wartość $secretname można uzyskać, odwołując się do danych wyjściowych po1 $. KeyAndSecretDetails.SecretUrl i przy użyciu tekstu po wpisów tajnych / np. wpis tajny w danych wyjściowych adres URL jest https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 i nazwa wpisu tajnego jest B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Wartość tagu DiskEncryptionKeyFileName jest taka sama jak nazwa wpisu tajnego.
> 3. Wartość DiskEncryptionKeyEncryptionKeyURL można uzyskać z magazynu kluczy po przywracania kluczy powrót i używanie [Get-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) polecenia cmdlet
>
>

## <a name="next-steps"></a>Kolejne kroki
Po przywróceniu klucza i wpisu tajnego wstecz do magazynu kluczy, zobacz artykuł [Zarządzanie kopia zapasowa i przywracanie maszyn wirtualnych platformy Azure przy użyciu programu PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) Aby utworzyć zaszyfrowanych maszyn wirtualnych na podstawie przywróconego dysku, klucza i wpisu tajnego.
