---
title: "aaaUpgrade ett valv tooa återställningstjänster säkerhetskopieringsvalv (förhandsversion) | Microsoft Docs"
description: Anvisningar och support information tooupgrade valvet Azure Backup tooa Recovery Services-valvet.
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/03/2017
ms.author: sogup;markgal;arunak
ms.openlocfilehash: 49062ca4556a009c82f143bb3a60ec71748bed01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-backup-vault-tooa-recovery-services-vault"></a>Uppgradera en Backup-valvet tooa Recovery Services-valvet

Den här artikeln förklarar hur tooupgrade ett säkerhetskopieringsvalv tooa återställningstjänster valvet. hello uppgraderingsprocessen påverkar inte körs säkerhetskopieringsjobb och inga säkerhetskopierade data går förlorade. hello primära orsakerna tooupgrade en Backup-valvet tooa Recovery Services-valvet:
- Alla funktioner i ett säkerhetskopieringsvalv behålls i Recovery Services-valvet.
- Recovery Services-valv har fler funktioner än säkerhetskopieringsvalv, inklusive: bättre säkerhet, integrerad övervakning, snabbare återställningar och återställning på objektnivå.
- Hantera Säkerhetskopiera objekt från en bättre och förenklad portal.
- Nya funktioner gäller endast tooRecovery Services-valv.

## <a name="impact-toooperations-during-upgrade"></a>Påverkan toooperations under uppgradering

När du uppgraderar en Backup-valvet tooa Recovery Services-valvet, påverkas inte tooyour dataåtgärder plan. Alla säkerhetskopieringsjobb Fortsätt som vanligt, och alla aktiva jobb fortsätta utan avbrott. Under uppgraderingen hello hanteringsåtgärder innebära ett kort driftstopp och du kan inte skydda nya objekt eller skapa ad hoc säkerhetskopieringar jobb. Återställningsjobb för IaaS-VM körs inte under hello uppgraderingen. hello valvet uppgraderingen tar under en timme toocomplete. När du är klar ersätter ett Recovery Services-valv hello Backup-valvet.

## <a name="changes-tooyour-automation-and-tool-after-upgrading"></a>Verktyget när du har uppgraderat och ändringar tooyour automation

