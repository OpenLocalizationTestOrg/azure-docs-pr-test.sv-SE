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
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="df8bb-103">Återställa Key Vault-nyckel och Hemlig för krypterade virtuella datorer med Azure Backup</span><span class="sxs-lookup"><span data-stu-id="df8bb-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="df8bb-104">Den här artikeln handlar om med hjälp av Azure VM Backup tooperform återställning av krypterade virtuella Azure-datorer om din nyckel och Hemlig inte finns i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-104">This article talks about using Azure VM Backup tooperform restore of encrypted Azure VMs, if your key and secret do not exist in hello key vault.</span></span> <span data-ttu-id="df8bb-105">De här stegen kan också användas om du vill toomaintain en separat kopia av nyckel (nyckel krypteringsnyckeln) och hemlighet (BitLocker-krypteringsnyckeln) för hello återställts VM.</span><span class="sxs-lookup"><span data-stu-id="df8bb-105">These steps can also be used if you want toomaintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for hello restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df8bb-106">Krav</span><span class="sxs-lookup"><span data-stu-id="df8bb-106">Prerequisites</span></span>
* <span data-ttu-id="df8bb-107">**Säkerhetskopiering krypterade VMs** - krypterade virtuella Azure-datorer har säkerhetskopierats med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="df8bb-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="df8bb-108">Se hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md) mer information om hur toobackup krypterade virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="df8bb-108">Refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how toobackup encrypted Azure VMs.</span></span>
* <span data-ttu-id="df8bb-109">**Konfigurera Azure Key Vault** – se till att nyckelvalvet toowhich nycklar och hemligheter måste toobe återställas finns redan.</span><span class="sxs-lookup"><span data-stu-id="df8bb-109">**Configure Azure Key Vault** – Ensure that key vault toowhich keys and secrets need toobe restored is already present.</span></span> <span data-ttu-id="df8bb-110">Se hello artikel [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md) mer information om hantering av nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-110">Refer hello article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="df8bb-111">**Återställa disk** -Kontrollera att du har utlösts återställningsjobbet för återställning av diskar där krypterade VM [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="df8bb-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="df8bb-112">Det beror på att jobbet genererar en JSON-fil i ditt lagringskonto som innehåller nycklar och hemligheter för hello krypterade VM toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="df8bb-112">This is because this job generates a JSON file in your storage account containing keys and secrets for hello encrypted VM toobe restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="df8bb-113">Hämta nyckel och Hemlig från Azure Backup</span><span class="sxs-lookup"><span data-stu-id="df8bb-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="df8bb-114">När disken har återställts för hello krypterad VM, kan du se till att:</span><span class="sxs-lookup"><span data-stu-id="df8bb-114">Once disk has been restored for hello encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="df8bb-115">$details fylls med jobbinformation för återställning disk som anges i [PowerShell steg under återställning hello diskar](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="df8bb-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore hello Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="df8bb-116">VM ska skapas från återställda diskar **när nyckel och Hemlig är återställda tookey valvet**.</span><span class="sxs-lookup"><span data-stu-id="df8bb-116">VM should be created from restored disks only **after key and secret is restored tookey vault**.</span></span>
>
>

