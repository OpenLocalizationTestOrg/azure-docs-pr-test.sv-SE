---
title: aaaAzure Cosmos DB resursmodell och begrepp | Microsoft Docs
description: "Läs mer om Azure Cosmos DB hierarkiska modell av databaser, samlingar, användardefinierad funktion (UDF), dokument, behörigheter toomanage resurser och mer."
keywords: Hierarkisk modellen cosmosdb, azure, Microsoft azure
services: cosmos-db
documentationcenter: 
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: ef9d5c0c-0867-4317-bb1b-98e219799fd5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: anhoh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc3642232b86cc27901ebd97456c386829324632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Hierarkisk Azure Cosmos DB-resursmodell och huvudkoncept
hello databasenheter som hanterar Azure Cosmos DB är refererad tooas **resurser**. Varje resurs identifieras unikt med en logisk URI. Du kan interagera med hello resurser med hjälp av HTTP-verb som standard, frågor och svar sidhuvuden och statuskoder. 

Läsa den här artikeln om du kommer att kunna tooanswer hello följande frågor:

* Vad är Cosmos DB modell?
* Vad är definierad resurser som skillnad från toouser definierats resurser?
* Hur åtgärdar en resurs?
* Hur fungerar med samlingar?
* Hur fungerar med lagrade procedurer, utlösare och användardefinierade funktioner (UDF)?

## <a name="hierarchical-resource-model"></a>Hierarkiska resursmodell
Eftersom hello följande diagram illustrerar, hello Cosmos DB hierarkiska **resursmodell** består av uppsättningar av resurser under ett databaskonto varje adresserbara via en logisk och stabil URI. En uppsättning resurser blir refererad tooas en **feed** i den här artikeln. 

> [!NOTE]
> Azure Cosmos-DB erbjuder en högeffektiv TCP-protokollet som också RESTful i sin kommunikation modellen tillgängliga via hello [DocumentDB .NET klienten API](documentdb-sdk-dotnet.md).
> 
> 

![Azure DB Cosmos hierarkiska resursmodell][1]  
**Hierarkiska resursmodell**   

toostart arbeta med resurser, måste du [skapa ett databaskonto](create-documentdb-dotnet.md) med din Azure-prenumeration. Ett databaskonto kan bestå av en uppsättning **databaser**, var och en innehåller flera **samlingar**, varje av som i sin tur innehåller **lagrade procedurer, utlösare, UDF: er, dokument**och relaterade **bilagor**. En databas har också associerade **användare**, var och en med en uppsättning **behörigheter** tooaccess samlingar, lagrade procedurer, utlösare, UDF: er, dokument eller bilagor. Databaser, användare, behörigheter och samlingar är systemdefinierade resurser med välkända scheman, dokument och bilagor innehåller godtyckligt, användardefinierat JSON-innehåll.  

| Resurs | Beskrivning |
| --- | --- |
| Databaskonto |Ett databaskonto är associerad med en uppsättning databaser och en fast mängd blob-lagring för bifogade filer. Du kan skapa en eller flera databasen konton med hjälp av din Azure-prenumeration. Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Databas |En databas är en logisk behållare för dokumentlagring, partitionerad över samlingarna. Det är en användarbehållare. |
| Användare |hello logiska namnområde scope behörigheter. |
| Behörighet |En autentiseringstoken som är kopplad till en användare för åtkomst tooa viss resurs. |
| Samling |En samling är en behållare för JSON-dokument och hello associerad JavaScript-programlogik. En samling är en fakturerbar enhet där hello [kostnaden](performance-levels.md) bestäms av hello prestandanivå hello samling. Samlingar kan omfatta en eller flera partitioner/servrar och kan skalas toohandle praktiskt taget obegränsade volymer av lagring eller dataflöde. |
| Lagrad procedur |Programlogik skriven i JavaScript som är registrerade med en samling och transaktionellt köras inom hello databasmotorn. |
| Utlösare |Programlogik som skrivits i JavaScript utföras före eller efter antingen ett insert-, ersätta eller ta bort igen. |
| UDF |Programlogik skriven i JavaScript. UDF: aktivera toomodel en anpassad fråga operator och utöka därmed hello core DocumentDB API-frågespråket. |
| Dokumentet |Användardefinierat (godtyckligt) JSON-innehåll. Inget schema måste toobe definieras som standard eller sekundärindex behöver toobe anges för alla hello dokument till tooa samling. |
| Bifogad fil |En bilaga är ett särskilt dokument som innehåller referenser och associerade metadata för extern blob/media. hello utvecklare kan välja toohave hello blob som hanteras av Cosmos DB eller lagra med en extern blob-leverantör OneDrive, Dropbox, t.ex. |

