---
title: "aaaDocumentDB API prestandanivåer | Microsoft Docs"
description: "Läs mer om hur prestandanivåer för DocumentDB API aktivera tooreserve genomströmning på grundval av per behållare."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="7b52f-103">Ta bort hello S1, S2 och S3 prestandanivåer</span><span class="sxs-lookup"><span data-stu-id="7b52f-103">Retiring hello S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7b52f-104">hello S1, S2 och S3 prestandanivåer i den här artikeln är som har återkallats och är inte längre tillgängliga för nya DocumentDB API-konton.</span><span class="sxs-lookup"><span data-stu-id="7b52f-104">hello S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="7b52f-105">Den här artikeln innehåller en översikt över S1, S2 och S3 prestandanivåer och beskriver hur hello samlingar som använder dessa prestandanivåer kommer att migrerade toosingle partitionssamlingar på 1 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="7b52f-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how hello collections that use these performance levels will be migrated toosingle partition collections on August 1st, 2017.</span></span> <span data-ttu-id="7b52f-106">När du har läst den här artikeln att du kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="7b52f-106">After reading this article, you'll be able tooanswer hello following questions:</span></span>

- [<span data-ttu-id="7b52f-107">Varför är hello S1, S2 och S3 prestanda nivåer som har återkallats?</span><span class="sxs-lookup"><span data-stu-id="7b52f-107">Why are hello S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="7b52f-108">Hur enskilda partitionssamlingar och partitionerade samlingar jämföra toohello S1, S2, prestandanivåer S3?</span><span class="sxs-lookup"><span data-stu-id="7b52f-108">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="7b52f-109">Vad gör jag behöver toodo tooensure oavbrutet dataåtkomst toomy?</span><span class="sxs-lookup"><span data-stu-id="7b52f-109">What do I need toodo tooensure uninterrupted access toomy data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="7b52f-110">Hur ändrar min samlingen efter hello migreringen?</span><span class="sxs-lookup"><span data-stu-id="7b52f-110">How will my collection change after hello migration?</span></span>](#collection-change)
- [<span data-ttu-id="7b52f-111">Hur min fakturering ändras när jag migrerade toosingle partitionssamlingar?</span><span class="sxs-lookup"><span data-stu-id="7b52f-111">How will my billing change after I’m migrated toosingle partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="7b52f-112">Vad händer om jag behöver fler än 10 GB lagringsutrymme?</span><span class="sxs-lookup"><span data-stu-id="7b52f-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="7b52f-113">Kan jag ändra mellan hello S1, S2 och S3 prestandanivåer före den 1 augusti 2017?</span><span class="sxs-lookup"><span data-stu-id="7b52f-113">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="7b52f-114">Hur vet jag om min samling har migrerats?</span><span class="sxs-lookup"><span data-stu-id="7b52f-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="7b52f-115">Hur migrera från hello S1, S2 partitionssamlingar för S3 prestanda nivåer toosingle själv?</span><span class="sxs-lookup"><span data-stu-id="7b52f-115">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="7b52f-116">Hur kan jag påverkas om jag en EA-kund?</span><span class="sxs-lookup"><span data-stu-id="7b52f-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="7b52f-117">Varför är hello S1, S2 och S3 prestanda nivåer som har återkallats?</span><span class="sxs-lookup"><span data-stu-id="7b52f-117">Why are hello S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="7b52f-118">hello S1, S2 och S3 prestandanivåer gör inte ger hello flexibilitet som erbjuder DocumentDB API samlingar.</span><span class="sxs-lookup"><span data-stu-id="7b52f-118">hello S1, S2, and S3 performance levels do not offer hello flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="7b52f-119">Med hello S1, S2 S3 prestandanivåer båda hello genomflöde och lagringskapaciteten har redan angetts och erbjöd inte elasticitet.</span><span class="sxs-lookup"><span data-stu-id="7b52f-119">With hello S1, S2, S3 performance levels, both hello throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="7b52f-120">Azure Cosmos-DB erbjuder hello möjlighet toocustomize din dataflöden och lagringsutrymmen, erbjuder mycket mer flexibilitet i din möjlighet tooscale eftersom dina behov förändras.</span><span class="sxs-lookup"><span data-stu-id="7b52f-120">Azure Cosmos DB now offers hello ability toocustomize your throughput and storage, offering you much more flexibility in your ability tooscale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a><span data-ttu-id="7b52f-121">Hur enskilda partitionssamlingar och partitionerade samlingar jämföra toohello S1, S2, prestandanivåer S3?</span><span class="sxs-lookup"><span data-stu-id="7b52f-121">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="7b52f-122">hello följande tabell jämförs hello dataflöden och lagringsutrymmen alternativen i enskilda partitionssamlingar partitionerade samlingar och S1, S2 S3 prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="7b52f-122">hello following table compares hello throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="7b52f-123">Här är ett exempel på oss östra 2 region:</span><span class="sxs-lookup"><span data-stu-id="7b52f-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="7b52f-124">Partitionerad samling</span><span class="sxs-lookup"><span data-stu-id="7b52f-124">Partitioned collection</span></span>|<span data-ttu-id="7b52f-125">Enskild partition samling</span><span class="sxs-lookup"><span data-stu-id="7b52f-125">Single partition collection</span></span>|<span data-ttu-id="7b52f-126">S1</span><span class="sxs-lookup"><span data-stu-id="7b52f-126">S1</span></span>|<span data-ttu-id="7b52f-127">S2</span><span class="sxs-lookup"><span data-stu-id="7b52f-127">S2</span></span>|<span data-ttu-id="7b52f-128">S3</span><span class="sxs-lookup"><span data-stu-id="7b52f-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="7b52f-129">Maximalt dataflöde</span><span class="sxs-lookup"><span data-stu-id="7b52f-129">Maximum throughput</span></span>|<span data-ttu-id="7b52f-130">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="7b52f-130">Unlimited</span></span>|<span data-ttu-id="7b52f-131">10 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-131">10K RU/s</span></span>|<span data-ttu-id="7b52f-132">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-132">250 RU/s</span></span>|<span data-ttu-id="7b52f-133">1 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-133">1 K RU/s</span></span>|<span data-ttu-id="7b52f-134">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="7b52f-135">Minsta dataflöde</span><span class="sxs-lookup"><span data-stu-id="7b52f-135">Minimum throughput</span></span>|<span data-ttu-id="7b52f-136">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-136">2.5K RU/s</span></span>|<span data-ttu-id="7b52f-137">400 RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-137">400 RU/s</span></span>|<span data-ttu-id="7b52f-138">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-138">250 RU/s</span></span>|<span data-ttu-id="7b52f-139">1 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-139">1 K RU/s</span></span>|<span data-ttu-id="7b52f-140">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="7b52f-141">Maximalt lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="7b52f-141">Maximum storage</span></span>|<span data-ttu-id="7b52f-142">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="7b52f-142">Unlimited</span></span>|<span data-ttu-id="7b52f-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-143">10 GB</span></span>|<span data-ttu-id="7b52f-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-144">10 GB</span></span>|<span data-ttu-id="7b52f-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-145">10 GB</span></span>|<span data-ttu-id="7b52f-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-146">10 GB</span></span>|
|<span data-ttu-id="7b52f-147">Pris (månad)</span><span class="sxs-lookup"><span data-stu-id="7b52f-147">Price (monthly)</span></span>|<span data-ttu-id="7b52f-148">Genomströmning: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="7b52f-149">Lagring: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="7b52f-150">Genomströmning: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="7b52f-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="7b52f-151">Lagring: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="7b52f-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="7b52f-152">$25 USD</span><span class="sxs-lookup"><span data-stu-id="7b52f-152">$25 USD</span></span>|<span data-ttu-id="7b52f-153">$50 USD</span><span class="sxs-lookup"><span data-stu-id="7b52f-153">$50 USD</span></span>|<span data-ttu-id="7b52f-154">$100 USD</span><span class="sxs-lookup"><span data-stu-id="7b52f-154">$100 USD</span></span>|

<span data-ttu-id="7b52f-155">Är du en kund med EA?</span><span class="sxs-lookup"><span data-stu-id="7b52f-155">Are you an EA customer?</span></span> <span data-ttu-id="7b52f-156">I så fall, finns [hur får jag påverkas om jag en EA-kund?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="7b52f-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a><span data-ttu-id="7b52f-157">Vad gör jag behöver toodo tooensure oavbrutet dataåtkomst toomy?</span><span class="sxs-lookup"><span data-stu-id="7b52f-157">What do I need toodo tooensure uninterrupted access toomy data?</span></span>

<span data-ttu-id="7b52f-158">Något, Cosmos DB hanterar hello-migreringen.</span><span class="sxs-lookup"><span data-stu-id="7b52f-158">Nothing, Cosmos DB handles hello migration for you.</span></span> <span data-ttu-id="7b52f-159">Om du har en samling S1, S2 och S3, blir din samling samling för migrerade tooa enskild partition på den 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="7b52f-159">If you have an S1, S2, or S3 collection, your current collection will be migrated tooa single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a><span data-ttu-id="7b52f-160">Hur ändrar min samlingen efter hello migreringen?</span><span class="sxs-lookup"><span data-stu-id="7b52f-160">How will my collection change after hello migration?</span></span>

<span data-ttu-id="7b52f-161">Om du har en samling S1 kommer du att migrerade tooa enda partition samling med 400 RU/s genomströmning.</span><span class="sxs-lookup"><span data-stu-id="7b52f-161">If you have an S1 collection, you will be migrated tooa single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="7b52f-162">400 RU/s är hello lägsta genomströmning tillgänglig med enskilda partitionssamlingar.</span><span class="sxs-lookup"><span data-stu-id="7b52f-162">400 RU/s is hello lowest throughput available with single partition collections.</span></span> <span data-ttu-id="7b52f-163">Dock hello hello kostnaden för 400 RU/s i en enda partition samling är ungefär hello samma som du betalar med S1 samlingen hello och 250 RU/s – så att du inte betalar för extra 150 RU/s tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="7b52f-163">However, hello cost for 400 RU/s in hello a single partition collection is approximately hello same as you were paying with your S1 collection and 250 RU/s – so you are not paying for hello extra 150 RU/s available tooyou.</span></span>

<span data-ttu-id="7b52f-164">Om du har en S2 samling, kommer du att migrerade tooa enda partition samling med 1 K RU/s.</span><span class="sxs-lookup"><span data-stu-id="7b52f-164">If you have an S2 collection, you will be migrated tooa single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="7b52f-165">Ingen ändring tooyour genomströmning nivå visas.</span><span class="sxs-lookup"><span data-stu-id="7b52f-165">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="7b52f-166">Om du har en samling S3, kommer du att migrerade tooa enda partition samling med 2,5 K RU/s.</span><span class="sxs-lookup"><span data-stu-id="7b52f-166">If you have an S3 collection, you will be migrated tooa single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="7b52f-167">Ingen ändring tooyour genomströmning nivå visas.</span><span class="sxs-lookup"><span data-stu-id="7b52f-167">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="7b52f-168">I dessa fall migrerat samlingen ska du kan toocustomize genomströmning-nivå, eller skala den uppåt och nedåt som behövs tooprovide låg latens åtkomst tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="7b52f-168">In each of these cases, after your collection is migrated, you will be able toocustomize your throughput level, or scale it up and down as needed tooprovide low-latency access tooyour users.</span></span> <span data-ttu-id="7b52f-169">toochange hello genomströmning nivå när din samling har migrerats helt enkelt öppna Cosmos-DB-konto i hello Azure-portalen, klickar du på skala, Välj din samling och sedan justera hello genomströmning nivå, som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="7b52f-169">toochange hello throughput level after your collection has migrated, simply open your Cosmos DB account in hello Azure portal, click Scale, choose your collection, and then adjust hello throughput level, as shown in hello following screenshot:</span></span>

![Hur tooscale genomflöde i hello Azure-portalen](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a><span data-ttu-id="7b52f-171">Hur min fakturering ändras när jag migrerade toohello enskilda partitionssamlingar?</span><span class="sxs-lookup"><span data-stu-id="7b52f-171">How will my billing change after I’m migrated toohello single partition collections?</span></span>

<span data-ttu-id="7b52f-172">Om du har 10 S1 samlingar, 1 GB lagringsutrymme i hello oss Öst region, och du migrerar de här 10 S1 samlingar too10 enskilda partitionssamlingar på 400 RU/s (hello lägsta nivå).</span><span class="sxs-lookup"><span data-stu-id="7b52f-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in hello US East region, and you migrate these 10 S1 collections too10 single partition collections at 400 RU/sec (hello minimum level).</span></span> <span data-ttu-id="7b52f-173">Fakturan ska se ut så här om du behåller hello 10 enskilda partitionssamlingar för en hel månad:</span><span class="sxs-lookup"><span data-stu-id="7b52f-173">Your bill will look as follows if you keep hello 10 single partition collections for a full month:</span></span>

![Hur S1 priser för 10 samlingar jämför too10 samlingar med priser för en enskild partition samling](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="7b52f-175">Vad händer om jag behöver fler än 10 GB lagringsutrymme?</span><span class="sxs-lookup"><span data-stu-id="7b52f-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="7b52f-176">Om du har en samling med en S1, S2 och S3 prestandanivå eller ha en enda partition samling, 10 GB tillgängligt lagringsutrymme som alla har kan du använda hello Cosmos DB datamigrering verktyget toomigrate data-tooa partitionerad samling med princip obegränsad lagring.</span><span class="sxs-lookup"><span data-stu-id="7b52f-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use hello Cosmos DB Data Migration tool toomigrate your data tooa partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="7b52f-177">Information om hello fördelar för en partitionerad samling finns [partitionering och skalning i Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="7b52f-177">For information about hello benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="7b52f-178">Information om hur toomigrate din S1 S2, S3 eller enskild partition samling tooa partitionerad samling, se [migrerar från en enskild partition toopartitioned samlingar](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="7b52f-178">For information about how toomigrate your S1, S2, S3, or single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="7b52f-179">Kan jag ändra mellan hello S1, S2 och S3 prestandanivåer före den 1 augusti 2017?</span><span class="sxs-lookup"><span data-stu-id="7b52f-179">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="7b52f-180">Endast befintliga konton med S1, S2 och S3 prestanda ska kunna toochange och ändra nivån prestandanivåer hello-portalen eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="7b52f-180">Only existing accounts with S1, S2, and S3 performance will be able toochange and alter performance level tiers through hello portal or programmatically.</span></span> <span data-ttu-id="7b52f-181">Med den 1 augusti 2017 hello S1 S2, och S3 prestandanivåer kommer inte längre tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7b52f-181">By August 1, 2017, hello S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="7b52f-182">Om du ändrar från S1, S3 eller S3 tooa enda partition samling kan återgå du inte toohello S1, S2 och S3 prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="7b52f-182">If you change from S1, S3, or S3 tooa single partition collection, you cannot return toohello S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="7b52f-183">Hur vet jag om min samling har migrerats?</span><span class="sxs-lookup"><span data-stu-id="7b52f-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="7b52f-184">hello migreringen sker på 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="7b52f-184">hello migration will occur on July 31, 2017.</span></span> <span data-ttu-id="7b52f-185">Om du har en samling som använder hello S1, S2 eller S3 prestandanivåer, hello Cosmos-DB-teamet kommer att kontakta dig via e-post innan hello migrering äger rum.</span><span class="sxs-lookup"><span data-stu-id="7b52f-185">If you have a collection that uses hello S1, S2 or S3 performance levels, hello Cosmos DB team will contact you by email before hello migration takes place.</span></span> <span data-ttu-id="7b52f-186">När hello migreringen är klar på 1 augusti 2017 visar hello Azure-portalen att din samling använder standardprisnivån.</span><span class="sxs-lookup"><span data-stu-id="7b52f-186">Once hello migration is complete, on August 1, 2017, hello Azure portal will show that your collection uses Standard pricing.</span></span>

![Hur tooconfirm din samling har migrerats toohello Standard prisnivå](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a><span data-ttu-id="7b52f-188">Hur migrera från hello S1, S2 partitionssamlingar för S3 prestanda nivåer toosingle själv?</span><span class="sxs-lookup"><span data-stu-id="7b52f-188">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>

<span data-ttu-id="7b52f-189">Du kan migrera från hello S1, S2, och S3 prestanda nivåer toosingle partitionssamlingar med hello Azure-portalen eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="7b52f-189">You can migrate from hello S1, S2, and S3 performance levels toosingle partition collections using hello Azure portal or programmatically.</span></span> <span data-ttu-id="7b52f-190">Du kan göra detta på din egen innan 1 augusti toobenefit från hello flexibla genomströmning alternativen med enskilda partitionssamlingar eller kommer vi att migrera samlingar för dig på den 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="7b52f-190">You can do this on your own before August 1 toobenefit from hello flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="7b52f-191">**toomigrate toosingle partitionssamlingar med hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="7b52f-191">**toomigrate toosingle partition collections using hello Azure portal**</span></span>

1. <span data-ttu-id="7b52f-192">I hello [ **Azure-portalen**](https://portal.azure.com), klickar du på **Azure Cosmos DB**, Välj hello Cosmos DB konto toomodify.</span><span class="sxs-lookup"><span data-stu-id="7b52f-192">In hello [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select hello Cosmos DB account toomodify.</span></span> 
 
    <span data-ttu-id="7b52f-193">Om **Azure Cosmos DB** är inte på hello Jumpbar klickar du på >, rulla för**databaser**väljer **Azure Cosmos DB**, och välj sedan hello DocumentDB-konto.</span><span class="sxs-lookup"><span data-stu-id="7b52f-193">If **Azure Cosmos DB** is not on hello Jumpbar, click >, scroll too**Databases**, select **Azure Cosmos DB**, and then select hello DocumentDB account.</span></span>  

2. <span data-ttu-id="7b52f-194">På menyn för hello-resurs under **behållare**, klickar du på **skala**, Välj hello samling toomodify hello nedrullningsbara listan och klicka sedan på **prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="7b52f-194">On hello resource menu, under **Containers**, click **Scale**, select hello collection toomodify from hello drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="7b52f-195">Konton med hjälp av fördefinierade genomströmning har en prisnivå för S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="7b52f-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="7b52f-196">I hello **Välj din prisnivå** bladet klickar du på **Standard** toochange toouser definierats dataflöde, och klicka sedan på **Välj** toosave ändringen.</span><span class="sxs-lookup"><span data-stu-id="7b52f-196">In hello **Choose your pricing tier** blade, click **Standard** toochange toouser-defined throughput, and then click **Select** toosave your change.</span></span>

    ![Skärmbild som visar hello inställningsbladet visar där toochange hello genomströmning värde](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="7b52f-198">Tillbaka i hello **skala** bladet, hello **prisnivån** ändras för**Standard** och hello **genomströmning (RU/s)** visas med en Standardvärdet för 400.</span><span class="sxs-lookup"><span data-stu-id="7b52f-198">Back in hello **Scale** blade, hello **Pricing Tier** is changed too**Standard** and hello **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="7b52f-199">Ange hello dataflödet mellan 400 och 10 000 [programbegäran](request-units.md)/second (RU/s).</span><span class="sxs-lookup"><span data-stu-id="7b52f-199">Set hello throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="7b52f-200">Hej **uppskattad Månadsfaktura** längst hello hello sidan uppdateras automatiskt tooprovide en uppskattning av hello kostnad varje månad.</span><span class="sxs-lookup"><span data-stu-id="7b52f-200">hello **Estimated Monthly Bill** at hello bottom of hello page updates automatically tooprovide an estimate of hello monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="7b52f-201">När du sparar ändringarna och flytta toohello Standard prisnivå återställa du inte toohello S1, S2 och S3 prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="7b52f-201">Once you save your changes and move toohello Standard pricing tier, you cannot roll back toohello S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="7b52f-202">Klicka på **spara** toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7b52f-202">Click **Save** toosave your changes.</span></span>

    <span data-ttu-id="7b52f-203">Om du anser att du behöver mer genomströmning (större än 10 000 RU/s) eller mer lagringsutrymme (större än 10GB) kan du skapa en partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="7b52f-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="7b52f-204">toomigrate en enskild partition samling tooa partitionerad samling finns [migrerar från en enskild partition toopartitioned samlingar](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="7b52f-204">toomigrate a single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="7b52f-205">Om du ändrar från S1, S2 och S3 tooStandard kan ta too2 minuter.</span><span class="sxs-lookup"><span data-stu-id="7b52f-205">Changing from S1, S2, or S3 tooStandard may take up too2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="7b52f-206">**toomigrate toosingle partitionssamlingar med hello .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="7b52f-206">**toomigrate toosingle partition collections using hello .NET SDK**</span></span>

<span data-ttu-id="7b52f-207">Ett annat alternativ för att ändra din samlingar prestandanivåer är via vårt SDK: er.</span><span class="sxs-lookup"><span data-stu-id="7b52f-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="7b52f-208">Det här avsnittet beskriver bara ändra prestanda för en samling nivå med hjälp av vår [DocumentDB .NET API](documentdb-sdk-dotnet.md), men hello processen är liknande för våra andra SDK: er.</span><span class="sxs-lookup"><span data-stu-id="7b52f-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but hello process is similar for our other SDKs.</span></span>

<span data-ttu-id="7b52f-209">Här är ett kodfragment för att ändra hello samling genomströmning too5 000 frågeenheter per sekund:</span><span class="sxs-lookup"><span data-stu-id="7b52f-209">Here is a code snippet for changing hello collection throughput too5,000 request units per second:</span></span>
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="7b52f-210">Besök [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview ytterligare exempel och lär dig mer om våra erbjudanden metoder:</span><span class="sxs-lookup"><span data-stu-id="7b52f-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="7b52f-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="7b52f-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="7b52f-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="7b52f-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="7b52f-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="7b52f-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="7b52f-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="7b52f-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="7b52f-215">Hur kan jag påverkas om jag en EA-kund?</span><span class="sxs-lookup"><span data-stu-id="7b52f-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="7b52f-216">EA-kunder kommer att pris skyddade fram hello slutet av avtalets aktuella.</span><span class="sxs-lookup"><span data-stu-id="7b52f-216">EA customers will be price protected until hello end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b52f-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b52f-217">Next steps</span></span>
<span data-ttu-id="7b52f-218">toolearn mer information om priser och hantera data med Azure Cosmos DB utforska gärna dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="7b52f-218">toolearn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="7b52f-219">[Partitionering data i Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="7b52f-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="7b52f-220">Förstå hello skillnaden mellan enskild partition behållare och partitionerade behållare samt tips om hur du implementerar en partitionering strategi tooscale sömlöst.</span><span class="sxs-lookup"><span data-stu-id="7b52f-220">Understand hello difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy tooscale seamlessly.</span></span>
2.  <span data-ttu-id="7b52f-221">[Priser för cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="7b52f-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="7b52f-222">Läs mer om hello kostnaden för etablering genomflöde och använda lagring.</span><span class="sxs-lookup"><span data-stu-id="7b52f-222">Learn about hello cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="7b52f-223">[Enheter för programbegäran](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="7b52f-223">[Request units](request-units.md).</span></span> <span data-ttu-id="7b52f-224">Förstå hello förbrukningen av genomströmning för olika åtgärdstyper, till exempel läsa, skriva frågan.</span><span class="sxs-lookup"><span data-stu-id="7b52f-224">Understand hello consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
