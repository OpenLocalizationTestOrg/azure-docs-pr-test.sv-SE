---
title: "Återställa Key Vault-nyckel och Hemlig för krypterade virtuella datorer med Azure Backup | Microsoft Docs"
description: "Lär dig hur du återställer Key Vault-nyckel och hemlig i Azure Backup med hjälp av PowerShell"
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
ms.openlocfilehash: f2db3449187d655248b13198b268841052570626
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="0937b-103">Återställa Key Vault-nyckel och Hemlig för krypterade virtuella datorer med Azure Backup</span><span class="sxs-lookup"><span data-stu-id="0937b-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="0937b-104">Den här artikeln handlar om att använda Azure VM säkerhetskopiering för att utföra återställning av krypterade virtuella Azure-datorer om din nyckel och Hemlig inte finns i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-104">This article talks about using Azure VM Backup to perform restore of encrypted Azure VMs, if your key and secret do not exist in the key vault.</span></span> <span data-ttu-id="0937b-105">De här stegen kan också användas om du vill upprätthålla en separat kopia av nyckel (nyckel krypteringsnyckeln) och hemlighet (BitLocker-krypteringsnyckeln) för den återställda virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0937b-105">These steps can also be used if you want to maintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for the restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0937b-106">Krav</span><span class="sxs-lookup"><span data-stu-id="0937b-106">Prerequisites</span></span>
* <span data-ttu-id="0937b-107">**Säkerhetskopiering krypterade VMs** - krypterade virtuella Azure-datorer har säkerhetskopierats med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="0937b-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="0937b-108">Läs artikeln [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md) mer information om hur du säkerhetskopierar krypterade virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="0937b-108">Refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how to backup encrypted Azure VMs.</span></span>
* <span data-ttu-id="0937b-109">**Konfigurera Azure Key Vault** – Kontrollera att nyckelvalvet som nycklar och hemligheter ska återställas finns redan.</span><span class="sxs-lookup"><span data-stu-id="0937b-109">**Configure Azure Key Vault** – Ensure that key vault to which keys and secrets need to be restored is already present.</span></span> <span data-ttu-id="0937b-110">Läs artikeln [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md) mer information om hantering av nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-110">Refer the article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="0937b-111">**Återställa disk** -Kontrollera att du har utlösts återställningsjobbet för återställning av diskar där krypterade VM [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="0937b-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="0937b-112">Det beror på att jobbet genererar en JSON-fil i ditt lagringskonto som innehåller nycklar och hemligheter för den kryptera virtuella datorn ska återställas.</span><span class="sxs-lookup"><span data-stu-id="0937b-112">This is because this job generates a JSON file in your storage account containing keys and secrets for the encrypted VM to be restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="0937b-113">Hämta nyckel och Hemlig från Azure Backup</span><span class="sxs-lookup"><span data-stu-id="0937b-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="0937b-114">När disken har återställts av krypterade VM, kan du se till att:</span><span class="sxs-lookup"><span data-stu-id="0937b-114">Once disk has been restored for the encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="0937b-115">$details fylls med jobbinformation för återställning disk som anges i [PowerShell steg för återställning avsnittet diskar](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="0937b-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore the Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="0937b-116">VM ska skapas från återställda diskar **när nyckel och Hemlig har återställts i nyckelvalvet**.</span><span class="sxs-lookup"><span data-stu-id="0937b-116">VM should be created from restored disks only **after key and secret is restored to key vault**.</span></span>
>
>

