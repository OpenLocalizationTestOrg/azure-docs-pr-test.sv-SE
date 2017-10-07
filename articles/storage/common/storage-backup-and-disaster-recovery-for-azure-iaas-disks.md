---
title: "aaaBackup och disaster recovery för Azure IaaS-diskar | Microsoft Docs"
description: "I den här artikeln förklarar vi hur tooplan för säkerhetskopiering och Disaster Recovery (DR) för IaaS-virtuella maskiner (VMs) och diskarna i Azure. Det här dokumentet beskriver både hanterade och ohanterade diskar"
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: luywang
ms.openlocfilehash: 49b0e7732d6df9407e1e44d9af2500c99a85b37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Säkerhetskopiering och katastrofåterställning återställning för Azure IaaS-diskar

I den här artikeln förklarar vi hur tooplan för säkerhetskopiering och Disaster Recovery (DR) för IaaS-virtuella maskiner (VMs) och diskarna i Azure. Det här dokumentet beskriver både hanterade och ohanterade diskar.

Först ska vi tala om hello inbyggda feltolerans i hello Azure-plattformen som bidrar till att skydda dig mot lokala fel. Sedan kommer vi att diskutera hello katastrofåterställning scenarier som inte omfattas av inbyggda funktioner för hello, vilket är hello hjälpavsnittet beskrivs i det här dokumentet. Dessutom lär flera exempel på arbetsbelastningsscenarier där olika överväganden för säkerhetskopiering och Katastrofåterställning kan tillkomma. Vi ska granska möjliga lösningar för Katastrofåterställning för IaaS-diskar. 

## <a name="introduction"></a>Introduktion

hello Azure-plattformen använder olika metoder för redundans och feltolerans tolerans toohelp skydda kunder från lokaliserade maskinvarufel som kan uppstå. Lokala fel kan omfatta problem med en Azure storage server-dator på servern som innehåller en del av hello data för en virtuell disk eller fel i en SSD och HDD. Sådana isolerade maskinvarufel för komponenten kan inträffa under normal drift och hello plattformen är utformad toobe flexibel toothese fel. Större katastrofer kan resultera i fel eller inaccessibility av stora mängder lagringsservrar eller ett helt datacenter. Normalt dina virtuella datorer och diskar skyddas från lokaliserade fel ytterligare steg är nödvändiga tooprotect din arbetsbelastning från region hela katastrofalt fel (till exempel en större katastrof) kan påverka den virtuella datorn och diskar.

Dessutom toohello möjlighet att plattform fel uppstå problem med hello kunden program eller data kan. En ny version av programmet kan till exempel oavsiktligen gör en ny ändra toohello data. I så fall kanske du vill toorevert hello program och hello data tooa tidigare version som innehåller hello senaste fungerande tillstånd. Detta kräver att underhålla regelbundna säkerhetskopieringar.

För regional katastrofåterställning, måste du säkerhetskopiera din IaaS VM diskar tooa annan region. 

Innan vi tittar på alternativ för säkerhetskopiering och Katastrofåterställning Sammanfattningsvis vi några metoder för att hantera lokaliserade fel.

### <a name="azure-iaas-resiliency"></a>Azure IaaS återhämtning

*Återhämtning* refererar toohello tolerans för vanliga fel som uppstår i maskinvarukomponenter. Återhämtning är hello möjlighet toorecover från fel och fortsätta toofunction. Det är inte om att undvika fel, men svarar toofailures på ett sätt som förhindrar avbrott eller dataförlust. hello målet för återhämtning är tooreturn hello tooa fullt fungerande programtillstånd efter ett fel. Azure virtuella datorer och diskar är utformad toobe flexibel toocommon maskinvarufel. Låt oss titta på hur hello Azure IaaS-plattformen ger den här återhämtning.

En virtuell dator består i huvudsak av två delar: (1) A compute-servern och (2) hello beständiga diskar. Både påverkar hello feltolerans för en virtuell dator.

Om hello Azure compute-värdserver som innehåller den virtuella datorn får ett maskinvarufel (som är ovanligt), är utformad tooautomatically återställning hello VM på en annan server i Azure. Om det händer, blir det en omstart och hello VM kommer att säkerhetskopiera en stund. Azure automatiskt identifierar sådana maskinvarufel och kör återställningar toohelp kontrollerar hello kunden VM tillgängligt så snart som möjligt.

