---
title: "kryptera aaaRestore Key Vault nyckel och hemligheten för virtuella datorer med Azure Backup | Microsoft Docs"
description: "Lär dig hur toorestore nyckel valvet nyckel och hemlig i Azure Backup med hjälp av PowerShell"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 45214083-d5fc-4eb3-a367-0239dc59e0f6
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 659b2f647493ffcc9494caee111c8bd584ef9c7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Återställa Key Vault-nyckel och Hemlig för krypterade virtuella datorer med Azure Backup
Den här artikeln handlar om med hjälp av Azure VM Backup tooperform återställning av krypterade virtuella Azure-datorer om din nyckel och Hemlig inte finns i hello nyckelvalvet. De här stegen kan också användas om du vill toomaintain en separat kopia av nyckel (nyckel krypteringsnyckeln) och hemlighet (BitLocker-krypteringsnyckeln) för hello återställts VM.

## <a name="prerequisites"></a>Krav
* **Säkerhetskopiering krypterade VMs** - krypterade virtuella Azure-datorer har säkerhetskopierats med Azure Backup. Se hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md) mer information om hur toobackup krypterade virtuella Azure-datorer.
* **Konfigurera Azure Key Vault** – se till att nyckelvalvet toowhich nycklar och hemligheter måste toobe återställas finns redan. Se hello artikel [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md) mer information om hantering av nyckelvalvet.
* **Återställa disk** -Kontrollera att du har utlösts återställningsjobbet för återställning av diskar där krypterade VM [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm). Det beror på att jobbet genererar en JSON-fil i ditt lagringskonto som innehåller nycklar och hemligheter för hello krypterade VM toobe återställs.

## <a name="get-key-and-secret-from-azure-backup"></a>Hämta nyckel och Hemlig från Azure Backup

> [!NOTE]
> När disken har återställts för hello krypterad VM, kan du se till att:
> 1. $details fylls med jobbinformation för återställning disk som anges i [PowerShell steg under återställning hello diskar](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. VM ska skapas från återställda diskar **när nyckel och Hemlig är återställda tookey valvet**.
>
>

Frågan hello återställt diskegenskaper för hello jobbinformation.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Ange hello Azure storage-kontexten och återställa JSON-konfigurationsfil som innehåller nyckel och Hemlig information för krypterade VM.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Återställa nyckeln
När hello JSON-filen har genererats i hello målsökväg som nämns ovan, generera nyckel blob-fil från hello JSON och feed toorestore viktiga cmdlet tooput hello nyckel (KEK) tillbaka i hello nyckelvalvet.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Återställa hemlighet
Använd hello JSON-fil skapas ovan tooget hemliga namn och värde och feed tooset hemliga cmdlet tooput hello hemligheten (BEK) tillbaka i hello nyckelvalvet. **Använda dessa cmdletar om den virtuella datorn är krypterad med BEK och KEK.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Om den virtuella datorn är **krypteras med BEK endast**, generera hemliga blob-fil från hello JSON och feed toorestore hemliga cmdlet tooput hello hemligheten (BEK) tillbaka i hello nyckelvalvet.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Värdet för $secretname kan erhållas genom hänvisar toohello utdata från $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl och via SMS efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Värdet för hello taggen DiskEncryptionKeyFileName är samma som hemliga namnet.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Skapa virtuell dator från återställda disk
Om du har säkerhetskopierat krypterade VM som använder Azure VM Backup hello PowerShell-cmdlets som nämns ovan hjälp du återställer nyckel och hemliga tillbaka toohello nyckelvalvet. När du återställer dem finns hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate krypterats virtuella datorer från återställda disk, nyckel och hemlig.

## <a name="legacy-approach"></a>Äldre metod
hello-metod som nämns ovan fungerar för alla återställningspunkter för hello. Dock är hello äldre metod för att få nyckel och Hemlig information från återställningspunkten giltig för återställningspunkter som är äldre än 11 juli 2017 för virtuella datorer som har krypterats med BEK och KEK. När disken Återställningsjobbet har slutförts för krypterade VM med hjälp av [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm), se till att $rp fylls med ett giltigt värde.

### <a name="restore-key"></a>Återställa nyckeln
Använd hello följande cmdlets tooget nyckel (KEK) information från återställningspunkten och feed det toorestore viktiga cmdlet tooput tillbaka i hello nyckelvalvet.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Återställa hemlighet
Använd hello följande information för cmdlets tooget secret (BEK) från återställningspunkten och feed det tooset hemliga cmdlet tooput tillbaka i hello nyckelvalvet.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. Värdet för $secretname kan erhållas genom att referera toohello utdata för $rp1. KeyAndSecretDetails.SecretUrl och via SMS efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Värdet för hello taggen DiskEncryptionKeyFileName är samma som hemliga namnet.
> 3. Värdet för DiskEncryptionKeyEncryptionKeyURL kan hämtas från nyckelvalvet efter återställning hello nycklar tillbaka och använder [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet
>
>

## <a name="next-steps"></a>Nästa steg
När du återställer nyckel och hemliga tillbaka tookey valvet finns hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate krypterats virtuella datorer från återställda disk, nyckel och hemlig.
