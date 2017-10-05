---
title: "Tips om nätverksprestanda - Azure Cosmos DB NoSQL | Microsoft Docs"
description: "Läs klienten konfigurationsalternativ för att förbättra Azure Cosmos DB databasprestanda"
keywords: "hur du förbättrar databasens prestanda"
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
ms.openlocfilehash: a94cda90eca9dca2b93adb8f5091d829e7375aa5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="performance-tips-for-azure-cosmos-db"></a>Prestandatips för Azure Cosmos DB
Azure Cosmos-DB är en snabb och flexibel distribuerad databas som kan skalas sömlöst med garanterad svarstid och genomströmning. Du behöver inte göra ändringar av större arkitektur eller skriva komplex kod för att skala databasen med Cosmos DB. Skala upp och ner är lika enkelt som att göra en enda API-anrop eller [SDK-anrop](set-throughput.md#set-throughput-sdk). Men eftersom Cosmos DB går att nå via nätverket anrop finns på klientsidan optimeringar du uppnår topprestanda.

Så om du begär ”hur kan jag förbättra Mina databasprestanda”? Överväg följande alternativ:

## <a name="networking"></a>Nätverk
<a id="direct-connection"></a>

1. **Anslutningsprincip för: Använd direktanslutning läge**

    Hur en klient ansluter till Cosmos DB har stor betydelse för prestanda, särskilt observerade klientens svarstid. Det finns två viktiga konfigurationsinställningar för att konfigurera klienten anslutningsprincip – anslutningen *läge* och [anslutning *protokollet*](#connection-protocol).  De två lägena finns:

   1. Gateway-läge (standard)
   2. Direkt-läge

      Gateway-läge stöds på alla SDK-plattformar och är konfigurerad som standard.  Om programmet körs i ett företagsnätverk med strikt brandväggsbegränsningar, är Gateway-läge det bästa valet eftersom den använder standardporten för HTTPS och en enda slutpunkt. Förhållandet prestanda är dock att Gateway-läge innebär ett hopp ytterligare nätverk varje gång data har lästs eller skrivits till Cosmos DB. Därmed ger direkt läge bättre prestanda på grund av färre nätverkshopp.
<a id="use-tcp"></a>
2. **Anslutningsprincip: använda TCP-protokollet**

    När du använder direkt läge, finns det två tillgängliga protokollalternativ:

   * TCP
   * HTTPS

     Cosmos DB erbjuder ett enkelt och öppna RESTful-programmeringsmiljö via HTTPS. Dessutom erbjuder en effektiv TCP-protokoll som är också RESTful i dess kommunikation modell och är tillgänglig via .NET klient-SDK. Använda SSL för första autentiseringen och kryptering trafik direkt TCP- och HTTPS. Använda TCP-protokollet när det är möjligt för bästa prestanda.

     När du använder TCP i Gateway-läge, TCP-Port 443 är Cosmos-DB-port och 10255 är MongoDB API-port. När du använder TCP i direkt läge, förutom portarna som Gateway, måste du kontrollera porten intervallet mellan 10000 och 20000 är öppen eftersom Cosmos DB använder dynamiska TCP-portar. Om dessa portar inte är öppna och försök att använda TCP, felmeddelandet 503 tjänsten är inte tillgänglig.

     Anslutningsläget konfigureras under konstruktion av DocumentClient-instans med parametern ConnectionPolicy. Om direkt läge används, kan du också ange protokollet i parametern ConnectionPolicy.

    ```C#
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from the Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    Eftersom TCP stöds endast i direkt läge om Gateway-läget används, HTTPS-protokollet används alltid för att kommunicera med gatewayen och protokollet värdet i ConnectionPolicy ignoreras.

    ![Bild av principen för Azure Cosmos DB](./media/performance-tips/connection-policy.png)

3. **Anropa OpenAsync för att undvika Start svarstid på förfrågan**

    Den första begäranden har en högre latens eftersom den har att hämta adressen routningstabellen som standard. Om du vill undvika det här Start fördröjning den första begäran bör du anropa OpenAsync() en gång under initieringen på följande sätt.

        await client.OpenAsync();
   <a id="same-region"></a>
4. **Samordna klienter i samma Azure-region för prestanda**

    När det är möjligt placera alla program som anropar Cosmos-DB i samma region som Cosmos-DB-databas. Anrop till Cosmos-DB i samma region som utför inom 1 – 2 ms för en ungefärlig jämförelse, men fördröjning mellan Väst och av USA: S > 50 ms. Den här fördröjningen kan förmodligen variera från begäran till begäran beroende på vägen som begäran när det överförs från klienten till den Azure datacenter-gränsen. Lägsta möjliga tidsfördröjning uppnås genom att säkerställa att det anropande programmet finns i samma Azure-region som etablerade Cosmos-DB-slutpunkt. En lista över tillgängliga regioner, se [Azure-regioner](https://azure.microsoft.com/regions/#services).

    ![Bild av principen för Azure Cosmos DB](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
5. **Öka antalet trådar/uppgifter**

    Eftersom anrop till Azure Cosmos DB görs över nätverket, kan du behöva variera grad av parallellitet med dina begäranden så att klientprogrammet använder mycket lite tid att vänta mellan begäranden. Till exempel om du använder. NET'S [parallella Uppgiftsbibliotek](https://msdn.microsoft.com//library/dd460717.aspx), skapa i 100-tal uppgifter läsning eller skrivning till Cosmos DB.

## <a name="sdk-usage"></a>SDK-användning
1. **Installera den senaste SDK**

    DB-SDK Cosmos är ständigt bättre för att tillhandahålla bästa prestanda. Finns det [Cosmos DB SDK](documentdb-sdk-dotnet.md) sidor för att avgöra den senaste SDK och granska förbättringar.
2. **Använd klienten en singleton-Cosmos-DB för livslängden för ditt program**

    Observera att varje DocumentClient-instans är trådsäker och utför effektiv hantering och cachelagring av adresser när direkt läge. Om du vill tillåta effektiv hantering och bättre prestanda med DocumentClient, rekommenderas att använda en enda instans av DocumentClient per AppDomain för livslängden för programmet.

   <a id="max-connection"></a>
3. **Öka System.Net MaxConnections per värd när du använder läget för Gateway**

    Cosmos DB begäranden som görs via HTTPS/RESTEN när du använder Gateway-läge och genomgår Standardgränsen för anslutning per värdnamn eller IP-adress. Du kan behöva ange MaxConnections till ett högre värde (100-1000) så att klientbiblioteket kan använda flera samtidiga anslutningar till Cosmos DB. I .NET SDK 1.8.0 och senare standardvärdet för [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) är 50 och du kan ange om du vill ändra värdet på [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) till ett högre värde.   
4. **Justera parallella frågor för partitionerade samlingar**

     DocumentDB .NET SDK version 1.9.0 och högre support parallella frågor, vilket gör det möjligt att fråga en partitionerad samling parallellt (se [arbeta med SDK: erna](documentdb-partition-data.md#working-with-the-azure-cosmos-db-sdks) och den relaterade [kodexempel](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) för mer information). Parallella frågor är utformade för att förbättra svarstid och genomströmning över sin seriella motsvarighet. Parallella frågor ange två parametrar som användare kan justera för anpassade efter deras krav, (a) MaxDegreeOfParallelism: att kontrollera det maximala antalet partitioner sedan kan efterfrågas parallellt och (b) MaxBufferedItemCount: att styra hur många tidigare hämtade resultat.

    (a) ***justera MaxDegreeOfParallelism\: *** parallell frågan fungerar genom att fråga flera partitioner parallellt. Dock hämtas data från en enskild partitionerade samla in seriellt med avseende på frågan. Så har om MaxDegreeOfParallelism till antalet partitioner högsta risken för att uppnå de flesta performant frågan, förutsatt att alla andra system villkoren förblir oförändrade. Om du inte vet antalet partitioner, du kan ange MaxDegreeOfParallelism till ett stort antal och minsta (antal partitioner, tillhandahålls användarindata) som MaxDegreeOfParallelism väljs automatiskt.

    Det är viktigt att notera att parallella frågor ger bästa fördelarna om data är jämnt fördelad över alla partitioner med avseende på frågan. Om partitionerade samlingen är partitionerad så att hela eller en majoritet av data som returneras av en fråga samlas i några partitioner (en partition i värsta fall) och sedan dessa partitioner skulle flaskhals prestanda för frågan.

    (b) ***justera MaxBufferedItemCount\: *** Parallel query är utformat för att före hämtar resultat när den aktuella batchen med resultat som behandlas av klienten. Det hjälper till att hämtas först i den övergripande svarstiden förbättring av en fråga. MaxBufferedItemCount är parametern för att begränsa antalet resultat som tidigare hämtade. MaxBufferedItemCount till det förväntade antalet resultat som returneras (eller fler) kan frågan för att hämta det maximala fördelar.

    Observera att hämta fungerar på samma sätt oavsett MaxDegreeOfParallelism och det finns en enda buffert för data från alla partitioner.  
5. **Aktivera servern GC**

    Minska frekvensen för skräpinsamling kan hjälpa i vissa fall. Ange i .NET, [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) till true.
6. **Implementera backoff med RetryAfter intervall**

    Under prestandatester, bör du öka belastningen tills en liten andel begäranden hämta begränsas. Om begränsas, bör klientprogrammet backoff på begränsning för intervallet-server har angetts. Respektera backoff garanterar att du ägnar minimal mängd väntetid mellan försöken. Försök stöd för Grupprincip ingår i Version 1.8.0 och senare i documentdb [.NET](documentdb-sdk-dotnet.md) och [Java](documentdb-sdk-java.md), version 1.9.0 och högre av de [Node.js](documentdb-sdk-node.md) och [Python](documentdb-sdk-python.md), och alla versioner av stöds i [.NET Core](documentdb-sdk-dotnet-core.md) SDK: er. Mer information finns i [Exceeding reserverat dataflöde gränser](request-units.md#RequestRateTooLarge) och [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **Skala upp din klient arbetsbelastning**

    Om du testar på hög genomströmning nivåer (> 50 000 RU/s), klientprogrammet kan bli en flaskhals på grund av den datorn tak som skall ut på processor eller användning. Om du når den här punkten kan fortsätta du att push-Cosmos-DB kontot ytterligare genom att skala ut ditt klientprogram på flera servrar.
8. **Cachelagra dokumentet URI: er för lägre skrivskyddade fördröjning**

    Dokumentet URI: er när det är möjligt för bästa prestanda för Läs-cache.
   <a id="tune-page-size"></a>
9. **Finjustera sidstorleken för frågor/läsa feeds för bättre prestanda**

    När utför en grupp av dokument med Läs feed funktioner (till exempel ReadDocumentFeedAsync) eller läsas vid utfärdande av en DocumentDB SQL-fråga, returneras resultatet i ett segmenterade sätt om resultatet är för stor. Resultaten returneras i mängder 100 objekt eller 1 MB som standard, oavsett vilken gränsen är träffar första.

    För att minska antalet nätverket förfrågningar krävs för att hämta alla tillämpliga resultat, kan du öka sidstorleken med x-ms-max--antal objekt begärandehuvudet till upp till 1000. I fall där du vill visa endast några resultat, till exempel om ditt användar-gränssnittet eller programmet API returnerar bara 10 resulterar en tid, du kan också minska sidstorleken till 10 för att minska den används för läsning och frågor.

    Du kan också ange sidstorlek med tillgängliga Cosmos DB SDK: erna.  Exempel:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Öka antalet trådar/uppgifter**

    Se [öka antalet trådar/uppgifter](#increase-threads) i avsnittet nätverk.

11. **64-bitars värden bearbetning**

    DocumentDB SDK fungerar i en 32-bitars värdprocess när du använder DocumentDB .NET SDK version 1.11.4 och högre. Om du använder mellan partition frågor rekommenderas 64-bitars värden bearbetning för bättre prestanda. Följande typer av program har 32-bitars värdprocess som standard, så för att kunna ändra som till 64-bitars, Följ dessa steg beroende på vilken typ av ditt program:

    - Körbara program, detta kan göras genom att avmarkera den **föredrar 32-bitars** alternativet i den **projektegenskaperna** fönstret på den **skapa** fliken.

    - För VSTest baserad testprojekt, kan du göra det genom att välja **testa**->**Testinställningar**->**standard processorarkitektur som X64**, från den **Visual Studio Test** menyalternativet.

    - För lokalt distribuerade ASP.NET-webbprogram, detta kan göras genom att kontrollera den **använder 64-bitarsversionen av IIS Express för webbplatser och projekt**under **verktyg**->**alternativ**->**projekt och lösningar**->**webbprojekt**.

    - För ASP.NET-webbprogram har distribuerats på Azure, detta kan göras genom att välja den **plattform som 64-bitars** i den **programinställningar** på Azure-portalen.

## <a name="indexing-policy"></a>Indexprincip
1. **Använd lazy indexering för snabbare belastning tid införandet priser**

    Cosmos DB kan du ange – på samlingsnivå – en indexprincip där du kan välja om du vill att dokument i en samling som indexeras automatiskt eller inte.  Dessutom kan du också välja mellan synkron (konsekvent) och asynkrona uppdateringar av index (Lazy). Indexet uppdateras synkront som standard på varje insert-, ersätta eller ta bort ett befintligt dokument i samlingen. Synkront läget kan frågor ta hänsyn till samma [konsekvensnivå](consistency-levels.md) som dokumentet läser utan fördröjning för indexet ”identifiera”.

    Lazy indexering kan övervägas för scenarier där data skrivs i belastning och du vill att amortera arbetet som krävs för att indexera innehåll under en längre tidsperiod. Lazy indexering kan du också använda din dataflöde effektivt och tjäna skrivbegäranden vid Högbelastningstider med minimal svarstid. Det är viktigt att Observera att frågeresultatet är överensstämmelse oavsett konsekvensnivå som konfigurerats för Cosmos-DB-konto när lazy indexering är aktiverad.

    Därför konsekvent indexering läge (IndexingPolicy.IndexingMode är inställt på konsekvent) ådrar sig högsta begäran enhet kostnad per skrivning vid Lazy indexering läge (IndexingPolicy.IndexingMode är inställt på Lazy) och ingen indexering (IndexingPolicy.Automatic är inställd på False) har noll indexering kostnad vid tidpunkten för skrivning.
2. **Uteslut oanvända sökvägar från indexering för snabbare skrivningar**

    Cosmos DBS indexprincip kan du ange vilka sökvägar dokument om du vill inkludera eller exkludera från indexering genom att använda indexering sökvägar (IndexingPolicy.IncludedPaths och IndexingPolicy.ExcludedPaths). Användning av indexering sökvägar kan erbjuda bättre skrivprestanda och lägre index lagring för scenarier där frågemönster är kända i förväg, som indexerings kostnader direkt kopplas ihop med antalet unika sökvägar som indexeras.  Följande kod visar exempelvis undanta (kallas även ett helt avsnitt av dokumenten en underkatalog) från att använda indexering av ”*” jokertecken.

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

    Cosmos DB erbjuder en omfattande uppsättning databasåtgärder inklusive relationella och hierarkiska frågor med UDF: er, lagrade procedurer och utlösare – alla operativsystem i dokumenten inom en samling i databasen. Kostnaden för att var och en av dessa åtgärder varierar beroende på CPU, IO och minne som krävs för att slutföra åtgärden. I stället för att tänka på och hantera maskinvaruresurser, Tänk på begäran-enhet (RU) som en enda åtgärd för de resurser som krävs för att utföra olika databasåtgärder och ett program begärde-tjänsten.

    [Enheter för programbegäran](request-units.md) etableras för varje konto baserat på antalet kapacitetsenheter som du köper. Konsumtion av begäran enheten utvärderas som en sats per sekund. Program som överskrider den etablerade begärandehastighet enhet för sitt konto begränsas förrän frekvensen sjunker under nivån reserverade för kontot. Om programmet kräver en högre säkerhetsnivå för genomflöde, kan du köpa ytterligare kapacitetsenheter.

    Komplexiteten i en fråga påverkar hur många enheter som begär förbrukas för en åtgärd. Antalet predikat, predikat, antalet UDF: er och storleken på alla källa datauppsättningen påverkar kostnaden för frågor.

    För mätning av alla åtgärder (skapa, uppdatera eller ta bort) inspektera huvudet x-ms-begäran-tillägget (eller motsvarande RequestCharge-egenskapen i ResourceResponse<T> eller FeedResponse<T> i .NET SDK) att mäta antalet begäran enheter som används av dessa åtgärder.

    ```C#
    // Measure the performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure the performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    Begäran-tillägget som returneras i det här sidhuvudet är en del av din dataflöde (d.v.s. 2000 RUs / sekund). Om den föregående frågan returnerar 1000 1KB-dokument, är kostnaden för åtgärden 1000. Inom en sekund godkänner servern därför bara två sådana begäranden innan begränsning efterföljande förfrågningar. Mer information finns i [programbegäran](request-units.md) och [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Hantera hastighet begränsa/förfrågningar för stor**

    När en klient försöker överskrida reserverat dataflöde för ett konto, finns det inga prestandaförsämring på servern och ingen användning av genomflödeskapaciteten utanför den reserverade. Servern kommer avsluta begäran och RequestRateTooLarge (HTTP-statuskod 429) och returnera x-ms-retry-efter-ms-huvudet som anger hur lång tid i millisekunder som användaren måste vänta innan ett nytt försök för begäran.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK: erna alla implicit catch-svaret respektera server angetts försök igen efter rubriken och försöka. Om ditt konto används samtidigt av flera klienter, lyckas nästa försök.

    Om du har mer än ett klientoperativsystem kumulativt konsekvent över den begärt frekvensen standard antal försök som för närvarande inställd på 9 internt av klienten inte finns tillräckligt; i det här fallet genererar klienten ett DocumentClientException med statuskoden 429 till programmet. Standardvärdet för antal återförsök kan ändras genom att ange RetryOptions på ConnectionPolicy-instans. Som standard returneras DocumentClientException med statuskoden 429 efter en kumulativ väntetid på 30 sekunder om begäran fortsätter att fungera över frekvensen med begäran. Detta inträffar även när det aktuella antalet nya försök är mindre än det högsta antal försök antalet måste vara den standardvärdet 9 eller ett användardefinierat värde.

    När automatiska försök beteendet hjälper oss för att förbättra återhämtning och användbarhet för de flesta program, kan det ha följt emot varandra när du gör prestandatester, särskilt när mätning av svarstiden.  Svarstid för klient-observerade kommer ökar om experimentet träffar server-begränsning och orsakar klient-SDK ska försöka utföra tyst. För att undvika svarstidsspikar under prestanda experiment mäta kostnad som returneras av varje åtgärd och se till att begäranden fungerar nedan reserverade förfrågningar. Mer information finns i [programbegäran](request-units.md).
3. **Design för mindre dokument för högre genomströmning**

    Begäran-tillägget (d.v.s. begäranbearbetningen kostnad) för en viss åtgärd korreleras direkt till samma storlek som dokumentet. Åtgärder för stora dokument kostar mer än åtgärder för små dokument.

## <a name="next-steps"></a>Nästa steg
Ett exempelprogram som används för att utvärdera Cosmos DB för scenarier med hög prestanda på några klientdatorer, se [prestanda och skalning testning med Cosmos DB](performance-testing.md).

Se även om du vill veta mer om hur du utformar ditt program för skalbarhet och hög prestanda [partitionering och skalning i Azure Cosmos DB](partition-data.md).
