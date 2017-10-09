---
title: aaaUse PowerShell tooback in Windows Server tooAzure | Microsoft Docs
description: "Lär dig hur toodeploy och hantera Azure Backup med hjälp av PowerShell"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: f13224f53abd6fbd132fee4347b0b99e8f5e2678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a>Distribuera och hantera säkerhetskopiering tooAzure för Windows Server och Windows-klient med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [ARM](backup-client-automation.md)
> * [Klassisk](backup-client-automation-classic.md)
>
>

Den här artikeln beskrivs hur du toouse PowerShell för att konfigurera Azure Backup på Windows Server eller en Windows-klient och hantera säkerhetskopiering och återställning.

## <a name="install-azure-powershell"></a>Installera Azure PowerShell
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Den här artikeln fokuserar på hello Azure Resource Manager (ARM) och hello MS Online Backup PowerShell-cmdlets som möjliggör toouse Recovery Services-valv i en resursgrupp.

Azure PowerShell 1.0 gavs ut i oktober 2015. Den här versionen lyckades hello 0.9.8 versionen och medfört några viktiga ändringar, särskilt i mönstret för hello hello-cmdlets. 1.0 cmdlets Följ hello namngivningsmönstret {verb}-AzureRm {substantiv}; medan hello 0.9.8 inte 0.9.8-namn inkluderar **Rm** (till exempel New-AzureRmResourceGroup istället för New-AzureResourceGroup). När du använder Azure PowerShell 0.9.8, måste du först aktivera hello Resource Manager-läget genom att köra hello **Switch-AzureMode AzureResourceManager** kommando. Det här kommandot behövs inte i 1.0 eller senare.

Om du vill toouse skripten skrivna för hello 0.9.8 miljö i hello 1.0 eller senare miljö kan du noggrant bör uppdatera och testa hello skript i en förproduktionsmiljö innan du använder dem i produktion tooavoid oväntat påverkan.

[Hämta hello senaste PowerShell versionen](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
hello följande steg leder dig genom att skapa ett Recovery Services-valvet. Recovery Services-valvet skiljer sig en Backup-valvet.

1. Om du använder Azure Backup för hello första gången, måste du använda hello **registrera AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider med din prenumeration.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. hello Recovery Services-valvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp. Du kan använda en befintlig resursgrupp eller skapa en ny. När du skapar en ny resursgrupp, ange hello namn och plats för hello resursgrupp.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Använd hello **ny AzureRmRecoveryServicesVault** cmdlet toocreate hello nytt valv. Glöm toospecify hello samma plats för hello valvet som användes för hello resursgrupp.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Ange hello typ av lagring redundans toouse; Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). hello följande exempel visar hello - BackupStorageRedundancy alternativet för testVault anges tooGeoRedundant.

   > [!TIP]
   > Många Azure Backup-cmdletar kräver hello Recovery Services-valvet objekt som indata. Därför är det praktiskt toostore hello säkerhetskopiering Recovery Services-valvet objekt i en variabel.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a>Visa hello valv i en prenumeration
Använd **Get-AzureRmRecoveryServicesVault** tooview hello lista över alla valv i hello nuvarande prenumeration. Du kan använda det här kommandot toocheck att ett nytt valv skapades eller toosee vilka valv är tillgängliga i hello prenumeration.

Kör hello kommando **Get-AzureRmRecoveryServicesVault**, och alla valv i hello prenumeration visas.

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


## <a name="installing-hello-azure-backup-agent"></a>Installera hello Azure Backup-agenten
Innan du installerar hello Azure Backup-agenten måste toohave hello installer hämtats och finns på hello Windows Server. Du kan hämta hello senaste versionen av hello installer från hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från hello återställningstjänster valvet instrumentpanelens sida. Spara hello installer tooan lättillgänglig plats som * C:\Downloads\*.

Du kan också använda PowerShell tooget hello installationshämtaren:
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

tooinstall hello agent, kör följande kommando i en upphöjd PowerShell-konsol hello:

```
PS C:\> MARSAgentInstaller.exe /q
```

Detta installerar hello agent med alla hello standardvärdet. hello installationen tar några minuter i hello bakgrund. Om du inte anger hello */nu* alternativet sedan hello **Windows Update** öppnas hello slutet av hello installation toocheck för alla uppdateringar. När du har installerat visas hello agent i hello listan över installerade program.

