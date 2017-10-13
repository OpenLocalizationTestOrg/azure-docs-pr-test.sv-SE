---
title: "Azure-säkerhetskopiering: Använd PowerShell för att säkerhetskopiera DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur du distribuerar och hanterar Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>Distribuera och hantera säkerhetskopiering till Azure för DPM-servrar (Data Protection Manager) med PowerShell
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Klassisk](backup-dpm-automation-classic.md)
>
>

Den här artikeln beskriver hur du använder PowerShell för att säkerhetskopiera och återställa DPM-data från ett säkerhetskopieringsvalv. Microsoft rekommenderar att du använder Recovery Services-valv för alla nya distributioner. Om du är en ny Azure Backup-användare kan använda i artikeln [distribuera och hantera Data Protection Manager-data till Azure med hjälp av PowerShell](backup-dpm-automation.md), så att du lagrar data i en Recovery Services-valvet.

> [!IMPORTANT]
> Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv. Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv). Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv. Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.
>- Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen. Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="setting-up-the-powershell-environment"></a>Ställa in PowerShell-miljö
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Innan du kan använda PowerShell för att hantera säkerhetskopiering från Data Protection Manager till Azure, behöver du har rätt miljö i PowerShell. Se till att du kör följande kommando för att importera modulerna som är rätt och att du kan korrekt refererar till DPM-cmdlets i början av PowerShell-session:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Installation och registrering
Börja:

1. [Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)
2. Aktivera Azure Backup-kommandon genom att växla till *AzureResourceManager* läge med hjälp av den **Switch-AzureMode** cmdleten igen:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

Följande uppgifter för installation och registrering kan automatiseras med PowerShell:

* Skapa ett säkerhetskopieringsvalv
* Installera Azure Backup-agenten
* Registreras med Azure Backup-tjänsten
* Nätverksinställningar
* Krypteringsinställningar

### <a name="create-a-backup-vault"></a>Skapa ett säkerhetskopieringsvalv
> [!WARNING]
> För kunder som använder Azure Backup för första gången, som du behöver registrera Azure Backup-provider som ska användas med din prenumeration. Detta kan göras genom att köra följande kommando: registrera AzureProvider - ProviderNamespace ”Microsoft.Backup”
>
>

