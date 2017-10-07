---
title: "aaaAzure Cosmos DB global distributionsplatsen självstudier för DocumentDB API | Microsoft Docs"
description: "Lär dig hur toosetup Azure Cosmos DB global distributionsplatsen med hello DocumentDB-API."
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
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a><span data-ttu-id="003d7-104">Hur toosetup Azure Cosmos DB global distributionsplatsen med hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="003d7-104">How toosetup Azure Cosmos DB global distribution using hello DocumentDB API</span></span>

<span data-ttu-id="003d7-105">I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="003d7-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello DocumentDB API.</span></span>

<span data-ttu-id="003d7-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="003d7-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="003d7-107">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="003d7-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="003d7-108">Konfigurera distributionslistor med hello [DocumentDB APIs](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="003d7-108">Configure global distribution using hello [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a><span data-ttu-id="003d7-109">Ansluta tooa önskad region med hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="003d7-109">Connecting tooa preferred region using hello DocumentDB API</span></span>

<span data-ttu-id="003d7-110">I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder.</span><span class="sxs-lookup"><span data-stu-id="003d7-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="003d7-111">Detta kan göras genom att ange hello anslutningsprincip.</span><span class="sxs-lookup"><span data-stu-id="003d7-111">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="003d7-112">Baserat på hello Azure Cosmos DB kontokonfigurationen anges regional tillgänglighet och hello inställningar lista hello de flesta optimala endpoint väljas av hello DocumentDB SDK tooperform skriva och läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="003d7-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello DocumentDB SDK tooperform write and read operations.</span></span>

<span data-ttu-id="003d7-113">Den här inställningen listan anges när initierar en anslutning med hello DocumentDB-SDK.</span><span class="sxs-lookup"><span data-stu-id="003d7-113">This preference list is specified when initializing a connection using hello DocumentDB SDKs.</span></span> <span data-ttu-id="003d7-114">hello SDK acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="003d7-114">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="003d7-115">hello SDK skickar automatiskt alla skrivningar toohello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="003d7-115">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="003d7-116">Alla Läs skickas toohello första tillgängliga region i hello PreferredLocations lista.</span><span class="sxs-lookup"><span data-stu-id="003d7-116">All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="003d7-117">Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="003d7-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="003d7-118">hello SDK försöker tooread från hello regioner som anges i PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="003d7-118">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="003d7-119">Så, till exempel om hello databaskonto finns tillgänglig i tre regioner, men hello klienten endast anger två hello-write regioner för PreferredLocations, sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.</span><span class="sxs-lookup"><span data-stu-id="003d7-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="003d7-120">hello-programmet kan verifiera hello aktuella skrivåtgärder slutpunkt och läsa slutpunkt som valts av hello SDK av kontrollerar två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="003d7-120">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="003d7-121">Om hello PreferredLocations egenskapen inte anges kommer alla begäranden hanteras från hello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="003d7-121">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="003d7-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="003d7-122">.NET SDK</span></span>
<span data-ttu-id="003d7-123">hello SDK kan användas utan någon kodändringar.</span><span class="sxs-lookup"><span data-stu-id="003d7-123">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="003d7-124">I det här fallet hello SDK automatiskt styr både läs och skriver toohello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="003d7-124">In this case, hello SDK automatically directs both reads and writes toohello current write region.</span></span>

<span data-ttu-id="003d7-125">I version 1,8 och senare av hello .NET SDK har hello ConnectionPolicy parametern för hello DocumentClient konstruktorn en egenskap som kallas Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="003d7-125">In version 1.8 and later of hello .NET SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="003d7-126">Den här egenskapen är av typen samling `<string>` och bör innehålla en lista över regionnamn.</span><span class="sxs-lookup"><span data-stu-id="003d7-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="003d7-127">hello strängvärden formateras per hello regionnamn kolumn på hello [Azure-regioner] [ regions] sida, utan blanksteg före eller efter hello första och sista tecknet respektive.</span><span class="sxs-lookup"><span data-stu-id="003d7-127">hello string values are formatted per hello Region Name column on hello [Azure Regions][regions] page, with no spaces before or after hello first and last character respectively.</span></span>

<span data-ttu-id="003d7-128">hello aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.WriteEndpoint och DocumentClient.ReadEndpoint respektive.</span><span class="sxs-lookup"><span data-stu-id="003d7-128">hello current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="003d7-129">hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter.</span><span class="sxs-lookup"><span data-stu-id="003d7-129">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="003d7-130">hello-tjänsten kan uppdatera dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="003d7-130">hello service may update these at any point.</span></span> <span data-ttu-id="003d7-131">hello SDK hanterar automatiskt den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="003d7-131">hello SDK handles this change automatically.</span></span>
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

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="003d7-132">NodeJS, JavaScript och Python SDK</span><span class="sxs-lookup"><span data-stu-id="003d7-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="003d7-133">hello SDK kan användas utan någon kodändringar.</span><span class="sxs-lookup"><span data-stu-id="003d7-133">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="003d7-134">I det här fallet kommer automatiskt att dirigera SDK hello både läser och skriver toohello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="003d7-134">In this case, hello SDK will automatically direct both reads and writes toohello current write region.</span></span>

