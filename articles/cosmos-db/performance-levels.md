---
title: "Prestandanivåer för DocumentDB API | Microsoft Docs"
description: "Läs mer om hur prestandanivåer för DocumentDB API gör det möjligt att reservera genomströmning på grundval av per behållare."
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
ms.openlocfilehash: c8d4733e57eb760dbb8e8ca96f6ba55671d1742f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="47d30-103">Ta bort prestandanivåer S1, S2 och S3</span><span class="sxs-lookup"><span data-stu-id="47d30-103">Retiring the S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="47d30-104">Prestandanivåer S1, S2 och S3 i den här artikeln är som har återkallats och är inte längre tillgängliga för nya DocumentDB API-konton.</span><span class="sxs-lookup"><span data-stu-id="47d30-104">The S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="47d30-105">Den här artikeln innehåller en översikt över S1, S2 och S3 prestandanivåer och beskriver hur de samlingar som använder dessa prestandanivåer kommer att migreras till enskilda partitionssamlingar på 1 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="47d30-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how the collections that use these performance levels will be migrated to single partition collections on August 1st, 2017.</span></span> <span data-ttu-id="47d30-106">När du har läst den här artikeln kommer du att kunna svara på följande frågor:</span><span class="sxs-lookup"><span data-stu-id="47d30-106">After reading this article, you'll be able to answer the following questions:</span></span>

