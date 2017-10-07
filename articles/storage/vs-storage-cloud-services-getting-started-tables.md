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
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Komma igång med Azure-tabellagring och Visual Studio anslutna tjänster (cloud services-projekt)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooget igång med Azure-tabellagring i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i cloud services-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan . Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.

hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data. hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.

tooget igång, måste du först toocreate en tabell i ditt lagringskonto. Vi lära dig hur toocreate en Azure table i koden och även hur tooperform Standardtabell och entiteten åtgärder, till exempel lägga till, ändra, läsa och läsa tabell entiteter. hello prover är skrivna i C\# code och använda hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Obs:** vissa hello API: er som utför anrop ut tooAzure lagring är asynkron. Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information. hello koden nedan förutsätter asynkrona programming metoder som används.

* Se [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md) mer information om programmässigt arbete i tabeller.
* Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.
* Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.
* Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.

## <a name="access-tables-in-code"></a>Åtkomst till tabeller i koden
tooaccess tabeller i molntjänstprojekt, behöver du tooinclude hello följande objekt tooany C#-källfiler som åt Azure table storage.

1. Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration hello.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > Använda alla hello ovan kod framför hello koden i hello följande exempel.
   > 
   > 
3. Hämta en **CloudTableClient** objekt tooreference hello tabellobjekt i ditt lagringskonto.
   
         // Create hello table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Hämta en **CloudTable** refererar till objektet tooreference en viss tabell och enheter.
   
        // Get a reference tooa table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Skapa en tabell i koden
toocreate hello Azure-tabellen, Lägg till ett anrop för**CreateIfNotExistsAsync** toohello när du har fått en **CloudTable** objekt enligt beskrivningen i avsnittet för hello ”åtkomst tabeller i kod”.

    // Create hello CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten. hello följande kod definierar en entitetsklass kallas **CustomerEntity** som använder hello kundens förnamn som radnyckel hello och hello efternamn som partitionsnyckel hello.

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

Tabellåtgärder som rör entiteter utförs med hjälp av hello **CloudTable** objekt som du skapade tidigare i ”åtkomst tabeller i kod”. Hej **TableOperation** -objektet representerar hello åtgärden toobe är klar. Hej följande exempel visas hur toocreate en **CloudTable** objekt och en **CustomerEntity** objekt. tooprepare hello åtgärden en **TableOperation** skapas tooinsert hello kundentiteten i hello-tabellen. Slutligen hello-åtgärden körs genom att anropa **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Infoga en batch med entiteter
Du kan infoga flera entiteter i en tabell i en enda skrivning. hello följande kodexempel skapar två entitetsobjekt (”Jeff Smith” och ”Ben Smith”), lägger till dem tooa **TableBatchOperation** objekt med hello infogning och sedan startar hello igen genom att anropa  **CloudTable.ExecuteBatchAsync**.

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

## <a name="get-all-of-hello-entities-in-a-partition"></a>Hämta alla hello entiteter i en partition
tooquery en tabell för alla hello entiteter i en partition, Använd en **TableQuery** objekt. hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”. Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.

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


## <a name="get-a-single-entity"></a>Hämta en enda entitet
Du kan skriva en fråga tooget en enda, specifik entitet. hello följande kod används en **TableOperation** objekt toospecify en kund som heter ”Ben Smith”. Den här metoden returnerar endast en entitet i stället för en samling och hello returneras värdet i **TableResult.Result** är en **CustomerEntity** objekt. Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från hello **tabell** service.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan ta bort en enhet när du har hittat. hello följande kod söker efter en kundentitet med namnet ”Ben Smith”, och om den hittar det tar bort den.

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

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

