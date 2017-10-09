---
title: aaaPerformance tips - Azure Cosmos DB NoSQL | Microsoft Docs
description: "Läs client configuration alternativ tooimprove Azure Cosmos DB databasprestanda"
keywords: hur tooimprove-databasprestanda
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 94ff155e-f9bc-488f-8c7a-5e7037091bb9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: mimig
ms.openlocfilehash: 4f3e82ae5048e3dbc20b0fd891a2d3aa22adf3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tips-for-azure-cosmos-db"></a>Prestandatips för Azure Cosmos DB
Azure Cosmos-DB är en snabb och flexibel distribuerad databas som kan skalas sömlöst med garanterad svarstid och genomströmning. Du inte har ändrats och toomake större arkitektur eller skriva komplex kod tooscale databasen med Cosmos DB. Skala upp och ner är lika enkelt som att göra en enda API-anrop eller [SDK-anrop](set-throughput.md#set-throughput-sdk). Eftersom Cosmos-databasen är tillgänglig emellertid nätverket anrop det klientsidan optimeringar du tooachieve topprestanda.

Så om du begär ”hur kan jag förbättra Mina databasprestanda”? Tänk hello följande alternativ:

## <a name="networking"></a>Nätverk
<a id="direct-connection"></a>

1. **Anslutningsprincip för: Använd direktanslutning läge**

    Hur en klient ansluter tooCosmos DB har stor betydelse för prestanda, särskilt observerade klientens svarstid. Det finns två viktiga konfigurationsinställningar för att konfigurera klienten anslutningsprincip – hello anslutning *läge* och hello [anslutning *protokollet*](#connection-protocol).  hello två lägena finns:

   1. Gateway-läge (standard)
   2. Direkt-läge

      Gateway-läge stöds på alla SDK-plattformar och är hello konfigurerad som standard.  Om programmet körs i ett företagsnätverk med strikt brandväggsbegränsningar, är Gateway-läge hello bästa valet eftersom den använder hello standard HTTPS-porten och en enda slutpunkt. Hej prestanda förhållandet, är dock att Gateway-läge innebär ett hopp ytterligare nätverk varje gång data har lästs eller skrivits tooCosmos DB. Därför ger direkt läge bättre prestanda på grund av toofewer nätverkshopp.
<a id="use-tcp"></a>
2. **Anslutningsprincip för: Använd hello TCP-protokoll**

    När du använder direkt läge, finns det två tillgängliga protokollalternativ:

   * TCP
   * HTTPS

     Cosmos DB erbjuder ett enkelt och öppna RESTful-programmeringsmiljö via HTTPS. Dessutom erbjuder en effektiv TCP-protokoll som är också RESTful i dess kommunikation modell och är tillgänglig via hello .NET klient-SDK. Använda SSL för första autentiseringen och kryptering trafik direkt TCP- och HTTPS. Använd hello TCP-protokoll om det är möjligt för bästa prestanda.

     När du använder TCP i Gateway-läge, TCP-Port 443 är hello Cosmos DB port och 10255 är hello MongoDB API-port. När du använder TCP i direkt läge i tillägg toohello Gateway hamnar du behöver tooensure hello port intervallet mellan 10000 och 20000 är öppen eftersom Cosmos DB använder dynamiska TCP-portar. Om dessa portar inte är öppna och försök toouse TCP, felmeddelandet 503 tjänsten är inte tillgänglig.

     hello anslutningsläget konfigureras under hello konstruktionen av hello DocumentClient instans med hello ConnectionPolicy parameter. Om direkt läge används, kan du också ange hello protokoll inom hello ConnectionPolicy parameter.

    ```C#
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from hello Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    Eftersom TCP stöds endast i direkt läge om Gateway-läget används, sedan använda hello HTTPS-protokollet är alltid toocommunicate med hello Gateway och hello protokollvärde i hello ConnectionPolicy ignoreras.

    ![Bild av hello Azure DB som Cosmos-princip](./media/performance-tips/connection-policy.png)

3. **Anropa OpenAsync tooavoid Start svarstid för första begäran**

    Som standard har hello första begäran en högre latens eftersom den har toofetch hello adress-routningstabellen. tooavoid den här fördröjningen för start på hello först begära, du måste anropa OpenAsync() en gång under initieringen på följande sätt.

        await client.OpenAsync();
   <a id="same-region"></a>
4. **Samordna klienter i samma Azure-region för prestanda**

    Om möjligt, för alla program som anropar Cosmos-DB hello samma region som hello Cosmos DB-databasen. Anropar en ungefärlig jämförelse tooCosmos DB i samma region slutföra inom 1 – 2 ms, men hello fördröjning mellan hello västra och av hello USA är hello > 50 ms. Den här fördröjningen kan förmodligen variera från begäran toorequest beroende på hello väg hello begäran när det överförs från hello klienten toohello Azure-datacenter gräns. hello lägsta möjliga tidsfördröjning uppnås genom att säkerställa hello anropar program finns i hello samma Azure-region som hello etableras Cosmos-DB-slutpunkten. En lista över tillgängliga regioner, se [Azure-regioner](https://azure.microsoft.com/regions/#services).

    ![Bild av hello Azure DB som Cosmos-princip](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
5. **Öka antalet trådar/uppgifter**

    Eftersom anrop tooAzure Cosmos DB görs över hello nätverk, kan du kanske måste toovary hello grad av parallellitet med dina begäranden så att klientprogrammet hello tillbringar mycket snabbt väntar på mellan begäranden. Till exempel om du använder. NET'S [parallella Uppgiftsbibliotek](https://msdn.microsoft.com//library/dd460717.aspx), skapa hello efter 100-tal uppgifter läsning eller skrivning tooCosmos DB.

## <a name="sdk-usage"></a>SDK-användning
1. **Installera hello senaste SDK**

    Hej Cosmos DB SDK: er som ständigt förbättrad tooprovide hello bästa prestanda. Se hello [Cosmos DB SDK](documentdb-sdk-dotnet.md) sidor toodetermine hello senaste SDK och granska förbättringar.
2. **Använd klienten en singleton-Cosmos-DB för hello livslängden för ditt program**

    Observera att varje DocumentClient-instans är trådsäker och utför effektiv hantering och cachelagring av adresser när direkt läge. tooallow effektiv hantering och bättre prestanda med DocumentClient, är det rekommenderade toouse en instans av DocumentClient per AppDomain för hello programmet hello livstid.

   <a id="max-connection"></a>
3. **Öka System.Net MaxConnections per värd när du använder läget för Gateway**

    Cosmos DB-begäranden görs över HTTPS/RESTEN när du använder Gateway-läge och är utsatts toohello standard anslutningsgränsen per värdnamn eller IP-adress. Du kanske måste tooset hello MaxConnections tooa högre värde (100-1000) så att hello klientbiblioteket kan använda flera samtidiga anslutningar tooCosmos DB. I hello .NET SDK 1.8.0 och ovan, hello standardvärdet för [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) är 50 och toochange hello värde kan du ange hello [Documents.Client.ConnectionPolicy.MaxConnectionLimit ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) tooa högre värde.   
4. **Justera parallella frågor för partitionerade samlingar**

     DocumentDB .NET SDK version 1.9.0 och högre support parallella frågor, vilket gör tooquery en partitionerad samling parallellt (se [arbeta med hello SDK](documentdb-partition-data.md#working-with-the-azure-cosmos-db-sdks) och hello relaterade [kodexempel](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) för Mer info). Parallella frågor är utformad tooimprove svarstid och genomströmning över sin seriella motsvarighet. Parallella frågor ange två parametrar att användare kan justera toocustom passning deras krav, (a) MaxDegreeOfParallelism: toocontrol hello högsta tillåtna antalet partitioner sedan kan efterfrågas parallellt och (b) MaxBufferedItemCount: toocontrol hello antal tidigare hämtade resultat.

    (a) ***justera MaxDegreeOfParallelism\: *** parallell frågan fungerar genom att fråga flera partitioner parallellt. Dock hämtas data från en enskild partitionerade samla in seriellt med avseende toohello fråga. Därför har inställningen hello MaxDegreeOfParallelism toohello antalet partitioner hello maximala risken för att uppnå Hej de flesta performant frågan, förutsatt att alla andra system villkoren förblir hello samma. Om du inte vet hello antalet partitioner, du kan ange hello MaxDegreeOfParallelism tooa högt värde och hello väljs automatiskt hello minsta (antal partitioner, tillhandahålls användarindata) som hello MaxDegreeOfParallelism.

    Det är viktigt toonote som parallella frågor producerar hello bästa fördelar om hello data är jämnt fördelad över alla partitioner med avseende toohello fråga. Om hello partitionerad samling är partitionerad så att hela eller en majoritet av hello data som returneras av en fråga samlas i några partitioner (en partition i värsta fall) och sedan dessa partitioner skulle flaskhals hello prestanda för hello frågan.

    (b) ***justera MaxBufferedItemCount\: *** parallella frågan är utformad toopre fetch resultat medan hello aktuell batch resultat bearbetas av hello-klienten. hello hämta hjälper dig i den övergripande svarstiden förbättring av en fråga. MaxBufferedItemCount är hello parametern toolimit hello antal tidigare hämtade resultat. Inställningen MaxBufferedItemCount förväntat toohello antal resultat som returneras (eller fler) tillåter hello frågan tooreceive största möjliga nytta från hämta.

    Observera att före hämtning fungerar hello samma sätt oavsett hello MaxDegreeOfParallelism och det finns en enda buffert för hello data från alla partitioner.  
5. **Aktivera servern GC**

    Minska hello frekvensen för skräpinsamling kan hjälpa i vissa fall. Ange i .NET, [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) tootrue.
6. **Implementera backoff med RetryAfter intervall**

    Under prestandatester, bör du öka belastningen tills en liten andel begäranden hämta begränsas. Om begränsas, bör hello klientprogrammet backoff på begränsning för hello server angetts återförsöksintervall. Respektera hello backoff garanterar att du ägnar minimal mängd väntetid mellan försöken. Stöd för återförsök Grupprincip ingår i Version 1.8.0 och senare av hello DocumentDB [.NET](documentdb-sdk-dotnet.md) och [Java](documentdb-sdk-java.md), version 1.9.0 och senare av hello [Node.js](documentdb-sdk-node.md) och [ Python](documentdb-sdk-python.md), och alla versioner av hello stöds [.NET Core](documentdb-sdk-dotnet-core.md) SDK: er. Mer information finns i [Exceeding reserverat dataflöde gränser](request-units.md#RequestRateTooLarge) och [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **Skala upp din klient arbetsbelastning**

    Om du testar på hög genomströmning nivåer (> 50 000 RU/s), hello klientprogrammet kan bli hello flaskhals på grund av toohello datorn tak som skall ut på processor eller användning. Om du når den här punkten kan fortsätta du toopush hello Cosmos DB konto ytterligare genom att skala ut ditt klientprogram på flera servrar.
8. **Cachelagra dokumentet URI: er för lägre skrivskyddade fördröjning**

    URI: er när det är möjligt för hello bästa läsprestanda för cache-dokument.
   <a id="tune-page-size"></a>
9. **Finjustera hello sidstorleken för frågor/läsa feeds för bättre prestanda**

    När utför en grupp av dokument med Läs feed funktioner (till exempel ReadDocumentFeedAsync) eller läsas vid utfärdande av en DocumentDB SQL-fråga, returneras hello resultat segmenterade överskrids om hello resultatmängden är för stor. Resultaten returneras i mängder 100 objekt eller 1 MB som standard, oavsett vilken gränsen är träffar första.

    tooreduce hello antal nätverk sändningar krävs tooretrieve alla tillämpliga resultat, kan du öka hello sidstorlek med hjälp av x-ms--objekt-maxantal begäran huvud tooup too1000. I fall där du behöver toodisplay endast några resultat, till exempel om ditt användar-gränssnittet eller programmet API returnerar bara 10 resulterar en tid, du kan också minska hello sidan storlek too10 tooreduce hello genomströmning används för läsning och frågor.

    Du kan också ange hello sidstorleken med hello tillgängliga Cosmos DB SDK.  Exempel:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Öka antalet trådar/uppgifter**

    Se [öka antalet trådar/uppgifter](#increase-threads) i hello nätverksavsnittet.

11. **64-bitars värden bearbetning**

    Hej DocumentDB SDK fungerar i en 32-bitars värdprocess när du använder DocumentDB .NET SDK version 1.11.4 och högre. Om du använder mellan partition frågor rekommenderas 64-bitars värden bearbetning för bättre prestanda. hello följande typer av program har 32-bitars värd bearbeta som hello standard, så Följ anvisningarna utifrån hello typ av ditt program i ordning toochange som too64-bitars:

    - Körbara program, detta kan göras genom att avmarkera hello **föredrar 32-bitars** alternativ i hello **projektegenskaperna** fönstret på hello **skapa** fliken.

    - För VSTest baserad testprojekt, kan du göra det genom att välja **testa**->**Testinställningar**->**standard processorarkitektur som X64**, från hello **Visual Studio Test** menyalternativet.

    - För lokalt distribuerade ASP.NET-webbprogram, detta kan göras genom att kontrollera hello **Använd hello 64-bitars version av IIS Express för webbplatser och projekt**under **verktyg** ->  **Alternativ**->**projekt och lösningar**->**Web projekt**.

    - För ASP.NET-webbprogram har distribuerats på Azure, detta kan göras genom att välja hello **plattform som 64-bitars** i hello **programinställningar** på hello Azure-portalen.

## <a name="indexing-policy"></a>Indexprincip
1. **Använd lazy indexering för snabbare belastning tid införandet priser**

    Cosmos DB kan toospecify – på samlingsnivå hello – en indexprincip, vilket gör att du toochoose om du vill att hello dokument i en samling toobe som indexeras automatiskt eller inte.  Dessutom kan du också välja mellan synkron (konsekvent) och asynkrona uppdateringar av index (Lazy). Hello indexet uppdateras synkront som standard på varje infoga, Ersätt eller borttagning av en dokumentsamling toohello. Synkront läge aktiverar hello frågor toohonor hello samma [konsekvensnivå](consistency-levels.md) som hello läser dokumentet utan fördröjning för hello index för ”ikapp”.

    Lazy indexering kan övervägas för scenarier där data skrivs i belastning och du vill att tooamortize hello arbetet som krävs för tooindex innehåll under en längre tidsperiod. Lazy indexering kan du också toouse din etablerats genomströmning effektivt och fungera skrivåtgärder vid Högbelastningstider med minimal svarstid. Det är viktigt toonote, men att frågeresultatet är överensstämmelse oavsett hello konsekvensnivå som konfigurerats för hello Cosmos DB konto när lazy indexering är aktiverad.

    Därför konsekvent indexering läge (IndexingPolicy.IndexingMode anges tooConsistent) ådrar sig hello högsta begäran enhet kostnad per skrivning vid Lazy indexering läge (IndexingPolicy.IndexingMode anges tooLazy) och ingen indexering (IndexingPolicy.Automatic anges tooFalse) har noll indexering kostnaden hello tiden för skrivning.
2. **Uteslut oanvända sökvägar från indexering för snabbare skrivningar**

    Cosmos DBS indexprincip kan du också toospecify som dokumentet sökvägar tooinclude eller undantas från att indexera genom att använda indexering sökvägar (IndexingPolicy.IncludedPaths och IndexingPolicy.ExcludedPaths). hello användning av indexering sökvägar kan erbjuda bättre skrivprestanda och lägre index lagring för scenarier där hello frågemönster är känt i förväg, som indexerings kostnader som är direkt korrelerade toohello antalet unika sökvägar som indexeras.  Till exempel visar hello följande kod hur tooexclude ett helt avsnitt med hello dokument (kallas även en underkatalog) från att indexera med hello ”*” jokertecken.

    ```C#
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Mer information finns i [Azure Cosmos DB indexering principer](indexing-policies.md).

## <a name="throughput"></a>Dataflöde
<a id="measure-rus"></a>

1. **Mäter och finjustera för lägre begäran enheter per sekund användning**

    Cosmos DB erbjuder en omfattande uppsättning databasåtgärder inklusive relationella och hierarkiska frågor med UDF: er, lagrade procedurer och utlösare – alla operativsystem i hello dokument inom en samling i databasen. hello kostnader som är kopplade till var och en av dessa åtgärder varierar beroende på hello CPU, IO och minne som krävs för toocomplete hello operation. I stället för att tänka på och hantera maskinvaruresurser, Tänk på begäran-enhet (RU) som en enda åtgärd för hello resurser krävs tooperform olika databasåtgärder och tjänsten ett program begärde.

    [Enheter för programbegäran](request-units.md) etableras för varje konto baserat på hello antalet kapacitetsenheter som du köper. Konsumtion av begäran enheten utvärderas som en sats per sekund. Program som överskrider hello etablerade begäran enhet klassificera för sitt konto är begränsad förrän hello frekvensen sjunker under hello reserverade nivå för hello-kontot. Om programmet kräver en högre säkerhetsnivå för genomflöde, kan du köpa ytterligare kapacitetsenheter.

    hello komplexiteten i en fråga påverkar hur många enheter som begär förbrukas för en åtgärd. hello antalet predikat, uppbyggnad hello predikat, antalet UDF: er och hello storleken på hello källa datauppsättning alla påverka hello kostnaden för fråga åtgärder.

    toomeasure hello arbetet med att alla åtgärder (skapa, uppdatera eller ta bort) inspektera hello x-ms-begäran-kostnad huvud (eller hello motsvarande RequestCharge egenskap i ResourceResponse<T> eller FeedResponse<T> i hello .NET SDK) toomeasure hello antal frågeenheter som används av dessa åtgärder.

    ```C#
    // Measure hello performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure hello performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    hello begäran returnerade i huvud är en del av din dataflöde (d.v.s. 2000 RUs / sekund). Om hello föregående fråga returnerar 1000 1KB-dokument är hello kostnaden för hello åtgärden 1000. Inom en sekund godkänner hello servern därför bara två sådana begäranden innan begränsning efterföljande förfrågningar. Mer information finns i [programbegäran](request-units.md) och hello [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Hantera hastighet begränsa/förfrågningar för stor**

    När en klient försöker tooexceed hello reserverat dataflöde för ett konto, är det utan prestandaförsämring på hello-servern och ingen användning av genomflödeskapaciteten utanför hello reserverade. hello server avslutas förebyggande syfte hello begäran med RequestRateTooLarge (HTTP-statuskod 429) och returnerar hello x-ms-retry-efter-ms-huvud som anger hello lång tid i millisekunder som hello användare måste vänta innan ett nytt försök hello-begäran.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    hello SDK alla implicit catch-svaret, respekterar hello server angetts försök igen efter rubrik och försök hello-begäran. Om ditt konto används samtidigt av flera klienter, lyckas hello nästa försök.

    Om du har mer än ett klientoperativsystem kumulativt konsekvent ovan hello förfrågningar hello standard antal försök för närvarande set too9 internt av hello-klienten inte finns tillräckligt; i det här fallet genererar hello klienten ett DocumentClientException med status kod 429 toohello program. hello standard återförsöksvärde kan ändras genom att inställningen hello RetryOptions på hello ConnectionPolicy instansen. Som standard returneras hello DocumentClientException med statuskoden 429 efter en kumulativ väntetid på 30 sekunder om hello begäran fortsätter toooperate ovan hello-förfrågningar. Detta inträffar även när hello aktuella återförsöksvärdet är mindre än Hej max återförsöksvärdet måste vara den hello standardvärdet 9 eller ett användardefinierat värde.

    När automatisk hello försök beteende hjälper tooimprove återhämtning och användbarhet för hello de flesta program, den kan tas emot varandra när du gör prestandatester, särskilt när mäta svarstid.  hello klient-observerade svarstid kommer ökar om hello experiment träffar hello server begränsning och orsakar hello klienten SDK toosilently försök igen. tooavoid fördröjning ger spikar i diagrammet under prestanda experiment mått hello kostnad som returneras av varje åtgärd och se till att begäranden fungerar nedan hello reserverade förfrågningar. Mer information finns i [programbegäran](request-units.md).
3. **Design för mindre dokument för högre genomströmning**

    hello begäran (d.v.s. begäranbearbetningen kostnad) för en viss åtgärd är direkt korrelerade toohello storlek på hello dokument. Åtgärder för stora dokument kostar mer än åtgärder för små dokument.

## <a name="next-steps"></a>Nästa steg
Ett exempel program som används för tooevaluate Cosmos DB för scenarier med hög prestanda på några klientdatorer, se [prestanda och skalning testning med Cosmos DB](performance-testing.md).

Toolearn mer information om hur du utformar ditt program för skalbarhet och hög prestanda, se även [partitionering och skalning i Azure Cosmos DB](partition-data.md).
