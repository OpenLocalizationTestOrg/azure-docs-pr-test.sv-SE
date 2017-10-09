---
title: 'Azure Cosmos DB: Utveckla med hello tabell API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB tabell-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a>Azure Cosmos DB: Utveckla med hello tabell API i .NET

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.

Den här kursen ingår hello följande uppgifter: 

> [!div class="checklist"] 
> * Skapa ett Azure Cosmos DB-konto 
> * Aktivera funktionen i hello app.config-fil 
> * Skapa en tabell med hello [tabell API](table-introduction.md) (förhandsgranskning)
> * Lägg till en entitet tooa tabell 
> * Infoga en batch med entiteter 
> * Hämta en enda entitet 
> * Fråga entiteter med hjälp av automatisk sekundärindex 
> * Ersätta en entitet 
> * Ta bort en entitet 
> * Ta bort en tabell
 
## <a name="tables-in-azure-cosmos-db"></a>Tabeller i Azure Cosmos DB 

Azure Cosmos-DB tillhandahåller hello [tabell API](table-introduction.md) (förhandsgranskning) för program som behöver ett nyckel / värde-Arkiv med en mindre schema-design. [Azure Table storage](../storage/common/storage-introduction.md) SDK: er och REST API: er kan använda toowork med Azure Cosmos DB. Du kan använda Azure Cosmos DB toocreate tabeller med krav på hög genomströmning. Azure Cosmos DB stöder dataflödesoptimerade tabeller (kallas informellt "premiumtabeller"), för närvarande i allmänt tillgängliga förhandsversioner. 

Du kan fortsätta toouse Azure Table storage för tabeller med hög lagring och lägre genomströmning krav. Azure Cosmos-DB vi lägger till stöd för storage optimerat tabeller i en kommande uppdatering och befintliga och nya Azure Table storage-konton kommer att sömlöst uppgraderas tooAzure Cosmos DB.

Om du använder Azure Table storage, får du följande fördelar med hello ”premium table” förhandsgranskning hello:

- Nyckelfärdig [global distributionsplatsen](distribute-data-globally.md) med multihoming och [automatisk och manuell växling vid fel](regional-failover.md)
- Stöd för automatisk schema-oberoende indexering mot alla egenskaper (”sekundärindex”) och snabb frågor 
- Stöd för [oberoende skalning av lagring och genomströmning](partition-data.md), till alla områden
- Stöd för [dedikerad genomströmning per tabell](request-units.md) som kan skalas från hundratals toomillions begäranden per sekund
- Stöd för [fem justerbara konsekvensnivåer](consistency-levels.md) tootrade av tillgänglighet, svarstid och konsekvenskontroll utifrån behoven för ditt program
- 99,99% tillgänglighet inom en enskild region och möjlighet tooadd flera regioner för högre tillgänglighet och [branschledande omfattande SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) på allmän tillgänglighet
- Arbeta med hello befintliga Azure storage .NET SDK och inget kod ändringar tooyour program

Hello förhandsversionen hello Azure Cosmos DB stöder tabell API: et med hello .NET SDK. Du kan hämta hello [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) från NuGet, som har hello samma klasser och metoden signaturer som hello [Azure Storage SDK: N](https://www.nuget.org/packages/WindowsAzure.Storage), men kan också ansluta tooAzure Cosmos DB konton med hjälp av hello Tabell-API.

toolearn mer om komplexa lagringsuppgifter för Azure Table, se:

* [Introduktion tooAzure Cosmos DB: tabell-API](table-introduction.md)
* Hej tabell referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er [Storage-klientbibliotek för .NET-referens](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

### <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen används för utvecklare som är bekant med hello Azure Table storage SDK och vill toouse hello premium-funktioner tillgängliga Azure Cosmos DB. Den är baserad på [komma igång med Azure Table storage med hjälp av .NET](table-storage-how-to-use-dotnet.md) och visar hur tootake nytta av ytterligare funktioner som sekundärindex, dataflöde och multihoming. Vi upp hur toouse hello Azure portal toocreate ett Azure DB som Cosmos-konto och sedan skapa och distribuera ett program för tabellen. Vi också igenom .NET-exempel för att skapa och tar bort en tabell och infoga, uppdatera, ta bort och frågar tabelldata. 

Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.  

> [!TIP]
> * Redan har ett Azure DB som Cosmos-konto? I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS).
> * Hade du ett Azure DocumentDB-konto? Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).  
> * Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS).
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona en tabell app från github, ange hello anslutningssträngen och kör den.

1. Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Öppna sedan hello lösningsfilen i Visual Studio.

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. I hello app.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello anslutningssträngen.

2. Öppna hello app.config-filen i Visual Studio. 

3. Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello konto-nyckeln i app.config. Använda hello-kontonamn som skapats tidigare för kontonamnet i app.config.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> toouse den här appen med standard Azure Table Storage, behöver du toochange hello anslutningssträngen i `app.config file`. Använda hello kontonamn som tabellen kontonamnet och nyckeln som primärnyckel för Azure-lagring. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a>Skapa och distribuera hello app
1. Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**. 

2. I hello NuGet **Bläddra** skriver ***WindowsAzure.Storage PremiumTable***. Kontrollera **är förhandsversioner**.

3. Hello resultat för att installera hello **WindowsAzure.Storage PremiumTable** och välj hello förhandsversionen `0.0.1-preview`. Den här åtgärden installerar hello Azure Table storage paketet och alla beroenden.

4. Klicka på CTRL + F5 toorun hello program. 

Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med tabelldata för den här. 

> [!NOTE]
> toouse appen med en Azure Cosmos DB-emulatorn du bara behöver toochange hello anslutningssträngen i `app.config file`. Använd hello mindre än värdet för emulatorn. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Azure DB Cosmos-funktioner
Azure Cosmos-DB stöder ett antal funktioner som inte är tillgängliga i hello Azure Table storage API. hello nya funktioner kan aktiveras via hello följande `appSettings` konfigurationsvärden. Vi inte införa några nya signaturer eller överlagringar toohello Förhandsgranska Azure Storage SDK: N. Detta ger dig tooconnect tooboth standard och premium-tabeller och fungerar med andra Azure Storage-tjänster som Blobbar och köer. 


