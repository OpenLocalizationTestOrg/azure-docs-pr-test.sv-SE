---
title: "Säkerhetskopiering och katastrofåterställning återställning för Azure IaaS-diskar | Microsoft Docs"
description: "I den här artikeln förklarar vi hur du planerar för säkerhetskopiering och Disaster Recovery (DR) för IaaS-virtuella maskiner (VMs) och diskarna i Azure. Det här dokumentet beskriver både hanterade och ohanterade diskar"
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
ms.openlocfilehash: 0de675e46dbd527148ac5fedcd35d81962adfb7e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Säkerhetskopiering och katastrofåterställning återställning för Azure IaaS-diskar

I den här artikeln förklarar vi hur du planerar för säkerhetskopiering och Disaster Recovery (DR) för IaaS-virtuella maskiner (VMs) och diskarna i Azure. Det här dokumentet beskriver både hanterade och ohanterade diskar.

Först ska vi tala om de inbyggda feltolerans i Azure-plattformen som bidrar till att skydda dig mot lokala fel. Sedan diskuterar vi katastrofåterställning-scenarier som inte omfattas av inbyggda funktioner som är hjälpavsnittet beskrivs i det här dokumentet. Dessutom lär flera exempel på arbetsbelastningsscenarier där olika överväganden för säkerhetskopiering och Katastrofåterställning kan tillkomma. Vi ska granska möjliga lösningar för Katastrofåterställning för IaaS-diskar. 

## <a name="introduction"></a>Introduktion

Azure-plattformen använder olika metoder för redundans och feltolerans för att skydda kunder från lokaliserade maskinvarufel som kan uppstå. Lokala fel kan omfatta problem med en Azure storage server-dator på servern som innehåller en del av data för en virtuell disk eller fel i en SSD och Hårddisk. Sådana isolerade maskinvarufel för komponenten kan inträffa under normal drift och plattformen är avsedd att vara motståndskraftiga mot dessa fel. Större katastrofer kan resultera i fel eller inaccessibility av stora mängder lagringsservrar eller ett helt datacenter. Medan dina virtuella datorer och diskar skyddas normalt från lokaliserade fel, krävs ytterligare steg för att skydda din arbetsbelastning från region hela katastrofalt fel (till exempel en större katastrof) kan påverka den virtuella datorn och diskar.

Utöver möjligheten att plattform fel kan uppstå problem med kunden program eller data. En ny version av programmet kan till exempel oavsiktligen gör en ny ändra till data. I så fall kanske du vill återställa programmet och data till en tidigare version som innehåller det senaste fungerande tillståndet. Detta kräver att underhålla regelbundna säkerhetskopieringar.

För regional katastrofåterställning, måste du säkerhetskopiera din IaaS-VM-diskarna till en annan region. 

Innan vi tittar på alternativ för säkerhetskopiering och Katastrofåterställning Sammanfattningsvis vi några metoder för att hantera lokaliserade fel.

### <a name="azure-iaas-resiliency"></a>Azure IaaS återhämtning

*Återhämtning* refererar till tolerans för vanliga fel som uppstår i maskinvarukomponenter. Återhämtning är möjligheten att återställa från fel och fortsätter att fungera. Det är inte om att undvika fel, men svarar på fel i ett sätt som förhindrar avbrott eller dataförlust. Målet med återhämtning är att returnera programmet till en fullt fungerande tillstånd efter fel. Azure virtuella datorer och diskar som är avsedda att vara motståndskraftiga mot vanliga maskinvarufel. Låt oss titta på hur Azure IaaS-plattformen ger den här återhämtning.

En virtuell dator består i huvudsak av två delar: (1) A compute-servern och (2) beständiga diskarna. Både påverkar feltolerans för en virtuell dator.

Om Azure compute värdservern som innehåller den virtuella datorn får ett maskinvarufel (som är ovanligt), för Azure att automatiskt återställa den virtuella datorn på en annan server. Om det händer kan du får en omstart och den virtuella datorn kommer att säkerhetskopiera en stund. Azure automatiskt identifierar sådana maskinvarufel och kör återställningar för att säkerställa kunden VM blir tillgängliga så snart som möjligt.

Om IaaS-diskar är hållbarhet av data viktigt för beständig lagring-plattformen. Azure-kunder har viktiga business-program körs med IaaS och de är beroende av beständiga data. Azure Designer skydd för dessa IaaS-diskar med tre redundanta kopior av data som lagras lokalt, ger hög hållbarhet mot lokala fel. Om en av de maskinvarukomponenter som innehåller disken misslyckas påverkas inte den virtuella datorn eftersom det finns två ytterligare kopior som stöd för disk-begäranden. Den fungerar även om två olika maskinvarukomponenter som stöd för en disk som inte samtidigt (som skulle vara mycket sällsynt). För att säkerställa att vi alltid har tre repliker, skapas Azure Storage-tjänsten automatiskt en ny kopia av data i bakgrunden om en av de tre kopiorna är tillgänglig. Det bör därför inte nödvändigt att använda RAID med Azure-diskar för feltolerans. En enkel RAID 0-konfigurationen ska vara tillräcklig för striping diskar om det behövs för att skapa större volymer.

På grund av den här arkitekturen **Azure har konsekvent levereras företagsklass hållbarhet för IaaS-diskar, med ett branschledande noll % [medel Felintervall](https://en.wikipedia.org/wiki/Annualized_failure_rate).**

Lokaliserade maskinvarufel på beräknade värd eller i plattformen lagring kan ibland resultatet i tillfällig otillgänglighet för den virtuella datorn som omfattas av den [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines/) för VM-tillgänglighet. Azure tillhandahåller också ett branschledande serviceavtal för enskild VM-instanser som använder Premiumlagring diskar.

Kunder kan använda för att skydda arbetsbelastningar för program från avbrott på grund av temporär otillgänglighet en disk eller VM [Tillgänglighetsuppsättningar](../virtual-machines/windows/manage-availability.md). Två eller flera virtuella datorer i en tillgänglighetsuppsättning ger redundans för programmet. Azure skapar sedan dessa virtuella datorer och diskar i separata feldomäner med olika ström, nätverk och server-komponenter. Därför påverkar lokaliserade maskinvarufel vanligtvis inte flera virtuella datorer i uppsättningen samtidigt, tillhandahåller hög tillgänglighet för ditt program. Den anses vara en bra idé att använda tillgänglighetsuppsättningar när hög tillgänglighet krävs. Mer information finns i Disaster Recovery-aspekter som beskrivs nedan.

### <a name="backup-and-disaster-recovery"></a>Säkerhetskopiering och katastrofåterställning

Katastrofåterställning (DR) är möjligheten att återställa från sällsynt men större incidenter: uppstod, storskaligt fel, till exempel avbrott i tjänsten som påverkar en hel region. Katastrofåterställning innehåller säkerhetskopiering och arkivering och kan innehålla manuella ingripanden, till exempel återställa en databas från en säkerhetskopia.

Plattformen Azure inbyggt skydd mot fel på lokaliserade skyddar kanske inte helt virtuella datorer/diskar vid större katastrofer vilket kan orsaka stora avbrott. Detta inkluderar katastrofer, till exempel ett datacenter som träffas av en orkan jordbävning, fire eller stor skala enhet maskinvarufel. Dessutom kan det uppstå fel som beror på programmet eller problem.

För att skydda dina IaaS-arbetsbelastningar från avbrott, bör du planera för redundans och att aktivera återställning av säkerhetskopior. Du bör planera redundans och säkerhetskopiering på en annan geografisk plats från den primära platsen för katastrofåterställning. På så sätt försäkrar du dig säkerhetskopiorna inte påverkas av samma händelse som ursprungligen påverkas VM eller diskar. Mer information finns i [katastrofåterställning för program som bygger på Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

DR-överväganden kan inkludera följande aspekter:

1. Hög tillgänglighet är möjligheten för att fortsätta köras i ett felfritt tillstånd utan betydande driftavbrott. Med ”felfritt tillstånd”, menar vi programmet svarar och användare kan ansluta till programmet och interagera med den. Vissa verksamhetskritiska program och databaser kan krävas ska vara tillgängliga alltid, även om det finns fel i plattformen. Du kan behöva planera redundans för programmet, samt data för dessa arbetsbelastningar.

2. Data hållbarhet: I vissa fall kan den huvudsakliga ersättningen är att säkerställa att dina data bevaras vid en katastrof. Du måste därför kan en säkerhetskopia av dina data i en annan plats. För dessa arbetsbelastningar, kanske du inte behöver fullständig redundans för programmet, men en regelbunden säkerhetskopiering av diskar.

## <a name="backup-and-dr-scenarios"></a>Säkerhetskopiering och Katastrofåterställning scenarier

Nu ska vi titta på några exempel på arbetsbelastning Programscenarier och överväganden för planering av Disaster Recovery.

### <a name="scenario-1-major-database-solutions"></a>Scenario 1: Större databas lösningar

Överväg att en produktionsserver databasen som SQL Server eller Oracle som har stöd för hög tillgänglighet. Kritiska produktionsprogram och användare beror på den här databasen. Plan för katastrofåterställning för det här systemet kan behöva stöd för följande krav:

1. Data måste vara skyddade och återställas.
2.  Servern måste vara tillgängliga för användning.

Du kan behöva underhålla en replik av databasen i en annan region som en säkerhetskopia. Beroende på kraven för servertillgänglighet och återställning av data kan lösningen röra sig om en plats för aktiv-aktiv eller aktivt-passivt replik till periodiska offline säkerhetskopior av data. Relationsdatabaser, till exempel SQL Server och Oracle ger olika alternativ för replikering. För SQL Server [SQL Server alltid på Tillgänglighetsgrupper](https://msdn.microsoft.com/library/hh510230.aspx) kan användas för hög tillgänglighet.

Det stöder också NoSQL-databaser som MongoDB [repliker](https://docs.mongodb.com/manual/replication/) för redundans. Du kan använda repliker för hög tillgänglighet.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scenario 2: Ett kluster med redundanta virtuella datorer

Överväg att en arbetsbelastning hanteras av ett kluster för virtuella datorer som ger redundans och belastningsutjämning. Ett exempel är ett Cassandra kluster som distribueras i en region. Den här typen av arkitekturen innehåller redan en hög nivå av redundans i detta område. Men om du vill skydda belastningen från ett regionalt nivån fel bör du sprida klustret mellan två regioner och regelbundna säkerhetskopieringar till en annan region.

### <a name="scenario-3-iaas-application-workload"></a>Scenario 3: IaaS programmet arbetsbelastning

Detta kan vara en produktionsögonblicksbild vid arbetsbelastningar som körs på en virtuell dator i Azure. Det kan till exempel vara en webbserver eller filserver håller innehållet och andra resurser för en plats. Det kan också vara ett specialbyggt affärsprogram som körs på en virtuell dator som lagras dess data, resurser och programtillstånd på VM-diskarna. I det här fallet är det viktigt att se till att du kan göra säkerhetskopior med jämna mellanrum. Frekvens för säkerhetskopiering ska baseras på typ av VM-arbetsbelastning. Till exempel om programmet körs varje dag och ändrar data, bör sedan säkerhetskopieringen tas varje timme.

Ett annat exempel är en rapportserver som hämtar data från andra källor och genererar aggregerade rapporter. Förlust av den här virtuella datorn eller diskar kommer att leda till förlust av rapporterna. Det kan dock vara möjligt att köra rapporterna och återskapa utdata. I så fall kan har du inte verkligen förlust av data även om rapportservern är träffa med katastrofåterställning, så du kan ha en högre nivå av feltolerans för att förlora en del av data på rapporteringsservern. I så fall mindre ofta återkommande säkerhetskopieringar är ett alternativ för att minska kostnaderna.

### <a name="scenario-4-iaas-application-data-issues"></a>Scenario 4: IaaS data programproblem

Du har ett program som beräknar, underhåller och levererar kritiska affärsdata, till exempel information om priser. En ny version av programmet hade ett programfel för programvara som felaktigt beräknade prissättning och skadad befintliga commerce data som hanteras av plattformen. Här är att bästa loppet av åtgärden återgå till den tidigare versionen av programmet och data som först. Om du vill aktivera det här ta periodiska säkerhetskopieringar av systemet.

## <a name="disaster-recovery-solution-azure-backup-service"></a>Lösning för katastrofåterställning: Azure Backup-tjänsten

[Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/) är kan användas för säkerhetskopiering och Katastrofåterställning och den fungerar med [hanterade diskar](storage-managed-disks-overview.md) samt [ohanterade diskar](storage-about-disks-and-vhds-windows.md#unmanaged-disks). Du kan skapa en säkerhetskopiering med tidsbaserade säkerhetskopieringar, enkelt VM-återställning och säkerhetskopiering bevarandeprinciper. 

Om du använder [Premium lagringsdiskar](storage-premium-storage.md), [hanterade diskar](storage-managed-disks-overview.md), eller andra disktyper med den [lokalt redundant lagring (LRS)](storage-redundancy.md#locally-redundant-storage) är särskilt viktigt att utnyttja periodiska DR-säkerhetskopieringar. Azure-säkerhetskopiering lagrar data i Recovery Services-valvet för långsiktig kvarhållning. Välj den [Geo-redundant lagring (GRS)](storage-redundancy.md#geo-redundant-storage) alternativ för säkerhetskopiering Recovery Services-valvet. Som säkerställer säkerhetskopieringar som replikeras till en annan Azure-region för att skydda från regionala katastrofer.

För [ohanterade diskar](storage-about-disks-and-vhds-windows.md#unmanaged-disks), du kan använda LRS lagringstypen för IaaS-diskar, men kontrollera att Azure Backup har aktiverats med alternativet GRS för Recovery Services-valvet.

**Om du använder den [GRS](storage-redundancy.md#geo-redundant-storage)/[RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage) alternativet för ohanterade diskar kan du fortfarande behöver programkonsekventa ögonblicksbilder för säkerhetskopiering och Katastrofåterställning.** Du måste använda antingen [Azure Backup-tjänsten](https://azure.microsoft.com/services/backup/) eller [programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots).

Följande är en översikt över lösningar för Katastrofåterställning.

| Scenario | Automatisk replikering | Lösning för Katastrofåterställning |
| --- | --- | --- |
| *Premium-lagringsdiskar* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Managed Disks* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Ohanterad LRS diskar* | Lokala ([LRS](storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| *Ohanterad GRS diskar* | Mellan region ([GRS](storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) |
| *Ohanterad RA-GRS-diskar* | Mellan region ([RA-GRS](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) |

Hög tillgänglighet kan bäst av uppfylls med hjälp av hanterade diskar i en Tillgänglighetsuppsättning tillsammans med Azure Backup. Om du använder ohanterade diskar, kan du fortfarande använda Azure Backup för Katastrofåterställning. Om det inte går att använda Azure Backup kan sedan ta [programkonsekventa ögonblicksbilder](#alternative-solution-consistent-snapshots) enligt beskrivningen i ett senare avsnitt är en alternativ lösning för säkerhetskopiering och Katastrofåterställning.

Dina val för hög tillgänglighet, säkerhetskopiering och Katastrofåterställning på programmet eller infrastrukturen kan representeras som nedan:

| *Nivå* | Hög tillgänglighet   | Säkerhetskopiering / DR |
| --- | --- | --- |
| *Programmet* | SQL Always On | Azure Backup |
| *Infrastruktur*  | Tillgänglighetsuppsättning  | GRS med programkonsekventa ögonblicksbilder |

### <a name="using-the-azure-backup-service"></a>Med hjälp av Azure Backup-tjänsten

[Azure-säkerhetskopiering](../backup/backup-azure-vms-introduction.md) kan säkerhetskopiera dina virtuella datorer som kör Windows eller Linux till Azure Recovery Services-valvet. Säkerhetskopiera och återställa verksamhetskritiska data är komplicerade av att verksamhetskritiska data måste säkerhetskopieras när de program som ger data körs. För att lösa det ger Azure Backup programkonsekvent säkerhetskopiering för Microsoft-arbetsbelastningar med hjälp av Volume Shadow Service (VSS) så att data skrivs korrekt till lagring. För Linux virtuella datorer kan är endast filkonsekventa säkerhetskopior möjligt eftersom Linux inte har funktioner som motsvarar VSS.

![][1]

När Azure Backup initierar en säkerhetskopiering på den schemalagda tiden, utlöser reservanknytning installerad på den virtuella datorn att ta en ögonblicksbild i tidpunkt. En ögonblicksbild tas tillsammans med VSS att hämta en programkonsekvent ögonblicksbild diskar i den virtuella datorn utan att behöva stänga av. Säkerhetskopiering tillägget på den virtuella datorn tömmer alla skrivningar innan du tar programkonsekvent ögonblicksbild av alla diskar. När ögonblicksbilden tas data har överförts av Azure Backup till säkerhetskopieringsvalvet. Om du vill att säkerhetskopieringen effektivare tjänsten identifierar och överför endast block av data som har ändrats sedan den senaste säkerhetskopieringen.


Om du vill återställa kan du visa tillgängliga säkerhetskopieringar via Azure Backup och startar sedan en återställning. Du kan skapa och återställa Azure-säkerhetskopieringar via den [Azure-portalen](https://portal.azure.com/), [med hjälp av PowerShell](../backup/backup-azure-vms-automation.md ), eller med hjälp av den [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install). Mer information finns i Azure Backup.

### <a name="steps-to-enable-backup"></a>Steg för att aktivera säkerhetskopiering

Följande steg används vanligtvis för att aktivera säkerhetskopiering av dina virtuella datorer med hjälp av den [Azure Portal](https://portal.azure.com/). Det blir vissa varianter beroende på exakt scenario. Referera till den [Azure Backup](../backup/backup-azure-vms-introduction.md) dokumentationen för fullständig information. Azure Backup också [stöder virtuella datorer med hanterade diskar](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Skapa ett recovery services-valv för en virtuell dator med följande steg:

    a.  Med hjälp av den [Azure-portalen](https://portal.azure.com/), bläddra alla resurser och hittar ”Recovery Services-valv”.

    b.  Klicka på Lägg till och följ stegen för att skapa ett nytt valv i samma region som den virtuella datorn på menyn Recovery Services-valv. Till exempel om den virtuella datorn finns i USA, västra region, Välj västra USA för valvet.

2.  Kontrollera Storage-replikering för nyskapade valvet. Om du vill göra detta åt valvet från bladet Recovery Services-valv och gå till inställningar eller säkerhetskopiering konfiguration. Kontrollera GRS-alternativet är markerat som standard. Se till att ditt valv replikeras automatiskt till ett sekundärt datacenter. Till exempel replikeras ditt valv i USA, västra automatiskt till östra USA.

3.  Konfigurera principen för säkerhetskopiering och välj den virtuella datorn från samma användargränssnitt.

4.  Kontrollera att Backup-agenten är installerad på den virtuella datorn. Om den virtuella datorn skapas med hjälp av en Azure-galleriet bild, installerat Backup-agenten redan. Annars (om du använder en anpassad avbildning), använder du instruktionerna för att [installera VM-agenten på den virtuella datorn](../backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Kontrollera att den virtuella datorn tillåter nätverksanslutningar för säkerhetskopieringstjänsten ska fungera. Följ dessa instruktioner för [nätverksanslutningar](../backup/backup-azure-arm-vms-prepare.md#network-connectivity).

6.  När ovanstående steg har slutförts, körs säkerhetskopieringen med regelbundna intervall som anges i principen för säkerhetskopiering. Om det behövs kan du utlösa den första säkerhetskopieringen manuellt från valvet instrumentpanelen på Azure-portalen.

För att automatisera Azure Backup med hjälp av skript, referera till [PowerShell-cmdletar för säkerhetskopiering](../backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Steg för återställning

Om du behöver reparera eller återskapa en virtuell dator kan du återställa den virtuella datorn från någon av återställningspunkter för säkerhetskopiering i valvet. Det finns ett par olika alternativ för att utföra återställningen:

1.  Du kan skapa en ny virtuell dator som en tidpunkt i representation av den virtuella datorn med säkerhetskopierade.

2.  Du kan återställa diskarna och sedan använda mallen för den virtuella datorn för att anpassa och återskapa den återställda virtuella datorn. 

Se anvisningar för att [Använd Azure-portalen för att återställa virtuella datorer](../backup/backup-azure-arm-restore-vms.md#restoring-a-vm-during-azure-datacenter-disaster). Dokumentet beskriver även hur för återställning av säkerhetskopierade virtuella datorer till parad datacenter med hjälp av din geo-redundant säkerhetskopieringsvalvet vid en katastrof på primära datacenter. I så fall använder Azure Backup beräknings-tjänsten från den sekundära regionen för att skapa den återställda virtuella datorn.

Du kan också använda PowerShell för [återställa en virtuell dator](../backup/backup-azure-vms-automation.md#restore-an-azure-vm) eller för [skapar en ny virtuell dator från återställts diskar](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternativa lösningen: Programkonsekventa ögonblicksbilder

Om det inte går att använda Azure Backup Service kan implementera du din egen mekanism för säkerhetskopiering med hjälp av ögonblicksbilder. Det är svårt att skapa programkonsekventa ögonblicksbilder för alla diskar som används av en virtuell dator och replikerar sedan dessa ögonblicksbilder till en annan region. Därför överväger Azure utnyttja Backup-tjänsten ett bättre alternativ än att skapa en anpassad lösning. Om du använder GRS-RA-GRS-lagring för diskar, replikeras ögonblicksbilder automatiskt till ett sekundärt datacenter. Om du använder LRS lagring för diskar måste replikera data själv. Mer information finns i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](storage-incremental-snapshots.md).

En ögonblicksbild är en representation av ett objekt vid en viss tidpunkt. En ögonblicksbild påförs fakturering för inkrementell storleken på data den innehåller. Mer information finns i [skapa en ögonblicksbild av blob](storage-blob-snapshots.md).

### <a name="creating-snapshots-while-the-vm-is-running"></a>Skapa ögonblicksbilder medan den virtuella datorn körs

Medan du kan ta en ögonblicksbild när som helst om den virtuella datorn körs, fortfarande är data som strömmas till diskar och ögonblicksbilder kan innehålla delvis åtgärder som rör sig. Även om det finns flera diskar, kan ögonblicksbilder av olika diskar ha uppstått vid olika tidpunkter, vilket innebär att dessa ögonblicksbilder inte kan samordnas. Detta är särskilt problematiskt för stripe-volymer vars filer kan skadas om ändringar som gjordes under säkerhetskopieringen.

Om du vill undvika detta genomföra säkerhetskopieringsprocessen följande steg:

1.  Låsa alla diskar

2.  Rensa alla väntande skrivningar

3.  Sedan [skapa en ögonblicksbild av blob](storage-blob-snapshots.md) för alla diskar

Vissa Windows-program som SQL Server innehåller en samordnad säkerhetskopiering funktion via VSS skapa programkonsekventa säkerhetskopior. Du kan använda ett verktyg som fsfreeze för samordning av diskar som kommer att tillhandahålla filkonsekventa säkerhetskopieringar, men inte programkonsekventa ögonblicksbilder i Linux. Den här processen är komplicerad, så och du bör använda [Azure Backup](../backup/backup-azure-vms-introduction.md) eller en lösning för säkerhetskopiering från tredje part som redan implementerar det här.

Ovanstående procedur resulterar i en samling samordnade ögonblicksbilder för alla Virtuella diskar, som representerar en specifik tidpunkt i vy för den virtuella datorn. Det här är en återställningspunkt för säkerhetskopiering för den virtuella datorn. Du kan upprepa processen med schemalagda intervall för att skapa regelbundna säkerhetskopieringar. Nedan följer stegen för att [kopiera säkerhetskopieringar till en annan region](#copy-the-snapshots-to-another-region) för Katastrofåterställning.

### <a name="creating-snapshots-while-the-vm-is-offline"></a>Skapa ögonblicksbilder medan den virtuella datorn är offline

Ett annat alternativ för att skapa konsekvent säkerhetskopior stänger av den virtuella datorn och tar blob ögonblicksbilder av varje disk. Detta är enklare än koordinerande ögonblicksbilder av en aktiv virtuell dator, men det krävs några minuter stillestånd. Följ dessa steg för den här processen:

1. Stäng av den virtuella datorn.

2. Skapa en ögonblicksbild av varje VHD-blobben som tar bara några sekunder.

    Du kan använda för att skapa en ögonblicksbild, [PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blob-snapshots), [Azure Storage Rest API](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install), eller ett Azure Storage-klientbibliotek som [Storage-klientbiblioteket för .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Starta den virtuella datorn, vilket avslutar avbrottstiden. Vanligtvis hela processen har slutförts inom några minuter.

Den här processen ger en uppsättning programkonsekventa ögonblicksbilder för alla diskar, vilket ger en återställningspunkt för säkerhetskopiering för den virtuella datorn. Nedan visas stegen för att kopiera ögonblicksbilder till en annan region för Katastrofåterställning.

### <a name="copy-the-snapshots-to-another-region"></a>Kopiera ögonblicksbilder till en annan region

Skapa ögonblicksbilder enbart räcker inte för Katastrofåterställning. Du måste också replikeras säkerhetskopiorna av ögonblicksbilder till en annan region.

Om du använder GRS eller RA-GRS för diskarna sedan replikeras ögonblicksbilderna till den sekundära regionen automatiskt. Det kan finnas några minuter för fördröjning innan replikeringen och om primära Datacenter kraschar innan ögonblicksbilderna Slutför replikering kan du inte få åtkomst till ögonblicksbilder från den sekundära datacentralen. Sannolikheten för det här är liten.

> [!Note] 
> Bara har diskarna i ett GRS eller RA-GRS-konto skydda inte den virtuella datorn från katastrofer. Du måste också skapa samordnade ögonblicksbilder eller använda Azure Backup. Detta är nödvändigt att återställa en virtuell dator till ett konsekvent tillstånd.

Om du använder LRS, måste du kopiera ögonblicksbilder till ett annat lagringskonto omedelbart efter att skapa ögonblicksbilden. Kopiera mål kan vara ett LRS-lagringskonto i en annan region, vilket resulterar i kopian som i en fjärransluten region. Du kan också kopiera ögonblicksbilden till en RA-GRS-lagringskonto i samma region. I det här fallet replikeras ögonblicksbilden lazy till den fjärranslutna sekundära regionen. Säkerhetskopian är skyddat från katastrofer på den primära platsen när kopieringen och replikeringen är klar.

Om du vill kopiera ditt inkrementell ögonblicksbilder för Katastrofåterställning effektivt läser du anvisningarna i [säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder](storage-incremental-snapshots.md).

![][2]

### <a name="recovery-from-snapshots"></a>Återställning från ögonblicksbilder

Kopiera den för att skapa en ny blob för att hämta en ögonblicksbild. Om du vill kopiera ögonblicksbilden från det primära kontot, kan du kopiera ögonblicksbilden över till ögonblicksbilden grundläggande blob därför återställa disken till ögonblicksbilden; Detta kallas att främja ögonblicksbilden. Om du kopierar ögonblicksbild av säkerhetskopian från ett sekundärt konto (för RA-GRS), måste den kopieras till primära konto. Du kan kopiera en ögonblicksbild [med hjälp av PowerShell](storage-powershell-guide-full.md#how-to-copy-a-snapshot-of-a-blob) eller med hjälp av verktyget AzCopy. Mer information finns i [överföra data med kommandoradsverktyget Azcopy](storage-use-azcopy.md).

För virtuella datorer med flera diskar, måste du kopiera alla ögonblicksbilder som ingår i samma samordnade återställningspunkt. Du kan använda blobar för att återskapa den virtuella datorn med hjälp av mallen för den virtuella datorn när du kopierar ögonblicksbilder till skrivbara VHD-blobbar.

## <a name="other-options"></a>Andra alternativ

### <a name="sql-server"></a>SQL Server

SQL Server som körs på en virtuell dator har sin egen inbyggda funktioner för att säkerhetskopiera SQL Server-databas till Azure Blob-lagring eller en filresurs. Om lagringskontot är GRS eller RA-GRS, kan du komma åt säkerhetskopieringarna i storage-konto sekundärt Datacenter vid katastrofåterställning, med samma begränsningar som beskrivits tidigare. Mer information finns i [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Utöver säkerhetskopiering och återställning, [SQL Server alltid på Tillgänglighetsgrupper](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) kan upprätthålla sekundära repliker av databaser för att avsevärt minska tid som disaster recovery.

## <a name="other-considerations"></a>Andra överväganden

Den här artikeln har beskrivs hur du säkerhetskopierar och ta ögonblicksbilder av dina virtuella datorer och deras diskar för att stödja katastrofåterställning och hur du använder dem för att återställa. Många använder mallar för att skapa deras virtuella datorer och andra infrastruktur i Azure med Azure Resource Manager-modellen. Du kan använda en mall för att skapa en virtuell dator som har samma konfiguration varje gång. Om du använder anpassade avbildningar för att skapa dina virtuella datorer, måste du också kontrollera bilderna skyddas med en RA-GRS-konto för att lagra dem.

Säkerhetskopieringsprocessen kan därför vara en kombination av två saker:

1. Säkerhetskopiera data (diskar)
2. Säkerhetskopiera konfiguration (mallar, anpassade avbildningar)

Beroende på säkerhetskopiering alternativ du väljer du kan behöva hantera säkerhetskopiering av både data och konfiguration eller säkerhetskopieringstjänsten kan hantera all som du.

## <a name="appendix---understanding-the-impact-of-lrs-grs-and-ra-grs"></a>Tillägg – förstå konsekvenserna med LRS-, GRS- och RA-GRS

Det finns tre typer av dataredundans som du bör tänka på avseende katastrofåterställning – lokalt redundant (LRS), geo-redundant (GRS), för storage-konton i Azure, eller geo-redundant med läsa åtkomst (RA-GRS). 

Lokalt behåller Redundant lagring (LRS) tre kopior av data i samma datacenter. När du skriver data uppdateras alla tre kopior innan lyckas returneras till anroparen, så att du vet att de är identiska. Disken är skyddat från lokala fel eftersom det är mycket troligt att det skulle påverkas alla tre kopior på samma gång. LRS, om det finns ingen geo-redundans, så att disken inte är skyddat från katastrofalt fel som kan påverka en hela data center eller lagring enhet.

Med GRS och RA-GRS tre kopior av dina data finns kvar i den primära region (valt) och tre fler kopior av dina data bevaras i en motsvarande sekundär region (anges av Azure). Om du lagrar data i USA, västra kommer data replikeras till östra USA. Detta görs asynkront, och det finns en kort fördröjning mellan uppdateringar till primära och sekundära. Repliker av diskar på den sekundära platsen är konsekvent på grundval av per disk (med fördröjning) men repliker av flera aktiva diskar är inte synkroniserade **med varandra**. Om du vill ha konsekvent repliker över flera diskar, behövs programkonsekventa ögonblicksbilder.

Den största skillnaden mellan GRS och RA-GRS är med RA-GRS, du kan läsa den alternativa kopian när som helst. Om det finns ett problem som återger data i den primära regionen otillgänglig, gör Azure-teamet allt för att återställa åtkomsten. När den primära servern är nere, om du har aktiverat RA-GRS kan du komma åt data i sekundära datacentret. Därför, om du planerar att läsa från repliken när den primära servern inte är tillgänglig, sedan RA-GRS bör övervägas.

Om det visar sig vara en betydande avbrott, kan Azure-teamet utlösa en geo-redundans och ändra de primära DNS-posterna så att den pekar till den sekundära lagringsplatsen. Om du har antingen GRS eller RA-GRS aktiverad, kan du nu komma åt data i den region som används för att vara sekundärt. Med andra ord om ditt lagringskonto är GRS och det finns ett problem, du kan komma åt den sekundära lagringsplatsen endast om det finns en geo-redundans.

Mer information finns i [vad du ska göra om ett Azure Storage-avbrott inträffar](storage-disaster-recovery-guidance.md). 

Observera att Microsoft kontrollerar om det uppstår redundans. Redundans inte styrs per lagringskonto, därför fattas inte av enskilda kunder. Om du vill implementera Haveriberedskap för specifika storage-konton eller virtuella diskar, måste du använda de tekniker som beskrivs tidigare i den här artikeln.



[1]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png