<span data-ttu-id="df8bb-117">Frågan hello återställt diskegenskaper för hello jobbinformation.</span><span class="sxs-lookup"><span data-stu-id="df8bb-117">Query hello restored disk properties for hello job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="df8bb-118">Ange hello Azure storage-kontexten och återställa JSON-konfigurationsfil som innehåller nyckel och Hemlig information för krypterade VM.</span><span class="sxs-lookup"><span data-stu-id="df8bb-118">Set hello Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="df8bb-119">Återställa nyckeln</span><span class="sxs-lookup"><span data-stu-id="df8bb-119">Restore key</span></span>
<span data-ttu-id="df8bb-120">När hello JSON-filen har genererats i hello målsökväg som nämns ovan, generera nyckel blob-fil från hello JSON och feed toorestore viktiga cmdlet tooput hello nyckel (KEK) tillbaka i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-120">Once hello JSON file is generated in hello destination path mentioned above, generate key blob file from hello JSON and feed it toorestore key cmdlet tooput hello key (KEK) back in hello key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="df8bb-121">Återställa hemlighet</span><span class="sxs-lookup"><span data-stu-id="df8bb-121">Restore secret</span></span>
<span data-ttu-id="df8bb-122">Använd hello JSON-fil skapas ovan tooget hemliga namn och värde och feed tooset hemliga cmdlet tooput hello hemligheten (BEK) tillbaka i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-122">Use hello JSON file generated above tooget secret name and value and feed it tooset secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span> <span data-ttu-id="df8bb-123">**Använda dessa cmdletar om den virtuella datorn är krypterad med BEK och KEK.**</span><span class="sxs-lookup"><span data-stu-id="df8bb-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="df8bb-124">Om den virtuella datorn är **krypteras med BEK endast**, generera hemliga blob-fil från hello JSON och feed toorestore hemliga cmdlet tooput hello hemligheten (BEK) tillbaka i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-124">If your VM is **encrypted using BEK only**, generate secret blob file from hello JSON and feed it toorestore secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="df8bb-125">Värdet för $secretname kan erhållas genom hänvisar toohello utdata från $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl och via SMS efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="df8bb-125">Value for $secretname can be obtained by referring toohello output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="df8bb-126">Värdet för hello taggen DiskEncryptionKeyFileName är samma som hemliga namnet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-126">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="df8bb-127">Skapa virtuell dator från återställda disk</span><span class="sxs-lookup"><span data-stu-id="df8bb-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="df8bb-128">Om du har säkerhetskopierat krypterade VM som använder Azure VM Backup hello PowerShell-cmdlets som nämns ovan hjälp du återställer nyckel och hemliga tillbaka toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-128">If you have backed up encrypted VM using Azure VM Backup, hello PowerShell cmdlets mentioned above help you restore key and secret back toohello key vault.</span></span> <span data-ttu-id="df8bb-129">När du återställer dem finns hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate krypterats virtuella datorer från återställda disk, nyckel och hemlig.</span><span class="sxs-lookup"><span data-stu-id="df8bb-129">After restoring them, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="df8bb-130">Äldre metod</span><span class="sxs-lookup"><span data-stu-id="df8bb-130">Legacy approach</span></span>
<span data-ttu-id="df8bb-131">hello-metod som nämns ovan fungerar för alla återställningspunkter för hello.</span><span class="sxs-lookup"><span data-stu-id="df8bb-131">hello approach mentioned above would work for all hello recovery points.</span></span> <span data-ttu-id="df8bb-132">Dock är hello äldre metod för att få nyckel och Hemlig information från återställningspunkten giltig för återställningspunkter som är äldre än 11 juli 2017 för virtuella datorer som har krypterats med BEK och KEK.</span><span class="sxs-lookup"><span data-stu-id="df8bb-132">However, hello older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="df8bb-133">När disken Återställningsjobbet har slutförts för krypterade VM med hjälp av [PowerShell steg](backup-azure-vms-automation.md#restore-an-azure-vm), se till att $rp fylls med ett giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="df8bb-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="df8bb-134">Återställa nyckeln</span><span class="sxs-lookup"><span data-stu-id="df8bb-134">Restore key</span></span>
<span data-ttu-id="df8bb-135">Använd hello följande cmdlets tooget nyckel (KEK) information från återställningspunkten och feed det toorestore viktiga cmdlet tooput tillbaka i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-135">Use hello following cmdlets tooget key (KEK) information from recovery point and feed it toorestore key cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="df8bb-136">Återställa hemlighet</span><span class="sxs-lookup"><span data-stu-id="df8bb-136">Restore secret</span></span>
<span data-ttu-id="df8bb-137">Använd hello följande information för cmdlets tooget secret (BEK) från återställningspunkten och feed det tooset hemliga cmdlet tooput tillbaka i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-137">Use hello following cmdlets tooget secret (BEK) information from recovery point and feed it tooset secret cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="df8bb-138">Värdet för $secretname kan erhållas genom att referera toohello utdata för $rp1. KeyAndSecretDetails.SecretUrl och via SMS efter hemligheter / t.ex. utdata hemliga URL är https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 och hemliga namnet är B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="df8bb-138">Value for $secretname can be obtained by referring toohello output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="df8bb-139">Värdet för hello taggen DiskEncryptionKeyFileName är samma som hemliga namnet.</span><span class="sxs-lookup"><span data-stu-id="df8bb-139">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="df8bb-140">Värdet för DiskEncryptionKeyEncryptionKeyURL kan hämtas från nyckelvalvet efter återställning hello nycklar tillbaka och använder [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span><span class="sxs-lookup"><span data-stu-id="df8bb-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring hello keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="df8bb-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df8bb-141">Next steps</span></span>
<span data-ttu-id="df8bb-142">När du återställer nyckel och hemliga tillbaka tookey valvet finns hello artikel [hantera säkerhetskopiering och återställning av virtuella Azure-datorer med hjälp av PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate krypterats virtuella datorer från återställda disk, nyckel och hemlig.</span><span class="sxs-lookup"><span data-stu-id="df8bb-142">After restoring key and secret back tookey vault, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key and secret.</span></span>
