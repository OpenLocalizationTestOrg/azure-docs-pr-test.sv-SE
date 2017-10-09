---
title: "aaaAzure hot, svalna, och arkivlagring för BLOB | Microsoft Docs"
description: "Frekvent, lågfrekvent och arkivlagring för Azure Blob Storage-konton."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 42fb699bf16147ba8a4d9f75a62debadea5af65e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-blob-storage-hot-cool-and-archive-preview-storage-tiers"></a>Azure Blob Storage: Nivåer för frekvent, lågfrekvent och arkivlagring (förhandsversion)

## <a name="overview"></a>Översikt

Azure Storage erbjuder tre lagringsnivåer för lagring av Blob-objekt, så att du kan lagra data så kostnadseffektivt som möjligt, beroende på din användning. hello Azure **frekventa lagringsnivå** är optimerad för att lagra data som används ofta. hello Azure **lågfrekvent åtkomstnivå** är optimerad för att lagra data som sällan används och lagras i minst en månad. Hej [Arkiv lagringsnivå (förhandsgranskning)](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) är optimerad för att lagra data som sällan används och lagras i minst sex månader med flexibla svarstidskrav (på hello efter timmar). Hej *Arkiv* lagringsnivå kan bara användas på hello blob-nivå och inte på hello hela lagringskontot. Data i hello lågfrekvent åtkomstnivå kan tolerera lite lägre tillgänglighet, men kräver fortfarande hög hållbarhet och liknande tid att nå och genomströmning egenskaper som varm data. För data i den lågfrekventa och arkivlagringsnivån är serviceavtal med lägre tillgänglighet och högre åtkomstkostnader godtagbara med tanke på de mycket lägre lagringskostnaderna.

Idag växer data som lagras i hello molnet i exponentiell takt. toomanage kostnader för dina växande lagringsbehov är det bra tooorganize dina data baserat på attribut som ofta åtkomst och planerad kvarhållningsperiod. Data som lagras i molnet hello kan vara olika beroende på hur den genereras, bearbetas och används under livslängden. Vissa data används aktivt och ändras under livslängden. Vissa data används ofta tidigt i livslängden och sedan minskar användning drastiskt som hello data blir äldre. Vissa data förblir inaktiva i hello molntjänster och används sällan, lagras kanske aldrig, en gång.

För varje scenario finns en lagringsnivå som är optimerad för motsvarande åtkomstmönster. I Azure Blob Storage finns en frekvent, lågfrekvent och arkivlagringsnivå som uppfyller behovet av olika lagringsnivåer med olika prissättningsmodeller.

## <a name="blob-storage-accounts"></a>Blob Storage-konton

**Blob Storage-konton** är särskilda lagringskonton för att lagra ostrukturerade data som blobar (objekt) i Azure Storage. Med Blob storage-konton nu kan du välja mellan hot och lågfrekvent nivåer på kontonivå eller hot, kall och arkivera nivåerna på hello blob-nivå, baserat på åtkomstmönster. Lagra din data som sällan används kalla på hello lägsta lagringskostnaden mindre ofta använda kall data till en lägre kostnad än hot och lagra data som används ofta mer frekvent kostnad hello lägsta åtkomst. BLOB storage-konton är liknande tooyour befintliga allmänna lagringskonton och dela alla hello hållbarhet, tillgänglighet, skalbarhet och prestandafunktioner som du använder idag, inklusive 100 procent API-konsekvens för blockblobbar och Lägg till blobbar.

> [!NOTE]
> Blob Storage-konton stöder endast block- och tilläggsblobar, inte sidblobar.

BLOB storage-konton exponerar hello **åtkomstnivå** -attribut som tillåter toospecify hello lagringsnivå som **frekvent** eller **kall** beroende på hello data som lagras i hello konto. Om det finns en ändring i hello användningsmönstret för dina data, kan du också växla mellan dessa lagringsnivåer när som helst. hello Arkiv nivån (förhandsversion) kan endast användas på hello blob-nivå.

