---
title: "aaaHow tooget igång med tabellagring och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur tooget igång med Azure Table storage i ASP.NET Core-projekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: e3eb3f3e65456108dd3cde7e3e470f98ba456e35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="1c7ba-103">Hur tooget igång med Azure Table storage och Visual Studio anslutna tjänster</span><span class="sxs-lookup"><span data-stu-id="1c7ba-103">How tooget started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="1c7ba-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1c7ba-104">Overview</span></span>
<span data-ttu-id="1c7ba-105">Den här artikeln beskriver hur få igång med Azure Table storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="1c7ba-106">hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-106">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="1c7ba-107">hello-tjänsten är en NoSQL-databas som accepterar autentiserade anrop inuti och utanför hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-107">hello service is a NoSQL data store that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="1c7ba-108">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="1c7ba-109">Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="1c7ba-110">Mer allmän information om hur du använder Azure Table storage finns [komma igång med Azure Table storage med hjälp av .NET](../storage/storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="1c7ba-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="1c7ba-111">tooget igång, måste du först toocreate en tabell i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-111">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="1c7ba-112">Vi lära dig hur toocreate en Azure tabell i kod.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-112">We'll show you how toocreate an Azure table in code.</span></span> <span data-ttu-id="1c7ba-113">Vi också lära dig hur tooperform Standardtabell och entiteten åtgärder, till exempel lägga till, ändra, läsa och läsa tabell entiteter.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-113">We'll also show you how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="1c7ba-114">hello prover är skrivna i C\# code och använda hello Azure Storage-klientbibliotek för .NET.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-114">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="1c7ba-115">**Obs** -vissa hello API: er som utför anrop ut tooAzure lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-115">**NOTE** - Some of hello APIs that perform calls out tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="1c7ba-116">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="1c7ba-117">hello koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-117">hello code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="1c7ba-118">Åtkomst till tabeller i koden</span><span class="sxs-lookup"><span data-stu-id="1c7ba-118">Access tables in code</span></span>
<span data-ttu-id="1c7ba-119">tooaccess tabeller i ASP.NET Core projekt, behöver du tooinclude hello följande objekt tooany C#-källfiler som åt Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-119">tooaccess tables in ASP.NET Core projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="1c7ba-120">Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-120">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="1c7ba-121">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="1c7ba-122">Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-122">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="1c7ba-123">**Obs** -använda alla hello ovan kod framför hello koden i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-123">**NOTE** - Use all of hello above code in front of hello code in hello following samples.</span></span>
3. <span data-ttu-id="1c7ba-124">Hämta en **CloudTableClient** objekt tooreference hello tabellobjekt i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-124">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="1c7ba-125">Hämta en **CloudTable** refererar till objektet tooreference en viss tabell och enheter.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-125">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="1c7ba-126">Skapa en tabell i koden</span><span class="sxs-lookup"><span data-stu-id="1c7ba-126">Create a table in code</span></span>
<span data-ttu-id="1c7ba-127">toocreate hello Azure-tabellen, Lägg till ett anrop för**CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-127">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync()**.</span></span>

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="1c7ba-128">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="1c7ba-128">Add an entity tooa table</span></span>
<span data-ttu-id="1c7ba-129">tooadd en entitet tooa tabell som du skapar en klass som definierar hello egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-129">tooadd an entity tooa table you create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="1c7ba-130">hello följande kod definierar en entitetsklass kallas **CustomerEntity** som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-130">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

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

<span data-ttu-id="1c7ba-131">Tabellåtgärder som rör entiteter utförs med hjälp av hello **CloudTable** objekt du skapade tidigare i ”åtkomst tabeller i kod”.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-131">Table operations involving entities are done using hello **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="1c7ba-132">Hej **TableOperation** -objektet representerar hello åtgärden toobe är klar.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-132">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="1c7ba-133">Hej följande exempel visas hur toocreate en **CloudTable** objekt och en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-133">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="1c7ba-134">tooprepare hello åtgärden en **TableOperation** skapas tooinsert hello kundentiteten i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-134">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="1c7ba-135">Slutligen körs åtgärden hello genom att anropa CloudTable.ExecuteAsync.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-135">Finally, hello operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="1c7ba-136">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="1c7ba-136">Insert a batch of entities</span></span>
<span data-ttu-id="1c7ba-137">Du kan infoga flera entiteter i en tabell i en enda skrivning.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="1c7ba-138">hello följande kodexempel skapar två entitetsobjekt (”Jeff Smith” och ”Ben Smith”), lägger till dem tooa **TableBatchOperation** objekt med hello **infoga** metoden och sedan startar hello åtgärden genom att anropar CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-138">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello **Insert** method, and then starts hello operation by calling CloudTable.ExecuteBatchAsync.</span></span>

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="1c7ba-139">Hämta alla hello entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="1c7ba-139">Get all of hello entities in a partition</span></span>
<span data-ttu-id="1c7ba-140">tooquery en tabell för alla hello entiteter i en partition, Använd en **TableQuery** objekt.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-140">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="1c7ba-141">hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-141">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="1c7ba-142">Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-142">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print hello fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a><span data-ttu-id="1c7ba-143">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="1c7ba-143">Get a single entity</span></span>
<span data-ttu-id="1c7ba-144">Du kan skriva en fråga tooget en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-144">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="1c7ba-145">hello följande kod används en **TableOperation** objekt toospecify en kund som heter ”Ben Smith”.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-145">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="1c7ba-146">Den här metoden returnerar endast en entitet i stället för en samling och hello returneras värdet i **TableResult.Result** är en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-146">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="1c7ba-147">Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från hello **tabell** service.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-147">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="1c7ba-148">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="1c7ba-148">Delete an entity</span></span>
<span data-ttu-id="1c7ba-149">Du kan ta bort en enhet när du har hittat.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="1c7ba-150">hello följande kod söker efter en kundentitet med namnet ”Ben Smith” och om den hittar det tar bort den.</span><span class="sxs-lookup"><span data-stu-id="1c7ba-150">hello following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign hello result tooa CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create hello Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute hello operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete hello entity.");

## <a name="next-steps"></a><span data-ttu-id="1c7ba-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c7ba-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