Om IaaS-diskar hållbarhet av data som är viktig för hello beständig lagring plattform. Azure-kunder har viktiga business-program körs med IaaS och de är beroende av hello persistence för hello data. Azure Designer skydd för dessa IaaS-diskar med tre redundanta kopior av data som lagras lokalt, ger hög hållbarhet mot lokala fel. Om en hello maskinvarukomponenter som innehåller disken misslyckas, påverkas inte den virtuella datorn eftersom det finns två ytterligare kopior toosupport disk begäranden. Den fungerar även om två olika maskinvarukomponenter som stöd för en disk inte vid hello samma tid (som skulle vara mycket sällsynt). toohelp se till att vi alltid har tre repliker, hello Azure Storage-tjänst skapas automatiskt en ny kopia av data i bakgrunden hello om något av tre kopior av hello blir otillgänglig. Det bör därför inte nödvändigt toouse RAID med Azure-diskar för feltolerans. En enkel RAID 0-konfigurationen ska vara tillräcklig för striping hello diskar om nödvändigt toocreate större volymer.

På grund av den här arkitekturen **Azure har konsekvent levereras företagsklass hållbarhet för IaaS-diskar, med ett branschledande noll % [medel Felintervall](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Lokaliserade maskinvarufel på hello compute-värd eller i hello lagring plattform ibland kan leda till tillfällig otillgänglighet för hello VM som omfattas av hello [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines/) för VM-tillgänglighet. Azure tillhandahåller också ett branschledande serviceavtal för enskild VM-instanser som använder Premiumlagring diskar.

toosafeguard programbelastningar driftstoppet på grund av toohello temporär otillgänglighet en disk eller en VM, kunder kan utnyttja [Tillgänglighetsuppsättningar](../../virtual-machines/windows/manage-availability.md). Två eller flera virtuella datorer i en tillgänglighetsuppsättning ger redundans för hello program. Azure skapar sedan dessa virtuella datorer och diskar i separata feldomäner med olika ström, nätverk och server-komponenter. Därför lokaliserade maskinvarufel vanligtvis inte påverkar flera virtuella datorer i hello på hello samtidigt, vilket ger hög tillgänglighet för ditt program. Anses det en bra idé toouse tillgänglighetsuppsättningar när hög tillgänglighet krävs. Mer information finns i hello katastrofåterställning aspekter beskrivs nedan.

### <a name="backup-and-disaster-recovery"></a>Säkerhetskopiering och katastrofåterställning

Katastrofåterställning (DR) är hello möjlighet toorecover från sällsynt men större incidenter: uppstod, storskaligt fel, till exempel avbrott i tjänsten som påverkar en hel region. Katastrofåterställning innehåller säkerhetskopiering och arkivering och kan innehålla manuella ingripanden, till exempel återställa en databas från en säkerhetskopia.

hello kanske Azure plattform inbyggt skydd mot fel på lokaliserade skyddar inte helt hello virtuella datorer/diskar hello gäller större katastrofer vilket kan orsaka stora avbrott. Detta inkluderar katastrofer, till exempel ett datacenter som träffas av en orkan jordbävning, fire eller stor skala enhet maskinvarufel. Dessutom kan det uppstå fel på grund av problem med tooapplication eller data.

toohelp skydda IaaS-arbetsbelastningar från avbrott, bör du planera för redundans och säkerhetskopiering tooenable återställning. Du bör planera redundans och säkerhetskopiering på en annan geografisk plats från hello primär plats för katastrofåterställning. Detta säkerställer säkerhetskopian inte påverkas av hello samma händelse som ursprungligen påverkas hello VM eller diskar. Mer information finns i [katastrofåterställning för program som bygger på Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

DR-överväganden kan inkludera hello följande aspekter:

1. Hög tillgänglighet är hello möjligheten för hello programmet toocontinue körs i ett felfritt tillstånd utan betydande driftavbrott. Med ”felfritt tillstånd”, menar vi hello program är känslig och användare kan ansluta toohello program och interagera med den. Vissa verksamhetskritiska program och databaser kanske krävs toobe tillgängliga alltid, även om det finns fel i hello-plattformen. Du kanske måste tooplan redundans för programmet hello samt hello data för dessa arbetsbelastningar.

2. Data hållbarhet: I vissa fall hello huvudsakliga övervägande är att säkerställa att hello data bevaras i hello skiftläget för en katastrofåterställning. Du måste därför kan en säkerhetskopia av dina data i en annan plats. För dessa arbetsbelastningar, kanske du inte behöver fullständig redundans för hello program, men en regelbunden säkerhetskopiering av hello diskar.

## <a name="backup-and-dr-scenarios"></a>Säkerhetskopiering och Katastrofåterställning scenarier

Nu ska vi titta på några exempel på arbetsbelastning Programscenarier och hello överväganden för planering av Disaster Recovery.

### <a name="scenario-1-major-database-solutions"></a>Scenario 1: Större databas lösningar

Överväg att en produktionsserver databasen som SQL Server eller Oracle som har stöd för hög tillgänglighet. Kritiska produktionsprogram och användare beror på den här databasen. hello Disaster Recovery-plan för det här systemet behöver toosupport hello följande krav:

1. Data måste vara skyddade och återställas.
2.  Servern måste vara tillgängliga för användning.

Du kan behöva underhålla en replik av hello databas i en annan region som en säkerhetskopia. Beroende på hello krav för servertillgänglighet och återställning av data, kan hello-lösning röra sig om en aktiv-aktiv eller aktivt-passivt replik plats tooperiodic offlinesäkerhetskopiering av hello data. Relationsdatabaser, till exempel SQL Server och Oracle ger olika alternativ för replikering. För SQL Server [SQL Server alltid på Tillgänglighetsgrupper](https://msdn.microsoft.com/library/hh510230.aspx) kan användas för hög tillgänglighet.

Det stöder också NoSQL-databaser som MongoDB [repliker](https://docs.mongodb.com/manual/replication/) för redundans. Du kan använda hello repliker för hög tillgänglighet.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scenario 2: Ett kluster med redundanta virtuella datorer

Överväg att en arbetsbelastning hanteras av ett kluster för virtuella datorer som ger redundans och belastningsutjämning. Ett exempel är ett Cassandra kluster som distribueras i en region. Den här typen av arkitekturen innehåller redan en hög nivå av redundans i detta område. Dock tooprotect hello arbetsbelastning från ett regionalt nivån fel, bör du sprida hello kluster mellan två regioner eller göra regelbundna säkerhetskopior tooanother region.

### <a name="scenario-3-iaas-application-workload"></a>Scenario 3: IaaS programmet arbetsbelastning

Detta kan vara en produktionsögonblicksbild vid arbetsbelastningar som körs på en virtuell dator i Azure. Det kan till exempel vara en webbserver eller filserver hålla hello innehåll och andra resurser för en plats. Det kan också vara ett specialbyggt affärsprogram som körs på en virtuell dator som lagras dess data, resurser och programtillstånd på hello Virtuella diskar. I det här fallet är det viktigt att du kan göra säkerhetskopior regelbundet toomake. Frekvens för säkerhetskopiering ska baseras på hello uppbyggnad hello VM arbetsbelastning. Till exempel om programmet hello körs varje dag och ändrar data, bör sedan hello säkerhetskopiering tas varje timme.

Ett annat exempel är en rapportserver som hämtar data från andra källor och genererar aggregerade rapporter. Förlust av den här virtuella datorn eller diskar leder toohello förlust av hello rapporter. Det kan dock vara möjligt toorerun hello reporting processen och generera hello utdata. I så fall kan du verkligen inte förlust av data även om hello rapporteringsservern är träffa med katastrofåterställning, så du kan ha en högre nivå av feltolerans för att förlora en del av hello data på hello reporting-servern. I så fall säkerhetskopiera mindre ofta är en alternativ tooreduce hello kostnad.

### <a name="scenario-4-iaas-application-data-issues"></a>Scenario 4: IaaS data programproblem

Du har ett program som beräknar, underhåller och levererar kritiska affärsdata, till exempel information om priser. En ny version av programmet hade ett programfel för programvara som felaktigt beräknade hello priser och skadad hello befintliga commerce data som hanteras av hello-plattformen. Här hello bästa erhåller skulle vara toorevert toohello tidigare version av programmet hello och hello data första. tooenable detta, vidta periodiska säkerhetskopieringar av systemet.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Lösning för katastrofåterställning: Azure Backup-tjänsten

[Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/) är kan användas för säkerhetskopiering och Katastrofåterställning och den fungerar med [hanterade diskar](../../virtual-machines/windows/managed-disks-overview.md) samt [ohanterade diskar](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). Du kan skapa en säkerhetskopiering med tidsbaserade säkerhetskopieringar, enkelt VM-återställning och säkerhetskopiering bevarandeprinciper. 

Om du använder [Premium lagringsdiskar](storage-premium-storage.md), [hanterade diskar](../../virtual-machines/windows/managed-disks-overview.md), eller andra disktyper med hello [lokalt redundant lagring (LRS)](storage-redundancy.md#locally-redundant-storage) alternativet, det är särskilt viktigt tooleverage periodiska DR-säkerhetskopieringar. Azure Backup lagras hello data i Recovery Services-valvet för långsiktig kvarhållning. Välj hello [Geo-redundant lagring (GRS)](storage-redundancy.md#geo-redundant-storage) alternativ för hello säkerhetskopiering Recovery Services-valvet. Som säkerställer säkerhetskopior är replikerade tooa olika Azure-region för att skydda från regionala katastrofer.

För [ohanterade diskar](../../virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), du kan använda hello LRS lagringstyp för IaaS-diskar, men kontrollera att Azure Backup är aktiverad med hello GRS alternativ för hello Recovery Services-valvet.

**Om du använder hello [GRS](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) alternativet för ohanterade diskar kan du fortfarande behöver programkonsekventa ögonblicksbilder för säkerhetskopiering och Katastrofåterställning.** Du måste använda antingen [Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/) eller [programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots).

hello följande är en sammanfattning av lösningar för Katastrofåterställning.

| Scenario | Automatisk replikering | Lösning för Katastrofåterställning |
| --- | --- | --- |
| *Premium-lagringsdiskar* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Ohanterad LRS diskar* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Ohanterad GRS diskar* | Mellan region ([GRS](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) |
| *Ohanterad RA-GRS-diskar* | Mellan region ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) |

Hög tillgänglighet kan bäst av uppfylls via hello hanterade diskar i en Tillgänglighetsuppsättning tillsammans med Azure Backup. Om du använder ohanterade diskar, kan du fortfarande använda Azure Backup för Katastrofåterställning. Om du toouse Azure Backup kan sedan ta [programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) enligt beskrivningen i ett senare avsnitt är en alternativ lösning för säkerhetskopiering och Katastrofåterställning.

Dina val för hög tillgänglighet, säkerhetskopiering och Katastrofåterställning på programmet eller infrastrukturen kan representeras som nedan:

| *Nivå* | Hög tillgänglighet   | Säkerhetskopiering / DR |
| --- | --- | --- |
| *Programmet* | SQL Always On | Azure Backup |
| *Infrastruktur*  | Tillgänglighetsuppsättning  | GRS med programkonsekventa ögonblicksbilder |

### <a name="using-hello-azure-backup-service"></a>Med hjälp av hello Azure Backup-tjänsten

[Azure-säkerhetskopiering](../../backup/backup-azure-vms-introduction.md) kan säkerhetskopiera dina virtuella datorer som kör Windows eller Linux toohello Azure Recovery Services-valvet. Säkerhetskopiera och återställa affärskritiska data är komplicerade av hello faktum att verksamhetskritiska data måste säkerhetskopieras när hello program producerar hello data körs. tooaddress, Azure Backup innehåller programkonsekvent säkerhetskopiering för Microsoft-arbetsbelastningar med hello Volume Shadow Service (VSS) tooensure att data skrivs korrekt toostorage. Endast filkonsekventa säkerhetskopior är möjligt för Linux virtuella datorer, eftersom Linux saknar motsvarande tooVSS för funktioner.

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png)   

När Azure Backup initierar ett säkerhetskopieringsjobb hello schemalagda tidpunkt, utlöser hello reservanknytning installerats i hello VM tootake en tidpunkt i ögonblicksbild. En ögonblicksbild tas tillsammans med VSS-tooget en programkonsekvent ögonblicksbild diskars hello hello virtuell dator utan att behöva tooshut den ned. hello säkerhetskopiering tillägget hello VM tömmer alla skrivningar innan du tar hello programkonsekvent ögonblicksbild av alla hello-diskar. När hello ögonblicksbild tas, överförs hello data med Azure Backup toohello säkerhetskopieringsvalvet. toomake hello säkerhetskopieringsprocessen mer effektivt hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan den senaste säkerhetskopieringen av hello.


toorestore, kan du visa hello tillgängliga säkerhetskopieringar via Azure Backup och starta en återställning. Du kan skapa och återställa Azure-säkerhetskopieringar via hello [Azure-portalen](https://portal.azure.com/), [med hjälp av PowerShell](../../backup/backup-azure-vms-automation.md ), eller med hjälp av hello [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install). Mer information finns i Azure Backup.

### <a name="steps-tooenable-backup"></a>Steg tooEnable säkerhetskopiering

hello följande steg är brukar användas tooenable säkerhetskopior av dina virtuella datorer med hjälp av hello [Azure Portal](https://portal.azure.com/). Det blir vissa varianter beroende på exakt scenario. Se toohello [Azure Backup](../../backup/backup-azure-vms-introduction.md) dokumentationen för fullständig information. Azure Backup också [stöder virtuella datorer med hanterade diskar](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Skapa ett recovery services-valv för en virtuell dator med hjälp av hello följande steg:

    a.  Med hjälp av hello [Azure-portalen](https://portal.azure.com/), bläddra alla resurser och hittar ”Recovery Services-valv”.

    b.  På hello Recovery Services-valv menyn, klicka på Lägg till och följ hello steg toocreate ett nytt valv i hello samma region som hello VM. Till exempel om den virtuella datorn finns i USA, västra region, Välj västra USA för hello-valvet.

2.  Kontrollera hello Storage-replikering för hello nyskapad valvet. toodo, åtkomst hello valvet från hello återställningstjänster valv bladet och gå toohello inställningar/säkerhetskopia konfiguration. Kontrollera hello GRS alternativet är markerat som standard. Se till att ditt valv är automatiskt replikerade tooa sekundärt datacenter. Till exempel blir ditt valv i USA, västra automatiskt replikerade tooEast USA.

3.  Konfigurera hello säkerhetskopieringsprincip och välj hello VM hello samma användargränssnitt.

4.  Se till att hello Backup-agenten är installerad på hello VM. Om den virtuella datorn skapas med hjälp av en Azure-galleriet bild, installerat hello Backup-agenten redan. Annars (om du använder en anpassad avbildning), Använd instruktioner för[installera hello VM-agenten på den virtuella datorn hello](../../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Kontrollera att hello VM tillåter nätverksanslutningar för hello säkerhetskopieringstjänsten toofunction. Följ dessa instruktioner för [nätverksanslutningar](../../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  När hello senare steg har slutförts, körs hello säkerhetskopiering med jämna mellanrum enligt hello säkerhetskopieringsprincip. Om det behövs kan du utlösa hello första säkerhetskopieringen manuellt från hello valvet instrumentpanelen på hello Azure-portalen.

För att automatisera Azure Backup med hjälp av skript, se för[PowerShell-cmdletar för säkerhetskopiering](../../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Steg för återställning

Om du behöver toorepair eller återskapa en virtuell dator kan återställa du hello VM från någon av hello Återställningspunkter för säkerhetskopiering i hello-valvet. Det finns ett par olika alternativ för hur du utför hello:

1.  Du kan skapa en ny virtuell dator som en tidpunkt i representation av den virtuella datorn med säkerhetskopierade.

2.  Du kan återställa hello diskar och sedan använda hello mall för hello VM toocustomize och återskapa hello återställas VM. 

Se anvisningar för[Använd Azure portal toorestore virtuella datorer](../../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). hello dokumentet förklarar också hello särskilda åtgärder för att återställa säkerhetskopierade VMs toohello parad datacenter med hjälp av din geo-redundant säkerhetskopieringsvalvet hello gäller en katastrof på hello primära datacenter. I så fall använder Azure Backup hello beräkning tjänst från hello sekundär region toocreate hello återställts virtuell dator.

Du kan också använda PowerShell för [återställa en virtuell dator](../../backup/backup-azure-vms-automation.md#restore-an-azure-vm) eller för [skapar en ny virtuell dator från återställts diskar](../../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternativa lösningen: Programkonsekventa ögonblicksbilder

Om du toouse hello Azure Backup Service kan implementera du ditt eget mekanism för säkerhetskopiering med hjälp av ögonblicksbilder. Det är komplicerat toocreate programkonsekventa ögonblicksbilder för alla diskar som används av en virtuell dator och replikera dessa ögonblicksbilder tooanother region. Därför överväger Azure användning hello Backup-tjänsten ett bättre alternativ än att skapa en anpassad lösning. Om du använder GRS-RA-GRS-lagring för diskar är ögonblicksbilder automatiskt replikerade tooa sekundärt datacenter. Om du använder LRS lagring för diskar, behöver du tooreplicate hello data själv. Mer information finns i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](../../virtual-machines/windows/incremental-snapshots.md).

En ögonblicksbild är en representation av ett objekt vid en viss tidpunkt. En ögonblicksbild påförs fakturering för hello inkrementell storleken på hello data den innehåller. Mer information finns i [skapa en ögonblicksbild av blob](../blobs/storage-blob-snapshots.md).

### <a name="creating-snapshots-while-hello-vm-is-running"></a>Skapa ögonblicksbilder när hello körs virtuella datorn

Medan du kan ta en ögonblicksbild när som helst om hello virtuella datorn körs, fortfarande är data som strömmas toohello diskar och hello ögonblicksbilder kan innehålla delvis åtgärder som rör sig. Även om det finns flera diskar, kan hello ögonblicksbilder av olika diskar ha uppstått vid olika tidpunkter, vilket innebär att dessa ögonblicksbilder inte kan samordnas. Detta är särskilt problematiskt för stripe-volymer vars filer kan skadas om ändringar som gjordes under säkerhetskopieringen.

tooavoid i den här situationen hello säkerhetskopieringen måste implementera hello följande steg:

1.  Låsa alla hello-diskar

2.  Rensa alla hello väntande skrivningar

3.  Sedan [skapa en ögonblicksbild av blob](../blobs/storage-blob-snapshots.md) för alla hello diskar

Vissa Windows-program som SQL Server tillhandahåller en mekanism för samordnade säkerhetskopiering via VSS toocreate programkonsekvent säkerhetskopiering. Du kan använda ett verktyg som fsfreeze för att samordna hello-diskar, som ger filkonsekventa säkerhetskopieringar, men inte programkonsekventa ögonblicksbilder i Linux. Den här processen är komplicerad, så och du bör använda [Azure Backup](../../backup/backup-azure-vms-introduction.md) eller en lösning för säkerhetskopiering från tredje part som redan implementerar det här.

hello ovan processen resulterar i en samling samordnade ögonblicksbilder för alla hello Virtuella diskar, som representerar en viss tidpunkt i vy över hello VM. Det här är en återställningspunkt för säkerhetskopiering för hello VM. Du kan upprepa processen hello vid schemalagda intervall toocreate periodiska säkerhetskopieringar. Nedan finns hello steg för[kopiera hello säkerhetskopieringar tooanother region](#copy-the-snapshots-to-another-region) för Katastrofåterställning.

### <a name="creating-snapshots-while-hello-vm-is-offline"></a>Att skapa ögonblicksbilder när hello är VM offline

Ett annat alternativ toocreate konsekvent säkerhetskopiering stängs av hello VM och blob ta ögonblicksbilder av varje disk. Detta är enklare än koordinerande ögonblicksbilder av en aktiv virtuell dator, men det krävs några minuter stillestånd. Följ dessa steg för den här processen:

1. Stäng hello VM.

2. Skapa en ögonblicksbild av varje VHD-blobben som tar bara några sekunder.

    toocreate en ögonblicksbild som du kan använda [PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), hello [Azure Storage Rest API](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install), eller en av hello Azure Storage, Klientbiblioteken som [ hello Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Starta hello VM, vilket avslutar hello driftstopp. Vanligtvis hello hela processen har slutförts inom några minuter.

Den här processen ger en uppsättning programkonsekventa ögonblicksbilder för alla hello-diskar, vilket ger en återställningspunkt för säkerhetskopiering för hello VM. Nedan visas hello steg toocopy hello ögonblicksbilder tooanother region för Katastrofåterställning.

### <a name="copy-hello-snapshots-tooanother-region"></a>Kopiera hello ögonblicksbilder tooanother region

Genereringen av hello ögonblicksbilder enbart räcker inte för Katastrofåterställning. Du måste också replikera hello ögonblicksbild säkerhetskopieringar tooanother region.

Om du använder GRS eller RA-GRS för diskarna sedan hello ögonblicksbilder är replikerade toohello sekundär region automatiskt. Det kan finnas några minuter på fördröjningen före hello replikering och om hello primära Datacenter kraschar innan hello ögonblicksbilder Slutför replikerar du kommer inte att kunna tooaccess hello ögonblicksbilder från hello sekundärt datacenter. hello sannolikheten för det här är liten.

> [!Note] 
> Endast med hello diskar i ett GRS eller RA-GRS-konto skyddar inte hello VM från katastrofer. Du måste också skapa samordnade ögonblicksbilder eller använda Azure Backup. Detta är obligatorisk toorecover ett konsekvent tillstånd för VM-tooa.

Om du använder LRS, måste du kopiera hello ögonblicksbilder tooa olika storage-konto omedelbart efter att skapa hello ögonblicksbilder. hello kopiera mål kan vara ett LRS-lagringskonto i en annan region, vilket resulterar i hello kopia som i en fjärransluten region. Du kan också kopiera hello ögonblicksbild tooan RA-GRS-lagringskonto i hello samma region. I det här fallet blir hello ögonblicksbild lazy replikerade toohello remote sekundär region. Säkerhetskopian är skyddat från katastrofer på hello primär plats när hello kopiera och replikeringen är klar.

toocopy din inkrementell ögonblicksbilder för DR effektivt, granska hello instruktionerna i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](../../virtual-machines/windows/incremental-snapshots.md).

![](./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png)   

### <a name="recovery-from-snapshots"></a>Återställning från ögonblicksbilder

tooretrieve en ögonblicksbild, kopiera den toomake en ny blob. Om du kopierar hello ögonblicksbild från hello primära konto, kan du kopiera hello ögonblicksbild över toohello grundläggande blob hello ögonblicksbilder, vilket Återför hello disk toohello ögonblicksbild; Detta kallas befordrar hello ögonblicksbild. Om du kopierar hello säkerhetskopian från ett sekundärt konto (i hello skiftläget för RA-GRS), måste den vara kopieras tooa primära konto. Du kan kopiera en ögonblicksbild [med hjälp av PowerShell](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) eller med hello AzCopy-verktyget. Mer information finns i [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).

Hello gäller virtuella datorer med flera diskar, måste du kopiera alla hello ögonblicksbilder som ingår i samma koordineras hello återställningspunkt. När du kopierar hello ögonblicksbilder toowritable VHD-blobbar, kan du använda hello blobbar toorecreate den virtuella datorn med hjälp av hello mall för hello VM.

## <a name="other-options"></a>Andra alternativ

### <a name="sql-server"></a>SQL Server

SQL Server som körs på en virtuell dator har sin egen inbyggda funktioner toobackup Blob storage för tooAzure din SQL Server-databas eller en filresurs. Om hello storage-konto är GRS eller RA-GRS, kan du använda dessa säkerhetskopieringar i hello lagringskontots sekundära datacenter i hello händelsen för katastrofåterställning, med hello samma begränsningar som beskrivits tidigare. Mer information finns i [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](../../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). I tillägg toobackup och återställning, [SQL Server alltid på Tillgänglighetsgrupper](../../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) kan upprätthålla sekundära repliker för databaser toogreatly minska tiden för hello disaster recovery.

## <a name="other-considerations"></a>Andra överväganden

Den här artikeln har beskrivs hur toobackup eller ta ögonblicksbilder av dina virtuella datorer och deras diskar toosupport katastrofåterställning, och hur toouse dessa toorecover. Med hello Azure Resource Manager-modellen, flera personer använder mallar toocreate deras virtuella datorer och annan infrastruktur i Azure. Du kan använda en mall toocreate en virtuell dator som har hello samma konfiguration varje gång. Om du använder anpassade avbildningar för att skapa din virtuella dator du måste också kontrollera bilderna skyddas med hjälp av en RA-GRS-konto toostore dem.

Säkerhetskopieringsprocessen kan därför vara en kombination av två saker:

1. Säkerhetskopiering hello data (diskar)
2. Säkerhetskopiering hello-konfiguration (mallar, anpassade avbildningar)

Beroende på hello säkerhetskopiering alternativ du väljer du kanske toohandle hello säkerhetskopiering av både hello data och hello konfiguration eller hello säkerhetskopieringstjänsten kan hantera all som du.

## <a name="appendix---understanding-hello-impact-of-lrs-grs-and-ra-grs"></a>Tillägg – förstå hur hello effekten av LRS-, GRS- och RA-GRS

Det finns tre typer av dataredundans som du bör tänka på avseende katastrofåterställning – lokalt redundant (LRS), geo-redundant (GRS), för storage-konton i Azure, eller geo-redundant med läsa åtkomst (RA-GRS). 

Lokalt Redundant lagring (LRS) behåller tre kopior av hello data i hello samma datacenter. När du skriver hello data uppdateras alla tre kopior innan lyckas returneras toohello anroparen så att du vet att de är identiska. Disken är skyddat från lokala fel eftersom det är mycket troligt att alla tre kopior skulle påverkas vid hello samtidigt. Hello gäller LRS finns det ingen geo-redundans, hello disk inte skyddas från katastrofalt fel som kan påverka en hela data center eller lagring enhet.

Med GRS och RA-GRS tre kopior av dina data finns kvar i hello primär region (valt) och tre fler kopior av dina data bevaras i en motsvarande sekundär region (anges av Azure). Om du lagrar data i USA, västra kommer hello data att replikerade tooEast USA. Detta görs asynkront, och det finns en kort fördröjning mellan uppdateringar toohello primära och sekundära. Repliker hello diskar på hello sekundär plats är konsekvent på grundval av per disk (med hello fördröjning) men repliker av flera aktiva diskar är inte synkroniserade **med varandra**. toohave konsekvent repliker över flera diskar, programkonsekventa ögonblicksbilder behövs.

hello största skillnaden mellan GRS och RA-GRS är med RA-GRS, du kan läsa hello sekundär kopia när som helst. Om det finns ett problem som återger hello data i hello primära region som är tillgänglig, gör hello Azure-teamet varje arbete toorestore åtkomst. Medan primära hello är nere, om du har aktiverat RA-GRS kan du komma åt hello data i hello sekundärt datacenter. Därför, om du planerar tooread från hello repliken när hello primära inte är tillgänglig, sedan RA-GRS bör övervägas.

Om det visar sig toobe betydande avbrott, kan hello Azure-teamet utlösa en geo-redundans och ändra hello primära DNS-poster toopoint toosecondary lagring. Om du har antingen GRS eller RA-GRS aktiverad måste använder du nu hello data i hello region som används för toobe hello sekundär. Med andra ord, om ditt lagringskonto är GRS och det finns ett problem, du kan komma åt hello sekundär lagring om det finns en geo-redundans.

Mer information finns i [vilka toodo om ett Azure Storage-avbrott inträffar](storage-disaster-recovery-guidance.md). 

Observera att Microsoft kontrollerar om det uppstår redundans. Redundans inte styrs per lagringskonto, därför fattas inte av enskilda kunder. tooimplement katastrofåterställning för specifika storage-konton eller virtuella diskar, måste du använda hello tekniker som beskrivs tidigare i den här artikeln.