## <a name="system-vs-user-defined-resources"></a>Kontra användaren systemdefinierade resurser
Resurser, till exempel databasen konton, databaser, samlingar, användare, behörigheter, lagrade procedurer, utlösare och UDF - alla har en fast schema och kallas systemresurser. Däremot har inga begränsningar hello schemat resurser, t.ex dokument och bilagor och är exempel på användardefinierade resurser. I Cosmos DB, både system- och definierade resurser representeras och hanteras som standard-kompatibel JSON. Alla resurser, system- eller användardefinierade, ha hello följande gemensamma egenskaper.

> [!NOTE]
> Observera att alla systemgenererat egenskaper i en resurspartnerorganisation prefix med ett understreck (_) i sina JSON-representation.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Egenskap</strong></p></td>
            <td valign="top"><p><strong>Ange användar- eller genereras av systemet?</strong></p></td>
            <td valign="top"><p><strong>Syfte</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Genereras av systemet</p></td>
            <td valign="top"><p>Systemgenererat, unikt och hierarkiska ID hello resurs</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Genereras av systemet</p></td>
            <td valign="top"><p>ETag hello-resurs som krävs för optimistisk samtidighetskontroll</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Genereras av systemet</p></td>
            <td valign="top"><p>Senast uppdaterad tidsstämpeln för hello resurs</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Genereras av systemet</p></td>
            <td valign="top"><p>Unikt adresserbara hello resurs-URI</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Genereras av systemet</p></td>
            <td valign="top"><p>Användardefinierad unikt namn för hello resurs (med hello partitions samma nyckelvärde). Om hello användare inte anger ett id, kommer ett id som att genereras av systemet</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Överföring representation av resurser
Cosmos DB tvingade inte eventuella egna tillägg toohello JSON standard eller särskilda kodningar; den fungerar med standard kompatibla JSON-dokument.  

### <a name="addressing-a-resource"></a>En resurs-adressering
Alla resurser är adresserbara URI. Hej värdet för hello **_self** egenskapen för en resurs representerar hello relativ URI för hello resurs. hello hello URI-formatet består av hello /\<feed\>/ {_rid} sökvägssegment:  

| Värdet för hello _self | Beskrivning |
| --- | --- |
| /DBS |Feed med databaser under ett databaskonto |
| /DBS/ {dbName} |Databasen med id matchar hello värdet {dbName} |
| /DBS/ {dbName} /colls/ |Feeden av samlingar under en-databas |
| /DBS/ {dbName} /colls/ {collName} |Samling med id matchar hello värdet {collName} |
| /DBS/ {dbName} /colls/ {collName} / docs |Feeden dokument under en samling |
| /DBS/ {dbName} /colls/ {collName} /docs/ {docId} |Dokument med ett id som matchar hello värdet {doc} |
| /DBS/ {dbName} /users/ |Flöde för användare under en-databas |
| /DBS/ {dbName} /users/ {userId} |Användare med ett id som matchar hello värdet {user} |
| /DBS/ {dbName} /users/ {userId} / behörigheter |Feeden behörigheter under en användare |
| /DBS/ {dbName} /users/ {userId} /permissions/ {permissionId} |Behörighet med id matchar hello värdet {behörighet} |

Varje resurs har en unik användardefinierad namn som exponeras via hello id-egenskap. Obs: för dokument, om hello användare inte anger ett id våra stöds SDK: er att automatiskt generera ett unikt id för hello dokumentet. hello-id är en användardefinierad sträng av upp too256 tecken som är unikt i hello kontexten för en viss överordnade resurs. 

Varje resurs har också en systemgenererad hierarkiska resource identifier (även hänvisade tooas ett RID), som är tillgänglig via hello _rid egenskapen. hello RID kodar hello hela hierarkin för en viss resurs och det är en lämplig interna representation används tooenforce referensintegritet på ett sätt som är distribuerad. hello RID är unika inom ett databaskonto och används internt av Cosmos DB för effektiv routning utan mellan partition sökningar. hello värdena för hello _self och hello _rid egenskaper är både alternativa och kanoniska representationer av en resurs. 

