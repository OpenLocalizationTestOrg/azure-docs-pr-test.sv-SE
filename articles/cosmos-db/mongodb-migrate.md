---
title: "aaaUse mongoimport och mongorestore med hello Azure Cosmos DB API för MongoDB | Microsoft Docs"
description: "Lär dig hur toouse mongoimport och mongorestore tooimport data tooan API för MongoDB-konto"
keywords: mongoimport mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 921354bc7b09a076a73e0cbf5e4aabcc9e83d5a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a><span data-ttu-id="156fd-104">Azure Cosmos DB: Importera MongoDB-data</span><span class="sxs-lookup"><span data-stu-id="156fd-104">Azure Cosmos DB: Import MongoDB data</span></span> 

<span data-ttu-id="156fd-105">toomigrate data från MongoDB tooan Azure DB som Cosmos-konto för användning av hello API för MongoDB, måste du:</span><span class="sxs-lookup"><span data-stu-id="156fd-105">toomigrate data from MongoDB tooan Azure Cosmos DB account for use with hello API for MongoDB, you must:</span></span>

* <span data-ttu-id="156fd-106">Hämta antingen *mongoimport.exe* eller *mongorestore.exe* från hello [MongoDB Download Center](https://www.mongodb.com/download-center).</span><span class="sxs-lookup"><span data-stu-id="156fd-106">Download either *mongoimport.exe* or *mongorestore.exe* from hello [MongoDB Download Center](https://www.mongodb.com/download-center).</span></span>
* <span data-ttu-id="156fd-107">Hämta din [API: et för MongoDB-anslutningssträng](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="156fd-107">Get your [API for MongoDB connection string](connect-mongodb-account.md).</span></span>

<span data-ttu-id="156fd-108">Om du importerar data från MongoDB och planera toouse med hello Azure Cosmos DB bör du använda hello [datamigreringsverktyget](import-data.md) tooimport data.</span><span class="sxs-lookup"><span data-stu-id="156fd-108">If you are importing data from MongoDB and plan toouse it with hello Azure Cosmos DB, you should use hello [Data Migration tool](import-data.md) tooimport data.</span></span>

<span data-ttu-id="156fd-109">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="156fd-109">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="156fd-110">Hämta din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="156fd-110">Retrieving your connection string</span></span>
> * <span data-ttu-id="156fd-111">Importera MongoDB-data med hjälp av mongoimport</span><span class="sxs-lookup"><span data-stu-id="156fd-111">Importing MongoDB data by using mongoimport</span></span>
> * <span data-ttu-id="156fd-112">Importera MongoDB-data med hjälp av mongorestore</span><span class="sxs-lookup"><span data-stu-id="156fd-112">Importing MongoDB data by using mongorestore</span></span>

## <a name="prerequisites"></a><span data-ttu-id="156fd-113">Krav</span><span class="sxs-lookup"><span data-stu-id="156fd-113">Prerequisites</span></span>

* <span data-ttu-id="156fd-114">Öka genomflödet: hello varaktighet för din datamigrering beror på hello mängden genomströmning som du angett för samlingar.</span><span class="sxs-lookup"><span data-stu-id="156fd-114">Increase throughput: hello duration of your data migration depends on hello amount of throughput you set up for your collections.</span></span> <span data-ttu-id="156fd-115">Vara säker på att tooincrease hello genomströmning för större migrering av data.</span><span class="sxs-lookup"><span data-stu-id="156fd-115">Be sure tooincrease hello throughput for larger data migrations.</span></span> <span data-ttu-id="156fd-116">När du har slutfört hello migreringen, minska hello genomströmning toosave kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="156fd-116">After you've completed hello migration, decrease hello throughput toosave costs.</span></span> <span data-ttu-id="156fd-117">Mer information om ökar genomströmningen i hello [Azure-portalen](https://portal.azure.com), se [prestandanivåer och prisnivåerna i Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="156fd-117">For more information about increasing throughput in hello [Azure portal](https://portal.azure.com), see [Performance levels and pricing tiers in Azure Cosmos DB](performance-levels.md).</span></span>

* <span data-ttu-id="156fd-118">Aktivera SSL: Azure Cosmos-DB har stränga säkerhetskrav och standarder.</span><span class="sxs-lookup"><span data-stu-id="156fd-118">Enable SSL: Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="156fd-119">Vara säker på att tooenable SSL när du kommunicerar med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="156fd-119">Be sure tooenable SSL when you interact with your account.</span></span> <span data-ttu-id="156fd-120">hello procedurer i hello resten av hello artikel innefattar hur tooenable SSL för mongoimport och mongorestore.</span><span class="sxs-lookup"><span data-stu-id="156fd-120">hello procedures in hello rest of hello article include how tooenable SSL for mongoimport and mongorestore.</span></span>

## <a name="find-your-connection-string-information-host-port-username-and-password"></a><span data-ttu-id="156fd-121">Hitta din Anslutningssträngsinformation (värd, port, användarnamn och lösenord)</span><span class="sxs-lookup"><span data-stu-id="156fd-121">Find your connection string information (host, port, username, and password)</span></span>

1. <span data-ttu-id="156fd-122">I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Cosmos DB** post.</span><span class="sxs-lookup"><span data-stu-id="156fd-122">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Cosmos DB** entry.</span></span>
2. <span data-ttu-id="156fd-123">I hello **prenumerationer** rutan, Välj namnet på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="156fd-123">In hello **Subscriptions** pane, select your account name.</span></span>
3. <span data-ttu-id="156fd-124">I hello **anslutningssträngen** bladet, klickar du på **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="156fd-124">In hello **Connection String** blade, click **Connection String**.</span></span>  
<span data-ttu-id="156fd-125">hello högra fönstret innehåller alla hello information du behöver toosuccessfully ansluta tooyour-konto.</span><span class="sxs-lookup"><span data-stu-id="156fd-125">hello right pane contains all hello information that you need toosuccessfully connect tooyour account.</span></span>

    ![Anslutningen sträng bladet](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a><span data-ttu-id="156fd-127">Importera data toohello API: et för MongoDB med mongoimport</span><span class="sxs-lookup"><span data-stu-id="156fd-127">Import data toohello API for MongoDB by using mongoimport</span></span>

<span data-ttu-id="156fd-128">tooimport data tooyour Azure DB som Cosmos-konto, använder hello följande mall.</span><span class="sxs-lookup"><span data-stu-id="156fd-128">tooimport data tooyour Azure Cosmos DB account, use hello following template.</span></span> <span data-ttu-id="156fd-129">Fyll i *värden*, *användarnamn*, och *lösenord* med hello-värden som är specifika tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="156fd-129">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>  

<span data-ttu-id="156fd-130">Mall:</span><span class="sxs-lookup"><span data-stu-id="156fd-130">Template:</span></span>

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

<span data-ttu-id="156fd-131">Exempel:</span><span class="sxs-lookup"><span data-stu-id="156fd-131">Example:</span></span>  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a><span data-ttu-id="156fd-132">Importera data toohello API: et för MongoDB med mongorestore</span><span class="sxs-lookup"><span data-stu-id="156fd-132">Import data toohello API for MongoDB by using mongorestore</span></span>

<span data-ttu-id="156fd-133">toorestore data tooyour API: et för MongoDB-konto, använder hello följande mall tooexecute hello import.</span><span class="sxs-lookup"><span data-stu-id="156fd-133">toorestore data tooyour API for MongoDB account, use hello following template tooexecute hello import.</span></span> <span data-ttu-id="156fd-134">Fyll i *värden*, *användarnamn*, och *lösenord* med hello-värden som är specifika tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="156fd-134">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>

<span data-ttu-id="156fd-135">Mall:</span><span class="sxs-lookup"><span data-stu-id="156fd-135">Template:</span></span>

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

<span data-ttu-id="156fd-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="156fd-136">Example:</span></span>

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a><span data-ttu-id="156fd-137">Guide för en lyckad migrering</span><span class="sxs-lookup"><span data-stu-id="156fd-137">Guide for a successful migration</span></span>

1. <span data-ttu-id="156fd-138">Skapa och skala dina samlingar:</span><span class="sxs-lookup"><span data-stu-id="156fd-138">Pre-create and scale your collections:</span></span>
        
    * <span data-ttu-id="156fd-139">Standard etablerar Azure Cosmos DB en ny MongoDB-samling med 1 000 frågeenheter (RUs).</span><span class="sxs-lookup"><span data-stu-id="156fd-139">By default, Azure Cosmos DB provisions a new MongoDB collection with 1,000 request units (RUs).</span></span> <span data-ttu-id="156fd-140">Innan du börjar hello migrering med hjälp av mongoimport, mongorestore eller mongomirror skapa alla samlingar från hello [Azure-portalen](https://portal.azure.com) eller från MongoDB-drivrutiner och verktyg.</span><span class="sxs-lookup"><span data-stu-id="156fd-140">Before you start hello migration by using mongoimport, mongorestore, or mongomirror, pre-create all your collections from hello [Azure portal](https://portal.azure.com) or from MongoDB drivers and tools.</span></span> <span data-ttu-id="156fd-141">Om samlingen är större än 10 GB, kontrollerar du att toocreate en [delat/partitionerad samling](partition-data.md) med en lämplig Fragmentera nyckel.</span><span class="sxs-lookup"><span data-stu-id="156fd-141">If your collection is greater than 10 GB, make sure toocreate a [sharded/partitioned collection](partition-data.md) with an appropriate shard key.</span></span>

    * <span data-ttu-id="156fd-142">Från hello [Azure-portalen](https://portal.azure.com), öka genomflödet av dina samlingar från 1 000 RUs för en enskild partition samling och 2 500 RUs för en delat samling för hello migrering.</span><span class="sxs-lookup"><span data-stu-id="156fd-142">From hello [Azure portal](https://portal.azure.com), increase your collections' throughput from 1,000 RUs for a single partition collection and 2,500 RUs for a sharded collection just for hello migration.</span></span> <span data-ttu-id="156fd-143">Med hello högre genomströmning, kan du undvika begränsning och migrera på kortare tid.</span><span class="sxs-lookup"><span data-stu-id="156fd-143">With hello higher throughput, you can avoid throttling and migrate in less time.</span></span> <span data-ttu-id="156fd-144">Med varje timme fakturering i Azure Cosmos-databasen, kan du minska hello genomströmning omedelbart efter hello migrering toosave kostnader.</span><span class="sxs-lookup"><span data-stu-id="156fd-144">With hourly billing in Azure Cosmos DB, you can reduce hello throughput immediately after hello migration toosave costs.</span></span>

2. <span data-ttu-id="156fd-145">Beräkna hello ungefärliga RU kostnad för att skriva ett enskilt dokument:</span><span class="sxs-lookup"><span data-stu-id="156fd-145">Calculate hello approximate RU charge for a single document write:</span></span>

    <span data-ttu-id="156fd-146">a.</span><span class="sxs-lookup"><span data-stu-id="156fd-146">a.</span></span> <span data-ttu-id="156fd-147">Ansluta tooyour Azure Cosmos DB MongoDB-databas från hello MongoDB-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="156fd-147">Connect tooyour Azure Cosmos DB MongoDB database from hello MongoDB Shell.</span></span> <span data-ttu-id="156fd-148">Du hittar anvisningar i [ansluta en MongoDB programmet tooAzure Cosmos DB](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="156fd-148">You can find instructions in [Connect a MongoDB application tooAzure Cosmos DB](connect-mongodb-account.md).</span></span>
    
    <span data-ttu-id="156fd-149">b.</span><span class="sxs-lookup"><span data-stu-id="156fd-149">b.</span></span> <span data-ttu-id="156fd-150">Kör ett Exempelkommando för insert genom att använda ett av dokumenten exempel hello MongoDB Shell:</span><span class="sxs-lookup"><span data-stu-id="156fd-150">Run a sample insert command by using one of your sample documents from hello MongoDB Shell:</span></span>
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    <span data-ttu-id="156fd-151">c.</span><span class="sxs-lookup"><span data-stu-id="156fd-151">c.</span></span> <span data-ttu-id="156fd-152">Kör ```db.runCommand({getLastRequestStatistics: 1})``` och du får ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="156fd-152">Run ```db.runCommand({getLastRequestStatistics: 1})``` and you will receive a response like this one:</span></span>
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    <span data-ttu-id="156fd-153">d.</span><span class="sxs-lookup"><span data-stu-id="156fd-153">d.</span></span> <span data-ttu-id="156fd-154">Anteckna hello begäran kostnad.</span><span class="sxs-lookup"><span data-stu-id="156fd-154">Take note of hello request charge.</span></span>
    
3. <span data-ttu-id="156fd-155">Avgör hello svarstid från din dator toohello Azure Cosmos DB Molntjänsten:</span><span class="sxs-lookup"><span data-stu-id="156fd-155">Determine hello latency from your machine toohello Azure Cosmos DB cloud service:</span></span>
    
    <span data-ttu-id="156fd-156">a.</span><span class="sxs-lookup"><span data-stu-id="156fd-156">a.</span></span> <span data-ttu-id="156fd-157">Aktivera utförlig loggning från hello MongoDB-gränssnittet med hjälp av det här kommandot:```setVerboseShell(true)```</span><span class="sxs-lookup"><span data-stu-id="156fd-157">Enable verbose logging from hello MongoDB Shell by using this command: ```setVerboseShell(true)```</span></span>
    
    <span data-ttu-id="156fd-158">b.</span><span class="sxs-lookup"><span data-stu-id="156fd-158">b.</span></span> <span data-ttu-id="156fd-159">Köra en enkel fråga mot databasen hello: ```db.coll.find().limit(1)```.</span><span class="sxs-lookup"><span data-stu-id="156fd-159">Run a simple query against hello database: ```db.coll.find().limit(1)```.</span></span> <span data-ttu-id="156fd-160">Du får ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="156fd-160">You will receive a response like this one:</span></span>

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. <span data-ttu-id="156fd-161">Ta bort hello infogas dokument innan hello migrering tooensure att det inte finns några dubbletter dokument.</span><span class="sxs-lookup"><span data-stu-id="156fd-161">Remove hello inserted document before hello migration tooensure that there are no duplicate documents.</span></span> <span data-ttu-id="156fd-162">Du kan ta bort dokument med hjälp av det här kommandot:```db.coll.remove({})```</span><span class="sxs-lookup"><span data-stu-id="156fd-162">You can remove documents by using this command: ```db.coll.remove({})```</span></span>

5. <span data-ttu-id="156fd-163">Beräkna hello ungefärliga *batchSize* och *numInsertionWorkers* värden:</span><span class="sxs-lookup"><span data-stu-id="156fd-163">Calculate hello approximate *batchSize* and *numInsertionWorkers* values:</span></span>

    * <span data-ttu-id="156fd-164">För *batchSize*, division hello totalt etablerats RUs av hello RUs förbrukad från dina dokument Skriv i steg 3.</span><span class="sxs-lookup"><span data-stu-id="156fd-164">For *batchSize*, divide hello total provisioned RUs by hello RUs consumed from your single document write in step 3.</span></span>
    
    * <span data-ttu-id="156fd-165">Om hello beräknade *batchSize* < = 24, använder den som din *batchSize* värde.</span><span class="sxs-lookup"><span data-stu-id="156fd-165">If hello calculated *batchSize* <= 24, use that number as your *batchSize* value.</span></span>
    
    * <span data-ttu-id="156fd-166">Om hello beräknade *batchSize* > 24, ange hello *batchSize* värdet too24.</span><span class="sxs-lookup"><span data-stu-id="156fd-166">If hello calculated *batchSize* > 24, set hello *batchSize* value too24.</span></span>
    
    * <span data-ttu-id="156fd-167">För *numInsertionWorkers*, Använd den här formeln: *numInsertionWorkers = (dataflöde * fördröjning i sekunder) / (batchstorlek * används RUs för en enskild skrivning)*.</span><span class="sxs-lookup"><span data-stu-id="156fd-167">For *numInsertionWorkers*, use this equation:   *numInsertionWorkers =  (provisioned throughput * latency in seconds) / (batch size * consumed RUs for a single write)*.</span></span>
        
    |<span data-ttu-id="156fd-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="156fd-168">Property</span></span>|<span data-ttu-id="156fd-169">Värde</span><span class="sxs-lookup"><span data-stu-id="156fd-169">Value</span></span>|
    |--------|-----|
    |<span data-ttu-id="156fd-170">batchSize</span><span class="sxs-lookup"><span data-stu-id="156fd-170">batchSize</span></span>| <span data-ttu-id="156fd-171">24</span><span class="sxs-lookup"><span data-stu-id="156fd-171">24</span></span> |
    |<span data-ttu-id="156fd-172">RUs etablerats</span><span class="sxs-lookup"><span data-stu-id="156fd-172">RUs provisioned</span></span> | <span data-ttu-id="156fd-173">10000</span><span class="sxs-lookup"><span data-stu-id="156fd-173">10000</span></span> |
    |<span data-ttu-id="156fd-174">Svarstid</span><span class="sxs-lookup"><span data-stu-id="156fd-174">Latency</span></span> | <span data-ttu-id="156fd-175">0.100 s</span><span class="sxs-lookup"><span data-stu-id="156fd-175">0.100 s</span></span> |
    |<span data-ttu-id="156fd-176">RU debiteras för 1 doc-skrivning</span><span class="sxs-lookup"><span data-stu-id="156fd-176">RU charged for 1 doc write</span></span> | <span data-ttu-id="156fd-177">10 RUs</span><span class="sxs-lookup"><span data-stu-id="156fd-177">10 RUs</span></span> |
    |<span data-ttu-id="156fd-178">numInsertionWorkers</span><span class="sxs-lookup"><span data-stu-id="156fd-178">numInsertionWorkers</span></span> | <span data-ttu-id="156fd-179">?</span><span class="sxs-lookup"><span data-stu-id="156fd-179">?</span></span> |
    
    <span data-ttu-id="156fd-180">*numInsertionWorkers = (10000 RUs x 0,1 s) / (24 x 10 RUs) = 4.1666*</span><span class="sxs-lookup"><span data-stu-id="156fd-180">*numInsertionWorkers = (10000 RUs x 0.1 s) / (24 x 10 RUs) = 4.1666*</span></span>

6. <span data-ttu-id="156fd-181">Kör hello slutliga migrering kommando:</span><span class="sxs-lookup"><span data-stu-id="156fd-181">Run hello final migration command:</span></span>

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a><span data-ttu-id="156fd-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="156fd-182">Next steps</span></span>

<span data-ttu-id="156fd-183">Du kan fortsätta toohello nästa kurs och lära dig hur tooquery MongoDB-data med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="156fd-183">You can proceed toohello next tutorial and learn how tooquery MongoDB data by using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="156fd-184">Hur tooquery MongoDB data?</span><span class="sxs-lookup"><span data-stu-id="156fd-184">How tooquery MongoDB data?</span></span>](../cosmos-db/tutorial-query-mongodb.md)
