---
title: Ut data i Azure Cosmos DB med time to live | Microsoft Docs
description: "Med TTL tillhandahåller Microsoft Azure Cosmos DB möjligheten att låta dokument automatiskt bort från systemet efter en viss tidsperiod."
services: cosmos-db
documentationcenter: 
keywords: Time to live-
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
ms.openlocfilehash: 6f1c43ca0113dc7579b0fc3743d3314c16ce78a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a><span data-ttu-id="493b8-104">Data i Azure Cosmos DB samlingar automatiskt med time to live-att gälla</span><span class="sxs-lookup"><span data-stu-id="493b8-104">Expire data in Azure Cosmos DB collections automatically with time to live</span></span>
<span data-ttu-id="493b8-105">Program kan skapa och lagra stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="493b8-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="493b8-106">Vissa av dessa data, t.ex. datorn genereras data, loggar och användaren händelsesessionen information är bara användbara för en bestämd tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="493b8-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="493b8-107">När data blir överflödiga enligt behov av programmet som det är säkert att rensa data och minska lagringsbehov för ett program.</span><span class="sxs-lookup"><span data-stu-id="493b8-107">Once the data becomes surplus to the needs of the application it is safe to purge this data and reduce the storage needs of an application.</span></span>

<span data-ttu-id="493b8-108">Med ”time to live” eller TTL, tillhandahåller Microsoft Azure Cosmos DB möjligheten att låta dokument automatiskt rensas bort från databasen efter en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="493b8-108">With "time to live" or TTL, Microsoft Azure Cosmos DB provides the ability to have documents automatically purged from the database after a period of time.</span></span> <span data-ttu-id="493b8-109">Standard time to live kan ställas på samlingsnivå och åsidosätts på grundval av per dokument.</span><span class="sxs-lookup"><span data-stu-id="493b8-109">The default time to live can be set at the collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="493b8-110">När TTL-värde har angetts som standard samlingen eller på en dokumentnivå bort Cosmos DB automatiskt dokument som finns efter den tid i sekunder, eftersom de senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="493b8-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="493b8-111">Time to live i Cosmos-DB-använder en förskjutning mot när dokumentet senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="493b8-111">Time to live in Cosmos DB uses an offset against when the document was last modified.</span></span> <span data-ttu-id="493b8-112">Om du vill används den `_ts` fält som finns på alla dokument.</span><span class="sxs-lookup"><span data-stu-id="493b8-112">To do this it uses the `_ts` field which exists on every document.</span></span> <span data-ttu-id="493b8-113">Fältet _ts är en unix-format epok tidsstämpel som representerar datumet och tiden.</span><span class="sxs-lookup"><span data-stu-id="493b8-113">The _ts field is a unix-style epoch timestamp representing the date and time.</span></span> <span data-ttu-id="493b8-114">Den `_ts` varje gång ett dokument ändras uppdateras fältet.</span><span class="sxs-lookup"><span data-stu-id="493b8-114">The `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="493b8-115">TTL-beteende</span><span class="sxs-lookup"><span data-stu-id="493b8-115">TTL behavior</span></span>
<span data-ttu-id="493b8-116">TTL-funktionen styrs av TTL-egenskaper på två nivåer - samlingsnivå och dokumentnivå.</span><span class="sxs-lookup"><span data-stu-id="493b8-116">The TTL feature is controlled by TTL properties at two levels - the collection level and the document level.</span></span> <span data-ttu-id="493b8-117">Värdena anges i sekunder och behandlas som ett dokument från den `_ts` dokumentet senast ändrad.</span><span class="sxs-lookup"><span data-stu-id="493b8-117">The values are set in seconds and are treated as a delta from the `_ts` that the document was last modified at.</span></span>

