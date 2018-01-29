---
title: "Säkerhetskopiera Windows Server till Azure med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur du distribuerar och hanterar Azure Backup med hjälp av PowerShell"
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
ms.openlocfilehash: 5a7189d9ccc8ab7aee61cd32e465b2c9b63680d2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/21/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>Distribuera och hantera säkerhetskopiering till Azure för Windows Server/Windows-klient med hjälp av PowerShell
Den här artikeln visar hur du använder PowerShell för att konfigurera Azure Backup på Windows Server eller en Windows-klient och hantera säkerhetskopiering och återställning.

## <a name="install-azure-powershell"></a>Installera Azure PowerShell
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Den här artikeln fokuserar på Azure Resource Manager (ARM) och MS Online Backup PowerShell-cmdlets som kan du använda ett Recovery Services-valv i en resursgrupp.

Azure PowerShell 1.0 gavs ut i oktober 2015. Den här versionen lyckades 0.9.8-versionen släpps och medfört några viktiga ändringar, särskilt i namnmönstret cmdlet. 1.0-cmdletar följer namngivningsmönstret {verb}-AzureRm {substantiv}, medan 0.9.8-namn inte inkluderar **Rm** (till exempel New-AzureRmResourceGroup istället för New-AzureResourceGroup). När du använder Azure PowerShell 0.9.8, måste du först aktivera Resource Manager-läget genom att köra kommandot **Switch-AzureMode AzureResourceManager**. Det här kommandot behövs inte i 1.0 eller senare.

Om du vill använda skript skrivna för 0.9.8-versionen miljö i miljön 1.0 eller senare, du bör noggrant uppdaterar och testa skripten i en förproduktionsmiljö innan du använder dem i produktion för att undvika oväntade påverkan.

[Hämta den senaste versionen av PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
Följande steg leder dig genom att skapa ett Recovery Services-valvet. Recovery Services-valvet skiljer sig en Backup-valvet.

1. Om du använder Azure Backup för första gången, måste du använda den **registrera AzureRMResourceProvider** för att registrera providern Azure Recovery-tjänsten med din prenumeration.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Recovery Services-valvet är en ARM-resurs, så du måste placera det inom en resursgrupp. Du kan använda en befintlig resursgrupp eller skapa en ny. När du skapar en ny resursgrupp måste ange namn och plats för resursgruppen.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Använd den **ny AzureRmRecoveryServicesVault** för att skapa det nya valvet. Se till att ange samma plats för valvet som användes för resursgruppen.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Ange vilken typ av lagring redundans ska användas. Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). I följande exempel visas alternativet - BackupStorageRedundancy för testVault anges till GeoRedundant.

   > [!TIP]
   > Många Azure Backup-cmdletar kräver Recovery Services-valvet objekt som indata. Därför är det praktiskt att lagra säkerhetskopian Recovery Services-valvet objekt i en variabel.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Visa valv i en prenumeration
Använd **Get-AzureRmRecoveryServicesVault** att visa listan över alla valv i den aktuella prenumerationen. Du kan använda det här kommandot för att kontrollera att ett nytt valv skapades eller för att se vilka valv är tillgängliga i prenumerationen.

Kör kommandot, **Get-AzureRmRecoveryServicesVault**, och alla valv i prenumerationen visas.

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