| Nyckel | Beskrivning |
| --- | --- |
| TableConnectionMode  | Azure Cosmos-DB stöder två lägen för anslutningen. I `Gateway` läge begäran görs alltid toohello Azure DB som Cosmos-gateway som vidarebefordrar det toohello partitioner för motsvarande data. I `Direct` anslutningsläget, hello klienten hämtar hello mappningen av tabeller toopartitions och begäranden som görs direkt mot datapartitioner. Vi rekommenderar `Direct`, hello standard.  |
| TableConnectionProtocol | Azure Cosmos-DB stöder två anslutningsprotokoll - `Https` och `Tcp`. `Tcp`hello standard och rekommenderas eftersom det är mer lightweight. |
| TablePreferredLocations | Kommaavgränsad lista över önskade (flera homing) platser för läsning. Varje Azure DB som Cosmos-kontot kan vara associerat med 1 – 30 + regioner. Varje klientinstans kan ange en delmängd av dessa regioner hello rekommenderas för låg latens läsningar. hello regioner måste namnges med deras [visningsnamn](https://msdn.microsoft.com/library/azure/gg441293.aspx), till exempel `West US`. Se även [multihoming API: er](tutorial-global-distribution-table.md).
| TableConsistencyLevel | Du kan handel av mellan svarstid, konsekvens och tillgänglighet genom att välja mellan fem väldefinierade konsekvensnivåer: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, och `Eventual`. Standardvärdet är `Session`. hello valet av konsekvensnivå gör betydande prestanda skillnader i flera regioner inställningar. Se [konsekvensnivåer](consistency-levels.md) mer information. |
| TableThroughput | Reserverat dataflöde för hello tabellen uttryckt i frågeenheter (RU) per sekund. Enskild tabeller har stöd för 100-tal miljontals RU/s. Se [programbegäran](request-units.md). Standardvärdet är`400` |
| TableIndexingPolicy | Konsekvent och automatisk sekundära indexering av alla kolumner i tabeller | JSON-sträng förväntad toohello indexering principspecifikationen. Se [indexering princip](indexing-policies.md) toosee hur du kan ändra indexering princip tooinclude/exkludera specifika kolumner. | Automatisk indexering av alla egenskaper (hash för strängar) och intervallet för tal |
| TableQueryMaxItemCount | Konfigurera hello maximalt antal objekt som returneras per fråga i en enda onödig kommunikation. Standardvärdet är `-1`, vilket gör att Azure Cosmos DB dynamiskt bestämma hello värdet vid körning. |
| TableQueryEnableScan | Om hello frågan inte kan använda hello index för filter, kör det ändå via en genomsökning. Standardvärdet är `false`.|
| TableQueryMaxDegreeOfParallelism | hello grad av parallellitet för körning av en fråga för cross-partition. `0`är seriell med ingen före hämtning, `1` är seriell med före hämtning och högre värden öka hello frekvensen parallellitet. Standardvärdet är `-1`, vilket gör att Azure Cosmos DB dynamiskt bestämma hello värdet vid körning. |

Standardvärdet för toochange hello, öppna hello `app.config` filen i Solutions Explorer i Visual Studio. Lägg till hello innehållet i hello `<appSettings>` elementet enligt nedan. Ersätt `account-name` med hello namnet på ditt lagringskonto och `account-key` med din åtkomstnyckel. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello `Program.cs` fil och du hittar att dessa rader med kod skapar hello tabellen resurser. 

## <a name="create-hello-table-client"></a>Skapa hello tabell klient
Du har initierat en `CloudTableClient` tooconnect toohello tabell konto.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Den här klienten har initierats med hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, och `TablePreferredLocations` konfiguration värden om anges i inställningarna för hello-app.
    
## <a name="create-a-table"></a>Skapa en tabell
Sedan kan du skapa en tabell med hjälp av `CloudTable`. Tabeller i Azure Cosmos DB kan skalas oberoende vad gäller lagring och genomflöde och partitionering hanteras automatiskt av hello-tjänsten. Azure Cosmos-DB stöder både med fast storlek och obegränsat antal tabeller. Se [partitionering i Azure Cosmos DB](partition-data.md) mer information. 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Det finns en viktig skillnad i hur du skapar tabeller. Azure Cosmos-DB reserverar dataflöde, till skillnad från Azure storage förbrukningsbaserad modell för transaktioner. hello reservation modellen har två viktiga fördelar:

* Din genomströmning reserveras dedikerad /, så du får aldrig begränsas om din frågehastigheten är vid eller under din dataflöde
* hello reservation modellen är mer [kostnadseffektivt för genomströmning tunga arbetsbelastningar](key-value-store-cost.md)

Du kan konfigurera hello standard genomströmning genom att konfigurera hello inställning för `TableThroughput` vad gäller RU (frågeenheter) per sekund. 

En läsning av en 1 KB entitet är normaliserat som 1 RU och andra åtgärder är normaliserat tooa fast RU värde baserat på deras förbrukning av CPU, minne och IOPS. Lär dig mer om [programbegäran i Azure Cosmos DB](request-units.md).

> [!NOTE]
> Medan tabellagring SDK inte stöder för närvarande ändra dataflöde, kan du ändra hello genomströmning omedelbart när som helst med hello Azure-portalen eller Azure CLI.

Sedan vi gå igenom hello enkla läsa och skriva CRUD-åtgärder med hjälp av hello Azure Table storage SDK. Den här kursen visar förutsägbar låg en siffra millisekunders latens och snabb frågor som tillhandahålls av Azure Cosmos DB.

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
Entiteter i Azure Table storage utöka från hello `TableEntity` klassen och måste ha `PartitionKey` och `RowKey` egenskaper. Här är ett exempel definitionen för en kundentitet.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

hello följande utdrag visar hur tooinsert en entitet med hello Azure storage SDK. Azure Cosmos-DB är utformad för garanteras låg latens i alla skalor över hälsningsmeddelande.

Slutför skrivningar < 15 ms på p99 och ~ 6 ms på p 50 för program som körs hello samma region som hello Azure DB som Cosmos-konto. Och varaktigheten konton för hello fakta som skriver är bekräftade tillbaka toohello klienten när de synkront replikeras, varaktigt allokerat och allt innehåll som är indexerad.

hello tabell API: et för Azure Cosmos DB finns i förhandsgranskningen. Vid allmän tillgänglighet backas hello p99 svarstid garantier upp av SLA: er som andra Azure Cosmos DB-API: er. 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Infoga en batch med entiteter
Azure Table storage stöder en batch åtgärden API, där du kan kombinera borttagningar, uppdateringar och infogningar i hello samma batchåtgärd. Azure Cosmos-DB har inte några hello begränsningar på hello batch API som Azure Table storage. Exempelvis kan du utföra flera läsningar inom en grupp, kan du utföra flera skrivningar toohello samma entitet i en batch och det finns ingen gräns på 100-åtgärder per batch. 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Hämta en enda entitet
Hämtar (hämtar) i Azure Cosmos DB fullständig < 10 ms på p99 och ~ 1 ms på p 50 i hello samma Azure-region. Du kan lägga till så många regioner tooyour för låg latens läsningar och distribuera program tooread från sin lokala region (”multi-homed”) genom att ange `TablePreferredLocations`. 

Du kan hämta en enda entitet med hjälp av följande fragment hello:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Lär dig mer om flera API: er på [utveckling med flera regioner](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Fråga entiteter med hjälp av automatisk sekundärindex
Tabeller kan efterfrågas med hello `TableQuery` klass. Azure Cosmos-DB har en Skriv-optimerad databasmotor som indexerar automatiskt alla kolumner i tabellen. Indexering i Azure Cosmos DB är storleksoberoende tooschema. Även om schemat skiljer sig mellan rader, eller om hello schemat utvecklas över tid, indexeras det därför automatiskt. Eftersom Azure Cosmos DB har stöd för automatisk sekundärindex, frågor mot en egenskap kan använda hello index och hanteras effektivt.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

Under förhandsgranskning, Azure Cosmos DB stöder hello samma fråga funktioner som Azure Table storage för hello tabell API. Azure Cosmos-DB också stöd för sortering, aggregeringar, geospatiala frågan, hierarki och en mängd olika inbyggda funktioner. hello ytterligare funktioner ska anges i hello tabell API i en framtida tjänstuppdatering. Se [Azure Cosmos DB-fråga](documentdb-sql-query.md) en översikt över dessa funktioner. 

## <a name="replace-an-entity"></a>Ersätta en entitet
Hämta den från tabelltjänsten hello tooupdate en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar. hello ändrar följande kod en befintlig kunds telefonnummer. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
På liknande sätt kan du utföra `InsertOrMerge` eller `Merge` åtgärder.  

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan enkelt ta bort en enhet när du har hämtat den genom att använda hello samma mönster för att uppdatera en entitet. hello följande kod hämtar och tar bort en kundentitet.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Ta bort en tabell
Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto. Du kan ta bort och återskapa en tabell omedelbart med Azure Cosmos DB.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Rensa resurser 

Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen.   

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.  
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**. 

## <a name="next-steps"></a>Nästa steg

Vi beskrivs hur tooget igång med Azure Cosmos DB med hello tabell API i den här självstudiekursen och du har gjort hello följande: 

> [!div class="checklist"] 
> * Skapa ett Azure DB som Cosmos-konto 
> * Funktioner som aktiveras i hello app.config-fil 
> * Skapa en tabell 
> * Lägga till en entitet tooa tabell 
> * Infoga en batch med entiteter 
> * Hämta en enda entitet 
> * Efterfrågade enheter med hjälp av automatisk sekundärindex 
> * Ersätta en entitet 
> * Ta bort en entitet 
> * Ta bort en tabell  

Du kan nu fortsätta toohello nästa kurs och lär dig mer om frågar tabelldata. 

> [!div class="nextstepaction"]
> [Frågan med hello tabell-API](tutorial-query-table.md)
