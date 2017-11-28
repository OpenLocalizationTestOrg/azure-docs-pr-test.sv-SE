---
title: "Hur du kommer igång med tabellagring och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur du kommer igång med Azure Table storage i ASP.NET Core-projekt i Visual Studio efter anslutning till ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
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
ms.openlocfilehash: 8d05fe3ed9a5c66f186a930d4107162c1f322c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="d89e9-103">Hur du kommer igång med Azure Table storage och Visual Studio anslutna tjänster</span><span class="sxs-lookup"><span data-stu-id="d89e9-103">How to get started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="d89e9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d89e9-104">Overview</span></span>
<span data-ttu-id="d89e9-105">Den här artikeln beskriver hur komma igång med Azure Table storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av Visual Studio **Lägg till anslutna tjänster** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d89e9-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="d89e9-106">Tjänsten Azure Table storage kan du lagra stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="d89e9-106">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="d89e9-107">Tjänsten är en NoSQL-databas som accepterar autentiserade anrop inuti och utanför Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="d89e9-107">The service is a NoSQL data store that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="d89e9-108">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="d89e9-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="d89e9-109">Den **Lägg till anslutna tjänster** åtgärden installerar lämpligt NuGet-paket för att komma åt Azure-lagring i ditt projekt och lägger till anslutningssträngen för lagringskontot konfigurationsfilerna projektet.</span><span class="sxs-lookup"><span data-stu-id="d89e9-109">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="d89e9-110">Mer allmän information om hur du använder Azure Table storage finns [komma igång med Azure Table storage med hjälp av .NET](../storage/storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="d89e9-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="d89e9-111">Om du vill komma igång, måste du först skapa en tabell i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d89e9-111">To get started, you first need to create a table in your storage account.</span></span> <span data-ttu-id="d89e9-112">Vi kommer att visa hur du skapar en Azure-tabellen i koden.</span><span class="sxs-lookup"><span data-stu-id="d89e9-112">We'll show you how to create an Azure table in code.</span></span> <span data-ttu-id="d89e9-113">Vi också lära dig hur du utför grundläggande tabell och entiteten åtgärder, till exempel lägga till, ändra, läsa och läsa tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="d89e9-113">We'll also show you how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="d89e9-114">Exemplen är skrivna i C\# code och använda Azure Storage-klientbiblioteket för .NET.</span><span class="sxs-lookup"><span data-stu-id="d89e9-114">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="d89e9-115">**Obs** -vissa av de API: er som utför anrop ut till Azure-lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="d89e9-115">**NOTE** - Some of the APIs that perform calls out to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="d89e9-116">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d89e9-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="d89e9-117">Koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="d89e9-117">The code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="d89e9-118">Åtkomst till tabeller i koden</span><span class="sxs-lookup"><span data-stu-id="d89e9-118">Access tables in code</span></span>
<span data-ttu-id="d89e9-119">För att komma åt tabeller i ASP.NET Core projekt måste du inkludera följande för att inga C# källfiler som kan komma åt Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="d89e9-119">To access tables in ASP.NET Core projects, you need to include the following items to any C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="d89e9-120">Se till att namnrymdsdeklarationer överst i C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="d89e9-120">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="d89e9-121">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d89e9-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d89e9-122">Använd följande kod för att få den dina anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d89e9-122">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="d89e9-123">**Obs** -använda alla koden ovan framför koden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d89e9-123">**NOTE** - Use all of the above code in front of the code in the following samples.</span></span>
3. <span data-ttu-id="d89e9-124">Hämta en **CloudTableClient** objektet att referera till tabellen objekten i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d89e9-124">Get a **CloudTableClient** object to reference the table objects in your storage account.</span></span>  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="d89e9-125">Hämta en **CloudTable** referensobjekt att referera till en viss tabell och enheter.</span><span class="sxs-lookup"><span data-stu-id="d89e9-125">Get a **CloudTable** reference object to reference a specific table and entities.</span></span>
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="d89e9-126">Skapa en tabell i koden</span><span class="sxs-lookup"><span data-stu-id="d89e9-126">Create a table in code</span></span>
<span data-ttu-id="d89e9-127">Om du vill skapa Azure-tabellen, Lägg till ett anrop till **CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="d89e9-127">To create the Azure table, just add a call to **CreateIfNotExistsAsync()**.</span></span>

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="d89e9-128">Lägga till en entitet i en tabell</span><span class="sxs-lookup"><span data-stu-id="d89e9-128">Add an entity to a table</span></span>
<span data-ttu-id="d89e9-129">Om du vill lägga till en entitet i en tabell kan du skapa en klass som definierar egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="d89e9-129">To add an entity to a table you create a class that defines the properties of your entity.</span></span> <span data-ttu-id="d89e9-130">Följande kod definierar en entitetsklass som kallas **CustomerEntity** som använder kundens förnamn som radnyckel och efternamn som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="d89e9-130">The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