hello REST API: er stöd för adressering av resurser och routning av begäran av både hello-id och hello _rid egenskaper.

## <a name="database-accounts"></a>Databasen konton
Du kan etablera ett eller flera Cosmos DB databasen konton med din Azure-prenumeration.

Du kan skapa och hantera konton för Cosmos-DB-databasen via hello Azure-portalen på [http://portal.azure.com/](https://portal.azure.com/). Skapa och hantera ett databaskonto kräver administratörsbehörighet och kan bara genomföras under din Azure-prenumeration. 

### <a name="database-account-properties"></a>Egenskaper för databas
Som en del av etablera och hantera ett konto kan du konfigurera och läsa hello följande egenskaper:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Egenskapsnamn</strong></p></td>
            <td valign="top"><p><strong>Beskrivning</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Princip för konsekvenskontroll</p></td>
            <td valign="top"><p>Ange den här egenskapen tooconfigure hello konsekvenskontroll standardnivån för alla hello samlingar under databaskontot för. Du kan åsidosätta hello konsekvensnivå på grundval av per begäran med hjälp av hello [x-ms-konsekvens-nivå] huvudet i begäran. <p><p>Observera att den här egenskapen endast tillämpar toohello <i>användardefinierad resurser</i>. Alla systemdefinierade resurser är konfigurerade toosupport läsningar/frågor med stark konsekvens.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Auktorisering nycklar</p></td>
            <td valign="top"><p>Dessa är hello primära och sekundära huvud- och readonly nycklar som ger administrativa åtkomst tooall hello resurser under hello konto.</p></td>
        </tr>
    </tbody>
</table>

Observera att i tillägg tooprovisioning, konfigurera och hantera ditt konto från hello Azure Portal även programmässigt skapa och hantera konton för Cosmos-DB-databas med hjälp av hello [Azure Cosmos DB REST API: er](/rest/api/documentdb/) som samt [client SDK](documentdb-sdk-dotnet.md).  

## <a name="databases"></a>Databaser
En Cosmos-DB-databas är en logisk behållare för en eller flera samlingar och användare, som visas i följande diagram hello. Du kan skapa valfritt antal databaser under en Cosmos-DB databasen konto ämne toooffer gränser.  

![Konto och samlingar hierarkiska databasmodellen][2]  
**En databas är en logisk behållare för användare och samlingar**

En databas kan innehålla praktiskt taget obegränsade dokumentlagring, partitionerad i samlingar.

### <a name="elastic-scale-of-a-cosmos-db-database"></a>Elastisk skalbarhet av en Cosmos-DB-databas
En Cosmos-DB-databas är elastisk standard-allt från några få GB toopetabytes SSD backas upp dokumentlagring och etablerat dataflöde. 

Till skillnad från en databas i traditionella RDBMS är databasen i Cosmos-databasen inte begränsade tooa enskild dator. Med Cosmos DB när skala ditt program behöver toogrow, kan du skapa flera samlingar, databaser eller båda. Faktiskt har olika första partsprogram i Microsoft använt Cosmos-DB i en konsument skala genom att skapa mycket stora Cosmos DB databaser varje med tusentals samlingar med terabytes dokumentlagring. Du kan öka eller minska en databas genom att lägga till eller ta bort samlingar toomeet skala programkrav. 

Du kan skapa valfritt antal samlingar i en databas subject toohello erbjudandet. Varje samling har säkerhetskopieras SSD-lagringen och dataflödet du beroende på hello valda prestandanivå.

En Cosmos-DB-databas är också en behållare för användare. En användare i sin tur är ett logiskt namnområde för en uppsättning behörigheter som ger detaljerad auktorisering och toocollections, dokument och bifogade filer.  

Eftersom andra resurser i hello Cosmos DB resursmodell databaser kan skapas, ersatts, tas bort, läsa eller räkna upp enkelt med hjälp av antingen hello [REST API: er](/rest/api/documentdb/) eller någon av hello [client SDK](documentdb-sdk-dotnet.md). Cosmos DB garanterar stark konsekvens för läsning eller fråga hello metadata för databasen. Ta bort en databas automatiskt garanterar att du inte kan komma åt någon av hello samlingar eller användare som finns i den.   

## <a name="collections"></a>Samlingar
En Cosmos-DB-samling är en behållare för JSON-dokument. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastisk SSD säkerhetskopieras dokumentlagring
En samling är filsystemen elastisk - och den automatiskt växer förminskas när du lägger till eller ta bort dokument. Samlingar är logiska nätverksresurser och kan sträcka sig över en eller flera partitioner fysiska servrar. hello antalet partitioner i en samling bestäms av Cosmos DB baserat på hello lagringsstorlek och hello etablerat dataflöde för din samling. Varje partition i Cosmos-databasen har en fast mängd SSD-baserad lagring som är kopplad till den och replikeras för hög tillgänglighet. Partitionen management hanteras helt av Azure Cosmos DB och du inte har toowrite komplex kod eller hantera din partitioner. Cosmos DB samlingar är **praktiskt taget obegränsade** vad gäller lagring och genomflöde. 

### <a name="automatic-indexing-of-collections"></a>Automatisk indexering av samlingar
Cosmos DB är ett sant schemafria databassystem. Varken antar eller kräver ett schema för hello JSON-dokument. När du lägger till dokument tooa samling Cosmos DB indexerar automatiskt dem och de är tillgängliga för du tooquery. Automatisk indexering av dokument utan schemat eller sekundärindex är en funktion som är viktiga för Cosmos-DB och aktiveras av write-optimerad, frigöra Lås och loggstrukturerad index Underhåll tekniker. Cosmos DB stöder varaktigt mängden mycket snabba skrivningar när betjänar konsekvent frågor. Både dokumentet och index lagring är används toocalculate hello lagringsutrymme som förbrukas av varje samling. Du kan styra hello lagrings- och prestandakrav kompromissa vad som är associerade med indexering genom att konfigurera hello indexprincip för en samling. 

### <a name="configuring-hello-indexing-policy-of-a-collection"></a>Konfigurera hello indexprincip i en samling
hello indexering princip av varje samling kan du toomake prestanda och lagring avvägningarna som är associerade med indexering. hello är följande alternativ tillgängliga tooyou som en del av indexerings-konfigurationen:  

* Välj om hello samling indexerar automatiskt alla hello dokument eller inte. Alla dokument indexeras automatiskt som standard. Du kan välja tooturn av den automatiska indexeringen och selektivt lägga till endast specifika dokument toohello index. Däremot kan du selektivt välja tooexclude endast vissa dokument. Du kan åstadkomma detta genom att ange hello automatisk egenskapen toobe SANT eller FALSKT på hello indexingPolicy i en samling med hello [x-ms-indexingdirective] begärandehuvudet när lägga till, ersätta eller ta bort ett dokument.  
* Välj om tooinclude eller undanta specifika sökvägar eller mönster i dina dokument från hello index. Du kan respektive åstadkomma detta genom att inställningen includedPaths och excludedPaths på hello indexingPolicy i en samling. Dessutom kan du konfigurera hello lagrings- och prestandakrav avvägningarna för intervall och hash-frågor för angiven sökväg mönster. 
* Välj mellan synkron (konsekvent) och uppdateringar av index för asynkron (lazy). Som standard uppdateras hello index synkront på varje insert-, ersätta eller borttagning av en dokumentsamling toohello. Detta möjliggör hello frågor toohonor hello samma konsekvensnivå som hello dokumentet läsningar. När Cosmos DB är skrivåtgärder optimerad och stöder varaktigt volymer av dokumentet skrivningar tillsammans med synkron index underhåll och betjänar konsekvent frågor, kan du konfigurera vissa samlingar tooupdate deras index lazy. Lazy indexering förstärker hello skrivprestanda ytterligare och är idealisk för bulk införandet scenarier för främst frekventa Läs samlingar.

hello indexering principen kan ändras genom att köra en PUT på hello samling. Detta kan vara uppnås antingen via hello [klient-SDK](documentdb-sdk-dotnet.md), hello [Azure Portal](https://portal.azure.com) eller hello [REST API: er](/rest/api/documentdb/).

### <a name="querying-a-collection"></a>Fråga till en samling
hello dokument i en samling kan ha godtyckliga scheman och du kan fråga dokument i en samling utan att ange något schema eller sekundärindex förskott. Du kan fråga hello samling med hello [Azure Cosmos DB DocumentDB API: referens för SQL-syntax](https://msdn.microsoft.com/library/azure/dn782250.aspx), vilket ger omfattande hierarkiska relationella och spatial operatorer och utvidgas via JavaScript-baserade UDF: er. JSON-grammatik gör det möjligt för att modellera JSON-dokument som träd med etiketter som trädnoder hello. Både av DocumentDB API-metoder för automatisk indexering samt DocumentDB API SQL dialect utnyttjas. Hej DocumetDB API-frågespråket består av tre huvudsakliga aspekter:   

1. En liten uppsättning frågeåtgärder som mappar naturligt toohello trädstrukturen inklusive hierarkiska frågor och projektioner. 
2. En delmängd av relationella operations inklusive sammansättning, filter, projektioner, mängder och self kopplingar. 
3. Ren JavaScript-baserade UDF: er som fungerar med (1) och (2).  

Hej Cosmos DB frågemodell försöker toostrike balans mellan funktionerna, effektivitet och enkelhet. Hej Cosmos-databasen databasmotor internt kompileras och körs hello SQL-uttryck i frågan. Du kan ställa frågor till en samling med hello [REST API: er](/rest/api/documentdb/) eller någon av hello [client SDK](documentdb-sdk-dotnet.md). hello .NET SDK innehåller en LINQ-providern.

> [!TIP]
> Du kan testa hello DocumentDB-API och köra SQL-frågor mot vår datauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Flera dokument transaktioner
Databastransaktioner ger en säker och förutsägbar programmeringsmodell för att hantera samtidiga ändringar toohello data. I RDBMS, hello traditionella sätt toowrite affärslogik är toowrite **lagrade procedurer** och/eller **utlösare** och levererar den toohello databasserver för transaktionell körning. I RDBMS är hello programutvecklare obligatoriska toodeal med två olika programmeringsspråk: 

* hello (icke-transaktionell) programmet programmeringsspråk (t.ex. JavaScript, Python, C#, Java, etc.)
* T-SQL, hello transaktionella programmeringsspråk som utförs internt av hello-databasen

Tack vare dess djup åtagande tooJavaScript och JSON direkt i databasmotorn hello Cosmos DB är ett intuitivt programmeringsmodell för verkställande JavaScript programlogik direkt utifrån hello samlingar som lagrade procedurer och utlösare. Detta möjliggör både hello följande:

* Effektiv implementering av samtidighet styra, återställning av automatisk indexering av hello JSON objektdiagram direkt i databasmotorn hello
* Naturligt uttrycka Kontrollflöde, variabel scope, tilldelning och integration av undantagshantering primitiver med databastransaktioner direkt vad gäller hello JavaScript-programmeringsspråket

hello JavaScript-logik som registrerats på en samlingsnivå kan sedan utfärda databasåtgärder i hello dokument av hello angivna samlingen. Cosmos DB implicit radbryter hello JavaScript baserat lagrade procedurer och utlösare inom en omgivande ACID-transaktioner med ögonblicksbildisolering över dokument i en samling. Under hello att körningen avbryts om JavaScript genererar ett undantag hello hello sedan hela transaktionen. hello resulterande programmeringsmodell är ett enkelt men ändå kraftfulla. JavaScript-utvecklare får en ”varaktiga” programmeringsmodell när du använder fortfarande sin bekant språkkonstruktioner och biblioteket primitiver.   

hello möjlighet tooexecute JavaScript direkt i hello databasen motorn i hello samma adressutrymmet som hello buffertpool aktiverar performant och transaktionell körning av databasåtgärder mot hello dokument i en samling. Dessutom Cosmos-databasen databasmotor gör en djup åtagande toohello JSON och JavaScript åtgärdar eventuella impedans matchningsfel mellan hello typ av program och hello-databasen.   

Efter att en samling kan du registrera lagrade procedurer, utlösare och UDF: er med en samling med hello [REST API: er](/rest/api/documentdb/) eller någon av hello [client SDK](documentdb-sdk-dotnet.md). Du kan referera till och köra dem efter registreringen. Överväg att hello följande lagrade proceduren helt skrivna i JavaScript, hello koden nedan tar två argument (namn och författarens namn) och skapar ett nytt dokument, frågar om ett dokument och uppdaterar sin – alla inom en implicit ACID-transaktion. Om en JavaScript-undantagsfel utlöses när som helst under körning av hello avbryter hello hela transaktionen.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

hello-klienten kan ”levereras” Hej ovanför JavaScript-logik toohello databasen för transaktionell körning via HTTP POST. Mer information om hur du använder HTTP-metoderna finns [RESTful interaktioner med Azure Cosmos DB resurser](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Observera att eftersom hello databasen förstår internt JSON och JavaScript, det finns inga typmatchningsfel system, ingen ”eller mappning” eller code generation Magiskt tal krävs.   

Lagrade procedurer och utlösare interagera med en samling och hello dokument i en samling via en väldefinierad objektmodell som visar hello samling kontext.  

Samlingar i hello DocumentDB API kan skapas, tas bort, läs- eller räkna upp enkelt med hjälp av antingen hello [REST API: er](/rest/api/documentdb/) eller någon av hello [client SDK](documentdb-sdk-dotnet.md). Hej DocumentDB API ger alltid stark konsekvens för läsning eller fråga hello metadata i en samling. Om du tar bort en samling automatiskt säkerställer att du kan inte komma åt någon av hello dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er som finns i den.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Lagrade procedurer, utlösare och användaren definierat funktioner (UDF)
Du kan skriva programmet logik toorun direkt i en transaktion i hello databasmotorn enligt beskrivningen i föregående avsnitt i hello. hello programlogik kan vara helt skrivna i JavaScript och kan modelleras som en lagrad procedur, utlösare eller en UDF. hello JavaScript-kod i en lagrad procedur eller en utlösare kan infoga, Ersätt, ta bort, läs- eller dokument i en samling. Hej på andra sidan, hello JavaScript inom en UDF går inte att infoga, ersätta eller ta bort dokument. UDF: er hello dokument för en frågeresultatet att räkna upp och skapar en annan resultatuppsättning. Cosmos DB tillämpar en strikt reservation baserat resurs styrning för flera innehavare. Alla lagrade procedurer, utlösare eller en UDF hämtar en fast quantum av operativsystemet resurser toodo sitt arbete. Dessutom lagrade procedurer, utlösare eller UDF: er kan inte länka mot externa JavaScript-bibliotek och övriga om de överskrider hello resurs budgetar fördelade toothem. Du kan registrera, avregistrera lagrade procedurer, utlösare eller UDF: er med en samling med hjälp av hello REST API: er.  Vid registreringen en lagrad procedur, utlösare eller en UDF före kompileras och lagras som bytekod som genomförs senare. hello efter avsnittet visar hur du kan använda hello Cosmos DB JavaScript SDK tooregister, köra och avregistrera en lagrad procedur, utlösare och en UDF. hello JavaScript SDK är en enkel omslutning över hello [REST API: er](/rest/api/documentdb/). 

### <a name="registering-a-stored-procedure"></a>Registrera en lagrad procedur
Registrering av en lagrad procedur skapar en ny resurs lagrad procedur på en samling via HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Kör en lagrad procedur
Körningen av en lagrad procedur görs genom att utfärda en HTTP POST mot en befintlig resurs lagrade proceduren genom att ange parametrar toohello procedur i hello begärandetexten.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Avregistrerar en lagrad procedur
Avregistrerar en lagrad procedur görs bara genom att utfärda ett HTTP DELETE mot en befintlig resurs lagrad procedur.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registrera en före utlösare
Registrering av en utlösare görs genom att skapa en ny utlösare resurs på en samling via HTTP POST. Du kan ange om hello utlösare är en eller en post utlösare och hello typ av åtgärd kan den vara associerad med (t.ex. Skapa, ersätta, ta bort eller alla).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Köra en före utlösare
Körningen av en utlösare görs genom att ange hello namn för en befintlig utlösare när hello hello POST/PUT/ta bort utfärdas av en resurs som dokument via hello huvudet i begäran.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in hello Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Avregistrerar en före utlösare
Avregistrerar en utlösare sker bara via utfärdar en HTTP-ta bort mot en befintlig resurs för utlösare.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registrera en UDF
Registrering av en UDF görs genom att skapa en ny UDF-resurs på en samling via HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-hello-query"></a>Köra en UDF som en del av hello fråga
En UDF kan anges som en del av hello SQL-fråga och används som en sätt tooextend hello kärna [SQL-frågespråket för hello DocumentDB API](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Avregistrerar en UDF
Avregistrerar en UDF görs bara genom att utfärda ett HTTP DELETE mot en befintlig UDF-resurs.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Även om hello kodavsnitt ovan visade hello-registrering (POST), avregistreringen (PUT), Läs-/ Liståtgärder (GET) och körningen (POST) via hello [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), du kan också använda hello [REST API: er](/rest/api/documentdb/) eller andra [client SDK](documentdb-sdk-dotnet.md). 

## <a name="documents"></a>Dokument
Du kan infoga, Ersätt, ta bort, Läs, räkna upp och fråga godtyckliga JSON-dokument i en samling. Cosmos DB tvingade inte alla scheman och kräver inte sekundärindex i ordning toosupport frågealternativ över dokument i en samling. hello maxstorleken för ett dokument är 2 MB.   

Som en tjänst verkligen öppna databasen Cosmos DB inte hittar några särskilda datatyper (t.ex. tidsvärdet) eller specifika kodningar för JSON-dokument. Observera att Cosmos DB inte kräver några särskilda JSON konventioner toocodify hello relationerna mellan olika dokument. hello SQL-syntaxen för Cosmos DB ger mycket kraftfulla hierarkiska och relationella frågeoperatorer tooquery och projekt dokument utan speciella anteckningar eller behöver toocodify relationerna mellan dokument med hjälp av egenskaper för unika.  

Som med alla andra resurser kan du skapa dokument, ersättas, tas bort, Läs-, räkna upp och frågas enkelt använda REST API: er eller någon av hello [client SDK](documentdb-sdk-dotnet.md). Ta bort ett dokument direkt Frigör hello kvoten motsvarande tooall hello kapslade bifogade filer. hello läsa konsekvensnivå dokument följer hello konsekvent princip på hello konto. Den här principen kan åsidosättas på grundval av per begäran beroende på kraven för konsekvenskontroll av ditt program. När du frågar dokument läsa hello konsekvent sätt hello indexering inställd på hello samling. Det följer hello konto konsekvent princip för ”konsekvent”. 

## <a name="attachments-and-media"></a>Bifogade filer och media
Cosmos DB kan toostore binära blobbar/media med Cosmos DB (högst 2 GB per konto) eller tooyour egna remote media store. Du kan också toorepresent hello metadata för ett medium i ett särskilt dokument kallas bifogad fil. Bifogad fil i Cosmos-databasen är ett särskilt (JSON) dokument som referenser hello media/blob som lagras på annan plats. En bilaga är bara ett särskilt dokument som samlar in hello metadata (t.ex. plats, författare o.s.v.) för ett medium som lagras i en fjärransluten media storage. 

Överväg att ett sociala läsning program som använder Cosmos DB toostore pennanteckningar och metadata, inklusive kommentarer, detaljer, bokmärken, klassificering, om/favoritsporter etc. som är kopplad till en e-bok för en viss användare.   

* hello hello book själva innehållet lagras i hello media antingen tillgängligt som en del av Cosmos DB databaskonto eller en fjärransluten mediebutik. 
* Ett program kan lagra metadata för varje användare som distinkta dokument – t.ex. Toms metadata för Bok1 lagras i ett dokument som refereras av /colls/joe/docs/book1. 
* Bifogade filer pekar toohello innehållssidor i en viss bok för en användare lagras under hello motsvarande dokument t.ex. /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 osv. 

Observera att hello exemplen ovan används eget ID tooconvey hello resurs hierarki. Resurser kan nås via hello REST API: er via unik resurs-ID: n. 

För hello media som hanteras av Cosmos DB, kommer hello _media-egenskapen för hello bilaga referera hello media med hjälp av dess URI. Cosmos DB säkerställer toogarbage samla in hello media när alla hello utestående referenser tas bort. Cosmos DB automatiskt genererar hello bifogad fil när du överför hello nytt media och fyller hello _media toopoint toohello nytillagda media. Om du väljer toostore hello media i en fjärransluten blobstore som hanteras av dig (t.ex. OneDrive, Azure Storage, DropBox osv.), kan du fortfarande använda bilagor tooreference hello media. I så fall måste du skapa hello bilaga själv och fylla i egenskapen _media.   

Precis som med alla andra resurser, bifogade filer kan skapas, ersätts, tas bort, läsa eller räkna upp enkelt använda REST API: er eller någon av hello klienten SDK: er. Följer hello konsekvent princip på hello databaskonto med dokument, läsa hello konsekvensnivå bifogade filer. Den här principen kan åsidosättas på grundval av per begäran beroende på kraven för konsekvenskontroll av ditt program. När du frågar efter bilagor läsa hello konsekvent sätt hello indexering inställd på hello samling. Det följer hello konto konsekvent princip för ”konsekvent”. 
 

## <a name="users"></a>Användare
En Cosmos-DB-användare representerar ett logiskt namnområde för gruppering av behörigheter. En Cosmos-DB-användare kan motsvarar tooa användare i ett identity-system eller en fördefinierad programrollen. För Cosmos DB representerar en användare helt enkelt en abstraktion toogroup en uppsättning behörigheter under en databas.   

Du kan skapa användare i Cosmos-databasen som motsvarar tooyour faktiska användare eller hello innehavare av ditt program för att implementera flera innehavare i ditt program. Du kan sedan skapa behörigheter för en viss användare som motsvarar toohello åtkomstkontroll över olika samlingar, dokument, bilagor och så vidare.   

Som dina program behöver tooscale med dina användare tillväxt, kan du anta olika sätt tooshard dina data. Du kan ange modellen varje användare på följande sätt:   

* Varje användare mappar tooa databas.
* Varje användare mappar tooa samling. 
* Dokument motsvarande toomultiple användare gå tooa dedikerad samling. 
* Dokument motsvarande toomultiple användare gå tooa uppsättning samlingar.   

Oavsett hello specifika horisontell partitionering strategi du väljer du modellera faktiska användarna som användare i Cosmos-DB-databas och associera användare med bra metataggkontroll behörighet tooeach.  

![Användarsamlingar][3]  
**Horisontell partitionering strategier och modellering användare**

Precis som alla andra resurser användare i Cosmos-DB kan skapas, ersätts, tas bort, läsa eller räknas upp enkelt använda REST API: er eller någon av hello klienten SDK: er. Cosmos DB ger alltid stark konsekvens för läsning eller fråga hello metadata för en användarresurs. Det är värt pekar på att ta bort en användare automatiskt garanterar att du inte kan komma åt någon hello behörigheter som finns i den. Även om hello Cosmos DB återtar hello kvoten hello behörigheter som en del av hello bort användare i hello bakgrund, hello bort behörigheter är tillgänglig direkt igen för du toouse.  

## <a name="permissions"></a>Behörigheter
Ur ett access control resurser, till exempel databasen konton, databaser, användare och behörigheten anses *administrativa* resurser eftersom de kräver administratörsbehörighet. Hello på andra sidan resurser inklusive hello samlingar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er omfång under en viss databas och anses vara *programresurser*. Motsvarande toohello två typer av resurser och hello roller som kommer åt dem (dvs hello administratörs- och) hello auktoriseringsmodellen definierar två typer av *åtkomstnycklar*: *huvudnyckeln* och *Resursnyckeln*. hello huvudnyckel är en del av hello databaskonto och tillhandahålls toohello utvecklare (eller administratör) som etablerar hello konto. Den här huvudnyckeln har administratören semantik i att det kan vara används tooauthorize åtkomst tooboth administrativa och programresurser. En Resursnyckeln är däremot ett detaljerade åtkomstnyckel som tillåter åtkomst tooa *specifika* programresursen. Det innebär den fångar upp hello förhållandet mellan hello användare av en databas och hello behörigheter hello användare för en viss resurs (t.ex. samling dokument, bifogad fil, lagrade procedurer, utlösare eller UDF).   

hello endast sätt tooobtain en Resursnyckeln är genom att skapa en behörighet resurs under en viss användare. Observera att i ordning toocreate eller hämta behörighet, en huvudnyckel måste vara angiven på hello authorization-huvud. En behörighet resurs ties hello resurs, dess åtkomst och hello användare. När du har skapat en behörighet resurs behöver hello användare bara toopresent hello associerade Resursnyckeln i ordning toogain åtkomst toohello relevant resurs. Därför kan en Resursnyckeln ses som en logisk och compact representation av hello behörighet för resursen.  

Precis som med alla andra resurser, behörigheter i Cosmos DB kan skapas, ersätts, tas bort, läsa eller räkna upp enkelt använda REST API: er eller någon av hello klienten SDK: er. Cosmos DB ger alltid stark konsekvens för läsning eller fråga hello metadata för en behörighet. 

## <a name="next-steps"></a>Nästa steg
Mer information om att arbeta med resurser med hjälp av HTTP-kommandon i [RESTful interaktioner med Cosmos DB resurser](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