## <a name="installing-the-azure-backup-agent"></a>Installera Azure Backup-agenten
Innan du installerar Azure Backup-agenten som du behöver ha installationsprogrammet hämtade och finns på Windows Server. Du kan hämta den senaste versionen av installationsprogrammet från den [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från instrumentpanelssida Recovery Services-valvet. Spara installationsprogrammet i en lättillgänglig plats som * C:\Downloads\*.

Du kan också använda PowerShell för att hämta under:
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

Om du vill installera agenten, kör du följande kommando i en upphöjd PowerShell-konsol:

```
PS C:\> MARSAgentInstaller.exe /q
```

Detta installerar agent med alla standardalternativ. Installationen tar några minuter i bakgrunden. Om du inte anger den */nu* alternativet sedan **Windows Update** öppnas i slutet av installationen att söka efter uppdateringar. När du har installerat visas agenten i listan över installerade program.

Listan över installerade program, gå till **Kontrollpanelen** > **program** > **program och funktioner**.

![Agenten har installerats](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Installationsalternativ
Om du vill se alla alternativ som är tillgängliga via kommandoraden använder du följande kommando:

```
PS C:\> MARSAgentInstaller.exe /?
```

Tillgängliga alternativ inkluderar:

| Alternativ | Information | Standard |
| --- | --- | --- |
| /q |Tyst installation |- |
| / p: ”plats” |Sökvägen till installationsmappen för Azure Backup-agenten. |C:\Program Files\Microsoft Azure Recovery Services-agenten |
| / s: ”plats” |Sökväg till cachemappen för Azure Backup-agenten. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Delta i Microsoft Update |- |
| /nu |Sök inte efter uppdateringar när installationen är klar |- |
| /d |Avinstallerar Microsoft Azure Recovery Services-agenten |- |
| /pH |Värden proxyadress |- |
| /po |Portnummer för proxyservern värden |- |
| /Pu |Värddatorn Proxyanvändarnamnet |- |
| /PW |Lösenord för proxy |- |

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Registrering av Windows Server eller Windows-klientdatorn till ett Recovery Services-valv
När du har skapat Recovery Services-valvet, ladda ned den senaste agenten och autentiseringsuppgifter för valv och lagra den på en lämplig plats som C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

På Windows Server och Windows-klientdatorn och kör den [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) för att registrera datorn med valvet.
Det här och andra cmdletar som används för säkerhetskopiering, kommer från modulen MSONLINE som Mars AgentInstaller läggas till som en del av installationen. 

Installationsprogrammet för agenten inte uppdatera $Env: PSModulePath variabeln. Det innebär att modulen automatisk inläsning misslyckas. För att lösa problemet kan du göra följande:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

Du kan också du kan manuellt läsa in modulen i skriptet på följande sätt:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

När du läser in Online Backup-cmdletar, kan du registrera autentiseringsuppgifter för valv:


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
> Använd inte relativa sökvägar för att ange valvautentiseringsfilen. Du måste ange en absolut sökväg som indata till cmdleten.
>
>

## <a name="networking-settings"></a>Nätverksinställningar
När anslutningen för Windows-dator till internet via en proxyserver anges proxyinställningarna också till agenten. I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.

Du kan även styra bandbreddsanvändningen med alternativ för ```work hour bandwidth``` och ```non-work hour bandwidth``` för en given uppsättning dagar i veckan.

Ange information för proxy och bandbredd görs med hjälp av den [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Krypteringsinställningar
Den säkerhetskopiera informationen som skickas till Azure Backup krypteras för att skydda sekretessen för data. Krypteringslösenfrasen är ”password” att dekryptera data vid tidpunkten för återställning.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> Behåll informationen lösenfras säkert när den har angetts. Du kan inte återställa data från Azure utan lösenfras.
>
>

## <a name="back-up-files-and-folders"></a>Säkerhetskopiera filer och mappar
Alla säkerhetskopieringar från Windows-servrar och klienter till Azure Backup styrs av en princip. Principen består av tre delar:

1. En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver tas och synkroniseras med tjänsten.
2. En **bevarandeschema** som anger hur lång tid att behålla återställningspunkter i Azure.
3. En **filen inkludering/undantag specifikationen** som avgör vad bör säkerhetskopieras.

I det här dokumentet eftersom vi automatisera säkerhetskopiering, vi antar ingenting inte har konfigurerats. Vi börjar med att skapa en ny princip för säkerhetskopiering med hjälp av [ny OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.

```
PS C:\> $newpolicy = New-OBPolicy
```

Just nu principen är tom och andra cmdletar är behövs för att definiera vilka objekt som ska inkluderade eller exkluderade, när säkerhetskopieringar ska köras, och där säkerhetskopiorna lagras.

### <a name="configuring-the-backup-schedule"></a>Konfigurera schemat för säkerhetskopiering
Först 3 delar av en princip är schemat för säkerhetskopiering, som skapas med hjälp av den [ny OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet. Schemat för säkerhetskopiering definierar när säkerhetskopieringar behöver åtgärdas. När du skapar ett schema som du måste ange 2 indataparametrar:

* **Dagar i veckan** som säkerhetskopieringen ska köras. Du kan köra jobbet på bara en dag eller varje dag i veckan eller en kombination i mellan.
* **Tider på dagen** när säkerhetskopieringen ska köras. Du kan ange upp till 3 olika tidpunkter på dagen när säkerhetskopieringen ska utlösas.

Du kan till exempel konfigurera en princip för säkerhetskopiering som körs klockan 4 varje lördag och söndag.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Schemat för säkerhetskopiering måste vara kopplad till en princip och detta kan uppnås med hjälp av den [Set OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurera en bevarandeprincip
Bevarandeprincipen definierar hur länge återställningspunkter som skapats från säkerhetskopieringsjobb bevaras. När du skapar en ny kvarhållning principen med hjälp av [ny OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) kan du ange antalet dagar som återställningspunkter för säkerhetskopiering ska behållas med Azure Backup. Exemplet nedan anger en bevarandeprincip 7 dagar.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Bevarandeprincipen måste associeras med den huvudsakliga principen med hjälp av cmdlet [Set OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

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
### <a name="including-and-excluding-files-to-be-backed-up"></a>Inklusive och exklusive filer som ska säkerhetskopieras
En ```OBFileSpec``` objektet definierar filerna som ska inkluderas och exkluderas i en säkerhetskopia. Det här är en uppsättning regler som omfång för skyddade filer och mappar på en dator. Du kan använda så många filen inkluderande eller exkluderande regler som krävs och koppla dem till en princip. När du skapar ett nytt OBFileSpec-objekt, kan du:

* Ange vilka filer och mappar som ska tas med
* Ange vilka filer och mappar som ska uteslutas
* Ange rekursiva säkerhetskopiering av data i en mapp (eller) om endast de översta filerna i den angivna mappen bör säkerhetskopieras upp.

Denna uppnås med hjälp av flaggan - rekursiv i kommandot New OBFileSpec.

I exemplet nedan vi säkerhetskopiera volym C: och D: och utesluta OS-binärfiler i Windows-mappen och alla temporära mappar. För att göra så skapar vi två filen anvisningar med hjälp av den [ny OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - en för inkludering och en för exnkludering. När filen specifikationer har skapats, de är kopplade till en princip med hjälp av den [Lägg till OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.

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

### <a name="applying-the-policy"></a>Tillämpa principen
Nu principobjektet är klar och har en associerad schemat för säkerhetskopiering, bevarandeprincip och en inkludering/undantagslista av filer. Den här principen kan nu allokeras för Azure Backup för att använda. Innan du installerar den nya principen se till att det inte finns några befintliga principer för säkerhetskopiering som är kopplade till servern med hjälp av den [ta bort OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet. Ta bort principen uppmanas att bekräfta. Hoppa över bekräftelse användningen av ```-Confirm:$false``` flaggan med cmdlet.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

Genomför principobjektet görs med hjälp av den [Set OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet. Detta kommer också ombeds bekräfta. Hoppa över bekräftelse användningen av ```-Confirm:$false``` flaggan med cmdlet.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
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

Du kan visa information om en befintlig princip för säkerhetskopiering med hjälp av den [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet. Du kan öka detaljnivån ytterligare med den [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) för schemat för säkerhetskopiering och [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet för bevarandeprinciper

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
När du har angett en princip för säkerhetskopiering sker säkerhetskopieringar per schemat. Utlöser en ad hoc-säkerhetskopiering är också möjligt med hjälp av den [Start OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Återställa data från Azure Backup
Det här avsnittet hjälper dig genom stegen för att automatisera återställning av data från Azure Backup. Detta omfattar följande steg:

1. Välj källvolymen
2. Välj en säkerhetskopieringspunkt att återställa
3. Välj ett objekt om du vill återställa
4. Utlösa återställningen

### <a name="picking-the-source-volume"></a>Plocka källvolymen
För att kunna återställa ett objekt från Azure Backup, måste du först identifiera källan för artikeln. Eftersom vi körs kommandon i kontexten för en Windows Server eller en Windows-klient måste identifieras redan datorn. Nästa steg för att identifiera källan är att identifiera volymen som innehåller den. En lista över volymer eller källor säkerhetskopieras från den här datorn kan hämtas genom att köra den [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet. Det här kommandot returnerar en matris med alla källor säkerhetskopierade från den här servern eller-klienten.

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

### <a name="choosing-a-backup-point-from-which-to-restore"></a>Om du väljer en plats för säkerhetskopia för återställning
Du kan hämta en lista över säkerhetskopiering punkter genom att köra den [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdleten med lämpliga parametrar. I vårt exempel väljer du den senaste säkerhetskopieringskälla tidpunkten för källvolymen *D:* och använda den för att återställa en viss fil.

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
Objektet ```$rps``` är en matris med säkerhetskopiering punkter. Det första elementet är den senaste tidpunkten och nte elementet är den äldsta. Om du vill välja den senaste tidpunkten, kommer vi att använda ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Om du väljer ett objekt om du vill återställa
Att identifiera exakt filen eller mappen för att återställa rekursivt används den [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet. På så sätt kan bläddra mapphierarkin enbart med den ```Get-OBRecoverableItem```.

I det här exemplet, om vi vill återställa filen *finances.xls* vi kan referera till som använder objektet ```$filesFolders[1]```.

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

Du kan också söka efter objekt som ska återställas med hjälp av den ```Get-OBRecoverableItem``` cmdlet. I vårt exempel att söka efter *finances.xls* vi gick att hämta en referens i filen genom att köra det här kommandot:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Utlösa återställningen
För att utlösa återställningen behöver vi först ange återställningsalternativ. Detta kan göras med hjälp av den [ny OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet. I det här exemplet antar vi att vi vill återställa filerna till *C:\temp*. Vi förutsätter också att vi vill hoppa över filer som redan finns i målmappen *C:\temp*. Om du vill skapa ett återställningsalternativ, använder du följande kommando:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Nu utlösa återställningen med de [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) på den valda ```$item``` från utdata från den ```Get-OBRecoverableItem``` cmdlet:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>Avinstallera Azure Backup-agenten
Avinstallera Azure Backup-agenten kan göras med hjälp av följande kommando:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Avinstallera binärfilerna för agenten från datorn påverkar vissa att tänka på:

* Fil-filter tas bort från datorn och spårning av ändringar har stoppats.
* All principinformation tas bort från datorn, men principinformation fortsätter lagras i tjänsten.
* Alla säkerhetskopieringsscheman tas bort och vidtas inga ytterligare säkerhetskopieringar.

Data lagras i Azure förblir och behålls enligt kvarhållning princip installationen av du. Äldre återställningspunkter rensas automatiskt bort.

## <a name="remote-management"></a>Fjärrhantering
All hantering runt Azure Backup agent, principer och datakällor kan göras via fjärranslutning via PowerShell. Den dator som ska hanteras via en fjärranslutning måste förberedas på rätt sätt.

Som standard konfigureras WinRM-tjänsten för manuell start. Starttyp måste anges till *automatisk* och tjänsten ska startas. Om du vill kontrollera att WinRM-tjänsten körs, värdet för egenskapen Status ska vara *kör*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell ska konfigureras för fjärrkommunikation.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Datorn kan nu fjärrhanteras - från installationen av agenten. Följande skript kopierar agenten till fjärrdatorn och installerar den.

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

* [Introduktion till Azure Backup](backup-introduction-to-azure-backup.md)
* [Säkerhetskopiera Windows-servrar](backup-configure-vault.md)