<span data-ttu-id="003d7-135">I version 1,8 och senare av varje SDK hello ConnectionPolicy parameter för hello DocumentClient konstruktorn den nya egenskapen DocumentClient.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="003d7-135">In version 1.8 and later of each SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="003d7-136">Detta är parametern är en matris med strängar som tar en lista över regionnamn.</span><span class="sxs-lookup"><span data-stu-id="003d7-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="003d7-137">hello namn är formaterade per hello regionnamn kolumn i hello [Azure-regioner] [ regions] sidan.</span><span class="sxs-lookup"><span data-stu-id="003d7-137">hello names are formatted per hello Region Name column in hello [Azure Regions][regions] page.</span></span> <span data-ttu-id="003d7-138">Du kan också använda hello fördefinierade konstanter i hello bekvämlighet objektet AzureDocuments.Regions</span><span class="sxs-lookup"><span data-stu-id="003d7-138">You can also use hello predefined constants in hello convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="003d7-139">hello aktuella Skriv- och Läs slutpunkter är tillgängliga i DocumentClient.getWriteEndpoint och DocumentClient.getReadEndpoint respektive.</span><span class="sxs-lookup"><span data-stu-id="003d7-139">hello current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="003d7-140">hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter.</span><span class="sxs-lookup"><span data-stu-id="003d7-140">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="003d7-141">hello-tjänsten kan uppdatera dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="003d7-141">hello service may update these at any point.</span></span> <span data-ttu-id="003d7-142">hello SDK kommer att hantera den här ändringen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="003d7-142">hello SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="003d7-143">Nedan visas ett kodexempel för NodeJS eller Javascript.</span><span class="sxs-lookup"><span data-stu-id="003d7-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="003d7-144">Python och Java följer hello samma mönster.</span><span class="sxs-lookup"><span data-stu-id="003d7-144">Python and Java will follow hello same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="003d7-145">REST</span><span class="sxs-lookup"><span data-stu-id="003d7-145">REST</span></span>
<span data-ttu-id="003d7-146">När ett databaskonto har gjorts i flera områden, kan klienterna fråga dess tillgänglighet genom att utföra en GET-begäran på hello följande URI.</span><span class="sxs-lookup"><span data-stu-id="003d7-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on hello following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="003d7-147">hello-tjänsten returnerar en lista över regioner och deras motsvarande Azure DB som Cosmos-slutpunkt URI: er för hello repliker.</span><span class="sxs-lookup"><span data-stu-id="003d7-147">hello service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for hello replicas.</span></span> <span data-ttu-id="003d7-148">hello aktuella skrivåtgärder region visas hello svar.</span><span class="sxs-lookup"><span data-stu-id="003d7-148">hello current write region will be indicated in hello response.</span></span> <span data-ttu-id="003d7-149">hello-klienten kan sedan välja lämpliga hello-slutpunkt för alla ytterligare REST API-begäranden på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="003d7-149">hello client can then select hello appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="003d7-150">Exempelsvar</span><span class="sxs-lookup"><span data-stu-id="003d7-150">Example response</span></span>

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


* <span data-ttu-id="003d7-151">PUT, POST och DELETE-begäranden måste gå toohello anges skriva URI</span><span class="sxs-lookup"><span data-stu-id="003d7-151">All PUT, POST and DELETE requests must go toohello indicated write URI</span></span>
* <span data-ttu-id="003d7-152">Alla hämtar och andra skrivskyddade förfrågningar (till exempel frågor) kan gå tooany endpoint hello klienten väljer</span><span class="sxs-lookup"><span data-stu-id="003d7-152">All GETs and other read-only requests (for example queries) may go tooany endpoint of hello client’s choice</span></span>

<span data-ttu-id="003d7-153">Skriva begäranden endast tooread regioner misslyckas med felkoden för HTTP 403 (”förbjuden”).</span><span class="sxs-lookup"><span data-stu-id="003d7-153">Write requests tooread-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="003d7-154">Om hello skrivåtgärder region ändras efter skriver hello klienten den inledande identifieringen-fasen efterföljande toohello tidigare skrivåtgärder region misslyckas med felkoden för HTTP 403 (”förbjuden”).</span><span class="sxs-lookup"><span data-stu-id="003d7-154">If hello write region changes after hello client’s initial discovery phase, subsequent writes toohello previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="003d7-155">hello klienten bör sedan få hello listan över regioner igen tooget hello uppdaterade skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="003d7-155">hello client should then GET hello list of regions again tooget hello updated write region.</span></span>

<span data-ttu-id="003d7-156">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="003d7-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="003d7-157">Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="003d7-157">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="003d7-158">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="003d7-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="003d7-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="003d7-159">Next steps</span></span>

<span data-ttu-id="003d7-160">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="003d7-160">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="003d7-161">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="003d7-161">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="003d7-162">Konfigurera distributionslistor med hello DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="003d7-162">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="003d7-163">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="003d7-163">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="003d7-164">Utveckla lokalt med hello-emulatorn</span><span class="sxs-lookup"><span data-stu-id="003d7-164">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

