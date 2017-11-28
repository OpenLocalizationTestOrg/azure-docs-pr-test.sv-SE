---
title: "aaaDeploy och hantera säkerhetskopior för Resource Manager distribuerade virtuella datorer med hjälp av PowerShell | Microsoft Docs"
description: "Använd PowerShell toodeploy och hantera säkerhetskopior i Azure för Resource Manager distribuerade virtuella datorer"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/28/2017
ms.author: markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486fb3ae1902403fe6bf303df57244b76677ab17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a><span data-ttu-id="a0bed-103">Använd AzureRM.RecoveryServices.Backup cmdlets tooback av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-103">Use AzureRM.RecoveryServices.Backup cmdlets tooback up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a0bed-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a0bed-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="a0bed-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="a0bed-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="a0bed-106">Den här artikeln visar hur toouse Azure PowerShell-cmdlets tooback in och återställer en virtuell Azure virtuell dator (VM) från en Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-106">This article shows you how toouse Azure PowerShell cmdlets tooback up and recover an Azure virtual machine (VM) from a Recovery Services vault.</span></span> <span data-ttu-id="a0bed-107">Recovery Services-valvet är en Azure Resource Manager-resurs och använda tooprotect data och tillgångar i både Azure Backup och Azure Site Recovery services.</span><span class="sxs-lookup"><span data-stu-id="a0bed-107">A Recovery Services vault is an Azure Resource Manager resource and is used tooprotect data and assets in both Azure Backup and Azure Site Recovery services.</span></span> <span data-ttu-id="a0bed-108">Du kan använda en Recovery Services-valvet tooprotect Azure Service Manager-distribuerade virtuella datorer och Azure Resource Manager distribuerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-108">You can use a Recovery Services vault tooprotect Azure Service Manager-deployed VMs, and Azure Resource Manager-deployed VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="a0bed-109">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-109">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a0bed-110">Den här artikeln ska användas med virtuella datorer som skapats med hjälp av hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="a0bed-110">This article is for use with VMs created using hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="a0bed-111">Den här artikeln vägleder dig genom med hjälp av PowerShell tooprotect en virtuell dator och Återställ data från en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-111">This article walks you through using PowerShell tooprotect a VM, and restore data from a recovery point.</span></span>

