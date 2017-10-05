---
title: Hantering av samtidighet och arbetsbelastning i SQL Data Warehouse | Microsoft Docs
description: "Förstå samtidighet och arbetsbelastningen hantering i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: eaf2d43286dbaa52ada1430fbb7ce1e37f41c0d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="57889-103">Hantering av samtidighet och arbetsbelastning i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="57889-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="57889-104">För att leverera förutsägbar prestanda i skala, Microsoft Azure SQL Data Warehouse hjälper dig styra samtidighet nivåer och resursfördelningar som minne och CPU-prioritering.</span><span class="sxs-lookup"><span data-stu-id="57889-104">To deliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="57889-105">Den här artikeln ger en introduktion till begreppet samtidighet och arbetsbelastningen hantering, förklarar hur båda funktionerna har genomförts och hur du kan kontrollera dem i ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="57889-105">This article introduces you to the concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="57889-106">Hantering av SQL Data Warehouse arbetsbelastning är avsedda att hjälpa dig stöd för miljöer med flera användare.</span><span class="sxs-lookup"><span data-stu-id="57889-106">SQL Data Warehouse workload management is intended to help you support multi-user environments.</span></span> <span data-ttu-id="57889-107">Det är inte avsedd för flera innehavare arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="57889-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="57889-108">Concurrency-gränser</span><span class="sxs-lookup"><span data-stu-id="57889-108">Concurrency limits</span></span>
<span data-ttu-id="57889-109">SQL Data Warehouse kan upp till 1 024 samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="57889-109">SQL Data Warehouse allows up to 1,024 concurrent connections.</span></span> <span data-ttu-id="57889-110">Alla 1 024 anslutningar kan skicka frågor samtidigt.</span><span class="sxs-lookup"><span data-stu-id="57889-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="57889-111">För att optimera genomflödet dock kö SQL Data Warehouse några frågor för att säkerställa att varje fråga får en minimal minnestilldelningen.</span><span class="sxs-lookup"><span data-stu-id="57889-111">However, to optimize throughput, SQL Data Warehouse may queue some queries to ensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="57889-112">Queuing inträffar vid körning i frågan.</span><span class="sxs-lookup"><span data-stu-id="57889-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="57889-113">Genom queuing frågor när samtidighet gränser nås, öka SQL Data Warehouse totala genomflödet genom att säkerställa att aktiva frågor får åtkomst till resurser ytterst nödvändigt minne.</span><span class="sxs-lookup"><span data-stu-id="57889-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access to critically needed memory resources.</span></span>  

<span data-ttu-id="57889-114">Concurrency gränser styrs av två begrepp: *samtidiga frågor* och *samtidighet fack*.</span><span class="sxs-lookup"><span data-stu-id="57889-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="57889-115">Det måste köras i både frågan samtidighet gränsen och samtidighet fack tilldelning att köra en fråga.</span><span class="sxs-lookup"><span data-stu-id="57889-115">For a query to execute, it must execute within both the query concurrency limit and the concurrency slot allocation.</span></span>

* <span data-ttu-id="57889-116">Samtidiga frågor är frågor som körs på samma gång.</span><span class="sxs-lookup"><span data-stu-id="57889-116">Concurrent queries are the queries executing at the same time.</span></span> <span data-ttu-id="57889-117">SQL Data Warehouse stöder upp till 32 samtidiga frågor på större DWU-storlekar.</span><span class="sxs-lookup"><span data-stu-id="57889-117">SQL Data Warehouse supports up to 32 concurrent queries on the larger DWU sizes.</span></span>
* <span data-ttu-id="57889-118">Concurrency platser tilldelas baserat på DWU.</span><span class="sxs-lookup"><span data-stu-id="57889-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="57889-119">Varje 100 DWU innehåller 4 samtidighet platser.</span><span class="sxs-lookup"><span data-stu-id="57889-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="57889-120">Till exempel en DW100 allokerar 4 samtidighet platser och DW1000 allokerar 40.</span><span class="sxs-lookup"><span data-stu-id="57889-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="57889-121">Varje fråga förbrukar en eller flera samtidiga platser, beroende på [resursklassen](#resource-classes) av frågan.</span><span class="sxs-lookup"><span data-stu-id="57889-121">Each query consumes one or more concurrency slots, dependent on the [resource class](#resource-classes) of the query.</span></span> <span data-ttu-id="57889-122">Frågor som körs i resursklassen smallrc använda en concurrency-plats.</span><span class="sxs-lookup"><span data-stu-id="57889-122">Queries running in the smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="57889-123">Frågor som körs i en högre resursklassen förbruka mer samtidighet platser.</span><span class="sxs-lookup"><span data-stu-id="57889-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="57889-124">I följande tabell beskrivs gränsvärdena för både samtidiga frågor och samtidighet fack vid olika DWU-storlekar.</span><span class="sxs-lookup"><span data-stu-id="57889-124">The following table describes the limits for both concurrent queries and concurrency slots at the various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="57889-125">Concurrency-gränser</span><span class="sxs-lookup"><span data-stu-id="57889-125">Concurrency limits</span></span>
| <span data-ttu-id="57889-126">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-126">DWU</span></span> | <span data-ttu-id="57889-127">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="57889-127">Max concurrent queries</span></span> | <span data-ttu-id="57889-128">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="57889-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="57889-129">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-129">DW100</span></span> |<span data-ttu-id="57889-130">4</span><span class="sxs-lookup"><span data-stu-id="57889-130">4</span></span> |<span data-ttu-id="57889-131">4</span><span class="sxs-lookup"><span data-stu-id="57889-131">4</span></span> |
| <span data-ttu-id="57889-132">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-132">DW200</span></span> |<span data-ttu-id="57889-133">8</span><span class="sxs-lookup"><span data-stu-id="57889-133">8</span></span> |<span data-ttu-id="57889-134">8</span><span class="sxs-lookup"><span data-stu-id="57889-134">8</span></span> |
| <span data-ttu-id="57889-135">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-135">DW300</span></span> |<span data-ttu-id="57889-136">12</span><span class="sxs-lookup"><span data-stu-id="57889-136">12</span></span> |<span data-ttu-id="57889-137">12</span><span class="sxs-lookup"><span data-stu-id="57889-137">12</span></span> |
| <span data-ttu-id="57889-138">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-138">DW400</span></span> |<span data-ttu-id="57889-139">16</span><span class="sxs-lookup"><span data-stu-id="57889-139">16</span></span> |<span data-ttu-id="57889-140">16</span><span class="sxs-lookup"><span data-stu-id="57889-140">16</span></span> |
| <span data-ttu-id="57889-141">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-141">DW500</span></span> |<span data-ttu-id="57889-142">20</span><span class="sxs-lookup"><span data-stu-id="57889-142">20</span></span> |<span data-ttu-id="57889-143">20</span><span class="sxs-lookup"><span data-stu-id="57889-143">20</span></span> |
| <span data-ttu-id="57889-144">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-144">DW600</span></span> |<span data-ttu-id="57889-145">24</span><span class="sxs-lookup"><span data-stu-id="57889-145">24</span></span> |<span data-ttu-id="57889-146">24</span><span class="sxs-lookup"><span data-stu-id="57889-146">24</span></span> |
| <span data-ttu-id="57889-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-147">DW1000</span></span> |<span data-ttu-id="57889-148">32</span><span class="sxs-lookup"><span data-stu-id="57889-148">32</span></span> |<span data-ttu-id="57889-149">40</span><span class="sxs-lookup"><span data-stu-id="57889-149">40</span></span> |
| <span data-ttu-id="57889-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-150">DW1200</span></span> |<span data-ttu-id="57889-151">32</span><span class="sxs-lookup"><span data-stu-id="57889-151">32</span></span> |<span data-ttu-id="57889-152">48</span><span class="sxs-lookup"><span data-stu-id="57889-152">48</span></span> |
| <span data-ttu-id="57889-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-153">DW1500</span></span> |<span data-ttu-id="57889-154">32</span><span class="sxs-lookup"><span data-stu-id="57889-154">32</span></span> |<span data-ttu-id="57889-155">60</span><span class="sxs-lookup"><span data-stu-id="57889-155">60</span></span> |
| <span data-ttu-id="57889-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-156">DW2000</span></span> |<span data-ttu-id="57889-157">32</span><span class="sxs-lookup"><span data-stu-id="57889-157">32</span></span> |<span data-ttu-id="57889-158">80</span><span class="sxs-lookup"><span data-stu-id="57889-158">80</span></span> |
| <span data-ttu-id="57889-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-159">DW3000</span></span> |<span data-ttu-id="57889-160">32</span><span class="sxs-lookup"><span data-stu-id="57889-160">32</span></span> |<span data-ttu-id="57889-161">120</span><span class="sxs-lookup"><span data-stu-id="57889-161">120</span></span> |
| <span data-ttu-id="57889-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-162">DW6000</span></span> |<span data-ttu-id="57889-163">32</span><span class="sxs-lookup"><span data-stu-id="57889-163">32</span></span> |<span data-ttu-id="57889-164">240</span><span class="sxs-lookup"><span data-stu-id="57889-164">240</span></span> |

<span data-ttu-id="57889-165">När något av dessa tröskelvärden uppfylls är nya frågor i kö och utförs på grundval först in, först ut.</span><span class="sxs-lookup"><span data-stu-id="57889-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="57889-166">En frågor har slutförts, och antalet frågor och fack understiger gränserna som släpps köade frågor.</span><span class="sxs-lookup"><span data-stu-id="57889-166">As a queries finishes and the number of queries and slots falls below the limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="57889-167">*Välj* frågor körs enbart på hantering av dynamiska vyer (av DMV: er) eller katalogvyer inte regleras av någon av gränserna som samtidighet.</span><span class="sxs-lookup"><span data-stu-id="57889-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of the concurrency limits.</span></span> <span data-ttu-id="57889-168">Du kan övervaka systemets oavsett antalet frågor som körs på den.</span><span class="sxs-lookup"><span data-stu-id="57889-168">You can monitor the system regardless of the number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="57889-169">Resursklasser</span><span class="sxs-lookup"><span data-stu-id="57889-169">Resource classes</span></span>
<span data-ttu-id="57889-170">Resursklasser hjälper dig att kontrollera minnesallokeringen och processorcyklerna som en fråga tilldelas.</span><span class="sxs-lookup"><span data-stu-id="57889-170">Resource classes help you control memory allocation and CPU cycles given to a query.</span></span> <span data-ttu-id="57889-171">Du kan tilldela en användare i form av databasroller två typer av resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-171">You can assign two types of resource classes to a user in the form of database roles.</span></span> <span data-ttu-id="57889-172">De två typerna av resursklasser är följande:</span><span class="sxs-lookup"><span data-stu-id="57889-172">The two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="57889-173">Dynamiska resurs-klasser (**smallrc mediumrc, largerc xlargerc**) allokera en variabel mängd minne beroende på den aktuella DWU.</span><span class="sxs-lookup"><span data-stu-id="57889-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on the current DWU.</span></span> <span data-ttu-id="57889-174">Det innebär att när du skalar upp till en större DWU dina frågor automatiskt få mer minne.</span><span class="sxs-lookup"><span data-stu-id="57889-174">This means that when you scale up to a larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="57889-175">Statiska resurs-klasser (**staticrc10 staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allokera samma mängd minne oavsett aktuella DWU (förutsatt att själva DWU har tillräckligt med minne).</span><span class="sxs-lookup"><span data-stu-id="57889-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate the same amount of memory regardless of the current DWU (provided that the DWU itself has enough memory).</span></span> <span data-ttu-id="57889-176">Det innebär att större dwu: er, du kan köra flera frågor i varje resursklassen samtidigt.</span><span class="sxs-lookup"><span data-stu-id="57889-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="57889-177">Användare i **smallrc** och **staticrc10** ges en mindre mängd minne och kan dra nytta av högre samtidighet.</span><span class="sxs-lookup"><span data-stu-id="57889-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="57889-178">Däremot användare tilldelade **xlargerc** eller **staticrc80** ges stora mängder minne, och därför färre deras frågor kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="57889-178">In contrast, users assigned to **xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="57889-179">Som standard varje användare är medlem i små resursklassen **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="57889-179">By default, each user is a member of the small resource class, **smallrc**.</span></span> <span data-ttu-id="57889-180">Proceduren `sp_addrolemember` används för att öka resursklassen och `sp_droprolemember` används för att minska resursklassen.</span><span class="sxs-lookup"><span data-stu-id="57889-180">The procedure `sp_addrolemember` is used to increase the resource class, and `sp_droprolemember` is used to decrease the resource class.</span></span> <span data-ttu-id="57889-181">Det här kommandot skulle till exempel öka resursklassen för loaduser till **largerc**:</span><span class="sxs-lookup"><span data-stu-id="57889-181">For example, this command would increase the resource class of loaduser to **largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="57889-182">Frågor som inte följer resursklasser</span><span class="sxs-lookup"><span data-stu-id="57889-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="57889-183">Det finns några typer av frågor som inte omfattas av en större minnesallokering.</span><span class="sxs-lookup"><span data-stu-id="57889-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="57889-184">Systemet ignorerar sina allokering av klassen och alltid köra dessa frågor i små resursklassen i stället.</span><span class="sxs-lookup"><span data-stu-id="57889-184">The system ignores their resource class allocation and always run these queries in the small resource class instead.</span></span> <span data-ttu-id="57889-185">Om de här frågorna körs alltid i små resursklassen, kan de köras när samtidighet platser är under hög belastning och de kommer inte använda fler platser än vad som behövs.</span><span class="sxs-lookup"><span data-stu-id="57889-185">If these queries always run in the small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="57889-186">Se [resurs klassen undantag](#query-exceptions-to-concurrency-limits) för mer information.</span><span class="sxs-lookup"><span data-stu-id="57889-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="57889-187">Information om resurs klassen tilldelning</span><span class="sxs-lookup"><span data-stu-id="57889-187">Details on resource class assignment</span></span>


<span data-ttu-id="57889-188">Några få mer information om resursklass:</span><span class="sxs-lookup"><span data-stu-id="57889-188">A few more details on resource class:</span></span>

* <span data-ttu-id="57889-189">*ALTER role* behörighet krävs för att ändra resursklassen för en användare.</span><span class="sxs-lookup"><span data-stu-id="57889-189">*Alter role* permission is required to change the resource class of a user.</span></span>
* <span data-ttu-id="57889-190">Du kan lägga till en användare till en eller flera av de högre resurs klasserna, åsidosätter dynamisk resursklasser statiska resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-190">Although you can add a user to one or more of the higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="57889-191">Det vill säga om en användare har tilldelats både **mediumrc**(dynamisk) och **staticrc80**(statisk) **mediumrc** är resursklass som har lösts in.</span><span class="sxs-lookup"><span data-stu-id="57889-191">That is, if a user is assigned to both **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is the resource class that is honored.</span></span>
 * <span data-ttu-id="57889-192">När en användare har tilldelats fler än en resursklass i en viss klass resurstyp (mer än en dynamisk resursklassen eller mer än en statisk resursklassen), funktion den högsta resursklassen.</span><span class="sxs-lookup"><span data-stu-id="57889-192">When a user is assigned to more than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), the highest resource class is honored.</span></span> <span data-ttu-id="57889-193">Om en användare har tilldelats både mediumrc och largerc funktion som är högre resursklass (largerc).</span><span class="sxs-lookup"><span data-stu-id="57889-193">That is, if a user is assigned to both mediumrc and largerc, the higher resource class (largerc) is honored.</span></span> <span data-ttu-id="57889-194">Och om en användare har tilldelats både **staticrc20** och **statirc80**, **staticrc80** är hanteras.</span><span class="sxs-lookup"><span data-stu-id="57889-194">And if a user is assigned to both **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="57889-195">Resursklass av den administrativa systemanvändaren kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="57889-195">The resource class of the system administrative user cannot be changed.</span></span>

