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
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a>Använd AzureRM.RecoveryServices.Backup cmdlets tooback av virtuella datorer
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klassisk](backup-azure-vms-classic-automation.md)
>
>

Den här artikeln visar hur toouse Azure PowerShell-cmdlets tooback in och återställer en virtuell Azure virtuell dator (VM) från en Recovery Services-valvet. Recovery Services-valvet är en Azure Resource Manager-resurs och använda tooprotect data och tillgångar i både Azure Backup och Azure Site Recovery services. Du kan använda en Recovery Services-valvet tooprotect Azure Service Manager-distribuerade virtuella datorer och Azure Resource Manager distribuerade virtuella datorer.

> [!NOTE]
> Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln ska användas med virtuella datorer som skapats med hjälp av hello Resource Manager-modellen.
>
>

Den här artikeln vägleder dig genom med hjälp av PowerShell tooprotect en virtuell dator och Återställ data från en återställningspunkt.

## <a name="concepts"></a>Koncept
Om du inte är bekant med hello Azure Backup-tjänsten för en översikt över hello tjänsten kolla [vad är Azure Backup?](backup-introduction-to-azure-backup.md) Innan du börjar, kontrollera att täcka hello essentials om hello kraven toowork med Azure Backup och hello begränsningar av hello aktuella VM lösning för säkerhetskopiering.

toouse PowerShell effektivt, är det nödvändigt toounderstand hello hierarkin med objekt och varifrån toostart.