1. <span data-ttu-id="493b8-118">DefaultTTL för samlingen</span><span class="sxs-lookup"><span data-stu-id="493b8-118">DefaultTTL for the collection</span></span>
   
   * <span data-ttu-id="493b8-119">Om de saknas (eller inställt på null) dokument tas inte bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="493b8-119">If missing (or set to null), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="493b8-120">Om finns, och värdet är ”-” 1 = oändlig – dokument inte ut som standard</span><span class="sxs-lookup"><span data-stu-id="493b8-120">If present and the value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="493b8-121">Om finns, och värdet är ett nummer (”n”) – dokument gälla sekunder ”n” efter senaste ändring</span><span class="sxs-lookup"><span data-stu-id="493b8-121">If present and the value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="493b8-122">TTL-värde för dokument:</span><span class="sxs-lookup"><span data-stu-id="493b8-122">TTL for the documents:</span></span> 
   
   * <span data-ttu-id="493b8-123">Egenskapen gäller endast om DefaultTTL finns för den överordnade samlingen.</span><span class="sxs-lookup"><span data-stu-id="493b8-123">Property is applicable only if DefaultTTL is present for the parent collection.</span></span>
   * <span data-ttu-id="493b8-124">Åsidosätter DefaultTTL för den överordnade samlingen.</span><span class="sxs-lookup"><span data-stu-id="493b8-124">Overrides the DefaultTTL value for the parent collection.</span></span>

<span data-ttu-id="493b8-125">När dokumentet har upphört att gälla (`ttl`  +  `_ts` > = aktuella servertiden), dokumentet har markerats som ”upphört att gälla”.</span><span class="sxs-lookup"><span data-stu-id="493b8-125">As soon as the document has expired (`ttl` + `_ts` >= current server time), the document is marked as "expired”.</span></span> <span data-ttu-id="493b8-126">Ingen åtgärd ska tillåtas på dessa dokument efter den tidpunkten och de kommer att uteslutas från resultatet av alla frågor som utförs.</span><span class="sxs-lookup"><span data-stu-id="493b8-126">No operation will be allowed on these documents after this time and they will be excluded from the results of any queries performed.</span></span> <span data-ttu-id="493b8-127">Dokumenten bort fysiskt i systemet och tas bort i bakgrunden tillfälligt vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="493b8-127">The documents are physically deleted in the system, and are deleted in the background opportunistically at a later time.</span></span> <span data-ttu-id="493b8-128">Detta inte tar upp någon [begära enheter (RUs)](request-units.md) från samlingen budget.</span><span class="sxs-lookup"><span data-stu-id="493b8-128">This does not consume any [Request Units (RUs)](request-units.md) from the collection budget.</span></span>

<span data-ttu-id="493b8-129">Ovanstående logiken kan visas i matrisen följande:</span><span class="sxs-lookup"><span data-stu-id="493b8-129">The above logic can be shown in the following matrix:</span></span>