<span data-ttu-id="57889-196">Ett detaljerat exempel finns [ändrar användaren resurs klassexempel](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="57889-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="57889-197">Minnesallokering</span><span class="sxs-lookup"><span data-stu-id="57889-197">Memory allocation</span></span>
<span data-ttu-id="57889-198">Det finns för- och nackdelar för ett ökande antal resursklassen för en användare.</span><span class="sxs-lookup"><span data-stu-id="57889-198">There are pros and cons to increasing a user's resource class.</span></span> <span data-ttu-id="57889-199">Öka en resursklass för en användare får sina frågor tillgång till mer minne, vilket kan innebära att köra frågor snabbare.</span><span class="sxs-lookup"><span data-stu-id="57889-199">Increasing a resource class for a user, gives their queries access to more memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="57889-200">Högre resurs klasserna dock minska också antalet frågor som kan köras.</span><span class="sxs-lookup"><span data-stu-id="57889-200">However, higher resource classes also reduce the number of concurrent queries that can run.</span></span> <span data-ttu-id="57889-201">Det här är en kompromiss mellan tilldelning av stora mängder minne till en enda fråga eller att tillåta att andra frågor som måste också minnesallokering körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="57889-201">This is the trade-off between allocating large amounts of memory to a single query or allowing other queries, which also need memory allocations, to run concurrently.</span></span> <span data-ttu-id="57889-202">Om en användare får hög tilldelning av minne för en fråga, andra användare inte tillgång till att samma minne för att köra en fråga.</span><span class="sxs-lookup"><span data-stu-id="57889-202">If one user is given high allocations of memory for a query, other users will not have access to that same memory to run a query.</span></span>

<span data-ttu-id="57889-203">I följande tabell mappar det minne som allokerats för varje distribution av DWU och resurs-klassen.</span><span class="sxs-lookup"><span data-stu-id="57889-203">The following table maps the memory allocated to each distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="57889-204">Minnesallokering per distribution för dynamisk resursklasser (MB)</span><span class="sxs-lookup"><span data-stu-id="57889-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="57889-205">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-205">DWU</span></span> | <span data-ttu-id="57889-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="57889-206">smallrc</span></span> | <span data-ttu-id="57889-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="57889-207">mediumrc</span></span> | <span data-ttu-id="57889-208">largerc</span><span class="sxs-lookup"><span data-stu-id="57889-208">largerc</span></span> | <span data-ttu-id="57889-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="57889-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="57889-210">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-210">DW100</span></span> |<span data-ttu-id="57889-211">100</span><span class="sxs-lookup"><span data-stu-id="57889-211">100</span></span> |<span data-ttu-id="57889-212">100</span><span class="sxs-lookup"><span data-stu-id="57889-212">100</span></span> |<span data-ttu-id="57889-213">200</span><span class="sxs-lookup"><span data-stu-id="57889-213">200</span></span> |<span data-ttu-id="57889-214">400</span><span class="sxs-lookup"><span data-stu-id="57889-214">400</span></span> |
| <span data-ttu-id="57889-215">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-215">DW200</span></span> |<span data-ttu-id="57889-216">100</span><span class="sxs-lookup"><span data-stu-id="57889-216">100</span></span> |<span data-ttu-id="57889-217">200</span><span class="sxs-lookup"><span data-stu-id="57889-217">200</span></span> |<span data-ttu-id="57889-218">400</span><span class="sxs-lookup"><span data-stu-id="57889-218">400</span></span> |<span data-ttu-id="57889-219">800</span><span class="sxs-lookup"><span data-stu-id="57889-219">800</span></span> |
| <span data-ttu-id="57889-220">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-220">DW300</span></span> |<span data-ttu-id="57889-221">100</span><span class="sxs-lookup"><span data-stu-id="57889-221">100</span></span> |<span data-ttu-id="57889-222">200</span><span class="sxs-lookup"><span data-stu-id="57889-222">200</span></span> |<span data-ttu-id="57889-223">400</span><span class="sxs-lookup"><span data-stu-id="57889-223">400</span></span> |<span data-ttu-id="57889-224">800</span><span class="sxs-lookup"><span data-stu-id="57889-224">800</span></span> |
| <span data-ttu-id="57889-225">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-225">DW400</span></span> |<span data-ttu-id="57889-226">100</span><span class="sxs-lookup"><span data-stu-id="57889-226">100</span></span> |<span data-ttu-id="57889-227">400</span><span class="sxs-lookup"><span data-stu-id="57889-227">400</span></span> |<span data-ttu-id="57889-228">800</span><span class="sxs-lookup"><span data-stu-id="57889-228">800</span></span> |<span data-ttu-id="57889-229">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-229">1,600</span></span> |
| <span data-ttu-id="57889-230">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-230">DW500</span></span> |<span data-ttu-id="57889-231">100</span><span class="sxs-lookup"><span data-stu-id="57889-231">100</span></span> |<span data-ttu-id="57889-232">400</span><span class="sxs-lookup"><span data-stu-id="57889-232">400</span></span> |<span data-ttu-id="57889-233">800</span><span class="sxs-lookup"><span data-stu-id="57889-233">800</span></span> |<span data-ttu-id="57889-234">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-234">1,600</span></span> |
| <span data-ttu-id="57889-235">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-235">DW600</span></span> |<span data-ttu-id="57889-236">100</span><span class="sxs-lookup"><span data-stu-id="57889-236">100</span></span> |<span data-ttu-id="57889-237">400</span><span class="sxs-lookup"><span data-stu-id="57889-237">400</span></span> |<span data-ttu-id="57889-238">800</span><span class="sxs-lookup"><span data-stu-id="57889-238">800</span></span> |<span data-ttu-id="57889-239">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-239">1,600</span></span> |
| <span data-ttu-id="57889-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-240">DW1000</span></span> |<span data-ttu-id="57889-241">100</span><span class="sxs-lookup"><span data-stu-id="57889-241">100</span></span> |<span data-ttu-id="57889-242">800</span><span class="sxs-lookup"><span data-stu-id="57889-242">800</span></span> |<span data-ttu-id="57889-243">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-243">1,600</span></span> |<span data-ttu-id="57889-244">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-244">3,200</span></span> |
| <span data-ttu-id="57889-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-245">DW1200</span></span> |<span data-ttu-id="57889-246">100</span><span class="sxs-lookup"><span data-stu-id="57889-246">100</span></span> |<span data-ttu-id="57889-247">800</span><span class="sxs-lookup"><span data-stu-id="57889-247">800</span></span> |<span data-ttu-id="57889-248">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-248">1,600</span></span> |<span data-ttu-id="57889-249">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-249">3,200</span></span> |
| <span data-ttu-id="57889-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-250">DW1500</span></span> |<span data-ttu-id="57889-251">100</span><span class="sxs-lookup"><span data-stu-id="57889-251">100</span></span> |<span data-ttu-id="57889-252">800</span><span class="sxs-lookup"><span data-stu-id="57889-252">800</span></span> |<span data-ttu-id="57889-253">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-253">1,600</span></span> |<span data-ttu-id="57889-254">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-254">3,200</span></span> |
| <span data-ttu-id="57889-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-255">DW2000</span></span> |<span data-ttu-id="57889-256">100</span><span class="sxs-lookup"><span data-stu-id="57889-256">100</span></span> |<span data-ttu-id="57889-257">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-257">1,600</span></span> |<span data-ttu-id="57889-258">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-258">3,200</span></span> |<span data-ttu-id="57889-259">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-259">6,400</span></span> |
| <span data-ttu-id="57889-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-260">DW3000</span></span> |<span data-ttu-id="57889-261">100</span><span class="sxs-lookup"><span data-stu-id="57889-261">100</span></span> |<span data-ttu-id="57889-262">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-262">1,600</span></span> |<span data-ttu-id="57889-263">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-263">3,200</span></span> |<span data-ttu-id="57889-264">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-264">6,400</span></span> |
| <span data-ttu-id="57889-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-265">DW6000</span></span> |<span data-ttu-id="57889-266">100</span><span class="sxs-lookup"><span data-stu-id="57889-266">100</span></span> |<span data-ttu-id="57889-267">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-267">3,200</span></span> |<span data-ttu-id="57889-268">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-268">6,400</span></span> |<span data-ttu-id="57889-269">12,800</span><span class="sxs-lookup"><span data-stu-id="57889-269">12,800</span></span> |

<span data-ttu-id="57889-270">I följande tabell mappar det minne som allokerats till varje distribution av DWU och statiska resursklassen.</span><span class="sxs-lookup"><span data-stu-id="57889-270">The following table maps the memory allocated to each distribution by DWU and static resource class.</span></span> <span data-ttu-id="57889-271">Observera att klasserna högre resurs har sina minne minskas för att respektera övergripande DWU-gränser.</span><span class="sxs-lookup"><span data-stu-id="57889-271">Note that the higher resource classes have their memory reduced to honor the global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="57889-272">Minnesallokering per distribution för statiska resursklasser (MB)</span><span class="sxs-lookup"><span data-stu-id="57889-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="57889-273">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-273">DWU</span></span> | <span data-ttu-id="57889-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="57889-274">staticrc10</span></span> | <span data-ttu-id="57889-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="57889-275">staticrc20</span></span> | <span data-ttu-id="57889-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="57889-276">staticrc30</span></span> | <span data-ttu-id="57889-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="57889-277">staticrc40</span></span> | <span data-ttu-id="57889-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="57889-278">staticrc50</span></span> | <span data-ttu-id="57889-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="57889-279">staticrc60</span></span> | <span data-ttu-id="57889-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="57889-280">staticrc70</span></span> | <span data-ttu-id="57889-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="57889-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="57889-282">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-282">DW100</span></span> |<span data-ttu-id="57889-283">100</span><span class="sxs-lookup"><span data-stu-id="57889-283">100</span></span> |<span data-ttu-id="57889-284">200</span><span class="sxs-lookup"><span data-stu-id="57889-284">200</span></span> |<span data-ttu-id="57889-285">400</span><span class="sxs-lookup"><span data-stu-id="57889-285">400</span></span> |<span data-ttu-id="57889-286">400</span><span class="sxs-lookup"><span data-stu-id="57889-286">400</span></span> |<span data-ttu-id="57889-287">400</span><span class="sxs-lookup"><span data-stu-id="57889-287">400</span></span> |<span data-ttu-id="57889-288">400</span><span class="sxs-lookup"><span data-stu-id="57889-288">400</span></span> |<span data-ttu-id="57889-289">400</span><span class="sxs-lookup"><span data-stu-id="57889-289">400</span></span> |<span data-ttu-id="57889-290">400</span><span class="sxs-lookup"><span data-stu-id="57889-290">400</span></span> |
| <span data-ttu-id="57889-291">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-291">DW200</span></span> |<span data-ttu-id="57889-292">100</span><span class="sxs-lookup"><span data-stu-id="57889-292">100</span></span> |<span data-ttu-id="57889-293">200</span><span class="sxs-lookup"><span data-stu-id="57889-293">200</span></span> |<span data-ttu-id="57889-294">400</span><span class="sxs-lookup"><span data-stu-id="57889-294">400</span></span> |<span data-ttu-id="57889-295">800</span><span class="sxs-lookup"><span data-stu-id="57889-295">800</span></span> |<span data-ttu-id="57889-296">800</span><span class="sxs-lookup"><span data-stu-id="57889-296">800</span></span> |<span data-ttu-id="57889-297">800</span><span class="sxs-lookup"><span data-stu-id="57889-297">800</span></span> |<span data-ttu-id="57889-298">800</span><span class="sxs-lookup"><span data-stu-id="57889-298">800</span></span> |<span data-ttu-id="57889-299">800</span><span class="sxs-lookup"><span data-stu-id="57889-299">800</span></span> |
| <span data-ttu-id="57889-300">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-300">DW300</span></span> |<span data-ttu-id="57889-301">100</span><span class="sxs-lookup"><span data-stu-id="57889-301">100</span></span> |<span data-ttu-id="57889-302">200</span><span class="sxs-lookup"><span data-stu-id="57889-302">200</span></span> |<span data-ttu-id="57889-303">400</span><span class="sxs-lookup"><span data-stu-id="57889-303">400</span></span> |<span data-ttu-id="57889-304">800</span><span class="sxs-lookup"><span data-stu-id="57889-304">800</span></span> |<span data-ttu-id="57889-305">800</span><span class="sxs-lookup"><span data-stu-id="57889-305">800</span></span> |<span data-ttu-id="57889-306">800</span><span class="sxs-lookup"><span data-stu-id="57889-306">800</span></span> |<span data-ttu-id="57889-307">800</span><span class="sxs-lookup"><span data-stu-id="57889-307">800</span></span> |<span data-ttu-id="57889-308">800</span><span class="sxs-lookup"><span data-stu-id="57889-308">800</span></span> |
| <span data-ttu-id="57889-309">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-309">DW400</span></span> |<span data-ttu-id="57889-310">100</span><span class="sxs-lookup"><span data-stu-id="57889-310">100</span></span> |<span data-ttu-id="57889-311">200</span><span class="sxs-lookup"><span data-stu-id="57889-311">200</span></span> |<span data-ttu-id="57889-312">400</span><span class="sxs-lookup"><span data-stu-id="57889-312">400</span></span> |<span data-ttu-id="57889-313">800</span><span class="sxs-lookup"><span data-stu-id="57889-313">800</span></span> |<span data-ttu-id="57889-314">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-314">1,600</span></span> |<span data-ttu-id="57889-315">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-315">1,600</span></span> |<span data-ttu-id="57889-316">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-316">1,600</span></span> |<span data-ttu-id="57889-317">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-317">1,600</span></span> |
| <span data-ttu-id="57889-318">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-318">DW500</span></span> |<span data-ttu-id="57889-319">100</span><span class="sxs-lookup"><span data-stu-id="57889-319">100</span></span> |<span data-ttu-id="57889-320">200</span><span class="sxs-lookup"><span data-stu-id="57889-320">200</span></span> |<span data-ttu-id="57889-321">400</span><span class="sxs-lookup"><span data-stu-id="57889-321">400</span></span> |<span data-ttu-id="57889-322">800</span><span class="sxs-lookup"><span data-stu-id="57889-322">800</span></span> |<span data-ttu-id="57889-323">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-323">1,600</span></span> |<span data-ttu-id="57889-324">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-324">1,600</span></span> |<span data-ttu-id="57889-325">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-325">1,600</span></span> |<span data-ttu-id="57889-326">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-326">1,600</span></span> |
| <span data-ttu-id="57889-327">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-327">DW600</span></span> |<span data-ttu-id="57889-328">100</span><span class="sxs-lookup"><span data-stu-id="57889-328">100</span></span> |<span data-ttu-id="57889-329">200</span><span class="sxs-lookup"><span data-stu-id="57889-329">200</span></span> |<span data-ttu-id="57889-330">400</span><span class="sxs-lookup"><span data-stu-id="57889-330">400</span></span> |<span data-ttu-id="57889-331">800</span><span class="sxs-lookup"><span data-stu-id="57889-331">800</span></span> |<span data-ttu-id="57889-332">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-332">1,600</span></span> |<span data-ttu-id="57889-333">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-333">1,600</span></span> |<span data-ttu-id="57889-334">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-334">1,600</span></span> |<span data-ttu-id="57889-335">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-335">1,600</span></span> |
| <span data-ttu-id="57889-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-336">DW1000</span></span> |<span data-ttu-id="57889-337">100</span><span class="sxs-lookup"><span data-stu-id="57889-337">100</span></span> |<span data-ttu-id="57889-338">200</span><span class="sxs-lookup"><span data-stu-id="57889-338">200</span></span> |<span data-ttu-id="57889-339">400</span><span class="sxs-lookup"><span data-stu-id="57889-339">400</span></span> |<span data-ttu-id="57889-340">800</span><span class="sxs-lookup"><span data-stu-id="57889-340">800</span></span> |<span data-ttu-id="57889-341">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-341">1,600</span></span> |<span data-ttu-id="57889-342">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-342">3,200</span></span> |<span data-ttu-id="57889-343">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-343">3,200</span></span> |<span data-ttu-id="57889-344">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-344">3,200</span></span> |
| <span data-ttu-id="57889-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-345">DW1200</span></span> |<span data-ttu-id="57889-346">100</span><span class="sxs-lookup"><span data-stu-id="57889-346">100</span></span> |<span data-ttu-id="57889-347">200</span><span class="sxs-lookup"><span data-stu-id="57889-347">200</span></span> |<span data-ttu-id="57889-348">400</span><span class="sxs-lookup"><span data-stu-id="57889-348">400</span></span> |<span data-ttu-id="57889-349">800</span><span class="sxs-lookup"><span data-stu-id="57889-349">800</span></span> |<span data-ttu-id="57889-350">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-350">1,600</span></span> |<span data-ttu-id="57889-351">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-351">3,200</span></span> |<span data-ttu-id="57889-352">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-352">3,200</span></span> |<span data-ttu-id="57889-353">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-353">3,200</span></span> |
| <span data-ttu-id="57889-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-354">DW1500</span></span> |<span data-ttu-id="57889-355">100</span><span class="sxs-lookup"><span data-stu-id="57889-355">100</span></span> |<span data-ttu-id="57889-356">200</span><span class="sxs-lookup"><span data-stu-id="57889-356">200</span></span> |<span data-ttu-id="57889-357">400</span><span class="sxs-lookup"><span data-stu-id="57889-357">400</span></span> |<span data-ttu-id="57889-358">800</span><span class="sxs-lookup"><span data-stu-id="57889-358">800</span></span> |<span data-ttu-id="57889-359">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-359">1,600</span></span> |<span data-ttu-id="57889-360">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-360">3,200</span></span> |<span data-ttu-id="57889-361">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-361">3,200</span></span> |<span data-ttu-id="57889-362">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-362">3,200</span></span> |
| <span data-ttu-id="57889-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-363">DW2000</span></span> |<span data-ttu-id="57889-364">100</span><span class="sxs-lookup"><span data-stu-id="57889-364">100</span></span> |<span data-ttu-id="57889-365">200</span><span class="sxs-lookup"><span data-stu-id="57889-365">200</span></span> |<span data-ttu-id="57889-366">400</span><span class="sxs-lookup"><span data-stu-id="57889-366">400</span></span> |<span data-ttu-id="57889-367">800</span><span class="sxs-lookup"><span data-stu-id="57889-367">800</span></span> |<span data-ttu-id="57889-368">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-368">1,600</span></span> |<span data-ttu-id="57889-369">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-369">3,200</span></span> |<span data-ttu-id="57889-370">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-370">6,400</span></span> |<span data-ttu-id="57889-371">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-371">6,400</span></span> |
| <span data-ttu-id="57889-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-372">DW3000</span></span> |<span data-ttu-id="57889-373">100</span><span class="sxs-lookup"><span data-stu-id="57889-373">100</span></span> |<span data-ttu-id="57889-374">200</span><span class="sxs-lookup"><span data-stu-id="57889-374">200</span></span> |<span data-ttu-id="57889-375">400</span><span class="sxs-lookup"><span data-stu-id="57889-375">400</span></span> |<span data-ttu-id="57889-376">800</span><span class="sxs-lookup"><span data-stu-id="57889-376">800</span></span> |<span data-ttu-id="57889-377">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-377">1,600</span></span> |<span data-ttu-id="57889-378">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-378">3,200</span></span> |<span data-ttu-id="57889-379">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-379">6,400</span></span> |<span data-ttu-id="57889-380">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-380">6,400</span></span> |
| <span data-ttu-id="57889-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-381">DW6000</span></span> |<span data-ttu-id="57889-382">100</span><span class="sxs-lookup"><span data-stu-id="57889-382">100</span></span> |<span data-ttu-id="57889-383">200</span><span class="sxs-lookup"><span data-stu-id="57889-383">200</span></span> |<span data-ttu-id="57889-384">400</span><span class="sxs-lookup"><span data-stu-id="57889-384">400</span></span> |<span data-ttu-id="57889-385">800</span><span class="sxs-lookup"><span data-stu-id="57889-385">800</span></span> |<span data-ttu-id="57889-386">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-386">1,600</span></span> |<span data-ttu-id="57889-387">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-387">3,200</span></span> |<span data-ttu-id="57889-388">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-388">6,400</span></span> |<span data-ttu-id="57889-389">12,800</span><span class="sxs-lookup"><span data-stu-id="57889-389">12,800</span></span> |

<span data-ttu-id="57889-390">Från föregående tabell kan du se att en fråga som körs på en DW2000 i den **xlargerc** resursklassen skulle ha åtkomst till 6 400 MB minne inom 60 distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="57889-390">From the preceding table, you can see that a query running on a DW2000 in the **xlargerc** resource class would have access to 6,400 MB of memory within each of the 60 distributed databases.</span></span>  <span data-ttu-id="57889-391">Det finns 60 distributioner i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="57889-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="57889-392">För att beräkna den totala minnesallokering för en fråga i en viss resurs bör därför ovanstående värden multipliceras med 60.</span><span class="sxs-lookup"><span data-stu-id="57889-392">Therefore, to calculate the total memory allocation for a query in a given resource class, the above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="57889-393">Minne-allokeringar systemomfattande (GB)</span><span class="sxs-lookup"><span data-stu-id="57889-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="57889-394">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-394">DWU</span></span> | <span data-ttu-id="57889-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="57889-395">smallrc</span></span> | <span data-ttu-id="57889-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="57889-396">mediumrc</span></span> | <span data-ttu-id="57889-397">largerc</span><span class="sxs-lookup"><span data-stu-id="57889-397">largerc</span></span> | <span data-ttu-id="57889-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="57889-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="57889-399">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-399">DW100</span></span> |<span data-ttu-id="57889-400">6</span><span class="sxs-lookup"><span data-stu-id="57889-400">6</span></span> |<span data-ttu-id="57889-401">6</span><span class="sxs-lookup"><span data-stu-id="57889-401">6</span></span> |<span data-ttu-id="57889-402">12</span><span class="sxs-lookup"><span data-stu-id="57889-402">12</span></span> |<span data-ttu-id="57889-403">23</span><span class="sxs-lookup"><span data-stu-id="57889-403">23</span></span> |
| <span data-ttu-id="57889-404">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-404">DW200</span></span> |<span data-ttu-id="57889-405">6</span><span class="sxs-lookup"><span data-stu-id="57889-405">6</span></span> |<span data-ttu-id="57889-406">12</span><span class="sxs-lookup"><span data-stu-id="57889-406">12</span></span> |<span data-ttu-id="57889-407">23</span><span class="sxs-lookup"><span data-stu-id="57889-407">23</span></span> |<span data-ttu-id="57889-408">47</span><span class="sxs-lookup"><span data-stu-id="57889-408">47</span></span> |
| <span data-ttu-id="57889-409">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-409">DW300</span></span> |<span data-ttu-id="57889-410">6</span><span class="sxs-lookup"><span data-stu-id="57889-410">6</span></span> |<span data-ttu-id="57889-411">12</span><span class="sxs-lookup"><span data-stu-id="57889-411">12</span></span> |<span data-ttu-id="57889-412">23</span><span class="sxs-lookup"><span data-stu-id="57889-412">23</span></span> |<span data-ttu-id="57889-413">47</span><span class="sxs-lookup"><span data-stu-id="57889-413">47</span></span> |
| <span data-ttu-id="57889-414">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-414">DW400</span></span> |<span data-ttu-id="57889-415">6</span><span class="sxs-lookup"><span data-stu-id="57889-415">6</span></span> |<span data-ttu-id="57889-416">23</span><span class="sxs-lookup"><span data-stu-id="57889-416">23</span></span> |<span data-ttu-id="57889-417">47</span><span class="sxs-lookup"><span data-stu-id="57889-417">47</span></span> |<span data-ttu-id="57889-418">94</span><span class="sxs-lookup"><span data-stu-id="57889-418">94</span></span> |
| <span data-ttu-id="57889-419">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-419">DW500</span></span> |<span data-ttu-id="57889-420">6</span><span class="sxs-lookup"><span data-stu-id="57889-420">6</span></span> |<span data-ttu-id="57889-421">23</span><span class="sxs-lookup"><span data-stu-id="57889-421">23</span></span> |<span data-ttu-id="57889-422">47</span><span class="sxs-lookup"><span data-stu-id="57889-422">47</span></span> |<span data-ttu-id="57889-423">94</span><span class="sxs-lookup"><span data-stu-id="57889-423">94</span></span> |
| <span data-ttu-id="57889-424">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-424">DW600</span></span> |<span data-ttu-id="57889-425">6</span><span class="sxs-lookup"><span data-stu-id="57889-425">6</span></span> |<span data-ttu-id="57889-426">23</span><span class="sxs-lookup"><span data-stu-id="57889-426">23</span></span> |<span data-ttu-id="57889-427">47</span><span class="sxs-lookup"><span data-stu-id="57889-427">47</span></span> |<span data-ttu-id="57889-428">94</span><span class="sxs-lookup"><span data-stu-id="57889-428">94</span></span> |
| <span data-ttu-id="57889-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-429">DW1000</span></span> |<span data-ttu-id="57889-430">6</span><span class="sxs-lookup"><span data-stu-id="57889-430">6</span></span> |<span data-ttu-id="57889-431">47</span><span class="sxs-lookup"><span data-stu-id="57889-431">47</span></span> |<span data-ttu-id="57889-432">94</span><span class="sxs-lookup"><span data-stu-id="57889-432">94</span></span> |<span data-ttu-id="57889-433">188</span><span class="sxs-lookup"><span data-stu-id="57889-433">188</span></span> |
| <span data-ttu-id="57889-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-434">DW1200</span></span> |<span data-ttu-id="57889-435">6</span><span class="sxs-lookup"><span data-stu-id="57889-435">6</span></span> |<span data-ttu-id="57889-436">47</span><span class="sxs-lookup"><span data-stu-id="57889-436">47</span></span> |<span data-ttu-id="57889-437">94</span><span class="sxs-lookup"><span data-stu-id="57889-437">94</span></span> |<span data-ttu-id="57889-438">188</span><span class="sxs-lookup"><span data-stu-id="57889-438">188</span></span> |
| <span data-ttu-id="57889-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-439">DW1500</span></span> |<span data-ttu-id="57889-440">6</span><span class="sxs-lookup"><span data-stu-id="57889-440">6</span></span> |<span data-ttu-id="57889-441">47</span><span class="sxs-lookup"><span data-stu-id="57889-441">47</span></span> |<span data-ttu-id="57889-442">94</span><span class="sxs-lookup"><span data-stu-id="57889-442">94</span></span> |<span data-ttu-id="57889-443">188</span><span class="sxs-lookup"><span data-stu-id="57889-443">188</span></span> |
| <span data-ttu-id="57889-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-444">DW2000</span></span> |<span data-ttu-id="57889-445">6</span><span class="sxs-lookup"><span data-stu-id="57889-445">6</span></span> |<span data-ttu-id="57889-446">94</span><span class="sxs-lookup"><span data-stu-id="57889-446">94</span></span> |<span data-ttu-id="57889-447">188</span><span class="sxs-lookup"><span data-stu-id="57889-447">188</span></span> |<span data-ttu-id="57889-448">375</span><span class="sxs-lookup"><span data-stu-id="57889-448">375</span></span> |
| <span data-ttu-id="57889-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-449">DW3000</span></span> |<span data-ttu-id="57889-450">6</span><span class="sxs-lookup"><span data-stu-id="57889-450">6</span></span> |<span data-ttu-id="57889-451">94</span><span class="sxs-lookup"><span data-stu-id="57889-451">94</span></span> |<span data-ttu-id="57889-452">188</span><span class="sxs-lookup"><span data-stu-id="57889-452">188</span></span> |<span data-ttu-id="57889-453">375</span><span class="sxs-lookup"><span data-stu-id="57889-453">375</span></span> |
| <span data-ttu-id="57889-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-454">DW6000</span></span> |<span data-ttu-id="57889-455">6</span><span class="sxs-lookup"><span data-stu-id="57889-455">6</span></span> |<span data-ttu-id="57889-456">188</span><span class="sxs-lookup"><span data-stu-id="57889-456">188</span></span> |<span data-ttu-id="57889-457">375</span><span class="sxs-lookup"><span data-stu-id="57889-457">375</span></span> |<span data-ttu-id="57889-458">750</span><span class="sxs-lookup"><span data-stu-id="57889-458">750</span></span> |

<span data-ttu-id="57889-459">Från den här tabellen systemomfattande minnesallokering, ser du en fråga som körs på en DW2000 i klassen xlargerc resurs tilldelas 375 GB minne totalt (6 400 MB * 60 distributioner / 1 024 att konvertera till GB) över hela ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="57889-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in the xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 to convert to GB) over the entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="57889-460">Samma beräkning gäller statiska resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-460">The same calculation applies to static resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="57889-461">Concurrency fack förbrukning</span><span class="sxs-lookup"><span data-stu-id="57889-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="57889-462">SQL Data Warehouse ger mer minne till frågor som körs i högre resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-462">SQL Data Warehouse grants more memory to queries running in higher resource classes.</span></span> <span data-ttu-id="57889-463">Minnet är en fast resurs.</span><span class="sxs-lookup"><span data-stu-id="57889-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="57889-464">Därför mer minne som allokerats per fråga, färre samtidiga frågor kan köra.</span><span class="sxs-lookup"><span data-stu-id="57889-464">Therefore, the more memory allocated per query, the fewer concurrent queries can execute.</span></span> <span data-ttu-id="57889-465">I följande tabell reiterates alla tidigare begreppen i en enda vy som visar antalet samtidiga fack som är tillgängliga genom DWU och de platser som används av varje resursklassen.</span><span class="sxs-lookup"><span data-stu-id="57889-465">The following table reiterates all of the previous concepts in a single view that shows the number of concurrency slots available by DWU and the slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="57889-466">Fördelningen och förbrukningen av samtidighet fack för dynamisk resursklasser</span><span class="sxs-lookup"><span data-stu-id="57889-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="57889-467">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-467">DWU</span></span> | <span data-ttu-id="57889-468">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="57889-468">Maximum concurrent queries</span></span> | <span data-ttu-id="57889-469">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="57889-469">Concurrency slots allocated</span></span> | <span data-ttu-id="57889-470">Platser som används av smallrc</span><span class="sxs-lookup"><span data-stu-id="57889-470">Slots used by smallrc</span></span> | <span data-ttu-id="57889-471">Platser som används av mediumrc</span><span class="sxs-lookup"><span data-stu-id="57889-471">Slots used by mediumrc</span></span> | <span data-ttu-id="57889-472">Platser som används av largerc</span><span class="sxs-lookup"><span data-stu-id="57889-472">Slots used by largerc</span></span> | <span data-ttu-id="57889-473">Platser som används av xlargerc</span><span class="sxs-lookup"><span data-stu-id="57889-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="57889-474">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-474">DW100</span></span> |<span data-ttu-id="57889-475">4</span><span class="sxs-lookup"><span data-stu-id="57889-475">4</span></span> |<span data-ttu-id="57889-476">4</span><span class="sxs-lookup"><span data-stu-id="57889-476">4</span></span> |<span data-ttu-id="57889-477">1</span><span class="sxs-lookup"><span data-stu-id="57889-477">1</span></span> |<span data-ttu-id="57889-478">1</span><span class="sxs-lookup"><span data-stu-id="57889-478">1</span></span> |<span data-ttu-id="57889-479">2</span><span class="sxs-lookup"><span data-stu-id="57889-479">2</span></span> |<span data-ttu-id="57889-480">4</span><span class="sxs-lookup"><span data-stu-id="57889-480">4</span></span> |
| <span data-ttu-id="57889-481">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-481">DW200</span></span> |<span data-ttu-id="57889-482">8</span><span class="sxs-lookup"><span data-stu-id="57889-482">8</span></span> |<span data-ttu-id="57889-483">8</span><span class="sxs-lookup"><span data-stu-id="57889-483">8</span></span> |<span data-ttu-id="57889-484">1</span><span class="sxs-lookup"><span data-stu-id="57889-484">1</span></span> |<span data-ttu-id="57889-485">2</span><span class="sxs-lookup"><span data-stu-id="57889-485">2</span></span> |<span data-ttu-id="57889-486">4</span><span class="sxs-lookup"><span data-stu-id="57889-486">4</span></span> |<span data-ttu-id="57889-487">8</span><span class="sxs-lookup"><span data-stu-id="57889-487">8</span></span> |
| <span data-ttu-id="57889-488">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-488">DW300</span></span> |<span data-ttu-id="57889-489">12</span><span class="sxs-lookup"><span data-stu-id="57889-489">12</span></span> |<span data-ttu-id="57889-490">12</span><span class="sxs-lookup"><span data-stu-id="57889-490">12</span></span> |<span data-ttu-id="57889-491">1</span><span class="sxs-lookup"><span data-stu-id="57889-491">1</span></span> |<span data-ttu-id="57889-492">2</span><span class="sxs-lookup"><span data-stu-id="57889-492">2</span></span> |<span data-ttu-id="57889-493">4</span><span class="sxs-lookup"><span data-stu-id="57889-493">4</span></span> |<span data-ttu-id="57889-494">8</span><span class="sxs-lookup"><span data-stu-id="57889-494">8</span></span> |
| <span data-ttu-id="57889-495">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-495">DW400</span></span> |<span data-ttu-id="57889-496">16</span><span class="sxs-lookup"><span data-stu-id="57889-496">16</span></span> |<span data-ttu-id="57889-497">16</span><span class="sxs-lookup"><span data-stu-id="57889-497">16</span></span> |<span data-ttu-id="57889-498">1</span><span class="sxs-lookup"><span data-stu-id="57889-498">1</span></span> |<span data-ttu-id="57889-499">4</span><span class="sxs-lookup"><span data-stu-id="57889-499">4</span></span> |<span data-ttu-id="57889-500">8</span><span class="sxs-lookup"><span data-stu-id="57889-500">8</span></span> |<span data-ttu-id="57889-501">16</span><span class="sxs-lookup"><span data-stu-id="57889-501">16</span></span> |
| <span data-ttu-id="57889-502">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-502">DW500</span></span> |<span data-ttu-id="57889-503">20</span><span class="sxs-lookup"><span data-stu-id="57889-503">20</span></span> |<span data-ttu-id="57889-504">20</span><span class="sxs-lookup"><span data-stu-id="57889-504">20</span></span> |<span data-ttu-id="57889-505">1</span><span class="sxs-lookup"><span data-stu-id="57889-505">1</span></span> |<span data-ttu-id="57889-506">4</span><span class="sxs-lookup"><span data-stu-id="57889-506">4</span></span> |<span data-ttu-id="57889-507">8</span><span class="sxs-lookup"><span data-stu-id="57889-507">8</span></span> |<span data-ttu-id="57889-508">16</span><span class="sxs-lookup"><span data-stu-id="57889-508">16</span></span> |
| <span data-ttu-id="57889-509">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-509">DW600</span></span> |<span data-ttu-id="57889-510">24</span><span class="sxs-lookup"><span data-stu-id="57889-510">24</span></span> |<span data-ttu-id="57889-511">24</span><span class="sxs-lookup"><span data-stu-id="57889-511">24</span></span> |<span data-ttu-id="57889-512">1</span><span class="sxs-lookup"><span data-stu-id="57889-512">1</span></span> |<span data-ttu-id="57889-513">4</span><span class="sxs-lookup"><span data-stu-id="57889-513">4</span></span> |<span data-ttu-id="57889-514">8</span><span class="sxs-lookup"><span data-stu-id="57889-514">8</span></span> |<span data-ttu-id="57889-515">16</span><span class="sxs-lookup"><span data-stu-id="57889-515">16</span></span> |
| <span data-ttu-id="57889-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-516">DW1000</span></span> |<span data-ttu-id="57889-517">32</span><span class="sxs-lookup"><span data-stu-id="57889-517">32</span></span> |<span data-ttu-id="57889-518">40</span><span class="sxs-lookup"><span data-stu-id="57889-518">40</span></span> |<span data-ttu-id="57889-519">1</span><span class="sxs-lookup"><span data-stu-id="57889-519">1</span></span> |<span data-ttu-id="57889-520">8</span><span class="sxs-lookup"><span data-stu-id="57889-520">8</span></span> |<span data-ttu-id="57889-521">16</span><span class="sxs-lookup"><span data-stu-id="57889-521">16</span></span> |<span data-ttu-id="57889-522">32</span><span class="sxs-lookup"><span data-stu-id="57889-522">32</span></span> |
| <span data-ttu-id="57889-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-523">DW1200</span></span> |<span data-ttu-id="57889-524">32</span><span class="sxs-lookup"><span data-stu-id="57889-524">32</span></span> |<span data-ttu-id="57889-525">48</span><span class="sxs-lookup"><span data-stu-id="57889-525">48</span></span> |<span data-ttu-id="57889-526">1</span><span class="sxs-lookup"><span data-stu-id="57889-526">1</span></span> |<span data-ttu-id="57889-527">8</span><span class="sxs-lookup"><span data-stu-id="57889-527">8</span></span> |<span data-ttu-id="57889-528">16</span><span class="sxs-lookup"><span data-stu-id="57889-528">16</span></span> |<span data-ttu-id="57889-529">32</span><span class="sxs-lookup"><span data-stu-id="57889-529">32</span></span> |
| <span data-ttu-id="57889-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-530">DW1500</span></span> |<span data-ttu-id="57889-531">32</span><span class="sxs-lookup"><span data-stu-id="57889-531">32</span></span> |<span data-ttu-id="57889-532">60</span><span class="sxs-lookup"><span data-stu-id="57889-532">60</span></span> |<span data-ttu-id="57889-533">1</span><span class="sxs-lookup"><span data-stu-id="57889-533">1</span></span> |<span data-ttu-id="57889-534">8</span><span class="sxs-lookup"><span data-stu-id="57889-534">8</span></span> |<span data-ttu-id="57889-535">16</span><span class="sxs-lookup"><span data-stu-id="57889-535">16</span></span> |<span data-ttu-id="57889-536">32</span><span class="sxs-lookup"><span data-stu-id="57889-536">32</span></span> |
| <span data-ttu-id="57889-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-537">DW2000</span></span> |<span data-ttu-id="57889-538">32</span><span class="sxs-lookup"><span data-stu-id="57889-538">32</span></span> |<span data-ttu-id="57889-539">80</span><span class="sxs-lookup"><span data-stu-id="57889-539">80</span></span> |<span data-ttu-id="57889-540">1</span><span class="sxs-lookup"><span data-stu-id="57889-540">1</span></span> |<span data-ttu-id="57889-541">16</span><span class="sxs-lookup"><span data-stu-id="57889-541">16</span></span> |<span data-ttu-id="57889-542">32</span><span class="sxs-lookup"><span data-stu-id="57889-542">32</span></span> |<span data-ttu-id="57889-543">64</span><span class="sxs-lookup"><span data-stu-id="57889-543">64</span></span> |
| <span data-ttu-id="57889-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-544">DW3000</span></span> |<span data-ttu-id="57889-545">32</span><span class="sxs-lookup"><span data-stu-id="57889-545">32</span></span> |<span data-ttu-id="57889-546">120</span><span class="sxs-lookup"><span data-stu-id="57889-546">120</span></span> |<span data-ttu-id="57889-547">1</span><span class="sxs-lookup"><span data-stu-id="57889-547">1</span></span> |<span data-ttu-id="57889-548">16</span><span class="sxs-lookup"><span data-stu-id="57889-548">16</span></span> |<span data-ttu-id="57889-549">32</span><span class="sxs-lookup"><span data-stu-id="57889-549">32</span></span> |<span data-ttu-id="57889-550">64</span><span class="sxs-lookup"><span data-stu-id="57889-550">64</span></span> |
| <span data-ttu-id="57889-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-551">DW6000</span></span> |<span data-ttu-id="57889-552">32</span><span class="sxs-lookup"><span data-stu-id="57889-552">32</span></span> |<span data-ttu-id="57889-553">240</span><span class="sxs-lookup"><span data-stu-id="57889-553">240</span></span> |<span data-ttu-id="57889-554">1</span><span class="sxs-lookup"><span data-stu-id="57889-554">1</span></span> |<span data-ttu-id="57889-555">32</span><span class="sxs-lookup"><span data-stu-id="57889-555">32</span></span> |<span data-ttu-id="57889-556">64</span><span class="sxs-lookup"><span data-stu-id="57889-556">64</span></span> |<span data-ttu-id="57889-557">128</span><span class="sxs-lookup"><span data-stu-id="57889-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="57889-558">Fördelningen och förbrukningen av samtidighet fack för statiska resursklasser</span><span class="sxs-lookup"><span data-stu-id="57889-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="57889-559">DWU</span><span class="sxs-lookup"><span data-stu-id="57889-559">DWU</span></span> | <span data-ttu-id="57889-560">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="57889-560">Maximum concurrent queries</span></span> | <span data-ttu-id="57889-561">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="57889-561">Concurrency slots allocated</span></span> |<span data-ttu-id="57889-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="57889-562">staticrc10</span></span> | <span data-ttu-id="57889-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="57889-563">staticrc20</span></span> | <span data-ttu-id="57889-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="57889-564">staticrc30</span></span> | <span data-ttu-id="57889-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="57889-565">staticrc40</span></span> | <span data-ttu-id="57889-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="57889-566">staticrc50</span></span> | <span data-ttu-id="57889-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="57889-567">staticrc60</span></span> | <span data-ttu-id="57889-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="57889-568">staticrc70</span></span> | <span data-ttu-id="57889-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="57889-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="57889-570">DW100</span><span class="sxs-lookup"><span data-stu-id="57889-570">DW100</span></span> |<span data-ttu-id="57889-571">4</span><span class="sxs-lookup"><span data-stu-id="57889-571">4</span></span> |<span data-ttu-id="57889-572">4</span><span class="sxs-lookup"><span data-stu-id="57889-572">4</span></span> |<span data-ttu-id="57889-573">1</span><span class="sxs-lookup"><span data-stu-id="57889-573">1</span></span> |<span data-ttu-id="57889-574">2</span><span class="sxs-lookup"><span data-stu-id="57889-574">2</span></span> |<span data-ttu-id="57889-575">4</span><span class="sxs-lookup"><span data-stu-id="57889-575">4</span></span> |<span data-ttu-id="57889-576">4</span><span class="sxs-lookup"><span data-stu-id="57889-576">4</span></span> |<span data-ttu-id="57889-577">4</span><span class="sxs-lookup"><span data-stu-id="57889-577">4</span></span> |<span data-ttu-id="57889-578">4</span><span class="sxs-lookup"><span data-stu-id="57889-578">4</span></span> |<span data-ttu-id="57889-579">4</span><span class="sxs-lookup"><span data-stu-id="57889-579">4</span></span> |<span data-ttu-id="57889-580">4</span><span class="sxs-lookup"><span data-stu-id="57889-580">4</span></span> |
| <span data-ttu-id="57889-581">DW200</span><span class="sxs-lookup"><span data-stu-id="57889-581">DW200</span></span> |<span data-ttu-id="57889-582">8</span><span class="sxs-lookup"><span data-stu-id="57889-582">8</span></span> |<span data-ttu-id="57889-583">8</span><span class="sxs-lookup"><span data-stu-id="57889-583">8</span></span> |<span data-ttu-id="57889-584">1</span><span class="sxs-lookup"><span data-stu-id="57889-584">1</span></span> |<span data-ttu-id="57889-585">2</span><span class="sxs-lookup"><span data-stu-id="57889-585">2</span></span> |<span data-ttu-id="57889-586">4</span><span class="sxs-lookup"><span data-stu-id="57889-586">4</span></span> |<span data-ttu-id="57889-587">8</span><span class="sxs-lookup"><span data-stu-id="57889-587">8</span></span> |<span data-ttu-id="57889-588">8</span><span class="sxs-lookup"><span data-stu-id="57889-588">8</span></span> |<span data-ttu-id="57889-589">8</span><span class="sxs-lookup"><span data-stu-id="57889-589">8</span></span> |<span data-ttu-id="57889-590">8</span><span class="sxs-lookup"><span data-stu-id="57889-590">8</span></span> |<span data-ttu-id="57889-591">8</span><span class="sxs-lookup"><span data-stu-id="57889-591">8</span></span> |
| <span data-ttu-id="57889-592">DW300</span><span class="sxs-lookup"><span data-stu-id="57889-592">DW300</span></span> |<span data-ttu-id="57889-593">12</span><span class="sxs-lookup"><span data-stu-id="57889-593">12</span></span> |<span data-ttu-id="57889-594">12</span><span class="sxs-lookup"><span data-stu-id="57889-594">12</span></span> |<span data-ttu-id="57889-595">1</span><span class="sxs-lookup"><span data-stu-id="57889-595">1</span></span> |<span data-ttu-id="57889-596">2</span><span class="sxs-lookup"><span data-stu-id="57889-596">2</span></span> |<span data-ttu-id="57889-597">4</span><span class="sxs-lookup"><span data-stu-id="57889-597">4</span></span> |<span data-ttu-id="57889-598">8</span><span class="sxs-lookup"><span data-stu-id="57889-598">8</span></span> |<span data-ttu-id="57889-599">8</span><span class="sxs-lookup"><span data-stu-id="57889-599">8</span></span> |<span data-ttu-id="57889-600">8</span><span class="sxs-lookup"><span data-stu-id="57889-600">8</span></span> |<span data-ttu-id="57889-601">8</span><span class="sxs-lookup"><span data-stu-id="57889-601">8</span></span> |<span data-ttu-id="57889-602">8</span><span class="sxs-lookup"><span data-stu-id="57889-602">8</span></span> |
| <span data-ttu-id="57889-603">DW400</span><span class="sxs-lookup"><span data-stu-id="57889-603">DW400</span></span> |<span data-ttu-id="57889-604">16</span><span class="sxs-lookup"><span data-stu-id="57889-604">16</span></span> |<span data-ttu-id="57889-605">16</span><span class="sxs-lookup"><span data-stu-id="57889-605">16</span></span> |<span data-ttu-id="57889-606">1</span><span class="sxs-lookup"><span data-stu-id="57889-606">1</span></span> |<span data-ttu-id="57889-607">2</span><span class="sxs-lookup"><span data-stu-id="57889-607">2</span></span> |<span data-ttu-id="57889-608">4</span><span class="sxs-lookup"><span data-stu-id="57889-608">4</span></span> |<span data-ttu-id="57889-609">8</span><span class="sxs-lookup"><span data-stu-id="57889-609">8</span></span> |<span data-ttu-id="57889-610">16</span><span class="sxs-lookup"><span data-stu-id="57889-610">16</span></span> |<span data-ttu-id="57889-611">16</span><span class="sxs-lookup"><span data-stu-id="57889-611">16</span></span> |<span data-ttu-id="57889-612">16</span><span class="sxs-lookup"><span data-stu-id="57889-612">16</span></span> |<span data-ttu-id="57889-613">16</span><span class="sxs-lookup"><span data-stu-id="57889-613">16</span></span> |
| <span data-ttu-id="57889-614">DW500</span><span class="sxs-lookup"><span data-stu-id="57889-614">DW500</span></span> | <span data-ttu-id="57889-615">20</span><span class="sxs-lookup"><span data-stu-id="57889-615">20</span></span>| <span data-ttu-id="57889-616">20</span><span class="sxs-lookup"><span data-stu-id="57889-616">20</span></span>| <span data-ttu-id="57889-617">1</span><span class="sxs-lookup"><span data-stu-id="57889-617">1</span></span>| <span data-ttu-id="57889-618">2</span><span class="sxs-lookup"><span data-stu-id="57889-618">2</span></span>| <span data-ttu-id="57889-619">4</span><span class="sxs-lookup"><span data-stu-id="57889-619">4</span></span>| <span data-ttu-id="57889-620">8</span><span class="sxs-lookup"><span data-stu-id="57889-620">8</span></span>| <span data-ttu-id="57889-621">16</span><span class="sxs-lookup"><span data-stu-id="57889-621">16</span></span>| <span data-ttu-id="57889-622">16</span><span class="sxs-lookup"><span data-stu-id="57889-622">16</span></span>| <span data-ttu-id="57889-623">16</span><span class="sxs-lookup"><span data-stu-id="57889-623">16</span></span>| <span data-ttu-id="57889-624">16</span><span class="sxs-lookup"><span data-stu-id="57889-624">16</span></span>|
| <span data-ttu-id="57889-625">DW600</span><span class="sxs-lookup"><span data-stu-id="57889-625">DW600</span></span> | <span data-ttu-id="57889-626">24</span><span class="sxs-lookup"><span data-stu-id="57889-626">24</span></span>| <span data-ttu-id="57889-627">24</span><span class="sxs-lookup"><span data-stu-id="57889-627">24</span></span>| <span data-ttu-id="57889-628">1</span><span class="sxs-lookup"><span data-stu-id="57889-628">1</span></span>| <span data-ttu-id="57889-629">2</span><span class="sxs-lookup"><span data-stu-id="57889-629">2</span></span>| <span data-ttu-id="57889-630">4</span><span class="sxs-lookup"><span data-stu-id="57889-630">4</span></span>| <span data-ttu-id="57889-631">8</span><span class="sxs-lookup"><span data-stu-id="57889-631">8</span></span>| <span data-ttu-id="57889-632">16</span><span class="sxs-lookup"><span data-stu-id="57889-632">16</span></span>| <span data-ttu-id="57889-633">16</span><span class="sxs-lookup"><span data-stu-id="57889-633">16</span></span>| <span data-ttu-id="57889-634">16</span><span class="sxs-lookup"><span data-stu-id="57889-634">16</span></span>| <span data-ttu-id="57889-635">16</span><span class="sxs-lookup"><span data-stu-id="57889-635">16</span></span>|
| <span data-ttu-id="57889-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="57889-636">DW1000</span></span> | <span data-ttu-id="57889-637">32</span><span class="sxs-lookup"><span data-stu-id="57889-637">32</span></span>| <span data-ttu-id="57889-638">40</span><span class="sxs-lookup"><span data-stu-id="57889-638">40</span></span>| <span data-ttu-id="57889-639">1</span><span class="sxs-lookup"><span data-stu-id="57889-639">1</span></span>| <span data-ttu-id="57889-640">2</span><span class="sxs-lookup"><span data-stu-id="57889-640">2</span></span>| <span data-ttu-id="57889-641">4</span><span class="sxs-lookup"><span data-stu-id="57889-641">4</span></span>| <span data-ttu-id="57889-642">8</span><span class="sxs-lookup"><span data-stu-id="57889-642">8</span></span>| <span data-ttu-id="57889-643">16</span><span class="sxs-lookup"><span data-stu-id="57889-643">16</span></span>| <span data-ttu-id="57889-644">32</span><span class="sxs-lookup"><span data-stu-id="57889-644">32</span></span>| <span data-ttu-id="57889-645">32</span><span class="sxs-lookup"><span data-stu-id="57889-645">32</span></span>| <span data-ttu-id="57889-646">32</span><span class="sxs-lookup"><span data-stu-id="57889-646">32</span></span>|
| <span data-ttu-id="57889-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="57889-647">DW1200</span></span> | <span data-ttu-id="57889-648">32</span><span class="sxs-lookup"><span data-stu-id="57889-648">32</span></span>| <span data-ttu-id="57889-649">48</span><span class="sxs-lookup"><span data-stu-id="57889-649">48</span></span>| <span data-ttu-id="57889-650">1</span><span class="sxs-lookup"><span data-stu-id="57889-650">1</span></span>| <span data-ttu-id="57889-651">2</span><span class="sxs-lookup"><span data-stu-id="57889-651">2</span></span>| <span data-ttu-id="57889-652">4</span><span class="sxs-lookup"><span data-stu-id="57889-652">4</span></span>| <span data-ttu-id="57889-653">8</span><span class="sxs-lookup"><span data-stu-id="57889-653">8</span></span>| <span data-ttu-id="57889-654">16</span><span class="sxs-lookup"><span data-stu-id="57889-654">16</span></span>| <span data-ttu-id="57889-655">32</span><span class="sxs-lookup"><span data-stu-id="57889-655">32</span></span>| <span data-ttu-id="57889-656">32</span><span class="sxs-lookup"><span data-stu-id="57889-656">32</span></span>| <span data-ttu-id="57889-657">32</span><span class="sxs-lookup"><span data-stu-id="57889-657">32</span></span>|
| <span data-ttu-id="57889-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="57889-658">DW1500</span></span> | <span data-ttu-id="57889-659">32</span><span class="sxs-lookup"><span data-stu-id="57889-659">32</span></span>| <span data-ttu-id="57889-660">60</span><span class="sxs-lookup"><span data-stu-id="57889-660">60</span></span>| <span data-ttu-id="57889-661">1</span><span class="sxs-lookup"><span data-stu-id="57889-661">1</span></span>| <span data-ttu-id="57889-662">2</span><span class="sxs-lookup"><span data-stu-id="57889-662">2</span></span>| <span data-ttu-id="57889-663">4</span><span class="sxs-lookup"><span data-stu-id="57889-663">4</span></span>| <span data-ttu-id="57889-664">8</span><span class="sxs-lookup"><span data-stu-id="57889-664">8</span></span>| <span data-ttu-id="57889-665">16</span><span class="sxs-lookup"><span data-stu-id="57889-665">16</span></span>| <span data-ttu-id="57889-666">32</span><span class="sxs-lookup"><span data-stu-id="57889-666">32</span></span>| <span data-ttu-id="57889-667">32</span><span class="sxs-lookup"><span data-stu-id="57889-667">32</span></span>| <span data-ttu-id="57889-668">32</span><span class="sxs-lookup"><span data-stu-id="57889-668">32</span></span>|
| <span data-ttu-id="57889-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="57889-669">DW2000</span></span> | <span data-ttu-id="57889-670">32</span><span class="sxs-lookup"><span data-stu-id="57889-670">32</span></span>| <span data-ttu-id="57889-671">80</span><span class="sxs-lookup"><span data-stu-id="57889-671">80</span></span>| <span data-ttu-id="57889-672">1</span><span class="sxs-lookup"><span data-stu-id="57889-672">1</span></span>| <span data-ttu-id="57889-673">2</span><span class="sxs-lookup"><span data-stu-id="57889-673">2</span></span>| <span data-ttu-id="57889-674">4</span><span class="sxs-lookup"><span data-stu-id="57889-674">4</span></span>| <span data-ttu-id="57889-675">8</span><span class="sxs-lookup"><span data-stu-id="57889-675">8</span></span>| <span data-ttu-id="57889-676">16</span><span class="sxs-lookup"><span data-stu-id="57889-676">16</span></span>| <span data-ttu-id="57889-677">32</span><span class="sxs-lookup"><span data-stu-id="57889-677">32</span></span>| <span data-ttu-id="57889-678">64</span><span class="sxs-lookup"><span data-stu-id="57889-678">64</span></span>| <span data-ttu-id="57889-679">64</span><span class="sxs-lookup"><span data-stu-id="57889-679">64</span></span>|
| <span data-ttu-id="57889-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="57889-680">DW3000</span></span> | <span data-ttu-id="57889-681">32</span><span class="sxs-lookup"><span data-stu-id="57889-681">32</span></span>| <span data-ttu-id="57889-682">120</span><span class="sxs-lookup"><span data-stu-id="57889-682">120</span></span>| <span data-ttu-id="57889-683">1</span><span class="sxs-lookup"><span data-stu-id="57889-683">1</span></span>| <span data-ttu-id="57889-684">2</span><span class="sxs-lookup"><span data-stu-id="57889-684">2</span></span>| <span data-ttu-id="57889-685">4</span><span class="sxs-lookup"><span data-stu-id="57889-685">4</span></span>| <span data-ttu-id="57889-686">8</span><span class="sxs-lookup"><span data-stu-id="57889-686">8</span></span>| <span data-ttu-id="57889-687">16</span><span class="sxs-lookup"><span data-stu-id="57889-687">16</span></span>| <span data-ttu-id="57889-688">32</span><span class="sxs-lookup"><span data-stu-id="57889-688">32</span></span>| <span data-ttu-id="57889-689">64</span><span class="sxs-lookup"><span data-stu-id="57889-689">64</span></span>| <span data-ttu-id="57889-690">64</span><span class="sxs-lookup"><span data-stu-id="57889-690">64</span></span>|
| <span data-ttu-id="57889-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="57889-691">DW6000</span></span> | <span data-ttu-id="57889-692">32</span><span class="sxs-lookup"><span data-stu-id="57889-692">32</span></span>| <span data-ttu-id="57889-693">240</span><span class="sxs-lookup"><span data-stu-id="57889-693">240</span></span>| <span data-ttu-id="57889-694">1</span><span class="sxs-lookup"><span data-stu-id="57889-694">1</span></span>| <span data-ttu-id="57889-695">2</span><span class="sxs-lookup"><span data-stu-id="57889-695">2</span></span>| <span data-ttu-id="57889-696">4</span><span class="sxs-lookup"><span data-stu-id="57889-696">4</span></span>| <span data-ttu-id="57889-697">8</span><span class="sxs-lookup"><span data-stu-id="57889-697">8</span></span>| <span data-ttu-id="57889-698">16</span><span class="sxs-lookup"><span data-stu-id="57889-698">16</span></span>| <span data-ttu-id="57889-699">32</span><span class="sxs-lookup"><span data-stu-id="57889-699">32</span></span>| <span data-ttu-id="57889-700">64</span><span class="sxs-lookup"><span data-stu-id="57889-700">64</span></span>| <span data-ttu-id="57889-701">128</span><span class="sxs-lookup"><span data-stu-id="57889-701">128</span></span>|

<span data-ttu-id="57889-702">Du kan se som kör SQL Data Warehouse som DW1000 allokerar högst 32 samtidiga frågor och totalt 40 samtidighet fack från dessa tabeller.</span><span class="sxs-lookup"><span data-stu-id="57889-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="57889-703">Om alla användare kör i smallrc, skulle 32 samtidiga förfrågningar tillåts eftersom varje fråga förbrukar 1 samtidighet plats.</span><span class="sxs-lookup"><span data-stu-id="57889-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="57889-704">Om alla användare på en DW1000 kördes i mediumrc, varje fråga skulle allokeras 800 MB per distribution för en totala minnesallokering 47 GB per fråga och samtidighet skulle vara begränsad till 5 användare (40 samtidighet fack / 8 kortplatser per mediumrc användare).</span><span class="sxs-lookup"><span data-stu-id="57889-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited to 5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="57889-705">Att välja rätt resursklassen</span><span class="sxs-lookup"><span data-stu-id="57889-705">Selecting proper resource class</span></span>  
<span data-ttu-id="57889-706">Det är en bra idé att permanent tilldela användare till en resursklass i stället för att ändra deras resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-706">A good practice is to permanently assign users to a resource class rather than changing their resource classes.</span></span> <span data-ttu-id="57889-707">Till exempel skapa belastningar grupperade columnstore-tabeller högre kvalitet index när mer minne allokeras.</span><span class="sxs-lookup"><span data-stu-id="57889-707">For example, loads to clustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="57889-708">Skapa en användare för att överföra data och permanent tilldela användaren till en högre resursklassen för att säkerställa att belastningen har åtkomst till högre minne.</span><span class="sxs-lookup"><span data-stu-id="57889-708">To ensure that loads have access to higher memory, create a user specifically for loading data and permanently assign this user to a higher resource class.</span></span>
<span data-ttu-id="57889-709">Det finns ett antal rekommendationer för här.</span><span class="sxs-lookup"><span data-stu-id="57889-709">There are a couple of best practices to follow here.</span></span> <span data-ttu-id="57889-710">Som nämnts ovan är SQL DW stöder två typer av klassen resurstyper: statisk resursklasser och dynamisk resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="57889-711">Inläsning av bästa praxis</span><span class="sxs-lookup"><span data-stu-id="57889-711">Loading best practices</span></span>
1.  <span data-ttu-id="57889-712">Om förväntningarna belastningar med vanlig mängd data, är en statisk resursklassen ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="57889-712">If the expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="57889-713">Senare, när du ökar få mer dataresurser, kommer datalagret att kunna köra flera samtidiga frågor out-of-the-box, eftersom belastningen användaren inte förbruka mer minne.</span><span class="sxs-lookup"><span data-stu-id="57889-713">Later, when scaling up to get more computational power, the data warehouse will be able to run more concurrent queries out-of-the-box, as the load user does not consume more memory.</span></span>
2.  <span data-ttu-id="57889-714">Om förväntningarna större belastningar i vissa fall, är en dynamisk resursklassen bra.</span><span class="sxs-lookup"><span data-stu-id="57889-714">If the expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="57889-715">Senare, när du ökar få mer dataresurser, får Läs in användaren mer minne out-of-the-box, därför att tillåta att belastningen utföra snabbare.</span><span class="sxs-lookup"><span data-stu-id="57889-715">Later, when scaling up to get more computational power, the load user will get more memory out-of-the-box, hence allowing the load to perform faster.</span></span>

<span data-ttu-id="57889-716">Det minne som behövs för att bearbeta belastningar effektivt beror på tabellen som lästs in och hur mycket data som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="57889-716">The memory needed to process loads efficiently depends on the nature of the table loaded and the amount of data processed.</span></span> <span data-ttu-id="57889-717">Till exempel kräver läsa in data i CCI tabeller minne så att CCI rowgroups nå uppfylldes.</span><span class="sxs-lookup"><span data-stu-id="57889-717">For instance, loading data into CCI tables requires some memory to let CCI rowgroups reach optimality.</span></span> <span data-ttu-id="57889-718">Mer information finns i Columnstore-index - vägledning för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="57889-718">For more details, see the Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="57889-719">Som bästa praxis rekommenderar vi att du använder minst 200MB minne för belastning.</span><span class="sxs-lookup"><span data-stu-id="57889-719">As a best practice, we advise you to use at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="57889-720">Frågar bästa praxis</span><span class="sxs-lookup"><span data-stu-id="57889-720">Querying best practices</span></span>
<span data-ttu-id="57889-721">Frågor har olika krav beroende på deras komplexitet.</span><span class="sxs-lookup"><span data-stu-id="57889-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="57889-722">Öka minnesmängd per fråga eller öka parallellkörning är båda giltiga sätt att utöka totala genomflödet beroende fråga behov.</span><span class="sxs-lookup"><span data-stu-id="57889-722">Increasing memory per query or increasing the concurrency are both valid ways to augment overall throughput depending on the query needs.</span></span>
1.  <span data-ttu-id="57889-723">Om förväntningarna regelbundna, komplexa frågor (till exempel att generera rapporter för dagliga och veckovisa) och behöver inte dra nytta av samtidighet, dynamiska resursklassen är ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="57889-723">If the expectations are regular, complex queries (for instance, to generate daily and weekly reports) and do not need to take advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="57889-724">Om systemet har mer data att bearbeta, ger skala upp datalagret därför automatiskt mer minne till användaren som kör frågan.</span><span class="sxs-lookup"><span data-stu-id="57889-724">If the system has more data to process, scaling up the data warehouse will therefore automatically provide more memory to the user running the query.</span></span>
2.  <span data-ttu-id="57889-725">Om förväntningarna variabel eller profil över samtidighet mönster (till exempel om databasen efterfrågas via ett webbgränssnitt brett tillgänglig), statiska resursklassen är ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="57889-725">If the expectations are variable or diurnal concurrency patterns (for instance if the database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="57889-726">Senare, när skala upp till data warehouse kommer användaren som är associerad med den statiska resursklassen automatiskt att kunna köra flera samtidiga frågor.</span><span class="sxs-lookup"><span data-stu-id="57889-726">Later, when scaling up to data warehouse, the user associated with the static resource class will automatically be able to run more concurrent queries.</span></span>

<span data-ttu-id="57889-727">Genom att välja rätt minnestilldelningen beroende på behovet av din fråga är icke-trivial som beror på många faktorer, till exempel hur mycket data som efterfrågas, vilka slags tabellscheman, och olika koppling, val och grupp-predikat.</span><span class="sxs-lookup"><span data-stu-id="57889-727">Selecting proper memory grant depending on the need of your query is non-trivial, as it depends on many factors, such as the amount of data queried, the nature of the table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="57889-728">Från en allmän synvinkel allokera mer minne tillåter frågor för att slutföra snabbare, men skulle minskar den övergripande.</span><span class="sxs-lookup"><span data-stu-id="57889-728">From a general standpoint, allocating more memory will allow queries to complete faster, but would reduce the overall concurrency.</span></span> <span data-ttu-id="57889-729">Om samtidighet inte är ett problem, skadas över minnesallokering inte.</span><span class="sxs-lookup"><span data-stu-id="57889-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="57889-730">För att finjustera genomströmning kan försök olika varianter av resursklasser krävas.</span><span class="sxs-lookup"><span data-stu-id="57889-730">To fine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="57889-731">Du kan använda följande lagrade procedur för att ta reda på samtidighet och minne bevilja per resursklassen vid en given SLO och närmaste bästa resursklassen för minne beräkningsintensiva CCI åtgärder på CCI partitionerade tabellen vid en viss resurs-klass:</span><span class="sxs-lookup"><span data-stu-id="57889-731">You can use the following stored procedure to figure out concurrency and memory grant per resource class at a given SLO and the closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="57889-732">Beskrivning:</span><span class="sxs-lookup"><span data-stu-id="57889-732">Description:</span></span>  
<span data-ttu-id="57889-733">Här är syftet med den här lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="57889-733">Here's the purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="57889-734">För att hjälpa användare att ta reda på bevilja samtidighet och minne per resursklassen vid ett givet Servicenivåmål.</span><span class="sxs-lookup"><span data-stu-id="57889-734">To help user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="57889-735">Användaren måste ange NULL för både schema- och tablename för den här som visas i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="57889-735">User needs to provide NULL for both schema and tablename for this as shown in the example below.</span></span>  
2. <span data-ttu-id="57889-736">För att användaren ta reda på den närmaste bästa resursklassen för minne intensed CCI åtgärder (belastning, kopiera tabellen återskapa index, etc.) på icke partitionerad CCI tabell till en viss resurs-klass.</span><span class="sxs-lookup"><span data-stu-id="57889-736">To help user figure out the closest best resource class for the memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="57889-737">Den lagrade proceduren använder tabellschemat för att ta reda på minnestilldelningen krävs för den här.</span><span class="sxs-lookup"><span data-stu-id="57889-737">The stored proc uses table schema to find out the required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="57889-738">Beroenden och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="57889-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="57889-739">Den här lagrade proceduren är inte avsedd att beräkna minnesutrymmet för partitionerad cci tabellen.</span><span class="sxs-lookup"><span data-stu-id="57889-739">This stored proc is not designed to calculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="57889-740">Den här lagrade proceduren tar inte minneskravet hänsyn för den VALDA delen av CTAS/INSERT-Välj och förutsätter att det ska vara ett enkelt SELECT.</span><span class="sxs-lookup"><span data-stu-id="57889-740">This stored proc doesn't take memory requirement into account for the SELECT part of CTAS/INSERT-SELECT and assumes it to be a simple SELECT.</span></span>
- <span data-ttu-id="57889-741">Den här lagrade proceduren använder en temporär tabell så det kan användas i sessionen där den här lagrade proceduren har skapats.</span><span class="sxs-lookup"><span data-stu-id="57889-741">This stored proc uses a temp table so this can be used in the session where this stored proc was created.</span></span>    
- <span data-ttu-id="57889-742">Den här lagrade proceduren beror på de aktuella erbjudandena (t.ex. maskinvarukonfiguration, DMS-konfiguration) och om någon av som ändras sedan denna lagrade procedur fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="57889-742">This stored proc depends on the current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="57889-743">Den här lagrade proceduren beror på befintliga erbjudna samtidighet gränsen och om att ändringar sedan den här lagrade proceduren fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="57889-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="57889-744">Denna lagrade procedur beror på befintlig resurs klassen erbjudanden och om att ändringar sedan detta lagrade procedur wuold fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="57889-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="57889-745">Om du inte får utdata när du kör lagrad procedur med parametrar som ges det kan vara två fall.</span><span class="sxs-lookup"><span data-stu-id="57889-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="57889-746">1. Antingen DW-parametern innehåller ett ogiltigt värde för Servicenivåmål</span><span class="sxs-lookup"><span data-stu-id="57889-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="57889-747">2. ELLER så finns det ingen matchande resursklassen för CCI åtgärd om tabellnamn angavs.</span><span class="sxs-lookup"><span data-stu-id="57889-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="57889-748">Exempelvis är DW100, högsta minnestilldelningen 400MB och om tabellschemat litet mellan kravet 400 MB.</span><span class="sxs-lookup"><span data-stu-id="57889-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough to cross the requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="57889-749">Exempel på användning:</span><span class="sxs-lookup"><span data-stu-id="57889-749">Usage example:</span></span>
<span data-ttu-id="57889-750">Syntax:</span><span class="sxs-lookup"><span data-stu-id="57889-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="57889-751">@DWU:Antingen ange en NULL-parameter om du vill extrahera den aktuella DWU från databasen för Datalagret eller ange eventuella stöds DWU i form av 'DW100'</span><span class="sxs-lookup"><span data-stu-id="57889-751">@DWU: Either provide a NULL parameter to extract the current DWU from the DW DB or provide any supported DWU in the form of 'DW100'</span></span>
2. <span data-ttu-id="57889-752">@SCHEMA_NAME:Ange ett namn på tabellen</span><span class="sxs-lookup"><span data-stu-id="57889-752">@SCHEMA_NAME: Provide a schema name of the table</span></span>
3. <span data-ttu-id="57889-753">@TABLE_NAME:Ange ett tabellnamn av intresse</span><span class="sxs-lookup"><span data-stu-id="57889-753">@TABLE_NAME: Provide a table name of the interest</span></span>

<span data-ttu-id="57889-754">Exempel köra den här lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="57889-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="57889-755">Tabell 1 används i ovanstående exempel kunde skapas enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="57889-755">Table1 used in the above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a><span data-ttu-id="57889-756">Här är definitionen för lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="57889-756">Here's the stored procedure definition:</span></span>
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a><span data-ttu-id="57889-757">Vikten av frågan</span><span class="sxs-lookup"><span data-stu-id="57889-757">Query importance</span></span>
<span data-ttu-id="57889-758">SQL Data Warehouse implementerar resursklasser med hjälp av arbetsbelastningsgrupper.</span><span class="sxs-lookup"><span data-stu-id="57889-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="57889-759">Det finns totalt åtta arbetsbelastningsgrupper som styr beteendet för resursklasser över olika DWU-storleken.</span><span class="sxs-lookup"><span data-stu-id="57889-759">There are a total of eight workload groups that control the behavior of the resource classes across the various DWU sizes.</span></span> <span data-ttu-id="57889-760">För alla DWU använder SQL Data Warehouse bara fyra grupperna åtta arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="57889-760">For any DWU, SQL Data Warehouse uses only four of the eight workload groups.</span></span> <span data-ttu-id="57889-761">Detta är meningsfullt eftersom varje arbetsbelastningsgruppen har tilldelats någon av fyra resursklasser: smallrc mediumrc, largerc, eller xlargerc.</span><span class="sxs-lookup"><span data-stu-id="57889-761">This makes sense because each workload group is assigned to one of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="57889-762">Vikten av att förstå grupperna arbetsbelastning är att vissa av dessa arbetsbelastningsgrupper som har angetts till högre *vikten*.</span><span class="sxs-lookup"><span data-stu-id="57889-762">The importance of understanding the workload groups is that some of these workload groups are set to higher *importance*.</span></span> <span data-ttu-id="57889-763">Vikten används för CPU schemaläggning.</span><span class="sxs-lookup"><span data-stu-id="57889-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="57889-764">Frågor som körs med hög prioritet får tre gånger så mycket CPU-cykler än de med medelhög prioritet.</span><span class="sxs-lookup"><span data-stu-id="57889-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="57889-765">Därför avgör samtidighet fack mappningar också CPU-prioritet.</span><span class="sxs-lookup"><span data-stu-id="57889-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="57889-766">När en fråga förbrukar 16 eller flera platser, körs det som hög prioritet.</span><span class="sxs-lookup"><span data-stu-id="57889-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="57889-767">Följande tabell visar vikten mappningar för varje arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="57889-767">The following table shows the importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a><span data-ttu-id="57889-768">Arbetsbelastningen gruppmappningar till samtidighet platser och betydelse</span><span class="sxs-lookup"><span data-stu-id="57889-768">Workload group mappings to concurrency slots and importance</span></span>
| <span data-ttu-id="57889-769">Arbetsbelastningsgrupper</span><span class="sxs-lookup"><span data-stu-id="57889-769">Workload groups</span></span> | <span data-ttu-id="57889-770">Concurrency fack mappning</span><span class="sxs-lookup"><span data-stu-id="57889-770">Concurrency slot mapping</span></span> | <span data-ttu-id="57889-771">MB / Distribution</span><span class="sxs-lookup"><span data-stu-id="57889-771">MB / Distribution</span></span> | <span data-ttu-id="57889-772">Vikten mappning</span><span class="sxs-lookup"><span data-stu-id="57889-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="57889-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="57889-773">SloDWGroupC00</span></span> |<span data-ttu-id="57889-774">1</span><span class="sxs-lookup"><span data-stu-id="57889-774">1</span></span> |<span data-ttu-id="57889-775">100</span><span class="sxs-lookup"><span data-stu-id="57889-775">100</span></span> |<span data-ttu-id="57889-776">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-776">Medium</span></span> |
| <span data-ttu-id="57889-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="57889-777">SloDWGroupC01</span></span> |<span data-ttu-id="57889-778">2</span><span class="sxs-lookup"><span data-stu-id="57889-778">2</span></span> |<span data-ttu-id="57889-779">200</span><span class="sxs-lookup"><span data-stu-id="57889-779">200</span></span> |<span data-ttu-id="57889-780">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-780">Medium</span></span> |
| <span data-ttu-id="57889-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="57889-781">SloDWGroupC02</span></span> |<span data-ttu-id="57889-782">4</span><span class="sxs-lookup"><span data-stu-id="57889-782">4</span></span> |<span data-ttu-id="57889-783">400</span><span class="sxs-lookup"><span data-stu-id="57889-783">400</span></span> |<span data-ttu-id="57889-784">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-784">Medium</span></span> |
| <span data-ttu-id="57889-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-785">SloDWGroupC03</span></span> |<span data-ttu-id="57889-786">8</span><span class="sxs-lookup"><span data-stu-id="57889-786">8</span></span> |<span data-ttu-id="57889-787">800</span><span class="sxs-lookup"><span data-stu-id="57889-787">800</span></span> |<span data-ttu-id="57889-788">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-788">Medium</span></span> |
| <span data-ttu-id="57889-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="57889-789">SloDWGroupC04</span></span> |<span data-ttu-id="57889-790">16</span><span class="sxs-lookup"><span data-stu-id="57889-790">16</span></span> |<span data-ttu-id="57889-791">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-791">1,600</span></span> |<span data-ttu-id="57889-792">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-792">High</span></span> |
| <span data-ttu-id="57889-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="57889-793">SloDWGroupC05</span></span> |<span data-ttu-id="57889-794">32</span><span class="sxs-lookup"><span data-stu-id="57889-794">32</span></span> |<span data-ttu-id="57889-795">3,200</span><span class="sxs-lookup"><span data-stu-id="57889-795">3,200</span></span> |<span data-ttu-id="57889-796">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-796">High</span></span> |
| <span data-ttu-id="57889-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="57889-797">SloDWGroupC06</span></span> |<span data-ttu-id="57889-798">64</span><span class="sxs-lookup"><span data-stu-id="57889-798">64</span></span> |<span data-ttu-id="57889-799">6,400</span><span class="sxs-lookup"><span data-stu-id="57889-799">6,400</span></span> |<span data-ttu-id="57889-800">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-800">High</span></span> |
| <span data-ttu-id="57889-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="57889-801">SloDWGroupC07</span></span> |<span data-ttu-id="57889-802">128</span><span class="sxs-lookup"><span data-stu-id="57889-802">128</span></span> |<span data-ttu-id="57889-803">12,800</span><span class="sxs-lookup"><span data-stu-id="57889-803">12,800</span></span> |<span data-ttu-id="57889-804">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-804">High</span></span> |

<span data-ttu-id="57889-805">Från den **fördelningen och förbrukningen av samtidighet fack** diagram, ser du att en DW500 1, 4, 8 och 16 samtidighet fack för smallrc, mediumrc, largerc och xlargerc, respektive.</span><span class="sxs-lookup"><span data-stu-id="57889-805">From the **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="57889-806">Du kan slå dessa värden upp i det föregående att hitta vikten för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="57889-806">You can look those values up in the preceding chart to find the importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-to-importance"></a><span data-ttu-id="57889-807">DW500 mappning av resursklasser betydelse</span><span class="sxs-lookup"><span data-stu-id="57889-807">DW500 mapping of resource classes to importance</span></span>
| <span data-ttu-id="57889-808">Resursklass</span><span class="sxs-lookup"><span data-stu-id="57889-808">Resource class</span></span> | <span data-ttu-id="57889-809">Arbetsbelastningsgruppen</span><span class="sxs-lookup"><span data-stu-id="57889-809">Workload group</span></span> | <span data-ttu-id="57889-810">Concurrency-platser som används</span><span class="sxs-lookup"><span data-stu-id="57889-810">Concurrency slots used</span></span> | <span data-ttu-id="57889-811">MB / Distribution</span><span class="sxs-lookup"><span data-stu-id="57889-811">MB / Distribution</span></span> | <span data-ttu-id="57889-812">Betydelse</span><span class="sxs-lookup"><span data-stu-id="57889-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="57889-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="57889-813">smallrc</span></span> |<span data-ttu-id="57889-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="57889-814">SloDWGroupC00</span></span> |<span data-ttu-id="57889-815">1</span><span class="sxs-lookup"><span data-stu-id="57889-815">1</span></span> |<span data-ttu-id="57889-816">100</span><span class="sxs-lookup"><span data-stu-id="57889-816">100</span></span> |<span data-ttu-id="57889-817">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-817">Medium</span></span> |
| <span data-ttu-id="57889-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="57889-818">mediumrc</span></span> |<span data-ttu-id="57889-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="57889-819">SloDWGroupC02</span></span> |<span data-ttu-id="57889-820">4</span><span class="sxs-lookup"><span data-stu-id="57889-820">4</span></span> |<span data-ttu-id="57889-821">400</span><span class="sxs-lookup"><span data-stu-id="57889-821">400</span></span> |<span data-ttu-id="57889-822">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-822">Medium</span></span> |
| <span data-ttu-id="57889-823">largerc</span><span class="sxs-lookup"><span data-stu-id="57889-823">largerc</span></span> |<span data-ttu-id="57889-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-824">SloDWGroupC03</span></span> |<span data-ttu-id="57889-825">8</span><span class="sxs-lookup"><span data-stu-id="57889-825">8</span></span> |<span data-ttu-id="57889-826">800</span><span class="sxs-lookup"><span data-stu-id="57889-826">800</span></span> |<span data-ttu-id="57889-827">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-827">Medium</span></span> |
| <span data-ttu-id="57889-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="57889-828">xlargerc</span></span> |<span data-ttu-id="57889-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="57889-829">SloDWGroupC04</span></span> |<span data-ttu-id="57889-830">16</span><span class="sxs-lookup"><span data-stu-id="57889-830">16</span></span> |<span data-ttu-id="57889-831">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-831">1,600</span></span> |<span data-ttu-id="57889-832">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-832">High</span></span> |
| <span data-ttu-id="57889-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="57889-833">staticrc10</span></span> |<span data-ttu-id="57889-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="57889-834">SloDWGroupC00</span></span> |<span data-ttu-id="57889-835">1</span><span class="sxs-lookup"><span data-stu-id="57889-835">1</span></span> |<span data-ttu-id="57889-836">100</span><span class="sxs-lookup"><span data-stu-id="57889-836">100</span></span> |<span data-ttu-id="57889-837">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-837">Medium</span></span> |
| <span data-ttu-id="57889-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="57889-838">staticrc20</span></span> |<span data-ttu-id="57889-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="57889-839">SloDWGroupC01</span></span> |<span data-ttu-id="57889-840">2</span><span class="sxs-lookup"><span data-stu-id="57889-840">2</span></span> |<span data-ttu-id="57889-841">200</span><span class="sxs-lookup"><span data-stu-id="57889-841">200</span></span> |<span data-ttu-id="57889-842">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-842">Medium</span></span> |
| <span data-ttu-id="57889-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="57889-843">staticrc30</span></span> |<span data-ttu-id="57889-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="57889-844">SloDWGroupC02</span></span> |<span data-ttu-id="57889-845">4</span><span class="sxs-lookup"><span data-stu-id="57889-845">4</span></span> |<span data-ttu-id="57889-846">400</span><span class="sxs-lookup"><span data-stu-id="57889-846">400</span></span> |<span data-ttu-id="57889-847">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-847">Medium</span></span> |
| <span data-ttu-id="57889-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="57889-848">staticrc40</span></span> |<span data-ttu-id="57889-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-849">SloDWGroupC03</span></span> |<span data-ttu-id="57889-850">8</span><span class="sxs-lookup"><span data-stu-id="57889-850">8</span></span> |<span data-ttu-id="57889-851">800</span><span class="sxs-lookup"><span data-stu-id="57889-851">800</span></span> |<span data-ttu-id="57889-852">Medel</span><span class="sxs-lookup"><span data-stu-id="57889-852">Medium</span></span> |
| <span data-ttu-id="57889-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="57889-853">staticrc50</span></span> |<span data-ttu-id="57889-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-854">SloDWGroupC03</span></span> |<span data-ttu-id="57889-855">16</span><span class="sxs-lookup"><span data-stu-id="57889-855">16</span></span> |<span data-ttu-id="57889-856">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-856">1,600</span></span> |<span data-ttu-id="57889-857">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-857">High</span></span> |
| <span data-ttu-id="57889-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="57889-858">staticrc60</span></span> |<span data-ttu-id="57889-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-859">SloDWGroupC03</span></span> |<span data-ttu-id="57889-860">16</span><span class="sxs-lookup"><span data-stu-id="57889-860">16</span></span> |<span data-ttu-id="57889-861">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-861">1,600</span></span> |<span data-ttu-id="57889-862">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-862">High</span></span> |
| <span data-ttu-id="57889-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="57889-863">staticrc70</span></span> |<span data-ttu-id="57889-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-864">SloDWGroupC03</span></span> |<span data-ttu-id="57889-865">16</span><span class="sxs-lookup"><span data-stu-id="57889-865">16</span></span> |<span data-ttu-id="57889-866">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-866">1,600</span></span> |<span data-ttu-id="57889-867">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-867">High</span></span> |
| <span data-ttu-id="57889-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="57889-868">staticrc80</span></span> |<span data-ttu-id="57889-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="57889-869">SloDWGroupC03</span></span> |<span data-ttu-id="57889-870">16</span><span class="sxs-lookup"><span data-stu-id="57889-870">16</span></span> |<span data-ttu-id="57889-871">1,600</span><span class="sxs-lookup"><span data-stu-id="57889-871">1,600</span></span> |<span data-ttu-id="57889-872">Hög</span><span class="sxs-lookup"><span data-stu-id="57889-872">High</span></span> |

<span data-ttu-id="57889-873">Du kan använda följande DMV-frågan granskar skillnaderna i minnet resursallokeringen i detalj ur resursstyrningen eller för att analysera active och historisk användning av arbetsbelastningsgrupper när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="57889-873">You can use the following DMV query to look at the differences in memory resource allocation in detail from the perspective of the resource governor, or to analyze active and historic usage of the workload groups when troubleshooting.</span></span>

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="57889-874">Frågor som följer gränser för samtidig användning</span><span class="sxs-lookup"><span data-stu-id="57889-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="57889-875">De flesta frågor regleras av resursklasser.</span><span class="sxs-lookup"><span data-stu-id="57889-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="57889-876">De här frågorna måste få plats inuti båda samtidiga frågan och samtidighet fack tröskelvärdena.</span><span class="sxs-lookup"><span data-stu-id="57889-876">These queries must fit inside both the concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="57889-877">En användare kan inte välja att undanta en fråga från concurrency fack-modell.</span><span class="sxs-lookup"><span data-stu-id="57889-877">A user cannot choose to exclude a query from the concurrency slot model.</span></span>

<span data-ttu-id="57889-878">Om du vill upprepar, respektera följande påståenden resursklasser:</span><span class="sxs-lookup"><span data-stu-id="57889-878">To reiterate, the following statements honor resource classes:</span></span>

* <span data-ttu-id="57889-879">INFOGA VÄLJER</span><span class="sxs-lookup"><span data-stu-id="57889-879">INSERT-SELECT</span></span>
* <span data-ttu-id="57889-880">UPPDATERING</span><span class="sxs-lookup"><span data-stu-id="57889-880">UPDATE</span></span>
* <span data-ttu-id="57889-881">TA BORT</span><span class="sxs-lookup"><span data-stu-id="57889-881">DELETE</span></span>
* <span data-ttu-id="57889-882">Välj (när du frågar användartabeller)</span><span class="sxs-lookup"><span data-stu-id="57889-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="57889-883">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="57889-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="57889-884">ALTER INDEX REORGANIZE</span><span class="sxs-lookup"><span data-stu-id="57889-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="57889-885">ALTER TABLE REBUILD</span><span class="sxs-lookup"><span data-stu-id="57889-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="57889-886">SKAPA INDEX</span><span class="sxs-lookup"><span data-stu-id="57889-886">CREATE INDEX</span></span>
* <span data-ttu-id="57889-887">SKAPA DET GRUPPERADE COLUMNSTORE-INDEX</span><span class="sxs-lookup"><span data-stu-id="57889-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="57889-888">SKAPA TABLE AS SELECT (CTAS)</span><span class="sxs-lookup"><span data-stu-id="57889-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="57889-889">Läsa in data</span><span class="sxs-lookup"><span data-stu-id="57889-889">Data loading</span></span>
* <span data-ttu-id="57889-890">Dataflyttsåtgärderna utförs av Data Movement Service (DMS)</span><span class="sxs-lookup"><span data-stu-id="57889-890">Data movement operations conducted by the Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-to-concurrency-limits"></a><span data-ttu-id="57889-891">Frågan undantag samtidighet gränserna</span><span class="sxs-lookup"><span data-stu-id="57889-891">Query exceptions to concurrency limits</span></span>
<span data-ttu-id="57889-892">Några frågor följdes inte resursklassen som användaren har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="57889-892">Some queries do not honor the resource class to which the user is assigned.</span></span> <span data-ttu-id="57889-893">Sådana undantag samtidighet gränserna görs när minnesresurserna som behövs för ett visst kommando är låg, ofta eftersom kommandot är en metadata-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="57889-893">These exceptions to the concurrency limits are made when the memory resources needed for a particular command are low, often because the command is a metadata operation.</span></span> <span data-ttu-id="57889-894">Syftet med sådana undantag är att undvika större minnesallokering för frågor som behöver dem aldrig.</span><span class="sxs-lookup"><span data-stu-id="57889-894">The goal of these exceptions is to avoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="57889-895">I dessa fall används standard små resursklassen (smallrc) alltid oavsett faktiska resursklassen för användaren.</span><span class="sxs-lookup"><span data-stu-id="57889-895">In these cases, the default small resource class (smallrc) is always used regardless of the actual resource class assigned to the user.</span></span> <span data-ttu-id="57889-896">Till exempel `CREATE LOGIN` körs alltid i smallrc.</span><span class="sxs-lookup"><span data-stu-id="57889-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="57889-897">De resurser som krävs för att utföra den här åtgärden är mycket låg, så det inte vara klokt att inkludera frågan i samtidighet fack modellen.</span><span class="sxs-lookup"><span data-stu-id="57889-897">The resources required to fulfill this operation are very low, so it does not make sense to include the query in the concurrency slot model.</span></span>  <span data-ttu-id="57889-898">De här frågorna också begränsas inte av antalet samtidiga 32 användare, ett obegränsat antal dessa frågor kan köra till högst 1 024 sessioner session.</span><span class="sxs-lookup"><span data-stu-id="57889-898">These queries are also not limited by the 32 user concurrency limit, an unlimited number of these queries can run up to the session limit of 1,024 sessions.</span></span>

<span data-ttu-id="57889-899">Följande uttryck följdes inte resursklasser:</span><span class="sxs-lookup"><span data-stu-id="57889-899">The following statements do not honor resource classes:</span></span>

* <span data-ttu-id="57889-900">Skapa eller ta bort tabell</span><span class="sxs-lookup"><span data-stu-id="57889-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="57889-901">ALTER TABLE... VÄXELN, dela och slå samman partitionen</span><span class="sxs-lookup"><span data-stu-id="57889-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="57889-902">ALTER INDEX INAKTIVERA</span><span class="sxs-lookup"><span data-stu-id="57889-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="57889-903">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="57889-903">DROP INDEX</span></span>
* <span data-ttu-id="57889-904">Skapa, uppdatera och ta bort statistik</span><span class="sxs-lookup"><span data-stu-id="57889-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="57889-905">TRUNKERA TABELLEN</span><span class="sxs-lookup"><span data-stu-id="57889-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="57889-906">ÄNDRA TILLSTÅND</span><span class="sxs-lookup"><span data-stu-id="57889-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="57889-907">SKAPA INLOGGNING</span><span class="sxs-lookup"><span data-stu-id="57889-907">CREATE LOGIN</span></span>
* <span data-ttu-id="57889-908">Skapa, ändra eller släppa användaren</span><span class="sxs-lookup"><span data-stu-id="57889-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="57889-909">Skapa, ändra eller släppa proceduren</span><span class="sxs-lookup"><span data-stu-id="57889-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="57889-910">Skapa eller ta bort vy</span><span class="sxs-lookup"><span data-stu-id="57889-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="57889-911">LÄGGA TILL VÄRDEN</span><span class="sxs-lookup"><span data-stu-id="57889-911">INSERT VALUES</span></span>
* <span data-ttu-id="57889-912">Välj systemvyer och av DMV: er</span><span class="sxs-lookup"><span data-stu-id="57889-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="57889-913">FÖRKLARAR</span><span class="sxs-lookup"><span data-stu-id="57889-913">EXPLAIN</span></span>
* <span data-ttu-id="57889-914">DBCC</span><span class="sxs-lookup"><span data-stu-id="57889-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="57889-915"><a name="changing-user-resource-class-example"></a>Ändra ett exempel på användaren resurs klass</span><span class="sxs-lookup"><span data-stu-id="57889-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="57889-916">**Skapa inloggning:** öppna en anslutning till din **master** databasen på SQLServer som värd för SQL Data Warehouse-databas och kör följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="57889-916">**Create login:** Open a connection to your **master** database on the SQL server hosting your SQL Data Warehouse database and execute the following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="57889-917">Det är en bra idé att skapa en användare i master-databasen för Azure SQL Data Warehouse-användare.</span><span class="sxs-lookup"><span data-stu-id="57889-917">It is a good idea to create a user in the master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="57889-918">Skapa en användare i master tillåter en användare att logga in med verktyg som SSMS utan att ange ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="57889-918">Creating a user in master allows a user to login using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="57889-919">Det gör också att de använder object explorer för att visa alla databaser på en SQLServer.</span><span class="sxs-lookup"><span data-stu-id="57889-919">It also allows them to use the object explorer to view all databases on a SQL server.</span></span>  <span data-ttu-id="57889-920">Mer information om att skapa och hantera användare finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="57889-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="57889-921">**Skapa SQL Data Warehouse användare:** öppna en anslutning till den **SQL Data Warehouse** databasen och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="57889-921">**Create SQL Data Warehouse user:** Open a connection to the **SQL Data Warehouse** database and execute the following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="57889-922">**Bevilja behörighet:** i följande exempel beviljas `CONTROL` på den **SQL Data Warehouse** databas.</span><span class="sxs-lookup"><span data-stu-id="57889-922">**Grant permissions:** The following example grants `CONTROL` on the **SQL Data Warehouse** database.</span></span> <span data-ttu-id="57889-923">`CONTROL`databasen är motsvarigheten till db_owner i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57889-923">`CONTROL` at the database level is the equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. <span data-ttu-id="57889-924">**Öka resursklass:** använder du följande fråga för att lägga till en användare till en högre roll för arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="57889-924">**Increase resource class:** Use the following query to add a user to a higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="57889-925">**Minska resursklass:** använder du följande fråga för att ta bort en användare från en roll för arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="57889-925">**Decrease resource class:** Use the following query to remove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="57889-926">Det går inte att ta bort en användare från smallrc.</span><span class="sxs-lookup"><span data-stu-id="57889-926">It is not possible to remove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="57889-927">Köade frågan identifiering och andra av DMV: er</span><span class="sxs-lookup"><span data-stu-id="57889-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="57889-928">Du kan använda den `sys.dm_pdw_exec_requests` DMV att identifiera frågor som väntar i en kö samtidighet.</span><span class="sxs-lookup"><span data-stu-id="57889-928">You can use the `sys.dm_pdw_exec_requests` DMV to identify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="57889-929">Frågar väntar på en plats för samtidighet har statusen **avbruten**.</span><span class="sxs-lookup"><span data-stu-id="57889-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="57889-930">Roller för hantering av arbetsbelastning kan visas med `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="57889-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="57889-931">Följande fråga visar vilken roll som varje användare har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="57889-931">The following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="57889-932">SQL Data Warehouse har följande vänta typer:</span><span class="sxs-lookup"><span data-stu-id="57889-932">SQL Data Warehouse has the following wait types:</span></span>

* <span data-ttu-id="57889-933">**LocalQueriesConcurrencyResourceType**: frågor som är placerade utanför samtidighet fack ramen.</span><span class="sxs-lookup"><span data-stu-id="57889-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of the concurrency slot framework.</span></span> <span data-ttu-id="57889-934">DMV frågor och system fungerar som `SELECT @@VERSION` är exempel på lokala frågor.</span><span class="sxs-lookup"><span data-stu-id="57889-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="57889-935">**UserConcurrencyResourceType**: frågor som visas inuti samtidighet fack ramen.</span><span class="sxs-lookup"><span data-stu-id="57889-935">**UserConcurrencyResourceType**: Queries that sit inside the concurrency slot framework.</span></span> <span data-ttu-id="57889-936">Frågor mot tabeller slutanvändarens representerar exempel som använder den här resurstypen.</span><span class="sxs-lookup"><span data-stu-id="57889-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="57889-937">**DmsConcurrencyResourceType**: Väntar till följd av dataflyttsåtgärderna.</span><span class="sxs-lookup"><span data-stu-id="57889-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="57889-938">**BackupConcurrencyResourceType**: den här vänta anger att en databas säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="57889-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="57889-939">Det maximala värdet för den här resurstypen är 1.</span><span class="sxs-lookup"><span data-stu-id="57889-939">The maximum value for this resource type is 1.</span></span> <span data-ttu-id="57889-940">Om flera säkerhetskopieringar har begärts samtidigt, kommer de andra kö.</span><span class="sxs-lookup"><span data-stu-id="57889-940">If multiple backups have been requested at the same time, the others will queue.</span></span>

<span data-ttu-id="57889-941">Den `sys.dm_pdw_waits` DMV kan användas för att se vilka resurser som väntar på en begäran.</span><span class="sxs-lookup"><span data-stu-id="57889-941">The `sys.dm_pdw_waits` DMV can be used to see which resources a request is waiting for.</span></span>

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

<span data-ttu-id="57889-942">Den `sys.dm_pdw_resource_waits` DMV visar endast de resursen väntar som används av en given fråga.</span><span class="sxs-lookup"><span data-stu-id="57889-942">The `sys.dm_pdw_resource_waits` DMV shows only the resource waits consumed by a given query.</span></span> <span data-ttu-id="57889-943">Väntetiden för resursen endast mäter den tid som väntar på resurser som ska tillhandahållas, till skillnad från signal väntetiden, vilket är den tid det tar för de underliggande SQL-servrarna att schemalägga frågan till Processorn.</span><span class="sxs-lookup"><span data-stu-id="57889-943">Resource wait time only measures the time waiting for resources to be provided, as opposed to signal wait time, which is the time it takes for the underlying SQL servers to schedule the query onto the CPU.</span></span>

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

<span data-ttu-id="57889-944">Den `sys.dm_pdw_wait_stats` DMV kan användas för analys av historiska trender för väntar.</span><span class="sxs-lookup"><span data-stu-id="57889-944">The `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a><span data-ttu-id="57889-945">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57889-945">Next steps</span></span>
<span data-ttu-id="57889-946">Mer information om hur du hanterar databasanvändare och säkerhet finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="57889-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="57889-947">Mer information om hur större resursklasser kan förbättra kvaliteten för grupperade columnstore-index, finns [bygga om index för att förbättra kvaliteten segment].</span><span class="sxs-lookup"><span data-stu-id="57889-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes to improve segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[bygga om index för att förbättra kvaliteten segment]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
