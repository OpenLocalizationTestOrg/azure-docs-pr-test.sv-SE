---
title: aaaExpire data i Azure Cosmos DB med tiden toolive | Microsoft Docs
description: "Med TTL tillhandahåller Microsoft Azure Cosmos DB hello möjlighet toohave dokument automatiskt rensas bort från hello systemet efter en viss tidsperiod."
services: cosmos-db
documentationcenter: 
keywords: tid toolive
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: 51d8ec46add72c9624457316a4ccd1e23fb83ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a><span data-ttu-id="1ab73-104">Data i Azure Cosmos DB samlingar automatiskt med tiden toolive att gälla</span><span class="sxs-lookup"><span data-stu-id="1ab73-104">Expire data in Azure Cosmos DB collections automatically with time toolive</span></span>
<span data-ttu-id="1ab73-105">Program kan skapa och lagra stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="1ab73-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="1ab73-106">Vissa av dessa data, t.ex. datorn genereras data, loggar och användaren händelsesessionen information är bara användbara för en bestämd tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="1ab73-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="1ab73-107">När hello data blir överflödiga toohello behov av programmet hello är säker toopurge dessa data och minska hello lagringsbehov för ett program.</span><span class="sxs-lookup"><span data-stu-id="1ab73-107">Once hello data becomes surplus toohello needs of hello application it is safe toopurge this data and reduce hello storage needs of an application.</span></span>