|  | <span data-ttu-id="493b8-130">DefaultTTL saknas ej på samlingen</span><span class="sxs-lookup"><span data-stu-id="493b8-130">DefaultTTL missing/not set on the collection</span></span> | <span data-ttu-id="493b8-131">DefaultTTL = -1 på en samling</span><span class="sxs-lookup"><span data-stu-id="493b8-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="493b8-132">DefaultTTL = ”n” på en samling</span><span class="sxs-lookup"><span data-stu-id="493b8-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="493b8-133">TTL-värde saknas i dokumentet</span><span class="sxs-lookup"><span data-stu-id="493b8-133">TTL Missing on document</span></span> |<span data-ttu-id="493b8-134">Inget att åsidosätta på dokumentnivå eftersom både dokumentet och samling har något begrepp om TTL-värde.</span><span class="sxs-lookup"><span data-stu-id="493b8-134">Nothing to override at document level since both the document and collection have no concept of TTL.</span></span> |<span data-ttu-id="493b8-135">Inga dokument i den här samlingen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="493b8-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="493b8-136">Dokument i den här samlingen upphör att gälla efter intervall n tiden.</span><span class="sxs-lookup"><span data-stu-id="493b8-136">The documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="493b8-137">TTL = -1 i dokumentet</span><span class="sxs-lookup"><span data-stu-id="493b8-137">TTL = -1 on document</span></span> |<span data-ttu-id="493b8-138">Inget att åsidosätta på dokumentnivå eftersom samlingen inte definierar egenskapen DefaultTTL som ett dokument kan åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="493b8-138">Nothing to override at the document level since the collection doesn’t define the DefaultTTL property that a document can override.</span></span> <span data-ttu-id="493b8-139">TTL-värde i ett dokument är icke tolkad av systemet.</span><span class="sxs-lookup"><span data-stu-id="493b8-139">TTL on a document is un-interpreted by the system.</span></span> |<span data-ttu-id="493b8-140">Inga dokument i den här samlingen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="493b8-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="493b8-141">Dokumentet med TTL =-1 i den här samlingen upphör aldrig att gälla.</span><span class="sxs-lookup"><span data-stu-id="493b8-141">The document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="493b8-142">Alla dokument upphör att gälla efter ”n” intervall.</span><span class="sxs-lookup"><span data-stu-id="493b8-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="493b8-143">TTL = n i dokumentet</span><span class="sxs-lookup"><span data-stu-id="493b8-143">TTL = n on document</span></span> |<span data-ttu-id="493b8-144">Inget att åsidosätta på dokumentnivå.</span><span class="sxs-lookup"><span data-stu-id="493b8-144">Nothing to override at the document level.</span></span> <span data-ttu-id="493b8-145">TTL-värde på ett dokument i icke tolkad av systemet.</span><span class="sxs-lookup"><span data-stu-id="493b8-145">TTL on a document in un-interpreted by the system.</span></span> |<span data-ttu-id="493b8-146">Dokumentet har TTL = n upphör att gälla efter intervall n, i sekunder.</span><span class="sxs-lookup"><span data-stu-id="493b8-146">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="493b8-147">Andra dokument kommer att ärva intervallet-1 och aldrig går ut.</span><span class="sxs-lookup"><span data-stu-id="493b8-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="493b8-148">Dokumentet har TTL = n upphör att gälla efter intervall n, i sekunder.</span><span class="sxs-lookup"><span data-stu-id="493b8-148">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="493b8-149">Andra dokument ärver ”n” intervall från samlingen.</span><span class="sxs-lookup"><span data-stu-id="493b8-149">Other documents will inherit "n" interval from the collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="493b8-150">Konfigurera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="493b8-150">Configuring TTL</span></span>
<span data-ttu-id="493b8-151">Time to live-är inaktiverat som standard i alla Cosmos DB samlingar och på alla dokument som standard.</span><span class="sxs-lookup"><span data-stu-id="493b8-151">By default, time to live is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="493b8-152">Aktivera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="493b8-152">Enabling TTL</span></span>
<span data-ttu-id="493b8-153">Om du vill aktivera TTL-värde på en samling eller dokument i en samling som du behöver ange egenskapen DefaultTTL i en samling till -1 eller ett positivt tal som inte är noll.</span><span class="sxs-lookup"><span data-stu-id="493b8-153">To enable TTL on a collection, or the documents within a collection, you need to set the DefaultTTL property of a collection to either -1 or a non-zero positive number.</span></span> <span data-ttu-id="493b8-154">Inställningen av DefaultTTL-1 innebär som som standard alla dokument i samlingen ska alltid live men Cosmos-DB-tjänsten bör du övervaka den här samlingen för dokument som har åsidosatt den här standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="493b8-154">Setting the DefaultTTL to -1 means that by default all documents in the collection will live forever but the Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="493b8-155">Konfigurera standard TTL-värde på en samling</span><span class="sxs-lookup"><span data-stu-id="493b8-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="493b8-156">Du kan konfigurera en standardtid för live på en samlingsnivå.</span><span class="sxs-lookup"><span data-stu-id="493b8-156">You are able to configure a default time to live at a collection level.</span></span> <span data-ttu-id="493b8-157">Om du vill ange TTL-värdet för en samling, måste du ange ett positivt tal som är noll som anger tiden, i sekunder att gälla alla dokument i samlingen när senast ändrade tidsstämpeln för dokumentet (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="493b8-157">To set the TTL on a collection, you need to provide a non-zero positive number that indicates the period, in seconds, to expire all documents in the collection after the last modified timestamp of the document (`_ts`).</span></span> <span data-ttu-id="493b8-158">Alternativt kan du ange standardvärdet-1, vilket innebär att alla dokument som infogas i samlingen kommer live på obestämd tid som standard.</span><span class="sxs-lookup"><span data-stu-id="493b8-158">Or, you can set the default to -1, which implies that all documents inserted in to the collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="493b8-159">Inställningen TTL-värde på ett dokument</span><span class="sxs-lookup"><span data-stu-id="493b8-159">Setting TTL on a document</span></span>
<span data-ttu-id="493b8-160">Du kan ange specifika TTL-värde på dokumentnivå utöver att ställa in en standard-TTL för en samling.</span><span class="sxs-lookup"><span data-stu-id="493b8-160">In addition to setting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="493b8-161">Detta åsidosätter standardvärdet i mängden.</span><span class="sxs-lookup"><span data-stu-id="493b8-161">Doing this will override the default of the collection.</span></span>

