---
title: "Azure-säkerhetskopiering: Använd PowerShell tooback in DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur toodeploy och hantera Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
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
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a>Distribuera och hantera säkerhetskopiering tooAzure för Data Protection Manager (DPM) servrar med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Klassisk](backup-dpm-automation-classic.md)
>
>

Den här artikeln förklarar hur toouse PowerShell tooback upp och återställa DPM-data från ett säkerhetskopieringsvalv. Microsoft rekommenderar att du använder Recovery Services-valv för alla nya distributioner. Om du är en ny Azure Backup-användare kan använda hello artikel [distribuera och hantera Data Protection Manager data tooAzure med hjälp av PowerShell](backup-dpm-automation.md), så att du lagrar data i en Recovery Services-valvet.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv. Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="setting-up-hello-powershell-environment"></a>Konfigurera hello PowerShell-miljö
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Innan du kan använda PowerShell toomanage säkerhetskopior från Data Protection Manager tooAzure måste toohave hello rätt miljö i PowerShell. Kontrollera att du kör hello efter kommandot tooimport hello rätt moduler och Tillåt toocorrectly referens hello DPM cmdlets hello början av hello PowerShell-session:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Installation och registrering
toobegin:

1. [Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)
2. Aktivera hello Azure Backup-kommandon genom att växla för*AzureResourceManager* läge med hjälp av hello **Switch-AzureMode** cmdleten igen:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

hello kan följande inställningar och registrering aktiviteter automatiseras med PowerShell:

* Skapa ett säkerhetskopieringsvalv
* Installera hello Azure Backup-agenten
* Registrera med hello Azure Backup-tjänsten
* Nätverksinställningar
* Krypteringsinställningar

### <a name="create-a-backup-vault"></a>Skapa ett säkerhetskopieringsvalv
> [!WARNING]
> För kunder som använder Azure Backup för hello första gången, måste tooregister hello Azure Backup-providern toobe används med din prenumeration. Detta kan göras genom att köra följande kommando hello: registrera AzureProvider - ProviderNamespace ”Microsoft.Backup”
>
>

Du kan skapa ett nytt säkerhetskopieringsvalv med hello **ny AzureRMBackupVault** cmdleten igen. Hej säkerhetskopieringsvalvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp. Kör följande kommandon hello i en upphöjd Azure PowerShell-konsol:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Du kan hämta en lista över alla hello säkerhetskopieringsvalv i en viss prenumeration med hjälp av hello **Get-AzureRMBackupVault** cmdleten igen.

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a>Hello Azure Backup agent installeras på en DPM-Server
Innan du installerar hello Azure Backup-agenten måste toohave hello installer hämtats och finns på hello Windows Server. Du kan hämta hello senaste versionen av hello installer från hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från hello säkerhetskopieringsvalvet instrumentpanelens sida. Spara hello installer tooan lättillgänglig plats som * C:\Downloads\*.

tooinstall hello agent, kör följande kommando i en upphöjd PowerShell-konsol hello **på hello DPM-servern**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Detta installerar hello agent med alla hello standardvärdet. hello installationen tar några minuter i hello bakgrund. Om du inte anger hello */nu* alternativet hello **Windows Update** öppnas hello slutet av hello installation toocheck för alla uppdateringar.

hello agenten visas i hello listan över installerade program. toosee hello listan över installerade program finns för**Kontrollpanelen** > **program** > **program och funktioner**.

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Installationsalternativ
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

### <a name="registering-with-hello-azure-backup-service"></a>Registrera med hello Azure Backup-tjänsten
Innan du kan registrera med hello Azure Backup-tjänsten måste tooensure som hello [krav](backup-azure-dpm-introduction.md) är uppfyllda. Du måste:

* Har ett giltigt Azure-prenumeration
* Har ett säkerhetskopieringsvalv

