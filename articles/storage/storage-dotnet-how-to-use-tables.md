---
title: "aaaGet igång med Azure Table storage med hjälp av .NET | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: marsma
ms.openlocfilehash: 9635079d61d874ff7f4bc9e7d610e0ad54b4fd6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a><span data-ttu-id="325f5-103">Komma igång med Azure Table Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="325f5-103">Get started with Azure Table storage using .NET</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="325f5-104">Azure Table storage är en tjänst som lagrar strukturerade NoSQL i hello moln, vilket ger ett nyckelattribut datalager med en schemalös design.</span><span class="sxs-lookup"><span data-stu-id="325f5-104">Azure Table storage is a service that stores structured NoSQL data in hello cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="325f5-105">Eftersom Table storage är schemalös är det enkelt tooadapt dina data efter hello behov utvecklas.</span><span class="sxs-lookup"><span data-stu-id="325f5-105">Because Table storage is schemaless, it's easy tooadapt your data as hello needs of your application evolve.</span></span> <span data-ttu-id="325f5-106">Åtkomst till tooTable storage data är snabb och kostnadseffektiv för många typer av program och är vanligtvis lägre kostnad än traditionella SQL för motsvarande volymer med data.</span><span class="sxs-lookup"><span data-stu-id="325f5-106">Access tooTable storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="325f5-107">Du kan använda Table storage toostore flexibla datauppsättningar som användardata för webbprogram, adressböcker, enhetsinformation och andra typer av metadata som din tjänst kräver.</span><span class="sxs-lookup"><span data-stu-id="325f5-107">You can use Table storage toostore flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="325f5-108">Du kan lagra valfritt antal enheter i en tabell och ett lagringskonto kan innehålla valfritt antal tabeller, upp toohello kapacitetsgränsen för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="325f5-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up toohello capacity limit of hello storage account.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="325f5-109">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="325f5-109">About this tutorial</span></span>
<span data-ttu-id="325f5-110">De här självstudierna visar hur toouse hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) i några vanliga scenarier för Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="325f5-110">This tutorial shows you how toouse hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in some common Azure Table storage scenarios.</span></span> <span data-ttu-id="325f5-111">Dessa scenarier illustreras med C#-exempel och visar hur du skapar och tar bort en tabell och hur du infogar, uppdaterar, tar bort och hämtar data från tabeller.</span><span class="sxs-lookup"><span data-stu-id="325f5-111">These scenarios are presented using C# examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="325f5-112">Krav</span><span class="sxs-lookup"><span data-stu-id="325f5-112">Prerequisites</span></span>

<span data-ttu-id="325f5-113">Du behöver hello följande toocomplete den här självstudiekursen har:</span><span class="sxs-lookup"><span data-stu-id="325f5-113">You need hello following toocomplete this tutorial successfully:</span></span>

