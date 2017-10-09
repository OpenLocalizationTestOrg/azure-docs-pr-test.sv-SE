---
title: "aaaGet igång med Azure Table storage med hjälp av .NET | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: mimig
ms.openlocfilehash: a3e9a4c6f6fd5e724535b86a3f99cd4c161de6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a>Komma igång med Azure Table Storage med hjälp av .NET
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table storage är en tjänst som lagrar strukturerade NoSQL i hello moln, vilket ger ett nyckelattribut datalager med en schemalös design. Eftersom Table storage är schemalös är det enkelt tooadapt dina data efter hello behov utvecklas. Åtkomst till tooTable storage data är snabb och kostnadseffektiv för många typer av program och är vanligtvis lägre kostnad än traditionella SQL för motsvarande volymer med data.

Du kan använda Table storage toostore flexibla datauppsättningar som användardata för webbprogram, adressböcker, enhetsinformation och andra typer av metadata som din tjänst kräver. Du kan lagra valfritt antal enheter i en tabell och ett lagringskonto kan innehålla valfritt antal tabeller, upp toohello kapacitetsgränsen för hello storage-konto.

### <a name="about-this-tutorial"></a>Om den här självstudiekursen
De här självstudierna visar hur toouse hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) i några vanliga scenarier för Azure Table storage. Dessa scenarier illustreras med C#-exempel och visar hur du skapar och tar bort en tabell och hur du infogar, uppdaterar, tar bort och hämtar data från tabeller.

## <a name="prerequisites"></a>Krav

