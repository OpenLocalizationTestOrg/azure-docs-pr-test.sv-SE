---
title: "Azure CosmosDB: Programbegäran per minut (RU/m) | Microsoft Docs"
description: "Lär dig att minska kostnaderna genom att använda frågeenheter per minut."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="db5b6-103">Frågeenheter per minut i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="db5b6-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="db5b6-104">Azure Cosmos-DB är utformat för att hjälpa dig att uppnå en snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program tillväxt.</span><span class="sxs-lookup"><span data-stu-id="db5b6-104">Azure Cosmos DB is designed to help you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="db5b6-105">Du kan etablera dataflödet på en Cosmos-DB-behållare på båda, per sekund och granulariteter per minut (RU/m).</span><span class="sxs-lookup"><span data-stu-id="db5b6-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="db5b6-106">Det etablerade dataflödet på minutprecision används för att hantera oväntade toppar i arbetsbelastning som sker med en per sekund-precision.</span><span class="sxs-lookup"><span data-stu-id="db5b6-106">The provisioned throughput at per-minute granularity is used to manage unexpected spikes in the workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="db5b6-107">Den här artikeln innehåller en översikt över hur etableringen av Frågeenhet per minut (RU/m) fungerar.</span><span class="sxs-lookup"><span data-stu-id="db5b6-107">This article provides an overview of how the provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="db5b6-108">Målet i åtanke med etablering av RU/m är att tillhandahålla en förutsägbar prestanda runt oförutsägbart behov (särskilt om du behöver köra analytics ovanpå operational data) och spiky arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="db5b6-108">The goal in mind with provisioning of RU/m is to provide a predictable performance around unpredictable needs (especially if you need to run analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="db5b6-109">Vi vill att kunderna förbruka mer genomströmning som de konfigurerar så att de kan skalas snabbt med sinnesro.</span><span class="sxs-lookup"><span data-stu-id="db5b6-109">We want to have our customers consume more the throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="db5b6-110">När du har läst den här artikeln kommer du att kunna svara på följande frågor:</span><span class="sxs-lookup"><span data-stu-id="db5b6-110">After reading this article, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="db5b6-111">Hur fungerar en Frågeenhet per minut?</span><span class="sxs-lookup"><span data-stu-id="db5b6-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="db5b6-112">Vad är skillnaden mellan Frågeenhet per minut och Frågeenhet per sekund?</span><span class="sxs-lookup"><span data-stu-id="db5b6-112">What is the difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="db5b6-113">Hur du etablerar RU/m?</span><span class="sxs-lookup"><span data-stu-id="db5b6-113">How to provision RU/m?</span></span>
* <span data-ttu-id="db5b6-114">Under vilket scenario beakta jag etablering Frågeenhet per minut?</span><span class="sxs-lookup"><span data-stu-id="db5b6-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="db5b6-115">Hur du använder portalen mått för att optimera min kostnad och prestanda?</span><span class="sxs-lookup"><span data-stu-id="db5b6-115">How to use the portal metrics to optimize my cost and performance?</span></span>
* <span data-ttu-id="db5b6-116">Definiera vilken typ av begäran kan använda din RU/m budget?</span><span class="sxs-lookup"><span data-stu-id="db5b6-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="db5b6-117">Etablering av frågeenheter per minut (RU/m)</span><span class="sxs-lookup"><span data-stu-id="db5b6-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="db5b6-118">När du etablerar Azure Cosmos DB på andra Granulariteten (RU/s) kan du hämta garanti för att din begäran lyckas på en låg latens om din genomströmning inte har överskridit den kapacitet som tilldelats i den andra.</span><span class="sxs-lookup"><span data-stu-id="db5b6-118">When you provision Azure Cosmos DB at the second granularity (RU/s), you get the guarantee that your request succeeds at a low latency if your throughput has not exceeded the capacity provisioned within that second.</span></span> <span data-ttu-id="db5b6-119">Med RU/m är Granulariteten minut med garanti för att begäran slutförs inom den minuten.</span><span class="sxs-lookup"><span data-stu-id="db5b6-119">With RU/m, the granularity is at the minute with the guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="db5b6-120">Jämfört med burst-överföring system, kontrollera vi att du får prestanda är förutsägbar och kan du planera på den.</span><span class="sxs-lookup"><span data-stu-id="db5b6-120">Compared to bursting systems, we make sure that the performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="db5b6-121">Hur per minut etablering fungerar är enkel:</span><span class="sxs-lookup"><span data-stu-id="db5b6-121">The way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="db5b6-122">RU/m faktureras timvis och utöver RU/s.</span><span class="sxs-lookup"><span data-stu-id="db5b6-122">RU/m is billed hourly and in addition to RU/s.</span></span> <span data-ttu-id="db5b6-123">Mer information finns i Azure Cosmos DB [sida med priser](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="db5b6-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="db5b6-124">RU/m kan aktiveras på samlingsnivå.</span><span class="sxs-lookup"><span data-stu-id="db5b6-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="db5b6-125">Som kan göras via SDK: er (Node.js, Java eller .net) eller via portalen (också inkludera MongoDB API arbetsbelastningar)</span><span class="sxs-lookup"><span data-stu-id="db5b6-125">That can be done through the SDKs (Node.js, Java, or .Net) or through the portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="db5b6-126">När RU/m är aktiverat för varje 100 RU/s etableras, också få 1 000 RU/m etablerats (förhållandet är 10 x)</span><span class="sxs-lookup"><span data-stu-id="db5b6-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (the ratio is 10x)</span></span>
* <span data-ttu-id="db5b6-127">Vid en given andra förbrukar en begäran enhet din RU/m etablering endast om du har överskridit din per andra etablering i den andra</span><span class="sxs-lookup"><span data-stu-id="db5b6-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="db5b6-128">När 60-sekunders tid (UTC) slutar den per minut etablering är fyllas</span><span class="sxs-lookup"><span data-stu-id="db5b6-128">Once the 60-second period (UTC) ends, the per minute provisioning is refilled</span></span>
* <span data-ttu-id="db5b6-129">RU/m kan aktiveras endast för samlingar med en maximal etablering av 5 000 RU/s per partition.</span><span class="sxs-lookup"><span data-stu-id="db5b6-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="db5b6-130">Om du skala dina behov av genomflöde och har en hög nivå av etablering per partition, får du ett varningsmeddelande</span><span class="sxs-lookup"><span data-stu-id="db5b6-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="db5b6-131">Nedan ett konkreta exempel, där en kund kan etablera 10kRU/s med 100kRU/m, sparar 73% kostnaden mot etablering för belastning (på 50kRU per sekund) via en 90 sekunder period på en samling som har 10 000 RU/s och 100 000 RU/m etableras:</span><span class="sxs-lookup"><span data-stu-id="db5b6-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="db5b6-132">1 sekund: RU/m budget är inställt på 100 000</span><span class="sxs-lookup"><span data-stu-id="db5b6-132">1st second: The RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="db5b6-133">3: e sekund: under den andra förbrukningen av Frågeenhet var 11,010 RUs, 1,010 RUs ovan RU/s etableringen.</span><span class="sxs-lookup"><span data-stu-id="db5b6-133">3rd second: During that second the consumption of Request Unit was 11,010 RUs, 1,010 RUs above the RU/s provisioning.</span></span> <span data-ttu-id="db5b6-134">Därför dras 1,010 RUs från RU/m budget.</span><span class="sxs-lookup"><span data-stu-id="db5b6-134">Therefore, 1,010 RUs are deducted from the RU/m budget.</span></span> <span data-ttu-id="db5b6-135">98,990 RUs är tillgängliga nästa 57 sekunder i budget RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-135">98,990 RUs are available for the next 57 seconds in the RU/m budget</span></span>
* <span data-ttu-id="db5b6-136">29: e sekund: under den andra stora topp har hänt (> 4 x som är högre än etablering per sekund) och förbrukningen av Frågeenhet var 46,920 RUs.</span><span class="sxs-lookup"><span data-stu-id="db5b6-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and the consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="db5b6-137">36,920 RUs dras från RU/m budget bort från 92,323 RUs (28 sekund) till 55,403 RUs (29: e sekund)</span><span class="sxs-lookup"><span data-stu-id="db5b6-137">36,920 RUs are deducted from the RU/m budget that dropped from 92,323 RUs (28th second) to 55,403 RUs (29th second)</span></span>
* <span data-ttu-id="db5b6-138">61st sekund: RU/m budget ändras till 100 000 RUs.</span><span class="sxs-lookup"><span data-stu-id="db5b6-138">61st second: RU/m budget is set back to 100,000 RUs.</span></span>
 
![Diagrammet visar förbrukningen och etablering av Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="db5b6-140">Ange kapacitet för begäran-enhet med RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="db5b6-141">När du skapar en Azure DB som Cosmos-samling kan du ange hur många frågeenheter per sekund (RU per sekund) som du vill reserverade för samlingen.</span><span class="sxs-lookup"><span data-stu-id="db5b6-141">When creating an Azure Cosmos DB collection, you specify the number of request units per second (RU per second) you want reserved for the collection.</span></span> <span data-ttu-id="db5b6-142">Du kan också bestämma om du vill lägga till RU per minut.</span><span class="sxs-lookup"><span data-stu-id="db5b6-142">You can also decide if you want to add RU per minute.</span></span> <span data-ttu-id="db5b6-143">Detta kan göras via portalen eller SDK.</span><span class="sxs-lookup"><span data-stu-id="db5b6-143">This can be done through the Portal or the SDK.</span></span> 

### <a name="through-the-portal"></a><span data-ttu-id="db5b6-144">Via portalen</span><span class="sxs-lookup"><span data-stu-id="db5b6-144">Through the Portal</span></span>

<span data-ttu-id="db5b6-145">Aktivera eller inaktivera RU per minut kräver bara ett klick vid etablering av en samling.</span><span class="sxs-lookup"><span data-stu-id="db5b6-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Skärmbild som visar hur du ställer in RU/m i Azure-portalen](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a><span data-ttu-id="db5b6-147">Via SDK</span><span class="sxs-lookup"><span data-stu-id="db5b6-147">Through the SDK</span></span>
<span data-ttu-id="db5b6-148">Först, är det viktigt att notera RU/m är endast tillgängligt för följande SDK:</span><span class="sxs-lookup"><span data-stu-id="db5b6-148">First, this is important to note that RU/m is only available for the following SDKs:</span></span>

* <span data-ttu-id="db5b6-149">.NET 1.14.0</span><span class="sxs-lookup"><span data-stu-id="db5b6-149">.Net 1.14.0</span></span>
* <span data-ttu-id="db5b6-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="db5b6-150">Java 1.11.0</span></span>
* <span data-ttu-id="db5b6-151">Node.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="db5b6-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="db5b6-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="db5b6-152">Python 2.2.0</span></span>

<span data-ttu-id="db5b6-153">Här är ett kodfragment för att skapa en samling med 3 000 frågeenheter per andra och 30 000 frågeenheter per minut med .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="db5b6-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using the .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="db5b6-154">Här är ett kodfragment för att ändra genomflödet av en samling till 5 000 frågeenheter per sekund utan etablering RU per minut med .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="db5b6-154">Here is a code snippet for changing the throughput of a collection to 5,000 request units per second without provisioning RU per minute using the .NET SDK:</span></span>

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="db5b6-155">Bra passa scenarier</span><span class="sxs-lookup"><span data-stu-id="db5b6-155">Good fit scenarios</span></span>

<span data-ttu-id="db5b6-156">I det här avsnittet ger vi en översikt över scenarier som passar bra för att aktivera frågeenheter per minut.</span><span class="sxs-lookup"><span data-stu-id="db5b6-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="db5b6-157">**Miljö för utveckling och testning:** passar bra.</span><span class="sxs-lookup"><span data-stu-id="db5b6-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="db5b6-158">Under utveckling-steget om du vill testa ditt program med olika arbetsbelastningar ange RU/m flexibilitet i det här skedet.</span><span class="sxs-lookup"><span data-stu-id="db5b6-158">During the development stage, if you are testing your application with different workloads, RU/m can provide the flexibility at this stage.</span></span> <span data-ttu-id="db5b6-159">När den [emulatorn](local-emulator.md) är en bra kostnadsfria verktyg för att testa Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="db5b6-159">While the [emulator](local-emulator.md) is a great free tool to test Azure Cosmos DB.</span></span> <span data-ttu-id="db5b6-160">Men om du vill starta i en molnmiljö behöver en stor flexibilitet med RU/m för dina behov för ad hoc-prestanda.</span><span class="sxs-lookup"><span data-stu-id="db5b6-160">However if you want to start in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="db5b6-161">Du kan göra fler inställningar som utvecklar mindre oroa prestandabehov först.</span><span class="sxs-lookup"><span data-stu-id="db5b6-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="db5b6-162">Vi rekommenderar att du börjar etablering av minsta RU/s och aktivera RU/m.</span><span class="sxs-lookup"><span data-stu-id="db5b6-162">We recommend starting with the minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="db5b6-163">**Oväntade, spiky, minut granularitet måste:** passar bra – besparingar: 25 75%.</span><span class="sxs-lookup"><span data-stu-id="db5b6-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="db5b6-164">Vi har sett stora improvement från RU/m och de flesta scenarier med produktion finns i den gruppen.</span><span class="sxs-lookup"><span data-stu-id="db5b6-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="db5b6-165">Om du har en IoT-arbetsbelastning som har topp några gånger i en minut, om du har frågor som körs när systemet gör masslagring insert samtidigt behöver extra kapacitet för handeling spiky behov.</span><span class="sxs-lookup"><span data-stu-id="db5b6-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at the same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="db5b6-166">Vi rekommenderar att optimera din resursbehov genom att använda vår metod för steg-för-steg nedan.</span><span class="sxs-lookup"><span data-stu-id="db5b6-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="db5b6-168">*Bild - RU förbrukning prestandamått*</span><span class="sxs-lookup"><span data-stu-id="db5b6-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="db5b6-169">**Sinnesro:** passar bra – besparingar: 10-20%.</span><span class="sxs-lookup"><span data-stu-id="db5b6-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="db5b6-170">Ibland kan du vill bara ha sinnesro och bry dig inte om potentiella toppar och begränsning.</span><span class="sxs-lookup"><span data-stu-id="db5b6-170">Sometimes, you just want to have peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="db5b6-171">Den här funktionen är rätt för dig.</span><span class="sxs-lookup"><span data-stu-id="db5b6-171">This feature is the right one for you.</span></span> <span data-ttu-id="db5b6-172">I så fall rekommenderar vi att aktivera RU/m och något lägre din per andra etablering.</span><span class="sxs-lookup"><span data-stu-id="db5b6-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="db5b6-173">Det här fallet skiljer sig från ovanstående som du inte försöka optimera aggressivt din etablering.</span><span class="sxs-lookup"><span data-stu-id="db5b6-173">This case is different from the above as you will not try to optimize aggressively your provisioning.</span></span> <span data-ttu-id="db5b6-174">Det här är mer av ett ”noll begränsning” tänkesätt du befinner dig i.</span><span class="sxs-lookup"><span data-stu-id="db5b6-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="db5b6-175">Kritiska åtgärder med ad hoc behov: ibland rekommenderas så att endast kritiska åtgärder åtkomst RU/m budget så budget inte får använda av ad hoc eller mindre viktiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="db5b6-175">Critical operations with adhoc needs: We sometimes recommend to only let critical operations access RU/m budget so the budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="db5b6-176">Som kan definieras enkelt i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="db5b6-176">That can be easily defined in the section below.</span></span>

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a><span data-ttu-id="db5b6-177">Använda portalen mått för att optimera kostnad och prestanda</span><span class="sxs-lookup"><span data-stu-id="db5b6-177">Using the portal metrics to optimize cost and performance</span></span>

<span data-ttu-id="db5b6-178">**Inom några veckor kommer vi ytterligare utveckla innehållet runt övervakning RUs minut förbrukningen för att optimera dina behov för genomströmning.**</span><span class="sxs-lookup"><span data-stu-id="db5b6-178">**In the coming weeks, we will further develop the content around monitoring RUs minute consumption to optimize your throughput needs.**</span></span>

<span data-ttu-id="db5b6-179">Du kan se hur mycket vanlig RU sekunder du använder jämfört med RU minuter via portalen mått.</span><span class="sxs-lookup"><span data-stu-id="db5b6-179">Through the portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="db5b6-180">Övervaka de här måtten hjälper dig att optimera din etablering.</span><span class="sxs-lookup"><span data-stu-id="db5b6-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="db5b6-181">Vi rekommenderar en steg-för-steg-metod för hur du använder RU/m till din fördel.</span><span class="sxs-lookup"><span data-stu-id="db5b6-181">We recommend a step by step approach on how to use RU/m to your advantage.</span></span> <span data-ttu-id="db5b6-182">För varje steg du bör ha en översikt över förbrukningen RU som representerar en fullständig cykel för din arbetsbelastning (det kan vara timmar, dagar, eller även veckor) och få insikter om användningen av du etablera.</span><span class="sxs-lookup"><span data-stu-id="db5b6-182">For each step, you should have an overview of the RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on the utilization of what you provision.</span></span>

<span data-ttu-id="db5b6-183">Principen bakom den här metoden är att din genomströmning etablering så nära som möjligt till en allokering som matchar sökvillkoren prestanda.</span><span class="sxs-lookup"><span data-stu-id="db5b6-183">The principle behind this approach is to make your throughput provisioning as close as possible to a provisioning point that matches your performance criteria below.</span></span> 

![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="db5b6-185">För att förstå den optimala etablering punkten för din arbetsbelastning, måste du förstå:</span><span class="sxs-lookup"><span data-stu-id="db5b6-185">To understand the optimal provisioning point for your workload, you need to understand:</span></span>

* <span data-ttu-id="db5b6-186">Förbrukning mönster: Nej, ovanligt eller varaktigt toppar?</span><span class="sxs-lookup"><span data-stu-id="db5b6-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="db5b6-187">Liten (2 x medelvärde), medel eller stor (> 10 x medelvärde) toppar?</span><span class="sxs-lookup"><span data-stu-id="db5b6-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="db5b6-188">Procent av begäranden för begränsad: känner du föredrar att om du har lite begränsning?</span><span class="sxs-lookup"><span data-stu-id="db5b6-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="db5b6-189">I så fall, med hur mycket?</span><span class="sxs-lookup"><span data-stu-id="db5b6-189">If so, by how much?</span></span> 

<span data-ttu-id="db5b6-190">När du har identifierat dina mål är kan kommer du att kunna hämta närmast optimal etableringen.</span><span class="sxs-lookup"><span data-stu-id="db5b6-190">Once you have identified what your goals are, you will be able to get closer to the optimal provisioning.</span></span>

<span data-ttu-id="db5b6-191">För att hjälpa dig som vi vill ge ett övergripande riktlinjer för hur du optimerar dina etablering enligt RU/m användning.</span><span class="sxs-lookup"><span data-stu-id="db5b6-191">To assist you, we want to provide an overall guidance on how to optimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="db5b6-192">Den här vägledningen gäller inte för alla typer av arbetsbelastningar men baserat på kunskap privat förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="db5b6-192">This guidance doesn’t apply to all kind of workloads but is based on the private preview knowledge.</span></span> <span data-ttu-id="db5b6-193">Vi kan ändra dessa baslinjer som vi Läs mer:</span><span class="sxs-lookup"><span data-stu-id="db5b6-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="db5b6-194">RU/m % användning</span><span class="sxs-lookup"><span data-stu-id="db5b6-194">RU/m % utilization</span></span>|<span data-ttu-id="db5b6-195">Grad av användning av RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="db5b6-196">Rekommenderade åtgärder för etablering</span><span class="sxs-lookup"><span data-stu-id="db5b6-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="db5b6-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="db5b6-197">0-1%</span></span>|<span data-ttu-id="db5b6-198">Under användning</span><span class="sxs-lookup"><span data-stu-id="db5b6-198">Under utilization</span></span>|<span data-ttu-id="db5b6-199">Lägre RU/s förbruka mer RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-199">Lower RU/s to consume more RU/m</span></span>|
|<span data-ttu-id="db5b6-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="db5b6-200">1-10%</span></span>|<span data-ttu-id="db5b6-201">Felfri användning</span><span class="sxs-lookup"><span data-stu-id="db5b6-201">Healthy use</span></span>|<span data-ttu-id="db5b6-202">Behåll samma etablering nivå</span><span class="sxs-lookup"><span data-stu-id="db5b6-202">Keep the same provisioning level</span></span>|
|<span data-ttu-id="db5b6-203">Över 10%</span><span class="sxs-lookup"><span data-stu-id="db5b6-203">Above 10%</span></span>|<span data-ttu-id="db5b6-204">Överbelastning</span><span class="sxs-lookup"><span data-stu-id="db5b6-204">Over utilization</span></span>|<span data-ttu-id="db5b6-205">Öka RU/s för att förlita sig mindre på RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-205">Increase RU/s to rely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-the-rum-budget"></a><span data-ttu-id="db5b6-206">Välj vilka aktiviteter kan förbruka budget RU/m</span><span class="sxs-lookup"><span data-stu-id="db5b6-206">Select which operations can consume the RU/m budget</span></span>

<span data-ttu-id="db5b6-207">På begäran-nivå, du kan också aktivera och inaktivera RU/m budget för att hantera begäran oavsett åtgärd.</span><span class="sxs-lookup"><span data-stu-id="db5b6-207">At request level, you can also enable/disable RU/m budget to serve the request irrespective of operation type.</span></span> <span data-ttu-id="db5b6-208">Om används reguljära etablerade RUs per sekund budget och begäran kan inte använda RU/m budget, kommer att begränsas denna begäran.</span><span class="sxs-lookup"><span data-stu-id="db5b6-208">If regular provisioned RUs/sec budget is consumed and the request cannot consume the RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="db5b6-209">Som standard är varje begäran hanteras av RU/m budget om RU/m genomströmning budget är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="db5b6-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="db5b6-210">Här är ett kodfragment för att inaktivera RU/m budget med DocumentDB-API för CRUD- och fråga.</span><span class="sxs-lookup"><span data-stu-id="db5b6-210">Here is a code snippet for disabling RU/m budget using the DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="db5b6-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db5b6-211">Next steps</span></span>

<span data-ttu-id="db5b6-212">Vi har i den här artikeln beskrivs hur partitionering fungerar i Azure Cosmos DB, hur du kan skapa partitionerade samlingar och hur du kan välja en bra partitionsnyckel för ditt program.</span><span class="sxs-lookup"><span data-stu-id="db5b6-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="db5b6-213">Utföra skalnings- och prestandatester med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="db5b6-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="db5b6-214">Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.</span><span class="sxs-lookup"><span data-stu-id="db5b6-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="db5b6-215">Komma igång med programmeringen med den [SDK](documentdb-sdk-dotnet.md) eller [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="db5b6-215">Get started coding with the [SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="db5b6-216">Lär dig mer om [etablerat dataflöde](request-units.md) i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="db5b6-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