toodownload hello valvautentiseringsuppgifter som kör hello **Get-AzureBackupVaultCredentials** kommandot i en Azure PowerShell-konsolen och lagra den i en lämplig plats som * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Registrera hello-dator med hello valvet görs med hjälp av hello [Start DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

Det kommer att registrera hello DPM-servern med namnet ”TestingServer” med Microsoft Azure-valvet med hello angetts valvet autentiseringsuppgifter.

> [!IMPORTANT]
> Du inte använda relativa sökvägar toospecify hello valvautentiseringsfilen. Du måste ange en absolut sökväg som en inkommande toohello cmdlet.
>
>

### <a name="initial-configuration-settings"></a>Inledande konfigurationsinställningar
När hello DPM-servern registreras med hello Azure Backup-valvet, startar den med standardinställningar för prenumerationen. Inställningarna prenumeration omfattar nätverk, kryptering och hello mellanlagringsområdet. toobegin ändra hello prenumerationsinställningar toofirst måste få en referens för hello befintliga (standard)-inställningar med hjälp av hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Alla ändringar har gjorts toothis lokala PowerShell-objektet ```$setting``` och sedan hello fullt objekt är allokerat tooDPM och Azure Backup toosave dem med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. Du behöver toouse hello ```–Commit``` flaggan tooensure som hello ändringar sparas. hello-inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Nätverk
Om hello anslutningen för hello DPM datorn toohello Azure Backup-tjänsten på hello internet via en proxyserver, bör hello proxyserverinställningar anges för säkerhetskopieringar toosucceed. Detta görs med hjälp av hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` och hello ```ProxyPassword``` parametrar med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning hello veckodagar. I det här exemplet anger vi inte någon begränsning.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a>Konfigurera hello mellanlagringsområde
hello Azure Backup-agenten körs på hello DPM-servern behöver tillfällig lagring för data som återställs från hello molnet (lokalt mellanlagringsområde). Konfigurera hello mellanlagringsområdet med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och hello ```-StagingAreaPath``` parameter.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

I hello-exemplet ovan, hello mellanlagringsområde ska anges för*C:\StagingArea* i hello PowerShell-objektet ```$setting```. Se till att hello mappen redan finns, annars hello slutliga genomförande av hello prenumerationsinställningar misslyckas.

### <a name="encryption-settings"></a>Krypteringsinställningar
hello säkerhetskopierade data skickas tooAzure säkerhetskopiering är krypterad tooprotect hello sekretess hello. Hej krypteringslösenfrasen har hello ”password” toodecrypt hello data vid hello tiden för återställning. Det är viktigt tookeep denna information säkert och säker när den har angetts.

I hello exemplet nedan hello första kommandot konverterar hello ```passphrase123456789``` tooa säker sträng och tilldelar hello säker toohello variabel med namnet ```$Passphrase```. hello andra kommandot anger hello säker sträng i ```$Passphrase``` som hello lösenord för kryptering av säkerhetskopieringar.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Hålla hello lösenfras information säkert när den har angetts. Du kommer inte att kunna toorestore data från Azure utan lösenfras.
>
>

Nu ska du har gjort alla hello krävs ändringar toohello ```$setting``` objekt. Kom ihåg toocommit hello ändringar.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a>Skydda data tooAzure säkerhetskopiering
I det här avsnittet kan du lägga till en server tooDPM för produktion och skydda hello datalagring toolocal DPM och tooAzure säkerhetskopiering. I hello exemplen visar vi hur tooback av filer och mappar. hello logik kan enkelt att utökade toobackup alla stöd för DPM-datakällor. Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:

1. **Medlemmar** är en lista över alla hello skyddsobjekt (även kallat *datakällor* i DPM) som du vill tooprotect i hello samma skyddsgrupp. Exempelvis kanske tooprotect produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering. Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste toomake att hello DPM-agenten är installerad på hello server och hanteras av DPM. Hello gör för [installerar hello DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länka den toohello lämpliga DPM-servern.
2. **Dataskyddsmetod** anger hello mål platser - band, disk och molnet. I vårt exempel skyddar vi data toohello lokal disk och toohello moln.
3. En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver toobe tas och hur ofta hello data ska synkroniseras mellan hello DPM-servern och hello produktionsservern.
4. En **bevarandeschema** som anger hur länge tooretain hello Återställningspunkter i Azure.

### <a name="creating-a-protection-group"></a>Skapa en skyddsgrupp
Börja med att skapa en ny Skyddsgrupp med hello [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

hello ovan cmdlet skapar en Skyddsgrupp med namnet *ProtectGroup01*. En befintlig skyddsgrupp kan också ändras senare tooadd säkerhetskopiering toohello Azure-molnet. Dock toomake ändringar toohello Skyddsgrupp – nya eller befintliga - vi behöver tooget en referens i en *ändringsbar* objekt med hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a>Att lägga till gruppen medlemmar toohello Skyddsgrupp
Varje DPM-agenten vet hello lista över datakällor på hello-server som den är installerad på. tooadd datasource-toohello Skyddsgrupp hello DPM-agenten måste toofirst skicka en lista över hello datakällor tillbaka toohello DPM-servern. En eller flera datakällor är markerade och tillagda toohello Skyddsgruppen. hello PowerShell steg som krävs för tooget uppnå detta är:

1. Hämta en lista över alla servrar som hanteras av DPM via hello DPM-agenten.
2. Välj en specifik server.
3. Hämta en lista över alla datakällor på hello-servern.
4. Välj en eller flera datakällor och lägga till dem toohello Skyddsgrupp

hello listan över servrar på vilka hello DPM-agenten är installerad och hanteras av hello DPM-servern förvärvas med hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet. I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Hämta nu hello lista över datakällor på ```$server``` med hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet. I det här exemplet vi filtrering för hello volym * D:\* som vi vill tooconfigure för säkerhetskopiering. Datakällan läggs sedan toohello Skyddsgruppen med hjälp av hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet. Kom ihåg toouse hello *modifable* skydd gruppobjekt ```$MPG``` toomake hello tillägg.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Upprepa detta steg så många gånger som krävs, tills du har lagt till alla hello valt datakällor toohello skyddsgruppen. Du kan också starta med en datakälla och fullständig hello arbetsflöde för att skapa hello Skyddsgruppen och senare lägga till flera datakällor toohello Skyddsgruppen.

### <a name="selecting-hello-data-protection-method"></a>Att välja hello dataskyddsmetod
När hello datakällorna har lagts till toohello Skyddsgrupp, hello nästa steg är toospecify hello skyddsmetod med hello [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet. I det här exemplet hello Skyddsgruppen inte inställningar för lokal disk och säkerhetskopiering i molnet. Du måste också toospecify hello datakällan som du vill tooprotect toocloud med hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a>Ange hello Kvarhållningsintervall
Ange hello kvarhållning för hello säkerhetskopiering punkter med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet. När det kan verka udda tooset hello kvarhållning innan hello schemat för säkerhetskopiering har definierats med hello ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras. Det är alltid möjligt tooset hello Säkerhetskopieringsschemat först och hello bevarandeprincip efter.

I hello exemplet nedan anger hello cmdlet hello kvarhållning parametrar för säkerhetskopiering till disk. Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan hello produktionsservern och hello DPM-servern. Hej ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data är kopierade toohello DPM-servern; Detta förhindrar att säkerhetskopiorna blir för stor.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

För säkerhetskopiering ska tooAzure (DPM refererar toothese som onlinesäkerhetskopieringar) hello kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md). Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper. I det här exemplet vi skapa en matris som representerar hello komplexa kvarhållning schema som vi vill och konfigurera hello Kvarhållningsintervall med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a>Ange schemat för säkerhetskopiering av hello
DPM anger en säkerhetskopiering standardschemat automatiskt om du anger hello skyddsmålet med hello ```Set-DPMPolicyObjective``` cmdlet. toochange hello standardscheman används hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av hello [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

I hello-exemplet ovan, ```$onlineSch``` är en matris med fyra element som innehåller hello befintligt onlineskydd schema för hello Skyddsgruppen i hello GFS schemat:

1. ```$onlineSch[0]```innehåller hello dagsschema
2. ```$onlineSch[1]```innehåller hello veckoschema
3. ```$onlineSch[2]```innehåller hello månadsschema
4. ```$onlineSch[3]```innehåller hello årlig schema

Om du behöver toomodify hello veckoschema måste du toorefer toohello ```$onlineSch[1]```.

### <a name="initial-backup"></a>Den första säkerhetskopieringen
När du säkerhetskopierar en datakälla för hello första gången, måste DPM toocreate en första replik som kommer att skapa en kopia av hello datasource toobe skyddas på DPM-replikvolymen. Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av hello [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) -cmdlet med parametern hello ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a>Ändra hello storlek på DPM-replikeringen & återställningspunktvolymen
Du kan också ändra hello storleken på DPM-replikvolymen samt skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet som hello exemplet nedan: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-hello-changes-toohello-protection-group"></a>Genomför hello ändringar toohello Skyddsgrupp
Slutligen måste hello ändringar toobe allokerat innan DPM kan säkerhetskopiera hello per hello ny Skyddsgrupp konfiguration. Detta görs med hjälp av hello [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a>Visa hello säkerhetskopiering punkter
Du kan använda hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget en lista över alla återställningspunkter för en datakälla. I det här exemplet ska du:

* Hämta alla hello PGs på hello DPM-server som kommer att lagras i en matris```$PG```
* Hämta hello datakällor motsvarande toohello```$PG[0]```
* Hämta alla hello Återställningspunkter för en datakälla.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Återställa skyddade data på Azure
Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt. I föregående avsnitt hello vi en lista över hello säkerhetskopiering punkter för en datakälla.

I hello exemplet nedan visar hur toorestore Hyper-V virtuell dator från Azure Backup genom att kombinera säkerhetskopiering punkter med hello mål för återställning. Detta omfattar:

* Skapa ett återställningsalternativ med hello [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.
* Hämtning hello punktmatrisen säkerhetskopiering med hello ```Get-DPMRecoveryPoint``` cmdlet.
* Om du väljer en säkerhetskopiering punkt toorestore från.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

hello kommandona kan enkelt utökas för någon typ av datakälla.

## <a name="next-steps"></a>Nästa steg
* Läs mer om Azure Backup för DPM [introduktion tooDPM säkerhetskopiering](backup-azure-dpm-introduction.md)
