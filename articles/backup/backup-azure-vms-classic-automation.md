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
# <a name="use-azurermbackup-cmdlets-tooback-up-virtual-machines"></a>Använd AzureRM.Backup cmdlets tooback av virtuella datorer
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klassisk](backup-azure-vms-classic-automation.md)
>
>

Den här artikeln beskrivs hur du toouse Azure PowerShell för säkerhetskopiering och återställning av virtuella datorer i Azure. I Azure finns två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk. Den här artikeln täcker hello klassisk distribution modellen tooback upp data tooa Backup-valvet. Om du inte har skapat ett säkerhetskopieringsvalv i din prenumeration finns hello Resource Manager-versionen av den här artikeln [Använd AzureRM.RecoveryServices.Backup cmdlets tooback av virtuella datorer](backup-azure-vms-automation.md). Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="concepts"></a>Koncept
Den här artikeln innehåller information om specifika toohello PowerShell-cmdlets används tooback av virtuella datorer. Grundläggande information om hur du skyddar virtuella Azure-datorer finns [planera din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md).

> [!NOTE]
> Innan du börjar läsa hello [krav](backup-azure-vms-prepare.md) krävs toowork med Azure Backup och hello [begränsningar](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) av hello aktuella VM lösning för säkerhetskopiering.
>
>

toouse PowerShell effektivt, ta en stund toounderstand hello hierarkin med objekt och varifrån toostart.

