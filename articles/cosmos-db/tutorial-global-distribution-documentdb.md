---
title: "Azure DB Cosmos global distributionsplatsen självstudier för DocumentDB API | Microsoft Docs"
description: "Lär dig hur du ställer in Azure Cosmos DB global distributionsplatsen med hjälp av DocumentDB-API."
services: cosmos-db
keywords: Global distributionsplatsen, documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: f4d8efe9814bd28bb902567a23b541bc9b5414a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-documentdb-api"></a><span data-ttu-id="0156f-104">Hur du konfigurerar Azure Cosmos DB global distributionsplatsen med hjälp av DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="0156f-104">How to setup Azure Cosmos DB global distribution using the DocumentDB API</span></span>

<span data-ttu-id="0156f-105">I den här artikeln visar vi hur du använder Azure-portalen för att konfigurera Azure Cosmos DB global distributionsplatsen och ansluter sedan med hjälp av DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="0156f-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the DocumentDB API.</span></span>

<span data-ttu-id="0156f-106">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="0156f-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="0156f-107">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0156f-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="0156f-108">Konfigurera distributionslistor med hjälp av den [DocumentDB APIs](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="0156f-108">Configure global distribution using the [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-documentdb-api"></a><span data-ttu-id="0156f-109">Ansluter till en önskad region med DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="0156f-109">Connecting to a preferred region using the DocumentDB API</span></span>

<span data-ttu-id="0156f-110">För att kunna dra nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange beställda inställningar listan över regioner som används för att utföra åtgärder för dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0156f-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="0156f-111">Detta kan göras genom att ställa in principen.</span><span class="sxs-lookup"><span data-stu-id="0156f-111">This can be done by setting the connection policy.</span></span> <span data-ttu-id="0156f-112">Baserat på konfigurationen av Azure DB som Cosmos-kontot, aktuella regional tillgänglighet och inställningar listan som anges, kommer den mest optimala slutpunkten väljas av DocumentDB SDK, för att utföra skrivåtgärder och läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0156f-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the DocumentDB SDK to perform write and read operations.</span></span>

<span data-ttu-id="0156f-113">Den här inställningen listan har angetts när du initierar en anslutning med DocumentDB-SDK.</span><span class="sxs-lookup"><span data-stu-id="0156f-113">This preference list is specified when initializing a connection using the DocumentDB SDKs.</span></span> <span data-ttu-id="0156f-114">SDK: erna acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="0156f-114">The SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="0156f-115">SDK skickar automatiskt alla skrivningar till aktuellt skriva region.</span><span class="sxs-lookup"><span data-stu-id="0156f-115">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="0156f-116">Alla Läs kommer att skickas till den första tillgängliga regionen i listan över PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0156f-116">All reads will be sent to the first available region in the PreferredLocations list.</span></span> <span data-ttu-id="0156f-117">Om begäran inte klienten misslyckas nedåt i listan till nästa region, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0156f-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="0156f-118">SDK: erna görs endast försök att läsa från de regioner som anges i PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0156f-118">The SDKs will only attempt to read from the regions specified in PreferredLocations.</span></span> <span data-ttu-id="0156f-119">Så, till exempel om det konto som är tillgänglig i tre regioner, men klienten endast anger två av regionerna som icke-och skrivbehörighet för PreferredLocations, sedan utan läsning hanteras utanför området skrivåtgärder även vid redundans.</span><span class="sxs-lookup"><span data-stu-id="0156f-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for PreferredLocations, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="0156f-120">Programmet kan verifiera den aktuella write-slutpunkten och läsa slutpunkt som valts av SDK genom att kontrollera två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="0156f-120">The application can verify the current write endpoint and read endpoint chosen by the SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="0156f-121">Om egenskapen PreferredLocations inte har angetts hanteras alla förfrågningar från det aktuella området för skrivning.</span><span class="sxs-lookup"><span data-stu-id="0156f-121">If the PreferredLocations property is not set, all requests will be served from the current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="0156f-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="0156f-122">.NET SDK</span></span>
<span data-ttu-id="0156f-123">SDK kan användas utan någon kodändringar.</span><span class="sxs-lookup"><span data-stu-id="0156f-123">The SDK can be used without any code changes.</span></span> <span data-ttu-id="0156f-124">I det här fallet SDK automatiskt styr både läs och skriver till det aktuella området för skrivning.</span><span class="sxs-lookup"><span data-stu-id="0156f-124">In this case, the SDK automatically directs both reads and writes to the current write region.</span></span>

