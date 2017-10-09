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
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Hur tooget igång med Azure Table storage och Visual Studio anslutna tjänster
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur få igång med Azure Table storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan.

hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data. hello-tjänsten är en NoSQL-databas som accepterar autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.

Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.

Mer allmän information om hur du använder Azure Table storage finns [komma igång med Azure Table storage med hjälp av .NET](../storage/storage-dotnet-how-to-use-tables.md).

tooget igång, måste du först toocreate en tabell i ditt lagringskonto. Vi lära dig hur toocreate en Azure tabell i kod. Vi också lära dig hur tooperform Standardtabell och entiteten åtgärder, till exempel lägga till, ändra, läsa och läsa tabell entiteter. hello prover är skrivna i C\# code och använda hello Azure Storage-klientbibliotek för .NET.

**Obs** -vissa hello API: er som utför anrop ut tooAzure lagring i ASP.NET Core är asynkron. Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information. hello koden nedan förutsätter asynkrona programming metoder som används.

## <a name="access-tables-in-code"></a>Åtkomst till tabeller i koden
tooaccess tabeller i ASP.NET Core projekt, behöver du tooinclude hello följande objekt tooany C#-källfiler som åt Azure table storage.

1. Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Obs** -använda alla hello ovan kod framför hello koden i hello följande exempel.
3. Hämta en **CloudTableClient** objekt tooreference hello tabellobjekt i ditt lagringskonto.  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Hämta en **CloudTable** refererar till objektet tooreference en viss tabell och enheter.
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Skapa en tabell i koden
toocreate hello Azure-tabellen, Lägg till ett anrop för**CreateIfNotExistsAsync()**.

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet tooa tabell som du skapar en klass som definierar hello egenskaperna för entiteten. hello följande kod definierar en entitetsklass kallas **CustomerEntity** som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.

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

Tabellåtgärder som rör entiteter utförs med hjälp av hello **CloudTable** objekt du skapade tidigare i ”åtkomst tabeller i kod”. Hej **TableOperation** -objektet representerar hello åtgärden toobe är klar. Hej följande exempel visas hur toocreate en **CloudTable** objekt och en **CustomerEntity** objekt. tooprepare hello åtgärden en **TableOperation** skapas tooinsert hello kundentiteten i hello-tabellen. Slutligen körs åtgärden hello genom att anropa CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Infoga en batch med entiteter
Du kan infoga flera entiteter i en tabell i en enda skrivning. hello följande kodexempel skapar två entitetsobjekt (”Jeff Smith” och ”Ben Smith”), lägger till dem tooa **TableBatchOperation** objekt med hello **infoga** metoden och sedan startar hello åtgärden genom att anropar CloudTable.ExecuteBatchAsync.

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
Du kan ta bort en enhet när du har hittat. hello följande kod söker efter en kundentitet med namnet ”Ben Smith” och om den hittar det tar bort den.

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

