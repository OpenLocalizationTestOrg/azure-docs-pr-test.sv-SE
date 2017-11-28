---
title: "aaaGet igång med tabellagring och Visual Studio anslutna tjänster (molntjänster) | Microsoft Docs"
description: "Hur tooget igång med Azure Table storage i ett molntjänstprojekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: efb16953e05764cb162cbdae4d0eab57f781682d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="de20a-103">Komma igång med Azure-tabellagring och Visual Studio anslutna tjänster (cloud services-projekt)</span><span class="sxs-lookup"><span data-stu-id="de20a-103">Getting started with Azure table storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="de20a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="de20a-104">Overview</span></span>
<span data-ttu-id="de20a-105">Den här artikeln beskriver hur tooget igång med Azure-tabellagring i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i cloud services-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan .</span><span class="sxs-lookup"><span data-stu-id="de20a-105">This article describes how tooget started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="de20a-106">Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.</span><span class="sxs-lookup"><span data-stu-id="de20a-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="de20a-107">hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="de20a-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="de20a-108">hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="de20a-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="de20a-109">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="de20a-109">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="de20a-110">tooget igång, måste du först toocreate en tabell i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="de20a-110">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="de20a-111">Vi lära dig hur toocreate en Azure table i koden och även hur tooperform Standardtabell och entiteten åtgärder, till exempel lägga till, ändra, läsa och läsa tabell entiteter.</span><span class="sxs-lookup"><span data-stu-id="de20a-111">We'll show you how toocreate an Azure table in code, and also how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="de20a-112">hello prover är skrivna i C\# code och använda hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="de20a-112">hello samples are written in C\# code and use hello [Microsoft Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="de20a-113">**Obs:** vissa hello API: er som utför anrop ut tooAzure lagring är asynkron.</span><span class="sxs-lookup"><span data-stu-id="de20a-113">**NOTE:** Some of hello APIs that perform calls out tooAzure storage are asynchronous.</span></span> <span data-ttu-id="de20a-114">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="de20a-114">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="de20a-115">hello koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="de20a-115">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="de20a-116">Se [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md) mer information om programmässigt arbete i tabeller.</span><span class="sxs-lookup"><span data-stu-id="de20a-116">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.</span></span>
* <span data-ttu-id="de20a-117">Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="de20a-117">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="de20a-118">Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="de20a-118">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="de20a-119">Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="de20a-119">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="de20a-120">Åtkomst till tabeller i koden</span><span class="sxs-lookup"><span data-stu-id="de20a-120">Access tables in code</span></span>
<span data-ttu-id="de20a-121">tooaccess tabeller i molntjänstprojekt, behöver du tooinclude hello följande objekt tooany C#-källfiler som åt Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="de20a-121">tooaccess tables in cloud service projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="de20a-122">Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="de20a-122">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="de20a-123">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="de20a-123">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="de20a-124">Använd följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="de20a-124">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > <span data-ttu-id="de20a-125">Använda alla hello ovan kod framför hello koden i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="de20a-125">Use all of hello above code in front of hello code in hello following samples.</span></span>
   > 
   > 
3. <span data-ttu-id="de20a-126">Hämta en **CloudTableClient** objekt tooreference hello tabellobjekt i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="de20a-126">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>
   
         // Create hello table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="de20a-127">Hämta en **CloudTable** refererar till objektet tooreference en viss tabell och enheter.</span><span class="sxs-lookup"><span data-stu-id="de20a-127">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="de20a-128">Skapa en tabell i koden</span><span class="sxs-lookup"><span data-stu-id="de20a-128">Create a table in code</span></span>
<span data-ttu-id="de20a-129">toocreate hello Azure-tabellen, Lägg till ett anrop för**CreateIfNotExistsAsync** toohello när du har fått en **CloudTable** objekt enligt beskrivningen i avsnittet för hello ”åtkomst tabeller i kod”.</span><span class="sxs-lookup"><span data-stu-id="de20a-129">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync** toohello after you get a **CloudTable** object as described in hello "Access tables in code" section.</span></span>

    // Create hello CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="de20a-130">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="de20a-130">Add an entity tooa table</span></span>
