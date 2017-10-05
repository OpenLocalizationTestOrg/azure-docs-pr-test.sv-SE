---
title: "Distribuera och hantera säkerhetskopiering för virtuella Azure-datorer med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur du distribuerar och hanterar Azure Backup med hjälp av PowerShell."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f0f06adb8177ce2d17aa0b40666470279c04e22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a><span data-ttu-id="97401-103">Använda AzureRM.Backup-cmdletar för att säkerhetskopiera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="97401-103">Use AzureRM.Backup cmdlets to back up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="97401-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="97401-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="97401-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="97401-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="97401-106">Den här artikeln visar hur du använder Azure PowerShell för säkerhetskopiering och återställning av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="97401-106">This article shows you how to use Azure PowerShell for backup and recovery of Azure VMs.</span></span> <span data-ttu-id="97401-107">I Azure finns två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="97401-107">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="97401-108">Den här artikeln täcker den klassiska distributionsmodellen för att säkerhetskopiera data till en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="97401-108">This article covers using the Classic deployment model to back up data to a Backup vault.</span></span> <span data-ttu-id="97401-109">Om du inte har skapat ett säkerhetskopieringsvalv i din prenumeration finns i Resource Manager-versionen av den här artikeln [Använd AzureRM.RecoveryServices.Backup-cmdletar för att säkerhetskopiera virtuella datorer](backup-azure-vms-automation.md).</span><span class="sxs-lookup"><span data-stu-id="97401-109">If you have not created a Backup vault in your subscription, see the Resource Manager version of this article, [Use AzureRM.RecoveryServices.Backup cmdlets to back up virtual machines](backup-azure-vms-automation.md).</span></span> <span data-ttu-id="97401-110">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="97401-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97401-111">Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="97401-111">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="97401-112">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="97401-112">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="97401-113">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="97401-113">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="97401-114">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="97401-114">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="97401-115">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="97401-115">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="97401-116">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="97401-116">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="97401-117">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="97401-117">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="97401-118">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="97401-118">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="concepts"></a><span data-ttu-id="97401-119">Koncept</span><span class="sxs-lookup"><span data-stu-id="97401-119">Concepts</span></span>
<span data-ttu-id="97401-120">Den här artikeln innehåller information om PowerShell-cmdlets som används för att säkerhetskopiera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="97401-120">This article provides information specific to the PowerShell cmdlets used to back up virtual machines.</span></span> <span data-ttu-id="97401-121">Grundläggande information om hur du skyddar virtuella Azure-datorer finns [planera din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="97401-121">For introductory information about protecting Azure VMs, please see [Plan your VM backup infrastructure in Azure](backup-azure-vms-introduction.md).</span></span>

> [!NOTE]
> <span data-ttu-id="97401-122">Innan du börjar bör du läsa den [krav](backup-azure-vms-prepare.md) krävs för att arbeta med Azure Backup och [begränsningar](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) säkerhetskopieringslösning för den aktuella virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="97401-122">Before you start, read the [prerequisites](backup-azure-vms-prepare.md) required to work with Azure Backup, and the [limitations](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) of the current VM backup solution.</span></span>
>
>

<span data-ttu-id="97401-123">Ta en stund att förstå hierarkin med objekt och från där du vill börja om du vill använda PowerShell effektivt.</span><span class="sxs-lookup"><span data-stu-id="97401-123">To use PowerShell effectively, take a moment to understand the hierarchy of objects and from where to start.</span></span>

![Objekthierarkin](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

<span data-ttu-id="97401-125">De två viktigaste flödena att aktivera skydd för en virtuell dator och återställa data från en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="97401-125">The two most important flows are enabling protection for a VM, and restoring data from a recovery point.</span></span> <span data-ttu-id="97401-126">Den här artikeln fokuserar på att hjälpa dig att bli bra när du arbetar med PowerShell-cmdletar för att aktivera dessa två scenarier.</span><span class="sxs-lookup"><span data-stu-id="97401-126">The focus of this article is to help you become adept at working with the PowerShell cmdlets to enable these two scenarios.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="97401-127">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="97401-127">Setup and Registration</span></span>
<span data-ttu-id="97401-128">Börja:</span><span class="sxs-lookup"><span data-stu-id="97401-128">To begin:</span></span>

1. <span data-ttu-id="97401-129">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="97401-129">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="97401-130">Hitta av Azure Backup PowerShell-cmdlets som är tillgängliga genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="97401-130">Find the Azure Backup PowerShell cmdlets available by typing the following command:</span></span>

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

<span data-ttu-id="97401-131">Följande uppgifter för installation och registrering kan automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="97401-131">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="97401-132">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="97401-132">Create a backup vault</span></span>
* <span data-ttu-id="97401-133">Registrerar de virtuella datorerna med Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="97401-133">Registering the VMs with the Azure Backup service</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="97401-134">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="97401-134">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="97401-135">För kunder som använder Azure Backup för första gången, som du behöver registrera Azure Backup-provider som ska användas med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="97401-135">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="97401-136">Detta kan göras genom att köra följande kommando: registrera AzureRmResourceProvider - ProviderNamespace ”Microsoft.Backup”</span><span class="sxs-lookup"><span data-stu-id="97401-136">This can be done by running the following command: Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="97401-137">Du kan skapa ett nytt säkerhetskopieringsvalv med den **ny AzureRmBackupVault** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-137">You can create a new backup vault using the **New-AzureRmBackupVault** cmdlet.</span></span> <span data-ttu-id="97401-138">Säkerhetskopieringsvalvet är en ARM-resurs, så du måste placera det inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="97401-138">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="97401-139">Kör följande kommandon i en kommandotolk med förhöjd Azure PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="97401-139">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="97401-140">Du kan hämta en lista över alla säkerhetskopieringsvalv i en viss prenumeration med hjälp av den **Get-AzureRmBackupVault** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-140">You can get a list of all the backup vaults in a given subscription using the **Get-AzureRmBackupVault** cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="97401-141">Det är praktiskt att lagra objektet säkerhetskopieringsvalvet i en variabel.</span><span class="sxs-lookup"><span data-stu-id="97401-141">It is convenient to store the backup vault object into a variable.</span></span> <span data-ttu-id="97401-142">Objektet valvet krävs som indata för många Azure Backup-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="97401-142">The vault object is needed as an input for many Azure Backup cmdlets.</span></span>
>
>

### <a name="registering-the-vms"></a><span data-ttu-id="97401-143">Registrerar de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="97401-143">Registering the VMs</span></span>
<span data-ttu-id="97401-144">Det första steget mot att konfigurera säkerhetskopiering med Azure Backup är att registrera din dator eller virtuell dator med Azure Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="97401-144">The first step towards configuring backup with Azure Backup is to register your machine or VM with an Azure Backup vault.</span></span> <span data-ttu-id="97401-145">Den **registrera AzureRmBackupContainer** cmdlet tar inkommande informationen för en virtuell Azure IaaS-dator och registrerar den med angivna valvet.</span><span class="sxs-lookup"><span data-stu-id="97401-145">The **Register-AzureRmBackupContainer** cmdlet takes the input information of an Azure IaaS virtual machine and registers it with the specified vault.</span></span> <span data-ttu-id="97401-146">Registrera igen associerar den virtuella Azure-datorn med säkerhetskopieringsvalvet och spårar den virtuella datorn under säkerhetskopiering livscykel.</span><span class="sxs-lookup"><span data-stu-id="97401-146">The register operation associates the Azure virtual machine with the backup vault and tracks the VM through the backup lifecycle.</span></span>

<span data-ttu-id="97401-147">Registrera den virtuella datorn med Azure Backup-tjänsten skapar ett översta behållarobjekt.</span><span class="sxs-lookup"><span data-stu-id="97401-147">Registering your VM with the Azure Backup service creates a top-level container object.</span></span> <span data-ttu-id="97401-148">En behållare innehåller vanligtvis flera objekt som kan säkerhetskopieras, men för virtuella datorer finns endast en säkerhetskopiering artikel för behållaren.</span><span class="sxs-lookup"><span data-stu-id="97401-148">A container typically contains multiple items that can be backed up, but in the case of VMs there will be only one backup item for the container.</span></span>

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a><span data-ttu-id="97401-149">Säkerhetskopiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="97401-149">Backup Azure VMs</span></span>
### <a name="create-a-protection-policy"></a><span data-ttu-id="97401-150">Skapa en skyddsprincip för</span><span class="sxs-lookup"><span data-stu-id="97401-150">Create a protection policy</span></span>
<span data-ttu-id="97401-151">Det är inte nödvändigt att skapa en ny skyddsprincip att starta säkerhetskopiering av dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="97401-151">It is not mandatory to create a new protection policy to start backup of your VMs.</span></span> <span data-ttu-id="97401-152">Valvet levereras med en 'standardprincip' som kan användas för att snabbt aktivera skydd och sedan redigera senare med rätt information.</span><span class="sxs-lookup"><span data-stu-id="97401-152">The vault comes with a 'Default Policy' that can be used to quickly enable protection, and then edited later with the right details.</span></span> <span data-ttu-id="97401-153">Du kan hämta en lista över principerna som är tillgängliga i valvet med hjälp av den **Get-AzureRmBackupProtectionPolicy** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="97401-153">You can get a list of the policies available in the vault by using the **Get-AzureRmBackupProtectionPolicy** cmdlet:</span></span>

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> <span data-ttu-id="97401-154">Tidszonen i fältet BackupTime i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="97401-154">The timezone of the BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="97401-155">När säkerhetskopieringstiden visas i Azure-portalen, justeras tidszonen till det lokala systemet tillsammans med UTC-förskjutningen.</span><span class="sxs-lookup"><span data-stu-id="97401-155">However, when the backup time is shown in the Azure portal, the timezone is aligned to your local system along with the UTC offset.</span></span>
>
>

<span data-ttu-id="97401-156">En princip för säkerhetskopiering är associerad med minst en bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="97401-156">A backup policy is associated with at least one retention policy.</span></span> <span data-ttu-id="97401-157">Bevarandeprincipen definierar hur länge en återställningspunkt sparas med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="97401-157">The retention policy defines how long a recovery point is kept with Azure Backup.</span></span> <span data-ttu-id="97401-158">Den **ny AzureRmBackupRetentionPolicy** cmdleten skapar PowerShell-objekt som innehåller information om principer bevarande.</span><span class="sxs-lookup"><span data-stu-id="97401-158">The **New-AzureRmBackupRetentionPolicy** cmdlet creates PowerShell objects that hold retention policy information.</span></span> <span data-ttu-id="97401-159">Dessa kvarhållning principobjekt används som indata för den *ny AzureRmBackupProtectionPolicy* cmdlet, eller direkt med den *aktivera AzureRmBackupProtection* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-159">These retention policy objects are used as inputs to the *New-AzureRmBackupProtectionPolicy* cmdlet, or directly with the *Enable-AzureRmBackupProtection* cmdlet.</span></span>

<span data-ttu-id="97401-160">En princip för säkerhetskopiering definierar när och hur ofta säkerhetskopieringen av ett objekt är klar.</span><span class="sxs-lookup"><span data-stu-id="97401-160">A backup policy defines when and how often the backup of an item is done.</span></span> <span data-ttu-id="97401-161">Den **ny AzureRmBackupProtectionPolicy** cmdlet skapar ett PowerShell-objekt som innehåller information om princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="97401-161">The **New-AzureRmBackupProtectionPolicy** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="97401-162">Säkerhetskopieringsprincipen som används som indata för den *aktivera AzureRmBackupProtection* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-162">The backup policy is used as an input to the *Enable-AzureRmBackupProtection* cmdlet.</span></span>

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a><span data-ttu-id="97401-163">Aktivera skydd</span><span class="sxs-lookup"><span data-stu-id="97401-163">Enable protection</span></span>
<span data-ttu-id="97401-164">Aktiverar skydd innebär två objekt - objektet och principen, och båda måste tillhöra samma valvet.</span><span class="sxs-lookup"><span data-stu-id="97401-164">Enabling protection involves two objects - the Item and the Policy, and both need to belong to the same vault.</span></span> <span data-ttu-id="97401-165">När principen har associerats med objektet, startar Säkerhetskopiering arbetsflödet på definierat schema.</span><span class="sxs-lookup"><span data-stu-id="97401-165">Once the policy has been associated with the item, the backup workflow will kick in at the defined schedule.</span></span>

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a><span data-ttu-id="97401-166">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="97401-166">Initial backup</span></span>
<span data-ttu-id="97401-167">Schemat för säkerhetskopiering hand tar om du gör den första fullständig för objektet och inkrementell kopian för efterföljande säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="97401-167">The backup schedule will take care of doing the full initial copy for the item and the incremental copy for the subsequent backups.</span></span> <span data-ttu-id="97401-168">Om du vill framtvinga den första säkerhetskopian ska hända vid en viss tidpunkt eller även omedelbart sedan använda den **säkerhetskopiering AzureRmBackupItem** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="97401-168">However, if you want to force the initial backup to happen at a certain time or even immediately then use the **Backup-AzureRmBackupItem** cmdlet:</span></span>

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="97401-169">Tidszonen StartTime och EndTime fält visas i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="97401-169">The timezone of the StartTime and EndTime fields shown in PowerShell is UTC.</span></span> <span data-ttu-id="97401-170">När liknande information visas i Azure-portalen, justeras tidszonen till din lokala systemklocka.</span><span class="sxs-lookup"><span data-stu-id="97401-170">However, when the similar information is shown in the Azure portal, the timezone is aligned to your local system clock.</span></span>
>
>

### <a name="monitoring-a-backup-job"></a><span data-ttu-id="97401-171">Övervakning av ett säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="97401-171">Monitoring a backup job</span></span>
<span data-ttu-id="97401-172">De flesta långvariga åtgärder i Azure Backup utformas som ett jobb.</span><span class="sxs-lookup"><span data-stu-id="97401-172">Most long-running operations in Azure Backup are modelled as a job.</span></span> <span data-ttu-id="97401-173">Detta gör det enkelt att följa förloppet utan att behålla den Azure-portalen öppna hela tiden.</span><span class="sxs-lookup"><span data-stu-id="97401-173">This makes it easy to track progress without having to keep the Azure portal open at all times.</span></span>

<span data-ttu-id="97401-174">För att få den senaste statusen för ett pågående jobb kan använda den **Get-AzureRmBackupJob** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-174">To get the latest status of an in-progress job, use the **Get-AzureRmBackupJob** cmdlet.</span></span>

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

<span data-ttu-id="97401-175">I stället för avsökning av dessa jobb för slutförande - som är onödiga, ytterligare kod – det är enklare att använda den **vänta AzureRmBackupJob** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-175">Instead of polling these jobs for completion - which is unnecessary, additional code - it is simpler to use the **Wait-AzureRmBackupJob** cmdlet.</span></span> <span data-ttu-id="97401-176">När den används i ett skript, pausar körningen cmdlet tills jobbet har slutförts eller det angivna tidsgränsvärdet har nåtts.</span><span class="sxs-lookup"><span data-stu-id="97401-176">When used in a script, the cmdlet will pause the execution until either the job completes or the specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a><span data-ttu-id="97401-177">Återställa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="97401-177">Restore an Azure VM</span></span>
<span data-ttu-id="97401-178">För att kunna återställa säkerhetskopierade data, måste du identifiera säkerhetskopierade artikeln och den återställningspunkt som innehåller data i tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="97401-178">In order to restore backup data, you need to identify the backed-up Item and the Recovery Point that holds the point-in-time data.</span></span> <span data-ttu-id="97401-179">Den här informationen anges till cmdleten återställning AzureRmBackupItem att starta en återställning av data från valvet på kundens konto.</span><span class="sxs-lookup"><span data-stu-id="97401-179">This information is supplied to the Restore-AzureRmBackupItem cmdlet to initiate a restore of data from the vault to the customer's account.</span></span>

### <a name="select-the-vm"></a><span data-ttu-id="97401-180">Välj den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="97401-180">Select the VM</span></span>
<span data-ttu-id="97401-181">För att få PowerShell-objektet som identifierar just säkerhetskopiering artikeln, måste du starta från en behållare i valvet och gå nedåt objekthierarkin.</span><span class="sxs-lookup"><span data-stu-id="97401-181">To get the PowerShell object that identifies the right backup Item, you need to start from the Container in the vault, and work your way down object hierarchy.</span></span> <span data-ttu-id="97401-182">Markera den behållare som representerar den virtuella datorn med hjälp av **Get-AzureRmBackupContainer** cmdlet och skicka som att den **Get-AzureRmBackupItem** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="97401-182">To select the container that represents the VM, use the **Get-AzureRmBackupContainer** cmdlet and pipe that to the **Get-AzureRmBackupItem** cmdlet.</span></span>

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="97401-183">Välj en återställningspunkt</span><span class="sxs-lookup"><span data-stu-id="97401-183">Choose a recovery point</span></span>
<span data-ttu-id="97401-184">Du kan nu visa alla återställningspunkter för Säkerhetskopiera objekt med den **Get-AzureRmBackupRecoveryPoint** cmdlet, och Välj återställningspunkt att återställa.</span><span class="sxs-lookup"><span data-stu-id="97401-184">You can now list all the recovery points for the backup item using the **Get-AzureRmBackupRecoveryPoint** cmdlet, and choose the recovery point to restore.</span></span> <span data-ttu-id="97401-185">Vanligtvis användare välja den senaste *AppConsistent* peka i listan.</span><span class="sxs-lookup"><span data-stu-id="97401-185">Typically users pick the most recent *AppConsistent* point in the list.</span></span>

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

<span data-ttu-id="97401-186">Variabeln ```$rp``` är en matris med återställningspunkter för den valda säkerhetskopian artikel, sorterade i omvänd ordning tid - den senaste återställningspunkten är vid index 0.</span><span class="sxs-lookup"><span data-stu-id="97401-186">The variable ```$rp``` is an array of recovery points for the selected backup item, sorted in reverse order of time - the latest recovery point is at index 0.</span></span> <span data-ttu-id="97401-187">Använd standard PowerShell matris indexering för att välja återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="97401-187">Use standard PowerShell array indexing to pick the recovery point.</span></span> <span data-ttu-id="97401-188">Till exempel: ```$rp[0]``` väljer den senaste återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="97401-188">For example: ```$rp[0]``` will select the latest recovery point.</span></span>

### <a name="restoring-disks"></a><span data-ttu-id="97401-189">Återställning av diskar</span><span class="sxs-lookup"><span data-stu-id="97401-189">Restoring disks</span></span>
<span data-ttu-id="97401-190">Det finns en nyckel skillnaden mellan återställningsåtgärderna klar via Azure-portalen och via Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="97401-190">There is a key difference between the restore operations done through the Azure portal and through Azure PowerShell.</span></span> <span data-ttu-id="97401-191">Med PowerShell kan stannar återställningen vid återställer diskar och konfigurationsinformation från återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="97401-191">With PowerShell, the restore operation stops at restoring the disks and config information from the recovery point.</span></span> <span data-ttu-id="97401-192">Den skapar inte en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97401-192">It does not create a virtual machine.</span></span>

> [!WARNING]
> <span data-ttu-id="97401-193">Restore-AzureRmBackupItem kan inte skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97401-193">The Restore-AzureRmBackupItem does not create a VM.</span></span> <span data-ttu-id="97401-194">Den återställer endast diskarna till det angivna lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="97401-194">It only restores the disks to the specified storage account.</span></span> <span data-ttu-id="97401-195">Detta är inte på samma sätt som du får i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="97401-195">This is not the same behavior you will experience in the Azure portal.</span></span>
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

<span data-ttu-id="97401-196">Du kan hämta information om återställning åtgärden med den **Get-AzureRmBackupJobDetails** cmdlet när Återställningsjobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="97401-196">You can get the details of the restore operation using the **Get-AzureRmBackupJobDetails** cmdlet once the Restore job has completed.</span></span> <span data-ttu-id="97401-197">Den *felinformation* egenskapen har information som behövs för att återskapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="97401-197">The *ErrorDetails* property will have the information needed to rebuild the VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a><span data-ttu-id="97401-198">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="97401-198">Build the VM</span></span>
<span data-ttu-id="97401-199">Skapa den virtuella datorn utanför de återställda diskarna kan göras med hjälp av äldre Azure Service Management PowerShell-cmdlets, de nya Azure Resource Manager-mallarna eller via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="97401-199">Building the VM out of the restored disks can be done using the older Azure Service Management PowerShell cmdlets, the new Azure Resource Manager templates, or even using the Azure portal.</span></span> <span data-ttu-id="97401-200">I ett enkelt exempel visar vi hur du hämtar det med hjälp av Azure Service Management-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="97401-200">In a quick example, we will show how to get there using the Azure Service Management cmdlets.</span></span>

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

<span data-ttu-id="97401-201">Mer information om hur du skapar en virtuell dator från de återställda diskarna Läs mer om följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="97401-201">For more information on how to build a VM from the restored disks, read about the following cmdlets:</span></span>

* [<span data-ttu-id="97401-202">Lägg till AzureDisk</span><span class="sxs-lookup"><span data-stu-id="97401-202">Add-AzureDisk</span></span>](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [<span data-ttu-id="97401-203">Ny AzureVMConfig</span><span class="sxs-lookup"><span data-stu-id="97401-203">New-AzureVMConfig</span></span>](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [<span data-ttu-id="97401-204">Ny AzureVM</span><span class="sxs-lookup"><span data-stu-id="97401-204">New-AzureVM</span></span>](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a><span data-ttu-id="97401-205">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="97401-205">Code samples</span></span>
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a><span data-ttu-id="97401-206">1. Hämta status för slutförande av jobbet underordnade aktiviteter</span><span class="sxs-lookup"><span data-stu-id="97401-206">1. Get the completion status of job sub-tasks</span></span>
<span data-ttu-id="97401-207">Du kan använda för att spåra status för slutförande av enskilda underordnade aktiviteter måste den **Get-AzureRmBackupJobDetails** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="97401-207">To track the completion status of individual sub-tasks, you can use the **Get-AzureRmBackupJobDetails** cmdlet:</span></span>

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a><span data-ttu-id="97401-208">2. Skapa en rapport för varje dag/vecka säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="97401-208">2. Create a daily/weekly report of backup jobs</span></span>
<span data-ttu-id="97401-209">Administratörer vill förmodligen veta säkerhetskopieringsjobb kördes under de senaste 24 timmarna statusen för dessa säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="97401-209">Administrators typically want to know what backup jobs ran in the last 24 hours, the status of those backup jobs.</span></span> <span data-ttu-id="97401-210">Dessutom ger mängden data som överförs administratörer ett sätt att beräkna sin månatlig dataanvändning.</span><span class="sxs-lookup"><span data-stu-id="97401-210">Additionally, the amount of data transferred gives administrators a way to estimate their monthly data usage.</span></span> <span data-ttu-id="97401-211">Skriptet nedan hämtar rådata hämtas från Azure Backup-tjänsten och visar information i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="97401-211">The script below pulls the raw data from the Azure Backup service and displays the information in the PowerShell console.</span></span>

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

<span data-ttu-id="97401-212">Om du vill lägga till diagram funktioner i den här rapportutdata Läs från TechNet blogginlägget [diagram med PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span><span class="sxs-lookup"><span data-stu-id="97401-212">If you want to add charting capabilities to this report output, learn from the TechNet blog post [Charting with PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="97401-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97401-213">Next steps</span></span>
<span data-ttu-id="97401-214">Om du vill använda PowerShell för att interagera med dina Azure-resurser kan ta en titt i PowerShell-artikeln för att skydda Windows Server [distribuera och hantera säkerhetskopiering för Windows Server](backup-client-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="97401-214">If you prefer using PowerShell to engage with your Azure resources, check out the PowerShell article for protecting Windows Server, [Deploy and Manage Backup for Windows Server](backup-client-automation-classic.md).</span></span> <span data-ttu-id="97401-215">Det finns också en artikel i PowerShell för att hantera DPM-säkerhetskopieringar [distribuera och hantera säkerhetskopiering för DPM](backup-dpm-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="97401-215">There is also a PowerShell article for managing DPM backups, [Deploy and Manage Backup for DPM](backup-dpm-automation-classic.md).</span></span> <span data-ttu-id="97401-216">Båda dessa artiklar har en version för Resource Manager distributioner samt klassiska distributioner.</span><span class="sxs-lookup"><span data-stu-id="97401-216">Both of these articles have a version for Resource Manager deployments as well as Classic deployments.</span></span>
