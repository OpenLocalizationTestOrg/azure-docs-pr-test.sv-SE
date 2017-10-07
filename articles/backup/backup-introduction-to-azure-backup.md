---
title: "aaaWhat är Azure Backup? | Microsoft Docs"
description: "Använd Azure Backup tooback och återställa data och arbetsbelastningar från Windows-servrar, Windows-arbetsstationer, System Center DPM-servrar och virtuella Azure-datorer."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering och återställning, återställningstjänster, lösningar för säkerhetskopiering"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/11/2017
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 953a19600f67a6b7451f71b1e3234d913816d18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-features-in-azure-backup"></a>Översikt över hello funktioner i Azure Backup
Azure Backup är hello Azure-baserad tjänst som du kan använda tooback (eller skydda) och återställa data i hello Microsoft cloud. Azure Backup ersätter din befintliga lokala eller externa säkerhetskopieringslösning med en tillförlitlig och säker molnbaserad lösning med ett konkurrenskraftigt pris. Azure Backup erbjuder flera komponenter som du kan hämta och distribuera på hello lämplig dator, server, eller i hello molnet. hello komponenten eller en agent som du distribuerar beror på vad du vill tooprotect. Alla Azure Backup-komponenter (oavsett om du skyddar data lokalt eller i molnet hello) kan vara används tooback upp data tooa Recovery Services-valvet i Azure. Se hello [Azure Backup komponenter tabell](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (senare i den här artikeln) för information om vilka komponenten toouse tooprotect specifika data, program eller arbetsbelastningar.

[Titta på en videoöversikt över Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Varför ska jag använda Azure Backup?
Traditionella lösningar för säkerhetskopiering har utvecklats tootreat hello moln som en slutpunkt eller statisk lagringsplats liknande toodisks eller band. När den här metoden är enkel är begränsat och dra inte full nytta av en underliggande molnplattform som översätter tooan dyra, ineffektiv lösning. Andra lösningar är billigare eftersom hamnar du betalar för hello fel typ av lagring eller lagring som du inte behöver. Andra lösningar är ofta ineffektiv eftersom de inte tillhandahåller hello typ eller mängden lagringsutrymme som du behöver eller administrativa uppgifter kräver att för mycket tid. Däremot ger Azure Backup följande viktiga fördelar:

**Automatisk lagringshantering** – hybridmiljöer kräver ofta heterogen lagring - vissa lokalt och vissa i hello molnet. Med Azure Backup är det kostnadsfritt att använda lokala lagringsenheter. Azure Backup allokerar och hanterar lagringen av säkerhetskopiorna automatiskt och tillämpar en modell där du betalar baserat på din användning. Lön-som-du-Använd innebär att du betalar bara för hello lagring som du använder. Mer information finns i hello [Azure priser artikel](https://azure.microsoft.com/pricing/details/backup).

**Obegränsade skalning** – Azure Backup använder hello underliggande power och obegränsade skalan för hello Azure cloud toodeliver hög tillgänglighet – med något underhåll eller övervakning kostnader. Du kan ställa in aviseringar tooprovide information om händelser, men du behöver inte tooworry om hög tillgänglighet för dina data i hello molnet.

**Flera lagringsalternativ** – en aspekt av hög tillgänglighet är lagringsreplikering. Azure Backup erbjuder två typer av replikering: [lokalt redundant lagring](../storage/common/storage-redundancy.md#locally-redundant-storage) och [geo-redundant lagring](../storage/common/storage-redundancy.md#geo-redundant-storage). Välj hello säkerhetskopieringslagring alternativ baserat på behov:

* Lokalt redundant lagring (LRS) replikerar data tre gånger (skapas tre kopior av dina data) i en parad datacenter i hello samma region. LRS är ett billigt alternativ för att skydda dina data mot fel i den lokala maskinvaran.

* GEO-redundant lagring (GRS) replikeras dina data tooa sekundär region (hundratals mil bort från hello primär plats hello källdata). GRS kostar mer än LRS, men GRS ger högre hållbarhet för dina data, även i händelse av ett regionalt avbrott.

**Obegränsade dataöverföring** – Azure Backup begränsar inte hello mängden inkommande eller utgående du dataöverföring. Azure-säkerhetskopiering också debitera inte för hello data som överförs. Men om du använder hello Azure Import/Export service tooimport stora mängder data, finns en kostnad som är kopplade till inkommande data. Mer information om kostnaden finns i [Offline-backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Arbetsflöde för säkerhetskopiering offline i Azure Backup). Utgående data refererar toodata som överförts från ett Recovery Services-valv under en återställning.

**Datakryptering** -kryptering av Data som kan användas för säker överföring och lagring av data i hello offentliga molnet. Du lagrar hello krypteringslösenfrasen lokalt och det aldrig överförs eller lagras i Azure. Om det är nödvändigt toorestore alla data hello, bara du har krypteringslösenfrasen eller nyckel.

**Programkonsekvent säkerhetskopiering** -om säkerhetskopiering av en filserver, en virtuell dator eller en SQL-databas, behövs tooknow att en återställningspunkt har alla nödvändiga data toorestore hello-säkerhetskopia. Azure Backup är programkonsekvent säkerhetskopiering, vilket garanteras ytterligare korrigeringar inte är nödvändiga toorestore hello data. Återställer konsekvent programdata minskar hello återställningen tid, så att du tooquickly returnerade tooa körs.

**Långsiktig kvarhållning** -i stället för att växla säkerhetskopior från disk tootape och flytta hello band tooan ställe på annan plats kan du använda Azure för kortsiktig och långsiktig kvarhållning. Azure begränsar inte hello tiden data förblir i en säkerhetskopiering eller Recovery Services-valvet. Du kan förvara data i ett valv hur länge du vill. Azure Backup har en gräns på 9999 återställningspunkter per skyddad instans. Se hello [säkerhetskopiering och kvarhållning](backup-introduction-to-azure-backup.md#backup-and-retention) i den här artikeln för en förklaring av hur den här gränsen kan påverka dina säkerhetskopieringsbehov.  

## <a name="which-azure-backup-components-should-i-use"></a>Vilka Azure Backup-komponenter ska jag använda?
Om du inte vet vilken Azure Backup-komponent fungerar för dina behov, se hello i den följande tabellen information om vad du kan skydda med varje komponent. hello Azure-portalen innehåller en guide som är inbyggd i hello portal, tooguide dig genom att välja hello komponenten toodownload och distribuera. hello guiden, som är en del av hello Recovery Services-valvet skapande, leder dig genom hello steg för att välja ett mål för säkerhetskopiering och välja hello tooprotect för data och program.

| Komponent | Fördelar | Begränsningar | Vad skyddas? | Var lagras säkerhetskopiorna? |
| --- | --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<li>Säkerhetskopiera filer och mappar på en fysisk eller virtuell dator med Windows OS (virtuella datorer kan finnas lokalt eller i Azure)<li>Ingen separat säkerhetskopieringsserver krävs. |<li>Säkerhetskopiera 3 gånger per dag <li>Inte programmedveten, endast återställning på fil-/mapp-/volymnivå, <li>  Inget stöd för Linux. |<li>Filer, <li>Mappar |Recovery Services-valv |
| System Center DPM |<li>Programmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när tootake säkerhetskopieringar<li>Återställningsprecision (allt)<li>Kan använda Recovery Services-valv<li>Linux-stöd på Hyper-V- och VMware-baserade virtuella datorer <li>Säkerhetskopiera och återställ virtuella VMware-datorer med DPM 2012 R2 |Det går inte att säkerhetskopiera Oracle-arbetsbelastningar.|<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Recovery Services-valv,<li> Lokalt ansluten disk,<li>  Band (endast lokalt) |
| Azure Backup Server |<li>Appmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när tootake säkerhetskopieringar<li>Återställningsprecision (allt)<li>Kan använda Recovery Services-valv<li>Linux-stöd på Hyper-V- och VMware-baserade virtuella datorer<li>Säkerhetskopiera och återställ virtuella VMware-datorer <li>Kräver inte en System Center-licens |<li>Det går inte att säkerhetskopiera Oracle-arbetsbelastningar.<li>Kräver alltid en aktiv Azure-prenumeration<li>Inget stöd för säkerhetskopiering på band |<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Recovery Services-valv,<li> Lokalt ansluten disk |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<li>Interna säkerhetskopieringar för Windows/Linux<li>Ingen specifik agentinstallation krävs<li>Säkerhetskopiering på infrastrukturnivå utan behov av en infrastruktur för säkerhetskopiering |<li>Säkerhetskopiera virtuella datorer en gång om dagen <li>Återställ virtuella datorer endast på disknivå<li>Det går inte att säkerhetskopiera lokalt |<li>Virtuella datorer, <li>Alla diskar (med PowerShell) |<p>Recovery Services-valv</p> |

## <a name="what-are-hello-deployment-scenarios-for-each-component"></a>Vad är hello distributionsscenarier för varje komponent?
| Komponent | Kan den distribueras i Azure? | Kan den distribuerade lokalt? | Mållagring som stöds |
| --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<p>**Ja**</p> <p>hello Azure Backup-agenten kan distribueras på alla Windows Server-VM som körs i Azure.</p> |<p>**Ja**</p> <p>hello Backup-agenten kan distribueras på en Windows Server-VM eller fysiska datorn.</p> |<p>Recovery Services-valv</p> |
| System Center DPM |<p>**Ja**</p><p>Lär dig mer om [hur tooprotect arbetsbelastningar i Azure med hjälp av System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur tooprotect arbetsbelastningar och virtuella datorer i ditt datacenter](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokalt ansluten disk,</p> <p>Recovery Services-valv,</p> <p>band (endast lokalt)</p> |
| Azure Backup Server |<p>**Ja**</p><p>Lär dig mer om [hur tooprotect arbetsbelastningar i Azure med hjälp av Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur tooprotect arbetsbelastningar i Azure med hjälp av Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokalt ansluten disk,</p> <p>Recovery Services-valv</p> |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<p>**Ja**</p><p>En del av Azure-infrastrukturen</p><p>Specialiserad för [säkerhetskopiering av virtuella Iaas-datorer (Infrastructure as a Service) i Azure](backup-azure-vms-introduction.md).</p> |<p>**Nej**</p> <p>Använd System Center DPM tooback av virtuella datorer i ditt datacenter.</p> |<p>Recovery Services-valv</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Vilka program och arbetsbelastningar kan säkerhetskopieras?
hello följande tabell innehåller en matris med hello data och arbetsbelastningar som kan skyddas med Azure Backup. hello Azure Backup lösning kolumnen har länkar toohello dokumentationen för lösningen. Varje komponent i Azure Backup kan distribueras i en klassisk (Service Manager-distribuering) eller Resource Manager-modellmiljö för distribuering.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Data eller arbetsbelastning | Källmiljö | Azure Backup-lösning |
| --- | --- | --- |
| Filer och mappar |Windows Server |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Filer och mappar |Windows-dator |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Virtuell Hyper-V-dator (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Virtuell Hyper-V-dator (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ hello Azure Backup-agenten)</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (inklusive hello Azure Backup-agenten)</p> |
| Virtuella IaaS-datorer i Azure (Windows) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |
| Virtuella IaaS-datorer i Azure (Linux) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux-support
hello visar följande tabell hello Azure Backup-komponenter som har stöd för Linux.  

| Komponent | Linux-stöd (Azure-godkänt) |
| --- | --- |
| Azure Backup-agent (MARS) |Nej (endast Windows-baserad agent) |
| System Center DPM |<li> Filkonsekvent säkerhetskopiering av virtuella Linux-gästdatorer på Hyper-V och VMWare<br/> <li> Återställning av virtuella Linux-gästdatorer på Hyper-V och VMWare </br> </br>  *Filkonsekvent säkerhetskopiering är inte tillgängligt för Azure VM* <br/> |
| Azure Backup Server |<li>Filkonsekvent säkerhetskopiering av virtuella Linux-gästdatorer på Hyper-V och VMWare<br/> <li> Återställning av virtuella Linux-gästdatorer på Hyper-V och VMWare </br></br> *Filkonsekvent säkerhetskopiering är inte tillgängligt för Azure VM*  |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |Programkonsekvent säkerhetskopiering med [ramverk för förskript och efterskript](backup-azure-linux-app-consistent.md)<br/> [Detaljerad filåterställning](backup-azure-restore-files-from-vm.md)<br/> [Återställ alla diskar på virtuella datorer](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [Återställning av virtuella datorer](backup-azure-arm-restore-vms.md#create-a-new-vm-from-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Använd virtuella Premium Storage-datorer med Azure Backup
Azure Backup skyddar virtuella datorer i Premium Storage. Azure Premium Storage är SSD-enhet (SSD)-baserad lagring som utformats för toosupport I/O-intensiva arbetsbelastningar. Premium Storage är attraktivt för arbetsbelastningar för virtuella datorer. Mer information om Premium-lagring finns hello artikeln [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../storage/common/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Säkerhetskopiera virtuella datorer i Premium Storage
När du säkerhetskopierar virtuella datorer Premium-lagring, hello-tjänsten för säkerhetskopiering skapar en tillfällig mellanlagring plats, med namnet ”AzureBackup-”, i hello Premium Storage-konto. hello är hello mellanlagringsplatsen lika toohello storleken på hello ögonblicksbild för återställning punkt. Glöm inte hello Premium Storage-konto har tillräckligt med ledigt utrymme tooaccommodate hello tillfällig mellanlagring plats. Mer information finns i artikeln hello [premium storage begränsningar](../storage/common/storage-premium-storage.md#scalability-and-performance-targets). När hello säkerhetskopieringsjobbet är klar raderas hello mellanlagringsplatsen. hello priset för lagringsutrymme som används för hello mellanlagringsplatsen är konsekventa med alla [Premium storage-priser](../storage/common/storage-premium-storage.md#pricing-and-billing).

> [!NOTE]
> Ändra eller inte redigera hello mellanlagringsplatsen.
>
>

### <a name="restore-premium-storage-vms"></a>Återställa virtuella datorer i Premium Storage
Premium-lagring virtuella datorer kan vara återställda tooeither Premium-lagring eller toonormal lagringsutrymme. Återställa en bakre tooPremium för Premium-lagring VM recovery punkt på lagring är hello typisk process för återställningen. Det kan dock vara kostnadseffektivt toorestore en Premium Storage VM recovery punkt toostandard lagring. Den här typen av återställningen kan användas om du behöver en delmängd av filer från hello VM.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Använda virtuella datorer med hanterade diskar med Azure Backup
Azure Backup skyddar virtuella datorer med hanterade diskar. Om du använder hanterade diskar behöver du inte hantera lagringskonton för virtuella datorer och det blir mycket enklare att etablera virtuella datorer.

### <a name="back-up-managed-disk-vms"></a>Säkerhetskopiera virtuella datorer med hanterade diskar
Säkerhetskopieringen av virtuella datorer på hanterade diskar fungerar på samma sätt som säkerhetskopieringen av virtuella datorer med Resource Manager. Du kan konfigurera hello säkerhetskopieringsjobbet direkt från hello virtuella vyn eller från hello återställningstjänster valvet vyn i hello Azure-portalen. Du kan säkerhetskopiera virtuella datorer på hanterade diskar via RestorePoint-samlingar som är byggda ovanpå hanterade diskar. Azure Backup stöder också säkerhetskopiering av virtuella datorer på hanterade diskar som krypterats med ADE (Azure Disk Encryption).

### <a name="restore-managed-disk-vms"></a>Återställa virtuella datorer med hanterade diskar
Azure Backup kan du toorestore en fullständig virtuell dator med hanterade diskar eller Återställ hanterade diskar tooa storage-konto. Azure hanterar hello hanterade diskar under hello återställningsprocessen. Du (kunden hello) hantera hello-lagringskonto som skapats som en del av hello återställningsprocessen. När du återställer hanteras krypterade VMs, ska hello Virtuella datorns nycklar och hemligheter finnas i hello nyckelvalv tidigare toostarting hello återställningen igen.

## <a name="what-are-hello-features-of-each-backup-component"></a>Vad är hello funktioner för varje komponent för säkerhetskopia?
hello följande avsnitt innehåller tabeller som summerar hello tillgänglighet och stöd för olika funktioner i varje Azure Backup-komponent. Se hello information efter varje tabell för ytterligare support eller information.

### <a name="storage"></a>Lagring
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Recovery Services-valv |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Disklagring | |![Ja][green] |![Ja][green] | |
| Bandlagring | |![Ja][green] | | |
| Komprimering <br/>(i Recovery Services-valv) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Inkrementell säkerhetskopiering |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Diskdeduplicering | |![Delvis][yellow] |![Delvis][yellow] | | |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key.png)

hello Recovery Services-valvet är hello prioriterade lagringsenheterna för alla komponenter. System Center DPM och Azure Backup Server dessutom hello alternativet toohave en lokal diskkopia. Endast System Center DPM tillhandahåller dock hello alternativet toowrite data tooa band lagringsenhet.

#### <a name="compression"></a>Komprimering
Säkerhetskopior komprimeras tooreduce hello krävs lagringsutrymme. hello endast komponenten som inte använder komprimering är hello VM-tillägget. hello VM-tillägget kopior alla säkerhetskopierade data från din lagring konto toohello Recovery Services-valvet i hello samma region. Ingen komprimering används när du överför hello data. Dataöverföringen hello utan komprimering något blåses upp hello lagringsutrymme som används. Lagra hello data utan komprimering ger snabbare återställning, bör du behöver dock denna återställningspunkt.


#### <a name="disk-deduplication"></a>Diskdeduplicering
Du kan dra nytta av datadeduplicering när du distribuerar System Center DPM eller Azure Backup Server [på en virtuell Hyper-V-dator](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server utför datadeduplicering (hello nivå för värddatorns) på virtuella hårddiskar (VHD) som är bifogade toohello virtuell dator som lagring för säkerhetskopiering.

> [!NOTE]
> Deduplicering är inte tillgängligt i Azure för någon Backup-komponenter. När System Center DPM och Backup Server distribueras i Azure, koppla hello lagringsdiskar toohello VM inte ska dedupliceras.
>
>

### <a name="incremental-backup-explained"></a>Förklaring av inkrementell säkerhetskopiering
Varje Azure Backup-komponenten stöder inkrementell säkerhetskopiering oavsett hello mållagring (disk, band, Recovery Services-valvet). Inkrementell säkerhetskopiering gör säkerhetskopior lagringsutrymme och tid effektivt, genom att överföra dessa ändringar som gjorts sedan den senaste säkerhetskopieringen av hello.

#### <a name="comparing-full-differential-and-incremental-backup"></a>Jämföra fullständig, differentiell och inkrementell säkerhetskopiering

Förbrukning av lagringsutrymme, mål för återställningstid (RTO) och nätverksförbrukning varierar beroende på metod för säkerhetskopiering. tookeep hello säkerhetskopiering totala ägandekostnader (TCO) ned, behöver du toounderstand hur toochoose hello som bästa lösning för säkerhetskopiering. Följande bild hello jämför fullständig säkerhetskopiering och differentiell säkerhetskopiering inkrementell säkerhetskopiering. Hello bild blockerar datakälla A består av 10 lagring A1 A10 som säkerhetskopieras varje månad. A2 block, A3, A4 och A9 ändra i hello första månad och blockera A5 ändringar i hello nästa månad.

![bild som visar jämförelser av metoder för säkerhetskopiering](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

Med **fullständig säkerhetskopiering**, varje säkerhetskopia innehåller hello hela datakällan. Fullständig säkerhetskopiering förbrukar en stor mängd av nätverkets bandbredd och lagring varje gång en säkerhetskopia överförs.

**Differentiella säkerhetskopian** lagrar bara hello blockerar som ändrats sedan hello initial fullständig säkerhetskopiering, vilket resulterar i en mindre mängd förbrukning av nätverk och lagring. Differentiella säkerhetskopieringar lagrar inte redundanta kopior av oförändrade data. Men eftersom hello datablock som ändras mellan efterföljande säkerhetskopieringar överförs och lagras, är differentiella säkerhetskopieringar ineffektiv. I hello säkerhetskopieras andra månaden ändrade block A2, A3, A4 och A9. I hello säkerhetskopieras tredje månad rutorna samma igen, tillsammans med ändrade block A5. hello ändrade block fortsätta toobe säkerhetskopieras förrän hello nästa fullständig säkerhetskopiering sker.

**Inkrementell säkerhetskopiering** uppnår effektivitet för lagring och nätverk genom att lagra endast hello datablock som ändrats sedan tidigare hello-säkerhetskopiering. Med inkrementell säkerhetskopiering behövs det ingen tootake regelbunden fullständig säkerhetskopiering. I exemplet hello när hello fullständig säkerhetskopiering utförs för hello första månad ändrade block A2, A3, A4 och A9 markeras som ändras och överförs för hello andra månaden. I hello ändrade tredje månad endast block A5 är markerad och överförs. Förflyttning av mindre datamängder sparar lagringsutrymme och nätverksresurser, vilket sänker den totala ägandekostnaden.   

### <a name="security"></a>Säkerhet
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkssäkerhet<br/> (tooAzure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |
| Datasäkerhet<br/> (i Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Nätverkssäkerhet
All säkerhetskopiering trafik från dina servrar toohello Recovery Services-valvet krypteras med Advanced Encryption Standard 256. hello säkerhetskopierade data skickas via en säker HTTPS-länk. hello säkerhetskopierade data lagras också i hello Recovery Services-valvet i krypterad form. Endast du hello Azure kunden har hello lösenfras toounlock dessa data. Microsoft kan inte dekryptera hello säkerhetskopierade data när som helst.

> [!WARNING]
> När du har skapat hello Recovery Services-valvet endast har du åtkomst toohello krypteringsnyckeln. Microsoft aldrig upprätthåller en kopia av krypteringsnyckeln och har inte åtkomst toohello nyckel. Om hello nyckel är felplacerad kan inte Microsoft återställa hello säkerhetskopierade data.
>
>

#### <a name="data-security"></a>Datasäkerhet
Säkerhetskopiering av virtuella Azure-datorer kräver att kryptering *inom* hello virtuell dator. Använd BitLocker på virtuella Windows-datorer och **dm crypt** på virtuella Linux-datorer. Azure Backup krypterar inte automatiskt säkerhetskopieringsdata som finns på den här sökvägen.

### <a name="network"></a>Nätverk
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkskomprimering <br/>(för**reservserver**) | |![Ja][green] |![Ja][green] | |
| Nätverkskomprimering <br/>(för**Recovery Services-valvet**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Nätverksprotokoll <br/>(för**reservserver**) | |TCP |TCP | |
| Nätverksprotokoll <br/>(för**Recovery Services-valvet**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key-2.png)

hello VM-tillägget (hello IaaS VM) läser hello data direkt från hello Azure storage-konto via hello lagringsnätverk, så det inte är nödvändigt toocompress den här trafiken.

Om du använder en System Center DPM-server eller Azure Backup-Server som en sekundär server för säkerhetskopiering, komprimera hello data från primär server hello toohello reservserver. Komprimera data innan den kan säkerhetskopieras tooDPM eller Azure Backup Server, sparar bandbredd.

#### <a name="network-throttling"></a>Nätverksbegränsningar
hello Azure Backup-agenten erbjuder nätverksbegränsning, vilket gör att du toocontrol hur nätverksbandbredden används när data överfördes. Begränsning kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik. Begränsning för data transfer gäller tooback upp och återställa aktiviteter.

## <a name="backup-and-retention"></a>Säkerhetskopiering och kvarhållning

Azure Backup har en gräns på 9 999 återställningspunkter (även kallade säkerhetskopior eller ögonblicksbilder) per *skyddad instans*. Skyddade instansen är en dator, server (fysiska eller virtuella) eller arbetsbelastning konfigurerad tooback upp data tooAzure. Mer information finns i avsnittet hello [vad är en instans av skyddade](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). En instans är skyddad när en säkerhetskopia av data har sparats. hello säkerhetskopia av data är hello skydd. Om hello källdata bröts eller skadades, kunde hello säkerhetskopia återställa hello källdata. hello visar följande tabell hello maximala säkerhetskopieringsfrekvensen för varje komponent. Konfigurationen av princip för säkerhetskopiering avgör hur snabbt du använder hello Återställningspunkter. Om du till exempel skapar en återställningspunkt om dagen kan du behålla återställningspunkter i 27 år innan de tar slut. Om du skapar en månatlig återställningspunkt, kan du behålla återställningspunkter 833 år innan du kör. hello Backup-tjänsten anger inte en tidsgräns för upphör att gälla på en återställningspunkt.

|  | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Säkerhetskopieringsfrekvens<br/> (tooRecovery Services valvet) |Tre säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |En säkerhetskopiering om dagen |
| Säkerhetskopieringsfrekvens<br/> (toodisk) |Inte tillämpligt |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar</p> |Inte tillämpligt |
| Kvarhållningsalternativ |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |
| Högsta antal återställningspunkter per skyddad instans |9999|9999|9999|9999|
| Högsta kvarhållningsperiod |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |
| Återställningspunkter på lokal disk |Inte tillämpligt |<li>64 för filservrar,<li>448 för programservrar |<li>64 för filservrar,<li>448 för programservrar |Inte tillämpligt |
| Återställningspunkter på band |Inte tillämpligt |Obegränsat |Inte tillämpligt |Inte tillämpligt |

## <a name="what-is-a-protected-instance"></a>Vad är en skyddad instans?
Skyddade instansen är en generisk referens tooa Windows-dator, en server (fysisk eller virtuell) eller SQL-databas som har konfigurerade tooback in tooAzure. En instans skyddas när du konfigurerar en princip för säkerhetskopiering för hello dator, server eller databas och skapa en säkerhetskopia av hello data. Efterföljande kopior av hello säkerhetskopierade data för den skydda instansen (som kallas återställningspunkter), öka hello mängden lagringsutrymme som förbrukas. Du kan skapa upp too9999 återställningspunkter för skyddade instansen. Om du tar bort en återställningspunkt från lagring räknas mot hello 9999 recovery punkt totalt.
Några vanliga exempel på skyddade instanser är virtuella datorer, servrar, databaser och personliga datorer som kör operativsystemet för Windows hello. Exempel:

* En virtuell dator som kör hello Hyper-V eller Azure IaaS hypervisor-infrastrukturen. hello gästoperativsystem för virtuella hello kan vara Windows Server- eller Linux.
* En programserver: hello-programservern kan vara en fysisk eller virtuell dator som kör Windows Server och arbetsbelastningar med data som behöver säkerhetskopieras toobe. Vanliga arbetsbelastningar är Microsoft SQL Server, Microsoft Exchange server, Microsoft SharePoint server och hello filserverrollen på Windows Server. tooback in dessa arbetsbelastningar behöver du System Center Data Protection Manager (DPM) eller Azure Backup Server.
* En dator, en arbetsstation eller en bärbar dator kör hello Windows-operativsystem.


## <a name="what-is-a-recovery-services-vault"></a>Vad är ett Recovery Services-valv?
Recovery Services-valvet är en onlinelagring entitet i Azure används toohold data, till exempel säkerhetskopior, återställningspunkter och principer för säkerhetskopiering. Du kan använda återställningstjänster valv toohold säkerhetskopierade data för Azure-tjänster och lokala servrar och arbetsstationer. Recovery Services-valv gör det enkelt tooorganize dina säkerhetskopierade data och minimerar hanteringskostnader. Du kan skapa hur många Recovery Services-valv du vill inom en prenumeration.

Säkerhetskopieringsvalv som baseras på Azure Service Manager, var hello första versionen av hello-valvet. Recovery Services-valv som lägger till funktioner för hello Azure Resource Manager-modellen, är hello andra versionen av hello-valvet. Se hello [Recovery Services-valvet översiktsartikel](backup-azure-recovery-services-vault-overview.md) för en fullständig beskrivning av hello funktionsskillnader. Du kan inte längre skapa Använd hello portal toocreate säkerhetskopieringsvalv, men säkerhetskopieringsvalv som fortfarande stöds.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> **15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv. <br/> **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Vad är skillnaden mellan Azure Backup och Azure Site Recovery?
Azure Backup och Azure Site Recovery är relaterade eftersom båda tjänsterna säkerhetskopierar och återställer data. Dessa tjänster tjänar dock olika syften när det gäller att tillhandahålla affärskontinuitet och haveriberedskap i organisationen. Använda Azure Backup tooprotect och återställa data på en mer detaljerad nivå. Till exempel om en presentation på en bärbar dator blev skadad använder du Azure Backup toorestore hello presentation. Om du vill använda tooreplicate hello konfiguration och data på en virtuell dator i en annan datacenter kan använda Azure Site Recovery.

Azure-säkerhetskopiering skyddar data lokalt och i hello molnet. Azure Site Recovery samordnar replikeringen på virtuella datorer och fysiska servrar, redundans och återställning. Båda tjänsterna är viktiga eftersom din lösning för katastrofåterställning måste tookeep din information och återställningsbara (säkerhetskopiering) *och* hålla dina arbetsbelastningar tillgängliga (Site Recovery) när avbrott inträffar.

hello följande begrepp som hjälper dig att fatta viktiga beslut runt säkerhetskopiering och katastrofåterställning.

| Begrepp | Information | Säkerhetskopiering | Haveriberedskap |
| --- | --- | --- | --- |
| Mål för återställningspunkt (RPO) |hello mängden godkända dataförlust om en återställning måste toobe klar. |Säkerhetskopieringslösningar har stor variation vad gäller deras godtagbara återställningspunktmål. Säkerhetskopieringar av virtuella datorer har vanligtvis ett återställningspunktmål på en dag, medan säkerhetskopieringar av databaser har återställningspunktmål på så lite som 15 minuter. |Lösningar för haveriberedskap har låga återställningspunktmål. hello DR kopia kan vara bakom några sekunder eller minuter. |
| Mål för återställningstid (RTO) |hello mängden tid det tar toocomplete en återställning eller återställning. |På grund av hello större Återställningspunktsmål, hello mängden data som en lösning för säkerhetskopiering behöver tooprocess är vanligtvis mycket högre som leder toolonger RTOs. Det kan till exempel ta dagar toorestore data från band, beroende på hello tid det tar tootransport hello band från en plats utanför kontoret. |Lösningar för katastrofåterställning har mindre RTOs eftersom de är mer synkroniserad med hello källa. Färre ändringar måste toobe bearbetas. |
| Kvarhållning |Hur länge data måste toobe lagras |För scenarier som kräver driftåterställning (datafel, oavsiktlig filborttagning, operativsystemfel osv.) bevaras säkerhetskopierade data vanligtvis i 30 dagar eller mindre.<br>Från en efterlevnad synvinkel behöva data toobe lagras för månader eller till och med år. Säkerhetskopierade data är idealiska för arkivering i dessa fall. |Katastrofåterställning måste bara operativa återställningsdata, vilket brukar tar några timmar eller in tooa dag. På grund av hello detaljerade infångade data används i DR-lösningar rekommenderas med hjälp av DR-data för långsiktig kvarhållning inte. |

## <a name="next-steps"></a>Nästa steg
Använd någon av följande självstudier för detaljerade steg för steg instruktioner för hur du skyddar data på Windows Server eller skyddar en virtuell dator (VM) hello i Azure:

* [Säkerhetskopiera filer och mappar](backup-try-azure-backup-in-10-mins.md)
* [Säkerhetskopiera Azure Virtual Machines](backup-azure-vms-first-look-arm.md)

Mer information om att skydda andra arbetsbelastningar finns i någon av följande artiklar:

* [Säkerhetskopiera Windows Server](backup-configure-vault.md)
* [Säkerhetskopiera programarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
* [Säkerhetskopiera virtuella IaaS-datorer i Azure](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