<span data-ttu-id="1ab73-108">Med ”time toolive” eller TTL tillhandahåller Microsoft Azure Cosmos DB hello möjlighet toohave dokument automatiskt bort från hello databasen efter en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="1ab73-108">With "time toolive" or TTL, Microsoft Azure Cosmos DB provides hello ability toohave documents automatically purged from hello database after a period of time.</span></span> <span data-ttu-id="1ab73-109">hello standardtid toolive kan ange på samlingsnivå hello och åsidosätts på grundval av per dokument.</span><span class="sxs-lookup"><span data-stu-id="1ab73-109">hello default time toolive can be set at hello collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="1ab73-110">När TTL-värde har angetts som standard samlingen eller på en dokumentnivå bort Cosmos DB automatiskt dokument som finns efter den tid i sekunder, eftersom de senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="1ab73-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="1ab73-111">Tid toolive i Cosmos-databasen använder en förskjutning mot när hello dokumentet senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="1ab73-111">Time toolive in Cosmos DB uses an offset against when hello document was last modified.</span></span> <span data-ttu-id="1ab73-112">toodo den här den använder hello `_ts` fält som finns på alla dokument.</span><span class="sxs-lookup"><span data-stu-id="1ab73-112">toodo this it uses hello `_ts` field which exists on every document.</span></span> <span data-ttu-id="1ab73-113">Hej _ts fältet är en unix-format epok tidsstämpel som representerar hello datum och tid.</span><span class="sxs-lookup"><span data-stu-id="1ab73-113">hello _ts field is a unix-style epoch timestamp representing hello date and time.</span></span> <span data-ttu-id="1ab73-114">Hej `_ts` varje gång ett dokument ändras uppdateras fältet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-114">hello `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="1ab73-115">TTL-beteende</span><span class="sxs-lookup"><span data-stu-id="1ab73-115">TTL behavior</span></span>
<span data-ttu-id="1ab73-116">hello TTL-funktionen styrs av TTL-egenskaper på två nivåer - hello samlingsnivå och hello för dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-116">hello TTL feature is controlled by TTL properties at two levels - hello collection level and hello document level.</span></span> <span data-ttu-id="1ab73-117">hello värden anges i sekunder och behandlas som en lista från hello `_ts` hello dokumentet har ändrades senast.</span><span class="sxs-lookup"><span data-stu-id="1ab73-117">hello values are set in seconds and are treated as a delta from hello `_ts` that hello document was last modified at.</span></span>

1. <span data-ttu-id="1ab73-118">DefaultTTL för hello samling</span><span class="sxs-lookup"><span data-stu-id="1ab73-118">DefaultTTL for hello collection</span></span>
   
   * <span data-ttu-id="1ab73-119">Om saknas (eller uppsättning toonull) dokument tas inte bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1ab73-119">If missing (or set toonull), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="1ab73-120">Om den finns och är-hello värdet ”1” = oändlig – dokument inte ut som standard</span><span class="sxs-lookup"><span data-stu-id="1ab73-120">If present and hello value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="1ab73-121">Om den finns och hello värde är ett nummer (”n”) – dokument gälla ”n” sekunder efter senast ändrades</span><span class="sxs-lookup"><span data-stu-id="1ab73-121">If present and hello value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="1ab73-122">TTL-värde för hello dokument:</span><span class="sxs-lookup"><span data-stu-id="1ab73-122">TTL for hello documents:</span></span> 
   
   * <span data-ttu-id="1ab73-123">Egenskapen gäller endast om DefaultTTL finns för hello överordnade samlingen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-123">Property is applicable only if DefaultTTL is present for hello parent collection.</span></span>
   * <span data-ttu-id="1ab73-124">Åsidosätter hello DefaultTTL för hello överordnade samlingen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-124">Overrides hello DefaultTTL value for hello parent collection.</span></span>

<span data-ttu-id="1ab73-125">Så snart hello dokumentet har upphört att gälla (`ttl`  +  `_ts` > = aktuella servertiden), hello dokumentet har markerats som ”upphört att gälla”.</span><span class="sxs-lookup"><span data-stu-id="1ab73-125">As soon as hello document has expired (`ttl` + `_ts` >= current server time), hello document is marked as "expired”.</span></span> <span data-ttu-id="1ab73-126">Ingen åtgärd ska tillåtas på dessa dokument efter den tidpunkten och de kommer att uteslutas från hello resultatet av alla frågor som utförs.</span><span class="sxs-lookup"><span data-stu-id="1ab73-126">No operation will be allowed on these documents after this time and they will be excluded from hello results of any queries performed.</span></span> <span data-ttu-id="1ab73-127">hello dokument fysiskt tas bort i hello system och tas bort i bakgrunden hello tillfälligt vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="1ab73-127">hello documents are physically deleted in hello system, and are deleted in hello background opportunistically at a later time.</span></span> <span data-ttu-id="1ab73-128">Detta inte tar upp någon [begära enheter (RUs)](request-units.md) från hello samling budget.</span><span class="sxs-lookup"><span data-stu-id="1ab73-128">This does not consume any [Request Units (RUs)](request-units.md) from hello collection budget.</span></span>

<span data-ttu-id="1ab73-129">hello ovan logik kan visas i följande matris hello:</span><span class="sxs-lookup"><span data-stu-id="1ab73-129">hello above logic can be shown in hello following matrix:</span></span>

|  | <span data-ttu-id="1ab73-130">DefaultTTL saknas ej på hello samling</span><span class="sxs-lookup"><span data-stu-id="1ab73-130">DefaultTTL missing/not set on hello collection</span></span> | <span data-ttu-id="1ab73-131">DefaultTTL = -1 på en samling</span><span class="sxs-lookup"><span data-stu-id="1ab73-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="1ab73-132">DefaultTTL = ”n” på en samling</span><span class="sxs-lookup"><span data-stu-id="1ab73-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="1ab73-133">TTL-värde saknas i dokumentet</span><span class="sxs-lookup"><span data-stu-id="1ab73-133">TTL Missing on document</span></span> |<span data-ttu-id="1ab73-134">Inget toooverride på dokumentnivå eftersom både hello dokumentet och samling har något begrepp om TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="1ab73-134">Nothing toooverride at document level since both hello document and collection have no concept of TTL.</span></span> |<span data-ttu-id="1ab73-135">Inga dokument i den här samlingen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="1ab73-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="1ab73-136">hello dokument i den här samlingen upphör att gälla efter intervall n tiden.</span><span class="sxs-lookup"><span data-stu-id="1ab73-136">hello documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="1ab73-137">TTL = -1 i dokumentet</span><span class="sxs-lookup"><span data-stu-id="1ab73-137">TTL = -1 on document</span></span> |<span data-ttu-id="1ab73-138">Inget toooverride nivån hello dokument eftersom hello samlingen inte definierar hello DefaultTTL egenskap som ett dokument kan åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="1ab73-138">Nothing toooverride at hello document level since hello collection doesn’t define hello DefaultTTL property that a document can override.</span></span> <span data-ttu-id="1ab73-139">TTL-värde i ett dokument är icke tolkad hello-systemet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-139">TTL on a document is un-interpreted by hello system.</span></span> |<span data-ttu-id="1ab73-140">Inga dokument i den här samlingen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="1ab73-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="1ab73-141">upphör aldrig att gälla hello dokument med TTL =-1 i den här samlingen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-141">hello document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="1ab73-142">Alla dokument upphör att gälla efter ”n” intervall.</span><span class="sxs-lookup"><span data-stu-id="1ab73-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="1ab73-143">TTL = n i dokumentet</span><span class="sxs-lookup"><span data-stu-id="1ab73-143">TTL = n on document</span></span> |<span data-ttu-id="1ab73-144">Inget toooverride på hello dokumentnivå.</span><span class="sxs-lookup"><span data-stu-id="1ab73-144">Nothing toooverride at hello document level.</span></span> <span data-ttu-id="1ab73-145">TTL-värde på ett dokument i icke tolkad hello-systemet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-145">TTL on a document in un-interpreted by hello system.</span></span> |<span data-ttu-id="1ab73-146">hello dokument med TTL = n upphör att gälla efter intervall n, i sekunder.</span><span class="sxs-lookup"><span data-stu-id="1ab73-146">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="1ab73-147">Andra dokument kommer att ärva intervallet-1 och aldrig går ut.</span><span class="sxs-lookup"><span data-stu-id="1ab73-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="1ab73-148">hello dokument med TTL = n upphör att gälla efter intervall n, i sekunder.</span><span class="sxs-lookup"><span data-stu-id="1ab73-148">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="1ab73-149">Andra dokument ärver ”n” intervall från hello samling.</span><span class="sxs-lookup"><span data-stu-id="1ab73-149">Other documents will inherit "n" interval from hello collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="1ab73-150">Konfigurera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="1ab73-150">Configuring TTL</span></span>
<span data-ttu-id="1ab73-151">Tid toolive är inaktiverad som standard i alla Cosmos DB samlingar och på alla dokument som standard.</span><span class="sxs-lookup"><span data-stu-id="1ab73-151">By default, time toolive is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="1ab73-152">Aktivera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="1ab73-152">Enabling TTL</span></span>
<span data-ttu-id="1ab73-153">tooenable TTL-värde på en samling eller hello dokument i en samling behöver du tooset hello DefaultTTL egenskap för en samling tooeither -1 eller ett positivt tal som inte är noll.</span><span class="sxs-lookup"><span data-stu-id="1ab73-153">tooenable TTL on a collection, or hello documents within a collection, you need tooset hello DefaultTTL property of a collection tooeither -1 or a non-zero positive number.</span></span> <span data-ttu-id="1ab73-154">Ange hello DefaultTTL för-1 innebär att som standard alla dokument i hello samlingen kommer live alltid men hello Cosmos-DB-tjänsten bör du övervaka den här samlingen för dokument som har åsidosatt den här standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-154">Setting hello DefaultTTL too-1 means that by default all documents in hello collection will live forever but hello Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="1ab73-155">Konfigurera standard TTL-värde på en samling</span><span class="sxs-lookup"><span data-stu-id="1ab73-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="1ab73-156">Du kan kan tooconfigure en toolive standardtid på en samlingsnivå.</span><span class="sxs-lookup"><span data-stu-id="1ab73-156">You are able tooconfigure a default time toolive at a collection level.</span></span> <span data-ttu-id="1ab73-157">tooset hello TTL-värde på en samling måste tooprovide ett positivt tal som är noll som visar hello period i sekunder, tooexpire alla dokument i hello samlingen när hello senast ändrad tidsstämpeln för hello dokumentet (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="1ab73-157">tooset hello TTL on a collection, you need tooprovide a non-zero positive number that indicates hello period, in seconds, tooexpire all documents in hello collection after hello last modified timestamp of hello document (`_ts`).</span></span> <span data-ttu-id="1ab73-158">Alternativt kan du ange hello standard för-1, vilket innebär att alla dokument som infogas i toohello samlingen kommer live på obestämd tid som standard.</span><span class="sxs-lookup"><span data-stu-id="1ab73-158">Or, you can set hello default too-1, which implies that all documents inserted in toohello collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="1ab73-159">Inställningen TTL-värde på ett dokument</span><span class="sxs-lookup"><span data-stu-id="1ab73-159">Setting TTL on a document</span></span>
<span data-ttu-id="1ab73-160">Dessutom toosetting standard TTL-värde för en samling du kan ange specifika TTL-värde på dokumentnivå.</span><span class="sxs-lookup"><span data-stu-id="1ab73-160">In addition toosetting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="1ab73-161">Detta åsidosätter hello standard hello mängden.</span><span class="sxs-lookup"><span data-stu-id="1ab73-161">Doing this will override hello default of hello collection.</span></span>

* <span data-ttu-id="1ab73-162">tooset hello TTL-värde på ett dokument, behöver du tooprovide ett positivt tal som är noll vilket betyder hello period i sekunder, tooexpire hello dokument när hello senast ändrad tidsstämpeln för hello dokumentet (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="1ab73-162">tooset hello TTL on a document, you need tooprovide a non-zero positive number which indicates hello period, in seconds, tooexpire hello document after hello last modified timestamp of hello document (`_ts`).</span></span>
* <span data-ttu-id="1ab73-163">Om ett dokument har inget TTL-fält hello standardvärdet hello samling gäller.</span><span class="sxs-lookup"><span data-stu-id="1ab73-163">If a document has no TTL field, then hello default of hello collection will apply.</span></span>
* <span data-ttu-id="1ab73-164">Om TTL-värdet är inaktiverat på samlingsnivå hello ignoreras hello TTL-fältet för hello dokument tills TTL aktiveras igen på hello samling.</span><span class="sxs-lookup"><span data-stu-id="1ab73-164">If TTL is disabled at hello collection level, hello TTL field on hello document will be ignored until TTL is enabled again on hello collection.</span></span>

<span data-ttu-id="1ab73-165">Här är ett kodfragment som visar hur tooset hello TTL upphör att gälla på ett dokument:</span><span class="sxs-lookup"><span data-stu-id="1ab73-165">Here's a snippet showing how tooset hello TTL expiration on a document:</span></span>

    // Include a property that serializes too"ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used tooset expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set hello value toohello expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="1ab73-166">Utöka TTL-värde på ett befintligt dokument</span><span class="sxs-lookup"><span data-stu-id="1ab73-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="1ab73-167">Du kan återställa hello TTL-värde i ett dokument genom att göra någon skrivåtgärd i hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-167">You can reset hello TTL on a document by doing any write operation on hello document.</span></span> <span data-ttu-id="1ab73-168">Detta kommer att ange hello `_ts` toohello aktuell tid och hello nedräkning toohello dokumentet upphör att gälla, som angetts av hello `ttl`, påbörjas igen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-168">Doing this will set hello `_ts` toohello current time, and hello countdown toohello document expiry, as set by hello `ttl`, will begin again.</span></span> <span data-ttu-id="1ab73-169">Om du inte vill toochange hello `ttl` i ett dokument, kan du uppdatera hello fält som du kan göra med något annat går fält.</span><span class="sxs-lookup"><span data-stu-id="1ab73-169">If you wish toochange hello `ttl` of a document, you can update hello field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="1ab73-170">Ta bort TTL-värde från ett dokument</span><span class="sxs-lookup"><span data-stu-id="1ab73-170">Removing TTL from a document</span></span>
<span data-ttu-id="1ab73-171">Om ett TTL-värde har angetts för ett dokument och du inte längre vill att dokumentet tooexpire, sedan du kan hämta hello dokument, ta bort hello TTL-fältet och ersätta hello dokument på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="1ab73-171">If a TTL has been set on a document and you no longer want that document tooexpire, then you can retrieve hello document, remove hello TTL field and replace hello document on hello server.</span></span> <span data-ttu-id="1ab73-172">När hello TTL-fältet tas bort från hello dokument kommer hello standardvärdet hello samlingen att tillämpas.</span><span class="sxs-lookup"><span data-stu-id="1ab73-172">When hello TTL field is removed from hello document, hello default of hello collection will be applied.</span></span> <span data-ttu-id="1ab73-173">toostop ett dokument upphör att gälla och inte ärver från hello samling måste tooset hello TTL-värdet för-1.</span><span class="sxs-lookup"><span data-stu-id="1ab73-173">toostop a document from expiring and not inherit from hello collection then you need tooset hello TTL value too-1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="1ab73-174">Inaktivera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="1ab73-174">Disabling TTL</span></span>
<span data-ttu-id="1ab73-175">toodisable TTL helt på en samling och stoppa hello bakgrund bearbeta från söker efter utgångna dokument hello DefaultTTL egenskapen på hello samling ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="1ab73-175">toodisable TTL entirely on a collection and stop hello background process from looking for expired documents hello DefaultTTL property on hello collection should be deleted.</span></span> <span data-ttu-id="1ab73-176">Ta bort den här egenskapen skiljer sig från att ange värdet 1 för.</span><span class="sxs-lookup"><span data-stu-id="1ab73-176">Deleting this property is different from setting it too-1.</span></span> <span data-ttu-id="1ab73-177">Inställningen för-1 innebär nya dokument lägga toohello samlingen kommer alltid live men du kan åsidosätta detta på specifika dokument i hello samling.</span><span class="sxs-lookup"><span data-stu-id="1ab73-177">Setting too-1 means new documents added toohello collection will live forever but you can override this on specific documents in hello collection.</span></span> <span data-ttu-id="1ab73-178">Ta bort den här egenskapen helt från hello samling innebär att inga dokument upphör att gälla, även om det finns dokument som tidigare har uttryckligen åsidosätts.</span><span class="sxs-lookup"><span data-stu-id="1ab73-178">Removing this property entirely from hello collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="1ab73-179">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="1ab73-179">FAQ</span></span>
<span data-ttu-id="1ab73-180">**Vad TTL kostar mig?**</span><span class="sxs-lookup"><span data-stu-id="1ab73-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="1ab73-181">Det finns inga extra kostnad toosetting ett TTL-värde i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="1ab73-181">There is no additional cost toosetting a TTL on a document.</span></span>

<span data-ttu-id="1ab73-182">**Hur lång tid tar det toodelete dokumentet när hello TTL-värde som är igång?**</span><span class="sxs-lookup"><span data-stu-id="1ab73-182">**How long will it take toodelete my document once hello TTL is up?**</span></span>

<span data-ttu-id="1ab73-183">hello dokument har upphört att gälla omedelbart när hello TTL-värde som är igång och kan inte nås via CRUD eller fråga API: er.</span><span class="sxs-lookup"><span data-stu-id="1ab73-183">hello documents are expired immediately once hello TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="1ab73-184">**TTL-värde i ett dokument har någon effekt på RU avgifter?**</span><span class="sxs-lookup"><span data-stu-id="1ab73-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="1ab73-185">Nej, det är ingen inverkan på RU kostnader för borttagningar av utgångna dokument via TTL-värde i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="1ab73-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="1ab73-186">**Hello TTL-funktionen endast gäller tooentire dokument eller kan jag upphöra att gälla egenskapsvärden för enskilda dokument?**</span><span class="sxs-lookup"><span data-stu-id="1ab73-186">**Does hello TTL feature only apply tooentire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="1ab73-187">TTL-värde gäller toohello hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-187">TTL applies toohello entire document.</span></span> <span data-ttu-id="1ab73-188">Om du vill att tooexpire rekommenderas bara en del av ett dokument, då du extrahera hello del från hello dokument i tooa separat ”länkade” dokument och sedan använda TTL-värde med det extraherade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1ab73-188">If you would like tooexpire just a portion of a document, then it is recommended that you extract hello portion from hello main document in tooa separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="1ab73-189">**Har hello TTL-funktionen eventuella särskilda krav för fulltextindexering?**</span><span class="sxs-lookup"><span data-stu-id="1ab73-189">**Does hello TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="1ab73-190">Ja.</span><span class="sxs-lookup"><span data-stu-id="1ab73-190">Yes.</span></span> <span data-ttu-id="1ab73-191">hello samlingen måste innehålla [indexering principen](indexing-policies.md) tooeither konsekvent eller Lazy.</span><span class="sxs-lookup"><span data-stu-id="1ab73-191">hello collection must have [indexing policy set](indexing-policies.md) tooeither Consistent or Lazy.</span></span> <span data-ttu-id="1ab73-192">Försök tooset DefaultTTL på en samling med indexering set tooNone resulterar i ett fel som kommer försök tooturn av indexering på en samling som har en DefaultTTL som redan angetts.</span><span class="sxs-lookup"><span data-stu-id="1ab73-192">Trying tooset DefaultTTL on a collection with indexing set tooNone will result in an error, as will trying tooturn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ab73-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ab73-193">Next steps</span></span>
<span data-ttu-id="1ab73-194">toolearn mer om Azure Cosmos DB finns toohello service [ *dokumentationen* ](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan.</span><span class="sxs-lookup"><span data-stu-id="1ab73-194">toolearn more about Azure Cosmos DB, refer toohello service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