När du förberedde hello valvet uppgradera infrastrukturen måste du uppdatera din befintliga automation eller tooling tooensure som den fortfarande toowork efter hello uppgradering.
Se hello PowerShell-cmdlets referenser för hello [Service Manager-distributionsmodellen](backup-client-automation-classic.md) och hello [Resource Manager-distributionsmodellen](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Innan du uppgraderar

Kontrollera hello följande problem innan du uppgraderar din Backup-valv tooRecovery Service valv.

- **Minsta agentversion**: tooupgrade ditt valv, se till att hello Microsoft Azure Recovery Services MARS-agenten är minst version 2.0.9070.0. Om hello MARS-agenten är äldre än 2.0.9070.0, uppdatera hello-agenten innan du startar uppgraderingsprocessen hello.
- **Instans-baserade faktureringsmodell som tillämpas**: återställningstjänsten valv stöder endast hello instans-baserade faktureringsmodell som tillämpas. Om du har ett säkerhetskopieringsvalv som använder hello äldre Storage-baserade fakturering modellen konvertera hello faktureringsmodell som tillämpas under uppgraderingen.
- **Inga åtgärder i pågående säkerhetskopieringskonfigurationen**: under uppgraderingen åtkomst toohello management plan är begränsad. Slutför alla hanteringsåtgärder plan och starta sedan hello uppgraderingen.

## <a name="using-powershell-scripts-tooupgrade-your-vaults"></a>Med hjälp av PowerShell-skript tooupgrade ditt valv

Du kan använda PowerShell-skript tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv. Kontrollera att du har hello nödvändig PowerShell komponenter tootrigger hello valvet uppgradering. PowerShell-skript för säkerhetskopieringsvalv fungerar inte för Recovery Services-valv. Förbereda din miljö tooupgrade hello valv:

1. Installera eller uppgradera [Windows Management Framework (WMF) tooversion 5](https://www.microsoft.com/download/details.aspx?id=50395) eller senare.
2. [Installera Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Hämta hello [PowerShell-skript](https://aka.ms/vaultupgradescript2) tooupgrade ditt valv.

### <a name="run-hello-powershell-script"></a>Kör hello PowerShell-skript

Använd hello följande skript tooupgrade ditt valv. Följande exempelskript hello har förklaringar av hello parametrar.

RecoveryServicesVaultUpgrade 1.0.2.ps1 **- SubscriptionID** `<subscriptionID>` **- VaultName** `<vaultname>` **-plats** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName**`<rgname>`

**SubscriptionID** -hello prenumeration ID-numret hello valvet som uppgraderas.<br/>
**VaultName** - hello namn på hello Backup-valvet som uppgraderas.<br/>
**Plats** -platsen för hello valvet som uppgraderas.<br/>
**ResourceType** -Använd BackupVault.<br/>
**TargetResourceGroupName** - eftersom du uppgraderar hello valvet tooa Resource Manager-baserade distribution, ange en resursgrupp. Du kan använda en befintlig resursgrupp eller skapa en genom att ange ett nytt namn. Om du skriver fel hello namnet på en resursgrupp kan du skapa en ny resursgrupp. toolearn mer om resursgrupper, Läs [översikt över resursgrupper](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Namn för resursgruppen har begränsningar. Vara säker på att toofollow hello vägledning. fel toodo så kan det orsaka valvet uppgraderingar toofail.
>
>

hello är följande kodavsnitt ett exempel på vad din PowerShell-kommandot ska se ut som:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

Du kan också köra skript hello utan några parametrar och du uppmanas tooprovide indata för alla obligatoriska parametrar.

hello PowerShell-skript uppmanas du tooenter dina autentiseringsuppgifter. Ange dina autentiseringsuppgifter två gånger: en gång för hello Service Manager-konto och en gång för hello Resource Manager-konto.

### <a name="pre-requisites-checking"></a>Kontroll av förutsättningar
När du har angett dina autentiseringsuppgifter för Azure, kontrollerar Azure att din miljö uppfyller hello följande krav:

- **Minsta agentversion** -uppgradering valv tooRecovery Services säkerhetskopieringsvalv kräver hello MARS-agenten toobe minst version 2.0.9070. Om du har artiklar registrerade tooa Backup-valvet med en agent före 2.0.9070, hello misslyckas. Om hello kravkontrollen misslyckas, uppdatera hello-agenten och försök tooupgrade hello valvet igen. Du kan hämta hello senaste versionen av hello agent från [http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe](http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Pågående configuration jobb**: om någon konfigurerar jobbet för ett säkerhetskopieringsvalv som toobe uppgraderas eller registrera ett objekt, hello kravkontrollen misslyckas. Slutför konfiguration av hello, eller avsluta registrering hello-objektet och starta sedan hello valvet uppgraderingsprocessen.
- **Storage-baserade faktureringsmodell som tillämpas**: återställningstjänster valv stöd hello instans-baserade faktureringsmodell som tillämpas. Om du kör hello valvet uppgraderingen på ett säkerhetskopieringsvalv som använder hello Storage-baserade faktureringsmodell som tillämpas, är du tillfrågas tooupgrade faktureringsmodellen tillsammans med hello valvet. Annars kan du uppdatera faktureringsmodellen först och sedan uppgraderar hello-valvet.
- Identifiera en resursgrupp för hello Recovery Services-valvet. tootake nytta av Hej funktioner för distribution av Resource Manager, måste du placera ett Recovery Services-valv i en resursgrupp. Om du inte vet vilken resursgrupp toouse, ange ett namn och en hello uppgraderingsprocessen skapas hello resursgruppen. hello uppgraderingsprocessen associerar även hello valvet med hello ny resursgrupp.

När hello uppgraderingen är klar kontrollerar hello förutsättningar uppmanas hello du toostart hello valvet uppgraderingen. När du har bekräftat ta hello uppgraderingsprocessen brukar runt 15 20 minuter toocomplete, beroende på hello storleken på ditt valv. Om du har ett stort valv kan uppgraderingen ta upp too90 minuter.

## <a name="managing-your-recovery-services-vaults"></a>Hantera Recovery Services-valv

hello följande skärmar visas ett nytt Recovery Services-valv som uppgraderats från säkerhetskopieringsvalvet i hello Azure-portalen. hello första skärmen visar hello valvet instrumentpanelen som visar viktiga entiteter för hello-valvet.

![exempel på Recovery Services-valv som uppgraderats från ett säkerhetskopieringsvalv](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

andra hello-skärmen visar hello hjälplänkar tillgängliga toohelp du komma igång med hello Recovery Services-valvet.

![hjälplänkar i hello Snabbstart-bladet](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Åtgärder efter uppgradering
Recovery Services-ventilen har stöd för att ange Tidszonsinformationen i princip för säkerhetskopiering. När valvet har uppgraderats, gå tooBackup principer från valvet inställningsmenyn och uppdatera hello tidszonsinformation för varje hello principer som konfigurerats i hello-valvet. Den här skärmen visar redan hello Säkerhetskopieringsschemat tid som angetts som per lokal tidszon som används när du har skapat principen. 

## <a name="enhanced-security"></a>Förbättrad säkerhet

När ett säkerhetskopieringsvalv uppgraderas tooa Recovery Services-valvet, hello säkerhetsinställningarna för det valvet aktiveras automatiskt. När hello säkerhetsinställningar som finns på vissa åtgärder, till exempel ta bort säkerhetskopior eller ändra en lösenfras kräva en [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) PIN-kod. Mer information om hello förbättrad säkerhet finns i hello artikel [säkerhetsfunktioner tooprotect hybrid säkerhetskopieringar](backup-azure-security-feature.md). 

När hello förbättrad säkerhet är aktiverad bevaras data in too14 dagar efter hello information om återställning har tagits bort från hello-valvet. Kunder som debiteras för lagring av den här säkerhetsdata. Säkerhet datalagring gäller toorecovery punkter för hello Azure Backup-agenten, Azure Backup Server och System Center Data Protection Manager. 

## <a name="gather-data-on-your-vault"></a>Samla in data på ditt valv

När du uppgraderar tooa Recovery Services-valvet, konfigurera rapporter för Azure Backup (för IaaS-VM och Microsoft Azure Recovery Services MARS-) och Använd Power BI tooaccess hello-rapporter. Mer information om att samla in data finns hello artikeln [konfigurera Azure Backup rapporterar](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**Påverkar hello uppgraderingsplanen mina pågående säkerhetskopior?**</br>
Nej. Säkerhetskopiorna pågående fortsätta utan avbrott under och efter uppgraderingen.

**Om jag inte uppgradera snart kan händer toomy valv?**</br>
Eftersom alla nya funktioner gäller endast tooRecovery Services-valv, bör du tooupgrade ditt valv. Microsoft kommer så småningom inaktualisera hello klassiska portalen. Startar den 1 September 2017 kommer Microsoft att börja uppgradera automatiskt säkerhetskopieringsvalv tooRecovery Services-valv. Med den 1 November 2017 kommer Microsoft att slutföra hello uppgraderingsprocessen. Ditt valv kan uppgraderas automatiskt under September eller oktober. Microsoft rekommenderar att du uppgraderar ditt valv så snart som möjligt.

**Vad betyder det här uppgradering för Mina befintliga verktygsuppsättning?**</br>
Uppdatera din verktygsuppsättning toohello Resource Manager-modellen. Recovery Services-valv skapades för använder i hello Resource Manager-distributionsmodellen. Planera för hello Resource Manager-modellen och för hello skillnad i ditt valv är viktigt. 

**Under uppgraderingen hello är det mycket driftavbrott?**</br>
Det beror på hello antal resurser som ska uppgraderas. För mindre distributioner (några tiotal skyddade instanser), bör hello hela uppgraderingen ta mindre än 20 minuter. För större distributioner bör ta högst en timme.

**Kan jag återställa efter uppgraderingen?**</br>
Nej. Återställning stöds inte när hello resurser har uppgraderats.

**Kan jag verifiera min prenumeration eller resurser toosee om de är kompatibelt med uppgraderingen?**</br>
Ja. hello första steget i uppgraderingen validerar att hello resurser av uppgraderingen. Om det inte går att hello verifiering av krav, kan du få meddelanden för alla hello orsaker hello uppgraderingen inte kan slutföras.

**Vilka behörigheter som ska jag har tootrigger valvet uppgraderingen?**</br>
tooperform hello valvet uppgraderingen, du måste läggas till som medadministratör för prenumerationen hello i hello klassiska Azure-portalen. Detta krävs även om du redan anges som ägare i hello Azure-portalen. Försök tooadd en medadministratör för hello-prenumeration i Azure klassiska portal toofind ut om du är medadministratör för hello prenumeration. Om du inte kan tooadd en medadministratör kontakta en administratör eller medadministratör för hello-prenumeration som du kan lägga till dig som medadministratör.

**Kan jag uppgradera min CSP-baserade Backup-valvet?**</br>
Nej. För närvarande kan uppgradera du inte CSP-baserade säkerhetskopieringsvalv. Vi lägger till stöd för uppgradering av CSP-baserade säkerhetskopieringsvalv i nästa hello-versioner.

**Kan jag visa min klassiska valvet efter uppgradering?**</br>
Nej. Du kan inte visa eller hantera din klassiska valvet efter uppgradering. Du kan bara kan toouse hello nya Azure-portalen för alla hanteringsåtgärder på hello-valvet.

**Min uppgraderingen misslyckades, men hello-dator som lagras hello agenten kräver uppdatering är inte finns längre. Vad gör jag i så fall?**</br>
Om du behöver toouse hello store hello säkerhetskopior av den här datorn för långsiktig kvarhållning och sedan inte kan tooupgrade hello valvet. I kommande versioner vi lägga till stöd för uppgradering av sådana valvet.
Om du inte behöver toostore hello säkerhetskopior av den här datorn längre och sedan kontrollera att avregistrera den här datorn från valvet hello och försök hello uppgraderingen.

**Varför visas inte hello jobb information för min lokala resurser efter uppgraderingen**</br>
Övervakning för en lokal säkerhetskopiering (MARS agent DPM och Azure Backup Server) är en ny funktion som du får när du uppgraderar din Backup-valvet tooRecovery Services-valvet. hello övervakningsinformation upptar too12 timmar toosync med hello-tjänsten.

**Hur rapporterar problem?**</br>
Om någon del av hello valvet uppgradera misslyckas Obs hello åtgärds-ID som anges i hello-fel. Microsoft-supporten fungerar proaktivt tooresolve hello problemet. Du kan nå ut tooSupport eller skicka e-post på rsvaultupgrade@service.microsoft.com med prenumerations-ID, valvnamnet och åtgärds-ID. Vi försöker tooresolve hello problemet så snabbt som möjligt. Försök inte hello igen såvida inte uttryckligen har angett toodo så av Microsoft.


## <a name="next-steps"></a>Nästa steg
Använd följande artikel till hello:</br>
[Säkerhetskopiera en IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[Säkerhetskopiera en Azure Backup-Server](backup-azure-microsoft-azure-backup.md)</br>
[Säkerhetskopiera Windows Server](backup-configure-vault.md).