toosee hello listan över installerade program finns för**Kontrollpanelen** > **program** > **program och funktioner**.

![Agenten är installerad](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Installationsalternativ
toosee alla hello alternativ som är tillgängliga via hello kommandoradsverktyg, använder hello följande kommando:

```
PS C:\> MARSAgentInstaller.exe /?
```

hello tillgängliga alternativ inkluderar:

| Alternativ | Information | Standard |
| --- | --- | --- |
| /q |Tyst installation |- |
| / p: ”plats” |Sökvägen toohello installationsmappen för hello Azure Backup-agenten. |C:\Program Files\Microsoft Azure Recovery Services-agenten |
| / s: ”plats” |Sökvägen toohello cachemappen för hello Azure Backup-agenten. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Anmäl tooMicrosoft uppdatering |- |
| /nu |Sök inte efter uppdateringar när installationen är klar |- |
| /d |Avinstallerar Microsoft Azure Recovery Services-agenten |- |
| /pH |Värden proxyadress |- |
| /po |Portnummer för proxyservern värden |- |
| /Pu |Värddatorn Proxyanvändarnamnet |- |
| /PW |Lösenord för proxy |- |

## <a name="registering-windows-server-or-windows-client-machine-tooa-recovery-services-vault"></a>Registrering av Windows Server eller Windows-klienten datorn tooa Recovery Services-valvet
När du har skapat hello Recovery Services-valvet Hämta senaste hello-agenten och hello valvautentiseringsuppgifter och lagra den på en lämplig plats som C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

Kör på hello Windows Server eller klientdator för Windows hello [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello datorn med hello-valvet.
Detta och andra cmdletar som används för säkerhetskopiering, är du vilka hello Mars AgentInstaller läggas till som en del av installationsprocessen hello från hello MSONLINE modul. 

hello installationsprogrammet för agenten inte uppdatera hello $Env: PSModulePath variabeln. Det innebär att modulen automatisk inläsning misslyckas. tooresolve detta kan du göra hello följande:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

Alternativt kan du manuellt läsa in modulen hello i skriptet på följande sätt:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

När du läser in hello Online Backup-cmdletar, kan du registrera hello valvautentiseringsuppgifter:


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Du inte använda relativa sökvägar toospecify hello valvautentiseringsfilen. Du måste ange en absolut sökväg som en inkommande toohello cmdlet.
>
>

## <a name="networking-settings"></a>Nätverksinställningar
När hello anslutningen för hello Windows datorn toohello internet är via en proxyserver, kan hello proxyinställningar också anges toohello agent. I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.

Bandbreddsanvändning kan också kontrolleras med hello-alternativen för ```work hour bandwidth``` och ```non-work hour bandwidth``` för en given uppsättning hello veckodagar.

Ange information för proxy och bandbredd av hello görs med hjälp av hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Krypteringsinställningar
hello säkerhetskopierade data skickas tooAzure säkerhetskopiering är krypterad tooprotect hello sekretess hello. Hej krypteringslösenfrasen har hello ”password” toodecrypt hello data vid hello tiden för återställning.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> Hålla hello lösenfras information säkert när den har angetts. Det är inte kan toorestore data från Azure utan lösenfras.
>
>

## <a name="back-up-files-and-folders"></a>Säkerhetskopiera filer och mappar
Alla säkerhetskopieringar från Windows-servrar och klienter tooAzure Säkerhetskopiering styrs av en princip. hello princip består av tre delar:

1. En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver toobe tas och synkroniseras med hello-tjänst.
2. En **bevarandeschema** som anger hur länge tooretain hello Återställningspunkter i Azure.
3. En **filen inkludering/undantag specifikationen** som avgör vad bör säkerhetskopieras.

I det här dokumentet eftersom vi automatisera säkerhetskopiering, vi antar ingenting inte har konfigurerats. Vi börjar med att skapa en ny säkerhetskopieringsprincip med hello [ny OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.

```
PS C:\> $newpolicy = New-OBPolicy
```

På den här gången hello princip är tom och andra cmdletar är nödvändiga toodefine vad som ska inkluderas eller uteslutas, när säkerhetskopieringar ska köras och där hello säkerhetskopior kommer att lagras.

### <a name="configuring-hello-backup-schedule"></a>Konfigurera schemat för säkerhetskopiering av hello
Hej först hello 3 delar av en princip är hello säkerhetskopieringsschema som skapas med hjälp av hello [ny OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet. hello schemat för säkerhetskopiering definierar när säkerhetskopieringar behöver toobe som vidtagits. När du skapar ett schema måste toospecify 2 indataparametrar:

* **Hello veckodagar** att hello säkerhetskopiering ska köras. Du kan köra hello säkerhetskopieringsjobb på bara en dag eller varje dag i veckan hello eller en kombination i mellan.
* **Tidpunkter på dagen hello** när hello säkerhetskopiering ska köras. Du kan definiera upp too3 olika tidpunkter på hello dagen när hello säkerhetskopiering ska utlösas.

Du kan till exempel konfigurera en princip för säkerhetskopiering som körs klockan 4 varje lördag och söndag.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

schemat för säkerhetskopiering av hello måste toobe som är associerade med en princip, och detta kan uppnås genom att använda hello [Set OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurera en bevarandeprincip
hello bevarandeprincip definierar hur länge recovery återställningspunkter som skapats från säkerhetskopieringsjobb bevaras. När du skapar en ny bevarandeprincip med hello [ny OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) kan du ange hello antalet dagar som hello Återställningspunkter för säkerhetskopiering måste toobe behålls med Azure Backup. hello exemplet nedan anger en bevarandeprincip 7 dagar.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

hello bevarandeprincip måste associeras med hello huvudsakliga princip med hello cmdlet [Set OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-toobe-backed-up"></a>Inklusive och utan filer toobe säkerhetskopieras
En ```OBFileSpec``` objektet definierar hello filer toobe inkluderas och exkluderas i en säkerhetskopia. Det här är en uppsättning regler som att scope ut hello skyddade filer och mappar på en dator. Du kan använda så många filen inkluderande eller exkluderande regler som krävs och koppla dem till en princip. När du skapar ett nytt OBFileSpec-objekt, kan du:

* Ange hello filer och mappar toobe ingår
* Ange hello filer och mappar toobe undantas
* Ange rekursiva säkerhetskopiering av data i en mapp (eller) om endast hello översta filer i hello angiven mapp ska säkerhetskopieras upp.

hello senare uppnås med hjälp av hello - rekursiv flaggan i hello ny OBFileSpec kommandot.

I hello exemplet nedan vi säkerhetskopiera volym C: och D: och utesluta hello OS-binärfiler i hello Windows-mappen och alla temporära mappar. toodo så skapar vi två filen anvisningar med hjälp av hello [ny OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - en för inkludering och en för exnkludering. När hello filen specifikationer har skapats, de är associerade med hello-princip med hello [Lägg till OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-hello-policy"></a>Hello-principer tillämpas
Nu hello principobjektet är klar och har en associerad schemat för säkerhetskopiering, bevarandeprincip och en inkludering/undantagslista av filer. Den här principen kan nu allokeras för Azure Backup toouse. Innan du installerar hello nyskapad princip Se till att det inte finns några befintliga principer för säkerhetskopiering som associeras med hello-servern med hjälp av hello [ta bort OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet. Tar bort hello princip uppmanas att bekräfta. tooskip hello bekräftelse använda hello ```-Confirm:$false``` flagga hello cmdlet.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

Genomför hello principobjektet görs med hjälp av hello [Set OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet. Detta kommer också ombeds bekräfta. tooskip hello bekräftelse använda hello ```-Confirm:$false``` flagga hello cmdlet.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Du kan visa hello information om hello säkerhetskopieringsprincip med hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet. Du kan gå nedåt ytterligare med hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet för hello schemat för säkerhetskopiering och hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet för hello bevarandeprinciper

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Utför en ad hoc-säkerhetskopiering
När du har angett en princip för säkerhetskopiering sker hello säkerhetskopior per hello schema. Utlöser en ad hoc-säkerhetskopiering kan också använda hello [Start OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing hello metadata VHD...
Data transfer is in progress. It might take longer since it is hello first backup and all data needs toobe transferred...
Data transfer completed and all backed up data is in hello cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Återställa data från Azure Backup
Det här avsnittet får du stegvisa hello stegen för att automatisera återställning av data från Azure Backup. Detta omfattar så hello följande steg:

1. Välj hello källvolymen
2. Välj en säkerhetskopiering punkt toorestore
3. Välj ett objekt toorestore
4. Utlösaren hello återställningsprocessen

### <a name="picking-hello-source-volume"></a>Plocka hello källvolymen
I ordning toorestore ett objekt från Azure Backup måste du först tooidentify hello källan för hello-objektet. Eftersom vi körs hello kommandon hello kontexten för en Windows Server eller en Windows-klient måste identifieras redan hello-datorn. hello nästa steg för att identifiera hello källa är tooidentify hello volymen som innehåller den. En lista över volymer eller källor säkerhetskopieras från den här datorn kan hämtas genom att köra hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet. Det här kommandot returnerar en matris med alla hello källor säkerhetskopierade från den här servern eller-klienten.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-toorestore"></a>Att välja en plats för säkerhetskopiering från vilken toorestore
Hämta en lista över säkerhetskopiering punkter genom att köra hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdleten med lämpliga parametrar. I vårt exempel väljer du hello senaste säkerhetskopieringspunkt för hello källvolymen *D:* och använda den toorecover en viss fil.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
hello objektet ```$rps``` är en matris med säkerhetskopiering punkter. hello första elementet är hello senaste tidpunkten och hello nte element är hello äldsta. toochoose hello senaste tidpunkten, kommer vi att använda ```$rps[0]```.

### <a name="choosing-an-item-toorestore"></a>Om du väljer ett objekt toorestore
tooidentify hello exakt filen eller mappen toorestore rekursivt använda hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet. Sätt hello mapphierarkin kan visas med enbart hello ```Get-OBRecoverableItem```.

I det här exemplet, om vi vill toorestore hello filen *finances.xls* vi kan referera till den med hjälp av hello objektet ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Du kan också söka efter objekt toorestore med hello ```Get-OBRecoverableItem``` cmdlet. I vårt exempel toosearch för *finances.xls* vi gick att hämta en referens på hello-filen genom att köra det här kommandot:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a>Utlösande hello återställningsprocessen
återställningsprocessen för tootrigger hello vi måste först toospecify hello återställningsalternativ. Detta kan göras med hjälp av hello [ny OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet. I det här exemplet antar vi att som vi vill toorestore hello filer för*C:\temp*. Vi förutsätter också att vi vill tooskip filer som redan finns på hello målmappen *C:\temp*. toocreate sådana ett återställningsalternativ använda hello följande kommando:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Nu utlösa hello återställningsprocessen med hello [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) på hello valt ```$item``` från hello utdata från hello ```Get-OBRecoverableItem``` cmdlet:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a>Avinstallera hello Azure Backup-agenten
Avinstallera hello Azure Backup-agenten kan göras med hjälp av hello följande kommando:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Avinstallera hello agent binärfiler från hello datorn har vissa konsekvenser tooconsider:

* Hello filfilter tas bort från datorn hello och spårning av ändringar har stoppats.
* All principinformation tas bort från hello dator, men hello principinformation fortsätter toobe lagras i hello-tjänsten.
* Alla säkerhetskopieringsscheman tas bort och vidtas inga ytterligare säkerhetskopieringar.

Hello data lagras i Azure förblir och behålls enligt hello kvarhållning princip installationen av du. Äldre återställningspunkter rensas automatiskt bort.

## <a name="remote-management"></a>Fjärrhantering
Alla hello management runt hello Azure Backup-agenten, principer och datakällor kan göras via fjärranslutning via PowerShell. hello-datorn som ska hanteras via en fjärranslutning måste toobe förberetts på rätt sätt.

Som standard har hello WinRM-tjänsten konfigurerats för manuell start. hello starttyp måste anges för*automatisk* och hello-tjänsten ska startas. tooverify som hello WinRM-tjänsten körs, hello statusegenskapen hello värdet bör vara *kör*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell ska konfigureras för fjärrkommunikation.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

hello-dator kan nu fjärrhanteras - från hello installationen av hello agent. Följande skript hello kopierar agent hello toohello fjärrdatorn och installerar den.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Backup för Windows Server eller-klienten finns

* [Introduktion tooAzure säkerhetskopiering](backup-introduction-to-azure-backup.md)
* [Säkerhetskopiera Windows-servrar](backup-configure-vault.md)