![Recovery Services-objekthierarkin](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

tooview hello AzureRm.RecoveryServices.Backup PowerShell cmdlet-referens, se hello [Azure Backup - Recovery Services cmdletar](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) i hello Azure-biblioteket.

## <a name="setup-and-registration"></a>Installation och registrering
toobegin:

1. [Hämta hello senaste versionen av PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello lägsta version som krävs är: 1.4.0)
2. Hitta hello Azure Backup PowerShell-cmdlets som är tillgängliga genom att skriva följande kommando hello:

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


hello följande aktiviteter kan automatiseras med PowerShell:

* Skapa ett Recovery Services-valv
* Säkerhetskopiera virtuella Azure-datorer
* Utlösa ett säkerhetskopieringsjobb
* Övervaka ett säkerhetskopieringsjobb
* Återställa en virtuell dator i Azure

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
hello följande steg leder dig genom att skapa ett Recovery Services-valvet. Recovery Services-valvet skiljer sig en Backup-valvet.

1. Om du använder Azure Backup för hello första gången, måste du använda hello  **[registrera AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  cmdlet tooregister hello Azure Recovery Service provider med din prenumeration.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. hello Recovery Services-valvet är en Resource Manager-resurs, så du måste tooplace den inom en resursgrupp. Du kan använda en befintlig resursgrupp eller skapa en resursgrupp med hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  cmdlet. När du skapar en resursgrupp, ange hello namn och plats för hello resursgrupp.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. Använd hello  **[ny AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  cmdlet toocreate hello Recovery Services-valvet. Glöm toospecify hello samma plats för hello valvet som användes för hello resursgrupp.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. Ange hello typ av lagring redundans toouse; Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). hello följande exempel visar hello - BackupStorageRedundancy alternativet för testvault anges tooGeoRedundant.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Många Azure Backup-cmdletar kräver hello Recovery Services-valvet objekt som indata. Därför är det praktiskt toostore hello säkerhetskopiering Recovery Services-valvet objekt i en variabel.
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a>Visa hello valv i en prenumeration
Använd  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  tooview hello lista över alla valv i hello nuvarande prenumeration. Du kan använda det här kommandot toocheck att ett nytt valv skapades eller toosee hello tillgängliga valv i hello prenumerationen.

Kör hello-kommandot Get-AzureRmRecoveryServicesVault tooview alla valv i hello prenumerationen. hello visar följande exempel hello information som visas för varje valvet.

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


## <a name="back-up-azure-vms"></a>Säkerhetskopiera virtuella Azure-datorer
Använd en Recovery Services-valvet tooprotect dina virtuella datorer. Innan du installerar hello skydd ange hello valvet kontext (hello typ av data som skyddas i hello valvet) och kontrollera hello protection-principen. hello protection-principen är hello schemalägga när hello säkerhetskopieringsjobb körs och hur länge varje ögonblicksbild av säkerhetskopian ska sparas.

### <a name="set-vault-context"></a>Ange valvet kontext
Innan du aktiverar skyddet på en virtuell dator använda  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello valvet kontext. När hello valvet kontexten har angetts gäller tooall efterföljande cmdlets. hello följande exempel anger hello valvet kontext för hello valvet, *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Skapa en skyddsprincip för
När du skapar ett Recovery Services-valv kommer med standardskydd och bevarandeprinciper. hello standardprincipen protection utlöser en säkerhetskopiering varje dag vid en viss tidpunkt. hello standard bevarandeprincip behåller hello dagliga återställningspunkt under 30 dagar. Du kan använda hello standard princip tooquickly skydda den virtuella datorn och redigera hello principen senare med annan information.

Använd  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  tooview hello protection-principer i hello-valvet. Du kan använda den här cmdlet-tooget en specifik princip eller tooview hello principerna som associeras med en Arbetsbelastningstyp. hello följande exempel hämtar principer för Arbetsbelastningstyp, AzureVM.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> hello tidszonen i hello BackupTime fält i PowerShell är UTC. När hello säkerhetskopieringstiden visas i hello Azure-portalen, men är justerade tooyour lokala tidszon hello tid.
>
>

En säkerhetskopiering protection-principen är associerad med minst en bevarandeprincip. Bevarandeprincip definierar hur länge en återställningspunkt sparas innan den tas bort. Använd  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  tooview hello standard bevarandeprincip.  På liknande sätt kan du använda  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello standardprincipen schema. Hej  **[ny AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet skapar ett PowerShell-objekt som innehåller information om princip för säkerhetskopiering. hello schema och förvaring principobjekt används som anger toohello  **[ny AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet. hello lagrar följande exempel hello schema principen och hello bevarandeprincip i variabler. hello exemplet används dessa variabler toodefine hello parametrar när du skapar en skyddsprincip *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Aktivera skydd
När du har definierat hello säkerhetskopiering protection-principen kan aktivera du fortfarande hello princip för ett objekt. Använd  **[aktivera AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable skydd. Aktiverar skydd kräver två objekt - hello och hello principdata. När hello princip har associerats med hello valvet, utlöses hello säkerhetskopiering arbetsflödet när hello enligt ett schema för hello.

följande exempel aktiverar skydd för hello objekt, V2VM, med hjälp av hello princip, NewPolicy hello. tooenable hello skydd på icke-krypterade hanteraren för virtuella datorer

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello skydd på krypterade virtuella datorer (krypterad med BEK och KEK) måste du toogive hello Azure Backup service behörighet tooread nycklar och hemligheter från nyckelvalvet.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello skydd på krypterade virtuella datorer (krypterade endast med BEK) måste du toogive hello Azure Backup service behörighet tooread hemligheter från nyckelvalvet.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Om du använder hello Azure Government molnet använder hello värdet ff281ffe-705c-4f53-9f37-a40e6f2c68f3 för hello parametern **- ServicePrincipalName** i [Set AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet .
>
>

För klassiska virtuella datorer

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Ändra en protection-princip
toomodify hello protection-principen, Använd [Set AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy eller RetentionPolicy objekt.

hello ändras följande exempel hello recovery punkt kvarhållning too365 dagar.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Utlös en säkerhetskopiering
Du kan använda  **[säkerhetskopiering AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger ett säkerhetskopieringsjobb. Om det är första hello-säkerhetskopian är en fullständig säkerhetskopia. Efterföljande säkerhetskopieringar ta en inkrementell kopia. Vara säker på att toouse  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello valvet kontexten innan hello säkerhetskopieringsjobb. hello följande exempel förutsätter valvet kontext angavs.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> hello tidszonen hello StartTime och EndTime fält i PowerShell är UTC. När hello tid visas i hello Azure-portalen, men är justerade tooyour lokala tidszon hello tid.
>
>

## <a name="monitoring-a-backup-job"></a>Övervakning av ett säkerhetskopieringsjobb
Du kan övervaka långvariga åtgärder, till exempel säkerhetskopieringsjobb, utan att använda hello Azure-portalen. tooget hello status för ett pågående jobb, Använd hello  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  cmdlet. Denna cmdlet hämtar hello säkerhetskopieringsjobb för en specifik valvet och att valvet har angetts i kontexten för hello-valvet. hello följande exempel hämtar hello status för ett pågående jobb som en matris och lagrar hello status i hello $joblist variabel.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Använd i stället för avsökning av dessa jobb för slutförande - som är onödiga ytterligare kod - hello  **[vänta AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet. Denna cmdlet pausar körningen hello tills hello jobbet slutförs eller hello anges tidsgränsen har nåtts.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Återställa en virtuell dator i Azure
Det finns en nyckel skillnaden mellan hello återställa en virtuell dator med hjälp av hello Azure-portalen och återställa en virtuell dator med hjälp av PowerShell. Hello återställningen är slutförd med PowerShell när hello diskar och konfigurationsinformation från hello återställningspunkt skapas.

> [!NOTE]
> hello återställningen skapar inte en virtuell dator.
>
>

toocreate en virtuell dator från disken, se avsnittet hello [skapa hello VM från lagrade diskar](backup-azure-vms-automation.md#create-a-vm-from-stored-disks). hello grundläggande steg toorestore en Azure VM är:

* Välj hello VM
* Välj en återställningspunkt
* Återställa hello diskar
* Skapa hello VM från lagrade diskar

hello visar följande bild hello objekthierarkin från hello RecoveryServicesVault ned toohello BackupRecoveryPoint.

![Recovery Services objekthierarkin visar BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

toorestore säkerhetskopierade data, identifiera hello säkerhetskopierade objekt och hello återställningspunkt som innehåller hello tidpunkts-data. Använd hello  **[återställning AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet toorestore data från hello valvet toohello kundens konto.

### <a name="select-hello-vm"></a>Välj hello VM
Säkerhetskopiera objekt tooget hello PowerShell-objektet som identifierar hello höger, start från hello behållare i hello valvet och gå nedåt hello objekthierarkin. tooselect hello behållare som representerar hello VM, Använd hello  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet och skicka den toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Välj en återställningspunkt
Använd hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  cmdlet toolist alla återställningspunkter för säkerhetskopiering hello-objektet. Välj hello recovery punkt toorestore. Om du inte vet vilken återställningspunkt punkt toouse är det en bra idé toochoose hello senaste RecoveryPointType = AppConsistent punkten i hello-listan.

I hello följande skript, hello variabel, **$rp**, är en matris med återställningspunkter för hello valda Säkerhetskopiera objekt från hello senaste sju dagarna. hello matrisen är sorterad i omvänd ordning tid med hello senaste återställningspunkt vid index 0. Använd standard PowerShell matris indexering toopick hello återställningspunkt. I exemplet hello väljer $rp [0] hello senaste återställningspunkten.

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



### <a name="restore-hello-disks"></a>Återställa hello diskar
Använd hello  **[återställning AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet toorestore Säkerhetskopiera objekt data och konfiguration tooa recovery punkt. När du har hittat en återställningspunkt, använder du den som hello värde för hello **- RecoveryPoint** parameter. I hello tidigare exempelkod **$rp [0]** var hello recovery punkt toouse. I följande exempelkod hello **$rp [0]** är hello recovery punkt toouse för att återställa hello disk.

toorestore hello diskar och konfigurationsinformation:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Använd hello  **[vänta AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet toowait för hello återställning jobbet toocomplete.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

När hello Återställningsjobbet har slutförts kan du använda hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  cmdlet tooget hello information om hello återställningsåtgärden. Hej JobDetails egenskapen har hello information behövs toorebuild hello VM.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

När du återställer hello diskar går toohello nästa avsnitt toocreate hello VM.

## <a name="create-a-vm-from-restored-disks"></a>Skapa en virtuell dator från återställda diskar
När du har återställt hello diskar, använder de här stegen toocreate och konfigurera hello virtuell dator från disken.

> [!NOTE]
> toocreate krypterade virtuella datorer från återställda diskar, din Azure roll måste ha behörigheten tooperform hello åtgärd **Microsoft.KeyVault/vaults/deploy/action**. Om din roll inte har denna behörighet kan du skapa en anpassad roll med den här åtgärden. Mer information finns i [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. Frågan hello återställt diskegenskaper för hello jobbinformation.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. Ange hello Azure storage-kontexten och återställa hello JSON-konfigurationsfil.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Använd hello JSON-fil toocreate hello VM konfiguration.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Koppla hello operativsystemdisken och datadiskar. Beroende på hello konfiguration av din virtuella dator, klicka på hello relevant länk tooview respektive cmdlets: 
    - [Ej hanterade, icke-krypterade virtuella datorer](#non-managed-non-encrypted-vms)
    - [Ej hanterade, krypterad virtuella datorer (endast BEK)](#non-managed-encrypted-vms-bek-only)
    - [Ej hanterade, krypterad virtuella datorer (BEK och KEK)](#non-managed-encrypted-vms-bek-and-kek)
    - [Hanterad, icke-krypterade virtuella datorer](#managed-non-encrypted-vms)
    - [Hanteras krypterade virtuella datorer (BEK och KEK)](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a>Ej hanterade, icke-krypterade virtuella datorer

    Använd hello följande exempel för icke-hanterad, icke-krypterade virtuella datorer.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Ej hanterade, krypterad virtuella datorer (endast BEK)

    För icke-hanterad och krypterad virtuella datorer (krypterade endast med BEK), behöver du toorestore hello hemliga toohello nyckelvalv innan du kan koppla diskar. Mer information finns i artikeln hello [återställa en krypterad virtuell dator från en återställningspunkt för Azure Backup](backup-azure-restore-key-secret.md). hello som följande exempel visar hur tooattach OS- och datadiskar för krypterade virtuella datorer.

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

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Ej hanterade, krypterad virtuella datorer (BEK och KEK)

    För icke-hanterad och krypterad virtuella datorer (krypterad med BEK och KEK), måste toorestore hello nyckel och hemliga toohello nyckelvalv innan du kan koppla diskar. Mer information finns i artikeln hello [återställa en krypterad virtuell dator från en återställningspunkt för Azure Backup](backup-azure-restore-key-secret.md). hello som följande exempel visar hur tooattach OS- och datadiskar för krypterade virtuella datorer.

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

    #### <a name="managed-non-encrypted-vms"></a>Hanterad, icke-krypterade virtuella datorer

    För hanterade icke-krypterade VMs ska du behöver toocreate hanterade diskar från blob storage och bifoga hello diskar. Detaljerad information finns i hello artikeln [bifoga en data disk tooa virtuell Windows-dator med hjälp av PowerShell](../virtual-machines/windows/attach-disk-ps.md). hello följande exempelkod visar hur tooattach hello datadiskar för hanterade icke-krypterade virtuella datorer.

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Hanteras krypterade virtuella datorer (BEK och KEK)

    För hanterade krypterade virtuella datorer (krypterad med BEK och KEK), ska du behöver toocreate hanterade diskar från blob storage och bifoga hello diskar. Detaljerad information finns i hello artikeln [bifoga en data disk tooa virtuell Windows-dator med hjälp av PowerShell](../virtual-machines/windows/attach-disk-ps.md). hello följande exempelkod visar hur tooattach hello datadiskar för hanterade krypterade virtuella datorer.

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

5. Ange hello nätverksinställningar.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Skapa hello virtuell dator.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a>Nästa steg
Om du föredrar toouse PowerShell tooengage med Azure-resurser finns hello PowerShell artikel [distribuera och hantera säkerhetskopiering för Windows Server](backup-client-automation.md). Om du hanterar DPM-säkerhetskopieringar finns hello artikeln [distribuera och hantera säkerhetskopiering för DPM](backup-dpm-automation.md). Båda dessa artiklar har en version för klassiska distributioner och Resource Manager distributioner.  