> [!NOTE]
> Ändra hello lagringsnivå kan resultera i ytterligare avgifter. Se hello [priser och fakturering](#pricing-and-billing) mer information.

### <a name="hot-access-tier"></a>Frekvent åtkomstnivå

Exempelscenarier för hello frekventa lagringsnivå inkluderar:

* Data som aktivt används eller förväntade toobe används (läsa och skriva) ofta.
* Data som mellanlagras för bearbetning och eventuell migrering toohello lågfrekvent åtkomstnivå.

### <a name="cool-access-tier"></a>Lågfrekvent åtkomstnivå

Exempelscenarier för lågfrekvent åtkomstnivå hello inkluderar:

* Datauppsättningar för kortsiktig säkerhetskopiering och haveriberedskap.
* Äldre medieinnehåll inte visas så ofta längre, men är förväntade toobe tillgängligt direkt vid behov.
* Stora datamängder som behöver toobe lagras kostnaden effektivt när mer data som samlas in för framtida bearbetning. (*Till exempel* långsiktig lagring av vetenskapliga data eller telemetridata (rådata) från en tillverkningsanläggning.)

### <a name="archive-access-tier-preview"></a>Arkivåtkomstnivå (förhandsversion)

[Arkivlagring](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) har hello lägsta lagring kostnad och högre data hämtning kostnader, jämfört med toohot och lågfrekvent.

När en blob finns i arkivlagring kan den inte läsas, kopieras, skrivas över eller ändras. Du kan inte heller ta ögonblicksbilder av en blob i arkivlagring. Du kan dock använda befintliga operations toodelete, lista, hämta blob egenskaper/metadata eller ändra hello nivå i din blob. tooread data i arkivlagring, måste du först ändra hello nivå av hello blob toohot eller coolbar. Den här processen kallas rehydration och kan ta upp too15 timmar toocomplete för blobbar som är mindre än 50 GB. Ytterligare tid som krävs för större BLOB varierar med hello blob genomströmning gräns.

Under rehydration, kan du kontrollera hello ”Arkiv status” blob egenskapen tooconfirm om hello nivå har ändrats. hello status är ”rehydrate-väntande-till-hot” eller ”rehydrate-väntande-till-lågfrekvent” beroende på hello mål nivå. På slutförande, hello ”Arkivera status” blob-egenskapen tas bort och hello ”åtkomstnivå” visar blob egenskapen hello varm eller kall nivå.  

Exempelscenarier för hello Arkiv lagringsnivå inkluderar:

* Datauppsättningar för långsiktig säkerhetskopiering, arkivering och haveriberedskap
* Ursprungliga rådata som måste bevaras, även efter att de har bearbetats till ett slutligt användbart format. (*Till exempel* mediefiler i RAW-format som har omkodats till andra format.)
* Kompatibilitet och arkiveringsdata som behöver toobe lagras under en längre tid och nästan aldrig används. (*Till exempel* film från säkerhetskameror, gamla röntgenbilder/magnetröntgenbilder för vårdorganisationer, ljudinspelningar och transkript av kundsamtal för ekonomiska tjänster.)

### <a name="recommendations"></a>Rekommendationer

Mer information om lagringskonton finns i [Om Azure Storage-konton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

För program som bara behöver block- eller tilläggsbloblagring, bör du använda Blob storage-konton, tootake nytta av hello differentierade prissättningsmodellerna för olika lagringsnivåer. Vi förstår dock inte kanske det är möjligt i vissa fall där med hjälp av allmänna storage konton skulle vara hello sätt toogo som:

* Du måste toouse tabeller, köer eller filer och vill att dina blobbar som lagras i hello samma lagringskonto. Observera att det inte finns några tekniska fördelar toostoring dessa i hello samma konto än med hello samma delade nycklar.

* Du måste fortfarande toouse hello klassiska distributionsmodellen. BLOB storage-konton är bara tillgängliga via hello Azure Resource Manager-distributionsmodellen.

* Du måste toouse sidblobar. Blob Storage-konton stöder inte sidblobar. Generellt rekommenderar vi att du använder blockblobar om du inte har ett särskilt behov av sidblobar.

* Du använder en version av hello [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) som är äldre än 2014-02-14 eller ett klientbiblioteket med en version lägre än 4.x och det går inte att uppgradera ditt program.

> [!NOTE]
> Blob Storage-konton stöds för närvarande i alla Azure-regionerna.
 

## <a name="blob-level-tiering-feature-preview"></a>Blob-nivåfunktion (förhandsversion)

BLOB-nivå skiktning kan du nu toochange hello nivå av dina data nivån hello-objekt med en enda åtgärd som kallas [ange Blob-nivå](/rest/api/storageservices/set-blob-tier). Du kan enkelt ändra hello åtkomstnivå till en blob bland hello hot, kall eller arkivera nivåer som användningsmönster förändras utan toomove data mellan konton. Alla nivåändringar sker omedelbart förutom när en blob återuppväcks från arkivet. Blobbar i alla tre storage nivåer kan finnas tillsammans i hello samma konto. Alla blob som inte har en uttryckligen har tilldelats nivå ärver hello nivån från hello konto åtkomstinställning nivå.

toouse funktionerna i förhandsgranskningen, följ instruktionerna för hello i hello [Azure arkivet och Blob-nivå skiktning blogg meddelande](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering).

hello Följ innehåller vissa begränsningar som gäller under förhandsgranskning för blob-nivå skiktning:

* Endast nya Blob Storage-konton som skapats i USA, östra 2 efter registrering för förhandsversionen har stöd för arkivlagring.

* Endast nya Blob Storage-konton som skapats i offentliga regioner efter registrering för förhandsversionen har stöd för blob-nivå.

* Blob-nivå och arkivlagring stöds endast i [LRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#locally-redundant-storage) -lagring. [GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#geo-redundant-storage) och [RA-GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#read-access-geo-redundant-storage) stöds i framtida hello.

* Du kan inte ändra hello nivå i en blob med ögonblicksbilder.

* Du kan inte kopiera eller skapa en ögonblicksbild av en blob i arkivlagring.

## <a name="comparison-of-hello-storage-tiers"></a>Jämförelse av hello lagringsnivåer

hello följande tabell visar en jämförelse av hello frekvent och lågfrekvent lagringsnivå. hello Arkiv blob-nivå nivå är i preview så att det finns inget serviceavtal för den.

| | **Frekvent lagringsnivå** | **Lågfrekvent lagringsnivå** |
| ---- | ----- | ----- |
| **Tillgänglighet** | 99,9 % | 99 % |
| **Tillgänglighet** <br> **(RA-GRS-läsningar)**| 99,99 % | 99,9 % |
| **Avgifter för användning** | Högre kostnader för lagring, lägre kostnader för åtkomst och transaktioner | Lägre kostnader för lagring, högre kostnader för åtkomst och transaktioner |
| **Minsta objektstorlek** | Saknas | Saknas |
| **Minsta lagringstid** | Saknas | Saknas |
| **Svarstid** <br> **(Tid toofirst byte)** | millisekunder | millisekunder |
| **Mål för skalbarhet och prestanda** | Samma som allmänna lagringskonton | Samma som allmänna lagringskonton |

> [!NOTE]
> BLOB storage-konton stöd hello samma mål av prestanda och skalbarhet som allmänna lagringskonton. Mer information finns i [Skalbarhets- och prestandamål i Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).


## <a name="pricing-and-billing"></a>Priser och fakturering
BLOB storage-konton använder en priserna för bloblagring baseras på hello lagringsnivå. När du använder ett Blob storage-konto gäller hello efter fakturering överväganden:

* **Lagringskostnader**: dessutom toohello mängden lagrade data hello kostnaden för datalagring varierar beroende på hello lagringsnivå. kostnaden för hello per gigabyte är lägre för hello lågfrekvent åtkomstnivå än för hello frekventa lagringsnivå.

* **Kostnader för dataåtkomst**: för data i hello lågfrekvent åtkomstnivå debiteras du data access avgift per gigabyte för läsningar och skrivningar.

* **Transaktionskostnader**: Det finns en kostnad per transaktion för båda nivåerna. Hello per transaktion kostnaden för hello lågfrekvent åtkomstnivå är dock högre än för hello frekventa lagringsnivå.

* **Dataöverföringskostnader för GEO-replikering**: Detta gäller endast tooaccounts med konfigurerad geo-replikering, inklusive GRS och RA-GRS. Dataöverföring för geo-replikering debiteras per gigabyte.

* **Kostnader för utgående dataöverföring**: Utgående dataöverföringar (data som överförs utanför en Azure-region) debiteras för bandbreddsanvändning per gigabyte, på samma sätt som för allmänna lagringskonton.

* **Ändra hello lagringsnivå**: ändra hello lagringsnivå från kall toohot alla hello data i hello storage-konto för varje övergång ådrar sig en lika tooreading kostnad. På hello är däremot ändrar hello lagringsnivå från varm toocool kostar.

> [!NOTE]
> Mer information om hello Prismodell för Blob storage-konton finns [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/) sidan. Mer information om hello utgående data transfer avgifter finns [prisinformation om dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/) sidan.

## <a name="quickstart"></a>Snabbstart

I det här avsnittet visar vi hello följande scenarier med hjälp av hello Azure-portalen:

* Hur toocreate Blob storage-kontot.
* Hur toomanage Blob storage-kontot.

Du kan inte ange hello åtkomst nivå tooarchive i hello följande exempel eftersom den här inställningen gäller toohello hela lagringskontot. Arkiv kan bara anges för en specifik blob.

### <a name="create-a-blob-storage-account-using-hello-azure-portal"></a>Skapa ett Blob storage-konto med hjälp av hello Azure-portalen

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. På navmenyn hello väljer **ny** > **Data + lagring** > **lagringskonto**.

3. Ange ett namn för lagringskontot.
   
    Det här namnet måste vara globalt unika. den används som en del av hello URL tooaccess hello objekt som används i hello storage-konto.  

4. Välj **Resource Manager** som hello distributionsmodell.
   
    Nivåindelad lagring kan bara användas med Resource Manager lagringskonton; Detta är hello rekommenderas distributionsmodell för nya resurser. Mer information finns på hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. I listrutan för hello konto typ väljer **Blob Storage**.
   
    Det är där du väljer hello typ av lagringskonto. Nivåindelad lagring är inte tillgänglig i allmänna lagring. Det finns bara i hello typen Blob storage-konto.     
   
    Observera att när du väljer detta hello prestandanivå anges tooStandard. Nivåindelad lagring är inte tillgänglig med hello premiumnivån prestanda.

6. Välj hello replikeringsalternativ för lagringskontot hello: **LRS**, **GRS**, eller **RA-GRS**. hello standardvärdet är **RA-GRS**.
   
    LRS = lokalt redundant lagring. GRS = geo-redundant lagring (2 regioner). RA-GRS är läsbehörighet geo-redundant lagring (2 regioner med Läs åtkomst toohello andra).
   
    Mer information om replikeringsalternativen för Azure Storage finns i [Azure Storage-replikering](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Välj hello rätt lagringsnivå för dina behov: Ange hello **åtkomstnivå** tooeither **kall** eller **frekvent**. hello standardvärdet är **frekvent**. 

8. Välj hello prenumeration där du vill toocreate hello nytt lagringskonto.

9. Ange en ny resursgrupp eller välj en befintlig resursgrupp. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Välj hello region för ditt lagringskonto.

11. Klicka på **skapa** toocreate hello storage-konto.

### <a name="change-hello-storage-tier-of-a-blob-storage-account-using-hello-azure-portal"></a>Ändra hello lagringsnivå för Blob storage-kontot med hello Azure-portalen

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. toonavigate tooyour storage-konto, markera alla resurser och välj sedan ditt lagringskonto.

3. I hello inställningar-bladet klickar du på **Configuration** tooview och/eller ändra hello-kontokonfigurationen.

4. Välj hello rätt lagringsnivå för dina behov: Ange hello **åtkomstnivå** tooeither **kall** eller **frekvent**...

5. Klicka på Spara hello överst i hello-bladet.

> [!NOTE]
> Ändra hello lagringsnivå kan resultera i ytterligare avgifter. Se hello [priser och fakturering](#pricing-and-billing) mer information.


## <a name="evaluating-and-migrating-tooblob-storage-accounts"></a>Utvärderar och migrera tooBlob storage-konton
hello syftet med det här avsnittet är toohelp användare toomake en mjuk övergång toousing Blob storage-konton. Det finns två scenarier:

* Du har ett allmänt lagringskonto och vill tooevaluate en ändring tooa Blob storage-konto med hello rätt lagringsnivå.
* Du har valt toouse Blob storage-kontot eller redan har en och vill tooevaluate om du ska använda hello varm eller kall lagringsnivå.

I båda fallen hello första uppgift är tooestimate hello kostnaden för att lagra och komma åt dina data som lagras i ett Blob storage-konto och jämföra som med din aktuella kostnader.

## <a name="evaluating-blob-storage-account-tiers"></a>Utvärdera Blob Storage-kontonivåer

I ordning tooestimate hello kostnaden för att lagra och komma åt data som lagras i ett Blob storage-konto, måste tooevaluate det befintliga användningsmönstret eller ungefärlig förväntade användningen i mönstret. I allmänhet önskade tooknow:

* Din lagringsanvändning – hur mycket data lagras och hur ändras lagringen månadsvis?

* Din lagring åtkomstmönster – hur mycket data som lästes från och skriftliga toohello konto (inklusive nya data)? Hur många transaktioner används för dataåtkomst och vilka typer av transaktioner är de?

## <a name="monitoring-existing-storage-accounts"></a>Övervaka befintliga lagringskonton

toomonitor din befintliga lagring konton och samla in dessa data kan du använda Azure Storage Analytics som utför loggning och tillhandahåller data för mått för ett lagringskonto. Storage Analytics kan lagra mått som innehåller aggregerade statistik och kapacitet transaktionsdata om begäranden toohello Blob storage-tjänst för både allmänna lagringskonton samt Blob storage-konton. Dessa data lagras i välkända tabeller i hello samma lagringskonto.

Mer information finns i [Om mätvärden i Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) och [Schema över måttabeller i Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> BLOB storage-konton exponerar hello tabelltjänst endast för att lagra och komma åt hello mått data för det kontot.

toomonitor hello användningen av lagringsutrymme för hello Blob storage-tjänst, behöver du tooenable hello kapacitetsdata.
Alternativet är aktiverat, kapacitetsinformation registreras dagligen för ett lagringskonto Blob-tjänsten, och registrerats som en post som skrivs toohello *$MetricsCapacityBlob* tabell inom hello samma lagringskonto.

toomonitor hello mönster för dataåtkomst för hello Blob storage-tjänst måste du tooenable hello timvis transaktion mått på en API-nivå. Alternativet är aktiverat, per API transaktioner är samman varje timme och registrerats som en post som skrivs toohello *$MetricsHourPrimaryTransactionsBlob* tabell inom hello samma lagringskonto. Hej *$MetricsHourSecondaryTransactionsBlob* poster hello sekundär slutpunkt för transaktioner toohello när du använder RA-GRS storage-konton.

> [!NOTE]
> Om du har ett allmänt lagringskonto där du har lagrat sidblobbar och virtuella datordiskar jämte block och tillsammans med blockdata och data för tilläggsblobbar så gäller inte den här uppskattningsberäkningen. Detta beror på att det finns inget sätt att särskilja kapacitet och transaktionen mått baserat på hello blob för endast blockera och tilläggsblobar, vilket kan vara migreras tooa Blob storage-konto.

tooget en bra uppskattning av dina data förbrukning och mönster för dataåtkomst, rekommenderas du väljer en kvarhållningsperiod för hello mätvärden som är representativ för din regelbunden användning och dra slutsatser. Ett alternativ är tooretain hello mätvärdesdata för 7 dagar och samla in hello data varje vecka för analys hello slutet av hello månad. Ett annat alternativ är tooretain hello mått data för hello senaste 30 dagarna och samla in och analysera hello hello slutet av hello 30-dagarsperiod.

Mer information om hur du aktiverar, samlar in och visar mätvärden finns i [Aktivera Azure Storage-mätvärden och visa måttdata](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Lagring, åtkomst och hämtning av analysdata debiteras på samma sätt som vanliga användardata.

### <a name="utilizing-usage-metrics-tooestimate-costs"></a>Använda förbrukning mått tooestimate kostnader

### <a name="storage-costs"></a>Lagringskostnader

Hej senaste posten i hello kapacitet mått tabellen *$MetricsCapacityBlob* med hello radnyckel *”uppgifter”* visar hello lagringskapacitet som används av användardata. Hej senaste posten i hello kapacitet mått tabellen *$MetricsCapacityBlob* med hello radnyckel *'analytics'* visar hello lagringskapacitet som används av hello analytics loggar.

Den här total kapacitet som används av båda användare data och analyser loggar (om aktiverat) sedan kan använda tooestimate hello kostnaden för lagring av data i hello storage-konto. hello samma metod kan även användas för att uppskatta lagringskostnader för block och tilläggsblobbar i allmänna lagringskonton.

### <a name="transaction-costs"></a>Transaktionskostnader

hello summan av *'TotalBillableRequests'*, över alla poster för ett API i hello transaktion mått tabell visar hello sammanlagda antalet transaktioner för det specifika API. *Till exempel*, hello Totalt antal *'GetBlob'* transaktioner inom en viss kan beräknas genom hello summan av fakturerbar förfrågningarna för alla poster med hello radnyckel *' användaren. GetBlob-*.

I ordning tooestimate transaktionskostnader för Blob storage-konton måste toobreak ned hello transaktioner i tre grupper eftersom de olika priser.

* Skrivtransaktioner som *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* och *'CopyBlob'*.
* Borttagningstransaktioner som *'DeleteBlob'* och *'DeleteContainer'*.
* Alla andra transaktioner.

I ordning tooestimate transaktionskostnader för allmänna lagringskonton behöver du tooaggregate alla transaktioner oavsett hello åtgärden/API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Dataöverföringskostnader för dataåtkomst och geo-replikering

Även om storage analytics inte tillhandahåller hello mängd data läses och skrivs tooa storage-konto, de kan ungefär beräknas genom att titta på tabellen hello mått. hello summan av *'TotalIngress'* över alla poster för ett API i hello transaktion mått tabell visar hello totala mängden ingång data i byte för det specifika API. På liknande sätt hello summan av *'TotalEgress'* anger hello totala mängd utgående data i byte.

I ordning tooestimate hello data access kostnader för Blob storage-konton måste toobreak ned hello transaktioner i två grupper. 

* hello mängden data som hämtats från hello storage-konto kan uppskattas genom att titta på hello summan av *'TotalEgress'* för främst hello *'GetBlob'* och *'CopyBlob'* åtgärder.

* hello mängden data som skrivs toohello storage-konto kan uppskattas genom att titta på hello summan av *'TotalIngress'* för främst hello *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* och *'AppendBlock'* åtgärder.

hello kostnaden för dataöverföring för geo-replikering för Blob storage-konton kan också beräknas med hjälp av hello uppskattning för hello mängden data som skrivs när du använder en GRS eller RA-GRS-lagringskonto.

> [!NOTE]
> Ta en titt på hello vanliga frågor och svar med rubriken för en mer ingående exempel beräkna hello kostnader för att använda hello varm eller kall lagringsnivå *”vad är frekvent och lågfrekvent åtkomstnivåer och hur bestämmer vilka en toouse”?* i hello [Azure Storage-priser sidan](https://azure.microsoft.com/pricing/details/storage/).
 
## <a name="migrating-existing-data"></a>Migrera befintliga data

Ett Blob Storage-konto är specialanpassat för lagring av endast block- och tilläggsblobar. Befintliga allmänna lagringskonton, som gör att du toostore tabeller, köer, filer och diskar, samt blobbar, får inte vara konverterade tooBlob storage-konton. toouse Hej lagringsnivåer, du behöver toocreate Blob storage-konton och migrera dina befintliga data till hello nyligen skapade konton.

Du kan använda följande metoder toomigrate befintliga data i Blob storage-konton från lokala lagringsenheter, från tredje-molnlagringsleverantörer eller från dina befintliga allmänna lagringskonton i Azure hello:

### <a name="azcopy"></a>AzCopy

AzCopy är en Windows-kommandoradsverktyg för högpresterande kopiering av data tooand från Azure Storage. Du kan använda AzCopy toocopy data till Blob storage-konto från dina befintliga allmänna lagringskonton eller tooupload data från din lokala lagringsenheter till ditt Blob storage-konto.

Mer information finns i [överföra data med kommandoradsverktyget Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Bibliotek för flytt av data

Azure Storage data movement biblioteket för .NET är baserat på hello core data movement som används för AzCopy. hello-biblioteket är utformat för högpresterande, tillförlitlig och enkel dataöverföring operations liknande tooAzCopy. Detta ger dig tootake fördelarna med hello-funktioner som tillhandahålls av azcopy direkt i ditt program internt utan toodeal köra och övervaka externa instanser av AzCopy.

Mer information finns i [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST-API eller klientbibliotek

Du kan skapa ett anpassat program toomigrate dina data till Blob storage-kontot med någon av hello Azures klientbibliotek eller hello Azure storage services REST API. Azure Storage innehåller omfattande klientbibliotek för flera språk och plattformar som .NET, Java, C++, Node.JS, PHP, Ruby och Python. Hej klientbiblioteken har avancerade funktioner, t.ex logik för omprövning, loggning och parallell överföring. Du kan också utveckla direkt mot hello REST-API som kan anropas med valfritt språk som gör HTTP/HTTPS-förfrågningar.

Mer information finns i [Get Started with Azure Blob storage](storage-dotnet-how-to-use-blobs.md) (Kom igång med Azure Blob Storage).

> [!NOTE]
> Blobar som krypteras med kryptering på klientsidan lagrar krypteringsrelaterade metadata lagras med hello blob. Det är absolut nödvändigt att kopieringsmekanismen bör kontrollera som hello blobbmetadata, och särskilt Hej krypteringsrelaterade metadata, bevaras. Om du kopierar hello blobar utan metadata hämtas inte igen hello blob-innehåll. Mer information om krypteringsrelaterade metadata finns i [Azure Storage Client Side Encryption](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
 
## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

1. **Är de befintliga lagringskontona fortfarande tillgängliga?**
   
    Ja, de befintliga lagringskontona är fortfarande tillgängliga. Varken priserna eller funktionerna har ändrats.  De behöver inte hello möjlighet toochoose en lagringsnivå och kommer inte heller i hello framtida.

2. **När och varför ska jag börja använda Blob Storage-konton?**
   
    BLOB storage-konton är särskilt utformade för att lagra blobar och ger oss möjlighet toointroduce nya blobanpassade funktioner. Framöver, är Blob storage-konton hello rekommenderat sätt för att lagra blobar eftersom framtida funktioner, till exempel hierarkisk lagring och skiktning kommer att läggas till baserat på den här kontotypen. Dock är det upp tooyou när du vill att toomigrate baserat på dina affärsbehov.

3. **Kan jag omvandla mitt befintliga lagring konto tooa Blob storage-konto?**
   
    Nej. Blob Storage-kontot är en annan typ av lagringskonto. Du måste skapa ett nytt konto och sedan migrera dina data enligt beskrivningen ovan.

4. **Kan jag lagra objekt i båda lagringsnivåer i hello samma konto?**
   
    Hej *'Åtkomstnivå'* attribut anger hello värde hello lagringsnivå inställd på en kontonivå och gäller tooall objekt på det kontot. Dock hello blob-nivå lagringsnivåer (förhandsgranskning)-funktionen kan du tooset hello åtkomstnivå på specifika blobbar och detta åsidosätter hello åtkomst nivån på hello-konto. 

5. **Kan jag ändra hello lagringsnivå för mitt Blob storage-konto?**
   
    Ja. Du kan ändra hello lagringsnivå genom att ange hello *'Åtkomstnivå'* attribut för hello storage-konto. Ändra hello lagringsnivå gäller tooall objekt som lagras i hello-konto. Ändra hello lagringsnivå från varm toocool inga avgifter, när du ändrar från kall toohot medför en per GB kostnaden för att läsa alla hello data i hello-konto.

6. **Hur ofta kan jag ändra hello lagringsnivå för mitt Blob storage-konto?**
   
    När vi inte behöver använda en begränsning på hur ofta hello lagringsnivå kan ändras, Tänk på att ändra hello lagringsnivå från kall toohot kan medföra betydande kostnader. Vi rekommenderar inte ändra hello lagringsnivå ofta.

7. **Hello blobar i lågfrekvent åtkomstnivå hello fungerar annorlunda än hello i hello frekventa lagringsnivå?**
   
    Blobar i frekvent lagringsnivå för hello har hello samma svarstid som blobar i allmänna lagringskonton. Blobar i hello lågfrekvent åtkomstnivå har liknande svarstid (i millisekunder) som blobar i allmänna lagringskonton.
   
    Blobar i lågfrekvent åtkomstnivå hello har en något lägre tillgänglighetsnivå (SLA) än hello blobar som lagras i hello frekventa lagringsnivå. Mer information finns i [SLA för Storage](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kan jag lagra sidblobar och virtuella datordiskar i Blob Storage-konton?**
   
    Blob Storage-konton stöder endast block- och tilläggsblobar, inte sidblobar. Azure virtuella diskar backas upp av sidblobar och därför Blob storage-konton kan inte använda toostore virtuella diskar. Men det är möjligt toostore säkerhetskopior av hello virtuella diskar som blockblobar i ett Blob storage-konto.

9. **Behöver jag toochange Mina befintliga program toouse Blob storage-konton?**
   
    Blob Storage-konton är API-konsekventa till 100 % med allmänna lagringskonton för block- och tilläggsblobar. Så länge som programmet använder blockblobar eller tilläggsblobar, och du använder hello 2014-02-14-versionen av hello [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) eller större ditt program bör fungera. Om du använder en äldre version av hello protokollet sedan måste du uppdatera den nya versionen för programmet toouse hello så toowork sömlöst med båda typerna av lagringskonton. I allmänhet rekommenderar vi alltid använder hello senaste versionen oavsett vilken lagringskontotyp du använder.

10. **Kommer användarupplevelsen att ändras?**
    
    BLOB storage-konton är mycket lik tooa allmänna lagringskonton för att lagra block och tilläggsblobbar och stöd för alla hello viktiga funktioner i Azure Storage, bland annat hög hållbarhet och tillgänglighet, skalbarhet, prestanda och säkerhet. Förutom hello funktioner och begränsningar för specifika tooBlob storage-konton och dess lagringsnivåer som nämnts ovan är allt hello annat förblir samma.

## <a name="next-steps"></a>Nästa steg

### <a name="evaluate-blob-storage-accounts"></a>Utvärdera Blob Storage-konton

[Kontrollera tillgängligheten för Blob Storage-konton per region](https://azure.microsoft.com/regions/#services)

[Utvärdera användningen av dina aktuella lagringskonton genom att aktivera mätvärden i Azure Storage.](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Se priser för Blob Storage per region](https://azure.microsoft.com/pricing/details/storage/)

[Se priser för dataöverföring](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Börja använda Blob Storage-konton

[Komma igång med Azure Blob Storage](storage-dotnet-how-to-use-blobs.md)

[Flytta data tooand från Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Överföra data med kommandoradsverktyget Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Bläddra igenom och utforska dina lagringskonton](http://storageexplorer.com/)