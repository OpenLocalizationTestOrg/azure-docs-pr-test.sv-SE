---
title: "aaaDeploy och hantera säkerhetskopiering för virtuella Azure-datorer med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toodeploy och hantera Azure Backup med hjälp av PowerShell."
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
ms.openlocfilehash: f3ecd3f94c5e3e8fc8018e8786302edd4847744b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermbackup-cmdlets-tooback-up-virtual-machines"></a><span data-ttu-id="75a74-103">Använd AzureRM.Backup cmdlets tooback av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="75a74-103">Use AzureRM.Backup cmdlets tooback up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="75a74-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="75a74-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="75a74-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="75a74-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="75a74-106">Den här artikeln beskrivs hur du toouse Azure PowerShell för säkerhetskopiering och återställning av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="75a74-106">This article shows you how toouse Azure PowerShell for backup and recovery of Azure VMs.</span></span> <span data-ttu-id="75a74-107">I Azure finns två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="75a74-107">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="75a74-108">Den här artikeln täcker hello klassisk distribution modellen tooback upp data tooa Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="75a74-108">This article covers using hello Classic deployment model tooback up data tooa Backup vault.</span></span> <span data-ttu-id="75a74-109">Om du inte har skapat ett säkerhetskopieringsvalv i din prenumeration finns hello Resource Manager-versionen av den här artikeln [Använd AzureRM.RecoveryServices.Backup cmdlets tooback av virtuella datorer](backup-azure-vms-automation.md).</span><span class="sxs-lookup"><span data-stu-id="75a74-109">If you have not created a Backup vault in your subscription, see hello Resource Manager version of this article, [Use AzureRM.RecoveryServices.Backup cmdlets tooback up virtual machines](backup-azure-vms-automation.md).</span></span> <span data-ttu-id="75a74-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="75a74-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75a74-111">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="75a74-111">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="75a74-112">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="75a74-112">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="75a74-113">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="75a74-113">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="75a74-114">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="75a74-114">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="75a74-115">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="75a74-115">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="75a74-116">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="75a74-116">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="75a74-117">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="75a74-117">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="75a74-118">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="75a74-118">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="concepts"></a><span data-ttu-id="75a74-119">Koncept</span><span class="sxs-lookup"><span data-stu-id="75a74-119">Concepts</span></span>
<span data-ttu-id="75a74-120">Den här artikeln innehåller information om specifika toohello PowerShell-cmdlets används tooback av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="75a74-120">This article provides information specific toohello PowerShell cmdlets used tooback up virtual machines.</span></span> <span data-ttu-id="75a74-121">Grundläggande information om hur du skyddar virtuella Azure-datorer finns [planera din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="75a74-121">For introductory information about protecting Azure VMs, please see [Plan your VM backup infrastructure in Azure](backup-azure-vms-introduction.md).</span></span>

> [!NOTE]
> <span data-ttu-id="75a74-122">Innan du börjar läsa hello [krav](backup-azure-vms-prepare.md) krävs toowork med Azure Backup och hello [begränsningar](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) av hello aktuella VM lösning för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="75a74-122">Before you start, read hello [prerequisites](backup-azure-vms-prepare.md) required toowork with Azure Backup, and hello [limitations](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) of hello current VM backup solution.</span></span>
>
>

<span data-ttu-id="75a74-123">toouse PowerShell effektivt, ta en stund toounderstand hello hierarkin med objekt och varifrån toostart.</span><span class="sxs-lookup"><span data-stu-id="75a74-123">toouse PowerShell effectively, take a moment toounderstand hello hierarchy of objects and from where toostart.</span></span>

![Objekthierarkin](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

<span data-ttu-id="75a74-125">hello är två viktigaste flöden att aktivera skydd för en virtuell dator och återställa data från en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="75a74-125">hello two most important flows are enabling protection for a VM, and restoring data from a recovery point.</span></span> <span data-ttu-id="75a74-126">hello fokuserar den här artikeln på toohelp blir experter på arbeta med hello PowerShell-cmdlets tooenable dessa två scenarier.</span><span class="sxs-lookup"><span data-stu-id="75a74-126">hello focus of this article is toohelp you become adept at working with hello PowerShell cmdlets tooenable these two scenarios.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="75a74-127">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="75a74-127">Setup and Registration</span></span>
<span data-ttu-id="75a74-128">toobegin:</span><span class="sxs-lookup"><span data-stu-id="75a74-128">toobegin:</span></span>

1. <span data-ttu-id="75a74-129">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="75a74-129">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="75a74-130">Hitta hello Azure Backup PowerShell-cmdlets som är tillgängliga genom att skriva följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="75a74-130">Find hello Azure Backup PowerShell cmdlets available by typing hello following command:</span></span>

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

<span data-ttu-id="75a74-131">hello kan följande inställningar och registrering aktiviteter automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="75a74-131">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="75a74-132">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="75a74-132">Create a backup vault</span></span>
* <span data-ttu-id="75a74-133">Registrerar hello virtuella datorer med hello Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="75a74-133">Registering hello VMs with hello Azure Backup service</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="75a74-134">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="75a74-134">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="75a74-135">För kunder som använder Azure Backup för hello första gången, måste tooregister hello Azure Backup-providern toobe används med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75a74-135">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="75a74-136">Detta kan göras genom att köra följande kommando hello: registrera AzureRmResourceProvider - ProviderNamespace ”Microsoft.Backup”</span><span class="sxs-lookup"><span data-stu-id="75a74-136">This can be done by running hello following command: Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="75a74-137">Du kan skapa ett nytt säkerhetskopieringsvalv med hello **ny AzureRmBackupVault** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-137">You can create a new backup vault using hello **New-AzureRmBackupVault** cmdlet.</span></span> <span data-ttu-id="75a74-138">Hej säkerhetskopieringsvalvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="75a74-138">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="75a74-139">Kör följande kommandon hello i en upphöjd Azure PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="75a74-139">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="75a74-140">Du kan hämta en lista över alla hello säkerhetskopieringsvalv i en viss prenumeration med hjälp av hello **Get-AzureRmBackupVault** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-140">You can get a list of all hello backup vaults in a given subscription using hello **Get-AzureRmBackupVault** cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="75a74-141">Det är praktiskt toostore hello säkerhetskopieringsvalvet objekt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="75a74-141">It is convenient toostore hello backup vault object into a variable.</span></span> <span data-ttu-id="75a74-142">hello valvet objekt krävs som indata för många Azure Backup-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="75a74-142">hello vault object is needed as an input for many Azure Backup cmdlets.</span></span>
>
>

### <a name="registering-hello-vms"></a><span data-ttu-id="75a74-143">Registrera hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="75a74-143">Registering hello VMs</span></span>
<span data-ttu-id="75a74-144">hello första steg för att konfigurera säkerhetskopiering med Azure Backup är tooregister din dator eller virtuell dator med Azure Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="75a74-144">hello first step towards configuring backup with Azure Backup is tooregister your machine or VM with an Azure Backup vault.</span></span> <span data-ttu-id="75a74-145">Hej **registrera AzureRmBackupContainer** cmdlet tar hello inkommande information för en virtuell Azure IaaS-dator och registrerar den angivna hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="75a74-145">hello **Register-AzureRmBackupContainer** cmdlet takes hello input information of an Azure IaaS virtual machine and registers it with hello specified vault.</span></span> <span data-ttu-id="75a74-146">hello registrera åtgärden associerar hello Azure-dator med hello säkerhetskopieringsvalv och spårar hello VM hello säkerhetskopiering livscykeln.</span><span class="sxs-lookup"><span data-stu-id="75a74-146">hello register operation associates hello Azure virtual machine with hello backup vault and tracks hello VM through hello backup lifecycle.</span></span>

<span data-ttu-id="75a74-147">Registrera den virtuella datorn med hello Azure Backup-tjänsten skapar ett översta behållarobjekt.</span><span class="sxs-lookup"><span data-stu-id="75a74-147">Registering your VM with hello Azure Backup service creates a top-level container object.</span></span> <span data-ttu-id="75a74-148">En behållare innehåller vanligtvis flera objekt som kan säkerhetskopieras, men i hello skiftläget för virtuella datorer finns endast en säkerhetskopiering artikel för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="75a74-148">A container typically contains multiple items that can be backed up, but in hello case of VMs there will be only one backup item for hello container.</span></span>

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a><span data-ttu-id="75a74-149">Säkerhetskopiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="75a74-149">Backup Azure VMs</span></span>
### <a name="create-a-protection-policy"></a><span data-ttu-id="75a74-150">Skapa en skyddsprincip för</span><span class="sxs-lookup"><span data-stu-id="75a74-150">Create a protection policy</span></span>
<span data-ttu-id="75a74-151">Det är inte obligatoriskt toocreate en ny säkerhetskopia av din virtuella dator toostart för principen av skydd.</span><span class="sxs-lookup"><span data-stu-id="75a74-151">It is not mandatory toocreate a new protection policy toostart backup of your VMs.</span></span> <span data-ttu-id="75a74-152">hello valvet levereras med en 'standardprincip' som kan använda tooquickly Aktivera skydd och sedan redigeras senare med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="75a74-152">hello vault comes with a 'Default Policy' that can be used tooquickly enable protection, and then edited later with hello right details.</span></span> <span data-ttu-id="75a74-153">Du kan hämta en lista över hello-principer som är tillgängliga i hello valvet med hello **Get-AzureRmBackupProtectionPolicy** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="75a74-153">You can get a list of hello policies available in hello vault by using hello **Get-AzureRmBackupProtectionPolicy** cmdlet:</span></span>

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> <span data-ttu-id="75a74-154">hello tidszonen i hello BackupTime fält i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="75a74-154">hello timezone of hello BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="75a74-155">När hello säkerhetskopieringstiden visas i hello Azure-portalen, men är justerade tooyour lokalt system tillsammans med hello UTC-förskjutningen hello tidszonen.</span><span class="sxs-lookup"><span data-stu-id="75a74-155">However, when hello backup time is shown in hello Azure portal, hello timezone is aligned tooyour local system along with hello UTC offset.</span></span>
>
>

<span data-ttu-id="75a74-156">En princip för säkerhetskopiering är associerad med minst en bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="75a74-156">A backup policy is associated with at least one retention policy.</span></span> <span data-ttu-id="75a74-157">hello bevarandeprincip definierar hur länge en återställningspunkt sparas med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="75a74-157">hello retention policy defines how long a recovery point is kept with Azure Backup.</span></span> <span data-ttu-id="75a74-158">Hej **ny AzureRmBackupRetentionPolicy** cmdleten skapar PowerShell-objekt som innehåller information om principer bevarande.</span><span class="sxs-lookup"><span data-stu-id="75a74-158">hello **New-AzureRmBackupRetentionPolicy** cmdlet creates PowerShell objects that hold retention policy information.</span></span> <span data-ttu-id="75a74-159">Dessa kvarhållning principobjekt används som anger toohello *ny AzureRmBackupProtectionPolicy* cmdlet, eller direkt via hello *aktivera AzureRmBackupProtection* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-159">These retention policy objects are used as inputs toohello *New-AzureRmBackupProtectionPolicy* cmdlet, or directly with hello *Enable-AzureRmBackupProtection* cmdlet.</span></span>

<span data-ttu-id="75a74-160">En princip för säkerhetskopiering definierar när och hur ofta hello säkerhetskopia av ett objekt är klar.</span><span class="sxs-lookup"><span data-stu-id="75a74-160">A backup policy defines when and how often hello backup of an item is done.</span></span> <span data-ttu-id="75a74-161">Hej **ny AzureRmBackupProtectionPolicy** cmdlet skapar ett PowerShell-objekt som innehåller information om princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="75a74-161">hello **New-AzureRmBackupProtectionPolicy** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="75a74-162">hello säkerhetskopieringsprincip används som en inkommande toohello *aktivera AzureRmBackupProtection* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-162">hello backup policy is used as an input toohello *Enable-AzureRmBackupProtection* cmdlet.</span></span>

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a><span data-ttu-id="75a74-163">Aktivera skydd</span><span class="sxs-lookup"><span data-stu-id="75a74-163">Enable protection</span></span>
<span data-ttu-id="75a74-164">Aktiverar skydd omfattar två objekt - hello objekt och hello princip och båda måste toobelong toohello samma valvet.</span><span class="sxs-lookup"><span data-stu-id="75a74-164">Enabling protection involves two objects - hello Item and hello Policy, and both need toobelong toohello same vault.</span></span> <span data-ttu-id="75a74-165">När hello princip har associerats med hello-objektet, startar hello säkerhetskopiering arbetsflödet på hello definierat schema.</span><span class="sxs-lookup"><span data-stu-id="75a74-165">Once hello policy has been associated with hello item, hello backup workflow will kick in at hello defined schedule.</span></span>

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a><span data-ttu-id="75a74-166">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="75a74-166">Initial backup</span></span>
<span data-ttu-id="75a74-167">schemat för säkerhetskopiering av hello hand tar om gör hello första fullständig kopia för hello objektet och hello inkrementell kopia för hello efterföljande säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="75a74-167">hello backup schedule will take care of doing hello full initial copy for hello item and hello incremental copy for hello subsequent backups.</span></span> <span data-ttu-id="75a74-168">Men om du vill tooforce hello första säkerhetskopiering toohappen vid en viss tidpunkt eller även omedelbart sedan använda hello **säkerhetskopiering AzureRmBackupItem** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="75a74-168">However, if you want tooforce hello initial backup toohappen at a certain time or even immediately then use hello **Backup-AzureRmBackupItem** cmdlet:</span></span>

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="75a74-169">Hej tidszonen för hello StartTime och EndTime fält som visas i PowerShell är UTC.</span><span class="sxs-lookup"><span data-stu-id="75a74-169">hello timezone of hello StartTime and EndTime fields shown in PowerShell is UTC.</span></span> <span data-ttu-id="75a74-170">När hello liknande information visas i hello Azure-portalen, men är justerade tooyour systemklockan hello tidszonen.</span><span class="sxs-lookup"><span data-stu-id="75a74-170">However, when hello similar information is shown in hello Azure portal, hello timezone is aligned tooyour local system clock.</span></span>
>
>

### <a name="monitoring-a-backup-job"></a><span data-ttu-id="75a74-171">Övervakning av ett säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="75a74-171">Monitoring a backup job</span></span>
<span data-ttu-id="75a74-172">De flesta långvariga åtgärder i Azure Backup utformas som ett jobb.</span><span class="sxs-lookup"><span data-stu-id="75a74-172">Most long-running operations in Azure Backup are modelled as a job.</span></span> <span data-ttu-id="75a74-173">Detta gör det enkelt tootrack pågår utan tookeep hello Azure portal öppna hela tiden.</span><span class="sxs-lookup"><span data-stu-id="75a74-173">This makes it easy tootrack progress without having tookeep hello Azure portal open at all times.</span></span>

<span data-ttu-id="75a74-174">tooget hello senaste statusen för ett pågående jobb, Använd hello **Get-AzureRmBackupJob** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-174">tooget hello latest status of an in-progress job, use hello **Get-AzureRmBackupJob** cmdlet.</span></span>

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

<span data-ttu-id="75a74-175">I stället för avsökning av dessa jobb för slutförande - som är onödiga, ytterligare kod – det är enklare toouse hello **vänta AzureRmBackupJob** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-175">Instead of polling these jobs for completion - which is unnecessary, additional code - it is simpler toouse hello **Wait-AzureRmBackupJob** cmdlet.</span></span> <span data-ttu-id="75a74-176">När den används i ett skript, pausa hello cmdlet hello körningen tills hello jobbet slutförs eller hello anges tidsgränsen har nåtts.</span><span class="sxs-lookup"><span data-stu-id="75a74-176">When used in a script, hello cmdlet will pause hello execution until either hello job completes or hello specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a><span data-ttu-id="75a74-177">Återställa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="75a74-177">Restore an Azure VM</span></span>
<span data-ttu-id="75a74-178">I ordning toorestore säkerhetskopierade data behöver du tooidentify hello säkerhetskopierade objekt och hello återställningspunkt som innehåller hello tidpunkts-data.</span><span class="sxs-lookup"><span data-stu-id="75a74-178">In order toorestore backup data, you need tooidentify hello backed-up Item and hello Recovery Point that holds hello point-in-time data.</span></span> <span data-ttu-id="75a74-179">Den här informationen är angivna toohello återställning AzureRmBackupItem cmdlet tooinitiate en återställning av data från hello valvet toohello kundens konto.</span><span class="sxs-lookup"><span data-stu-id="75a74-179">This information is supplied toohello Restore-AzureRmBackupItem cmdlet tooinitiate a restore of data from hello vault toohello customer's account.</span></span>

### <a name="select-hello-vm"></a><span data-ttu-id="75a74-180">Välj hello VM</span><span class="sxs-lookup"><span data-stu-id="75a74-180">Select hello VM</span></span>
<span data-ttu-id="75a74-181">tooget hello PowerShell-objektet som identifierar Hej höger Säkerhetskopiera objekt, du behöver toostart från hello behållare i hello-valvet och gå nedåt objekthierarkin.</span><span class="sxs-lookup"><span data-stu-id="75a74-181">tooget hello PowerShell object that identifies hello right backup Item, you need toostart from hello Container in hello vault, and work your way down object hierarchy.</span></span> <span data-ttu-id="75a74-182">tooselect hello behållare som representerar hello VM, Använd hello **Get-AzureRmBackupContainer** cmdlet och skicka den toohello **Get-AzureRmBackupItem** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75a74-182">tooselect hello container that represents hello VM, use hello **Get-AzureRmBackupContainer** cmdlet and pipe that toohello **Get-AzureRmBackupItem** cmdlet.</span></span>

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="75a74-183">Välj en återställningspunkt</span><span class="sxs-lookup"><span data-stu-id="75a74-183">Choose a recovery point</span></span>
<span data-ttu-id="75a74-184">Du kan nu visa alla hello Återställningspunkter för hello Säkerhetskopiera objekt med hjälp av hello **Get-AzureRmBackupRecoveryPoint** cmdlet, och välj hello recovery punkt toorestore.</span><span class="sxs-lookup"><span data-stu-id="75a74-184">You can now list all hello recovery points for hello backup item using hello **Get-AzureRmBackupRecoveryPoint** cmdlet, and choose hello recovery point toorestore.</span></span> <span data-ttu-id="75a74-185">Vanligtvis användare välja hello senaste *AppConsistent* peka i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="75a74-185">Typically users pick hello most recent *AppConsistent* point in hello list.</span></span>

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

<span data-ttu-id="75a74-186">hello variabeln ```$rp``` är en matris med återställningspunkter för hello markerat säkerhetskopiering objekt, sorterade i omvänd ordning tid - hello senaste tillgängliga återställningspunkten är vid index 0.</span><span class="sxs-lookup"><span data-stu-id="75a74-186">hello variable ```$rp``` is an array of recovery points for hello selected backup item, sorted in reverse order of time - hello latest recovery point is at index 0.</span></span> <span data-ttu-id="75a74-187">Använd standard PowerShell matris indexering toopick hello återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="75a74-187">Use standard PowerShell array indexing toopick hello recovery point.</span></span> <span data-ttu-id="75a74-188">Till exempel: ```$rp[0]``` väljer hello senaste återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="75a74-188">For example: ```$rp[0]``` will select hello latest recovery point.</span></span>

### <a name="restoring-disks"></a><span data-ttu-id="75a74-189">Återställning av diskar</span><span class="sxs-lookup"><span data-stu-id="75a74-189">Restoring disks</span></span>
<span data-ttu-id="75a74-190">Det finns en nyckel skillnaden mellan hello återställningsåtgärderna klar via hello Azure-portalen och via Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75a74-190">There is a key difference between hello restore operations done through hello Azure portal and through Azure PowerShell.</span></span> <span data-ttu-id="75a74-191">Med PowerShell stannar hello återställningen vid återställning hello diskar och konfigurationsinformation från hello återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="75a74-191">With PowerShell, hello restore operation stops at restoring hello disks and config information from hello recovery point.</span></span> <span data-ttu-id="75a74-192">Den skapar inte en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="75a74-192">It does not create a virtual machine.</span></span>

> [!WARNING]
> <span data-ttu-id="75a74-193">Hej AzureRmBackupItem återställning kan inte skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="75a74-193">hello Restore-AzureRmBackupItem does not create a VM.</span></span> <span data-ttu-id="75a74-194">Återställs bara hello diskar toohello angetts storage-konto.</span><span class="sxs-lookup"><span data-stu-id="75a74-194">It only restores hello disks toohello specified storage account.</span></span> <span data-ttu-id="75a74-195">Detta är inte hello samma beteende som du får i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75a74-195">This is not hello same behavior you will experience in hello Azure portal.</span></span>
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

<span data-ttu-id="75a74-196">Du kan hämta hello information om hello återställningen igen med hello **Get-AzureRmBackupJobDetails** cmdlet när hello Återställningsjobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="75a74-196">You can get hello details of hello restore operation using hello **Get-AzureRmBackupJobDetails** cmdlet once hello Restore job has completed.</span></span> <span data-ttu-id="75a74-197">Hej *felinformation* egenskapen har hello information behövs toorebuild hello VM.</span><span class="sxs-lookup"><span data-stu-id="75a74-197">hello *ErrorDetails* property will have hello information needed toorebuild hello VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-hello-vm"></a><span data-ttu-id="75a74-198">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="75a74-198">Build hello VM</span></span>
<span data-ttu-id="75a74-199">Skapa hello VM utanför hello återställts diskar kan göras med hello äldre Azure Service Management PowerShell-cmdlets hello nya Azure Resource Manager-mallar eller även med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75a74-199">Building hello VM out of hello restored disks can be done using hello older Azure Service Management PowerShell cmdlets, hello new Azure Resource Manager templates, or even using hello Azure portal.</span></span> <span data-ttu-id="75a74-200">I ett enkelt exempel visas hur tooget där med hello Azure Service Management-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="75a74-200">In a quick example, we will show how tooget there using hello Azure Service Management cmdlets.</span></span>

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

<span data-ttu-id="75a74-201">Mer information om hur toobuild en virtuell dator från hello återställts diskar Läs mer om hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="75a74-201">For more information on how toobuild a VM from hello restored disks, read about hello following cmdlets:</span></span>

* [<span data-ttu-id="75a74-202">Lägg till AzureDisk</span><span class="sxs-lookup"><span data-stu-id="75a74-202">Add-AzureDisk</span></span>](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [<span data-ttu-id="75a74-203">Ny AzureVMConfig</span><span class="sxs-lookup"><span data-stu-id="75a74-203">New-AzureVMConfig</span></span>](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [<span data-ttu-id="75a74-204">Ny AzureVM</span><span class="sxs-lookup"><span data-stu-id="75a74-204">New-AzureVM</span></span>](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a><span data-ttu-id="75a74-205">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="75a74-205">Code samples</span></span>
### <a name="1-get-hello-completion-status-of-job-sub-tasks"></a><span data-ttu-id="75a74-206">1. Hämta hello Slutförandestatus för jobbet underordnade aktiviteter</span><span class="sxs-lookup"><span data-stu-id="75a74-206">1. Get hello completion status of job sub-tasks</span></span>
<span data-ttu-id="75a74-207">tootrack hello status för slutförande av enskilda underordnade aktiviteter som du kan använda hello **Get-AzureRmBackupJobDetails** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="75a74-207">tootrack hello completion status of individual sub-tasks, you can use hello **Get-AzureRmBackupJobDetails** cmdlet:</span></span>

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data tooBackup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a><span data-ttu-id="75a74-208">2. Skapa en rapport för varje dag/vecka säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="75a74-208">2. Create a daily/weekly report of backup jobs</span></span>
<span data-ttu-id="75a74-209">Administratörer vill förmodligen tooknow vilka säkerhetskopieringsjobb kördes i hello senaste 24 timmarna hello statusen för dessa säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="75a74-209">Administrators typically want tooknow what backup jobs ran in hello last 24 hours, hello status of those backup jobs.</span></span> <span data-ttu-id="75a74-210">Dessutom ger hello mängden data som överförs administratörer en sätt tooestimate sin månatlig dataanvändning.</span><span class="sxs-lookup"><span data-stu-id="75a74-210">Additionally, hello amount of data transferred gives administrators a way tooestimate their monthly data usage.</span></span> <span data-ttu-id="75a74-211">hello-skriptet nedan hämtar hello rådata från hello Azure Backup-tjänsten och visar hello information i hello PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="75a74-211">hello script below pulls hello raw data from hello Azure Backup service and displays hello information in hello PowerShell console.</span></span>

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
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -too$enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for hello last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract hello information for hello reports
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

<span data-ttu-id="75a74-212">Om du vill tooadd diagram funktioner toothis rapportutdata Läs från hello TechNet blogginlägget [diagram med PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span><span class="sxs-lookup"><span data-stu-id="75a74-212">If you want tooadd charting capabilities toothis report output, learn from hello TechNet blog post [Charting with PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="75a74-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75a74-213">Next steps</span></span>
<span data-ttu-id="75a74-214">Om du föredrar att använda PowerShell tooengage med Azure-resurser, kolla hello PowerShell artikel för att skydda Windows Server, [distribuera och hantera säkerhetskopiering för Windows Server](backup-client-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="75a74-214">If you prefer using PowerShell tooengage with your Azure resources, check out hello PowerShell article for protecting Windows Server, [Deploy and Manage Backup for Windows Server](backup-client-automation-classic.md).</span></span> <span data-ttu-id="75a74-215">Det finns också en artikel i PowerShell för att hantera DPM-säkerhetskopieringar [distribuera och hantera säkerhetskopiering för DPM](backup-dpm-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="75a74-215">There is also a PowerShell article for managing DPM backups, [Deploy and Manage Backup for DPM](backup-dpm-automation-classic.md).</span></span> <span data-ttu-id="75a74-216">Båda dessa artiklar har en version för Resource Manager distributioner samt klassiska distributioner.</span><span class="sxs-lookup"><span data-stu-id="75a74-216">Both of these articles have a version for Resource Manager deployments as well as Classic deployments.</span></span>
