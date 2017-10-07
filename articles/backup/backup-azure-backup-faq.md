---
title: "aaaAzure säkerhetskopiering vanliga frågor och svar | Microsoft Docs"
description: "Svar på toocommon frågor om: Azure Backup-funktioner inklusive återställningstjänster valv, den kan säkerhetskopiera upp, hur det fungerar, kryptering och gränser. "
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering och katastrofåterställning, säkerhetskopieringstjänst"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/21/2017
ms.author: markgal;arunak;trinadhk;
ms.openlocfilehash: 3338f7620bcc6ebf53c9c161191f2d8bca1a29da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-service"></a>Frågor om hello Azure Backup-tjänsten
Den här artikeln innehåller svar toocommon frågor toohelp du snabbt kan förstå hello Azure Backup-komponenter. I vissa hello-svar finns länkar toohello artiklar som har omfattande information. Du kan ställa frågor om Azure Backup genom att klicka på **kommentarer** (toohello till höger). Kommentarerna visas längst ned hello i den här artikeln. Ett Livefyre konto är obligatoriskt toocomment. Du kan också ställa frågor om hello Azure Backup-tjänsten i hello [diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

tooquickly genomsökning hello avsnitt i den här artikeln använder hello länkar toohello höger under **i den här artikeln**.


## <a name="recovery-services-vault"></a>Recovery Services-valv

### <a name="is-there-any-limit-on-hello-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Finns det någon gräns för hello antalet valv som kan skapas i varje Azure-prenumeration? <br/>
Ja. Från och med september 2016 kan du skapa 25 Recovery Services- eller säkerhetskopieringsvalv per prenumeration. Du kan skapa upp too25 återställningstjänster valv per stöds region för Azure Backup per prenumeration. Om du behöver fler valv skapar du ytterligare en prenumeration.

### <a name="are-there-limits-on-hello-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Finns det någon gräns på hello många servrar/datorer som kan registreras mot varje valvet? <br/>
Ja, kan du registrera dig too50 datorer per valvet. För Azure IaaS-virtuella datorer är hello gränsen 200 virtuella datorer per valvet. Om du behöver tooregister flera datorer, kan du skapa ett annat valv.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Min organisation har ett valv. Hur kan jag isolera en servers data från en annan server när jag återställer data?<br/>
Alla servrar som är registrerade toohello samma valvet kan återställa hello data som säkerhetskopieras av andra servrar *som använder hello samma lösenfras*. Om du har servrar vars säkerhetskopierade data du vill tooisolate från andra servrar i din organisation använder en lösenfras som är avsedda för dessa servrar. HR-servrarna kan till exempel använda en krypteringslösenfras, redovisningsservrarna en annan och lagringsservrar en tredje.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Kan jag ”migrera” mina säkerhetskopierade data eller mitt säkerhetskopierade valv mellan prenumerationer? <br/>
Nej. hello valvet har skapats på en prenumerationsnivå och kan inte omtilldelas tooanother prenumeration när den har skapats.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery Services-valv är baserade på resurshanteraren. Stöds säkerhetskopieringsvalv (klassiskt läge) fortfarande? <br/>
Alla befintliga säkerhetskopieringsvalv i hello [klassiska portalen](https://manage.windowsazure.com) fortsätta toobe som stöds. Du kan inte längre använda hello klassiska portal toodeploy nya säkerhetskopieringsvalv. Microsoft rekommenderar att du använder Recovery Services-valv för alla distributioner eftersom framtida förbättringar gäller tooRecovery Services valv, endast. Om du försöker toocreate ett säkerhetskopieringsvalv i hello klassiska portal kommer du att omdirigerade toohello [Azure-portalen](https://portal.azure.com).

### <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Kan jag migrera en Backup-valvet tooa Recovery Services-valvet? <br/>
Tyvärr Nej, kan inte du migrera hello innehållet i en Backup-valvet tooa Recovery Services-valvet. Vi arbetar med att lägga till den här funktionen, men den är inte tillgänglig för närvarande.

### <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Jag har säkerhetskopierat mina klassiska virtuella datorer i ett säkerhetskopieringsvalv. Kan jag migrera Mina virtuella datorer från klassiskt läge tooResource Manager läge och skydda dem i ett Recovery Services-valv?
Klassiska Virtuella återställningspunkter i ett säkerhetskopieringsvalv migrerar inte tooa Recovery Services-valvet automatiskt när du flyttar hello VM från klassiska tooResource Manager-läget. Följ dessa steg tootransfer VM-säkerhetskopieringar:

1. Hello Backup-valvet, gå toohello **skyddade objekt** fliken och markera hello VM. Klicka på [Stoppa skydd](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Lämna alternativet *Ta bort associerade säkerhetskopieringsdata* **avmarkerat**.
2. Ta bort hello säkerhetskopiering/ögonblicksbild tillägg från hello VM.
3. Migrera hello virtuell dator från klassiskt läge tooResource Manager läge. Kontrollera att hello lagring och nätverk information motsvarande toohello virtuell dator är också migreras tooResource Manager-läget.
4. Skapa ett Recovery Services-valvet och konfigurera säkerhetskopiering på hello migrerade virtuella datorn med hjälp av **säkerhetskopiering** åtgärd ovanpå valvet instrumentpanelen. Detaljerad information om säkerhetskopiering av en VM tooa Recovery Services valvet, finns hello artikeln [skydda virtuella Azure-datorer med ett Recovery Services-valv](backup-azure-vms-first-look-arm.md).

## <a name="azure-backup-agent"></a>Azure Backup-agent
En detaljerad lista med frågor finns i [vanliga frågor och svar om säkerhetskopiering av Azure-filmappen](backup-azure-file-folder-backup-faq.md)

## <a name="azure-vm-backup"></a>Säkerhetskopiering av virtuell Azure-dator
En detaljerad lista med frågor finns i avsnittet [Vanliga frågor och svar om säkerhetskopiering av virtuella Azure-datorer](backup-azure-vm-backup-faq.md)

## <a name="back-up-vmware-servers"></a>Säkerhetskopiera VMware-servrar

### <a name="can-i-back-up-vmware-vcenter-servers-tooazure"></a>Kan jag säkerhetskopiera VMware vCenter-servrar tooAzure?

Ja. Du kan använda Azure Backup Server tooback VMware vCenter-och ESXi tooAzure. Information om hello stöds VMware version finns hello artikel [Azure Backup Server protection matrisen](backup-mabs-protection-matrix.md). Stegvisa instruktioner finns [Använd Azure Backup Server tooback in en VMware-server](backup-azure-backup-server-vmware.md).


## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Server och System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-toocreate-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kan jag använda Azure Backup-Server toocreate en Bare Metal Recovery (BMR) säkerhetskopia för en fysisk server? <br/>
Ja.

### <a name="can-i-register-my-dpm-server-toomultiple-vaults-br"></a>Kan jag registrera min toomultiple valv för DPM-servern? <br/>
Nej. En server med DPM eller MABS kan vara registrerade tooonly en valvet.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Vilken version av System Center Data Protection Manager stöds? <br/>
Vi rekommenderar att du installerar hello [senaste](http://aka.ms/azurebackup_agent) Azure Backup-agenten på hello senaste samlade uppdateringen (UR) för System Center Data Protection Manager (DPM). Från och med augusti 2016 är Update Rollup 11 hello senaste uppdateringen.

### <a name="i-have-installed-azure-backup-agent-tooprotect-my-files-and-folders-can-i-now-install-system-center-dpm-toowork-with-azure-backup-agent-tooprotect-on-premises-applicationvm-workloads-tooazure-br"></a>Jag har installerat Azure Backup agent tooprotect Mina filer och mappar. Kan jag nu installera System Center DPM toowork med Azure Backup agent tooprotect lokala program och VM-arbetsbelastningar tooAzure? <br/>
toouse Azure Backup med System Center Data Protection Manager (DPM), installera DPM först och sedan installera Azure Backup-agenten. Installera hello Azure Backup-komponenter i den här ordningen säkerställer hello Azure Backup-agenten fungerar med DPM. Hello Azure Backup agent installeras innan du installerar DPM är inte bäst eller så stöds.


## <a name="how-azure-backup-works"></a>Så här fungerar Azure Backup
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-hello-transferred-backup-data-deleted-br"></a>Om jag avbryter en säkerhetskopiering när den har startat hello överförda säkerhetskopieringsdata raderas? <br/>
Nej. Alla data som överförs till hello valvet innan hello säkerhetskopieringsjobbet avbröts förblir i hello-valvet. Azure Backup använder en kontrollpunkt mekanism toooccasionally lägga till kontrollpunkter toohello säkerhetskopierade data under hello säkerhetskopiering. Eftersom det finns kontrollpunkter i hello säkerhetskopieringsdata Validera hello nästa säkerhetskopieringsprocessen hello integriteten hos hello-filer. hello nästa säkerhetskopieringsjobb kommer att inkrementella toohello data som tidigare har säkerhetskopierat. Inkrementella säkerhetskopieringar Överför bara nya eller ändrade data som är lika toobetter utnyttjande av bandbredd.

Om du avbryter ett säkerhetskopieringsjobb för en virtuella Azure-dator ignoreras alla överförda data. hello nästa säkerhetskopieringsjobb överför inkrementella data från hello senaste lyckade säkerhetskopieringsjobb.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Finns det någon gräns för när eller hur många gånger ett säkerhetskopieringsjobb kan schemaläggas?<br/>
Ja. Du kan köra säkerhetskopieringsjobb på Windows Server eller Windows-arbetsstationer in toothree tider / dag. Du kan köra säkerhetskopieringsjobb på System Center DPM in tootwice per dag. Du kan köra ett säkerhetskopieringsjobb för virtuella IaaS-datorer en gång om dagen. Du kan använda hello schemalägger princip för Windows Server eller Windows-arbetsstation toospecify dagliga och veckovisa scheman. Med System Center DPM kan du definiera dags-, vecko-, månads- och årsscheman.

### <a name="why-is-hello-size-of-hello-data-transferred-toohello-recovery-services-vault-smaller-than-hello-data-i-backed-upbr"></a>Varför är hello storleken på hello data som överförs toohello Recovery Services-valvet mindre än hello data I säkerhetskopierade?<br/>
 Alla hello-data som har säkerhetskopierats från Azure Backup-agenten eller SCDPM eller Azure Backup Server, komprimeras och krypteras innan de överförs. När hello komprimering och kryptering används är hello data i hello säkerhetskopieringsvalvet 30-40% mindre.

## <a name="what-can-i-back-up"></a>Vad kan jag säkerhetskopiera
### <a name="which-operating-systems-do-azure-backup-support-br"></a>Vilka operativsystem stöder Azure Backup? <br/>
Azure Backup stöder följande lista över operativsystem för att säkerhetskopiera hello: filer och mappar och arbetsbelastningen program som skyddas med Azure Backup Server och System Center Data Protection Manager (DPM).

| Operativsystem | Plattform | SKU |
|:--- | --- |:--- |
| Windows 8 och senaste Service Pack |64-bitars |Enterprise, Pro |
| Windows 7 och senaste Service Pack |64-bitars |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 och senaste Service Pack |64-bitars |Enterprise, Pro |
| Windows 10 |64-bitars |Enterprise, Pro, Home |
| Windows Server 2016 |64-bitars |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 och senaste Service Pack |64-bitars |Standard, Datacenter, Foundation |
| Windows Server 2012 och senaste Service Pack |64-bitars |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 och senaste Service Pack |64-bitars |Standard, Workgroup | 
| Windows Storage Server 2012 R2 och senaste Service Pack |64-bitars |Standard, Workgroup |
| Windows Storage Server 2012 och senaste Service Pack |64-bitars |Standard, Workgroup |
| Windows Server 2012 R2 och senaste Service Pack |64-bitars |Essential |
| Windows Server 2008 R2 SP1 |64-bitars |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64-bitars |Standard, Enterprise, Datacenter, Foundation |

**Säkerhetskopiering av virtuell Azure-dator:**

* **Linux**: Azure Backup stöder [en lista över distributioner som godkänts av Azure](../virtual-machines/linux/endorsed-distros.md) med undantag för Core OS Linux.  Andra Bring-Your-äger-Linux-distributioner kan också fungera så länge hello VM agent är tillgänglig på den virtuella datorn hello och stöd för Python finns.
* **Windows Server**: versioner som är äldre än Windows Server 2008 R2 stöds inte.


### <a name="is-there-a-limit-on-hello-size-of-each-data-source-being-backed-up-br"></a>Finns det en gräns för hello storleken för varje datakälla som säkerhetskopieras? <br/>
Det finns ingen gräns på hello mängden data som du kan säkerhetskopiera tooa valvet. Azure-säkerhetskopiering begränsar hello maxstorleken för hello datakällan, men dessa gränser är stora. Från och med augusti 2015 är hello maximala storleken för en datakälla för hello stöds operativsystem:

| Nr | Operativsystem | Största storlek på datakälla |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 eller senare |54 400 GB |
| 2 |Windows 8 eller senare |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1 700 GB |
| 4 |Windows 7 |1 700 GB |

hello i den följande tabellen beskrivs hur varje datakällans storlek bestäms.

| Datakälla | Detaljer |
|:---:|:--- |
| Volym |hello mängden data som säkerhetskopieras från enkel volym på en server eller klient-dator |
| Virtuell Hyper-V-dator |Summan av data för alla hello virtuella hårddiskar för hello virtuella datorn säkerhetskopieras |
| Microsoft SQL Server-databas |Storleken på en enskild SQL-databas som säkerhetskopieras |
| Microsoft SharePoint |Summan av hello innehåll och konfiguration databaser i en SharePoint-servergruppen säkerhetskopieras |
| Microsoft Exchange |Summan av alla Exchange-databaser på en Exchange-server som säkerhetskopieras |
| BMR/systemtillstånd |Varje enskild kopia av BMR eller systemtillstånd för hello datorn säkerhetskopieras |

För Virtuella Azure-säkerhetskopiering, kan varje virtuell dator har in too16 datadiskar med varje data disk som storlek 1 023 GB eller mindre. 

## <a name="retention-policy-and-recovery-points"></a>Bevarandeprincip och återställningspunkter
### <a name="is-there-a-difference-between-hello-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Det är skillnad mellan hello bevarandeprincip för DPM och Windows-servern eller-klienten (det vill säga på Windows Server utan DPM)?<br/>
Ingen, både DPM och Windows Server/Windows-klienten har dagliga, veckovisa, månatliga och årliga bevarandeprinciper.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Kan jag konfigurera mina bevarandeprinciper selektivt, dvs. konfigurera veckovisa och dagliga men inte årliga och månatliga?<br/>
Ja, hello Azure Backup kvarhållning strukturen kan du toohave fullständig flexibilitet för att definiera hello bevarandeprincip enligt dina krav.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Kan jag ”schemalägga en säkerhetskopiering” kl. 18:00 och ange bevarandeprinciper vid en annan tidpunkt?<br/>
Nej. Bevarandeprinciper kan bara användas med säkerhetskopieringspunkter. I följande bild hello, har hello bevarandeprincip angetts för säkerhetskopieringar som utförts på 12: 00 och 18: 00. <br/>

![Schemalägg säkerhetskopiering och kvarhållning](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-toorecover-an-older-data-point-br"></a>Om en säkerhetskopia sparas för en lång tid tar det mer tid toorecover ett äldre datapunkt? <br/>
Hello tid toorecover hello äldsta eller hello senaste punkt är Nej – hello samma. Varje återställningspunkt beter sig som en fullständig punkt.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-hello-total-billable-backup-storagebr"></a>Påverkar den hello totala fakturerbar säkerhetskopieringslagring om varje återställningspunkt som en fullständig plats?<br/>
Typiska produkter för långsiktiga kvarhållningspunkter lagrar säkerhetskopierade data som fullständiga punkter. hello fullständig punkter är lagring *ineffektiv* men är enklare och snabbare toorestore. Inkrementell kopiorna är lagring *effektivt* men kräver toorestore en kedja av data, vilket påverkar tiden för återställning. Azure Backup storage-arkitektur ger du hello bäst hos båda genom att lagra data för snabb återställning och medför låg lagringskostnader optimalt. Den här datalagringsmetoden säkerställer att den in- och utgående bandbredden används effektivt. Både hello tidsperiod data lagring och hello behövs toorecover hello data, behålls tooa minsta. Läs mer om vad som gör [inkrementell säkerhetskopiering](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) effektiv.

### <a name="is-there-a-limit-on-hello-number-of-recovery-points-that-can-be-createdbr"></a>Finns det en gräns på hello antal återställningspunkter som kan skapas?<br/>
Du kan skapa upp too9999 återställningspunkter per skyddad instans. Skyddade instansen är en dator, server (fysiska eller virtuella) eller arbetsbelastning konfigurerad tooback upp data tooAzure. Mer information finns i hello förklaringar av [säkerhetskopiering och kvarhållning](./backup-introduction-to-azure-backup.md#backup-and-retention), och [vad är en skyddad instans](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-recoveries-can-i-perform-on-hello-data-that-is-backed-up-tooazurebr"></a>Hur många återställningar kan utföra på hello data som säkerhetskopieras tooAzure?<br/>
Det finns ingen gräns för hello antalet återställningar från Azure Backup.

### <a name="when-restoring-data-do-i-pay-for-hello-egress-traffic-from-azure-br"></a>När du återställer data betalar jag för hello utgående trafik från Azure? <br/>
Nej. Din återställningar är ledigt och du debiteras inte för hello utgående trafik.

## <a name="azure-backup-encryption"></a>Azure Backup-kryptering
### <a name="is-hello-data-sent-tooazure-encrypted-br"></a>Hello data skickas tooAzure krypteras? <br/>
Ja. Data krypteras hello lokalt SCDPM-server/klient datorn med AES256 och hello data skickas via en säker HTTPS-länk.

### <a name="is-hello-backup-data-on-azure-encrypted-as-wellbr"></a>Är hello säkerhetskopierade data i Azure krypteras samt?<br/>
Ja. hello data skickas tooAzure förblir krypterade (vilande). Microsoft dekrypterar inte hello säkerhetskopierade data när som helst. När du säkerhetskopierar en Azure VM använder Azure Backup kryptering av hello virtuell dator. Till exempel om den virtuella datorn är krypterad med hjälp av Azure Disk Encryption eller någon annan krypteringsteknik, använder Azure Backup den kryptering toosecure dina data.

### <a name="what-is-hello-minimum-length-of-encryption-key-used-tooencrypt-backup-data-br"></a>Vad hello minsta längd på krypteringsnyckel används tooencrypt säkerhetskopierade data? <br/>
hello krypteringsnyckeln ska vara på minst 16 tecken när du använder Azure backup-agenten. Det finns ingen gräns toolength av nycklar som används av Azure KeyVault för virtuella Azure-datorer. 

### <a name="what-happens-if-i-misplace-hello-encryption-key-can-i-recover-hello-data-or-can-microsoft-recover-hello-data-br"></a>Vad händer om jag misplace hello krypteringsnyckeln? Kan jag återställa hello data (eller) kan återställa Microsoft hello data? <br/>
hello nyckel används tooencrypt hello säkerhetskopierade data finns bara lokalt hello kunden. Microsoft har inte en kopia i Azure och har inte någon toohello snabbtangent. Om hello kunden misplaces hello nyckel, återställa inte Microsoft hello säkerhetskopierade data.