<span data-ttu-id="de20a-131">tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="de20a-131">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="de20a-132">hello följande kod definierar en entitetsklass kallas **CustomerEntity** som använder hello kundens förnamn som radnyckel hello och hello efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="de20a-132">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and hello last name as hello partition key.</span></span>

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

<span data-ttu-id="de20a-133">Tabellåtgärder som rör entiteter utförs med hjälp av hello **CloudTable** objekt som du skapade tidigare i ”åtkomst tabeller i kod”.</span><span class="sxs-lookup"><span data-stu-id="de20a-133">Table operations involving entities are done using hello **CloudTable** object that you created earlier in "Access tables in code."</span></span> <span data-ttu-id="de20a-134">Hej **TableOperation** -objektet representerar hello åtgärden toobe är klar.</span><span class="sxs-lookup"><span data-stu-id="de20a-134">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="de20a-135">Hej följande exempel visas hur toocreate en **CloudTable** objekt och en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="de20a-135">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="de20a-136">tooprepare hello åtgärden en **TableOperation** skapas tooinsert hello kundentiteten i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="de20a-136">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="de20a-137">Slutligen hello-åtgärden körs genom att anropa **CloudTable.ExecuteAsync**.</span><span class="sxs-lookup"><span data-stu-id="de20a-137">Finally, hello operation is executed by calling **CloudTable.ExecuteAsync**.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="de20a-138">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="de20a-138">Insert a batch of entities</span></span>
<span data-ttu-id="de20a-139">Du kan infoga flera entiteter i en tabell i en enda skrivning.</span><span class="sxs-lookup"><span data-stu-id="de20a-139">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="de20a-140">hello följande kodexempel skapar två entitetsobjekt (”Jeff Smith” och ”Ben Smith”), lägger till dem tooa **TableBatchOperation** objekt med hello infogning och sedan startar hello igen genom att anropa  **CloudTable.ExecuteBatchAsync**.</span><span class="sxs-lookup"><span data-stu-id="de20a-140">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello Insert method, and then starts hello operation by calling **CloudTable.ExecuteBatchAsync**.</span></span>

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

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="de20a-141">Hämta alla hello entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="de20a-141">Get all of hello entities in a partition</span></span>
<span data-ttu-id="de20a-142">tooquery en tabell för alla hello entiteter i en partition, Använd en **TableQuery** objekt.</span><span class="sxs-lookup"><span data-stu-id="de20a-142">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="de20a-143">hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="de20a-143">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="de20a-144">Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="de20a-144">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a><span data-ttu-id="de20a-145">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="de20a-145">Get a single entity</span></span>
<span data-ttu-id="de20a-146">Du kan skriva en fråga tooget en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="de20a-146">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="de20a-147">hello följande kod används en **TableOperation** objekt toospecify en kund som heter ”Ben Smith”.</span><span class="sxs-lookup"><span data-stu-id="de20a-147">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="de20a-148">Den här metoden returnerar endast en entitet i stället för en samling och hello returneras värdet i **TableResult.Result** är en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="de20a-148">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="de20a-149">Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från hello **tabell** service.</span><span class="sxs-lookup"><span data-stu-id="de20a-149">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="de20a-150">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="de20a-150">Delete an entity</span></span>
<span data-ttu-id="de20a-151">Du kan ta bort en enhet när du har hittat.</span><span class="sxs-lookup"><span data-stu-id="de20a-151">You can delete an entity after you find it.</span></span> <span data-ttu-id="de20a-152">hello följande kod söker efter en kundentitet med namnet ”Ben Smith”, och om den hittar det tar bort den.</span><span class="sxs-lookup"><span data-stu-id="de20a-152">hello following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="de20a-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de20a-153">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