- [<span data-ttu-id="47d30-107">Varför är S1, S2 och S3 prestanda nivåer som har återkallats?</span><span class="sxs-lookup"><span data-stu-id="47d30-107">Why are the S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="47d30-108">Hur enskilda partitionssamlingar och partitionerade samlingar Jämför med S1, S2, prestandanivåer S3?</span><span class="sxs-lookup"><span data-stu-id="47d30-108">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="47d30-109">Vad behöver jag för att säkerställa kontinuerlig tillgång till Mina data?</span><span class="sxs-lookup"><span data-stu-id="47d30-109">What do I need to do to ensure uninterrupted access to my data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="47d30-110">Hur ändrar jag samlingen efter migreringen?</span><span class="sxs-lookup"><span data-stu-id="47d30-110">How will my collection change after the migration?</span></span>](#collection-change)
- [<span data-ttu-id="47d30-111">Hur ändras efter att jag har migrerats till enskilda partitionssamlingar av min fakturering?</span><span class="sxs-lookup"><span data-stu-id="47d30-111">How will my billing change after I’m migrated to single partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="47d30-112">Vad händer om jag behöver fler än 10 GB lagringsutrymme?</span><span class="sxs-lookup"><span data-stu-id="47d30-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="47d30-113">Kan jag ändra mellan S1, S2 och S3 prestandanivåer före den 1 augusti 2017?</span><span class="sxs-lookup"><span data-stu-id="47d30-113">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="47d30-114">Hur vet jag om min samling har migrerats?</span><span class="sxs-lookup"><span data-stu-id="47d30-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="47d30-115">Hur jag migrera från S1, S2, S3 prestandanivåer för enskilda partitionssamlingar själv?</span><span class="sxs-lookup"><span data-stu-id="47d30-115">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="47d30-116">Hur kan jag påverkas om jag en EA-kund?</span><span class="sxs-lookup"><span data-stu-id="47d30-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="47d30-117">Varför är S1, S2 och S3 prestanda nivåer som har återkallats?</span><span class="sxs-lookup"><span data-stu-id="47d30-117">Why are the S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="47d30-118">S1, S2 och S3 prestandanivåer erbjuder inte den flexibilitet som erbjuder DocumentDB API samlingar.</span><span class="sxs-lookup"><span data-stu-id="47d30-118">The S1, S2, and S3 performance levels do not offer the flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="47d30-119">S1, S2 S3 prestandanivåer genomströmning-och lagringskapacitet har redan angetts och erbjöd inte elasticitet.</span><span class="sxs-lookup"><span data-stu-id="47d30-119">With the S1, S2, S3 performance levels, both the throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="47d30-120">Azure Cosmos-DB erbjuder nu möjlighet att anpassa din dataflöden och lagringsutrymmen, ger mycket större flexibilitet i möjligheten att skala efter dina behov förändras.</span><span class="sxs-lookup"><span data-stu-id="47d30-120">Azure Cosmos DB now offers the ability to customize your throughput and storage, offering you much more flexibility in your ability to scale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a><span data-ttu-id="47d30-121">Hur enskilda partitionssamlingar och partitionerade samlingar Jämför med S1, S2, prestandanivåer S3?</span><span class="sxs-lookup"><span data-stu-id="47d30-121">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="47d30-122">I följande tabell jämförs dataflöden och lagringsutrymmen alternativen i enskilda partitionssamlingar partitionerade samlingar och S1, S2 S3 prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="47d30-122">The following table compares the throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="47d30-123">Här är ett exempel på oss östra 2 region:</span><span class="sxs-lookup"><span data-stu-id="47d30-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="47d30-124">Partitionerad samling</span><span class="sxs-lookup"><span data-stu-id="47d30-124">Partitioned collection</span></span>|<span data-ttu-id="47d30-125">Enskild partition samling</span><span class="sxs-lookup"><span data-stu-id="47d30-125">Single partition collection</span></span>|<span data-ttu-id="47d30-126">S1</span><span class="sxs-lookup"><span data-stu-id="47d30-126">S1</span></span>|<span data-ttu-id="47d30-127">S2</span><span class="sxs-lookup"><span data-stu-id="47d30-127">S2</span></span>|<span data-ttu-id="47d30-128">S3</span><span class="sxs-lookup"><span data-stu-id="47d30-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="47d30-129">Maximalt dataflöde</span><span class="sxs-lookup"><span data-stu-id="47d30-129">Maximum throughput</span></span>|<span data-ttu-id="47d30-130">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="47d30-130">Unlimited</span></span>|<span data-ttu-id="47d30-131">10 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-131">10K RU/s</span></span>|<span data-ttu-id="47d30-132">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-132">250 RU/s</span></span>|<span data-ttu-id="47d30-133">1 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-133">1 K RU/s</span></span>|<span data-ttu-id="47d30-134">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="47d30-135">Minsta dataflöde</span><span class="sxs-lookup"><span data-stu-id="47d30-135">Minimum throughput</span></span>|<span data-ttu-id="47d30-136">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-136">2.5K RU/s</span></span>|<span data-ttu-id="47d30-137">400 RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-137">400 RU/s</span></span>|<span data-ttu-id="47d30-138">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-138">250 RU/s</span></span>|<span data-ttu-id="47d30-139">1 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-139">1 K RU/s</span></span>|<span data-ttu-id="47d30-140">2.5 K RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="47d30-141">Maximalt lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="47d30-141">Maximum storage</span></span>|<span data-ttu-id="47d30-142">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="47d30-142">Unlimited</span></span>|<span data-ttu-id="47d30-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="47d30-143">10 GB</span></span>|<span data-ttu-id="47d30-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="47d30-144">10 GB</span></span>|<span data-ttu-id="47d30-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="47d30-145">10 GB</span></span>|<span data-ttu-id="47d30-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="47d30-146">10 GB</span></span>|
|<span data-ttu-id="47d30-147">Pris (månad)</span><span class="sxs-lookup"><span data-stu-id="47d30-147">Price (monthly)</span></span>|<span data-ttu-id="47d30-148">Genomströmning: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="47d30-149">Lagring: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="47d30-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="47d30-150">Genomströmning: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="47d30-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="47d30-151">Lagring: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="47d30-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="47d30-152">$25 USD</span><span class="sxs-lookup"><span data-stu-id="47d30-152">$25 USD</span></span>|<span data-ttu-id="47d30-153">$50 USD</span><span class="sxs-lookup"><span data-stu-id="47d30-153">$50 USD</span></span>|<span data-ttu-id="47d30-154">$100 USD</span><span class="sxs-lookup"><span data-stu-id="47d30-154">$100 USD</span></span>|

<span data-ttu-id="47d30-155">Är du en kund med EA?</span><span class="sxs-lookup"><span data-stu-id="47d30-155">Are you an EA customer?</span></span> <span data-ttu-id="47d30-156">I så fall, finns [hur får jag påverkas om jag en EA-kund?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="47d30-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a><span data-ttu-id="47d30-157">Vad behöver jag för att säkerställa kontinuerlig tillgång till Mina data?</span><span class="sxs-lookup"><span data-stu-id="47d30-157">What do I need to do to ensure uninterrupted access to my data?</span></span>

<span data-ttu-id="47d30-158">Något, Cosmos DB hanterar du migreringen.</span><span class="sxs-lookup"><span data-stu-id="47d30-158">Nothing, Cosmos DB handles the migration for you.</span></span> <span data-ttu-id="47d30-159">Om du har en S1, S2 och S3 samling kommer aktuella samlingen att migreras till en enda partition samling på 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="47d30-159">If you have an S1, S2, or S3 collection, your current collection will be migrated to a single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a><span data-ttu-id="47d30-160">Hur ändrar jag samlingen efter migreringen?</span><span class="sxs-lookup"><span data-stu-id="47d30-160">How will my collection change after the migration?</span></span>

<span data-ttu-id="47d30-161">Om du har en S1 samling migreras till en enda partition samling med 400 RU/s genomströmning.</span><span class="sxs-lookup"><span data-stu-id="47d30-161">If you have an S1 collection, you will be migrated to a single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="47d30-162">400 RU/s är den lägsta genomflödet som är tillgängliga med enskilda partitionssamlingar.</span><span class="sxs-lookup"><span data-stu-id="47d30-162">400 RU/s is the lowest throughput available with single partition collections.</span></span> <span data-ttu-id="47d30-163">Dock kostnaden för 400 RU/s i på en enda partition samling är ungefär detsamma som betalar med S1 samlingen och 250 RU/s – så att du inte betalar för extra 150 RU/s tillgängliga för dig.</span><span class="sxs-lookup"><span data-stu-id="47d30-163">However, the cost for 400 RU/s in the a single partition collection is approximately the same as you were paying with your S1 collection and 250 RU/s – so you are not paying for the extra 150 RU/s available to you.</span></span>

<span data-ttu-id="47d30-164">Om du har en S2 samling migreras till en enda partition samling med 1 K RU/s.</span><span class="sxs-lookup"><span data-stu-id="47d30-164">If you have an S2 collection, you will be migrated to a single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="47d30-165">Ingen ändring ser du till genomflödet-nivå.</span><span class="sxs-lookup"><span data-stu-id="47d30-165">You will see no change to your throughput level.</span></span>

<span data-ttu-id="47d30-166">Om du har en S3 samling migreras till en enda partition samling med 2,5 K RU/s.</span><span class="sxs-lookup"><span data-stu-id="47d30-166">If you have an S3 collection, you will be migrated to a single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="47d30-167">Ingen ändring ser du till genomflödet-nivå.</span><span class="sxs-lookup"><span data-stu-id="47d30-167">You will see no change to your throughput level.</span></span>

<span data-ttu-id="47d30-168">I dessa fall migrerat samlingen kommer du att kunna anpassa genomströmning-nivå eller skala den upp eller ned efter behov för att ge låg latens åtkomst till dina användare.</span><span class="sxs-lookup"><span data-stu-id="47d30-168">In each of these cases, after your collection is migrated, you will be able to customize your throughput level, or scale it up and down as needed to provide low-latency access to your users.</span></span> <span data-ttu-id="47d30-169">Om du vill ändra dataflödesnivån när din samling har migrerats, helt enkelt öppna Cosmos-DB-konto i Azure-portalen, klickar du på skala, Välj din samling och sedan justera dataflöde, som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="47d30-169">To change the throughput level after your collection has migrated, simply open your Cosmos DB account in the Azure portal, click Scale, choose your collection, and then adjust the throughput level, as shown in the following screenshot:</span></span>

![Så här skalar genomflöde i Azure-portalen](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a><span data-ttu-id="47d30-171">Hur ändras efter att jag har migrerats till enskilda partitionssamlingar av min fakturering?</span><span class="sxs-lookup"><span data-stu-id="47d30-171">How will my billing change after I’m migrated to the single partition collections?</span></span>

<span data-ttu-id="47d30-172">Om du har 10 S1 samlingar, 1 GB lagringsutrymme i region oss Öst och du migrerar dessa 10 S1 samlingar till 10 enskilda partitionssamlingar på 400 RU/s (lägsta nivå).</span><span class="sxs-lookup"><span data-stu-id="47d30-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in the US East region, and you migrate these 10 S1 collections to 10 single partition collections at 400 RU/sec (the minimum level).</span></span> <span data-ttu-id="47d30-173">Fakturan ska se ut så här om du behåller 10 enskilda partitionssamlingar för en hel månad:</span><span class="sxs-lookup"><span data-stu-id="47d30-173">Your bill will look as follows if you keep the 10 single partition collections for a full month:</span></span>

![Hur S1 priser för 10 samlingar jämför till 10 samlingar med priser för en enskild partition samling](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="47d30-175">Vad händer om jag behöver fler än 10 GB lagringsutrymme?</span><span class="sxs-lookup"><span data-stu-id="47d30-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="47d30-176">Om du har en samling med en S1, S2 och S3 prestandanivå eller ha en enda partition samling, 10 GB tillgängligt lagringsutrymme som alla har kan du använda Cosmos-Migreringsverktyget DB Data för att migrera dina data till en partitionerad samling med praktiskt taget obegränsade lagring.</span><span class="sxs-lookup"><span data-stu-id="47d30-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use the Cosmos DB Data Migration tool to migrate your data to a partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="47d30-177">Information om fördelarna med en partitionerad samling finns [partitionering och skalning i Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="47d30-177">For information about the benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="47d30-178">Information om hur du migrerar din S1 S2, S3 eller enskild partition samlingen till en partitionerad samling finns [migrera från en enskild partition till partitionerade samlingar](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="47d30-178">For information about how to migrate your S1, S2, S3, or single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="47d30-179">Kan jag ändra mellan S1, S2 och S3 prestandanivåer före den 1 augusti 2017?</span><span class="sxs-lookup"><span data-stu-id="47d30-179">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="47d30-180">Endast befintliga konton med S1, S2 och S3 prestanda kommer att kunna ändra och ändra nivån prestandanivåer via portalen eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="47d30-180">Only existing accounts with S1, S2, and S3 performance will be able to change and alter performance level tiers through the portal or programmatically.</span></span> <span data-ttu-id="47d30-181">Med den 1 augusti 2017 kommer prestandanivåer S1, S2 och S3 inte längre tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="47d30-181">By August 1, 2017, the S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="47d30-182">Om du ändrar från S1, S3 eller S3 till en enda partition samling, kan du återgå till prestandanivåer S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="47d30-182">If you change from S1, S3, or S3 to a single partition collection, you cannot return to the S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="47d30-183">Hur vet jag om min samling har migrerats?</span><span class="sxs-lookup"><span data-stu-id="47d30-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="47d30-184">Migreringen sker på 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="47d30-184">The migration will occur on July 31, 2017.</span></span> <span data-ttu-id="47d30-185">Om du har en samling som använder S1 S2 eller S3 prestandanivåer, Cosmos-DB-teamet kommer att kontakta dig via e-post innan migreringen sker.</span><span class="sxs-lookup"><span data-stu-id="47d30-185">If you have a collection that uses the S1, S2 or S3 performance levels, the Cosmos DB team will contact you by email before the migration takes place.</span></span> <span data-ttu-id="47d30-186">När migreringen är klar på 1 augusti 2017 visar Azure-portalen att din samling använder standardprisnivån.</span><span class="sxs-lookup"><span data-stu-id="47d30-186">Once the migration is complete, on August 1, 2017, the Azure portal will show that your collection uses Standard pricing.</span></span>

![Hur du kontrollerar din samling har migrerats till Standard prisnivån](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a><span data-ttu-id="47d30-188">Hur jag migrera från S1, S2, S3 prestandanivåer för enskilda partitionssamlingar själv?</span><span class="sxs-lookup"><span data-stu-id="47d30-188">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>

<span data-ttu-id="47d30-189">Du kan migrera från prestandanivåer S1, S2 och S3 till enskilda partitionssamlingar med Azure-portalen eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="47d30-189">You can migrate from the S1, S2, and S3 performance levels to single partition collections using the Azure portal or programmatically.</span></span> <span data-ttu-id="47d30-190">Du kan göra detta på egen hand före den 1 augusti att dra nytta av flexibla genomströmning-alternativen med enskilda partitionssamlingar eller kommer vi att migrera samlingar för dig på den 31 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="47d30-190">You can do this on your own before August 1 to benefit from the flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="47d30-191">**Att migrera till enskilda partitionssamlingar med Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="47d30-191">**To migrate to single partition collections using the Azure portal**</span></span>

1. <span data-ttu-id="47d30-192">I den [ **Azure-portalen**](https://portal.azure.com), klickar du på **Azure Cosmos DB**, Välj Cosmos-DB-konto för att ändra.</span><span class="sxs-lookup"><span data-stu-id="47d30-192">In the [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select the Cosmos DB account to modify.</span></span> 
 
    <span data-ttu-id="47d30-193">Om **Azure Cosmos DB** är inte i Jumpbar klickar du på >, bläddra till **databaser**väljer **Azure Cosmos DB**, och välj sedan DocumentDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="47d30-193">If **Azure Cosmos DB** is not on the Jumpbar, click >, scroll to **Databases**, select **Azure Cosmos DB**, and then select the DocumentDB account.</span></span>  

2. <span data-ttu-id="47d30-194">På menyn resurs under **behållare**, klickar du på **skala**, markera samlingen för att ändra listrutan och klicka sedan på **prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="47d30-194">On the resource menu, under **Containers**, click **Scale**, select the collection to modify from the drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="47d30-195">Konton med hjälp av fördefinierade genomströmning har en prisnivå för S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="47d30-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="47d30-196">I den **Välj din prisnivå** bladet, klickar du på **Standard** ändra till användardefinierade genomflöde och klicka sedan på **Välj** att spara ändringen.</span><span class="sxs-lookup"><span data-stu-id="47d30-196">In the **Choose your pricing tier** blade, click **Standard** to change to user-defined throughput, and then click **Select** to save your change.</span></span>

    ![Skärmbild av inställningsbladet visar var du kan ändra värdet för dataflöde](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="47d30-198">I den **skala** bladet den **prisnivån** ändras till **Standard** och **genomströmning (RU/s)** visas med ett standardvärde 400.</span><span class="sxs-lookup"><span data-stu-id="47d30-198">Back in the **Scale** blade, the **Pricing Tier** is changed to **Standard** and the **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="47d30-199">Ange dataflöde mellan 400 och 10 000 [programbegäran](request-units.md)/second (RU/s).</span><span class="sxs-lookup"><span data-stu-id="47d30-199">Set the throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="47d30-200">Den **uppskattad Månadsfaktura** längst ned på sidan uppdateringar automatiskt för att ge en uppskattning av månadskostnaden.</span><span class="sxs-lookup"><span data-stu-id="47d30-200">The **Estimated Monthly Bill** at the bottom of the page updates automatically to provide an estimate of the monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="47d30-201">När du sparar ändringarna och gå till den Standardprisnivån kan inte du återställa prestandanivåer S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="47d30-201">Once you save your changes and move to the Standard pricing tier, you cannot roll back to the S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="47d30-202">Klicka på **spara** att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="47d30-202">Click **Save** to save your changes.</span></span>

    <span data-ttu-id="47d30-203">Om du anser att du behöver mer genomströmning (större än 10 000 RU/s) eller mer lagringsutrymme (större än 10GB) kan du skapa en partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="47d30-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="47d30-204">Om du vill migrera en enskild partition samling till en partitionerad samling finns [migrera från en enskild partition till partitionerade samlingar](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="47d30-204">To migrate a single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="47d30-205">Ändra från S1, S2 och S3 till Standard kan det ta upp till 2 minuter.</span><span class="sxs-lookup"><span data-stu-id="47d30-205">Changing from S1, S2, or S3 to Standard may take up to 2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="47d30-206">**Att migrera till enskilda partitionssamlingar med .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="47d30-206">**To migrate to single partition collections using the .NET SDK**</span></span>

<span data-ttu-id="47d30-207">Ett annat alternativ för att ändra din samlingar prestandanivåer är via vårt SDK: er.</span><span class="sxs-lookup"><span data-stu-id="47d30-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="47d30-208">Det här avsnittet beskriver bara ändra prestanda för en samling nivå med hjälp av vår [DocumentDB .NET API](documentdb-sdk-dotnet.md), men processen är liknande för våra andra SDK: er.</span><span class="sxs-lookup"><span data-stu-id="47d30-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but the process is similar for our other SDKs.</span></span>

<span data-ttu-id="47d30-209">Här är ett kodfragment för att ändra samlingen genomströmning till 5 000 frågeenheter per sekund:</span><span class="sxs-lookup"><span data-stu-id="47d30-209">Here is a code snippet for changing the collection throughput to 5,000 request units per second:</span></span>
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="47d30-210">Besök [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) att visa ytterligare exempel och lär dig mer om våra erbjudanden metoder:</span><span class="sxs-lookup"><span data-stu-id="47d30-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) to view additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="47d30-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="47d30-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="47d30-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="47d30-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="47d30-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="47d30-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="47d30-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="47d30-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="47d30-215">Hur kan jag påverkas om jag en EA-kund?</span><span class="sxs-lookup"><span data-stu-id="47d30-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="47d30-216">EA-kunder kommer att pris skyddade till slutet av avtalets aktuella.</span><span class="sxs-lookup"><span data-stu-id="47d30-216">EA customers will be price protected until the end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47d30-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47d30-217">Next steps</span></span>
<span data-ttu-id="47d30-218">Mer information om priser och hantera data med Azure Cosmos DB utforska dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="47d30-218">To learn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="47d30-219">[Partitionering data i Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="47d30-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="47d30-220">Förstå skillnaden mellan enskild partition behållare och partitionerade behållare samt tips om hur du implementerar en partitioneringsstrategi att sömlöst skala.</span><span class="sxs-lookup"><span data-stu-id="47d30-220">Understand the difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy to scale seamlessly.</span></span>
2.  <span data-ttu-id="47d30-221">[Priser för cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="47d30-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="47d30-222">Läs mer om kostnaden för etablering genomflöde och använda lagring.</span><span class="sxs-lookup"><span data-stu-id="47d30-222">Learn about the cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="47d30-223">[Enheter för programbegäran](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="47d30-223">[Request units](request-units.md).</span></span> <span data-ttu-id="47d30-224">Förstå förbrukningen av genomströmning för olika åtgärdstyper, till exempel läsa, skriva frågan.</span><span class="sxs-lookup"><span data-stu-id="47d30-224">Understand the consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