* [<span data-ttu-id="325f5-114">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="325f5-114">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="325f5-115">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="325f5-115">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="325f5-116">Azure Configuration Manager för .NET</span><span class="sxs-lookup"><span data-stu-id="325f5-116">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [<span data-ttu-id="325f5-117">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="325f5-117">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="325f5-118">Fler exempel</span><span class="sxs-lookup"><span data-stu-id="325f5-118">More samples</span></span>
<span data-ttu-id="325f5-119">Ytterligare exempel med Table Storage finns i [Komma igång med Azure Table Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="325f5-119">For additional examples using Table storage, see [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span></span> <span data-ttu-id="325f5-120">Du kan ladda ned exempelprogrammet hello och kör den, eller bläddra hello koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="325f5-120">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="325f5-121">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="325f5-121">Add using directives</span></span>
<span data-ttu-id="325f5-122">Lägg till följande hello **med** direktiven toohello överkant hello `Program.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="325f5-122">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="325f5-123">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="325f5-123">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a><span data-ttu-id="325f5-124">Skapa hello tabelltjänstens klient</span><span class="sxs-lookup"><span data-stu-id="325f5-124">Create hello Table service client</span></span>
<span data-ttu-id="325f5-125">Hej [CloudTableClient] [ dotnet_CloudTableClient] klassen kan du tooretrieve tabeller och de entiteter som lagras i Table storage.</span><span class="sxs-lookup"><span data-stu-id="325f5-125">hello [CloudTableClient][dotnet_CloudTableClient] class enables you tooretrieve tables and entities stored in Table storage.</span></span> <span data-ttu-id="325f5-126">Här är ett sätt toocreate hello tabelltjänstens klient:</span><span class="sxs-lookup"><span data-stu-id="325f5-126">Here's one way toocreate hello Table service client:</span></span>

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

<span data-ttu-id="325f5-127">Nu är du redo toowrite kod som läser data från och skriver tooTable datalagring.</span><span class="sxs-lookup"><span data-stu-id="325f5-127">Now you are ready toowrite code that reads data from and writes data tooTable storage.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="325f5-128">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="325f5-128">Create a table</span></span>
<span data-ttu-id="325f5-129">Det här exemplet illustrerar hur toocreate en tabell om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="325f5-129">This example shows how toocreate a table if it does not already exist:</span></span>

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

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="325f5-130">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="325f5-130">Add an entity tooa table</span></span>
<span data-ttu-id="325f5-131">Entiteter mappar tooC #-objekt med hjälp av en anpassad klass som härleds från [TableEntity][dotnet_TableEntity].</span><span class="sxs-lookup"><span data-stu-id="325f5-131">Entities map tooC# objects by using a custom class derived from [TableEntity][dotnet_TableEntity].</span></span> <span data-ttu-id="325f5-132">tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="325f5-132">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="325f5-133">hello definierar följande kod en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="325f5-133">hello following code defines an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="325f5-134">Tillsammans identifieras en entitets partition och radnyckel kan i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="325f5-134">Together, an entity's partition and row key uniquely identify it in hello table.</span></span> <span data-ttu-id="325f5-135">Entiteter med samma partitionsnyckel kan frågas snabbare än entiteter med olika hello partitions nycklar, men med olika partitionsnycklar möjliggör bättre skalbarhet av parallella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="325f5-135">Entities with hello same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="325f5-136">Entiteter toobe lagras i tabeller måste vara av en typ som stöds, till exempel härleds från hello [TableEntity] [ dotnet_TableEntity] klass.</span><span class="sxs-lookup"><span data-stu-id="325f5-136">Entities toobe stored in tables must be of a supported type, for example derived from hello [TableEntity][dotnet_TableEntity] class.</span></span> <span data-ttu-id="325f5-137">Entitetsegenskaper som toostore i en tabell måste vara offentliga egenskaper av typen hello och stöder både komma och inställningen av värden.</span><span class="sxs-lookup"><span data-stu-id="325f5-137">Entity properties you'd like toostore in a table must be public properties of hello type, and support both getting and setting of values.</span></span> <span data-ttu-id="325f5-138">Dessutom *måste* entitetstypen exponera en parameterlös konstruktor.</span><span class="sxs-lookup"><span data-stu-id="325f5-138">Also, your entity type *must* expose a parameter-less constructor.</span></span>

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

<span data-ttu-id="325f5-139">Tabellåtgärder som rör entiteter utförs via hello [CloudTable] [ dotnet_CloudTable] objekt som du skapade tidigare i hello ”skapa en tabell”.</span><span class="sxs-lookup"><span data-stu-id="325f5-139">Table operations that involve entities are performed via hello [CloudTable][dotnet_CloudTable] object that you created earlier in hello "Create a table" section.</span></span> <span data-ttu-id="325f5-140">hello åtgärden toobe utföras representeras av en [TableOperation] [ dotnet_TableOperation] objekt.</span><span class="sxs-lookup"><span data-stu-id="325f5-140">hello operation toobe performed is represented by a [TableOperation][dotnet_TableOperation] object.</span></span> <span data-ttu-id="325f5-141">hello följande kodexempel visar hello skapandet av hello [CloudTable] [ dotnet_CloudTable] objekt och sedan en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="325f5-141">hello following code example shows hello creation of hello [CloudTable][dotnet_CloudTable] object and then a **CustomerEntity** object.</span></span> <span data-ttu-id="325f5-142">tooprepare hello åtgärden en [TableOperation] [ dotnet_TableOperation] objekt skapas tooinsert hello kundentiteten i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="325f5-142">tooprepare hello operation, a [TableOperation][dotnet_TableOperation] object is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="325f5-143">Slutligen hello-åtgärden körs genom att anropa [CloudTable][dotnet_CloudTable].[ Köra][dotnet_CloudTable_Execute].</span><span class="sxs-lookup"><span data-stu-id="325f5-143">Finally, hello operation is executed by calling [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span></span>

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

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="325f5-144">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="325f5-144">Insert a batch of entities</span></span>
<span data-ttu-id="325f5-145">Du kan infoga en batch med entiteter i en tabell i samma skrivåtgärd.</span><span class="sxs-lookup"><span data-stu-id="325f5-145">You can insert a batch of entities into a table in one write operation.</span></span> <span data-ttu-id="325f5-146">Några anmärkningar om batchåtgärder:</span><span class="sxs-lookup"><span data-stu-id="325f5-146">Some other notes on batch operations:</span></span>

* <span data-ttu-id="325f5-147">Du kan utföra uppdateringar, borttagningar och infogningar i hello enkel samma batchåtgärd.</span><span class="sxs-lookup"><span data-stu-id="325f5-147">You can perform updates, deletes, and inserts in hello same single batch operation.</span></span>
* <span data-ttu-id="325f5-148">En batchåtgärd kan omfatta upp too100 entiteter.</span><span class="sxs-lookup"><span data-stu-id="325f5-148">A single batch operation can include up too100 entities.</span></span>
* <span data-ttu-id="325f5-149">Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="325f5-149">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="325f5-150">Även om det är möjligt tooperform en fråga som en batchåtgärd måste den vara hello enda åtgärden i hello batch.</span><span class="sxs-lookup"><span data-stu-id="325f5-150">While it is possible tooperform a query as a batch operation, it must be hello only operation in hello batch.</span></span>

<span data-ttu-id="325f5-151">hello följande kodexempel skapar två entitetsobjekt och lägger till varje för[TableBatchOperation] [ dotnet_TableBatchOperation] med hjälp av hello [infoga] [ dotnet_TableBatchOperation_Insert] metod.</span><span class="sxs-lookup"><span data-stu-id="325f5-151">hello following code example creates two entity objects and adds each too[TableBatchOperation][dotnet_TableBatchOperation] by using hello [Insert][dotnet_TableBatchOperation_Insert] method.</span></span> <span data-ttu-id="325f5-152">Sedan [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] tooexecute hello åtgärden anropas.</span><span class="sxs-lookup"><span data-stu-id="325f5-152">Then, [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] is called tooexecute hello operation.</span></span>

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

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="325f5-153">Hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="325f5-153">Retrieve all entities in a partition</span></span>
<span data-ttu-id="325f5-154">tooquery en tabell efter alla entiteter i en partition, Använd en [TableQuery] [ dotnet_TableQuery] objekt.</span><span class="sxs-lookup"><span data-stu-id="325f5-154">tooquery a table for all entities in a partition, use a [TableQuery][dotnet_TableQuery] object.</span></span> <span data-ttu-id="325f5-155">hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="325f5-155">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="325f5-156">Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="325f5-156">This example prints hello fields of each entity in hello query results toohello console.</span></span>

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

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="325f5-157">Hämta ett intervall med enheter i en partition</span><span class="sxs-lookup"><span data-stu-id="325f5-157">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="325f5-158">Om du inte vill tooquery alla entiteter i en partition kan ange du ett intervall genom att kombinera hello partitionsnyckelfiltret med ett radnyckelfilter.</span><span class="sxs-lookup"><span data-stu-id="325f5-158">If you don't want tooquery all entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="325f5-159">hello följande kodexempel används två filter tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav före ”E” i alfabetet hello och skriver ut hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="325f5-159">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter before 'E' in hello alphabet, then prints hello query results.</span></span>

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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="325f5-160">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="325f5-160">Retrieve a single entity</span></span>
<span data-ttu-id="325f5-161">Du kan skriva en fråga tooretrieve en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="325f5-161">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="325f5-162">hello följande kod används [TableOperation] [ dotnet_TableOperation] toospecify hello kunden ”Ben Smith”.</span><span class="sxs-lookup"><span data-stu-id="325f5-162">hello following code uses [TableOperation][dotnet_TableOperation] toospecify hello customer 'Ben Smith'.</span></span> <span data-ttu-id="325f5-163">Den här metoden returnerar endast en entitet i stället för en samling och hello returneras värdet i [TableResult][dotnet_TableResult].[ Resultatet] [ dotnet_TableResult_Result] är en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="325f5-163">This method returns just one entity rather than a collection, and hello returned value in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is a **CustomerEntity** object.</span></span> <span data-ttu-id="325f5-164">Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="325f5-164">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

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

## <a name="replace-an-entity"></a><span data-ttu-id="325f5-165">Ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="325f5-165">Replace an entity</span></span>
<span data-ttu-id="325f5-166">Hämta den från tabelltjänsten hello tooupdate en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="325f5-166">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="325f5-167">hello ändrar följande kod en befintlig kunds telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="325f5-167">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="325f5-168">I stället för att anropa [Insert][dotnet_TableOperation_Insert] använder den här koden [Replace][dotnet_TableOperation_Replace].</span><span class="sxs-lookup"><span data-stu-id="325f5-168">Instead of calling [Insert][dotnet_TableOperation_Insert], this code uses [Replace][dotnet_TableOperation_Replace].</span></span> <span data-ttu-id="325f5-169">[Ersätt] [ dotnet_TableOperation_Replace] orsaker hello toobe för entiteten ersätts helt på hello-servern om hello entiteten på hello-servern har ändrats sedan den hämtades, i så fall hello åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="325f5-169">[Replace][dotnet_TableOperation_Replace] causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="325f5-170">Det här felet är tooprevent ditt program oavsiktligt skriver över en ändring som gjorts mellan hello hämtning och uppdatering av en annan komponent i ditt program.</span><span class="sxs-lookup"><span data-stu-id="325f5-170">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="325f5-171">hello korrekt hantering av det här felet är tooretrieve hello entiteten igen, gör dina ändringar (om det är fortfarande giltigt) och sedan köra en [ersätta] [ dotnet_TableOperation_Replace] igen.</span><span class="sxs-lookup"><span data-stu-id="325f5-171">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another [Replace][dotnet_TableOperation_Replace] operation.</span></span> <span data-ttu-id="325f5-172">hello nästa avsnitt visar hur toooverride problemet.</span><span class="sxs-lookup"><span data-stu-id="325f5-172">hello next section will show you how toooverride this behavior.</span></span>

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

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="325f5-173">Infoga eller ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="325f5-173">Insert-or-replace an entity</span></span>
<span data-ttu-id="325f5-174">[Ersätt] [ dotnet_TableOperation_Replace] åtgärder misslyckas om hello entiteten har ändrats sedan den hämtades från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="325f5-174">[Replace][dotnet_TableOperation_Replace] operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="325f5-175">Dessutom måste du hämta hello entitet från hello servern först för hello [ersätta] [ dotnet_TableOperation_Replace] åtgärden toobe lyckades.</span><span class="sxs-lookup"><span data-stu-id="325f5-175">Furthermore, you must retrieve hello entity from hello server first in order for hello [Replace][dotnet_TableOperation_Replace] operation toobe successful.</span></span> <span data-ttu-id="325f5-176">Ibland, men vet du inte om hello entiteten finns på hello-servern och hello aktuella värden som lagras i den är irrelevanta.</span><span class="sxs-lookup"><span data-stu-id="325f5-176">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant.</span></span> <span data-ttu-id="325f5-177">Dina uppdateringar ska skriva över alla.</span><span class="sxs-lookup"><span data-stu-id="325f5-177">Your update should overwrite them all.</span></span> <span data-ttu-id="325f5-178">tooaccomplish, använder du en [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] igen.</span><span class="sxs-lookup"><span data-stu-id="325f5-178">tooaccomplish this, you would use an [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation.</span></span> <span data-ttu-id="325f5-179">Den här åtgärden infogar hello entiteten om den finns inte eller ersätter den om den finns, oavsett när hello senaste uppdateringen gjordes.</span><span class="sxs-lookup"><span data-stu-id="325f5-179">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span>

<span data-ttu-id="325f5-180">I följande kodexempel hello, skapas en kundentitet för 'Fred Jones' och infogade i hello ”personer”-tabellen.</span><span class="sxs-lookup"><span data-stu-id="325f5-180">In hello following code example, a customer entity for 'Fred Jones' is created and inserted into hello 'people' table.</span></span> <span data-ttu-id="325f5-181">Nu ska vi använda hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] åtgärden toosave en entitet med hello samma partitionsnyckel (Karlsson) och raden nyckel (Fred) toohello servern nu med ett annat värde för hello PhoneNumber Egenskapen.</span><span class="sxs-lookup"><span data-stu-id="325f5-181">Next, we use hello [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation toosave an entity with hello same partition key (Jones) and row key (Fred) toohello server, this time with a different value for hello PhoneNumber property.</span></span> <span data-ttu-id="325f5-182">Eftersom vi använder [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] ersätts alla dess egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="325f5-182">Because we use [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], all of its property values are replaced.</span></span> <span data-ttu-id="325f5-183">Men om en 'Fred Jones' enhet inte ännu redan finns i hello tabellen, skulle det har infogats.</span><span class="sxs-lookup"><span data-stu-id="325f5-183">However, if a 'Fred Jones' entity hadn't already existed in hello table, it would have been inserted.</span></span>

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

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="325f5-184">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="325f5-184">Query a subset of entity properties</span></span>
<span data-ttu-id="325f5-185">En tabellfråga kan hämta bara några få egenskaper från en entitet i stället för alla hello entitetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="325f5-185">A table query can retrieve just a few properties from an entity instead of all hello entity properties.</span></span> <span data-ttu-id="325f5-186">Den här tekniken, kallad projektion, minskar bandbredden och kan förbättra frågeprestanda, i synnerhet för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="325f5-186">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="325f5-187">hello-fråga i hello följande kod returnerar bara hello e-postadresserna för entiteter i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="325f5-187">hello query in hello following code returns only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="325f5-188">Detta görs med hjälp av en fråga med [DynamicTableEntity][dotnet_DynamicTableEntity] och [EntityResolver][dotnet_EntityResolver].</span><span class="sxs-lookup"><span data-stu-id="325f5-188">This is done by using a query of [DynamicTableEntity][dotnet_DynamicTableEntity] and also [EntityResolver][dotnet_EntityResolver].</span></span> <span data-ttu-id="325f5-189">Du kan lära dig mer om projektion i hello [Introducing Upsert and Query Projection blogginlägget][blog_post_upsert].</span><span class="sxs-lookup"><span data-stu-id="325f5-189">You can learn more about projection in hello [Introducing Upsert and Query Projection blog post][blog_post_upsert].</span></span> <span data-ttu-id="325f5-190">Projektion stöds inte av hello storage-emulatorn, så den här koden endast körs när du använder ett konto i tabelltjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="325f5-190">Projection is not supported by hello storage emulator, so this code runs only when you're using an account in hello Table service.</span></span>

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

## <a name="delete-an-entity"></a><span data-ttu-id="325f5-191">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="325f5-191">Delete an entity</span></span>
<span data-ttu-id="325f5-192">Du kan enkelt ta bort en enhet när du har hämtat den genom att använda hello samma mönster för att uppdatera en entitet.</span><span class="sxs-lookup"><span data-stu-id="325f5-192">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="325f5-193">hello följande kod hämtar och tar bort en kundentitet.</span><span class="sxs-lookup"><span data-stu-id="325f5-193">hello following code retrieves and deletes a customer entity.</span></span>

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

## <a name="delete-a-table"></a><span data-ttu-id="325f5-194">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="325f5-194">Delete a table</span></span>
<span data-ttu-id="325f5-195">Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="325f5-195">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="325f5-196">En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="325f5-196">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>

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

## <a name="retrieve-entities-in-pages-asynchronously"></a><span data-ttu-id="325f5-197">Hämta entiteter på sidor asynkront</span><span class="sxs-lookup"><span data-stu-id="325f5-197">Retrieve entities in pages asynchronously</span></span>
<span data-ttu-id="325f5-198">Om du läser ett stort antal entiteter och du vill ha tooprocess/Visa entiteter när de hämtas i stället för att vänta på att alla tooreturn, kan du hämta entiteter med hjälp av en segmenterad fråga.</span><span class="sxs-lookup"><span data-stu-id="325f5-198">If you are reading a large number of entities, and you want tooprocess/display entities as they are retrieved rather than waiting for them all tooreturn, you can retrieve entities by using a segmented query.</span></span> <span data-ttu-id="325f5-199">Det här exemplet visar hur tooreturn resultat på sidor med hello Async-Await-mönstret så att körningen inte blockeras medan du väntar på ett stort antal resultat tooreturn.</span><span class="sxs-lookup"><span data-stu-id="325f5-199">This example shows how tooreturn results in pages by using hello Async-Await pattern so that execution is not blocked while you're waiting for a large set of results tooreturn.</span></span> <span data-ttu-id="325f5-200">Mer information om hur du använder hello Async-Await-mönstret i .NET, finns [asynkron programmering med Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="325f5-200">For more details on using hello Async-Await pattern in .NET, see [Asynchronous programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="325f5-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="325f5-201">Next steps</span></span>
<span data-ttu-id="325f5-202">Nu när du har lärt dig hello grunderna i Table storage, följa dessa länkar toolearn om mer komplexa lagringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="325f5-202">Now that you've learned hello basics of Table storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="325f5-203">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="325f5-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="325f5-204">Du hittar fler Table Storage-exempel i [Komma igång med Azure Table Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span><span class="sxs-lookup"><span data-stu-id="325f5-204">See more Table storage samples in [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span></span>
* <span data-ttu-id="325f5-205">Visa hello tabell referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er:</span><span class="sxs-lookup"><span data-stu-id="325f5-205">View hello Table service reference documentation for complete details about available APIs:</span></span>
* [<span data-ttu-id="325f5-206">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="325f5-206">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [<span data-ttu-id="325f5-207">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="325f5-207">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="325f5-208">Lär dig hur toosimplify hello koden du skriver toowork med Azure Storage med hjälp av hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="325f5-208">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span></span>
* <span data-ttu-id="325f5-209">Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="325f5-209">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
* <span data-ttu-id="325f5-210">[Komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) toostore Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="325f5-210">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
* <span data-ttu-id="325f5-211">[Ansluta tooSQL databasen med hjälp av .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relationella data.</span><span class="sxs-lookup"><span data-stu-id="325f5-211">[Connect tooSQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relational data.</span></span>

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
