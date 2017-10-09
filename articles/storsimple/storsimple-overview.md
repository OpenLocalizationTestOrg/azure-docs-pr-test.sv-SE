---
title: "aaaStorSimple 8000-serien lösning: översikt | Microsoft Docs"
description: "Beskriver StorSimple skiktning hello enhet, virtuell enhet, tjänster och lagringshantering och beskriver viktiga termer som används i StorSimple."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 0891841186dcd4c46f48d13ed829b98fab7bdf67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000-serien: en hybridlagringslösning för molnet
## <a name="overview"></a>Översikt
Välkommen tooMicrosoft Azure StorSimple, en integrerad lagringslösning som hanterar lagringsuppgifter mellan lokala enheter och Microsoft Azure-molnlagring. StorSimple är en, kostnadseffektiv och enkelt hanterbara area network (SAN) lagringslösning som eliminerar många hello problem och kostnader som är associerade med enterprise lagring och dataskydd. Den använder hello egna StorSimple 8000-serien enheten, kan integreras med molntjänster och innehåller en uppsättning av verktyg för en sömlös vy över alla företagslagring, inklusive lagringsutrymmet i molnet. (hello StorSimple distributionsinformation publiceras på Microsoft Azure-webbplats hello gäller tooStorSimple 8000-serien endast enheter. Om du använder en serieenhet för StorSimple 5000/7000-, gå för[StorSimple hjälp](http://onlinehelp.storsimple.com/).)

StorSimple använder [lagringsnivåer](#automatic-storage-tiering) toomanage lagrade data mellan olika lagringsmedium. hello aktuella arbetsminnet är lagras lokalt på solid state-hårddiskar (SSD) data som används mindre ofta lagras på hårddiskar (HDD) och arkiveringsdata pushas toohello moln. Dessutom StorSimple använder deduplicering och komprimering tooreduce hello mängden lagringsutrymme som hello data förbrukar. Mer information finns för[Deduplicering och komprimering](#deduplication-and-compression). Definitioner av andra viktiga termer och begrepp som används i dokumentationen för hello StorSimple 8000-serien finns för[StorSimple terminologi](#storsimple-terminology) hello slutet av den här artikeln.

Dessutom toostorage management StorSimple Dataskyddsfunktioner aktivera du toocreate på begäran och schemalagda säkerhetskopieringar och sedan lagra dem lokalt eller i hello molnet. Säkerhetskopieringar vidtas i hello form av inkrementell ögonblicksbilder, vilket innebär att de kan skapas och återställs snabbt. Molnögonblicksbilder kan vara ytterst viktigt i katastrofåterställning eftersom de ersätta sekundära lagringssystem (till exempel bandsäkerhetskopiering) och Tillåt toorestore data tooyour datacenter eller tooalternate platser om det behövs.

![ikon för videoklipp](./media/storsimple-overview/video_icon.png) Titta på videon hello för en snabb introduktion tooMicrosoft Azure StorSimple.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>Varför använda StorSimple?
hello beskrivs följande tabell några av hello viktiga fördelar med Microsoft Azure StorSimple.

| Funktion | Fördelar |
| --- | --- |
| Transparent integrering |Använder hello iSCSI-protokollet tooinvisibly länk data lagringsutrymmen. Detta garanterar att data som lagras i hello molnet i hello datacenter, eller så visas toobe som lagras på en plats på fjärrservrar. |
| Minskad lagringskostnader |Allokerar tillräckligt lokala eller aktuella lagringsbehov för toomeet för moln och utökar molnlagring bara när det behövs. Det ytterligare minskar lagringsbehov och utgifter genom att eliminera redundant versioner av hello samma data (deduplicering) och med hjälp av komprimering. |
| Förenklad lagringshantering |Innehåller system administration tools tooconfigure och hantera data som lagras lokalt på en fjärrserver, och i hello molnet. Dessutom kan du hantera säkerhetskopiering och återställa funktioner från en snapin-modulen Microsoft Management Console (MMC).|
| Förbättrad återställning och efterlevnad |Kräver inte utökade återställningstid. I stället återställer data när det behövs. Det innebär att normal drift kan fortsätta med minimala störningar. Dessutom kan du konfigurera principer toospecify säkerhetskopieringsscheman och datalagring. |
| Data mobility |Data överförs tooMicrosoft Azure cloud services kan nås från andra platser för återställning och migrering. Du kan dessutom använda StorSimple tooconfigure StorSimple moln installationer på virtuella datorer (VM) som körs i Microsoft Azure. hello virtuella datorer kan sedan använda virtuella enheter tooaccess lagras data för test- eller återställning. |
| Verksamhetskontinuitet |Tillåter StorSimple 5000 7000-serien användare toomigrate sina data tooa StorSimple 8000-serien enheten. |
| Tillgänglighet i hello Azure Government-portalen |StorSimple finns i hello Azure Government-portalen. Mer information finns i [distribuera din lokala StorSimple-enhet i hello Government Portal](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Dataskydd och tillgänglighet |Hej StorSimple 8000-serien stöder zonen Redundant lagring (ZRS), i tillägg tooLocally Redundant lagring (LRS) och Geo-redundant lagring (GRS). Se för[i den här artikeln på Azure Storage redundansalternativ](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS information. |
| Stöd för kritiska program |StorSimple kan du identifiera volymerna som lokalt Fäst som i sin tur garanterar att data som krävs av kritiska program inte är nivåindelade toohello moln. Lokalt fästa volymer är inte ämne toocloud fördröjningar eller problem med nätverksanslutningen. Mer information om lokalt fästa volymer finns [använda hello StorSimple Enhetshanteraren service toomanage volymer](storsimple-8000-manage-volumes-u2.md). |
| Låg latens och hög prestanda |Du kan skapa moln installationer som utnyttjar hello hög prestanda, låg latens funktioner i Azure premium-lagring. Läs mer om StorSimple premium molnet installationer [distribuera och hantera en enhet med StorSimple moln i Azure](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple-komponenter
hello Microsoft Azure StorSimple-lösningen innehåller hello följande komponenter:

* **Microsoft Azure StorSimple-enhet** – en lokal hybrid-lagringsmatris som innehåller SSD och hårddiskar, tillsammans med funktioner för automatisk redundans och redundanta styrenheter. hello domänkontrollanter hantera lagring skiktning, placera för närvarande används (eller varm) data på lokal lagring (i hello enhet eller lokala servrar), när du flyttar data som används mindre ofta toohello moln.
* **StorSimple-enhet för molnet** – kallas även Hej StorSimple virtuell installation, detta är en programvaruversion av hello StorSimple-enhet som replikerar hello arkitektur och de flesta funktionerna i hello fysiska hybrid lagringsenhet. Hej StorSimple moln installation körs på en enskild nod i en virtuell Azure-dator. Premium virtuella enheter, vilket utnyttja Azure premium-lagring finns tillgängliga i uppdatering 2 och senare.
* **StorSimple Enhetshanteraren service** – en förlängning av hello Azure-portalen där du kan hantera en StorSimple-enhet eller StorSimple moln installation från ett enda webbgränssnitt. Du kan använda hello StorSimple Enhetshanteraren service toocreate och hantera tjänster, visa och hantera enheter, Visa aviseringar, hantera volymer, och visa och hantera principer för säkerhetskopiering och hello säkerhetskopieringskatalogen.
* **Windows PowerShell för StorSimple** – ett kommandoradsgränssnitt som du kan använda toomanage hello StorSimple-enhet. Windows PowerShell för StorSimple har funktioner som gör att du tooregister din StorSimple-enhet, konfigurera hello nätverksgränssnittet på din enhet, installera vissa typer av uppdateringar, felsöka enheten genom att öppna hello stöd sessionen och ändra hello enhetens tillstånd. Du kan komma åt Windows PowerShell för StorSimple av anslutande toohello seriekonsolen eller genom att använda Windows PowerShell-fjärrkommunikation.
* **Azure PowerShell StorSimple-cmdlets** – en samling med Windows PowerShell-cmdlets som gör att du tooautomate servicenivåer och migrering uppgifter från hello-kommandoraden. Mer information om hello Azure PowerShell-cmdlets för StorSimple gå toohello [cmdlet-referens för](/powershell/module/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – en MMC-snapin-modul som använder volymen grupper och hello Windows Volume Shadow Copy Service toogenerate programkonsekvent säkerhetskopiering. Du kan dessutom använda StorSimple Snapshot Manager toocreate scheman för säkerhetskopiering och klona eller återställa volymer.
* **StorSimple-kortet för SharePoint** – ett verktyg som utökar transparent lagring och dataskydd i Microsoft Azure StorSimple tooSharePoint servergrupper när StorSimple lagring kan visas och hanteras från hello SharePoint Central -Administrationsportalen.

hello diagrammet nedan ger en övergripande bild av hello Microsoft Azure StorSimple-arkitektur och komponenter.

![StorSimple-arkitektur](./media/storsimple-overview/overview-big-picture.png)

hello följande avsnitt beskrivs var och en av dessa komponenter i detalj och förklara hur hello lösning ordnar data allokerar lagringsutrymme och underlättar lagringshantering och dataskydd. hello sista avsnittet innehåller definitioner för några av hello viktiga termer och begrepp relaterade tooStorSimple komponenter och deras hantering.

## <a name="storsimple-device"></a>StorSimple-enhet
hello Microsoft Azure StorSimple-enheten är en lokal hybrid-lagringsmatris som innehåller primära lagring och iSCSI-åtkomst toodata lagras på den. Den hanterar kommunikationen med lagringsutrymmet i molnet och hjälper tooensure hello säkerhet och sekretess för alla data som lagras på hello Microsoft Azure StorSimple-lösning.

Hej StorSimple-enheten inkluderar SSD och hårddiskar hårddiskar samt stöd för klustring och automatisk redundans. Den innehåller en delad processor, delad lagring och två speglade styrenheter. Varje styrenhet innehåller hello följande:

* Anslutningen tooa värddatorn
* Konfigurera toosix nätverk portar tooconnect toohello lokalt nätverk (LAN)
* Övervakning av maskinvara
* Icke-obeständigt minne (NVRAM) som bevarar information även om strömmen bryts
* Klustermedveten uppdatering toomanage programuppdateringar på servrar i ett redundanskluster så att hello uppdateringar har minimal eller ingen effekt på tjänstetillgänglighet
* Klustertjänst, som fungerar som en backend-kluster tillhandahåller hög tillgänglighet och minimera de negativa effekterna som kan uppstå om en Hårddisk eller SSD misslyckas eller kopplas från

Endast en domänkontrollant är aktiv när som helst i tid. Om hello aktiva styrenheten misslyckas aktiveras hello andra controller automatiskt.

Mer information finns för[StorSimple status och maskinvarukomponenter](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
Du kan använda StorSimple toocreate en moln-installation som replikerar hello arkitektur och funktioner i hello fysiska hybrid lagringsenhet. Hej StorSimple moln installation (även kallat hello StorSimple virtuell installation) körs på en enskild nod i en virtuell Azure-dator. (En moln-installation kan bara skapas på en virtuell Azure-dator. Du kan inte skapa en på en StorSimple-enhet eller en lokal server.)

hello molnet installation har hello följande funktioner:

* Den fungerar som en fysisk installation och kan erbjuda ett iSCSI gränssnittet toovirtual datorer i hello molnet.
* Du kan skapa ett obegränsat antal installationer i molnet i hello molnet och aktivera dem på och av efter behov.
* Det kan hjälpa dig att simulera lokala miljöer i katastrofåterställning, utveckling och testscenarier och kan hjälpa dig med objektsnivå från säkerhetskopior.

Hej StorSimple moln installation finns i två modeller: hello 8010 enhet (kallades tidigare hello 1100 modellen) och hello 8020 enhet. Hej 8010 enhet har en maximal kapacitet för 30 TB. Hej 8020 enhet som utnyttjar Azure premium-lagring har en maximal kapacitet för 64 TB. (I lokala nivåerna Azure premium storage lagrar data på SSD medan standardlagring lagrar data på hårddiskar.) Observera att du måste ha en Azure premium storage-konto toouse premium-lagring. Mer information om premium-lagring finns för[Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../storage/common/storage-premium-storage.md).

Mer information om hello StorSimple-enhet för molnet går för[distribuera och hantera en enhet med StorSimple moln i Azure](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>StorSimple enheten Manager-tjänsten
Microsoft Azure StorSimple innehåller ett webbaserat användargränssnitt (hello StorSimple Enhetshanteraren service) som gör att du toocentrally hantera datacenter och molnlagring. Du kan använda hello StorSimple Enhetshanteraren service tooperform hello följande uppgifter:

* Konfigurera inställningar för StorSimple-enheter.
* Konfigurera och hantera säkerhetsinställningar för StorSimple-enheter.
* Konfigurera autentiseringsuppgifter för molnet och egenskaper.
* Konfigurera och hantera volymer på en server.
* Konfigurera volym grupper.
* Säkerhetskopiera och återställa data.
* Övervaka prestanda.
* Granska systeminställningarna för och identifiera eventuella problem.

Du kan använda hello StorSimple Enhetshanteraren service tooperform alla administrationsåtgärder utom de som kräver system stillestånd, till exempel installationen och installation av uppdateringar.

Mer information finns för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell för StorSimple
Windows PowerShell för StorSimple innehåller ett kommandoradsgränssnitt som du kan använda toocreate och hantera hello Microsoft Azure StorSimple-tjänsten och konfigurera och övervaka StorSimple-enheter. Det är ett kommandoradsgränssnitt för Windows PowerShell-baserade som innehåller dedikerade cmdletar för att hantera din StorSimple-enhet. Windows PowerShell för StorSimple har funktioner som gör att du kan:

* Registrera en enhet.
* Konfigurera hello nätverksgränssnittet på en enhet.
* Installera vissa typer av uppdateringar.
* Felsöka din enhet genom att öppna hello stöd session.
* Ändra hello enhetens tillstånd.

Du kan använda Windows PowerShell för StorSimple från en seriekonsolen (på en värd datorn ansluten direkt toohello enhet) eller via fjärranslutning med Windows PowerShell-fjärrkommunikation. Observera att vissa Windows PowerShell för StorSimple uppgifter, till exempel första enhetsregistreringen, kan bara utföras på hello seriekonsol.

Mer information finns för[använda Windows PowerShell för StorSimple tooadminister enheten](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-cmdlets
hello Azure PowerShell StorSimple cmdlets är en samling med Windows PowerShell-cmdlets som gör att tooautomate servicenivåer och migreringen från hello-kommandoraden. Mer information om hello Azure PowerShell-cmdlets för StorSimple gå toohello [cmdlet-referens för](/powershell/module/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager är en snapin-modul för Microsoft Management Console (MMC) som du kan använda toocreate konsekvent och tidpunkt i säkerhetskopior av lokala data och molndata. hello snapin-modulen körs på en Windows Server – baserad värd. Du kan använda StorSimple Snapshot Manager till:

* Konfigurera, säkerhetskopiera och ta bort volymer.
* Konfigurera volym grupper tooensure säkerhetskopierade data är programkonsekventa.
* Hantera principer för säkerhetskopiering så att data säkerhetskopieras enligt ett förutbestämt schema och lagras på en angiven plats (lokalt eller i molnet hello).
* Återställa volymer och enskilda filer.

Säkerhetskopieringar avbildas som ögonblicksbilder som registrerar endast hello ändringar eftersom hello senaste ögonblicksbilden togs och kräver mindre lagringsutrymme än fullständiga säkerhetskopieringar. Du kan skapa scheman för säkerhetskopiering eller vidta omedelbara säkerhetskopieringar efter behov. Du kan dessutom använda StorSimple Snapshot Manager tooestablish bevarandeprinciper som styr hur många ögonblicksbilder ska sparas. Om du senare behöver toorestore data från en säkerhetskopiering, StorSimple Snapshot Manager kan välja du mellan hello katalogen i lokala eller molnögonblicksbilder. 

Om en olycka inträffar, eller om du behöver toorestore data i en annan orsak, återställer StorSimple Snapshot Manager det inkrementellt när det behövs. Återställning av data kräver inte att stänga hello hela systemet när du återställer en fil, ersätta utrustning eller flytta operations tooanother plats.

Mer information finns för[vad är StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple-adapter för SharePoint
Microsoft Azure StorSimple innehåller hello StorSimple nätverkskort för SharePoint, en valfri komponent som transparent funktioner tooSharePoint servergrupper utökar StorSimple lagring och dataskydd. hello nätverkskort fungerar med en Remote Blob Storage (RBS)-provider och hello RBS för SQL Server-funktionen, vilket gör att du toomove Blobbar tooa server säkerhetskopieras av hello Microsoft Azure StorSimple-system. Microsoft Azure StorSimple sedan lagrar hello BLOB-data lokalt eller i hello molnet baserat på användning.

Hej StorSimple-kortet för SharePoint hanteras i hello Central Administration av SharePoint-portalen. Följaktligen SharePoint management förblir centraliserad och alla lagringsutrymmen visas toobe i hello SharePoint-servergrupp.

Mer information finns för[StorSimple-kortet för SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Tekniker för hantering av lagring
Dessutom toohello dedikerade StorSimple-enhet, virtuella enheten och andra komponenter, Microsoft Azure StorSimple använder hello följande programvara tekniker tooprovide Snabbåtkomst toodata och tooreduce användningen av lagringsutrymme:

* [Automatisk lagringsnivåer](#automatic-storage-tiering) 
* [Tunn allokering](#thin-provisioning) 
* [Deduplicering och komprimering](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>automatisk lagringsnivåer
Microsoft Azure StorSimple ordnar automatiskt data i logiska nivåer, baserat på aktuell användning, ålder och tooother relationsdata. Data som är de flesta active lagras lokalt, medan mindre aktiva och inaktiva data är automatiskt migreras toohello moln. hello följande diagram illustrerar den här metoden för lagring.

![Lagringsnivåer för StorSimple](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

tooenable Snabbåtkomst, StorSimple lagrar mycket aktiv data (varm data) på SSD-enheter i hello StorSimple-enhet. Den lagrar data som används ibland (varm data) på hårddiskar i hello enhet eller på servrar på hello datacenter. Flyttas inaktiva data, säkerhetskopierade data och data som behålls för arkivering eller efterlevnad syfte toohello moln. 

> [!NOTE]
> I uppdatering 2 eller senare, kan du ange en lokalt Fäst volym, i vilket fall hello data förblir på hello lokala enhet och inte nivåer toohello moln. 


StorSimple anpassar och ordnar om data och ändra lagring tilldelningar som användningsmönster. Viss information kan till exempel bli mindre aktiva över tid. Vartefter det blir mindre progressivt aktiva flyttas den från SSD tooHDDs och toohello moln. Om samma data blir aktiva igen, är det migrerade tillbaka toohello lagringsenhet.

hello händer lagring lagringsnivåer följande:

1. En administratör ställer in ett Microsoft Azure cloud storage-konto.
2. Hej administratör använder hello seriella konsolen och hello StorSimple Device Manager-tjänsten (som körs i hello Azure-portalen) tooconfigure hello enhets- och server skapar volymerna och data protection-principer. Lokala datorer (till exempel filservrar) använda hello Internet Small Computer System Interface (iSCSI) tooaccess hello StorSimple-enhet.
3. Inledningsvis lagrar StorSimple data på hello snabb SSD-nivå hello enhet.
4. Som hello SSD-nivåkapacitet metoder, StorSimple deduplicates komprimerar hello äldsta datablock och flyttar dem toohello hårddisknivå.
5. StorSimple krypterar hello äldsta datablock som hello hårddiskkapacitet nivå metoder och skickar dem på ett säkert sätt toohello Microsoft Azure storage-konto via HTTPS.
6. Microsoft Azure skapar flera repliker av hello data i sitt datacenter och i en fjärransluten datacenter, se till att hello data kan återställas om en olycka inträffar.
7. När hello filserver begär data som lagras i molnet hello, StorSimple returnerar det sömlöst och lagrar en kopia på hello SSD-nivån av hello StorSimple-enhet.

#### <a name="how-storsimple-manages-cloud-data"></a>Hur hanterar StorSimple molndata

StorSimple deduplicates kundinformation över alla hello ögonblicksbilder och hello primära data (data som skrivs av värdar). Deduplicering är bra för lagringseffektivitet, gör hello frågan ”vad är i hello moln” komplicerade. Hej överlappar nivåindelade primära data och hello ögonblicksbilddata varandra. Ett enda segment av data i molnet hello kan användas som nivåindelade primära data och också refereras av flera ögonblicksbilder. Varje ögonblicksbild i molnet säkerställer att en kopia av alla hello tidpunkt i data är låst i hello moln tills denna ögonblicksbild tas bort.

Data raderas från hello molnet endast när det finns inga referenser toothat data. Om vi tar en ögonblicksbild av alla data som hello moln som är i hello StorSimple-enhet och ta sedan bort primära data vi till exempel visas hello _primära_ släppa omedelbart. Hej _molnet data_ som innehåller hello nivåindelade data och hello säkerhetskopieringar, förblir hello samma. Det beror på att det finns en ögonblicksbild som fortfarande refererar hello molndata. Efter hello molnet ögonblicksbild tas bort (och andra ögonblicksbild som refererar till hello samma data), cloud förbrukning således. Innan vi tar bort molndata, kontrollera att inga fortfarande hänvisar till dessa data. Den här processen kallas _skräpinsamling_ och körs ett Bakgrundstjänster på hello enhet. Borttagning av molndata är inte omedelbar som hello skräp samling tjänsten söker efter andra referenser toothat data innan hello borttagning. skräpinsamling hello hastighet beror på hello totala antalet ögonblicksbilder och hello totala data. Hej molndata rensas vanligtvis mindre än en vecka.


### <a name="thin-provisioning"></a>Tunn allokering
Tunn allokering är en virtualiseringsteknik som tillgängligt lagringsutrymme visas tooexceed fysiska resurser. I stället för att reservera tillräckligt med lagringsutrymme i förväg använder StorSimple tunn etablering tooallocate bara tillräckligt med utrymme toomeet aktuella kraven. hello elastisk uppbyggnad molnlagring underlättar den här metoden eftersom StorSimple kan öka eller minska molnet toomeet ändra lagringsbehov.

> [!NOTE]
> Lokalt fästa volymer som inte tunt etablerats. Lagringsutrymme som allokerats tooa endast lokalt volym etableras i sin helhet när hello volym skapas.


### <a name="deduplication-and-compression"></a>Deduplicering och komprimering
Microsoft Azure StorSimple använder deduplicering och data komprimering toofurther minska utrymmeskraven.

Deduplicering minskar hello övergripande mängden data som lagras genom att eliminera redundans i hello lagras datamängden. När informationen ändras StorSimple ignorerar hello oförändrade data och registrerar hello endast ändringar. Dessutom minskar StorSimple hello mängden lagrade data genom att identifiera och ta bort onödig information. 

> [!NOTE]
> Data på lokalt fästa volymer är inte dedupliceras eller komprimerad. Säkerhetskopior av lokalt fästa volymer är dock deduplicerad och komprimeras.


## <a name="storsimple-workload-summary"></a>Översikt över arbetsbelastningar för StorSimple
En sammanfattning av hello stöds StorSimple arbetsbelastningar visas i tabellen nedan.

| Scenario | Arbetsbelastning | Stöds | Begränsningar | Version |
| --- | --- | --- | --- | --- |
| Samarbete |Fildelning |Ja | |Alla versioner |
| Samarbete |Distribuerade fildelning |Ja | |Alla versioner |
| Samarbete |SharePoint |Ja* |Stöds endast med lokalt fästa volymer |Uppdatering 2 och senare |
| Arkivering |Enkel arkivering |Ja | |Alla versioner |
| Virtualisering |Virtuella datorer |Ja* |Stöds endast med lokalt fästa volymer |Uppdatering 2 och senare |
| Databas |SQL |Ja* |Stöds endast med lokalt fästa volymer |Uppdatering 2 och senare |
| Video övervakning |Video övervakning |Ja* |När StorSimple-enhet är dedikerad enbart toothis arbetsbelastning som stöds |Uppdatering 2 och senare |
| Säkerhetskopiering |Säkerhetskopiering av primära mål |Ja* |När StorSimple-enhet är dedikerad enbart toothis arbetsbelastning som stöds |Uppdatering 3 och senare |
| Säkerhetskopiering |Säkerhetskopiering av sekundära mål |Ja* |När StorSimple-enhet är dedikerad enbart toothis arbetsbelastning som stöds |Uppdatering 3 och senare |

*Ja &#42; -Lösningen riktlinjer och begränsningar som ska användas.*

följande arbetsbelastningar hello stöds inte av StorSimple 8000-serien enheter. Om distribuerats på StorSimple, kommer dessa arbetsbelastningar resultera i en konfiguration som inte stöds.

* Medicinska avbildning
* Exchange
* VDI
* Oracle
* SAP
* Stordata
* Innehållsdistribution
* Start från SCSI

Nedan följer en lista över hello infrastrukturkomponenter för StorSimple som stöds.

| Scenario | Arbetsbelastning | Stöds | Begränsningar | Version |
| --- | --- | --- | --- | --- |
| Allmänt |Express Route |Ja | |Alla versioner |
| Allmänt |DataCore FC |Ja* |Stöds med DataCore SANsymphony |Alla versioner |
| Allmänt |DFSR |Ja* |Stöds endast med lokalt fästa volymer |Alla versioner |
| Allmänt |Indexering |Ja* |För nivåindelade volymer endast metadata indexering stöds (inga data).<br>För lokalt fästa volymer stöds fullständig indexering. |Alla versioner |
| Allmänt |Antivirusprogram |Ja* |För nivåindelade volymer stöds endast genomsökning på Öppna och Stäng.<br> Fullständig genomsökning har stöd för lokalt fästa volymer. |Alla versioner |

*Ja &#42; -Lösningen riktlinjer och begränsningar som ska användas.*

Nedan följer en lista över andra program som används med StorSimple toobuild lösningar.

| Arbetsbelastningstyp | Programvara som används med StorSimple | Versioner som stöds|Länken toosolution guide| 
| --- | --- | --- | --- |
| Mål för säkerhetskopian |Veeam |Veeam v 9 och senare |[StorSimple som ett mål med Veaam](storsimple-configure-backup-target-veeam.md)|
| Mål för säkerhetskopian |Veritas Backup Exec |Backup Exec 16 och senare |[StorSimple som ett mål med Backup Exec](storsimple-configure-backup-target-using-backup-exec.md)|
| Mål för säkerhetskopian |Veritas NetBackup |NetBackup 7.7.x och senare  |[StorSimple som ett mål med NetBackup](storsimple-configure-backuptarget-netbackup.md)|
| Globala fildelning <br></br> Samarbete |Talon  |[StorSimple med Talon](https://www.talonstorage.com/products/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>Terminologi för StorSimple
Innan du distribuerar Microsoft Azure StorSimple-lösningen rekommenderar vi att du läser hello följande termer och definitioner.

### <a name="key-terms-and-definitions"></a>Viktiga termer och definitioner
| Termen (förkortning eller förkortning) | Beskrivning |
| --- | --- |
| åtkomstkontrollpost (ACR) |En post som är kopplade till en volym på din Microsoft Azure StorSimple-enhet som styr vilka värdar som kan ansluta tooit. hello bestämning baseras på iSCSI-hello kvalificerade namn (IQN) för hello-värdar (som ingår i hello ACR) som ansluter tooyour StorSimple-enhet. |
| AES 256 |En 256-bitars Advanced Encryption Standard (AES) algoritm för att kryptera data som flyttas tooand från hello molnet. |
| storlek på allokeringsenhet (Australien) |hello minsta mängden diskutrymme som kan ha tilldelats toohold en fil i Windows-filsystem. Om en filstorlek som inte är en jämn multipel av hello klusterstorleken extra utrymme måste vara används toohold hello-fil (in toohello nästa multipel av hello klusterstorleken) ledde till förlorade utrymme och fragmentering av hello hårddisk. <br>hello rekommenderas Australien för Azure StorSimple-volymer är 64 KB eftersom den fungerar bra med hello dedupliceringsalgoritmer. |
| automatisk lagringsnivåer |Flyttar automatiskt mindre aktiva data från SSD tooHDDs och tooa nivå i hello molnet och aktivera hantering av alla lagring från ett centralt gränssnitt. |
| Säkerhetskopieringskatalogen |En samling av säkerhetskopieringar, oftast relaterade av hello programtyp som användes. Den här samlingen visas i hello säkerhetskopieringskatalog bladet för hello StorSimple Enhetshanteraren service Användargränssnittet. |
| säkerhetskopiering katalogfil |En fil som innehåller en lista över tillgängliga ögonblicksbilder som för närvarande lagras i hello säkerhetskopian av databasen för StorSimple Snapshot Manager. |
| princip för säkerhetskopiering |Ett urval av volymer, typ av säkerhetskopiering och en tidsplan som tillåter toocreate säkerhetskopieringar enligt ett fördefinierat schema. |
| binära stora objekt (BLOB) |En samling för binära data som lagras som en enda enhet i ett system för hantering av databasen. Blobbar är vanligtvis bilder, ljud eller andra multimedia objekt, men ibland binära körbara koden lagras som en BLOB. |
| Challenge Handshake Authentication Protocol (CHAP) |Ett protokoll används tooauthenticate hello peer för en anslutning, baserat på hello peer dela ett lösenord eller hemlighet. CHAP kan vara enkelriktade eller ömsesidigt. Enkelriktade CHAP autentiserar hello målet en initierare. Samtidig CHAP kräver att hello mål autentisera hello initieraren och den hello initieraren autentiserar hello mål. |
| Klona |En dubblett av en volym. |
| Moln som en nivå (CaaT) |Molnlagring integrerad som en nivå i hello lagringsarkitektur så att alla lagringsutrymmen visas toobe en del av en enterprise-lagringsnätverk. |
| molntjänstleverantör (CSP) |En leverantör av tjänster för molntjänster. |
| ögonblicksbild i molnet |En tidpunkt i kopia av volymens data som lagras i hello molnet. En ögonblicksbild i molnet motsvarar tooa ögonblicksbild replikeras på ett annat, externt lagringssystem. Molnögonblicksbilder är särskilt användbart i katastrofåterställning. |
| Krypteringsnyckel för molnlagring |Ett lösenord eller en nyckel som används av din StorSimple-enhet tooaccess hello krypterade data som skickas av din enhet toohello moln. |
| klustermedveten uppdatering |Hantera programuppdateringar på servrar i ett redundanskluster så att hello uppdateringar har minimal eller ingen effekt på tjänstetillgänglighet. |
| DataPath |En samling funktionella enheter som utför åtgärder sammankopplade databearbetning. |
| Inaktivera |En permanent åtgärd att radbrytningar hello anslutningen mellan hello StorSimple-enhet och hello associerade tjänst i molnet. Molnögonblicksbilder av hello enheten kan kvar efter den här processen och klonas eller användas för katastrofåterställning. |
| diskspegling |Replikering av logisk diskvolymer på olika hårddiskar eller enheter i realtid tooensure kontinuerlig tillgänglighet. |
| dynamisk diskspegling |Replikering av logisk diskvolymer på diskar. |
| dynamiska diskar |En volym diskformat som använder hello LDM Logical Disk Manager ()-toostore och hantera data över flera fysiska diskar. Dynamiska diskar kan vara förstorade tooprovide mer ledigt utrymme. |
| Utökade Bunch av diskar (EBOD) hölje |En sekundär inneslutning av din Microsoft Azure StorSimple-enhet som innehåller extra hårddisk diskar för ytterligare lagringsutrymme. |
| FAT-etablering |En konventionell lagringsetablering i vilka storage allokeras utrymmet baserat på förväntat behov (och är vanligtvis utöver hello aktuella behov). Se även *tunn allokering*. |
| hårddisken (HDD) |En enhet som använder roterar skivorna toostore data. |
| hybridmolnlagring |Lagringsarkitektur som använder lokala och extern resurser, inklusive lagringsutrymmet i molnet. |
| Internet Small Computer System Interface (iSCSI) |En nätverk standard Internet Protocol IP-baserad lagring för att länka data lagring utrustning eller lokaler. |
| iSCSI-initierare |En programvarukomponent som gör att en värddator som kör Windows tooconnect tooan extern lagring för iSCSI-baserade nätverk. |
| iSCSI kvalificerade namn (IQN) |Ett unikt namn som identifierar en iSCSI-mål eller initierare. |
| iSCSI-mål |En programvarukomponent som tillhandahåller centraliserad iSCSI-diskundersystem i SAN-nätverk. |
| Live arkivering |En lagring metod där arkiveringsdata är tillgänglig tid för alla hello (inte lagras utanför kontoret på band, till exempel). Microsoft Azure StorSimple använder live arkivering. |
| lokalt Fäst volym |en volym som finns på hello enheten och aldrig nivåer toohello moln. |
| lokal ögonblicksbild |En tidpunkt i kopia av volymens data som lagras på hello Microsoft Azure StorSimple-enhet. |
| Microsoft Azure StorSimple |En kraftfull lösning som består av en lagringsenhet för datacenter och programvara som gör att IT-organisationer tooleverage molnlagring som om det vore datacenter lagring. StorSimple förenklar dataskydd och datahantering samtidigt minska kostnaderna. hello lösning konsoliderar primära lagring, arkivera, säkerhetskopiering och disaster recovery (DR) via sömlös integration med hello molnet. Genom att kombinera hantering för SAN-lagring och molnet data på en plattform i företagsklass, aktivera hastighet, enkel och tillförlitlighet för alla lagringsrelaterade StorSimple-enheter. |
| Ström och kylning modul (PCM) |Maskinvarukomponenter för din StorSimple-enhet som består av hello strömförsörjning och kylning fläkt, hello hello därför namn ström och kylning modulen. hello primära inneslutning av hello enheten har två 764W PCMs hello EBOD hölje har två 580 w vid PCMs. |
| Primär enhet |Huvudsakliga höljet av StorSimple-enheten som innehåller hello programmet plattform domänkontrollanterna. |
| Återställning tid mål för Återställningstid |hello maximala tidsperiod som ska vara förbrukad innan en affärsprocess eller system har återställts efter en katastrof. |
| serieansluten SCSI (SAS) |En typ av hårddisken (HDD). |
| krypteringsnyckel för tjänstdata |En nyckel som har gjorts tillgängliga tooany nya StorSimple-enhet som registreras med hello StorSimple enheten Manager-tjänsten. hello configuration data som överförs mellan hello StorSimple Device Manager-tjänsten och hello krypteras med en offentlig nyckel och kan sedan dekrypteras endast på hello-enhet med hjälp av en privat nyckel. Krypteringsnyckel för tjänstdata kan hello service tooobtain den privata nyckeln för dekryptering. |
| Nyckel för tjänstregistrering |En nyckel som hjälper till att registrera hello StorSimple-enhet med hello StorSimple enheten Manager-tjänsten så att den visas i hello Azure-portalen för ytterligare hanteringsåtgärder för. |
| Small Computer System Interface (SCSI) |En uppsättning standarder för att ansluta datorer och överföra data mellan dem fysiskt. |
| Solid-State-hårddisk (SSD) |En disk som innehåller inga rörliga delar; till exempel flash-enhet. |
| Storage-konto |En uppsättning autentiseringsuppgifter länkade tooyour storage-konto för en viss molntjänstleverantören. |
| StorSimple-adapter för SharePoint |En komponent i Microsoft Azure StorSimple som transparent tooSharePoint servergrupper utökar StorSimple lagring och dataskydd. |
| StorSimple enheten Manager-tjänsten |En förlängning av hello Azure-portalen där du toomanage dina Azure StorSimple lokala och virtuella enheter. |
| StorSimple Snapshot Manager |En Microsoft Management Console (MMC) snapin-modulen för hantering av säkerhetskopiering och återställning i Microsoft Azure StorSimple. |
| ta säkerhetskopia |En funktion som gör hello användaren tootake en interaktiv säkerhetskopia av en volym. Det är något annat sätt att göra en manuell säkerhetskopiering för en volym som skillnad från tootaking en automatisk säkerhetskopiering via en definierad princip. |
| Tunn allokering |En metod för att optimera hello effektivitet med vilka hello används tillgängligt lagringsutrymme i lagringssystem. I tunn allokering allokeras hello lagring mellan flera användare baserat på hello minsta utrymmet som krävs för varje användare vid en given tidpunkt. Se även *fat etablering*. |
| Skiktning |Ordna data i logiska grupper baserat på aktuell användning, ålder och tooother relationsdata. StorSimple ordnar automatiskt data i nivåer. |
| Volym |Logiska lagringsutrymmen som presenteras i hello form av enheter. StorSimple-volymer motsvarar toohello volymer som monterats av hello värden, inklusive de som identifierats via hello användning av iSCSI- och en StorSimple-enhet. |
| volymbehållare |En gruppering av volymer och hello-inställningar som gäller toothem. Alla volymer på StorSimple-enhet är grupperade i volymbehållare. Volymen behållaren inställningar inkluderar storage-konton, krypteringsinställningar för data skickas toocloud med associerade krypteringsnycklar och bandbredd för åtgärder som rör hello molnet. |
| volymen grupp |StorSimple Snapshot Manager en volym grupp är en uppsättning volymer som konfigurerats toofacilitate uppgifter. |
| Tjänsten Volume Shadow Copy (VSS) |En Windows Server operativsystemtjänst som underlättar programkonsekvens genom att kommunicera med VSS-medvetna program toocoordinate hello inkrementell att ögonblicksbilder skapas. VSS säkerställer att hello program är tillfälligt inaktiv när ögonblicksbilder tas. |
| Windows PowerShell för StorSimple |Ett Windows PowerShell-baserade kommandoradsgränssnitt används toooperate och hantera din StorSimple-enhet. Det här gränssnittet har ytterligare dedikerad cmdlets som är inriktad på att hantera en StorSimple-enhet utan att några av hello grundläggande funktionerna i Windows PowerShell. |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [StorSimple-säkerhet](storsimple-8000-security.md).

