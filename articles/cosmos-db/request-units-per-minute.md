---
title: "Azure CosmosDB: Programbegäran per minut (RU/m) | Microsoft Docs"
description: "Lär dig hur tooreduce kostnader genom att använda enheter för programbegäran per minut."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="a940d-103">Frågeenheter per minut i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a940d-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="a940d-104">Azure Cosmos-DB är utformad toohelp du få en snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program tillväxt.</span><span class="sxs-lookup"><span data-stu-id="a940d-104">Azure Cosmos DB is designed toohelp you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="a940d-105">Du kan etablera dataflödet på en Cosmos-DB-behållare på båda, per sekund och granulariteter per minut (RU/m).</span><span class="sxs-lookup"><span data-stu-id="a940d-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="a940d-106">hello dataflöde på per minut Granularitet är används toomanage oväntat toppar i hello arbetsbelastning som äger rum med en kornighet per sekund.</span><span class="sxs-lookup"><span data-stu-id="a940d-106">hello provisioned throughput at per-minute granularity is used toomanage unexpected spikes in hello workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="a940d-107">Den här artikeln innehåller en översikt över hur hello etablering av Frågeenhet per minut (RU/m) fungerar.</span><span class="sxs-lookup"><span data-stu-id="a940d-107">This article provides an overview of how hello provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="a940d-108">hello mål i åtanke med etablering av RU/m är tooprovide en förutsägbar prestanda runt oförutsägbart behov (särskilt om du behöver toorun analytics ovanpå operational data) och spiky arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="a940d-108">hello goal in mind with provisioning of RU/m is tooprovide a predictable performance around unpredictable needs (especially if you need toorun analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="a940d-109">Vi vill toohave våra kunder förbruka mer hello genomströmning de konfigurerar så att de kan skalas snabbt med sinnesro.</span><span class="sxs-lookup"><span data-stu-id="a940d-109">We want toohave our customers consume more hello throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="a940d-110">När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="a940d-110">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="a940d-111">Hur fungerar en Frågeenhet per minut?</span><span class="sxs-lookup"><span data-stu-id="a940d-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="a940d-112">Vad är hello skillnaden mellan Frågeenhet per minut och Frågeenhet per sekund?</span><span class="sxs-lookup"><span data-stu-id="a940d-112">What is hello difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="a940d-113">Hur tooprovision RU/m?</span><span class="sxs-lookup"><span data-stu-id="a940d-113">How tooprovision RU/m?</span></span>
* <span data-ttu-id="a940d-114">Under vilket scenario beakta jag etablering Frågeenhet per minut?</span><span class="sxs-lookup"><span data-stu-id="a940d-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="a940d-115">Hur hello toouse portal mått toooptimize min kostnad och prestanda?</span><span class="sxs-lookup"><span data-stu-id="a940d-115">How toouse hello portal metrics toooptimize my cost and performance?</span></span>
* <span data-ttu-id="a940d-116">Definiera vilken typ av begäran kan använda din RU/m budget?</span><span class="sxs-lookup"><span data-stu-id="a940d-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="a940d-117">Etablering av frågeenheter per minut (RU/m)</span><span class="sxs-lookup"><span data-stu-id="a940d-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="a940d-118">När du etablerar Azure Cosmos DB på hello andra granularitet (RU/s) kan du hämta hello garanti för att din begäran lyckas på en låg latens om din genomströmning inte har överstigit hello kapacitet etablerats i den andra.</span><span class="sxs-lookup"><span data-stu-id="a940d-118">When you provision Azure Cosmos DB at hello second granularity (RU/s), you get hello guarantee that your request succeeds at a low latency if your throughput has not exceeded hello capacity provisioned within that second.</span></span> <span data-ttu-id="a940d-119">Med RU/m är hello granularitet på hello minut med hello garanti för att begäran slutförs inom den minuten.</span><span class="sxs-lookup"><span data-stu-id="a940d-119">With RU/m, hello granularity is at hello minute with hello guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="a940d-120">Jämfört med toobursting system kan vi se till att hello prestanda som du får är förutsägbar och du kan planera på den.</span><span class="sxs-lookup"><span data-stu-id="a940d-120">Compared toobursting systems, we make sure that hello performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="a940d-121">hello sätt per minut etablering fungerar är enkel:</span><span class="sxs-lookup"><span data-stu-id="a940d-121">hello way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="a940d-122">RU/m faktureras timvis och tillägg tooRU/s.</span><span class="sxs-lookup"><span data-stu-id="a940d-122">RU/m is billed hourly and in addition tooRU/s.</span></span> <span data-ttu-id="a940d-123">Mer information finns i Azure Cosmos DB [sida med priser](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="a940d-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="a940d-124">RU/m kan aktiveras på samlingsnivå.</span><span class="sxs-lookup"><span data-stu-id="a940d-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="a940d-125">Som kan göras via hello SDK: er (Node.js, Java eller .net) eller hello-portal (också inkludera MongoDB API arbetsbelastningar)</span><span class="sxs-lookup"><span data-stu-id="a940d-125">That can be done through hello SDKs (Node.js, Java, or .Net) or through hello portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="a940d-126">När RU/m är aktiverat för varje 100 RU/s etableras, också få 1 000 RU/m etablerats (hello förhållandet är 10 x)</span><span class="sxs-lookup"><span data-stu-id="a940d-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (hello ratio is 10x)</span></span>
* <span data-ttu-id="a940d-127">Vid en given andra förbrukar en begäran enhet din RU/m etablering endast om du har överskridit din per andra etablering i den andra</span><span class="sxs-lookup"><span data-stu-id="a940d-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="a940d-128">En gång Hej 60-sekunders tid (UTC) avslutas, hello per minut etablering fyllas</span><span class="sxs-lookup"><span data-stu-id="a940d-128">Once hello 60-second period (UTC) ends, hello per minute provisioning is refilled</span></span>
* <span data-ttu-id="a940d-129">RU/m kan aktiveras endast för samlingar med en maximal etablering av 5 000 RU/s per partition.</span><span class="sxs-lookup"><span data-stu-id="a940d-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="a940d-130">Om du skala dina behov av genomflöde och har en hög nivå av etablering per partition, får du ett varningsmeddelande</span><span class="sxs-lookup"><span data-stu-id="a940d-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="a940d-131">Nedan ett konkreta exempel, där en kund kan etablera 10kRU/s med 100kRU/m, sparar 73% kostnaden mot etablering för belastning (på 50kRU per sekund) via en 90 sekunder period på en samling som har 10 000 RU/s och 100 000 RU/m etableras:</span><span class="sxs-lookup"><span data-stu-id="a940d-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="a940d-132">1 sekund: hello RU/m budget är inställt på 100 000</span><span class="sxs-lookup"><span data-stu-id="a940d-132">1st second: hello RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="a940d-133">3: e sekund: under den andra hello förbrukningen av Frågeenhet var 11,010 RUs, 1,010 RUs ovan hello RU/s-etablering.</span><span class="sxs-lookup"><span data-stu-id="a940d-133">3rd second: During that second hello consumption of Request Unit was 11,010 RUs, 1,010 RUs above hello RU/s provisioning.</span></span> <span data-ttu-id="a940d-134">Därför dras 1,010 RUs från hello RU/m budget.</span><span class="sxs-lookup"><span data-stu-id="a940d-134">Therefore, 1,010 RUs are deducted from hello RU/m budget.</span></span> <span data-ttu-id="a940d-135">98,990 RUs är tillgängliga för hello nästa 57 sekunder i hello RU/m budget</span><span class="sxs-lookup"><span data-stu-id="a940d-135">98,990 RUs are available for hello next 57 seconds in hello RU/m budget</span></span>
* <span data-ttu-id="a940d-136">29: e sekund: under den andra stora topp har hänt (> 4 x som är högre än etablering per sekund) och hello förbrukningen av Frågeenhet var 46,920 RUs.</span><span class="sxs-lookup"><span data-stu-id="a940d-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and hello consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="a940d-137">36,920 RUs dras från hello RU/m budget som tagits bort från 92,323 RUs (28 sekund) too55, 403 RUs (29: e sekund)</span><span class="sxs-lookup"><span data-stu-id="a940d-137">36,920 RUs are deducted from hello RU/m budget that dropped from 92,323 RUs (28th second) too55,403 RUs (29th second)</span></span>
* <span data-ttu-id="a940d-138">61st sekund: RU/m budget anges tillbaka too100 000 RUs.</span><span class="sxs-lookup"><span data-stu-id="a940d-138">61st second: RU/m budget is set back too100,000 RUs.</span></span>
 
![Diagram som visar hello förbrukning och etablering av Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="a940d-140">Ange kapacitet för begäran-enhet med RU/m</span><span class="sxs-lookup"><span data-stu-id="a940d-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="a940d-141">När du skapar en Azure DB som Cosmos-samling kan du ange hello många frågeenheter per sekund (RU per sekund) som du vill reserverade för hello samling.</span><span class="sxs-lookup"><span data-stu-id="a940d-141">When creating an Azure Cosmos DB collection, you specify hello number of request units per second (RU per second) you want reserved for hello collection.</span></span> <span data-ttu-id="a940d-142">Du kan också bestämma om du vill tooadd RU per minut.</span><span class="sxs-lookup"><span data-stu-id="a940d-142">You can also decide if you want tooadd RU per minute.</span></span> <span data-ttu-id="a940d-143">Detta kan göras via hello portalen eller hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a940d-143">This can be done through hello Portal or hello SDK.</span></span> 

### <a name="through-hello-portal"></a><span data-ttu-id="a940d-144">Via hello Portal</span><span class="sxs-lookup"><span data-stu-id="a940d-144">Through hello Portal</span></span>

<span data-ttu-id="a940d-145">Aktivera eller inaktivera RU per minut kräver bara ett klick vid etablering av en samling.</span><span class="sxs-lookup"><span data-stu-id="a940d-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Skärmbild som visar hur tooset RU/m i hello Azure-portalen](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a><span data-ttu-id="a940d-147">Via hello SDK</span><span class="sxs-lookup"><span data-stu-id="a940d-147">Through hello SDK</span></span>
<span data-ttu-id="a940d-148">Först, det är viktigt toonote RU/m är endast tillgängligt för följande SDK hello:</span><span class="sxs-lookup"><span data-stu-id="a940d-148">First, this is important toonote that RU/m is only available for hello following SDKs:</span></span>

* <span data-ttu-id="a940d-149">.NET 1.14.0</span><span class="sxs-lookup"><span data-stu-id="a940d-149">.Net 1.14.0</span></span>
* <span data-ttu-id="a940d-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a940d-150">Java 1.11.0</span></span>
* <span data-ttu-id="a940d-151">Node.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="a940d-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="a940d-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="a940d-152">Python 2.2.0</span></span>

<span data-ttu-id="a940d-153">Här är ett kodfragment för att skapa en samling med 3 000 frågeenheter per andra och 30 000 frågeenheter per minut med hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="a940d-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using hello .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="a940d-154">Här är ett kodfragment för att ändra hello genomströmningen i en samling too5, 000 frågeenheter per sekund utan etablering RU per minut med hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="a940d-154">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second without provisioning RU per minute using hello .NET SDK:</span></span>

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="a940d-155">Bra passa scenarier</span><span class="sxs-lookup"><span data-stu-id="a940d-155">Good fit scenarios</span></span>

<span data-ttu-id="a940d-156">I det här avsnittet ger vi en översikt över scenarier som passar bra för att aktivera frågeenheter per minut.</span><span class="sxs-lookup"><span data-stu-id="a940d-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="a940d-157">**Miljö för utveckling och testning:** passar bra.</span><span class="sxs-lookup"><span data-stu-id="a940d-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="a940d-158">Hello development etapp, om du vill testa ditt program med olika arbetsbelastningar ange RU/m hello flexibilitet i det här skedet.</span><span class="sxs-lookup"><span data-stu-id="a940d-158">During hello development stage, if you are testing your application with different workloads, RU/m can provide hello flexibility at this stage.</span></span> <span data-ttu-id="a940d-159">När hello [emulatorn](local-emulator.md) är en bra gratisverktyg tootest Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a940d-159">While hello [emulator](local-emulator.md) is a great free tool tootest Azure Cosmos DB.</span></span> <span data-ttu-id="a940d-160">Om du vill toostart i en molnmiljö kan du men har en stor flexibilitet med RU/m för dina behov för ad hoc-prestanda.</span><span class="sxs-lookup"><span data-stu-id="a940d-160">However if you want toostart in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="a940d-161">Du kan göra fler inställningar som utvecklar mindre oroa prestandabehov först.</span><span class="sxs-lookup"><span data-stu-id="a940d-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="a940d-162">Vi rekommenderar att du börjar med hello minsta RU/s-etablering och aktivera RU/m.</span><span class="sxs-lookup"><span data-stu-id="a940d-162">We recommend starting with hello minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="a940d-163">**Oväntade, spiky, minut granularitet måste:** passar bra – besparingar: 25 75%.</span><span class="sxs-lookup"><span data-stu-id="a940d-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="a940d-164">Vi har sett stora improvement från RU/m och de flesta scenarier med produktion finns i den gruppen.</span><span class="sxs-lookup"><span data-stu-id="a940d-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="a940d-165">Om du har en IoT-arbetsbelastning som har topp några gånger i en minut, om du har frågor som körs när systemet gör vikt infogningen hello samma tid och du behöver extra kapacitet för handeling spiky måste.</span><span class="sxs-lookup"><span data-stu-id="a940d-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at hello same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="a940d-166">Vi rekommenderar att optimera din resursbehov genom att använda vår metod för steg-för-steg nedan.</span><span class="sxs-lookup"><span data-stu-id="a940d-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="a940d-168">*Bild - RU förbrukning prestandamått*</span><span class="sxs-lookup"><span data-stu-id="a940d-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="a940d-169">**Sinnesro:** passar bra – besparingar: 10-20%.</span><span class="sxs-lookup"><span data-stu-id="a940d-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="a940d-170">Ibland kan du vill bara toohave sinnesro och bry dig inte om potentiella toppar och begränsning.</span><span class="sxs-lookup"><span data-stu-id="a940d-170">Sometimes, you just want toohave peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="a940d-171">Den här funktionen är hello rätt en för dig.</span><span class="sxs-lookup"><span data-stu-id="a940d-171">This feature is hello right one for you.</span></span> <span data-ttu-id="a940d-172">I så fall rekommenderar vi att aktivera RU/m och något lägre din per andra etablering.</span><span class="sxs-lookup"><span data-stu-id="a940d-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="a940d-173">Det här fallet skiljer sig från hello ovan som du inte försöker toooptimize aggressivt din etablering.</span><span class="sxs-lookup"><span data-stu-id="a940d-173">This case is different from hello above as you will not try toooptimize aggressively your provisioning.</span></span> <span data-ttu-id="a940d-174">Det här är mer av ett ”noll begränsning” tänkesätt du befinner dig i.</span><span class="sxs-lookup"><span data-stu-id="a940d-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="a940d-175">Kritiska åtgärder med ad hoc behov: Vi rekommenderar ibland tooonly kan kritiska åtgärder åtkomst RU/m budget så hello budget inte får använda av ad hoc eller mindre viktiga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a940d-175">Critical operations with adhoc needs: We sometimes recommend tooonly let critical operations access RU/m budget so hello budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="a940d-176">Som kan definieras enkelt i hello nedan.</span><span class="sxs-lookup"><span data-stu-id="a940d-176">That can be easily defined in hello section below.</span></span>

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a><span data-ttu-id="a940d-177">Med hjälp av hello portal mått toooptimize kostnad och prestanda</span><span class="sxs-lookup"><span data-stu-id="a940d-177">Using hello portal metrics toooptimize cost and performance</span></span>

<span data-ttu-id="a940d-178">**I hello några veckor, kommer vi ytterligare utveckla hello innehåll runt övervakning RUs minut förbrukning toooptimize din genomströmning måste.**</span><span class="sxs-lookup"><span data-stu-id="a940d-178">**In hello coming weeks, we will further develop hello content around monitoring RUs minute consumption toooptimize your throughput needs.**</span></span>

<span data-ttu-id="a940d-179">Via hello portal statistik och, kan du se hur mycket vanlig RU sekunder du använder jämfört med RU minuter.</span><span class="sxs-lookup"><span data-stu-id="a940d-179">Through hello portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="a940d-180">Övervaka de här måtten hjälper dig att optimera din etablering.</span><span class="sxs-lookup"><span data-stu-id="a940d-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="a940d-181">Vi rekommenderar en steg-för-steg-metod på hur toouse RU/m tooyour nytta.</span><span class="sxs-lookup"><span data-stu-id="a940d-181">We recommend a step by step approach on how toouse RU/m tooyour advantage.</span></span> <span data-ttu-id="a940d-182">För varje steg du bör ha en översikt över hello RU förbrukning som representerar en fullständig cykel för din arbetsbelastning (det kan vara timmar, dagar, eller även veckor) och få insikter om hello användning av du etablera.</span><span class="sxs-lookup"><span data-stu-id="a940d-182">For each step, you should have an overview of hello RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on hello utilization of what you provision.</span></span>

<span data-ttu-id="a940d-183">hello principen bakom den här metoden är toomake din genomströmning etablering som nära som möjligt tooa etablering som matchar sökvillkoren prestanda.</span><span class="sxs-lookup"><span data-stu-id="a940d-183">hello principle behind this approach is toomake your throughput provisioning as close as possible tooa provisioning point that matches your performance criteria below.</span></span> 

![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="a940d-185">toounderstand hello optimala etablering platsen för din arbetsbelastning behöver du toounderstand:</span><span class="sxs-lookup"><span data-stu-id="a940d-185">toounderstand hello optimal provisioning point for your workload, you need toounderstand:</span></span>

* <span data-ttu-id="a940d-186">Förbrukning mönster: Nej, ovanligt eller varaktigt toppar?</span><span class="sxs-lookup"><span data-stu-id="a940d-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="a940d-187">Liten (2 x medelvärde), medel eller stor (> 10 x medelvärde) toppar?</span><span class="sxs-lookup"><span data-stu-id="a940d-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="a940d-188">Procent av begäranden för begränsad: känner du föredrar att om du har lite begränsning?</span><span class="sxs-lookup"><span data-stu-id="a940d-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="a940d-189">I så fall, med hur mycket?</span><span class="sxs-lookup"><span data-stu-id="a940d-189">If so, by how much?</span></span> 

<span data-ttu-id="a940d-190">När du har identifierat dina mål är, kommer du att kunna tooget närmare toohello optimala etablering.</span><span class="sxs-lookup"><span data-stu-id="a940d-190">Once you have identified what your goals are, you will be able tooget closer toohello optimal provisioning.</span></span>

<span data-ttu-id="a940d-191">tooassist, vi vill tooprovide en allmän vägledning om hur toooptimize din etablering enligt RU/m användning.</span><span class="sxs-lookup"><span data-stu-id="a940d-191">tooassist you, we want tooprovide an overall guidance on how toooptimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="a940d-192">Den här vägledningen gäller inte tooall typer av arbetsbelastningar men baseras på hello privat förhandsversion kunskap.</span><span class="sxs-lookup"><span data-stu-id="a940d-192">This guidance doesn’t apply tooall kind of workloads but is based on hello private preview knowledge.</span></span> <span data-ttu-id="a940d-193">Vi kan ändra dessa baslinjer som vi Läs mer:</span><span class="sxs-lookup"><span data-stu-id="a940d-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="a940d-194">RU/m % användning</span><span class="sxs-lookup"><span data-stu-id="a940d-194">RU/m % utilization</span></span>|<span data-ttu-id="a940d-195">Grad av användning av RU/m</span><span class="sxs-lookup"><span data-stu-id="a940d-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="a940d-196">Rekommenderade åtgärder för etablering</span><span class="sxs-lookup"><span data-stu-id="a940d-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="a940d-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="a940d-197">0-1%</span></span>|<span data-ttu-id="a940d-198">Under användning</span><span class="sxs-lookup"><span data-stu-id="a940d-198">Under utilization</span></span>|<span data-ttu-id="a940d-199">Lägre RU/s tooconsume mer RU/m</span><span class="sxs-lookup"><span data-stu-id="a940d-199">Lower RU/s tooconsume more RU/m</span></span>|
|<span data-ttu-id="a940d-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="a940d-200">1-10%</span></span>|<span data-ttu-id="a940d-201">Felfri användning</span><span class="sxs-lookup"><span data-stu-id="a940d-201">Healthy use</span></span>|<span data-ttu-id="a940d-202">Hello Behåll samma etablering nivå</span><span class="sxs-lookup"><span data-stu-id="a940d-202">Keep hello same provisioning level</span></span>|
|<span data-ttu-id="a940d-203">Över 10%</span><span class="sxs-lookup"><span data-stu-id="a940d-203">Above 10%</span></span>|<span data-ttu-id="a940d-204">Överbelastning</span><span class="sxs-lookup"><span data-stu-id="a940d-204">Over utilization</span></span>|<span data-ttu-id="a940d-205">Öka RU/s toorely mindre på RU/m</span><span class="sxs-lookup"><span data-stu-id="a940d-205">Increase RU/s toorely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a><span data-ttu-id="a940d-206">Välj vilka aktiviteter kan förbruka hello RU/m budget</span><span class="sxs-lookup"><span data-stu-id="a940d-206">Select which operations can consume hello RU/m budget</span></span>

<span data-ttu-id="a940d-207">På begäran-nivå, du kan också aktivera och inaktivera RU/m budget tooserve hello begäran oavsett åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a940d-207">At request level, you can also enable/disable RU/m budget tooserve hello request irrespective of operation type.</span></span> <span data-ttu-id="a940d-208">Om används reguljära etablerade RUs per sekund budget och hello begäran kan inte använda hello RU/m budget, kommer att begränsas denna begäran.</span><span class="sxs-lookup"><span data-stu-id="a940d-208">If regular provisioned RUs/sec budget is consumed and hello request cannot consume hello RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="a940d-209">Som standard är varje begäran hanteras av RU/m budget om RU/m genomströmning budget är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a940d-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="a940d-210">Här är ett kodfragment för att inaktivera RU/m budget med hello DocumentDB API för CRUD- och fråga.</span><span class="sxs-lookup"><span data-stu-id="a940d-210">Here is a code snippet for disabling RU/m budget using hello DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="a940d-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a940d-211">Next steps</span></span>

<span data-ttu-id="a940d-212">Vi har i den här artikeln beskrivs hur partitionering fungerar i Azure Cosmos DB, hur du kan skapa partitionerade samlingar och hur du kan välja en bra partitionsnyckel för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a940d-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="a940d-213">Utföra skalnings- och prestandatester med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a940d-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="a940d-214">Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.</span><span class="sxs-lookup"><span data-stu-id="a940d-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="a940d-215">Komma igång med programmeringen med hello [SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="a940d-215">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="a940d-216">Lär dig mer om [etablerat dataflöde](request-units.md) i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a940d-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

