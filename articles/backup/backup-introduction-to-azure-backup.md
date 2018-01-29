---
title: "Vad är Azure Backup? | Microsoft Docs"
description: "Du kan använda Azure Backup till att säkerhetskopiera och återställa data och arbetsbelastningar från Windows-servrar, Windows-arbetsstationer, System Center DPM-servrar och virtuella datorer i Azure."
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
ms.topic: overview
ms.date: 1/5/2018
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 59beaac1d8619c3f4afa1c75074546a849dfce6b
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/06/2018
---
# <a name="overview-of-the-features-in-azure-backup"></a>Översikt över funktionerna i Azure Backup
Azure Backup är en Azure-baserad tjänst som du använder för att säkerhetskopiera (eller skydda) och återställa data i Microsoft-molnet. Azure Backup ersätter din befintliga lokala eller externa säkerhetskopieringslösning med en tillförlitlig och säker molnbaserad lösning med ett konkurrenskraftigt pris. Azure Backup erbjuder flera komponenter som du kan ladda ned och distribuera på den aktuella datorn, servern eller i molnet. Komponenten eller agenten som du distribuerar beror på vad du vill skydda. Alla Azure Backup-komponenter (oavsett om du skyddar data lokalt eller i molnet) kan användas för att säkerhetskopiera data till ett Recovery Services-valv i Azure. I [tabellen med Azure Backup-komponenter](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (längre ned i den här artikeln) finns information om vilken komponent du ska använda för att skydda specifika data, program eller arbetsbelastningar.

[Titta på en videoöversikt över Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Varför ska jag använda Azure Backup?
Traditionella säkerhetskopieringslösningar har utvecklats för att behandla molnet som en slutpunkt eller statisk lagringsdestination som liknar diskar eller band. Samtidigt som den här metoden är enkel är den begränsad och drar inte full nytta av en underliggande molnplattform, vilket innebär en dyr, ineffektiv lösning. Andra lösningar är dyra eftersom du får betala för fel typ av lagring eller lagring som du inte behöver. Andra lösningar är ofta ineffektiva eftersom de inte tillhandahåller den typ av eller mängd lagringsutrymme du behöver, eller så tar administrativa uppgifter för mycket tid. Däremot ger Azure Backup följande viktiga fördelar:

**Automatisk lagringshantering** – hybridmiljöer kräver ofta heterogen lagring – vissa lokalt och vissa i molnet. Med Azure Backup är det kostnadsfritt att använda lokala lagringsenheter. Azure Backup allokerar och hanterar lagringen av säkerhetskopiorna automatiskt och tillämpar en modell där du betalar baserat på din användning. Att betala baserat på din användning innebär att du bara betalar för den lagring du konsumerar. Mer information finns i artikeln om [prissättning i Azure](https://azure.microsoft.com/pricing/details/backup).

**Obegränsad skalning** – Azure Backup använder Azure-molnets underliggande kraft och obegränsade skala för att tillhandahålla hög tillgänglighet – utan underhåll och omkostnader för övervakning. Du kan ställa in aviseringar för att tillhandahålla information om händelser, men du behöver inte oroa dig för hög tillgänglighet för dina data i molnet.

**Flera lagringsalternativ** – en aspekt av hög tillgänglighet är lagringsreplikering. Azure Backup erbjuder två typer av replikering: [lokalt redundant lagring](../storage/common/storage-redundancy.md#locally-redundant-storage) och [geo-redundant lagring](../storage/common/storage-redundancy.md#geo-redundant-storage). Välj lagring för säkerhetskopiering baserat på behov:

* Lokalt redundant lagring (LRS) replikerar data tre gånger (det skapas tre kopior av dina data) i ett parat datacenter i samma region. LRS är ett billigt alternativ för att skydda dina data mot fel i den lokala maskinvaran.

* Geo-redundant lagring (GRS) replikerar dina data till en sekundär region (hundratals mil bort från den primära platsen för datakällan). GRS kostar mer än LRS, men GRS ger högre hållbarhet för dina data, även i händelse av ett regionalt avbrott.

**Obegränsad dataöverföring** – Azure Backup begränsar inte hur mycket inkommande eller utgående data du överför. Azure Backup debiterar inte heller för de data som överförs. Om du använder Azure Import/Export-tjänsten för att importera stora mängder data finns det dock en kostnad som är kopplad till inkommande data. Mer information om kostnaden finns i [Offline-backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Arbetsflöde för säkerhetskopiering offline i Azure Backup). Utgående data syftar på data som överförs från ett Recovery Services-valv under en återställningsåtgärd.

**Datakryptering** – Datakryptering möjliggör säker överföring och lagring av dina data i det offentliga molnet. Krypteringslösenfrasen lagras på lokalt och överförs eller lagras aldrig i Azure. Om det är nödvändigt att återställa data kan du göra det om du har krypteringslösenfrasen eller nyckeln.

**Programkonsekvent säkerhetskopiering** – Oavsett om du säkerhetskopierar en filserver, en virtuell dator eller en SQL-databas behöver du veta att en återställningspunkt har alla nödvändiga data för att återställa säkerhetskopian. Azure Backup innehåller programkonsekventa säkerhetskopior vilket garanterar att inga ytterligare korrigeringar behövs för att återställa data. Återställning av konsekventa programdata minskar tiden för återställning, så att du snabbt kan återgå till körläge.

**Långsiktig kvarhållning** – I stället för att växla säkerhetskopior från disk till band och sedan flytta bandet till en annan plats kan du använda Azure för kortsiktig och långsiktig kvarhållning. Azure begränsar inte hur lång tid data behålls i ett Backup- eller Recovery Services-valv. Du kan förvara data i ett valv hur länge du vill. Azure Backup har en gräns på 9999 återställningspunkter per skyddad instans. Läs avsnittet [Säkerhetskopiering och kvarhållning](backup-introduction-to-azure-backup.md#backup-and-retention) i den här artikeln för att få en förklaring av hur den här gränsen kan påverka dina säkerhetskopieringsbehov.  

## <a name="which-azure-backup-components-should-i-use"></a>Vilka Azure Backup-komponenter ska jag använda?
Om du inte är säker på vilken Azure Backup-komponent som passar dina behov kan du ta en titt i följande tabell och få information om vad du kan skydda med varje komponent. Azure Portal innehåller en guide, som är inbyggd i portalen, som hjälper dig att välja en komponent att ladda ned och distribuera. Guiden som är en del av skapandet av Recovery Services-valvet leder dig genom stegen för att välja ett mål för säkerhetskopiering och data eller program som ska skyddas.

| Komponent | Fördelar | Begränsningar | Vad skyddas? | Var lagras säkerhetskopiorna? |
| --- | --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<li>Säkerhetskopiera filer och mappar på en fysisk eller virtuell dator med Windows OS (virtuella datorer kan finnas lokalt eller i Azure)<li>Ingen separat säkerhetskopieringsserver krävs. |<li>Säkerhetskopiera 3 gånger per dag <li>Inte programmedveten, endast återställning på fil-/mapp-/volymnivå, <li>  Inget stöd för Linux. |<li>Filer, <li>Mappar, <li>Systemtillstånd |Recovery Services-valv |
| System Center DPM |<li>Programmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när du vill skapa säkerhetskopior<li>Återställningsprecision (allt)<li>Kan använda Recovery Services-valv<li>Linux-stöd på Hyper-V- och VMware-baserade virtuella datorer <li>Säkerhetskopiera och återställ virtuella VMware-datorer med DPM 2012 R2 |Det går inte att säkerhetskopiera Oracle-arbetsbelastningar.|<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Recovery Services-valv,<li> Lokalt ansluten disk,<li>  Band (endast lokalt) |
| Azure Backup Server |<li>Appmedvetna ögonblicksbilder (VSS)<li>Fullständig flexibilitet när du vill skapa säkerhetskopior<li>Återställningsprecision (allt)<li>Kan använda Recovery Services-valv<li>Linux-stöd på Hyper-V- och VMware-baserade virtuella datorer<li>Säkerhetskopiera och återställ virtuella VMware-datorer <li>Kräver inte en System Center-licens |<li>Det går inte att säkerhetskopiera Oracle-arbetsbelastningar.<li>Kräver alltid en aktiv Azure-prenumeration<li>Inget stöd för säkerhetskopiering på band |<li>Filer, <li>Mappar,<li> Volymer, <li>Virtuella datorer,<li> Program,<li> Arbetsbelastningar |<li>Recovery Services-valv,<li> Lokalt ansluten disk |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<li>Interna säkerhetskopieringar för Windows/Linux<li>Ingen specifik agentinstallation krävs<li>Säkerhetskopiering på infrastrukturnivå utan behov av en infrastruktur för säkerhetskopiering |<li>Säkerhetskopiera virtuella datorer en gång om dagen <li>Återställ virtuella datorer endast på disknivå<li>Det går inte att säkerhetskopiera lokalt |<li>Virtuella datorer, <li>Alla diskar (med PowerShell) |<p>Recovery Services-valv</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Vilka är distributionsscenarierna för varje komponent?
| Komponent | Kan den distribueras i Azure? | Kan den distribuerade lokalt? | Mållagring som stöds |
| --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<p>**Ja**</p> <p>Azure Backup-agenten kan distribueras på virtuella datorer med Windows som körs i Azure.</p> |<p>**Ja**</p> <p>Backup-agenten kan distribueras på virtuella datorer med Windows Server eller en fysisk dator.</p> |<p>Recovery Services-valv</p> |
| System Center DPM |<p>**Ja**</p><p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med hjälp av System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur du skyddar arbetsbelastningar och virtuella datorer i ditt datacenter](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokalt ansluten disk,</p> <p>Recovery Services-valv,</p> <p>band (endast lokalt)</p> |
| Azure Backup Server |<p>**Ja**</p><p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Lär dig mer om [hur du skyddar arbetsbelastningar i Azure med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokalt ansluten disk,</p> <p>Recovery Services-valv</p> |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |<p>**Ja**</p><p>En del av Azure-infrastrukturen</p><p>Specialiserad för [säkerhetskopiering av virtuella Iaas-datorer (Infrastructure as a Service) i Azure](backup-azure-vms-introduction.md).</p> |<p>**Nej**</p> <p>Använd System Center DPM för att säkerhetskopiera virtuella datorer i datacentret.</p> |<p>Recovery Services-valv</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Vilka program och arbetsbelastningar kan säkerhetskopieras?
Följande tabell innehåller en matris med data och arbetsbelastningar som kan skyddas med Azure Backup. I kolumnen med Azure Backup-lösningar finns länkar till dokumentationen för lösningen. 

| Data eller arbetsbelastning | Källmiljö | Azure Backup-lösning |
| --- | --- | --- |
| Filer och mappar |Windows Server |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Filer och mappar |Windows-dator |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuell Hyper-V-dator (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuell Hyper-V-dator (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuell VMware-dator |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-agenten),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (Azure Backup-agenten ingår)</p> |
| Virtuella IaaS-datorer i Azure (Windows) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |
| Virtuella IaaS-datorer i Azure (Linux) |som körs i Azure |[Azure Backup (VM-tillägg)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux-support
I följande tabell visas de Azure Backup-komponenter som har stöd för Linux.  

| Komponent | Linux-stöd (Azure-godkänt) |
| --- | --- |
| Azure Backup-agent (MARS) |Nej (endast Windows-baserad agent) |
| System Center DPM |<li> Filkonsekvent säkerhetskopiering av virtuella Linux-gästdatorer på Hyper-V och VMWare<br/> <li> Återställning av virtuella Linux-gästdatorer på Hyper-V och VMWare </br> </br>  *Filkonsekvent säkerhetskopiering är inte tillgängligt för Azure VM* <br/> |
| Azure Backup Server |<li>Filkonsekvent säkerhetskopiering av virtuella Linux-gästdatorer på Hyper-V och VMWare<br/> <li> Återställning av virtuella Linux-gästdatorer på Hyper-V och VMWare </br></br> *Filkonsekvent säkerhetskopiering är inte tillgängligt för Azure VM*  |
| Säkerhetskopiering av virtuella IaaS-datorer i Azure |Programkonsekvent säkerhetskopiering med [ramverk för förskript och efterskript](backup-azure-linux-app-consistent.md)<br/> [Detaljerad filåterställning](backup-azure-restore-files-from-vm.md)<br/> [Återställ alla diskar på virtuella datorer](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [Återställning av virtuella datorer](backup-azure-arm-restore-vms.md#create-a-new-vm-from-a-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Använd virtuella Premium Storage-datorer med Azure Backup
Azure Backup skyddar virtuella datorer i Premium Storage. Azure Premium Storage är SSD-baserad (solid-state drive) lagring som har utformats för att fungera med I/O-intensiva arbetsbelastningar. Premium Storage är attraktivt för arbetsbelastningar för virtuella datorer. Mer information om Premium-lagring finns i artikeln [Premium Storage: högpresterande lagring för virtuella Azure-datorbelastningar](../virtual-machines/windows/premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Säkerhetskopiera virtuella datorer i Premium Storage
När du säkerhetskopierar virtuella datorer i Premium Storage skapar Backup-tjänsten en tillfällig mellanlagringsplats med namnet ”AzureBackup-” i Premium Storage-kontot. Storleken på mellanlagringsplatsen är lika stor som återställningspunktens ögonblicksbild. Kontrollera att Premium Storage-kontot har tillräckligt med ledigt utrymme så att den tillfälliga mellanlagringsplatsen får plats. Mer information finns i artikeln om [Premium Storage-begränsningar](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets). När säkerhetskopieringen är klar tas mellanlagringsplatsen bort. Priset för lagringen som används för mellanlagringsplatsen följer [prissättningen för Premium-lagring](../virtual-machines/windows/premium-storage.md#pricing-and-billing).

> [!NOTE]
> Ändra inte mellanlagringsplatsen.
>
>

### <a name="restore-premium-storage-vms"></a>Återställa virtuella datorer i Premium Storage
Virtuell dator för Premium Storage VM kan återställas till antingen Premium Storage-lagring eller normal lagring. I en typisk återställningsprocess återställs en återställningspunkt för virtuella datorer i Premium Storage till Premium Storage. Det kan dock vara kostnadseffektivt att återställa en återställningspunkt för virtuella datorer i Premium Storage till standardlagring. Den här typen av återställning kan vara praktisk om du behöver en delmängd av filerna från den virtuella datorn.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Använda virtuella datorer med hanterade diskar med Azure Backup
Azure Backup skyddar virtuella datorer med hanterade diskar. Om du använder hanterade diskar behöver du inte hantera lagringskonton för virtuella datorer och det blir mycket enklare att etablera virtuella datorer.

### <a name="back-up-managed-disk-vms"></a>Säkerhetskopiera virtuella datorer med hanterade diskar
Säkerhetskopieringen av virtuella datorer på hanterade diskar fungerar på samma sätt som säkerhetskopieringen av virtuella datorer med Resource Manager. I Azure Portal kan du konfigurera säkerhetskopieringen direkt från vyn Virtuell dator eller från Recovery Services-valvvyn. Du kan säkerhetskopiera virtuella datorer på hanterade diskar via RestorePoint-samlingar som är byggda ovanpå hanterade diskar. Azure Backup stöder också säkerhetskopiering av virtuella datorer på hanterade diskar som krypterats med ADE (Azure Disk Encryption).

### <a name="restore-managed-disk-vms"></a>Återställa virtuella datorer med hanterade diskar
Med Azure Backup kan du återställa en hel virtuell dator med hanterade diskar eller återställa hanterade diskar till ett lagringskonto. Azure hanterar de hanterade diskarna under återställningsprocessen. Du (kunden) hanterar lagringskontot som skapas som en del av återställningsprocessen. När du återställer hanterade krypterade virtuella datorer bör de virtuella datorernas nycklar och hemligheter finnas i nyckelvalvet innan återställningsåtgärden startar.

## <a name="what-are-the-features-of-each-backup-component"></a>Vilka är funktionerna i varje Backup-komponent?
Följande avsnitt innehåller tabeller som sammanfattar tillgänglighet eller stöd för olika funktioner i varje komponent i Azure Backup. Titta på informationen efter varje tabell för ytterligare support eller information.

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

Recovery Services-valvet är det prioriterade lagringsmålet för alla komponenter. Med System Center DPM och Azure Backup Server kan du också välja att kopiera en lokal disk. Dock kan du endast skriva data till en bandlagringsenhet med System Center DPM.

#### <a name="compression"></a>Komprimering
Säkerhetskopior komprimeras för att minska lagringsutrymmet som krävs. Den enda komponenten som inte använder komprimering är VM-tillägget. VM-tillägget kopierar alla säkerhetskopierade data från ditt lagringskonto till Recovery Services-valvet i samma region. Ingen komprimering används vid överföringen av data. Överföringen av data utan komprimering förbrukar något mer lagringsutrymme. Lagringen av data utan komprimering medför dock snabbare återställning, om du behöver använda den återställningspunkten.


#### <a name="disk-deduplication"></a>Diskdeduplicering
Du kan dra nytta av datadeduplicering när du distribuerar System Center DPM eller Azure Backup Server [på en virtuell Hyper-V-dator](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server utför datadeduplicering (på värdnivå) på virtuella hårddiskar (VHD) som är kopplade till den virtuella datorn som lagringsutrymme för säkerhetskopior.

> [!NOTE]
> Deduplicering är inte tillgängligt i Azure för någon Backup-komponenter. Om System Center DPM och Backup Server distribueras i Azure går det inte att deduplicera lagringsdiskarna som är kopplade till den virtuella datorn.
>
>

### <a name="incremental-backup-explained"></a>Förklaring av inkrementell säkerhetskopiering
Alla Azure Backup-komponenter stöder inkrementell säkerhetskopiering oavsett mållagring (disk, band eller Recovery Services-valv). Inkrementell säkerhetskopiering ser till att säkerhetskopieringarna är lagrings- och tidseffektiva genom att endast överföra de ändringar som gjorts sedan den senaste säkerhetskopieringen.

#### <a name="comparing-full-differential-and-incremental-backup"></a>Jämföra fullständig, differentiell och inkrementell säkerhetskopiering

Förbrukning av lagringsutrymme, mål för återställningstid (RTO) och nätverksförbrukning varierar beroende på metod för säkerhetskopiering. För att hålla nere säkerhetskostnadens TCO (totalkostnad för ägarskap) måste du förstå hur du ska välja den bästa säkerhetskopieringslösningen. Följande bild jämför fullständig, differentiell och inkrementell säkerhetskopiering. I bilden består datakälla A av 10 lagringsblock, A1–A10, som säkerhetskopieras varje månad. Block A2, A3, A4 och A9 ändras under den första månaden, och block A5 ändras nästa månad.

![bild som visar jämförelser av metoder för säkerhetskopiering](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

Med **fullständig säkerhetskopiering** innehåller varje säkerhetskopia hela datakällan. Fullständig säkerhetskopiering förbrukar en stor mängd av nätverkets bandbredd och lagring varje gång en säkerhetskopia överförs.

**Differentiell säkerhetskopiering** lagrar endast de block som har ändrats sedan den första fullständiga säkerhetskopieringen, vilket resulterar i en lägre förbrukning av nätverk och lagringsutrymme. Differentiella säkerhetskopieringar lagrar inte redundanta kopior av oförändrade data. Men eftersom de datablock som förblir oförändrade mellan efterföljande säkerhetskopieringar överförs och lagras är differentiella säkerhetskopieringar ineffektiva. I den andra månaden säkerhetskopieras ändrade A2- A3- A4- och A9-block. Den tredje månaden säkerhetskopieras samma block igen, tillsammans med ändrade A5-block. De ändrade blocken fortsätter att säkerhetskopieras tills nästa fullständiga säkerhetskopiering sker.

**Inkrementell säkerhetskopiering** uppnår hög effektivitet för lagring och nätverk genom att endast lagra de datablock som har ändrats sedan föregående säkerhetskopiering. Med inkrementell säkerhetskopiering behöver du inte göra regelbundna fullständiga säkerhetskopior. I exemplet är ändrade block A2, A3, A4, och A9 markerade som ändrade och överförda för den andra månaden efter att en fullständig säkerhetskopiering har gjorts för den första månaden. I den tredje månaden markeras och överförs endast ändrade A5-block. Förflyttning av mindre datamängder sparar lagringsutrymme och nätverksresurser, vilket sänker den totala ägandekostnaden.   

### <a name="security"></a>Säkerhet
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkssäkerhet<br/> (för Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |
| Datasäkerhet<br/> (i Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Delvis][yellow] |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Nätverkssäkerhet
All säkerhetskopieringstrafik från dina servrar till Recovery Services-valvet krypteras med hjälp av Advanced Encryption Standard 256. Säkerhetskopierade data skickas via en säker HTTPS-anslutning. Säkerhetskopierade data lagras också i Recovery Services-valvet i krypterad form. Endast du, Azure-kunden, har tillgång till lösenfrasen som krävs för att låsa upp dessa data. Microsoft kan aldrig dekryptera säkerhetskopierade data.

> [!WARNING]
> När du har etablerat Recovery Services-valvet är du den enda som har åtkomst till krypteringsnyckeln. Microsoft sparar aldrig en kopia av krypteringsnyckeln och har inte åtkomst till nyckeln. Om du tappar bort nyckeln kan inte Microsoft återställa dina säkerhetskopierade data.
>
>

#### <a name="data-security"></a>Datasäkerhet
Säkerhetskopieringen av virtuella datorer i Azure kräver krypteringsinställningar *på* den virtuella datorn. Använd BitLocker på virtuella Windows-datorer och **dm crypt** på virtuella Linux-datorer. Azure Backup krypterar inte automatiskt säkerhetskopieringsdata som finns på den här sökvägen.

### <a name="network"></a>Nätverk
| Funktion | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Nätverkskomprimering <br/>(till **backup server**) | |![Ja][green] |![Ja][green] | |
| Nätverkskomprimering <br/>(till **Recovery Services-valv**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Nätverksprotokoll <br/>(till **backup server**) | |TCP |TCP | |
| Nätverksprotokoll <br/>(till **Recovery Services-valv**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tabellförklaring](./media/backup-introduction-to-azure-backup/table-key-2.png)

VM-tillägget (eller virtuella IaaS-datorer) läser data direkt från Azure Storage-kontot i lagringsnätverket behöver du inte komprimera den här trafiken.

Om du använder en System Center DPM-server eller Azure Backup Server som en sekundär server för säkerhetskopiering, komprimerar du data som överförs från den primära servern till säkerhetskopieringsservern. Du kan spara bandbredd genom att komprimera data innan du säkerhetskopierar dem till DPM eller Azure Backup Server.

#### <a name="network-throttling"></a>Nätverksbegränsningar
Azure Backup-agenten tillhandahåller nätverksbegränsning som du kan använda för att styra hur nätverksbandbredden används under dataöverföringar. Begränsning kan vara användbart om du behöver säkerhetskopiera data under arbetstid, men inte vill att säkerhetskopieringsprocessen ska störa annan Internettrafik. Begränsningar av dataöverföringar gäller säkerhetskopierings- och återställningsaktiviteter.

## <a name="backup-and-retention"></a>Säkerhetskopiering och kvarhållning

Azure Backup har en gräns på 9 999 återställningspunkter (även kallade säkerhetskopior eller ögonblicksbilder) per *skyddad instans*. En skyddad instans är en dator eller server (fysisk eller virtuell) eller en arbetsbelastning som konfigurerats för att säkerhetskopiera data till Azure. Mer information finns i avsnittet [Vad är en skyddad instans?](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). En instans är skyddad när en säkerhetskopia av data har sparats. Säkerhetskopian av data är skyddet. Om datakällan går förlorad eller skadas kan den återställas med hjälp av säkerhetskopian. Följande tabell visar den högsta säkerhetskopieringsfrekvensen för varje komponent. Din konfiguration av säkerhetskopieringspolicyer avgör hur snabbt du förbrukar återställningspunkterna. Om du till exempel skapar en återställningspunkt om dagen kan du behålla återställningspunkter i 27 år innan de tar slut. Om du skapar en månatlig återställningspunkt kan du behålla återställningspunkter i 833 år innan de tar slut. Backup-tjänsten ställer inte in någon gräns för giltighetstiden för en återställningspunkt.

|  | Azure Backup-agent | System Center DPM | Azure Backup Server | Säkerhetskopiering av virtuella IaaS-datorer i Azure |
| --- | --- | --- | --- | --- |
| Säkerhetskopieringsfrekvens<br/> (till Recovery Services-valv) |Tre säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |Två säkerhetskopieringar om dagen |En säkerhetskopiering om dagen |
| Säkerhetskopieringsfrekvens<br/> (till disk) |Inte tillämpligt |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar |<li>Varje kvart för SQL Server <li>Varje timme för andra arbetsbelastningar</p> |Inte tillämpligt |
| Kvarhållningsalternativ |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |Varje dag, varje vecka, varje månad, varje år |
| Högsta antal återställningspunkter per skyddad instans |9999|9999|9999|9999|
| Högsta kvarhållningsperiod |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |Beror på säkerhetskopieringsfrekvensen |
| Återställningspunkter på lokal disk |Inte tillämpligt |<li>64 för filservrar,<li>448 för programservrar |<li>64 för filservrar,<li>448 för programservrar |Inte tillämpligt |
| Återställningspunkter på band |Inte tillämpligt |Obegränsat |Inte tillämpligt |Inte tillämpligt |

## <a name="what-is-a-protected-instance"></a>Vad är en skyddad instans?
En skyddad instans är en generisk referens till en Windows-dator, en server (fysisk eller virtuell) eller en SQL-databas som har konfigurerats för att säkerhetskopiera till Azure. En instans är skyddad när du har konfigurerat en säkerhetskopieringsprincip för datorn, servern eller databasen och skapar en säkerhetskopia av data. Efterföljande kopior av säkerhetskopierade data för den skyddade instansen (kallade återställningspunkter) ökar mängden lagringsutrymme som förbrukas. Du kan skapa upp till 9 999 återställningspunkter för en skyddad instans. Om du tar bort en återställningspunkt från lagring räknas den inte mot det sammanlagda antalet på 9 999 återställningspunkter.
Några vanliga exempel på skyddade instanser är virtuella datorer, programservrar, databaser och personliga datorer som kör Windows-operativsystemet. Till exempel:

* En virtuell dator som kör Hyper-V- eller Azure IaaS-hypervisorinfrastrukturen. Gästoperativsystemen för den virtuella datorn kan vara Windows Server eller Linux.
* En programserver: Programservern kan vara en fysisk eller virtuell dator som kör Windows Server och arbetsbelastningar med data som behöver säkerhetskopieras. Vanliga arbetsbelastningar är Microsoft SQL Server, Microsoft Exchange Server, Microsoft SharePoint Server och filserverrollen i Windows Server. Om du vill säkerhetskopiera dessa arbetsbelastningar behöver du System Center Data Protection Manager (DPM) eller Azure Backup Server.
* En personlig dator, arbetsstation eller bärbar dator som kör Windows-operativsystemet.


## <a name="what-is-a-recovery-services-vault"></a>Vad är ett Recovery Services-valv?
Ett Recovery Services-valv är en onlinelagringsentitet i Azure som används för att lagra data, som säkerhetskopior, återställningspunkter och principer för säkerhetskopiering. Du kan använda Recovery Services-valv till att lagra säkerhetskopierade data för Azure-tjänster, lokala servrar och arbetsstationer. Med Recovery Services-valv är det enkelt att organisera dina säkerhetskopierade data samtidigt som du minimerar hanteringskostnaden. Du kan skapa hur många Recovery Services-valv du vill inom en prenumeration.

Säkerhetskopieringsvalv, som baseras på Azure Service Manager, var den första valvversionen. Recovery Services-valv, som lägger till funktioner från Azure Resource Manager-modellen, är den andra versionen. I artikeln [Recovery Services vault overview](backup-azure-recovery-services-vault-overview.md) (Översikt över Recovery Services-valv) ges en fullständig beskrivning av funktionsskillnaderna. Du kan inte längre använda portalen för att skapa säkerhetskopieringsvalv, men säkerhetskopieringsvalv kan fortfarande användas. Du måste använda Azure Portal för att hantera dina säkerhetskopieringsvalv.

> [!IMPORTANT]
> Du kan uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv. Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv). Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.<br/> Efter den 30 november 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv och alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Vad är skillnaden mellan Azure Backup och Azure Site Recovery?
Azure Backup och Azure Site Recovery är relaterade eftersom båda tjänsterna säkerhetskopierar och återställer data. Dessa tjänster tjänar dock olika syften när det gäller att tillhandahålla affärskontinuitet och haveriberedskap i organisationen. Använd Azure Backup om du vill skydda och återställa data på en mer detaljerad nivå. Om till exempel en presentation på en bärbar dator skadas, använder du Azure Backup för att återställa presentationen. Om du vill replikera konfigurationen och data på en virtuell dator på ett annat datacenter använder du Azure Site Recovery.

Azure Backup skyddar data lokalt och i molnet. Azure Site Recovery samordnar replikeringen på virtuella datorer och fysiska servrar, redundans och återställning. Båda tjänsterna är viktiga eftersom en haveriberedskapslösning måste skydda dina data, se till att de kan återställas (säkerhetskopiering) *och* säkerställa att arbetsbelastningarna förblir tillgängliga (Site Recovery) i händelse av avbrott.

Följande begrepp hjälper dig att fatta viktiga beslut om säkerhetskopiering och haveriberedskap.

| Begrepp | Information | Backup | Haveriberedskap |
| --- | --- | --- | --- |
| Mål för återställningspunkt (RPO) |Mängden godtagbar dataförlust om en återställning krävs. |Säkerhetskopieringslösningar har stor variation vad gäller deras godtagbara återställningspunktmål. Säkerhetskopieringar av virtuella datorer har vanligtvis ett återställningspunktmål på en dag, medan säkerhetskopieringar av databaser har återställningspunktmål på så lite som 15 minuter. |Lösningar för haveriberedskap har låga återställningspunktmål. Kopian för haveriberedskap kan ligga några få sekunder eller minuter efter. |
| Mål för återställningstid (RTO) |Hur lång tid det tar att slutföra en återställning. |På grund av det större återställningspunktmålet är mängden data som en säkerhetskopieringslösning behöver bearbeta normalt mycket högre, vilket leder till längre mål för återställningstid. Det kan till exempel ta dagar att återställa data från band, beroende på hur lång tid det tar att överföra bandet från den externa platsen. |Lösningar för haveriberedskap har mindre mål för återställningstid eftersom de är mer synkroniserade med källan. Färre ändringar behöver bearbetas. |
| Kvarhållning |Hur länge data ska lagras. |För scenarier som kräver driftåterställning (datafel, oavsiktlig filborttagning, operativsystemfel osv.) bevaras säkerhetskopierade data vanligtvis i 30 dagar eller mindre.<br>Ur efterlevnadssynvinkel kanske data behöver sparas i flera månader eller till och med år. Säkerhetskopierade data är idealiska för arkivering i dessa fall. |Haveriberedskap kräver endast driftåterställningsdata, som normalt tar några timmar eller upp till en dag. På grund av den detaljerade datainsamlingen i lösningar för haveriberedskap rekommenderar vi inte att haveriberedskapsdata används för långsiktig kvarhållning. |

## <a name="next-steps"></a>Nästa steg
Använd någon av följande självstudiekurser för att få detaljerade steg för steg-instruktioner för hur du skyddar data på Windows Server eller på en virtuell dator (VM) i Azure:

* [Säkerhetskopiera filer och mappar](backup-try-azure-backup-in-10-mins.md)
* [Säkerhetskopiera Azure Virtual Machines](backup-azure-vms-first-look-arm.md)

Mer information om att skydda andra arbetsbelastningar finns i någon av följande artiklar:

* [Säkerhetskopiera Windows Server](backup-configure-vault.md)
* [Säkerhetskopiera programarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
* [Säkerhetskopiera virtuella IaaS-datorer i Azure](backup-azure-arm-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