<span data-ttu-id="d89e9-131">Tabellåtgärder som rör entiteter utförs med hjälp av den **CloudTable** objekt du skapade tidigare i ”åtkomst tabeller i kod”.</span><span class="sxs-lookup"><span data-stu-id="d89e9-131">Table operations involving entities are done using the **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="d89e9-132">Den **TableOperation** -objektet representerar åtgärden görs.</span><span class="sxs-lookup"><span data-stu-id="d89e9-132">The **TableOperation** object represents the operation to be done.</span></span> <span data-ttu-id="d89e9-133">Följande kodexempel visar hur du skapar en **CloudTable** objekt och en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="d89e9-133">The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="d89e9-134">Att förbereda åtgärden skapas en **TableOperation** skapas som infogar kundentiteten i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d89e9-134">To prepare the operation, a **TableOperation** is created to insert the customer entity into the table.</span></span> <span data-ttu-id="d89e9-135">Slutligen körs åtgärden genom att anropa CloudTable.ExecuteAsync.</span><span class="sxs-lookup"><span data-stu-id="d89e9-135">Finally, the operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="d89e9-136">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="d89e9-136">Insert a batch of entities</span></span>
<span data-ttu-id="d89e9-137">Du kan infoga flera entiteter i en tabell i en enda skrivning.</span><span class="sxs-lookup"><span data-stu-id="d89e9-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="d89e9-138">Följande exempel skapar två entitetsobjekt (”Jeff Smith” och ”Ben Smith”), läggs till i en **TableBatchOperation** objekt med hjälp av **infoga** -metoden och startar sedan åtgärden genom att anropa CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="d89e9-138">The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the **Insert** method, and then starts the operation by calling CloudTable.ExecuteBatchAsync.</span></span>

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a><span data-ttu-id="d89e9-139">Hämta alla enheter i en partition</span><span class="sxs-lookup"><span data-stu-id="d89e9-139">Get all of the entities in a partition</span></span>
<span data-ttu-id="d89e9-140">Om du vill fråga en tabell efter alla entiteter i en partition använder en **TableQuery** objekt.</span><span class="sxs-lookup"><span data-stu-id="d89e9-140">To query a table for all of the entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="d89e9-141">I följande kodexempel anges ett filter för entiteter där partitionsnyckeln är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="d89e9-141">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="d89e9-142">Det här exemplet skriver ut fälten för varje entitet i frågeresultatet till konsolen.</span><span class="sxs-lookup"><span data-stu-id="d89e9-142">This example prints the fields of each entity in the query results to the console.</span></span>

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
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

## <a name="get-a-single-entity"></a><span data-ttu-id="d89e9-143">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="d89e9-143">Get a single entity</span></span>
<span data-ttu-id="d89e9-144">Du kan skriva en fråga om du vill ha en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="d89e9-144">You can write a query to get a single, specific entity.</span></span> <span data-ttu-id="d89e9-145">I följande kod används en **TableOperation** objekt för att ange en kund som heter ”Ben Smith”.</span><span class="sxs-lookup"><span data-stu-id="d89e9-145">The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="d89e9-146">Den här metoden returnerar endast en entitet i stället för en samling och det returnerade värdet i **TableResult.Result** är en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="d89e9-146">This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="d89e9-147">Ange både partitions- och nycklar i en fråga är det snabbaste sättet att hämta en enda entitet från den **tabell** service.</span><span class="sxs-lookup"><span data-stu-id="d89e9-147">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="d89e9-148">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="d89e9-148">Delete an entity</span></span>
<span data-ttu-id="d89e9-149">Du kan ta bort en enhet när du har hittat.</span><span class="sxs-lookup"><span data-stu-id="d89e9-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="d89e9-150">Följande kod söker efter en kundentitet med namnet ”Ben Smith” och om den hittar det tar bort den.</span><span class="sxs-lookup"><span data-stu-id="d89e9-150">The following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a><span data-ttu-id="d89e9-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d89e9-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