<span data-ttu-id="0156f-125">Parametern ConnectionPolicy för konstruktorn DocumentClient har en egenskap som kallas Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations i version 1,8 och senare av .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="0156f-125">In version 1.8 and later of the .NET SDK, the ConnectionPolicy parameter for the DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="0156f-126">Den här egenskapen är av typen samling `<string>` och bör innehålla en lista över regionnamn.</span><span class="sxs-lookup"><span data-stu-id="0156f-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="0156f-127">Strängvärden formateras per Region namnkolumnen på den [Azure-regioner] [ regions] sida, utan blanksteg före eller efter först och sista tecknet respektive.</span><span class="sxs-lookup"><span data-stu-id="0156f-127">The string values are formatted per the Region Name column on the [Azure Regions][regions] page, with no spaces before or after the first and last character respectively.</span></span>

<span data-ttu-id="0156f-128">Aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.WriteEndpoint och DocumentClient.ReadEndpoint respektive.</span><span class="sxs-lookup"><span data-stu-id="0156f-128">The current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="0156f-129">URL: er för slutpunkterna ska inte betraktas som långlivade konstanter.</span><span class="sxs-lookup"><span data-stu-id="0156f-129">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="0156f-130">Tjänsten kan uppdatera dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="0156f-130">The service may update these at any point.</span></span> <span data-ttu-id="0156f-131">SDK hanterar automatiskt den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="0156f-131">The SDK handles this change automatically.</span></span>
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="0156f-132">NodeJS, JavaScript och Python SDK</span><span class="sxs-lookup"><span data-stu-id="0156f-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="0156f-133">SDK kan användas utan någon kodändringar.</span><span class="sxs-lookup"><span data-stu-id="0156f-133">The SDK can be used without any code changes.</span></span> <span data-ttu-id="0156f-134">I det här fallet SDK kommer automatiskt att dirigera både läsningar och skrivningar till aktuellt skriva region.</span><span class="sxs-lookup"><span data-stu-id="0156f-134">In this case, the SDK will automatically direct both reads and writes to the current write region.</span></span>