Du kan skapa ett nytt säkerhetskopieringsvalv med den **ny AzureRMBackupVault** cmdleten igen. Säkerhetskopieringsvalvet är en ARM-resurs, så du måste placera det inom en resursgrupp. Kör följande kommandon i en kommandotolk med förhöjd Azure PowerShell-konsol:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Du kan hämta en lista över alla säkerhetskopieringsvalv i en viss prenumeration med hjälp av den **Get-AzureRMBackupVault** cmdleten igen.

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Installation av Azure Backup-agenten på en DPM-Server
Innan du installerar Azure Backup-agenten som du behöver ha installationsprogrammet hämtade och finns på Windows Server. Du kan hämta den senaste versionen av installationsprogrammet från den [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från det säkerhetskopieringsvalv instrumentpanelens sida. Spara installationsprogrammet i en lättillgänglig plats som * C:\Downloads\*.

Om du vill installera agenten, kör du följande kommando i en upphöjd PowerShell-konsol **på DPM-servern**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Detta installerar agent med alla standardalternativ. Installationen tar några minuter i bakgrunden. Om du inte anger den */nu* alternativet den **Windows Update** öppnas i slutet av installationen att söka efter uppdateringar.

Agenten visas i listan över installerade program. Listan över installerade program, gå till **Kontrollpanelen** > **program** > **program och funktioner**.

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Installationsalternativ
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

### <a name="registering-with-the-azure-backup-service"></a>Registreras med Azure Backup-tjänsten
Innan du kan registrera med Azure Backup-tjänsten måste du se till att den [krav](backup-azure-dpm-introduction.md) är uppfyllda. Du måste:

* Har ett giltigt Azure-prenumeration
* Har ett säkerhetskopieringsvalv

Om du vill hämta valvautentiseringsuppgifter som kör den **Get-AzureBackupVaultCredentials** kommandot i en Azure PowerShell-konsolen och lagra den i en lämplig plats som * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Registrera datorn med valvet görs med hjälp av den [Start DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

DPM-servern med namnet ”TestingServer” med Microsoft Azure-valvet med hjälp av de angivna valvautentiseringsuppgifter registreras.

> [!IMPORTANT]
> Använd inte relativa sökvägar för att ange valvautentiseringsfilen. Du måste ange en absolut sökväg som indata till cmdleten.
>
>

### <a name="initial-configuration-settings"></a>Inledande konfigurationsinställningar
När DPM-servern registreras med Azure Backup-valvet, startar den med standardinställningar för prenumerationen. Inställningarna prenumeration omfattar nätverk, kryptering och mellanlagringsområdet. Om du vill ändra prenumerationsinställningar måste du först få grepp om de befintliga inställningarna för (standard) med hjälp av den [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Alla ändringar som görs till den här lokala PowerShell-objektet ```$setting``` och sedan fullständig objektet strävar efter att DPM och Azure Backup för att spara dem med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. Du måste använda den ```–Commit``` flagga för att säkerställa att ändringarna sparas. Inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Nätverk
Om anslutningen till DPM-datorn till Azure Backup-tjänsten på internet via en proxyserver, måste inställningarna för proxyservern anges för säkerhetskopiering ska lyckas. Detta görs med hjälp av den ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` och ```ProxyPassword``` parametrar med den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning dagar i veckan. I det här exemplet anger vi inte någon begränsning.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a>Konfigurera mellanlagringsområdet
Azure Backup-agenten som körs på DPM-servern behöver tillfällig lagring för data som återställs från molnet (lokalt mellanlagringsområde). Konfigurera mellanlagringsområdet med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och ```-StagingAreaPath``` parameter.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

I exemplet ovan mellanlagringsområdet anges till *C:\StagingArea* i PowerShell-objektet ```$setting```. Se till att den angivna mappen finns redan, annars misslyckas det slutliga genomförandet av prenumerationsinställningar.

### <a name="encryption-settings"></a>Krypteringsinställningar
Den säkerhetskopiera informationen som skickas till Azure Backup krypteras för att skydda sekretessen för data. Krypteringslösenfrasen är ”password” att dekryptera data vid tidpunkten för återställning. Det är viktigt att behålla den här informationen säkert när den har angetts.

I exemplet nedan det första kommandot konverterar strängen ```passphrase123456789``` till en säker sträng och tilldelar säker sträng till variabeln med namnet ```$Passphrase```. Det andra kommandot anger säker sträng i ```$Passphrase``` som lösenord för kryptering av säkerhetskopieringar.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Behåll informationen lösenfras säkert när den har angetts. Du kommer inte att återställa data från Azure utan lösenfras.
>
>

Nu ska du har gjort alla nödvändiga ändringar av den ```$setting``` objekt. Kom ihåg att spara ändringarna.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Skydda data till Azure Backup
I det här avsnittet kan du lägga till en produktionsservern till DPM och skydda sedan data till lokala DPM-lagring och sedan till Azure Backup. I exemplen visar vi hur du säkerhetskopierar filer och mappar. Logiken kan enkelt utökas för att säkerhetskopiera alla datakällor som stöds av DPM. Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:

1. **Medlemmar** är en lista över alla skyddsobjekt (även kallat *datakällor* i DPM) som du vill skydda i samma skyddsgrupp. Du kanske vill skydda produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering. Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste du kontrollera att DPM-agenten är installerad på servern och hanteras av DPM. Följ anvisningarna för [installera DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länkas till rätt DPM-servern.
2. **Dataskyddsmetod** anger de målplatserna - band, disk och molnet. I vårt exempel skyddar vi data till den lokala disken och till molnet.
3. En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver tas och hur ofta data ska synkroniseras mellan DPM-servern och produktionsservern.
4. En **bevarandeschema** som anger hur lång tid att behålla återställningspunkter i Azure.

### <a name="creating-a-protection-group"></a>Skapa en skyddsgrupp
Börja med att skapa en ny Skyddsgrupp med hjälp av [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Cmdleten ovan skapar en Skyddsgrupp med namnet *ProtectGroup01*. En befintlig skyddsgrupp kan också ändras senare om du vill lägga till säkerhetskopiering till Azure-molnet. Dock göra några ändringar i skyddsgruppen - ny eller befintlig - vi behöver skaffa en referens till en *ändringsbar* objekt med hjälp av [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Lägga till medlemmar i skyddsgruppen
Varje DPM-agenten vet listan med datakällor på den server som den är installerad på. DPM-agenten måste först skicka en lista med datakällorna på DPM-servern för att lägga till en datakälla i skyddsgruppen. En eller flera datakällor är sedan valt och lägga till Skyddsgruppen. PowerShell-åtgärder som behövs för att få uppnå detta är:

1. Hämta en lista över alla servrar som hanterade med DPM via DPM-agenten.
2. Välj en specifik server.
3. Hämta en lista över alla datakällor på servern.
4. Välj en eller flera datakällor och lägga till dem i Skyddsgruppen

I listan med servrar där DPM-agenten är installerad och hanteras av DPM-servern förvärvas med den [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet. I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Nu att hämta listan över datakällor på ```$server``` med hjälp av den [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet. I det här exemplet vi filtrering för volymen * D:\* som vi vill konfigurera för säkerhetskopiering. Den här datakällan läggs sedan till Skyddsgruppen med hjälp av den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet. Kom ihåg att använda den *modifable* skydd gruppobjekt ```$MPG``` att tilläggen.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Upprepa detta steg så många gånger som krävs, tills du har lagt till de valda datakällorna i skyddsgruppen. Du kan också starta med en datakälla och slutföra arbetsflödet för att skapa Skyddsgruppen och senare lägga till flera datakällor i Skyddsgruppen.

### <a name="selecting-the-data-protection-method"></a>Välja dataskyddsmetod
När datakällorna har lagts till Skyddsgruppen, nästa steg är att ange metoden skydd med den [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet. I det här exemplet Skyddsgruppen inte inställningar för lokal disk och säkerhetskopiering i molnet. Du måste också ange datasource som du vill skydda till molnet med den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Ange kvarhållningsintervallet
Loggperiod för säkerhetskopieringen återställningspunkter med de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet. När det kan verka udda loggperiod innan schemat för säkerhetskopiering har definierats med den ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras. Det är alltid möjligt att ange säkerhetskopieringen schemalägga först och bevarandeprincip efter.

I exemplet nedan anger cmdleten kvarhållning parametrarna för säkerhetskopiering till disk. Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan produktionsservern och DPM-servern. Den ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data kopieras till DPM-servern; Detta förhindrar att säkerhetskopiorna blir för stor.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

För säkerhetskopiering ska Azure (DPM refererar till dessa som onlinesäkerhetskopieringar) i kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md). Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper. I det här exemplet vi skapa en matris som representerar det komplexa kvarhållning schema som vi vill och sedan konfigurera kvarhållning intervall med den [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Ange schemat för säkerhetskopiering
DPM anger en säkerhetskopiering standardschemat automatiskt om du anger en skydd mål med hjälp av den ```Set-DPMPolicyObjective``` cmdlet. Du kan ändra standardscheman den [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av den [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

I exemplet ovan, ```$onlineSch``` är en matris med fyra element som innehåller befintliga schema för onlineskydd för Skyddsgruppen i GFS schemat:

1. ```$onlineSch[0]```innehåller dagsschema
2. ```$onlineSch[1]```innehåller veckoschemat
3. ```$onlineSch[2]```innehåller månadsschema
4. ```$onlineSch[3]```innehåller årlig schemat

Om du behöver ändra veckoschemat måste referera till den ```$onlineSch[1]```.

### <a name="initial-backup"></a>Den första säkerhetskopieringen
När du säkerhetskopierar en datakälla för första gången måste DPM skapa en första replik som kommer att skapa en kopia i datasource som ska skyddas på DPM-replikvolymen. Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av den [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet med parametern ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>Ändra storlek på DPM-replikeringen & återställningspunktvolymen
Du kan också ändra storleken på DPM-replikvolymen samt skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet som i det exemplet nedan: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Bekräfta ändringarna i skyddsgruppen
Slutligen måste ändringarna genomföras innan DPM kan säkerhetskopiera per den nya Skyddsgruppen-konfigurationen. Detta görs med hjälp av den [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a>Visa säkerhetskopiering punkter
Du kan använda den [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) att hämta en lista över alla återställningspunkter för en datakälla. I det här exemplet ska du:

* Hämta alla PGs på DPM-servern som ska lagras i en matris```$PG```
* Hämta datakällorna som motsvarar den```$PG[0]```
* Hämta alla återställningspunkter för en datakälla.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Återställa skyddade data på Azure
Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt. I det föregående avsnittet vi en lista över säkerhetskopiering punkter för en datakälla.

I exemplet nedan visar vi hur du återställer en virtuell dator för Hyper-V från Azure Backup genom att kombinera säkerhetskopiering punkter med mål för återställning. Detta omfattar:

* Skapa en återställningspunkt alternativet med hjälp av den [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.
* Hämtar punktmatrisen säkerhetskopiering med hjälp av den ```Get-DPMRecoveryPoint``` cmdlet.
* Om du väljer en säkerhetskopieringspunkt att återställa från.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Kommandona kan enkelt utökas för någon typ av datakälla.

## <a name="next-steps"></a>Nästa steg
* Läs mer om Azure Backup för DPM [introduktion till DPM-säkerhetskopiering](backup-azure-dpm-introduction.md)