## <a name="concepts"></a><span data-ttu-id="a0bed-112">Koncept</span><span class="sxs-lookup"><span data-stu-id="a0bed-112">Concepts</span></span>
<span data-ttu-id="a0bed-113">Om du inte är bekant med hello Azure Backup-tjänsten för en översikt över hello tjänsten kolla [vad är Azure Backup?](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="a0bed-113">If you are not familiar with hello Azure Backup service, for an overview of hello service, check out [What is Azure Backup?](backup-introduction-to-azure-backup.md)</span></span> <span data-ttu-id="a0bed-114">Innan du börjar, kontrollera att täcka hello essentials om hello kraven toowork med Azure Backup och hello begränsningar av hello aktuella VM lösning för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="a0bed-114">Before you start, ensure that you cover hello essentials about hello prerequisites needed toowork with Azure Backup, and hello limitations of hello current VM backup solution.</span></span>

<span data-ttu-id="a0bed-115">toouse PowerShell effektivt, är det nödvändigt toounderstand hello hierarkin med objekt och varifrån toostart.</span><span class="sxs-lookup"><span data-stu-id="a0bed-115">toouse PowerShell effectively, it is necessary toounderstand hello hierarchy of objects and from where toostart.</span></span>

![Recovery Services-objekthierarkin](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

<span data-ttu-id="a0bed-117">tooview hello AzureRm.RecoveryServices.Backup PowerShell cmdlet-referens, se hello [Azure Backup - Recovery Services cmdletar](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) i hello Azure-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="a0bed-117">tooview hello AzureRm.RecoveryServices.Backup PowerShell cmdlet reference, see hello [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in hello Azure library.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="a0bed-118">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="a0bed-118">Setup and Registration</span></span>
<span data-ttu-id="a0bed-119">toobegin:</span><span class="sxs-lookup"><span data-stu-id="a0bed-119">toobegin:</span></span>

1. <span data-ttu-id="a0bed-120">[Hämta hello senaste versionen av PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello lägsta version som krävs är: 1.4.0)</span><span class="sxs-lookup"><span data-stu-id="a0bed-120">[Download hello latest version of PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello minimum version required is: 1.4.0)</span></span>
2. <span data-ttu-id="a0bed-121">Hitta hello Azure Backup PowerShell-cmdlets som är tillgängliga genom att skriva följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a0bed-121">Find hello Azure Backup PowerShell cmdlets available by typing hello following command:</span></span>

```
PS C:\> Get-Command *azurermrecoveryservices*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
```


<span data-ttu-id="a0bed-122">hello följande aktiviteter kan automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a0bed-122">hello following tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="a0bed-123">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="a0bed-123">Create a Recovery Services vault</span></span>
* <span data-ttu-id="a0bed-124">Säkerhetskopiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-124">Back up Azure VMs</span></span>
* <span data-ttu-id="a0bed-125">Utlösa ett säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="a0bed-125">Trigger a backup job</span></span>
* <span data-ttu-id="a0bed-126">Övervaka ett säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="a0bed-126">Monitor a backup job</span></span>
* <span data-ttu-id="a0bed-127">Återställa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="a0bed-127">Restore an Azure VM</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="a0bed-128">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="a0bed-128">Create a recovery services vault</span></span>
<span data-ttu-id="a0bed-129">hello följande steg leder dig genom att skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-129">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="a0bed-130">Recovery Services-valvet skiljer sig en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-130">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="a0bed-131">Om du använder Azure Backup för hello första gången, måste du använda hello  **[registrera AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  cmdlet tooregister hello Azure Recovery Service provider med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a0bed-131">If you are using Azure Backup for hello first time, you must use hello **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="a0bed-132">hello Recovery Services-valvet är en Resource Manager-resurs, så du måste tooplace den inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0bed-132">hello Recovery Services vault is a Resource Manager resource, so you need tooplace it within a resource group.</span></span> <span data-ttu-id="a0bed-133">Du kan använda en befintlig resursgrupp eller skapa en resursgrupp med hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-133">You can use an existing resource group, or create a resource group with hello **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet.</span></span> <span data-ttu-id="a0bed-134">När du skapar en resursgrupp, ange hello namn och plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0bed-134">When creating a resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="a0bed-135">Använd hello  **[ny AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  cmdlet toocreate hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-135">Use hello **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet toocreate hello Recovery Services vault.</span></span> <span data-ttu-id="a0bed-136">Glöm toospecify hello samma plats för hello valvet som användes för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0bed-136">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="a0bed-137">Ange hello typ av lagring redundans toouse; Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="a0bed-137">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="a0bed-138">hello följande exempel visar hello - BackupStorageRedundancy alternativet för testvault anges tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="a0bed-138">hello following example shows hello -BackupStorageRedundancy option for testvault is set tooGeoRedundant.</span></span>

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > <span data-ttu-id="a0bed-139">Många Azure Backup-cmdletar kräver hello Recovery Services-valvet objekt som indata.</span><span class="sxs-lookup"><span data-stu-id="a0bed-139">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="a0bed-140">Därför är det praktiskt toostore hello säkerhetskopiering Recovery Services-valvet objekt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="a0bed-140">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="a0bed-141">Visa hello valv i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0bed-141">View hello vaults in a subscription</span></span>
<span data-ttu-id="a0bed-142">Använd  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  tooview hello lista över alla valv i hello nuvarande prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a0bed-142">Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="a0bed-143">Du kan använda det här kommandot toocheck att ett nytt valv skapades eller toosee hello tillgängliga valv i hello prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a0bed-143">You can use this command toocheck that a new vault was created, or toosee hello available vaults in hello subscription.</span></span>

<span data-ttu-id="a0bed-144">Kör hello-kommandot Get-AzureRmRecoveryServicesVault tooview alla valv i hello prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a0bed-144">Run hello command, Get-AzureRmRecoveryServicesVault, tooview all vaults in hello subscription.</span></span> <span data-ttu-id="a0bed-145">hello visar följande exempel hello information som visas för varje valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-145">hello following example shows hello information displayed for each vault.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a><span data-ttu-id="a0bed-146">Säkerhetskopiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-146">Back up Azure VMs</span></span>
<span data-ttu-id="a0bed-147">Använd en Recovery Services-valvet tooprotect dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-147">Use a Recovery Services vault tooprotect your virtual machines.</span></span> <span data-ttu-id="a0bed-148">Innan du installerar hello skydd ange hello valvet kontext (hello typ av data som skyddas i hello valvet) och kontrollera hello protection-principen.</span><span class="sxs-lookup"><span data-stu-id="a0bed-148">Before you apply hello protection, set hello vault context (hello type of data protected in hello vault), and verify hello protection policy.</span></span> <span data-ttu-id="a0bed-149">hello protection-principen är hello schemalägga när hello säkerhetskopieringsjobb körs och hur länge varje ögonblicksbild av säkerhetskopian ska sparas.</span><span class="sxs-lookup"><span data-stu-id="a0bed-149">hello protection policy is hello schedule when hello backup jobs run, and how long each backup snapshot is retained.</span></span>

### <a name="set-vault-context"></a><span data-ttu-id="a0bed-150">Ange valvet kontext</span><span class="sxs-lookup"><span data-stu-id="a0bed-150">Set vault context</span></span>
<span data-ttu-id="a0bed-151">Innan du aktiverar skyddet på en virtuell dator använda  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello valvet kontext.</span><span class="sxs-lookup"><span data-stu-id="a0bed-151">Before enabling protection on a VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context.</span></span> <span data-ttu-id="a0bed-152">När hello valvet kontexten har angetts gäller tooall efterföljande cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a0bed-152">Once hello vault context is set, it applies tooall subsequent cmdlets.</span></span> <span data-ttu-id="a0bed-153">hello följande exempel anger hello valvet kontext för hello valvet, *testvault*.</span><span class="sxs-lookup"><span data-stu-id="a0bed-153">hello following example sets hello vault context for hello vault, *testvault*.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a><span data-ttu-id="a0bed-154">Skapa en skyddsprincip för</span><span class="sxs-lookup"><span data-stu-id="a0bed-154">Create a protection policy</span></span>
<span data-ttu-id="a0bed-155">När du skapar ett Recovery Services-valv kommer med standardskydd och bevarandeprinciper.</span><span class="sxs-lookup"><span data-stu-id="a0bed-155">When you create a Recovery Services vault, it comes with default protection and retention policies.</span></span> <span data-ttu-id="a0bed-156">hello standardprincipen protection utlöser en säkerhetskopiering varje dag vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-156">hello default protection policy triggers a backup job each day at a specified time.</span></span> <span data-ttu-id="a0bed-157">hello standard bevarandeprincip behåller hello dagliga återställningspunkt under 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-157">hello default retention policy retains hello daily recovery point for 30 days.</span></span> <span data-ttu-id="a0bed-158">Du kan använda hello standard princip tooquickly skydda den virtuella datorn och redigera hello principen senare med annan information.</span><span class="sxs-lookup"><span data-stu-id="a0bed-158">You can use hello default policy tooquickly protect your VM and edit hello policy later with different details.</span></span>

<span data-ttu-id="a0bed-159">Använd  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  tooview hello protection-principer i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-159">Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** tooview hello protection policies in hello vault.</span></span> <span data-ttu-id="a0bed-160">Du kan använda den här cmdlet-tooget en specifik princip eller tooview hello principerna som associeras med en Arbetsbelastningstyp.</span><span class="sxs-lookup"><span data-stu-id="a0bed-160">You can use this cmdlet tooget a specific policy, or tooview hello policies associated with a workload type.</span></span> <span data-ttu-id="a0bed-161">hello följande exempel hämtar principer för Arbetsbelastningstyp, AzureVM.</span><span class="sxs-lookup"><span data-stu-id="a0bed-161">hello following example gets policies for workload type, AzureVM.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> <span data-ttu-id="a0bed-162">hello tidszonen i hello BackupTime fält i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="a0bed-162">hello timezone of hello BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="a0bed-163">När hello säkerhetskopieringstiden visas i hello Azure-portalen, men är justerade tooyour lokala tidszon hello tid.</span><span class="sxs-lookup"><span data-stu-id="a0bed-163">However, when hello backup time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

<span data-ttu-id="a0bed-164">En säkerhetskopiering protection-principen är associerad med minst en bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="a0bed-164">A backup protection policy is associated with at least one retention policy.</span></span> <span data-ttu-id="a0bed-165">Bevarandeprincip definierar hur länge en återställningspunkt sparas innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="a0bed-165">Retention policy defines how long a recovery point is kept before it is deleted.</span></span> <span data-ttu-id="a0bed-166">Använd  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  tooview hello standard bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="a0bed-166">Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** tooview hello default retention policy.</span></span>  <span data-ttu-id="a0bed-167">På liknande sätt kan du använda  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello standardprincipen schema.</span><span class="sxs-lookup"><span data-stu-id="a0bed-167">Similarly you can use **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** tooobtain hello default schedule policy.</span></span> <span data-ttu-id="a0bed-168">Hej  **[ny AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet skapar ett PowerShell-objekt som innehåller information om princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="a0bed-168">hello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="a0bed-169">hello schema och förvaring principobjekt används som anger toohello  **[ny AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-169">hello schedule and retention policy objects are used as inputs toohello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet.</span></span> <span data-ttu-id="a0bed-170">hello lagrar följande exempel hello schema principen och hello bevarandeprincip i variabler.</span><span class="sxs-lookup"><span data-stu-id="a0bed-170">hello following example stores hello schedule policy and hello retention policy in variables.</span></span> <span data-ttu-id="a0bed-171">hello exemplet används dessa variabler toodefine hello parametrar när du skapar en skyddsprincip *NewPolicy*.</span><span class="sxs-lookup"><span data-stu-id="a0bed-171">hello example uses those variables toodefine hello parameters when creating a protection policy, *NewPolicy*.</span></span>

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a><span data-ttu-id="a0bed-172">Aktivera skydd</span><span class="sxs-lookup"><span data-stu-id="a0bed-172">Enable protection</span></span>
<span data-ttu-id="a0bed-173">När du har definierat hello säkerhetskopiering protection-principen kan aktivera du fortfarande hello princip för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-173">Once you have defined hello backup protection policy, you still must enable hello policy for an item.</span></span> <span data-ttu-id="a0bed-174">Använd  **[aktivera AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable skydd.</span><span class="sxs-lookup"><span data-stu-id="a0bed-174">Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** tooenable protection.</span></span> <span data-ttu-id="a0bed-175">Aktiverar skydd kräver två objekt - hello och hello principdata.</span><span class="sxs-lookup"><span data-stu-id="a0bed-175">Enabling protection requires two objects - hello item and hello policy.</span></span> <span data-ttu-id="a0bed-176">När hello princip har associerats med hello valvet, utlöses hello säkerhetskopiering arbetsflödet när hello enligt ett schema för hello.</span><span class="sxs-lookup"><span data-stu-id="a0bed-176">Once hello policy has been associated with hello vault, hello backup workflow is triggered at hello time defined in hello policy schedule.</span></span>

<span data-ttu-id="a0bed-177">följande exempel aktiverar skydd för hello objekt, V2VM, med hjälp av hello princip, NewPolicy hello.</span><span class="sxs-lookup"><span data-stu-id="a0bed-177">hello following example enables protection for hello item, V2VM, using hello policy, NewPolicy.</span></span> <span data-ttu-id="a0bed-178">tooenable hello skydd på icke-krypterade hanteraren för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-178">tooenable hello protection on non-encrypted Resource Manager VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="a0bed-179">tooenable hello skydd på krypterade virtuella datorer (krypterad med BEK och KEK) måste du toogive hello Azure Backup service behörighet tooread nycklar och hemligheter från nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-179">tooenable hello protection on encrypted VMs (encrypted using BEK and KEK), you need toogive hello Azure Backup service permission tooread keys and secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="a0bed-180">tooenable hello skydd på krypterade virtuella datorer (krypterade endast med BEK) måste du toogive hello Azure Backup service behörighet tooread hemligheter från nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-180">tooenable hello protection on encrypted VMs (encrypted using BEK only), you need toogive hello Azure Backup service permission tooread secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> <span data-ttu-id="a0bed-181">Om du använder hello Azure Government molnet använder hello värdet ff281ffe-705c-4f53-9f37-a40e6f2c68f3 för hello parametern **- ServicePrincipalName** i [Set AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet .</span><span class="sxs-lookup"><span data-stu-id="a0bed-181">If you are using hello Azure Government cloud, then use hello value ff281ffe-705c-4f53-9f37-a40e6f2c68f3 for hello parameter **-ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.</span></span>
>
>

<span data-ttu-id="a0bed-182">För klassiska virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-182">For classic VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a><span data-ttu-id="a0bed-183">Ändra en protection-princip</span><span class="sxs-lookup"><span data-stu-id="a0bed-183">Modify a protection policy</span></span>
<span data-ttu-id="a0bed-184">toomodify hello protection-principen, Använd [Set AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy eller RetentionPolicy objekt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-184">toomodify hello protection policy, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy or RetentionPolicy objects.</span></span>

<span data-ttu-id="a0bed-185">hello ändras följande exempel hello recovery punkt kvarhållning too365 dagar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-185">hello following example changes hello recovery point retention too365 days.</span></span>

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a><span data-ttu-id="a0bed-186">Utlös en säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="a0bed-186">Trigger a backup</span></span>
<span data-ttu-id="a0bed-187">Du kan använda  **[säkerhetskopiering AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger ett säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="a0bed-187">You can use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** tootrigger a backup job.</span></span> <span data-ttu-id="a0bed-188">Om det är första hello-säkerhetskopian är en fullständig säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="a0bed-188">If it is hello initial backup, it is a full backup.</span></span> <span data-ttu-id="a0bed-189">Efterföljande säkerhetskopieringar ta en inkrementell kopia.</span><span class="sxs-lookup"><span data-stu-id="a0bed-189">Subsequent backups take an incremental copy.</span></span> <span data-ttu-id="a0bed-190">Vara säker på att toouse  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello valvet kontexten innan hello säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="a0bed-190">Be sure toouse **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context before triggering hello backup job.</span></span> <span data-ttu-id="a0bed-191">hello följande exempel förutsätter valvet kontext angavs.</span><span class="sxs-lookup"><span data-stu-id="a0bed-191">hello following example assumes vault context was set.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> <span data-ttu-id="a0bed-192">hello tidszonen hello StartTime och EndTime fält i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="a0bed-192">hello timezone of hello StartTime and EndTime fields in PowerShell is UTC.</span></span> <span data-ttu-id="a0bed-193">När hello tid visas i hello Azure-portalen, men är justerade tooyour lokala tidszon hello tid.</span><span class="sxs-lookup"><span data-stu-id="a0bed-193">However, when hello time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

## <a name="monitoring-a-backup-job"></a><span data-ttu-id="a0bed-194">Övervakning av ett säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="a0bed-194">Monitoring a backup job</span></span>
<span data-ttu-id="a0bed-195">Du kan övervaka långvariga åtgärder, till exempel säkerhetskopieringsjobb, utan att använda hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0bed-195">You can monitor long-running operations, such as backup jobs, without using hello Azure portal.</span></span> <span data-ttu-id="a0bed-196">tooget hello status för ett pågående jobb, Använd hello  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-196">tooget hello status of an in-progress job, use hello **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="a0bed-197">Denna cmdlet hämtar hello säkerhetskopieringsjobb för en specifik valvet och att valvet har angetts i kontexten för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-197">This cmdlet gets hello backup jobs for a specific vault, and that vault is specified in hello vault context.</span></span> <span data-ttu-id="a0bed-198">hello följande exempel hämtar hello status för ett pågående jobb som en matris och lagrar hello status i hello $joblist variabel.</span><span class="sxs-lookup"><span data-stu-id="a0bed-198">hello following example gets hello status of an in-progress job as an array, and stores hello status in hello $joblist variable.</span></span>

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="a0bed-199">Använd i stället för avsökning av dessa jobb för slutförande - som är onödiga ytterligare kod - hello  **[vänta AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-199">Instead of polling these jobs for completion - which is unnecessary additional code - use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="a0bed-200">Denna cmdlet pausar körningen hello tills hello jobbet slutförs eller hello anges tidsgränsen har nåtts.</span><span class="sxs-lookup"><span data-stu-id="a0bed-200">This cmdlet pauses hello execution until either hello job completes or hello specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a><span data-ttu-id="a0bed-201">Återställa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="a0bed-201">Restore an Azure VM</span></span>
<span data-ttu-id="a0bed-202">Det finns en nyckel skillnaden mellan hello återställa en virtuell dator med hjälp av hello Azure-portalen och återställa en virtuell dator med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a0bed-202">There is a key difference between hello restoring a VM using hello Azure portal and restoring a VM using PowerShell.</span></span> <span data-ttu-id="a0bed-203">Hello återställningen är slutförd med PowerShell när hello diskar och konfigurationsinformation från hello återställningspunkt skapas.</span><span class="sxs-lookup"><span data-stu-id="a0bed-203">With PowerShell, hello restore operation is complete once hello disks and configuration information from hello recovery point are created.</span></span>

> [!NOTE]
> <span data-ttu-id="a0bed-204">hello återställningen skapar inte en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a0bed-204">hello restore operation does not create a virtual machine.</span></span>
>
>

<span data-ttu-id="a0bed-205">toocreate en virtuell dator från disken, se avsnittet hello [skapa hello VM från lagrade diskar](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span><span class="sxs-lookup"><span data-stu-id="a0bed-205">toocreate a virtual machine from disk, see hello section, [Create hello VM from stored disks](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span></span> <span data-ttu-id="a0bed-206">hello grundläggande steg toorestore en Azure VM är:</span><span class="sxs-lookup"><span data-stu-id="a0bed-206">hello basic steps toorestore an Azure VM are:</span></span>

* <span data-ttu-id="a0bed-207">Välj hello VM</span><span class="sxs-lookup"><span data-stu-id="a0bed-207">Select hello VM</span></span>
* <span data-ttu-id="a0bed-208">Välj en återställningspunkt</span><span class="sxs-lookup"><span data-stu-id="a0bed-208">Choose a recovery point</span></span>
* <span data-ttu-id="a0bed-209">Återställa hello diskar</span><span class="sxs-lookup"><span data-stu-id="a0bed-209">Restore hello disks</span></span>
* <span data-ttu-id="a0bed-210">Skapa hello VM från lagrade diskar</span><span class="sxs-lookup"><span data-stu-id="a0bed-210">Create hello VM from stored disks</span></span>

<span data-ttu-id="a0bed-211">hello visar följande bild hello objekthierarkin från hello RecoveryServicesVault ned toohello BackupRecoveryPoint.</span><span class="sxs-lookup"><span data-stu-id="a0bed-211">hello following graphic shows hello object hierarchy from hello RecoveryServicesVault down toohello BackupRecoveryPoint.</span></span>

![Recovery Services objekthierarkin visar BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

<span data-ttu-id="a0bed-213">toorestore säkerhetskopierade data, identifiera hello säkerhetskopierade objekt och hello återställningspunkt som innehåller hello tidpunkts-data.</span><span class="sxs-lookup"><span data-stu-id="a0bed-213">toorestore backup data, identify hello backed-up item and hello recovery point that holds hello point-in-time data.</span></span> <span data-ttu-id="a0bed-214">Använd hello  **[återställning AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet toorestore data från hello valvet toohello kundens konto.</span><span class="sxs-lookup"><span data-stu-id="a0bed-214">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore data from hello vault toohello customer's account.</span></span>

### <a name="select-hello-vm"></a><span data-ttu-id="a0bed-215">Välj hello VM</span><span class="sxs-lookup"><span data-stu-id="a0bed-215">Select hello VM</span></span>
<span data-ttu-id="a0bed-216">Säkerhetskopiera objekt tooget hello PowerShell-objektet som identifierar hello höger, start från hello behållare i hello valvet och gå nedåt hello objekthierarkin.</span><span class="sxs-lookup"><span data-stu-id="a0bed-216">tooget hello PowerShell object that identifies hello right backup item, start from hello container in hello vault, and work your way down hello object hierarchy.</span></span> <span data-ttu-id="a0bed-217">tooselect hello behållare som representerar hello VM, Använd hello  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet och skicka den toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-217">tooselect hello container that represents hello VM, use hello **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet and pipe that toohello **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="a0bed-218">Välj en återställningspunkt</span><span class="sxs-lookup"><span data-stu-id="a0bed-218">Choose a recovery point</span></span>
<span data-ttu-id="a0bed-219">Använd hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  cmdlet toolist alla återställningspunkter för säkerhetskopiering hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="a0bed-219">Use hello **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet toolist all recovery points for hello backup item.</span></span> <span data-ttu-id="a0bed-220">Välj hello recovery punkt toorestore.</span><span class="sxs-lookup"><span data-stu-id="a0bed-220">Then choose hello recovery point toorestore.</span></span> <span data-ttu-id="a0bed-221">Om du inte vet vilken återställningspunkt punkt toouse är det en bra idé toochoose hello senaste RecoveryPointType = AppConsistent punkten i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="a0bed-221">If you are unsure which recovery point toouse, it is a good practice toochoose hello most recent RecoveryPointType = AppConsistent point in hello list.</span></span>

<span data-ttu-id="a0bed-222">I hello följande skript, hello variabel, **$rp**, är en matris med återställningspunkter för hello valda Säkerhetskopiera objekt från hello senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="a0bed-222">In hello following script, hello variable, **$rp**, is an array of recovery points for hello selected backup item, from hello past seven days.</span></span> <span data-ttu-id="a0bed-223">hello matrisen är sorterad i omvänd ordning tid med hello senaste återställningspunkt vid index 0.</span><span class="sxs-lookup"><span data-stu-id="a0bed-223">hello array is sorted in reverse order of time with hello latest recovery point at index 0.</span></span> <span data-ttu-id="a0bed-224">Använd standard PowerShell matris indexering toopick hello återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-224">Use standard PowerShell array indexing toopick hello recovery point.</span></span> <span data-ttu-id="a0bed-225">I exemplet hello väljer $rp [0] hello senaste återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="a0bed-225">In hello example, $rp[0] selects hello latest recovery point.</span></span>

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-hello-disks"></a><span data-ttu-id="a0bed-226">Återställa hello diskar</span><span class="sxs-lookup"><span data-stu-id="a0bed-226">Restore hello disks</span></span>
<span data-ttu-id="a0bed-227">Använd hello  **[återställning AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet toorestore Säkerhetskopiera objekt data och konfiguration tooa recovery punkt.</span><span class="sxs-lookup"><span data-stu-id="a0bed-227">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore a backup item's data and configuration tooa recovery point.</span></span> <span data-ttu-id="a0bed-228">När du har hittat en återställningspunkt, använder du den som hello värde för hello **- RecoveryPoint** parameter.</span><span class="sxs-lookup"><span data-stu-id="a0bed-228">Once you have identified a recovery point, use it as hello value for hello **-RecoveryPoint** parameter.</span></span> <span data-ttu-id="a0bed-229">I hello tidigare exempelkod **$rp [0]** var hello recovery punkt toouse.</span><span class="sxs-lookup"><span data-stu-id="a0bed-229">In hello previous sample code, **$rp[0]** was hello recovery point toouse.</span></span> <span data-ttu-id="a0bed-230">I följande exempelkod hello **$rp [0]** är hello recovery punkt toouse för att återställa hello disk.</span><span class="sxs-lookup"><span data-stu-id="a0bed-230">In hello following sample code, **$rp[0]** is hello recovery point toouse for restoring hello disk.</span></span>

<span data-ttu-id="a0bed-231">toorestore hello diskar och konfigurationsinformation:</span><span class="sxs-lookup"><span data-stu-id="a0bed-231">toorestore hello disks and configuration information:</span></span>

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="a0bed-232">Använd hello  **[vänta AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet toowait för hello återställning jobbet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a0bed-232">Use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet toowait for hello Restore job toocomplete.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

<span data-ttu-id="a0bed-233">När hello Återställningsjobbet har slutförts kan du använda hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  cmdlet tooget hello information om hello återställningsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a0bed-233">Once hello Restore job has completed, use hello **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet tooget hello details of hello restore operation.</span></span> <span data-ttu-id="a0bed-234">Hej JobDetails egenskapen har hello information behövs toorebuild hello VM.</span><span class="sxs-lookup"><span data-stu-id="a0bed-234">hello JobDetails property has hello information needed toorebuild hello VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

<span data-ttu-id="a0bed-235">När du återställer hello diskar går toohello nästa avsnitt toocreate hello VM.</span><span class="sxs-lookup"><span data-stu-id="a0bed-235">Once you restore hello disks, go toohello next section toocreate hello VM.</span></span>

## <a name="create-a-vm-from-restored-disks"></a><span data-ttu-id="a0bed-236">Skapa en virtuell dator från återställda diskar</span><span class="sxs-lookup"><span data-stu-id="a0bed-236">Create a VM from restored disks</span></span>
<span data-ttu-id="a0bed-237">När du har återställt hello diskar, använder de här stegen toocreate och konfigurera hello virtuell dator från disken.</span><span class="sxs-lookup"><span data-stu-id="a0bed-237">After you have restored hello disks, use these steps toocreate and configure hello virtual machine from disk.</span></span>

> [!NOTE]
> <span data-ttu-id="a0bed-238">toocreate krypterade virtuella datorer från återställda diskar, din Azure roll måste ha behörigheten tooperform hello åtgärd **Microsoft.KeyVault/vaults/deploy/action**.</span><span class="sxs-lookup"><span data-stu-id="a0bed-238">toocreate encrypted VMs from restored disks, your Azure role must have permission tooperform hello action, **Microsoft.KeyVault/vaults/deploy/action**.</span></span> <span data-ttu-id="a0bed-239">Om din roll inte har denna behörighet kan du skapa en anpassad roll med den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a0bed-239">If your role does not have this permission, create a custom role with this action.</span></span> <span data-ttu-id="a0bed-240">Mer information finns i [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-240">For more information, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>
>
>

1. <span data-ttu-id="a0bed-241">Frågan hello återställt diskegenskaper för hello jobbinformation.</span><span class="sxs-lookup"><span data-stu-id="a0bed-241">Query hello restored disk properties for hello job details.</span></span>

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. <span data-ttu-id="a0bed-242">Ange hello Azure storage-kontexten och återställa hello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a0bed-242">Set hello Azure storage context and restore hello JSON configuration file.</span></span>

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. <span data-ttu-id="a0bed-243">Använd hello JSON-fil toocreate hello VM konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a0bed-243">Use hello JSON configuration file toocreate hello VM configuration.</span></span>

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. <span data-ttu-id="a0bed-244">Koppla hello operativsystemdisken och datadiskar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-244">Attach hello OS disk and data disks.</span></span> <span data-ttu-id="a0bed-245">Beroende på hello konfiguration av din virtuella dator, klicka på hello relevant länk tooview respektive cmdlets:</span><span class="sxs-lookup"><span data-stu-id="a0bed-245">Depending on hello configuration of your VMs, click on hello relevant link tooview respective cmdlets:</span></span> 
    - [<span data-ttu-id="a0bed-246">Ej hanterade, icke-krypterade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-246">Non-managed, non-encrypted VMs</span></span>](#non-managed-non-encrypted-vms)
    - [<span data-ttu-id="a0bed-247">Ej hanterade, krypterad virtuella datorer (endast BEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-247">Non-managed, encrypted VMs (BEK only)</span></span>](#non-managed-encrypted-vms-bek-only)
    - [<span data-ttu-id="a0bed-248">Ej hanterade, krypterad virtuella datorer (BEK och KEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-248">Non-managed, encrypted VMs (BEK and KEK)</span></span>](#non-managed-encrypted-vms-bek-and-kek)
    - [<span data-ttu-id="a0bed-249">Hanterad, icke-krypterade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-249">Managed, non-encrypted VMs</span></span>](#managed-non-encrypted-vms)
    - [<span data-ttu-id="a0bed-250">Hanteras krypterade virtuella datorer (BEK och KEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-250">Managed, encrypted VMs (BEK and KEK)</span></span>](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a><span data-ttu-id="a0bed-251">Ej hanterade, icke-krypterade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-251">Non-managed, non-encrypted VMs</span></span>

    <span data-ttu-id="a0bed-252">Använd hello följande exempel för icke-hanterad, icke-krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-252">Use hello following sample for non-managed, non-encrypted VMs.</span></span>

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a><span data-ttu-id="a0bed-253">Ej hanterade, krypterad virtuella datorer (endast BEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-253">Non-managed, encrypted VMs (BEK only)</span></span>

    <span data-ttu-id="a0bed-254">För icke-hanterad och krypterad virtuella datorer (krypterade endast med BEK), behöver du toorestore hello hemliga toohello nyckelvalv innan du kan koppla diskar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-254">For non-managed, encrypted VMs (encrypted using BEK only), you need toorestore hello secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="a0bed-255">Mer information finns i artikeln hello [återställa en krypterad virtuell dator från en återställningspunkt för Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-255">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="a0bed-256">hello som följande exempel visar hur tooattach OS- och datadiskar för krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-256">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="a0bed-257">Ej hanterade, krypterad virtuella datorer (BEK och KEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-257">Non-managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="a0bed-258">För icke-hanterad och krypterad virtuella datorer (krypterad med BEK och KEK), måste toorestore hello nyckel och hemliga toohello nyckelvalv innan du kan koppla diskar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-258">For non-managed, encrypted VMs (encrypted using BEK and KEK), you need toorestore hello key and secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="a0bed-259">Mer information finns i artikeln hello [återställa en krypterad virtuell dator från en återställningspunkt för Azure Backup](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-259">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="a0bed-260">hello som följande exempel visar hur tooattach OS- och datadiskar för krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-260">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="managed-non-encrypted-vms"></a><span data-ttu-id="a0bed-261">Hanterad, icke-krypterade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a0bed-261">Managed, non-encrypted VMs</span></span>

    <span data-ttu-id="a0bed-262">För hanterade icke-krypterade VMs ska du behöver toocreate hanterade diskar från blob storage och bifoga hello diskar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-262">For managed non-encrypted VMs, you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="a0bed-263">Detaljerad information finns i hello artikeln [bifoga en data disk tooa virtuell Windows-dator med hjälp av PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-263">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="a0bed-264">hello följande exempelkod visar hur tooattach hello datadiskar för hanterade icke-krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-264">hello following sample code shows how tooattach hello data disks for managed non-encrypted VMs.</span></span>

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="a0bed-265">Hanteras krypterade virtuella datorer (BEK och KEK)</span><span class="sxs-lookup"><span data-stu-id="a0bed-265">Managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="a0bed-266">För hanterade krypterade virtuella datorer (krypterad med BEK och KEK), ska du behöver toocreate hanterade diskar från blob storage och bifoga hello diskar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-266">For managed encrypted VMs (encrypted using BEK and KEK), you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="a0bed-267">Detaljerad information finns i hello artikeln [bifoga en data disk tooa virtuell Windows-dator med hjälp av PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-267">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="a0bed-268">hello följande exempelkod visar hur tooattach hello datadiskar för hanterade krypterade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a0bed-268">hello following sample code shows how tooattach hello data disks for managed encrypted VMs.</span></span>

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

5. <span data-ttu-id="a0bed-269">Ange hello nätverksinställningar.</span><span class="sxs-lookup"><span data-stu-id="a0bed-269">Set hello Network settings.</span></span>

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. <span data-ttu-id="a0bed-270">Skapa hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a0bed-270">Create hello virtual machine.</span></span>

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a><span data-ttu-id="a0bed-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0bed-271">Next steps</span></span>
<span data-ttu-id="a0bed-272">Om du föredrar toouse PowerShell tooengage med Azure-resurser finns hello PowerShell artikel [distribuera och hantera säkerhetskopiering för Windows Server](backup-client-automation.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-272">If you prefer toouse PowerShell tooengage with your Azure resources, see hello PowerShell article, [Deploy and Manage Backup for Windows Server](backup-client-automation.md).</span></span> <span data-ttu-id="a0bed-273">Om du hanterar DPM-säkerhetskopieringar finns hello artikeln [distribuera och hantera säkerhetskopiering för DPM](backup-dpm-automation.md).</span><span class="sxs-lookup"><span data-stu-id="a0bed-273">If you manage DPM backups, see hello article, [Deploy and Manage Backup for DPM](backup-dpm-automation.md).</span></span> <span data-ttu-id="a0bed-274">Båda dessa artiklar har en version för klassiska distributioner och Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="a0bed-274">Both of these articles have a version for Resource Manager deployments and Classic deployments.</span></span>  