<span data-ttu-id="0937b-117">Fråga om återställda disk egenskaperna för Jobbinformationen.</span><span class="sxs-lookup"><span data-stu-id="0937b-117">Query the restored disk properties for the job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="0937b-118">Konfigurera Azure storage-kontexten och återställa JSON-konfigurationsfil som innehåller nyckel och Hemlig information för krypterade VM.</span><span class="sxs-lookup"><span data-stu-id="0937b-118">Set the Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="0937b-119">Återställa nyckeln</span><span class="sxs-lookup"><span data-stu-id="0937b-119">Restore key</span></span>
<span data-ttu-id="0937b-120">När JSON-filen har genererats i målsökväg som nämns ovan, generera nyckel blob-fil från JSON och feed det för att återställa nyckeln cmdlet för att placera nyckel (KEK) tillbaka i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-120">Once the JSON file is generated in the destination path mentioned above, generate key blob file from the JSON and feed it to restore key cmdlet to put the key (KEK) back in the key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="0937b-121">Återställa hemlighet</span><span class="sxs-lookup"><span data-stu-id="0937b-121">Restore secret</span></span>
<span data-ttu-id="0937b-122">Använd JSON-filen som skapas ovan för att hämta det hemliga namnet och värdet och feed det om du vill ange hemliga för att placera hemligheten (BEK) tillbaka i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-122">Use the JSON file generated above to get secret name and value and feed it to set secret cmdlet to put the secret (BEK) back in the key vault.</span></span> <span data-ttu-id="0937b-123">**Använda dessa cmdletar om den virtuella datorn är krypterad med BEK och KEK.**</span><span class="sxs-lookup"><span data-stu-id="0937b-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="0937b-124">Om den virtuella datorn är **krypteras med BEK endast**, generera hemliga blob-fil från JSON och feed återställs hemliga cmdlet för att placera hemligheten (BEK) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-124">If your VM is **encrypted using BEK only**, generate secret blob file from the JSON and feed it to restore secret cmdlet to put the secret (BEK) back in the key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="0937b-125">Värdet för $secretname kan hämtas genom att referera till utdataporten för $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl och text efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="0937b-125">Value for $secretname can be obtained by referring to the output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="0937b-126">Värdet för taggen DiskEncryptionKeyFileName är samma som det hemliga namnet.</span><span class="sxs-lookup"><span data-stu-id="0937b-126">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="0937b-127">Skapa virtuell dator från återställda disk</span><span class="sxs-lookup"><span data-stu-id="0937b-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="0937b-128">Om du har säkerhetskopierat krypterade VM som använder Azure VM säkerhetskopiering av PowerShell-cmdlets som nämns ovan hjälp du återställa nyckel och hemliga tillbaka till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-128">If you have backed up encrypted VM using Azure VM Backup, the PowerShell cmdlets mentioned above help you restore key and secret back to the key vault.</span></span> <span data-ttu-id="0937b-129">När du återställer dem finns i artikeln [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) skapa krypterade virtuella datorer från återställda disk, nyckel och hemlig.</span><span class="sxs-lookup"><span data-stu-id="0937b-129">After restoring them, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="0937b-130">Äldre metod</span><span class="sxs-lookup"><span data-stu-id="0937b-130">Legacy approach</span></span>
<span data-ttu-id="0937b-131">Den metod som nämns ovan fungerar för alla återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="0937b-131">The approach mentioned above would work for all the recovery points.</span></span> <span data-ttu-id="0937b-132">Dock är äldre metod för att få nyckel och Hemlig information från återställningspunkten giltig för återställningspunkter som är äldre än 11 juli 2017 för virtuella datorer som har krypterats med BEK och KEK.</span><span class="sxs-lookup"><span data-stu-id="0937b-132">However, the older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="0937b-133">När disken Återställningsjobbet har slutförts för krypterade VM med hjälp av [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm), se till att $rp fylls med ett giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="0937b-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="0937b-134">Återställa nyckeln</span><span class="sxs-lookup"><span data-stu-id="0937b-134">Restore key</span></span>
<span data-ttu-id="0937b-135">Använda följande cmdletar för att hämta information om nyckel (KEK) från återställningspunkten och feed det för att återställa nyckeln cmdlet för att placera den i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-135">Use the following cmdlets to get key (KEK) information from recovery point and feed it to restore key cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="0937b-136">Återställa hemlighet</span><span class="sxs-lookup"><span data-stu-id="0937b-136">Restore secret</span></span>
<span data-ttu-id="0937b-137">Använda följande cmdletar för att hämta information om hemliga (BEK) från återställningspunkten och feed det om du vill ange hemliga för att placera den i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0937b-137">Use the following cmdlets to get secret (BEK) information from recovery point and feed it to set secret cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="0937b-138">Värdet för $secretname kan erhållas genom att referera till utdata för $rp1. KeyAndSecretDetails.SecretUrl och via SMS efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="0937b-138">Value for $secretname can be obtained by referring to the output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="0937b-139">Värdet för taggen DiskEncryptionKeyFileName är samma som det hemliga namnet.</span><span class="sxs-lookup"><span data-stu-id="0937b-139">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="0937b-140">Värdet för DiskEncryptionKeyEncryptionKeyURL kan hämtas från nyckelvalvet efter återställning av nycklar tillbaka och använder [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span><span class="sxs-lookup"><span data-stu-id="0937b-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring the keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="0937b-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0937b-141">Next steps</span></span>
<span data-ttu-id="0937b-142">När du återställer nyckel och hemliga tillbaka till nyckelvalvet, finns i artikeln [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) skapa krypterade virtuella datorer från återställda disk, nyckel och hemlig.</span><span class="sxs-lookup"><span data-stu-id="0937b-142">After restoring key and secret back to key vault, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key and secret.</span></span>