* <span data-ttu-id="493b8-162">Om du vill ange TTL-värdet för ett dokument, måste du ange ett positivt tal som är noll som anger tiden, i sekunder att gå ut dokumentet när senast ändrade tidsstämpeln för dokumentet (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="493b8-162">To set the TTL on a document, you need to provide a non-zero positive number which indicates the period, in seconds, to expire the document after the last modified timestamp of the document (`_ts`).</span></span>
* <span data-ttu-id="493b8-163">Om ett dokument har inget fält med TTL-värde, gäller standard i mängden.</span><span class="sxs-lookup"><span data-stu-id="493b8-163">If a document has no TTL field, then the default of the collection will apply.</span></span>
* <span data-ttu-id="493b8-164">Om TTL-värdet är inaktiverat på samlingsnivå ignoreras TTL-fältet på dokumentet förrän TTL aktiveras igen på samlingen.</span><span class="sxs-lookup"><span data-stu-id="493b8-164">If TTL is disabled at the collection level, the TTL field on the document will be ignored until TTL is enabled again on the collection.</span></span>

<span data-ttu-id="493b8-165">Här är ett kodfragment som visar hur du ställer in TTL upphör att gälla på ett dokument:</span><span class="sxs-lookup"><span data-stu-id="493b8-165">Here's a snippet showing how to set the TTL expiration on a document:</span></span>

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="493b8-166">Utöka TTL-värde på ett befintligt dokument</span><span class="sxs-lookup"><span data-stu-id="493b8-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="493b8-167">Du kan återställa TTL för ett dokument genom att göra någon skrivåtgärd i dokumentet.</span><span class="sxs-lookup"><span data-stu-id="493b8-167">You can reset the TTL on a document by doing any write operation on the document.</span></span> <span data-ttu-id="493b8-168">Om du gör detta anger den `_ts` till aktuell tid och nedräkningen till dokumentet upphör att gälla, som angetts av den `ttl`, påbörjas igen.</span><span class="sxs-lookup"><span data-stu-id="493b8-168">Doing this will set the `_ts` to the current time, and the countdown to the document expiry, as set by the `ttl`, will begin again.</span></span> <span data-ttu-id="493b8-169">Om du vill ändra den `ttl` i ett dokument, du kan uppdatera fältet som du kan göra med något annat går fält.</span><span class="sxs-lookup"><span data-stu-id="493b8-169">If you wish to change the `ttl` of a document, you can update the field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="493b8-170">Ta bort TTL-värde från ett dokument</span><span class="sxs-lookup"><span data-stu-id="493b8-170">Removing TTL from a document</span></span>
<span data-ttu-id="493b8-171">Om ett TTL-värde har angetts för ett dokument och du inte längre vill att dokumentet ska upphöra att gälla, kan sedan du hämta dokumentet, ta bort fältet TTL-värde och ersätter dokumentet på servern.</span><span class="sxs-lookup"><span data-stu-id="493b8-171">If a TTL has been set on a document and you no longer want that document to expire, then you can retrieve the document, remove the TTL field and replace the document on the server.</span></span> <span data-ttu-id="493b8-172">När TTL-fältet tas bort från dokumentet, används standardvärdet i mängden.</span><span class="sxs-lookup"><span data-stu-id="493b8-172">When the TTL field is removed from the document, the default of the collection will be applied.</span></span> <span data-ttu-id="493b8-173">Om du vill stoppa ett dokument upphör att gälla och inte ärver från en samling måste du ange TTL-värdet-1.</span><span class="sxs-lookup"><span data-stu-id="493b8-173">To stop a document from expiring and not inherit from the collection then you need to set the TTL value to -1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="493b8-174">Inaktivera TTL-värde</span><span class="sxs-lookup"><span data-stu-id="493b8-174">Disabling TTL</span></span>
<span data-ttu-id="493b8-175">Om du vill inaktivera TTL helt på en samling och stoppa bakgrunden från söker efter utgångna dokument egenskapen DefaultTTL i samlingen ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="493b8-175">To disable TTL entirely on a collection and stop the background process from looking for expired documents the DefaultTTL property on the collection should be deleted.</span></span> <span data-ttu-id="493b8-176">Ta bort den här egenskapen skiljer sig från att ange-1.</span><span class="sxs-lookup"><span data-stu-id="493b8-176">Deleting this property is different from setting it to -1.</span></span> <span data-ttu-id="493b8-177">Inställningen för att 1 innebär nya dokument läggas till i samlingen ska alltid live men du kan åsidosätta detta på specifika dokument i samlingen.</span><span class="sxs-lookup"><span data-stu-id="493b8-177">Setting to -1 means new documents added to the collection will live forever but you can override this on specific documents in the collection.</span></span> <span data-ttu-id="493b8-178">Ta bort den här egenskapen helt från samlingen innebär att inga dokument upphör att gälla, även om det finns dokument som tidigare har uttryckligen åsidosätts.</span><span class="sxs-lookup"><span data-stu-id="493b8-178">Removing this property entirely from the collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="493b8-179">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="493b8-179">FAQ</span></span>
<span data-ttu-id="493b8-180">**Vad TTL kostar mig?**</span><span class="sxs-lookup"><span data-stu-id="493b8-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="493b8-181">Det finns utan extra kostnad för att ange ett TTL-värde i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="493b8-181">There is no additional cost to setting a TTL on a document.</span></span>

<span data-ttu-id="493b8-182">**Hur lång tid tar det att ta bort dokumentet när TTL-värdet är igång?**</span><span class="sxs-lookup"><span data-stu-id="493b8-182">**How long will it take to delete my document once the TTL is up?**</span></span>

<span data-ttu-id="493b8-183">Dokument som har upphört att gälla omedelbart när TTL-värdet är igång och kan inte nås via CRUD eller fråga API: er.</span><span class="sxs-lookup"><span data-stu-id="493b8-183">The documents are expired immediately once the TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="493b8-184">**TTL-värde i ett dokument har någon effekt på RU avgifter?**</span><span class="sxs-lookup"><span data-stu-id="493b8-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="493b8-185">Nej, det är ingen inverkan på RU kostnader för borttagningar av utgångna dokument via TTL-värde i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="493b8-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="493b8-186">**Funktionen TTL endast gäller för hela dokument eller kan jag upphöra att gälla egenskapsvärden för enskilda dokument?**</span><span class="sxs-lookup"><span data-stu-id="493b8-186">**Does the TTL feature only apply to entire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="493b8-187">TTL-värde gäller för hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="493b8-187">TTL applies to the entire document.</span></span> <span data-ttu-id="493b8-188">Om du vill att gälla bara en del av ett dokument, sedan rekommenderas det att du extrahera delen från det huvudsakliga dokumentet till ett separat ”länkade” dokument och sedan använda TTL-värde med det extraherade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="493b8-188">If you would like to expire just a portion of a document, then it is recommended that you extract the portion from the main document in to a separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="493b8-189">**Har funktionen TTL eventuella särskilda krav för fulltextindexering?**</span><span class="sxs-lookup"><span data-stu-id="493b8-189">**Does the TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="493b8-190">Ja.</span><span class="sxs-lookup"><span data-stu-id="493b8-190">Yes.</span></span> <span data-ttu-id="493b8-191">Samlingen måste ha [indexering principen](indexing-policies.md) konsekvent eller Lazy.</span><span class="sxs-lookup"><span data-stu-id="493b8-191">The collection must have [indexing policy set](indexing-policies.md) to either Consistent or Lazy.</span></span> <span data-ttu-id="493b8-192">Försök att ange DefaultTTL på en samling med indexering inställd på None resulterar i ett fel som kommer försök att inaktivera indexering på en samling som har en DefaultTTL som redan angetts.</span><span class="sxs-lookup"><span data-stu-id="493b8-192">Trying to set DefaultTTL on a collection with indexing set to None will result in an error, as will trying to turn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="493b8-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="493b8-193">Next steps</span></span>
<span data-ttu-id="493b8-194">Om du vill veta mer om Azure Cosmos DB kan referera till tjänsten [ *dokumentationen* ](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan.</span><span class="sxs-lookup"><span data-stu-id="493b8-194">To learn more about Azure Cosmos DB, refer to the service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