Du behöver hello följande toocomplete den här självstudiekursen har:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Fler exempel
Ytterligare exempel med Table Storage finns i [Komma igång med Azure Table Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Du kan ladda ned exempelprogrammet hello och kör den, eller bläddra hello koden på GitHub.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Lägga till med hjälp av direktiv
Lägg till följande hello **med** direktiven toohello överkant hello `Program.cs` fil:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a>Parsa anslutningssträngen för hello
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a>Skapa hello tabelltjänstens klient
Hej [CloudTableClient] [ dotnet_CloudTableClient] klassen kan du tooretrieve tabeller och de entiteter som lagras i Table storage. Här är ett sätt toocreate hello tabelltjänstens klient:

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Nu är du redo toowrite kod som läser data från och skriver tooTable datalagring.

## <a name="create-a-table"></a>Skapa en tabell
Det här exemplet illustrerar hur toocreate en tabell om den inte redan finns:

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference toohello table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
Entiteter mappar tooC #-objekt med hjälp av en anpassad klass som härleds från [TableEntity][dotnet_TableEntity]. tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten. hello definierar följande kod en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello. Tillsammans identifieras en entitets partition och radnyckel kan i hello tabell. Entiteter med samma partitionsnyckel kan frågas snabbare än entiteter med olika hello partitions nycklar, men med olika partitionsnycklar möjliggör bättre skalbarhet av parallella åtgärder. Entiteter toobe lagras i tabeller måste vara av en typ som stöds, till exempel härleds från hello [TableEntity] [ dotnet_TableEntity] klass. Entitetsegenskaper som toostore i en tabell måste vara offentliga egenskaper av typen hello och stöder både komma och inställningen av värden. Dessutom *måste* entitetstypen exponera en parameterlös konstruktor.

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

Tabellåtgärder som rör entiteter utförs via hello [CloudTable] [ dotnet_CloudTable] objekt som du skapade tidigare i hello ”skapa en tabell”. hello åtgärden toobe utföras representeras av en [TableOperation] [ dotnet_TableOperation] objekt. hello följande kodexempel visar hello skapandet av hello [CloudTable] [ dotnet_CloudTable] objekt och sedan en **CustomerEntity** objekt. tooprepare hello åtgärden en [TableOperation] [ dotnet_TableOperation] objekt skapas tooinsert hello kundentiteten i hello-tabellen. Slutligen hello-åtgärden körs genom att anropa [CloudTable][dotnet_CloudTable].[ Köra][dotnet_CloudTable_Execute].

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

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
Du kan infoga en batch med entiteter i en tabell i samma skrivåtgärd. Några anmärkningar om batchåtgärder:

* Du kan utföra uppdateringar, borttagningar och infogningar i hello enkel samma batchåtgärd.
* En batchåtgärd kan omfatta upp too100 entiteter.
* Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.
* Även om det är möjligt tooperform en fråga som en batchåtgärd måste den vara hello enda åtgärden i hello batch.

hello följande kodexempel skapar två entitetsobjekt och lägger till varje för[TableBatchOperation] [ dotnet_TableBatchOperation] med hjälp av hello [infoga] [ dotnet_TableBatchOperation_Insert] metod. Sedan [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] tooexecute hello åtgärden anropas.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

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

## <a name="retrieve-all-entities-in-a-partition"></a>Hämta alla entiteter i en partition
tooquery en tabell efter alla entiteter i en partition, Använd en [TableQuery] [ dotnet_TableQuery] objekt. hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”. Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct hello query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print hello fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Hämta ett intervall med enheter i en partition
Om du inte vill tooquery alla entiteter i en partition kan ange du ett intervall genom att kombinera hello partitionsnyckelfiltret med ett radnyckelfilter. hello följande kodexempel används två filter tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav före ”E” i alfabetet hello och skriver ut hello frågeresultat.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through hello results, displaying information about hello entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a>Hämta en enda entitet
Du kan skriva en fråga tooretrieve en enda, specifik entitet. hello följande kod används [TableOperation] [ dotnet_TableOperation] toospecify hello kunden ”Ben Smith”. Den här metoden returnerar endast en entitet i stället för en samling och hello returneras värdet i [TableResult][dotnet_TableResult].[ Resultatet] [ dotnet_TableResult_Result] är en **CustomerEntity** objekt. Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print hello phone number of hello result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("hello phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a>Ersätta en entitet
Hämta den från tabelltjänsten hello tooupdate en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar. hello ändrar följande kod en befintlig kunds telefonnummer. I stället för att anropa [Insert][dotnet_TableOperation_Insert] använder den här koden [Replace][dotnet_TableOperation_Replace]. [Ersätt] [ dotnet_TableOperation_Replace] orsaker hello toobe för entiteten ersätts helt på hello-servern om hello entiteten på hello-servern har ändrats sedan den hämtades, i så fall hello åtgärden misslyckas. Det här felet är tooprevent ditt program oavsiktligt skriver över en ändring som gjorts mellan hello hämtning och uppdatering av en annan komponent i ditt program. hello korrekt hantering av det här felet är tooretrieve hello entiteten igen, gör dina ändringar (om det är fortfarande giltigt) och sedan köra en [ersätta] [ dotnet_TableOperation_Replace] igen. hello nästa avsnitt visar hur toooverride problemet.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change hello phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create hello Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute hello operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a>Infoga eller ersätta en entitet
[Ersätt] [ dotnet_TableOperation_Replace] åtgärder misslyckas om hello entiteten har ändrats sedan den hämtades från hello-servern. Dessutom måste du hämta hello entitet från hello servern först för hello [ersätta] [ dotnet_TableOperation_Replace] åtgärden toobe lyckades. Ibland, men vet du inte om hello entiteten finns på hello-servern och hello aktuella värden som lagras i den är irrelevanta. Dina uppdateringar ska skriva över alla. tooaccomplish, använder du en [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] igen. Den här åtgärden infogar hello entiteten om den finns inte eller ersätter den om den finns, oavsett när hello senaste uppdateringen gjordes.

I följande kodexempel hello, skapas en kundentitet för 'Fred Jones' och infogade i hello ”personer”-tabellen. Nu ska vi använda hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] åtgärden toosave en entitet med hello samma partitionsnyckel (Karlsson) och raden nyckel (Fred) toohello servern nu med ett annat värde för hello PhoneNumber Egenskapen. Eftersom vi använder [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] ersätts alla dess egenskapsvärden. Men om en 'Fred Jones' enhet inte ännu redan finns i hello tabellen, skulle det har infogats.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute hello operation.
table.Execute(insertOperation);

// Create another customer entity with hello same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it toothe
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create hello InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute hello operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, hello entity would be
// added toohello table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper
En tabellfråga kan hämta bara några få egenskaper från en entitet i stället för alla hello entitetsegenskaper. Den här tekniken, kallad projektion, minskar bandbredden och kan förbättra frågeprestanda, i synnerhet för stora entiteter. hello-fråga i hello följande kod returnerar bara hello e-postadresserna för entiteter i hello tabell. Detta görs med hjälp av en fråga med [DynamicTableEntity][dotnet_DynamicTableEntity] och [EntityResolver][dotnet_EntityResolver]. Du kan lära dig mer om projektion i hello [Introducing Upsert and Query Projection blogginlägget][blog_post_upsert]. Projektion stöds inte av hello storage-emulatorn, så den här koden endast körs när du använder ett konto i tabelltjänsten hello.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define hello query, and select only hello Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver toowork with hello entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan enkelt ta bort en enhet när du har hämtat den genom att använda hello samma mönster för att uppdatera en entitet. hello följande kod hämtar och tar bort en kundentitet.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create hello Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute hello operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve hello entity.");
}
```

## <a name="delete-a-table"></a>Ta bort en tabell
Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto. En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning.

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete hello table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a>Hämta entiteter på sidor asynkront
Om du läser ett stort antal entiteter och du vill ha tooprocess/Visa entiteter när de hämtas i stället för att vänta på att alla tooreturn, kan du hämta entiteter med hjälp av en segmenterad fråga. Det här exemplet visar hur tooreturn resultat på sidor med hello Async-Await-mönstret så att körningen inte blockeras medan du väntar på ett stort antal resultat tooreturn. Mer information om hur du använder hello Async-Await-mönstret i .NET, finns [asynkron programmering med Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

```csharp
// Initialize a default TableQuery tooretrieve all hello entities in hello table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize hello continuation token toonull toostart from hello beginning of hello table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up too1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign hello new continuation token tootell hello service where to
    // continue on hello next iteration (or null if it has reached hello end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print hello number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating hello end of hello table.
} while(continuationToken != null);
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i Table storage, följa dessa länkar toolearn om mer komplexa lagringsuppgifter:

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* Du hittar fler Table Storage-exempel i [Komma igång med Azure Table Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
* Visa hello tabell referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er:
* [Storage-klientbibliotek för .NET-referens](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [REST API-referens](http://msdn.microsoft.com/library/azure/dd179355)
* Lär dig hur toosimplify hello koden du skriver toowork med Azure Storage med hjälp av hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
* Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.
* [Komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) toostore Ostrukturerade data.
* [Ansluta tooSQL databasen med hjälp av .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relationella data.

[Download and install hello Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[blog_post_upsert]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx
