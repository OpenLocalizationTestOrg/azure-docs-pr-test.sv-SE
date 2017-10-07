---
title: "aaaUse toocollect loggar och mått data för Azure Storage Analytics | Microsoft Docs"
description: "Storage Analytics kan du tootrack mått data för alla storage-tjänster och toocollect loggar för lagring av Blob, köer och tabellen."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7894993b-ca42-4125-8f17-8f6dfe3dca76
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: aba1a236cb69fa2e60bcb8fda5d34841e36e379a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storage-analytics"></a>Lagringsanalys

Med Azure Storage Analytics kan du logga och skapa statistik för ett lagringskonto. Du kan använda den här tootrace begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto.

toouse Storage Analytics, måste du aktivera det individuellt för varje tjänst du vill toomonitor. Du kan aktivera det från hello [Azure Portal](https://portal.azure.com). Mer information finns i [övervaka ett lagringskonto i hello Azure Portal](storage-monitor-storage-account.md). Du kan också aktivera Storage Analytics programmässigt via hello REST API eller klientbibliotek för hello. Använd hello [hämta egenskaper för Blob-tjänsten](https://msdn.microsoft.com/library/hh452239.aspx), [hämta egenskaper för kön](https://msdn.microsoft.com/library/hh452243.aspx), [hämta Service Tabellegenskaper](https://msdn.microsoft.com/library/hh452238.aspx), och [hämta egenskaper för filtjänsten](https://msdn.microsoft.com/library/mt427369.aspx) operations tooenable Storage Analytics för varje tjänst.

hello aggregerade data lagras i en välkänd blob (för loggning) och välkända tabeller (för mått) som kan användas med hello Blob- och tabelltjänsterna API: er.

Storage Analytics har en gräns på 20 TB på hello mängden lagrade data som är oberoende av hello totala gränsen för ditt lagringskonto. Läs mer om fakturering och datakvarhållningsprinciper [Storage Analytics och fakturering för](https://msdn.microsoft.com/library/hh360997.aspx). Läs mer om lagringskontogränser [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md).

Utförliga instruktioner om hur du använder Storage Analytics och andra verktyg tooidentify, diagnostisera i, och felsöka problem med Azure Storage, se [övervaka, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Om Storage Analytics loggning
Storage Analytics loggar detaljerad information om lyckade och misslyckade begäranden tooa storage-tjänst. Den här informationen kan vara används toomonitor enskilda förfrågningar och toodiagnose problem med en storage-tjänst. Begäranden loggas för bästa prestanda.

Loggposter skapas endast om tjänsteaktivitet för lagring. Till exempel skapas ett lagringskonto har aktivitet i dess Blob-tjänsten men inte i tabell- eller Köberoenden tjänsterna, endast loggar som rör toohello Blob-tjänsten.

Storage Analytics loggning är inte tillgänglig för Azure File storage.

### <a name="logging-authenticated-requests"></a>Autentiserade loggningsbegäranden
följande typer av autentiserade begäranden hello loggas:

* Lyckade begäranden.
* Misslyckade begäranden, inklusive timeout, begränsning, nätverk, auktorisering och andra fel.
* Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden.
* Begäranden tooanalytics data.

Förfrågningar som görs av Storage Analytics, till exempel loggen skapas eller tas bort, loggas inte. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://msdn.microsoft.com/library/hh343260.aspx) och [Storage Analytics loggformatet](https://msdn.microsoft.com/library/hh343259.aspx) avsnitt.

### <a name="logging-anonymous-requests"></a>Loggning anonyma förfrågningar
följande typer av anonyma begäranden hello loggas:

* Lyckade begäranden.
* Serverfel.
* Timeout-fel för både klient och server.
* Misslyckade GET-begäranden med felkoden 304 (ändra inte).

Alla andra misslyckade anonyma begäranden har inte loggats. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://msdn.microsoft.com/library/hh343260.aspx) och [Storage Analytics loggformatet](https://msdn.microsoft.com/library/hh343259.aspx) avsnitt.

### <a name="how-logs-are-stored"></a>Hur loggar lagras
Alla loggar lagras i blockblobbar i en behållare med namnet $logs som skapas automatiskt när Storage Analytics har aktiverats för ett lagringskonto. Hej $logs behållaren finns i hello blob namnområdet för hello storage-konto, till exempel: `http://<accountname>.blob.core.windows.net/$logs`. Den här behållaren kan inte tas bort när Storage Analytics har aktiverats, även om dess innehåll kan tas bort.

> [!NOTE]
> Hej $logs behållaren visas inte när en behållare med åtgärden utförs, till exempel hello [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) metod. Den måste kunna nås direkt. Du kan till exempel använda hello [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) metoden tooaccess hello blobbar i hello `$logs` behållare.
> Storage Analytics kommer att som begäranden loggas överföra mellanresultat som block. Med jämna mellanrum Storage Analytics genomför dessa block och göra dem tillgängliga som en blob.
> 
> 

Kan det finnas dubbletter för loggar som skapats i hello samma timme. Du kan fastställa om en post är en dubblett genom att kontrollera hello **RequestId** och **åtgärden** nummer.

### <a name="log-naming-conventions"></a>Logga namngivningskonventioner
Varje logg skrivs i hello följande format.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

hello beskrivs följande tabell alla attribut i hello loggnamn.

| Attribut | Beskrivning |
| --- | --- |
| < tjänstnamn > |hello namnet på hello storage-tjänst. Till exempel: blob-, tabell- eller köberoenden. |
| ÅÅÅÅ |hello fyrsiffriga år för hello-loggen. Till exempel: 2011. |
| MM |hello tvåsiffrig månad för hello logg. Till exempel: 07. |
| DD |hello tvåsiffrig månad för hello logg. Till exempel: 07. |
| hh |hello tvåsiffrig timme som visar hello startar timme för hello loggar i 24-timmarsformat UTC. Till exempel: 18. |
| mm |hello två siffror som visar hello startar minut för hello loggar. Det här värdet stöds inte i hello nuvarande version av Storage Analytics och dess värde ska alltid vara 00. |
| <counter> |En Nollbaserad räknare med sex siffror som visar hello antal loggen blobbar som genererats för hello storage-tjänst på en timme tidsperiod. Den här räknaren startar vid 000000. Till exempel: 000001. |

hello följande är ett fullständigt exempel loggnamn som kombinerar hello föregående exempel.

    blob/2011/07/31/1800/000001.log

hello följande är ett exempel på en URI som kan använda tooaccess hello tidigare loggen.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

När en begäran om lagring loggas korrelerar hello resulterande loggnamn toohello timme när hello begärde åtgärden slutfördes. Om exempelvis en begäran om GetBlob slutfördes klockan 6:30. på 2011-7-31 skulle hello loggen skrivas med hello efter prefix:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Log-metadata
Alla loggen BLOB lagras med metadata som kan använda tooidentify vilka loggning data hello blob innehåller. hello i den följande tabellen beskrivs varje metadataattribut.

| Attribut | Beskrivning |
| --- | --- |
| LogType |Beskriver om hello-loggen innehåller information som rör tooread-, Skriv- eller delete-åtgärder. Det här värdet kan innehålla en typ eller en kombination av alla tre, avgränsade med kommatecken. Exempel 1: skrivning. Exempel 2: läsa, skriva; Exempel 3: läsa, skriva, ta bort. |
| StartTime |Hej tidigaste tidpunkt för en post i loggen för hello i hello formatet ÅÅÅÅ-MM-ddTHH. Till exempel: 2011-07-31T18:21:46Z. |
| Sluttid |Hej senaste tid för en post i loggen för hello i hello formatet ÅÅÅÅ-MM-ddTHH. Till exempel: 2011-07-31T18:22:09Z. |
| LogVersion |hello version av hello loggformat. Hello stöds endast värdet är för närvarande 1.0. |

hello visar följande lista fullständigt exempel metadata med hjälp av hello föregående exempel.

* LogType = skriva
* StartTime = 2011-07-31T18:21:46Z
* EndTime = 2011-07-31T18:22:09Z
* LogVersion = 1.0

### <a name="accessing-logging-data"></a>Åtkomst till loggningsdata
Alla data i hello `$logs` behållare kan nås med hjälp av API: er hello Blob-tjänsten, inklusive hello .NET-API: er som tillhandahålls av hello Azure hanterade biblioteket. Hej lagringskontoadministratören kan läsa och ta bort loggar, men det går inte att skapa eller uppdatera dem. Både hello log-metadata och loggnamn kan användas när du frågar efter en logg. Det är möjligt för en given timme loggar tooappear i ordning, men hello metadata anger alltid hello timespan poster i en logg. Därför kan du använda en kombination av namn på loggen och metadata när du söker efter en viss logg.

## <a name="about-storage-analytics-metrics"></a>Om Storage Analytics mätvärden
Storage Analytics kan lagra mått som innehåller aggregerade statistik och kapacitet transaktionsdata om begäranden tooa storage-tjänst. Transaktioner rapporteras på både åtgärden hello API-nivå samt på hello storage-servicenivå och kapacitet rapporteras vid hello storage-servicenivå. Mätvärdesdata kan vara används tooanalyze lagringskvoten på tjänsten, diagnostisera problem med begäranden som görs mot hello storage-tjänst och tooimprove hello prestanda för program som använder en tjänst.

toouse Storage Analytics, måste du aktivera det individuellt för varje tjänst du vill toomonitor. Du kan aktivera det från hello [Azure Portal](https://portal.azure.com). Mer information finns i [övervaka ett lagringskonto i hello Azure Portal](storage-monitor-storage-account.md). Du kan också aktivera Storage Analytics programmässigt via hello REST API eller klientbibliotek för hello. Använd hello **hämta tjänstegenskaper** operations tooenable Storage Analytics för varje tjänst.

### <a name="transaction-metrics"></a>Transaktionen mått
En kraftfull uppsättning data registreras med timvis eller minuters intervall för varje lagringstjänsten begärde API-åtgärd, inklusive ingång-/ utgång, tillgänglighet, fel, och kategoriseras begäran procenttal. Du kan se en fullständig lista över hello transaktionsinformation i hello [Storage Analytics mätvärden tabellschemat](https://msdn.microsoft.com/library/hh343264.aspx) avsnittet.

Transaktionsdata registreras på två nivåer – hello servicenivå och hello API-nivå för åtgärden. Statistik för sammanfattning av alla begär hello servicenivå-API: et skrivs tooa tabellentiteten varje timme även om inga begäranden har gjorts toohello service. På hello API åtgärden nivå är statistik endast skriftliga tooan entiteten om hello åtgärden begärdes inom den timmen.

Till exempel om du utför en **GetBlob** åtgärden på tjänsten Blob Storage Analytics mätvärden loggar in hello begäran och inkludera den i hello aggregerade data för både hello Blob-tjänsten samt hello **GetBlob** igen. Men om inget **GetBlob** åtgärden begärs under hello timma, en entitet kommer inte att skriva toohello `$MetricsTransactionsBlob` för den åtgärden.

Transaktionen mått registreras för både användarförfrågningar och förfrågningar som görs av Storage Analytics sig själv. Till exempel registreras begäranden av Storage Analytics toowrite loggar och tabellen enheter. Mer information om hur dessa begäranden debiteras finns [Storage Analytics och fakturering för](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapacitet mått
> [!NOTE]
> Kapacitetsdata är för närvarande bara tillgängliga för hello Blob-tjänsten. Kapacitet mätvärden för hello tabelltjänsten och kötjänsten blir tillgänglig i framtida versioner av Storage Analytics.
> 
> 

Kapacitet data registreras dagligen för ett lagringskonto Blob-tjänsten och två tabellentiteter skrivs. En entitet innehåller statistik för användardata och hello andra tillhandahåller statistik om hello `$logs` blob-behållare som används av Storage Analytics. Hej `$MetricsCapacityBlob` tabellen innehåller hello följande statistik:

* **Kapacitet**: hello mängden lagringsutrymme som används av hello lagringskontots Blob-tjänsten, i byte.
* **ContainerCount**: hello antal blobbbehållare i hello lagringskontots Blob-tjänsten.
* **ObjectCount**: hello antal bekräftade och ogenomförda block eller sidan blobbar i hello lagringskontots Blob-tjänsten.

Läs mer om hello kapacitetsdata [Storage Analytics mätvärden tabellschemat](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Hur mått lagras
Alla mätvärdesdata för varje hello lagringstjänster lagras i tre tabeller som är reserverade för tjänsten: en tabell för transaktionsinformation, en tabell för minut transaktionsinformation och en annan tabell för kapacitetsinformation. Transaktionen och minut transaktionsinformation består av data för begäran och svar och kapacitetsinformation består av användningsdata för lagring. Mätvärden för timme, minut mätvärden och kapacitet för ett lagringskonto Blob-tjänsten kan nås i tabeller som är namngivna enligt beskrivningen i följande tabell hello.

| Mått nivå | Tabellnamn | Versioner som stöds |
| --- | --- | --- |
| Varje timme statistik, primär plats |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Versioner tidigare too2013-08-15 endast. När dessa namn stöds fortfarande, bör du växla toousing hello tabeller som anges nedan. |
| Varje timme statistik, primär plats |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Alla versioner, inklusive 2013-08-15. |
| Minut statistik, primär plats |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Alla versioner, inklusive 2013-08-15. |
| Varje timme statistik, sekundär plats |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Alla versioner, inklusive 2013-08-15. Läsbehörighet geo-redundant replikering måste aktiveras. |
| Minut statistik, sekundär plats |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Alla versioner, inklusive 2013-08-15. Läsbehörighet geo-redundant replikering måste aktiveras. |
| Kapacitet (endast Blob-tjänst) |$MetricsCapacityBlob |Alla versioner, inklusive 2013-08-15. |

Dessa tabeller skapas automatiskt när Storage Analytics har aktiverats för ett lagringskonto. De kan nås via hello namnområdet för hello storage-konto, till exempel:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Dataåtkomst mått
Alla data i hello mått tabeller kan komma åt via hello tabelltjänsten API: er, inklusive hello .NET-API: er som tillhandahålls av hello Azure hanterade biblioteket. hello lagringskontoadministratören kan läsa och ta bort tabellentiteter, men det går inte att skapa eller uppdatera dem.

## <a name="billing-for-storage-analytics"></a>Fakturering för Storage Analytics
Alla mätvärdesdata skrivs av hello tjänster för ett lagringskonto. Därför är varje skrivåtgärd som utförs av Storage Analytics fakturerbar. Dessutom är också hello mängden lagringsutrymme som används av mätvärdesdata fakturerbar.

hello följande åtgärder som utförs av Storage Analytics är fakturerbar:

* Begär toocreate BLOB för loggning. 
* Begäranden toocreate tabellentiteter för mått.

Om du har konfigurerat en databevarandeprincip debiteras inte du för att ta borttagningstransaktioner när Storage Analytics tar bort gamla data för loggning och mått. Ta borttagningstransaktioner från en klient är dock fakturerbar. Läs mer om bevarandeprinciper [ange en Storage Analytics Databevarandeprincip](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Förstå fakturerbar begäranden
Varje begäran tooan konto lagringstjänsten är fakturerbar eller icke Debiterbart. Storage Analytics loggar varje enskild begäran tooa tjänsten, inklusive ett statusmeddelande som anger hur hello begäran behandlades. På liknande sätt, lagrar Storage Analytics mätvärden för både en tjänst och hello-API: et för tjänsten, inklusive hello procenttal och antalet vissa statusmeddelanden. Sammantaget ser kan dessa funktioner hjälpa dig analysera dina fakturerbar begäranden, göra förbättringar för ditt program och diagnostisera problem med begäranden tooyour tjänster. Mer information om fakturering finns [förstå Azure Storage fakturerings - bandbredd, transaktioner och kapacitet](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

När du tittar på Storage Analytics data, du kan använda hello tabeller i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://msdn.microsoft.com/library/azure/hh343260.aspx) avsnittet toodetermine vad begär är fakturerbar. Du kan sedan jämföra dina loggar och mått data toohello status meddelanden toosee om du har debiteras för en viss begäran. Du kan också använda hello tabeller i hello föregående avsnittet tooinvestigate tillgänglighet för en lagringstjänsten eller enskilda API-åtgärd.

## <a name="next-steps"></a>Nästa steg
### <a name="setting-up-storage-analytics"></a>Konfigurera Storage Analytics
* [Övervaka ett lagringskonto i hello Azure-portalen](storage-monitor-storage-account.md)
* [Aktivera och konfigurera Storage Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Storage Analytics loggning
* [Om Storage Analytics loggning](https://msdn.microsoft.com/library/hh343262.aspx)
* [Storage Analytics loggformat](https://msdn.microsoft.com/library/hh343259.aspx)
* [Storage Analytics loggade åtgärder och statusmeddelanden](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Storage Analytics mätvärden
* [Om Storage Analytics mätvärden](https://msdn.microsoft.com/library/hh343258.aspx)
* [Storage Analytics mätvärden tabellens Schema](https://msdn.microsoft.com/library/hh343264.aspx)
* [Storage Analytics loggade åtgärder och statusmeddelanden](https://msdn.microsoft.com/library/hh343260.aspx)  