<span data-ttu-id="0156f-135">I version 1,8 och senare av varje SDK parametern ConnectionPolicy för konstruktorn DocumentClient en ny egenskap kallas DocumentClient.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0156f-135">In version 1.8 and later of each SDK, the ConnectionPolicy parameter for the DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="0156f-136">Detta är parametern är en matris med strängar som tar en lista över regionnamn.</span><span class="sxs-lookup"><span data-stu-id="0156f-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="0156f-137">Namnen är formaterade per Region namnkolumnen i den [Azure-regioner] [ regions] sidan.</span><span class="sxs-lookup"><span data-stu-id="0156f-137">The names are formatted per the Region Name column in the [Azure Regions][regions] page.</span></span> <span data-ttu-id="0156f-138">Du kan också använda fördefinierade konstanter i informationssyfte objektet AzureDocuments.Regions</span><span class="sxs-lookup"><span data-stu-id="0156f-138">You can also use the predefined constants in the convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="0156f-139">Aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.getWriteEndpoint och DocumentClient.getReadEndpoint respektive.</span><span class="sxs-lookup"><span data-stu-id="0156f-139">The current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="0156f-140">URL: er för slutpunkterna ska inte betraktas som långlivade konstanter.</span><span class="sxs-lookup"><span data-stu-id="0156f-140">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="0156f-141">Tjänsten kan uppdatera dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="0156f-141">The service may update these at any point.</span></span> <span data-ttu-id="0156f-142">Den här ändringen ska hanteras automatiskt av SDK.</span><span class="sxs-lookup"><span data-stu-id="0156f-142">The SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="0156f-143">Nedan visas ett kodexempel för NodeJS eller Javascript.</span><span class="sxs-lookup"><span data-stu-id="0156f-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="0156f-144">Python och Java följer samma mönster.</span><span class="sxs-lookup"><span data-stu-id="0156f-144">Python and Java will follow the same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="0156f-145">REST</span><span class="sxs-lookup"><span data-stu-id="0156f-145">REST</span></span>
<span data-ttu-id="0156f-146">När ett databaskonto har gjorts i flera områden, kan klienterna fråga dess tillgänglighet genom att utföra en GET-begäran för följande URI.</span><span class="sxs-lookup"><span data-stu-id="0156f-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on the following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="0156f-147">Tjänsten returneras en lista över regioner och deras motsvarande Azure DB som Cosmos-slutpunkt URI: er för replikerna.</span><span class="sxs-lookup"><span data-stu-id="0156f-147">The service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for the replicas.</span></span> <span data-ttu-id="0156f-148">Det aktuella området skrivåtgärder kommer att visas i svaret.</span><span class="sxs-lookup"><span data-stu-id="0156f-148">The current write region will be indicated in the response.</span></span> <span data-ttu-id="0156f-149">Klienten kan sedan välja lämpliga slutpunkten för alla ytterligare REST API-begäranden på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="0156f-149">The client can then select the appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="0156f-150">Exempelsvar</span><span class="sxs-lookup"><span data-stu-id="0156f-150">Example response</span></span>

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* <span data-ttu-id="0156f-151">PUT, POST och DELETE-begäranden måste gå till den angivna skrivningen URI</span><span class="sxs-lookup"><span data-stu-id="0156f-151">All PUT, POST and DELETE requests must go to the indicated write URI</span></span>
* <span data-ttu-id="0156f-152">Alla hämtar och andra skrivskyddade förfrågningar (till exempel frågor) kan gå till en slutpunkt av klientens val</span><span class="sxs-lookup"><span data-stu-id="0156f-152">All GETs and other read-only requests (for example queries) may go to any endpoint of the client’s choice</span></span>

<span data-ttu-id="0156f-153">Skriva begäranden till skrivskyddad regioner misslyckas med felkoden för HTTP 403 (”förbjuden”).</span><span class="sxs-lookup"><span data-stu-id="0156f-153">Write requests to read-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="0156f-154">Om skrivning region ändras efter klientens inledande identifieringen fas, misslyckas efterföljande skrivningar till den föregående skrivåtgärder regionen med HTTP-felkod 403 (”förbjuden”).</span><span class="sxs-lookup"><span data-stu-id="0156f-154">If the write region changes after the client’s initial discovery phase, subsequent writes to the previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="0156f-155">Klienten bör sedan få listan över regioner igen för att hämta den uppdaterade write-regionen.</span><span class="sxs-lookup"><span data-stu-id="0156f-155">The client should then GET the list of regions again to get the updated write region.</span></span>

<span data-ttu-id="0156f-156">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0156f-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="0156f-157">Du kan lära dig hur du hanterar konsekvensen för globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="0156f-157">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="0156f-158">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="0156f-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0156f-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0156f-159">Next steps</span></span>

<span data-ttu-id="0156f-160">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="0156f-160">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0156f-161">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0156f-161">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="0156f-162">Konfigurera globala distribution via DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="0156f-162">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="0156f-163">Du kan nu fortsätta till nästa kurs att lära dig hur du utvecklar lokalt med hjälp av lokala Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="0156f-163">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0156f-164">Utveckla lokalt med emulatorn</span><span class="sxs-lookup"><span data-stu-id="0156f-164">Develop locally with the emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