![Objekthierarkin](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

hello är två viktigaste flöden att aktivera skydd för en virtuell dator och återställa data från en återställningspunkt. hello fokuserar den här artikeln på toohelp blir experter på arbeta med hello PowerShell-cmdlets tooenable dessa två scenarier.

## <a name="setup-and-registration"></a>Installation och registrering
toobegin:

1. [Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)
2. Hitta hello Azure Backup PowerShell-cmdlets som är tillgängliga genom att skriva följande kommando hello:

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

hello kan följande inställningar och registrering aktiviteter automatiseras med PowerShell:

* Skapa ett säkerhetskopieringsvalv
* Registrerar hello virtuella datorer med hello Azure Backup-tjänsten

### <a name="create-a-backup-vault"></a>Skapa ett säkerhetskopieringsvalv
> [!WARNING]
> För kunder som använder Azure Backup för hello första gången, måste tooregister hello Azure Backup-providern toobe används med din prenumeration. Detta kan göras genom att köra följande kommando hello: registrera AzureRmResourceProvider - ProviderNamespace ”Microsoft.Backup”
>
>

Du kan skapa ett nytt säkerhetskopieringsvalv med hello **ny AzureRmBackupVault** cmdlet. Hej säkerhetskopieringsvalvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp. Kör följande kommandon hello i en upphöjd Azure PowerShell-konsol:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Du kan hämta en lista över alla hello säkerhetskopieringsvalv i en viss prenumeration med hjälp av hello **Get-AzureRmBackupVault** cmdlet.

> [!NOTE]
> Det är praktiskt toostore hello säkerhetskopieringsvalvet objekt i en variabel. hello valvet objekt krävs som indata för många Azure Backup-cmdletar.
>
>

### <a name="registering-hello-vms"></a>Registrera hello virtuella datorer
hello första steg för att konfigurera säkerhetskopiering med Azure Backup är tooregister din dator eller virtuell dator med Azure Backup-valvet. Hej **registrera AzureRmBackupContainer** cmdlet tar hello inkommande information för en virtuell Azure IaaS-dator och registrerar den angivna hello-valvet. hello registrera åtgärden associerar hello Azure-dator med hello säkerhetskopieringsvalv och spårar hello VM hello säkerhetskopiering livscykeln.

Registrera den virtuella datorn med hello Azure Backup-tjänsten skapar ett översta behållarobjekt. En behållare innehåller vanligtvis flera objekt som kan säkerhetskopieras, men i hello skiftläget för virtuella datorer finns endast en säkerhetskopiering artikel för hello behållare.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Säkerhetskopiera virtuella Azure-datorer
### <a name="create-a-protection-policy"></a>Skapa en skyddsprincip för
Det är inte obligatoriskt toocreate en ny säkerhetskopia av din virtuella dator toostart för principen av skydd. hello valvet levereras med en 'standardprincip' som kan använda tooquickly Aktivera skydd och sedan redigeras senare med hello rätt information. Du kan hämta en lista över hello-principer som är tillgängliga i hello valvet med hello **Get-AzureRmBackupProtectionPolicy** cmdlet:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> hello tidszonen i hello BackupTime fält i PowerShell är UTC. När hello säkerhetskopieringstiden visas i hello Azure-portalen, men är justerade tooyour lokalt system tillsammans med hello UTC-förskjutningen hello tidszonen.
>
>

En princip för säkerhetskopiering är associerad med minst en bevarandeprincip. hello bevarandeprincip definierar hur länge en återställningspunkt sparas med Azure Backup. Hej **ny AzureRmBackupRetentionPolicy** cmdleten skapar PowerShell-objekt som innehåller information om principer bevarande. Dessa kvarhållning principobjekt används som anger toohello *ny AzureRmBackupProtectionPolicy* cmdlet, eller direkt via hello *aktivera AzureRmBackupProtection* cmdlet.

En princip för säkerhetskopiering definierar när och hur ofta hello säkerhetskopia av ett objekt är klar. Hej **ny AzureRmBackupProtectionPolicy** cmdlet skapar ett PowerShell-objekt som innehåller information om princip för säkerhetskopiering. hello säkerhetskopieringsprincip används som en inkommande toohello *aktivera AzureRmBackupProtection* cmdlet.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Aktivera skydd
Aktiverar skydd omfattar två objekt - hello objekt och hello princip och båda måste toobelong toohello samma valvet. När hello princip har associerats med hello-objektet, startar hello säkerhetskopiering arbetsflödet på hello definierat schema.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Den första säkerhetskopieringen
schemat för säkerhetskopiering av hello hand tar om gör hello första fullständig kopia för hello objektet och hello inkrementell kopia för hello efterföljande säkerhetskopieringar. Men om du vill tooforce hello första säkerhetskopiering toohappen vid en viss tidpunkt eller även omedelbart sedan använda hello **säkerhetskopiering AzureRmBackupItem** cmdlet:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> Hej tidszonen för hello StartTime och EndTime fält som visas i PowerShell är UTC. När hello liknande information visas i hello Azure-portalen, men är justerade tooyour systemklockan hello tidszonen.
>
>

### <a name="monitoring-a-backup-job"></a>Övervakning av ett säkerhetskopieringsjobb
De flesta långvariga åtgärder i Azure Backup utformas som ett jobb. Detta gör det enkelt tootrack pågår utan tookeep hello Azure portal öppna hela tiden.

tooget hello senaste statusen för ett pågående jobb, Använd hello **Get-AzureRmBackupJob** cmdlet.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

I stället för avsökning av dessa jobb för slutförande - som är onödiga, ytterligare kod – det är enklare toouse hello **vänta AzureRmBackupJob** cmdlet. När den används i ett skript, pausa hello cmdlet hello körningen tills hello jobbet slutförs eller hello anges tidsgränsen har nåtts.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Återställa en virtuell dator i Azure
I ordning toorestore säkerhetskopierade data behöver du tooidentify hello säkerhetskopierade objekt och hello återställningspunkt som innehåller hello tidpunkts-data. Den här informationen är angivna toohello återställning AzureRmBackupItem cmdlet tooinitiate en återställning av data från hello valvet toohello kundens konto.

### <a name="select-hello-vm"></a>Välj hello VM
tooget hello PowerShell-objektet som identifierar Hej höger Säkerhetskopiera objekt, du behöver toostart från hello behållare i hello-valvet och gå nedåt objekthierarkin. tooselect hello behållare som representerar hello VM, Använd hello **Get-AzureRmBackupContainer** cmdlet och skicka den toohello **Get-AzureRmBackupItem** cmdlet.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Välj en återställningspunkt
Du kan nu visa alla hello Återställningspunkter för hello Säkerhetskopiera objekt med hjälp av hello **Get-AzureRmBackupRecoveryPoint** cmdlet, och välj hello recovery punkt toorestore. Vanligtvis användare välja hello senaste *AppConsistent* peka i hello-listan.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

hello variabeln ```$rp``` är en matris med återställningspunkter för hello markerat säkerhetskopiering objekt, sorterade i omvänd ordning tid - hello senaste tillgängliga återställningspunkten är vid index 0. Använd standard PowerShell matris indexering toopick hello återställningspunkt. Till exempel: ```$rp[0]``` väljer hello senaste återställningspunkten.

### <a name="restoring-disks"></a>Återställning av diskar
Det finns en nyckel skillnaden mellan hello återställningsåtgärderna klar via hello Azure-portalen och via Azure PowerShell. Med PowerShell stannar hello återställningen vid återställning hello diskar och konfigurationsinformation från hello återställningspunkt. Den skapar inte en virtuell dator.

> [!WARNING]
> Hej AzureRmBackupItem återställning kan inte skapa en virtuell dator. Återställs bara hello diskar toohello angetts storage-konto. Detta är inte hello samma beteende som du får i hello Azure-portalen.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Du kan hämta hello information om hello återställningen igen med hello **Get-AzureRmBackupJobDetails** cmdlet när hello Återställningsjobbet har slutförts. Hej *felinformation* egenskapen har hello information behövs toorebuild hello VM.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-hello-vm"></a>Skapa hello VM
Skapa hello VM utanför hello återställts diskar kan göras med hello äldre Azure Service Management PowerShell-cmdlets hello nya Azure Resource Manager-mallar eller även med hello Azure-portalen. I ett enkelt exempel visas hur tooget där med hello Azure Service Management-cmdlets.

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

Mer information om hur toobuild en virtuell dator från hello återställts diskar Läs mer om hello följande cmdlets:

* [Lägg till AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Ny AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Ny AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Kodexempel
### <a name="1-get-hello-completion-status-of-job-sub-tasks"></a>1. Hämta hello Slutförandestatus för jobbet underordnade aktiviteter
tootrack hello status för slutförande av enskilda underordnade aktiviteter som du kan använda hello **Get-AzureRmBackupJobDetails** cmdlet:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data tooBackup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Skapa en rapport för varje dag/vecka säkerhetskopieringsjobb
Administratörer vill förmodligen tooknow vilka säkerhetskopieringsjobb kördes i hello senaste 24 timmarna hello statusen för dessa säkerhetskopieringsjobb. Dessutom ger hello mängden data som överförs administratörer en sätt tooestimate sin månatlig dataanvändning. hello-skriptet nedan hämtar hello rådata från hello Azure Backup-tjänsten och visar hello information i hello PowerShell-konsolen.

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

Om du vill tooadd diagram funktioner toothis rapportutdata Läs från hello TechNet blogginlägget [diagram med PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Nästa steg
Om du föredrar att använda PowerShell tooengage med Azure-resurser, kolla hello PowerShell artikel för att skydda Windows Server, [distribuera och hantera säkerhetskopiering för Windows Server](backup-client-automation-classic.md). Det finns också en artikel i PowerShell för att hantera DPM-säkerhetskopieringar [distribuera och hantera säkerhetskopiering för DPM](backup-dpm-automation-classic.md). Båda dessa artiklar har en version för Resource Manager distributioner samt klassiska distributioner.
