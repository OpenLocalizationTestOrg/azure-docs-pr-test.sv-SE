---
title: hantering av aaaConcurrency och arbetsbelastning i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 7f7e77aa687760252aed16573b609817ed9111c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="73e0e-103">Hantering av samtidighet och arbetsbelastning i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="73e0e-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="73e0e-104">toodeliver förutsägbar prestanda i skala, Microsoft Azure SQL Data Warehouse hjälper dig att styra samtidighet nivåer och resursfördelningar som minne och CPU-prioritering.</span><span class="sxs-lookup"><span data-stu-id="73e0e-104">toodeliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="73e0e-105">Den här artikeln beskriver toohello begrepp av samtidighet och arbetsbelastningen management, förklarar hur båda funktionerna har genomförts och hur du kan kontrollera dem i ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="73e0e-105">This article introduces you toohello concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="73e0e-106">Hantering av SQL Data Warehouse arbetsbelastning är avsedda toohelp du stöd för miljöer med flera användare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-106">SQL Data Warehouse workload management is intended toohelp you support multi-user environments.</span></span> <span data-ttu-id="73e0e-107">Det är inte avsedd för flera innehavare arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="73e0e-108">Concurrency-gränser</span><span class="sxs-lookup"><span data-stu-id="73e0e-108">Concurrency limits</span></span>
<span data-ttu-id="73e0e-109">Det låter SQL Data Warehouse dig too1 024 samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-109">SQL Data Warehouse allows up too1,024 concurrent connections.</span></span> <span data-ttu-id="73e0e-110">Alla 1 024 anslutningar kan skicka frågor samtidigt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="73e0e-111">Toooptimize dataflöde, SQL Data Warehouse kan dock kö vissa frågor tooensure att varje fråga får en minimal minnestilldelningen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-111">However, toooptimize throughput, SQL Data Warehouse may queue some queries tooensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="73e0e-112">Queuing inträffar vid körning i frågan.</span><span class="sxs-lookup"><span data-stu-id="73e0e-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="73e0e-113">MSMQ-frågor när gränsen har uppnåtts samtidighet SQL Data Warehouse kan öka behövs totala genomflödet genom att säkerställa att aktiva frågor får åtkomst toocritically minnesresurser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access toocritically needed memory resources.</span></span>  

<span data-ttu-id="73e0e-114">Concurrency gränser styrs av två begrepp: *samtidiga frågor* och *samtidighet fack*.</span><span class="sxs-lookup"><span data-stu-id="73e0e-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="73e0e-115">För en fråga tooexecute måste den köras i både hello frågan samtidighet gränsen och hello samtidighet fördelning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-115">For a query tooexecute, it must execute within both hello query concurrency limit and hello concurrency slot allocation.</span></span>

* <span data-ttu-id="73e0e-116">Samtidiga frågor är hello frågor som körs med hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="73e0e-116">Concurrent queries are hello queries executing at hello same time.</span></span> <span data-ttu-id="73e0e-117">SQL Data Warehouse stöder upp too32 samtidiga frågor på hello större DWU storlekar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-117">SQL Data Warehouse supports up too32 concurrent queries on hello larger DWU sizes.</span></span>
* <span data-ttu-id="73e0e-118">Concurrency platser tilldelas baserat på DWU.</span><span class="sxs-lookup"><span data-stu-id="73e0e-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="73e0e-119">Varje 100 DWU innehåller 4 samtidighet platser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="73e0e-120">Till exempel en DW100 allokerar 4 samtidighet platser och DW1000 allokerar 40.</span><span class="sxs-lookup"><span data-stu-id="73e0e-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="73e0e-121">Varje fråga förbrukar en eller flera samtidiga platser, beroende på hello [resursklassen](#resource-classes) av hello fråga.</span><span class="sxs-lookup"><span data-stu-id="73e0e-121">Each query consumes one or more concurrency slots, dependent on hello [resource class](#resource-classes) of hello query.</span></span> <span data-ttu-id="73e0e-122">Frågor som körs i hello smallrc resursklassen använda en concurrency-plats.</span><span class="sxs-lookup"><span data-stu-id="73e0e-122">Queries running in hello smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="73e0e-123">Frågor som körs i en högre resursklassen förbruka mer samtidighet platser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="73e0e-124">hello följande tabell beskrivs hello gränser för både samtidiga frågor och samtidighet fack på hello olika DWU-storlekar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-124">hello following table describes hello limits for both concurrent queries and concurrency slots at hello various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="73e0e-125">Concurrency-gränser</span><span class="sxs-lookup"><span data-stu-id="73e0e-125">Concurrency limits</span></span>
| <span data-ttu-id="73e0e-126">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-126">DWU</span></span> | <span data-ttu-id="73e0e-127">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="73e0e-127">Max concurrent queries</span></span> | <span data-ttu-id="73e0e-128">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="73e0e-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="73e0e-129">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-129">DW100</span></span> |<span data-ttu-id="73e0e-130">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-130">4</span></span> |<span data-ttu-id="73e0e-131">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-131">4</span></span> |
| <span data-ttu-id="73e0e-132">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-132">DW200</span></span> |<span data-ttu-id="73e0e-133">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-133">8</span></span> |<span data-ttu-id="73e0e-134">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-134">8</span></span> |
| <span data-ttu-id="73e0e-135">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-135">DW300</span></span> |<span data-ttu-id="73e0e-136">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-136">12</span></span> |<span data-ttu-id="73e0e-137">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-137">12</span></span> |
| <span data-ttu-id="73e0e-138">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-138">DW400</span></span> |<span data-ttu-id="73e0e-139">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-139">16</span></span> |<span data-ttu-id="73e0e-140">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-140">16</span></span> |
| <span data-ttu-id="73e0e-141">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-141">DW500</span></span> |<span data-ttu-id="73e0e-142">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-142">20</span></span> |<span data-ttu-id="73e0e-143">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-143">20</span></span> |
| <span data-ttu-id="73e0e-144">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-144">DW600</span></span> |<span data-ttu-id="73e0e-145">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-145">24</span></span> |<span data-ttu-id="73e0e-146">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-146">24</span></span> |
| <span data-ttu-id="73e0e-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-147">DW1000</span></span> |<span data-ttu-id="73e0e-148">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-148">32</span></span> |<span data-ttu-id="73e0e-149">40</span><span class="sxs-lookup"><span data-stu-id="73e0e-149">40</span></span> |
| <span data-ttu-id="73e0e-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-150">DW1200</span></span> |<span data-ttu-id="73e0e-151">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-151">32</span></span> |<span data-ttu-id="73e0e-152">48</span><span class="sxs-lookup"><span data-stu-id="73e0e-152">48</span></span> |
| <span data-ttu-id="73e0e-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-153">DW1500</span></span> |<span data-ttu-id="73e0e-154">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-154">32</span></span> |<span data-ttu-id="73e0e-155">60</span><span class="sxs-lookup"><span data-stu-id="73e0e-155">60</span></span> |
| <span data-ttu-id="73e0e-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-156">DW2000</span></span> |<span data-ttu-id="73e0e-157">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-157">32</span></span> |<span data-ttu-id="73e0e-158">80</span><span class="sxs-lookup"><span data-stu-id="73e0e-158">80</span></span> |
| <span data-ttu-id="73e0e-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-159">DW3000</span></span> |<span data-ttu-id="73e0e-160">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-160">32</span></span> |<span data-ttu-id="73e0e-161">120</span><span class="sxs-lookup"><span data-stu-id="73e0e-161">120</span></span> |
| <span data-ttu-id="73e0e-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-162">DW6000</span></span> |<span data-ttu-id="73e0e-163">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-163">32</span></span> |<span data-ttu-id="73e0e-164">240</span><span class="sxs-lookup"><span data-stu-id="73e0e-164">240</span></span> |

<span data-ttu-id="73e0e-165">När något av dessa tröskelvärden uppfylls är nya frågor i kö och utförs på grundval först in, först ut.</span><span class="sxs-lookup"><span data-stu-id="73e0e-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="73e0e-166">Som en frågor är klar och hello antalet frågor och fack understiger hello gränser, släpps köade frågor.</span><span class="sxs-lookup"><span data-stu-id="73e0e-166">As a queries finishes and hello number of queries and slots falls below hello limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="73e0e-167">*Välj* frågor körs enbart på hantering av dynamiska vyer (av DMV: er) eller katalogvyer inte regleras av någon av hello samtidighet gränser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of hello concurrency limits.</span></span> <span data-ttu-id="73e0e-168">Du kan övervaka hello system oavsett hello antalet frågor som körs på den.</span><span class="sxs-lookup"><span data-stu-id="73e0e-168">You can monitor hello system regardless of hello number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="73e0e-169">Resursklasser</span><span class="sxs-lookup"><span data-stu-id="73e0e-169">Resource classes</span></span>
<span data-ttu-id="73e0e-170">Resursen klasserna hjälper dig styra minnesallokering och CPU-cykler som anges tooa frågan.</span><span class="sxs-lookup"><span data-stu-id="73e0e-170">Resource classes help you control memory allocation and CPU cycles given tooa query.</span></span> <span data-ttu-id="73e0e-171">Du kan tilldela två typer av resursen klasser tooa användaren i hello form av databasroller.</span><span class="sxs-lookup"><span data-stu-id="73e0e-171">You can assign two types of resource classes tooa user in hello form of database roles.</span></span> <span data-ttu-id="73e0e-172">hello två typer av resursklasser är följande:</span><span class="sxs-lookup"><span data-stu-id="73e0e-172">hello two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="73e0e-173">Dynamiska resurs-klasser (**smallrc mediumrc, largerc xlargerc**) allokera en variabel mängd minne beroende på hello aktuella DWU.</span><span class="sxs-lookup"><span data-stu-id="73e0e-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on hello current DWU.</span></span> <span data-ttu-id="73e0e-174">Detta innebär att när du skalar upp tooa större DWU dina frågor automatiskt få mer minne.</span><span class="sxs-lookup"><span data-stu-id="73e0e-174">This means that when you scale up tooa larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="73e0e-175">Statiska resurs-klasser (**staticrc10 staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allokera hello samma mängd minne oavsett hello aktuella DWU (förutsatt att hello DWU sig själv har tillräckligt med minne).</span><span class="sxs-lookup"><span data-stu-id="73e0e-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate hello same amount of memory regardless of hello current DWU (provided that hello DWU itself has enough memory).</span></span> <span data-ttu-id="73e0e-176">Det innebär att större dwu: er, du kan köra flera frågor i varje resursklassen samtidigt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="73e0e-177">Användare i **smallrc** och **staticrc10** ges en mindre mängd minne och kan dra nytta av högre samtidighet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="73e0e-178">Däremot användare som har tilldelats för**xlargerc** eller **staticrc80** ges stora mängder minne, och därför färre deras frågor kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-178">In contrast, users assigned too**xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="73e0e-179">Som standard varje användare är medlem i hello små resursklassen **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="73e0e-179">By default, each user is a member of hello small resource class, **smallrc**.</span></span> <span data-ttu-id="73e0e-180">Hej proceduren `sp_addrolemember` är används tooincrease hello resursklassen, och `sp_droprolemember` är används toodecrease hello resursklassen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-180">hello procedure `sp_addrolemember` is used tooincrease hello resource class, and `sp_droprolemember` is used toodecrease hello resource class.</span></span> <span data-ttu-id="73e0e-181">Till exempel kommandot skulle öka hello resursklassen för loaduser för**largerc**:</span><span class="sxs-lookup"><span data-stu-id="73e0e-181">For example, this command would increase hello resource class of loaduser too**largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="73e0e-182">Frågor som inte följer resursklasser</span><span class="sxs-lookup"><span data-stu-id="73e0e-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="73e0e-183">Det finns några typer av frågor som inte omfattas av en större minnesallokering.</span><span class="sxs-lookup"><span data-stu-id="73e0e-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="73e0e-184">hello system ignorerar sina allokering av klassen och alltid köra dessa frågor i hello små resursklassen i stället.</span><span class="sxs-lookup"><span data-stu-id="73e0e-184">hello system ignores their resource class allocation and always run these queries in hello small resource class instead.</span></span> <span data-ttu-id="73e0e-185">Om de här frågorna körs alltid i hello små resursklassen, kan de köras när samtidighet platser är under hög belastning och de kommer inte använda fler platser än vad som behövs.</span><span class="sxs-lookup"><span data-stu-id="73e0e-185">If these queries always run in hello small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="73e0e-186">Se [resurs klassen undantag](#query-exceptions-to-concurrency-limits) för mer information.</span><span class="sxs-lookup"><span data-stu-id="73e0e-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="73e0e-187">Information om resurs klassen tilldelning</span><span class="sxs-lookup"><span data-stu-id="73e0e-187">Details on resource class assignment</span></span>


<span data-ttu-id="73e0e-188">Några få mer information om resursklass:</span><span class="sxs-lookup"><span data-stu-id="73e0e-188">A few more details on resource class:</span></span>

* <span data-ttu-id="73e0e-189">*ALTER role* krävs behörighet toochange hello resursklassen för en användare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-189">*Alter role* permission is required toochange hello resource class of a user.</span></span>
* <span data-ttu-id="73e0e-190">Du kan lägga till en användare tooone eller flera av hello högre resursklasser, åsidosätter klasser dynamisk resurs statiska resursklasser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-190">Although you can add a user tooone or more of hello higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="73e0e-191">Det vill säga om en användare har tilldelats tooboth **mediumrc**(dynamisk) och **staticrc80**(statisk) **mediumrc** är hello resursklass som har lösts in.</span><span class="sxs-lookup"><span data-stu-id="73e0e-191">That is, if a user is assigned tooboth **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is hello resource class that is honored.</span></span>
 * <span data-ttu-id="73e0e-192">När en användare har tilldelats toomore än en resursklass i en viss klass resurstyp (mer än en dynamisk resursklassen eller mer än en statisk resursklassen), funktion hello högsta resursklassen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-192">When a user is assigned toomore than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), hello highest resource class is honored.</span></span> <span data-ttu-id="73e0e-193">Om en användare har tilldelats tooboth mediumrc och largerc, funktion som är hello högre resursklass (largerc).</span><span class="sxs-lookup"><span data-stu-id="73e0e-193">That is, if a user is assigned tooboth mediumrc and largerc, hello higher resource class (largerc) is honored.</span></span> <span data-ttu-id="73e0e-194">Och om en användare har tilldelats tooboth **staticrc20** och **statirc80**, **staticrc80** är hanteras.</span><span class="sxs-lookup"><span data-stu-id="73e0e-194">And if a user is assigned tooboth **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="73e0e-195">hello resursklassen för hello system administrativa användare kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="73e0e-195">hello resource class of hello system administrative user cannot be changed.</span></span>

<span data-ttu-id="73e0e-196">Ett detaljerat exempel finns [ändrar användaren resurs klassexempel](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="73e0e-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="73e0e-197">Minnesallokering</span><span class="sxs-lookup"><span data-stu-id="73e0e-197">Memory allocation</span></span>
<span data-ttu-id="73e0e-198">Det finns för- och nackdelar tooincreasing resursklassen för en användare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-198">There are pros and cons tooincreasing a user's resource class.</span></span> <span data-ttu-id="73e0e-199">Öka en resursklass för en användare får sina frågor åtkomst toomore minne, vilket kan innebära att köra frågor snabbare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-199">Increasing a resource class for a user, gives their queries access toomore memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="73e0e-200">Högre resurs klasserna dock minska också hello antalet frågor som kan köras.</span><span class="sxs-lookup"><span data-stu-id="73e0e-200">However, higher resource classes also reduce hello number of concurrent queries that can run.</span></span> <span data-ttu-id="73e0e-201">Detta är hello säkerhetsaspekten tilldelning av stora mängder minne tooa enskild fråga eller att tillåta att andra frågor som måste också minnesallokering toorun samtidigt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-201">This is hello trade-off between allocating large amounts of memory tooa single query or allowing other queries, which also need memory allocations, toorun concurrently.</span></span> <span data-ttu-id="73e0e-202">Om en användare får hög tilldelning av minne för en fråga, andra användare har åtkomst toothat inte samma minne toorun en fråga.</span><span class="sxs-lookup"><span data-stu-id="73e0e-202">If one user is given high allocations of memory for a query, other users will not have access toothat same memory toorun a query.</span></span>

<span data-ttu-id="73e0e-203">i den följande tabellen hello mappar hello-minne som allokerats tooeach distribution av DWU och resurs-klassen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-203">hello following table maps hello memory allocated tooeach distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="73e0e-204">Minnesallokering per distribution för dynamisk resursklasser (MB)</span><span class="sxs-lookup"><span data-stu-id="73e0e-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="73e0e-205">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-205">DWU</span></span> | <span data-ttu-id="73e0e-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-206">smallrc</span></span> | <span data-ttu-id="73e0e-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-207">mediumrc</span></span> | <span data-ttu-id="73e0e-208">largerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-208">largerc</span></span> | <span data-ttu-id="73e0e-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="73e0e-210">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-210">DW100</span></span> |<span data-ttu-id="73e0e-211">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-211">100</span></span> |<span data-ttu-id="73e0e-212">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-212">100</span></span> |<span data-ttu-id="73e0e-213">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-213">200</span></span> |<span data-ttu-id="73e0e-214">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-214">400</span></span> |
| <span data-ttu-id="73e0e-215">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-215">DW200</span></span> |<span data-ttu-id="73e0e-216">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-216">100</span></span> |<span data-ttu-id="73e0e-217">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-217">200</span></span> |<span data-ttu-id="73e0e-218">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-218">400</span></span> |<span data-ttu-id="73e0e-219">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-219">800</span></span> |
| <span data-ttu-id="73e0e-220">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-220">DW300</span></span> |<span data-ttu-id="73e0e-221">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-221">100</span></span> |<span data-ttu-id="73e0e-222">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-222">200</span></span> |<span data-ttu-id="73e0e-223">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-223">400</span></span> |<span data-ttu-id="73e0e-224">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-224">800</span></span> |
| <span data-ttu-id="73e0e-225">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-225">DW400</span></span> |<span data-ttu-id="73e0e-226">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-226">100</span></span> |<span data-ttu-id="73e0e-227">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-227">400</span></span> |<span data-ttu-id="73e0e-228">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-228">800</span></span> |<span data-ttu-id="73e0e-229">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-229">1,600</span></span> |
| <span data-ttu-id="73e0e-230">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-230">DW500</span></span> |<span data-ttu-id="73e0e-231">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-231">100</span></span> |<span data-ttu-id="73e0e-232">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-232">400</span></span> |<span data-ttu-id="73e0e-233">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-233">800</span></span> |<span data-ttu-id="73e0e-234">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-234">1,600</span></span> |
| <span data-ttu-id="73e0e-235">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-235">DW600</span></span> |<span data-ttu-id="73e0e-236">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-236">100</span></span> |<span data-ttu-id="73e0e-237">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-237">400</span></span> |<span data-ttu-id="73e0e-238">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-238">800</span></span> |<span data-ttu-id="73e0e-239">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-239">1,600</span></span> |
| <span data-ttu-id="73e0e-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-240">DW1000</span></span> |<span data-ttu-id="73e0e-241">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-241">100</span></span> |<span data-ttu-id="73e0e-242">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-242">800</span></span> |<span data-ttu-id="73e0e-243">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-243">1,600</span></span> |<span data-ttu-id="73e0e-244">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-244">3,200</span></span> |
| <span data-ttu-id="73e0e-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-245">DW1200</span></span> |<span data-ttu-id="73e0e-246">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-246">100</span></span> |<span data-ttu-id="73e0e-247">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-247">800</span></span> |<span data-ttu-id="73e0e-248">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-248">1,600</span></span> |<span data-ttu-id="73e0e-249">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-249">3,200</span></span> |
| <span data-ttu-id="73e0e-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-250">DW1500</span></span> |<span data-ttu-id="73e0e-251">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-251">100</span></span> |<span data-ttu-id="73e0e-252">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-252">800</span></span> |<span data-ttu-id="73e0e-253">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-253">1,600</span></span> |<span data-ttu-id="73e0e-254">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-254">3,200</span></span> |
| <span data-ttu-id="73e0e-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-255">DW2000</span></span> |<span data-ttu-id="73e0e-256">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-256">100</span></span> |<span data-ttu-id="73e0e-257">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-257">1,600</span></span> |<span data-ttu-id="73e0e-258">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-258">3,200</span></span> |<span data-ttu-id="73e0e-259">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-259">6,400</span></span> |
| <span data-ttu-id="73e0e-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-260">DW3000</span></span> |<span data-ttu-id="73e0e-261">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-261">100</span></span> |<span data-ttu-id="73e0e-262">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-262">1,600</span></span> |<span data-ttu-id="73e0e-263">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-263">3,200</span></span> |<span data-ttu-id="73e0e-264">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-264">6,400</span></span> |
| <span data-ttu-id="73e0e-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-265">DW6000</span></span> |<span data-ttu-id="73e0e-266">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-266">100</span></span> |<span data-ttu-id="73e0e-267">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-267">3,200</span></span> |<span data-ttu-id="73e0e-268">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-268">6,400</span></span> |<span data-ttu-id="73e0e-269">12,800</span><span class="sxs-lookup"><span data-stu-id="73e0e-269">12,800</span></span> |

<span data-ttu-id="73e0e-270">i den följande tabellen hello mappar hello-minne som allokerats tooeach distribution av DWU och statiska resursklassen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-270">hello following table maps hello memory allocated tooeach distribution by DWU and static resource class.</span></span> <span data-ttu-id="73e0e-271">Observera att hello högre resurs klasserna ha sina minne minskas toohonor hello övergripande DWU gränser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-271">Note that hello higher resource classes have their memory reduced toohonor hello global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="73e0e-272">Minnesallokering per distribution för statiska resursklasser (MB)</span><span class="sxs-lookup"><span data-stu-id="73e0e-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="73e0e-273">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-273">DWU</span></span> | <span data-ttu-id="73e0e-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="73e0e-274">staticrc10</span></span> | <span data-ttu-id="73e0e-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="73e0e-275">staticrc20</span></span> | <span data-ttu-id="73e0e-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="73e0e-276">staticrc30</span></span> | <span data-ttu-id="73e0e-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="73e0e-277">staticrc40</span></span> | <span data-ttu-id="73e0e-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="73e0e-278">staticrc50</span></span> | <span data-ttu-id="73e0e-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="73e0e-279">staticrc60</span></span> | <span data-ttu-id="73e0e-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="73e0e-280">staticrc70</span></span> | <span data-ttu-id="73e0e-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="73e0e-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="73e0e-282">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-282">DW100</span></span> |<span data-ttu-id="73e0e-283">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-283">100</span></span> |<span data-ttu-id="73e0e-284">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-284">200</span></span> |<span data-ttu-id="73e0e-285">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-285">400</span></span> |<span data-ttu-id="73e0e-286">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-286">400</span></span> |<span data-ttu-id="73e0e-287">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-287">400</span></span> |<span data-ttu-id="73e0e-288">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-288">400</span></span> |<span data-ttu-id="73e0e-289">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-289">400</span></span> |<span data-ttu-id="73e0e-290">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-290">400</span></span> |
| <span data-ttu-id="73e0e-291">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-291">DW200</span></span> |<span data-ttu-id="73e0e-292">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-292">100</span></span> |<span data-ttu-id="73e0e-293">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-293">200</span></span> |<span data-ttu-id="73e0e-294">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-294">400</span></span> |<span data-ttu-id="73e0e-295">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-295">800</span></span> |<span data-ttu-id="73e0e-296">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-296">800</span></span> |<span data-ttu-id="73e0e-297">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-297">800</span></span> |<span data-ttu-id="73e0e-298">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-298">800</span></span> |<span data-ttu-id="73e0e-299">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-299">800</span></span> |
| <span data-ttu-id="73e0e-300">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-300">DW300</span></span> |<span data-ttu-id="73e0e-301">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-301">100</span></span> |<span data-ttu-id="73e0e-302">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-302">200</span></span> |<span data-ttu-id="73e0e-303">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-303">400</span></span> |<span data-ttu-id="73e0e-304">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-304">800</span></span> |<span data-ttu-id="73e0e-305">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-305">800</span></span> |<span data-ttu-id="73e0e-306">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-306">800</span></span> |<span data-ttu-id="73e0e-307">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-307">800</span></span> |<span data-ttu-id="73e0e-308">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-308">800</span></span> |
| <span data-ttu-id="73e0e-309">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-309">DW400</span></span> |<span data-ttu-id="73e0e-310">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-310">100</span></span> |<span data-ttu-id="73e0e-311">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-311">200</span></span> |<span data-ttu-id="73e0e-312">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-312">400</span></span> |<span data-ttu-id="73e0e-313">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-313">800</span></span> |<span data-ttu-id="73e0e-314">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-314">1,600</span></span> |<span data-ttu-id="73e0e-315">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-315">1,600</span></span> |<span data-ttu-id="73e0e-316">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-316">1,600</span></span> |<span data-ttu-id="73e0e-317">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-317">1,600</span></span> |
| <span data-ttu-id="73e0e-318">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-318">DW500</span></span> |<span data-ttu-id="73e0e-319">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-319">100</span></span> |<span data-ttu-id="73e0e-320">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-320">200</span></span> |<span data-ttu-id="73e0e-321">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-321">400</span></span> |<span data-ttu-id="73e0e-322">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-322">800</span></span> |<span data-ttu-id="73e0e-323">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-323">1,600</span></span> |<span data-ttu-id="73e0e-324">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-324">1,600</span></span> |<span data-ttu-id="73e0e-325">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-325">1,600</span></span> |<span data-ttu-id="73e0e-326">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-326">1,600</span></span> |
| <span data-ttu-id="73e0e-327">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-327">DW600</span></span> |<span data-ttu-id="73e0e-328">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-328">100</span></span> |<span data-ttu-id="73e0e-329">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-329">200</span></span> |<span data-ttu-id="73e0e-330">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-330">400</span></span> |<span data-ttu-id="73e0e-331">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-331">800</span></span> |<span data-ttu-id="73e0e-332">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-332">1,600</span></span> |<span data-ttu-id="73e0e-333">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-333">1,600</span></span> |<span data-ttu-id="73e0e-334">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-334">1,600</span></span> |<span data-ttu-id="73e0e-335">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-335">1,600</span></span> |
| <span data-ttu-id="73e0e-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-336">DW1000</span></span> |<span data-ttu-id="73e0e-337">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-337">100</span></span> |<span data-ttu-id="73e0e-338">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-338">200</span></span> |<span data-ttu-id="73e0e-339">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-339">400</span></span> |<span data-ttu-id="73e0e-340">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-340">800</span></span> |<span data-ttu-id="73e0e-341">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-341">1,600</span></span> |<span data-ttu-id="73e0e-342">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-342">3,200</span></span> |<span data-ttu-id="73e0e-343">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-343">3,200</span></span> |<span data-ttu-id="73e0e-344">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-344">3,200</span></span> |
| <span data-ttu-id="73e0e-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-345">DW1200</span></span> |<span data-ttu-id="73e0e-346">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-346">100</span></span> |<span data-ttu-id="73e0e-347">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-347">200</span></span> |<span data-ttu-id="73e0e-348">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-348">400</span></span> |<span data-ttu-id="73e0e-349">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-349">800</span></span> |<span data-ttu-id="73e0e-350">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-350">1,600</span></span> |<span data-ttu-id="73e0e-351">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-351">3,200</span></span> |<span data-ttu-id="73e0e-352">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-352">3,200</span></span> |<span data-ttu-id="73e0e-353">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-353">3,200</span></span> |
| <span data-ttu-id="73e0e-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-354">DW1500</span></span> |<span data-ttu-id="73e0e-355">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-355">100</span></span> |<span data-ttu-id="73e0e-356">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-356">200</span></span> |<span data-ttu-id="73e0e-357">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-357">400</span></span> |<span data-ttu-id="73e0e-358">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-358">800</span></span> |<span data-ttu-id="73e0e-359">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-359">1,600</span></span> |<span data-ttu-id="73e0e-360">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-360">3,200</span></span> |<span data-ttu-id="73e0e-361">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-361">3,200</span></span> |<span data-ttu-id="73e0e-362">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-362">3,200</span></span> |
| <span data-ttu-id="73e0e-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-363">DW2000</span></span> |<span data-ttu-id="73e0e-364">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-364">100</span></span> |<span data-ttu-id="73e0e-365">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-365">200</span></span> |<span data-ttu-id="73e0e-366">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-366">400</span></span> |<span data-ttu-id="73e0e-367">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-367">800</span></span> |<span data-ttu-id="73e0e-368">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-368">1,600</span></span> |<span data-ttu-id="73e0e-369">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-369">3,200</span></span> |<span data-ttu-id="73e0e-370">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-370">6,400</span></span> |<span data-ttu-id="73e0e-371">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-371">6,400</span></span> |
| <span data-ttu-id="73e0e-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-372">DW3000</span></span> |<span data-ttu-id="73e0e-373">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-373">100</span></span> |<span data-ttu-id="73e0e-374">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-374">200</span></span> |<span data-ttu-id="73e0e-375">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-375">400</span></span> |<span data-ttu-id="73e0e-376">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-376">800</span></span> |<span data-ttu-id="73e0e-377">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-377">1,600</span></span> |<span data-ttu-id="73e0e-378">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-378">3,200</span></span> |<span data-ttu-id="73e0e-379">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-379">6,400</span></span> |<span data-ttu-id="73e0e-380">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-380">6,400</span></span> |
| <span data-ttu-id="73e0e-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-381">DW6000</span></span> |<span data-ttu-id="73e0e-382">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-382">100</span></span> |<span data-ttu-id="73e0e-383">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-383">200</span></span> |<span data-ttu-id="73e0e-384">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-384">400</span></span> |<span data-ttu-id="73e0e-385">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-385">800</span></span> |<span data-ttu-id="73e0e-386">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-386">1,600</span></span> |<span data-ttu-id="73e0e-387">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-387">3,200</span></span> |<span data-ttu-id="73e0e-388">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-388">6,400</span></span> |<span data-ttu-id="73e0e-389">12,800</span><span class="sxs-lookup"><span data-stu-id="73e0e-389">12,800</span></span> |

<span data-ttu-id="73e0e-390">Från föregående tabell hello, kan du se att en fråga som körs på en DW2000 i hello **xlargerc** resursklassen skulle ha åtkomst too6, 400 MB minne inom varje hello 60 distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-390">From hello preceding table, you can see that a query running on a DW2000 in hello **xlargerc** resource class would have access too6,400 MB of memory within each of hello 60 distributed databases.</span></span>  <span data-ttu-id="73e0e-391">Det finns 60 distributioner i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73e0e-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="73e0e-392">Därför bör toocalculate hello totala minnesallokering för en fråga i en viss resurs-klass, hello ovan värden multipliceras med 60.</span><span class="sxs-lookup"><span data-stu-id="73e0e-392">Therefore, toocalculate hello total memory allocation for a query in a given resource class, hello above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="73e0e-393">Minne-allokeringar systemomfattande (GB)</span><span class="sxs-lookup"><span data-stu-id="73e0e-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="73e0e-394">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-394">DWU</span></span> | <span data-ttu-id="73e0e-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-395">smallrc</span></span> | <span data-ttu-id="73e0e-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-396">mediumrc</span></span> | <span data-ttu-id="73e0e-397">largerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-397">largerc</span></span> | <span data-ttu-id="73e0e-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="73e0e-399">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-399">DW100</span></span> |<span data-ttu-id="73e0e-400">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-400">6</span></span> |<span data-ttu-id="73e0e-401">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-401">6</span></span> |<span data-ttu-id="73e0e-402">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-402">12</span></span> |<span data-ttu-id="73e0e-403">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-403">23</span></span> |
| <span data-ttu-id="73e0e-404">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-404">DW200</span></span> |<span data-ttu-id="73e0e-405">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-405">6</span></span> |<span data-ttu-id="73e0e-406">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-406">12</span></span> |<span data-ttu-id="73e0e-407">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-407">23</span></span> |<span data-ttu-id="73e0e-408">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-408">47</span></span> |
| <span data-ttu-id="73e0e-409">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-409">DW300</span></span> |<span data-ttu-id="73e0e-410">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-410">6</span></span> |<span data-ttu-id="73e0e-411">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-411">12</span></span> |<span data-ttu-id="73e0e-412">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-412">23</span></span> |<span data-ttu-id="73e0e-413">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-413">47</span></span> |
| <span data-ttu-id="73e0e-414">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-414">DW400</span></span> |<span data-ttu-id="73e0e-415">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-415">6</span></span> |<span data-ttu-id="73e0e-416">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-416">23</span></span> |<span data-ttu-id="73e0e-417">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-417">47</span></span> |<span data-ttu-id="73e0e-418">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-418">94</span></span> |
| <span data-ttu-id="73e0e-419">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-419">DW500</span></span> |<span data-ttu-id="73e0e-420">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-420">6</span></span> |<span data-ttu-id="73e0e-421">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-421">23</span></span> |<span data-ttu-id="73e0e-422">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-422">47</span></span> |<span data-ttu-id="73e0e-423">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-423">94</span></span> |
| <span data-ttu-id="73e0e-424">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-424">DW600</span></span> |<span data-ttu-id="73e0e-425">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-425">6</span></span> |<span data-ttu-id="73e0e-426">23</span><span class="sxs-lookup"><span data-stu-id="73e0e-426">23</span></span> |<span data-ttu-id="73e0e-427">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-427">47</span></span> |<span data-ttu-id="73e0e-428">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-428">94</span></span> |
| <span data-ttu-id="73e0e-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-429">DW1000</span></span> |<span data-ttu-id="73e0e-430">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-430">6</span></span> |<span data-ttu-id="73e0e-431">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-431">47</span></span> |<span data-ttu-id="73e0e-432">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-432">94</span></span> |<span data-ttu-id="73e0e-433">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-433">188</span></span> |
| <span data-ttu-id="73e0e-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-434">DW1200</span></span> |<span data-ttu-id="73e0e-435">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-435">6</span></span> |<span data-ttu-id="73e0e-436">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-436">47</span></span> |<span data-ttu-id="73e0e-437">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-437">94</span></span> |<span data-ttu-id="73e0e-438">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-438">188</span></span> |
| <span data-ttu-id="73e0e-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-439">DW1500</span></span> |<span data-ttu-id="73e0e-440">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-440">6</span></span> |<span data-ttu-id="73e0e-441">47</span><span class="sxs-lookup"><span data-stu-id="73e0e-441">47</span></span> |<span data-ttu-id="73e0e-442">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-442">94</span></span> |<span data-ttu-id="73e0e-443">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-443">188</span></span> |
| <span data-ttu-id="73e0e-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-444">DW2000</span></span> |<span data-ttu-id="73e0e-445">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-445">6</span></span> |<span data-ttu-id="73e0e-446">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-446">94</span></span> |<span data-ttu-id="73e0e-447">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-447">188</span></span> |<span data-ttu-id="73e0e-448">375</span><span class="sxs-lookup"><span data-stu-id="73e0e-448">375</span></span> |
| <span data-ttu-id="73e0e-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-449">DW3000</span></span> |<span data-ttu-id="73e0e-450">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-450">6</span></span> |<span data-ttu-id="73e0e-451">94</span><span class="sxs-lookup"><span data-stu-id="73e0e-451">94</span></span> |<span data-ttu-id="73e0e-452">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-452">188</span></span> |<span data-ttu-id="73e0e-453">375</span><span class="sxs-lookup"><span data-stu-id="73e0e-453">375</span></span> |
| <span data-ttu-id="73e0e-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-454">DW6000</span></span> |<span data-ttu-id="73e0e-455">6</span><span class="sxs-lookup"><span data-stu-id="73e0e-455">6</span></span> |<span data-ttu-id="73e0e-456">188</span><span class="sxs-lookup"><span data-stu-id="73e0e-456">188</span></span> |<span data-ttu-id="73e0e-457">375</span><span class="sxs-lookup"><span data-stu-id="73e0e-457">375</span></span> |<span data-ttu-id="73e0e-458">750</span><span class="sxs-lookup"><span data-stu-id="73e0e-458">750</span></span> |

<span data-ttu-id="73e0e-459">Från den här tabellen systemomfattande minnesallokering, ser du att en fråga som körs på en DW2000 i hello xlargerc resursklassen fördelas 375 GB minne totalt (6 400 MB * 60 distributioner / 1 024 tooconvert tooGB) över hello hela ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73e0e-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in hello xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 tooconvert tooGB) over hello entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="73e0e-460">hello gäller samma beräkning toostatic resursklasser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-460">hello same calculation applies toostatic resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="73e0e-461">Concurrency fack förbrukning</span><span class="sxs-lookup"><span data-stu-id="73e0e-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="73e0e-462">SQL Data Warehouse ger mer minne tooqueries kör högre resurs klasserna.</span><span class="sxs-lookup"><span data-stu-id="73e0e-462">SQL Data Warehouse grants more memory tooqueries running in higher resource classes.</span></span> <span data-ttu-id="73e0e-463">Minnet är en fast resurs.</span><span class="sxs-lookup"><span data-stu-id="73e0e-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="73e0e-464">Därför hello allokera mer minne per fråga hello färre samtidiga frågor kan köra.</span><span class="sxs-lookup"><span data-stu-id="73e0e-464">Therefore, hello more memory allocated per query, hello fewer concurrent queries can execute.</span></span> <span data-ttu-id="73e0e-465">hello reiterates följande tabell alla hello tidigare begrepp i en enda vy som visar hello antalet samtidiga tillgängliga platser av DWU och hello fack som används av varje resursklassen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-465">hello following table reiterates all of hello previous concepts in a single view that shows hello number of concurrency slots available by DWU and hello slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="73e0e-466">Fördelningen och förbrukningen av samtidighet fack för dynamisk resursklasser</span><span class="sxs-lookup"><span data-stu-id="73e0e-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="73e0e-467">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-467">DWU</span></span> | <span data-ttu-id="73e0e-468">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="73e0e-468">Maximum concurrent queries</span></span> | <span data-ttu-id="73e0e-469">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="73e0e-469">Concurrency slots allocated</span></span> | <span data-ttu-id="73e0e-470">Platser som används av smallrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-470">Slots used by smallrc</span></span> | <span data-ttu-id="73e0e-471">Platser som används av mediumrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-471">Slots used by mediumrc</span></span> | <span data-ttu-id="73e0e-472">Platser som används av largerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-472">Slots used by largerc</span></span> | <span data-ttu-id="73e0e-473">Platser som används av xlargerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="73e0e-474">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-474">DW100</span></span> |<span data-ttu-id="73e0e-475">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-475">4</span></span> |<span data-ttu-id="73e0e-476">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-476">4</span></span> |<span data-ttu-id="73e0e-477">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-477">1</span></span> |<span data-ttu-id="73e0e-478">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-478">1</span></span> |<span data-ttu-id="73e0e-479">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-479">2</span></span> |<span data-ttu-id="73e0e-480">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-480">4</span></span> |
| <span data-ttu-id="73e0e-481">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-481">DW200</span></span> |<span data-ttu-id="73e0e-482">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-482">8</span></span> |<span data-ttu-id="73e0e-483">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-483">8</span></span> |<span data-ttu-id="73e0e-484">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-484">1</span></span> |<span data-ttu-id="73e0e-485">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-485">2</span></span> |<span data-ttu-id="73e0e-486">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-486">4</span></span> |<span data-ttu-id="73e0e-487">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-487">8</span></span> |
| <span data-ttu-id="73e0e-488">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-488">DW300</span></span> |<span data-ttu-id="73e0e-489">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-489">12</span></span> |<span data-ttu-id="73e0e-490">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-490">12</span></span> |<span data-ttu-id="73e0e-491">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-491">1</span></span> |<span data-ttu-id="73e0e-492">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-492">2</span></span> |<span data-ttu-id="73e0e-493">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-493">4</span></span> |<span data-ttu-id="73e0e-494">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-494">8</span></span> |
| <span data-ttu-id="73e0e-495">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-495">DW400</span></span> |<span data-ttu-id="73e0e-496">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-496">16</span></span> |<span data-ttu-id="73e0e-497">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-497">16</span></span> |<span data-ttu-id="73e0e-498">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-498">1</span></span> |<span data-ttu-id="73e0e-499">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-499">4</span></span> |<span data-ttu-id="73e0e-500">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-500">8</span></span> |<span data-ttu-id="73e0e-501">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-501">16</span></span> |
| <span data-ttu-id="73e0e-502">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-502">DW500</span></span> |<span data-ttu-id="73e0e-503">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-503">20</span></span> |<span data-ttu-id="73e0e-504">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-504">20</span></span> |<span data-ttu-id="73e0e-505">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-505">1</span></span> |<span data-ttu-id="73e0e-506">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-506">4</span></span> |<span data-ttu-id="73e0e-507">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-507">8</span></span> |<span data-ttu-id="73e0e-508">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-508">16</span></span> |
| <span data-ttu-id="73e0e-509">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-509">DW600</span></span> |<span data-ttu-id="73e0e-510">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-510">24</span></span> |<span data-ttu-id="73e0e-511">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-511">24</span></span> |<span data-ttu-id="73e0e-512">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-512">1</span></span> |<span data-ttu-id="73e0e-513">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-513">4</span></span> |<span data-ttu-id="73e0e-514">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-514">8</span></span> |<span data-ttu-id="73e0e-515">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-515">16</span></span> |
| <span data-ttu-id="73e0e-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-516">DW1000</span></span> |<span data-ttu-id="73e0e-517">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-517">32</span></span> |<span data-ttu-id="73e0e-518">40</span><span class="sxs-lookup"><span data-stu-id="73e0e-518">40</span></span> |<span data-ttu-id="73e0e-519">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-519">1</span></span> |<span data-ttu-id="73e0e-520">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-520">8</span></span> |<span data-ttu-id="73e0e-521">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-521">16</span></span> |<span data-ttu-id="73e0e-522">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-522">32</span></span> |
| <span data-ttu-id="73e0e-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-523">DW1200</span></span> |<span data-ttu-id="73e0e-524">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-524">32</span></span> |<span data-ttu-id="73e0e-525">48</span><span class="sxs-lookup"><span data-stu-id="73e0e-525">48</span></span> |<span data-ttu-id="73e0e-526">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-526">1</span></span> |<span data-ttu-id="73e0e-527">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-527">8</span></span> |<span data-ttu-id="73e0e-528">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-528">16</span></span> |<span data-ttu-id="73e0e-529">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-529">32</span></span> |
| <span data-ttu-id="73e0e-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-530">DW1500</span></span> |<span data-ttu-id="73e0e-531">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-531">32</span></span> |<span data-ttu-id="73e0e-532">60</span><span class="sxs-lookup"><span data-stu-id="73e0e-532">60</span></span> |<span data-ttu-id="73e0e-533">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-533">1</span></span> |<span data-ttu-id="73e0e-534">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-534">8</span></span> |<span data-ttu-id="73e0e-535">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-535">16</span></span> |<span data-ttu-id="73e0e-536">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-536">32</span></span> |
| <span data-ttu-id="73e0e-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-537">DW2000</span></span> |<span data-ttu-id="73e0e-538">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-538">32</span></span> |<span data-ttu-id="73e0e-539">80</span><span class="sxs-lookup"><span data-stu-id="73e0e-539">80</span></span> |<span data-ttu-id="73e0e-540">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-540">1</span></span> |<span data-ttu-id="73e0e-541">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-541">16</span></span> |<span data-ttu-id="73e0e-542">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-542">32</span></span> |<span data-ttu-id="73e0e-543">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-543">64</span></span> |
| <span data-ttu-id="73e0e-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-544">DW3000</span></span> |<span data-ttu-id="73e0e-545">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-545">32</span></span> |<span data-ttu-id="73e0e-546">120</span><span class="sxs-lookup"><span data-stu-id="73e0e-546">120</span></span> |<span data-ttu-id="73e0e-547">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-547">1</span></span> |<span data-ttu-id="73e0e-548">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-548">16</span></span> |<span data-ttu-id="73e0e-549">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-549">32</span></span> |<span data-ttu-id="73e0e-550">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-550">64</span></span> |
| <span data-ttu-id="73e0e-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-551">DW6000</span></span> |<span data-ttu-id="73e0e-552">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-552">32</span></span> |<span data-ttu-id="73e0e-553">240</span><span class="sxs-lookup"><span data-stu-id="73e0e-553">240</span></span> |<span data-ttu-id="73e0e-554">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-554">1</span></span> |<span data-ttu-id="73e0e-555">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-555">32</span></span> |<span data-ttu-id="73e0e-556">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-556">64</span></span> |<span data-ttu-id="73e0e-557">128</span><span class="sxs-lookup"><span data-stu-id="73e0e-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="73e0e-558">Fördelningen och förbrukningen av samtidighet fack för statiska resursklasser</span><span class="sxs-lookup"><span data-stu-id="73e0e-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="73e0e-559">DWU</span><span class="sxs-lookup"><span data-stu-id="73e0e-559">DWU</span></span> | <span data-ttu-id="73e0e-560">Maximalt antal samtidiga frågor</span><span class="sxs-lookup"><span data-stu-id="73e0e-560">Maximum concurrent queries</span></span> | <span data-ttu-id="73e0e-561">Concurrency fack allokerade</span><span class="sxs-lookup"><span data-stu-id="73e0e-561">Concurrency slots allocated</span></span> |<span data-ttu-id="73e0e-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="73e0e-562">staticrc10</span></span> | <span data-ttu-id="73e0e-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="73e0e-563">staticrc20</span></span> | <span data-ttu-id="73e0e-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="73e0e-564">staticrc30</span></span> | <span data-ttu-id="73e0e-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="73e0e-565">staticrc40</span></span> | <span data-ttu-id="73e0e-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="73e0e-566">staticrc50</span></span> | <span data-ttu-id="73e0e-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="73e0e-567">staticrc60</span></span> | <span data-ttu-id="73e0e-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="73e0e-568">staticrc70</span></span> | <span data-ttu-id="73e0e-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="73e0e-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="73e0e-570">DW100</span><span class="sxs-lookup"><span data-stu-id="73e0e-570">DW100</span></span> |<span data-ttu-id="73e0e-571">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-571">4</span></span> |<span data-ttu-id="73e0e-572">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-572">4</span></span> |<span data-ttu-id="73e0e-573">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-573">1</span></span> |<span data-ttu-id="73e0e-574">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-574">2</span></span> |<span data-ttu-id="73e0e-575">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-575">4</span></span> |<span data-ttu-id="73e0e-576">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-576">4</span></span> |<span data-ttu-id="73e0e-577">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-577">4</span></span> |<span data-ttu-id="73e0e-578">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-578">4</span></span> |<span data-ttu-id="73e0e-579">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-579">4</span></span> |<span data-ttu-id="73e0e-580">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-580">4</span></span> |
| <span data-ttu-id="73e0e-581">DW200</span><span class="sxs-lookup"><span data-stu-id="73e0e-581">DW200</span></span> |<span data-ttu-id="73e0e-582">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-582">8</span></span> |<span data-ttu-id="73e0e-583">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-583">8</span></span> |<span data-ttu-id="73e0e-584">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-584">1</span></span> |<span data-ttu-id="73e0e-585">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-585">2</span></span> |<span data-ttu-id="73e0e-586">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-586">4</span></span> |<span data-ttu-id="73e0e-587">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-587">8</span></span> |<span data-ttu-id="73e0e-588">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-588">8</span></span> |<span data-ttu-id="73e0e-589">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-589">8</span></span> |<span data-ttu-id="73e0e-590">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-590">8</span></span> |<span data-ttu-id="73e0e-591">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-591">8</span></span> |
| <span data-ttu-id="73e0e-592">DW300</span><span class="sxs-lookup"><span data-stu-id="73e0e-592">DW300</span></span> |<span data-ttu-id="73e0e-593">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-593">12</span></span> |<span data-ttu-id="73e0e-594">12</span><span class="sxs-lookup"><span data-stu-id="73e0e-594">12</span></span> |<span data-ttu-id="73e0e-595">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-595">1</span></span> |<span data-ttu-id="73e0e-596">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-596">2</span></span> |<span data-ttu-id="73e0e-597">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-597">4</span></span> |<span data-ttu-id="73e0e-598">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-598">8</span></span> |<span data-ttu-id="73e0e-599">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-599">8</span></span> |<span data-ttu-id="73e0e-600">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-600">8</span></span> |<span data-ttu-id="73e0e-601">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-601">8</span></span> |<span data-ttu-id="73e0e-602">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-602">8</span></span> |
| <span data-ttu-id="73e0e-603">DW400</span><span class="sxs-lookup"><span data-stu-id="73e0e-603">DW400</span></span> |<span data-ttu-id="73e0e-604">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-604">16</span></span> |<span data-ttu-id="73e0e-605">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-605">16</span></span> |<span data-ttu-id="73e0e-606">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-606">1</span></span> |<span data-ttu-id="73e0e-607">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-607">2</span></span> |<span data-ttu-id="73e0e-608">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-608">4</span></span> |<span data-ttu-id="73e0e-609">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-609">8</span></span> |<span data-ttu-id="73e0e-610">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-610">16</span></span> |<span data-ttu-id="73e0e-611">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-611">16</span></span> |<span data-ttu-id="73e0e-612">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-612">16</span></span> |<span data-ttu-id="73e0e-613">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-613">16</span></span> |
| <span data-ttu-id="73e0e-614">DW500</span><span class="sxs-lookup"><span data-stu-id="73e0e-614">DW500</span></span> | <span data-ttu-id="73e0e-615">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-615">20</span></span>| <span data-ttu-id="73e0e-616">20</span><span class="sxs-lookup"><span data-stu-id="73e0e-616">20</span></span>| <span data-ttu-id="73e0e-617">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-617">1</span></span>| <span data-ttu-id="73e0e-618">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-618">2</span></span>| <span data-ttu-id="73e0e-619">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-619">4</span></span>| <span data-ttu-id="73e0e-620">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-620">8</span></span>| <span data-ttu-id="73e0e-621">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-621">16</span></span>| <span data-ttu-id="73e0e-622">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-622">16</span></span>| <span data-ttu-id="73e0e-623">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-623">16</span></span>| <span data-ttu-id="73e0e-624">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-624">16</span></span>|
| <span data-ttu-id="73e0e-625">DW600</span><span class="sxs-lookup"><span data-stu-id="73e0e-625">DW600</span></span> | <span data-ttu-id="73e0e-626">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-626">24</span></span>| <span data-ttu-id="73e0e-627">24</span><span class="sxs-lookup"><span data-stu-id="73e0e-627">24</span></span>| <span data-ttu-id="73e0e-628">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-628">1</span></span>| <span data-ttu-id="73e0e-629">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-629">2</span></span>| <span data-ttu-id="73e0e-630">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-630">4</span></span>| <span data-ttu-id="73e0e-631">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-631">8</span></span>| <span data-ttu-id="73e0e-632">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-632">16</span></span>| <span data-ttu-id="73e0e-633">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-633">16</span></span>| <span data-ttu-id="73e0e-634">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-634">16</span></span>| <span data-ttu-id="73e0e-635">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-635">16</span></span>|
| <span data-ttu-id="73e0e-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="73e0e-636">DW1000</span></span> | <span data-ttu-id="73e0e-637">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-637">32</span></span>| <span data-ttu-id="73e0e-638">40</span><span class="sxs-lookup"><span data-stu-id="73e0e-638">40</span></span>| <span data-ttu-id="73e0e-639">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-639">1</span></span>| <span data-ttu-id="73e0e-640">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-640">2</span></span>| <span data-ttu-id="73e0e-641">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-641">4</span></span>| <span data-ttu-id="73e0e-642">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-642">8</span></span>| <span data-ttu-id="73e0e-643">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-643">16</span></span>| <span data-ttu-id="73e0e-644">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-644">32</span></span>| <span data-ttu-id="73e0e-645">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-645">32</span></span>| <span data-ttu-id="73e0e-646">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-646">32</span></span>|
| <span data-ttu-id="73e0e-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="73e0e-647">DW1200</span></span> | <span data-ttu-id="73e0e-648">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-648">32</span></span>| <span data-ttu-id="73e0e-649">48</span><span class="sxs-lookup"><span data-stu-id="73e0e-649">48</span></span>| <span data-ttu-id="73e0e-650">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-650">1</span></span>| <span data-ttu-id="73e0e-651">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-651">2</span></span>| <span data-ttu-id="73e0e-652">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-652">4</span></span>| <span data-ttu-id="73e0e-653">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-653">8</span></span>| <span data-ttu-id="73e0e-654">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-654">16</span></span>| <span data-ttu-id="73e0e-655">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-655">32</span></span>| <span data-ttu-id="73e0e-656">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-656">32</span></span>| <span data-ttu-id="73e0e-657">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-657">32</span></span>|
| <span data-ttu-id="73e0e-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="73e0e-658">DW1500</span></span> | <span data-ttu-id="73e0e-659">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-659">32</span></span>| <span data-ttu-id="73e0e-660">60</span><span class="sxs-lookup"><span data-stu-id="73e0e-660">60</span></span>| <span data-ttu-id="73e0e-661">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-661">1</span></span>| <span data-ttu-id="73e0e-662">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-662">2</span></span>| <span data-ttu-id="73e0e-663">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-663">4</span></span>| <span data-ttu-id="73e0e-664">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-664">8</span></span>| <span data-ttu-id="73e0e-665">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-665">16</span></span>| <span data-ttu-id="73e0e-666">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-666">32</span></span>| <span data-ttu-id="73e0e-667">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-667">32</span></span>| <span data-ttu-id="73e0e-668">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-668">32</span></span>|
| <span data-ttu-id="73e0e-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="73e0e-669">DW2000</span></span> | <span data-ttu-id="73e0e-670">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-670">32</span></span>| <span data-ttu-id="73e0e-671">80</span><span class="sxs-lookup"><span data-stu-id="73e0e-671">80</span></span>| <span data-ttu-id="73e0e-672">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-672">1</span></span>| <span data-ttu-id="73e0e-673">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-673">2</span></span>| <span data-ttu-id="73e0e-674">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-674">4</span></span>| <span data-ttu-id="73e0e-675">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-675">8</span></span>| <span data-ttu-id="73e0e-676">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-676">16</span></span>| <span data-ttu-id="73e0e-677">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-677">32</span></span>| <span data-ttu-id="73e0e-678">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-678">64</span></span>| <span data-ttu-id="73e0e-679">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-679">64</span></span>|
| <span data-ttu-id="73e0e-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="73e0e-680">DW3000</span></span> | <span data-ttu-id="73e0e-681">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-681">32</span></span>| <span data-ttu-id="73e0e-682">120</span><span class="sxs-lookup"><span data-stu-id="73e0e-682">120</span></span>| <span data-ttu-id="73e0e-683">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-683">1</span></span>| <span data-ttu-id="73e0e-684">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-684">2</span></span>| <span data-ttu-id="73e0e-685">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-685">4</span></span>| <span data-ttu-id="73e0e-686">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-686">8</span></span>| <span data-ttu-id="73e0e-687">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-687">16</span></span>| <span data-ttu-id="73e0e-688">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-688">32</span></span>| <span data-ttu-id="73e0e-689">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-689">64</span></span>| <span data-ttu-id="73e0e-690">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-690">64</span></span>|
| <span data-ttu-id="73e0e-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="73e0e-691">DW6000</span></span> | <span data-ttu-id="73e0e-692">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-692">32</span></span>| <span data-ttu-id="73e0e-693">240</span><span class="sxs-lookup"><span data-stu-id="73e0e-693">240</span></span>| <span data-ttu-id="73e0e-694">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-694">1</span></span>| <span data-ttu-id="73e0e-695">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-695">2</span></span>| <span data-ttu-id="73e0e-696">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-696">4</span></span>| <span data-ttu-id="73e0e-697">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-697">8</span></span>| <span data-ttu-id="73e0e-698">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-698">16</span></span>| <span data-ttu-id="73e0e-699">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-699">32</span></span>| <span data-ttu-id="73e0e-700">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-700">64</span></span>| <span data-ttu-id="73e0e-701">128</span><span class="sxs-lookup"><span data-stu-id="73e0e-701">128</span></span>|

<span data-ttu-id="73e0e-702">Du kan se som kör SQL Data Warehouse som DW1000 allokerar högst 32 samtidiga frågor och totalt 40 samtidighet fack från dessa tabeller.</span><span class="sxs-lookup"><span data-stu-id="73e0e-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="73e0e-703">Om alla användare kör i smallrc, skulle 32 samtidiga förfrågningar tillåts eftersom varje fråga förbrukar 1 samtidighet plats.</span><span class="sxs-lookup"><span data-stu-id="73e0e-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="73e0e-704">Om alla användare på en DW1000 kördes i mediumrc, varje fråga skulle allokeras 800 MB per distribution för en totala minnesallokering 47 GB per fråga och samtidighet skulle vara begränsad too5 användare (40 samtidighet fack / 8 kortplatser per mediumrc användare).</span><span class="sxs-lookup"><span data-stu-id="73e0e-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited too5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="73e0e-705">Att välja rätt resursklassen</span><span class="sxs-lookup"><span data-stu-id="73e0e-705">Selecting proper resource class</span></span>  
<span data-ttu-id="73e0e-706">Det är en bra idé toopermanently tilldela användare tooa resursklassen i stället för att ändra deras resursklasser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-706">A good practice is toopermanently assign users tooa resource class rather than changing their resource classes.</span></span> <span data-ttu-id="73e0e-707">Till exempel skapa belastningar tooclustered columnstore-tabeller högre kvalitet index när mer minne allokeras.</span><span class="sxs-lookup"><span data-stu-id="73e0e-707">For example, loads tooclustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="73e0e-708">tooensure som belastningar har toohigher minne, skapa en användare för att överföra data och tilldela den här användaren tooa högre resursklassen permanent.</span><span class="sxs-lookup"><span data-stu-id="73e0e-708">tooensure that loads have access toohigher memory, create a user specifically for loading data and permanently assign this user tooa higher resource class.</span></span>
<span data-ttu-id="73e0e-709">Det finns ett par av bästa praxis toofollow här.</span><span class="sxs-lookup"><span data-stu-id="73e0e-709">There are a couple of best practices toofollow here.</span></span> <span data-ttu-id="73e0e-710">Som nämnts ovan är SQL DW stöder två typer av klassen resurstyper: statisk resursklasser och dynamisk resursklasser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="73e0e-711">Inläsning av bästa praxis</span><span class="sxs-lookup"><span data-stu-id="73e0e-711">Loading best practices</span></span>
1.  <span data-ttu-id="73e0e-712">Om hello förväntningar belastningar med vanlig mängd data, är en statisk resursklassen ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="73e0e-712">If hello expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="73e0e-713">Senare, när skala upp tooget mer dataresurser, hello datalagret ska kunna frågar toorun flera samtidiga out-of-the-box, som hello belastningen användaren inte förbruka mer minne.</span><span class="sxs-lookup"><span data-stu-id="73e0e-713">Later, when scaling up tooget more computational power, hello data warehouse will be able toorun more concurrent queries out-of-the-box, as hello load user does not consume more memory.</span></span>
2.  <span data-ttu-id="73e0e-714">Om hello förväntningar större belastningar i vissa fall, är en dynamisk resursklassen bra.</span><span class="sxs-lookup"><span data-stu-id="73e0e-714">If hello expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="73e0e-715">Senare, när du ökar tooget mer dataresurser, hello belastningen användare får mer minne out-of-the-box, så därför att hello belastningen tooperform snabbare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-715">Later, when scaling up tooget more computational power, hello load user will get more memory out-of-the-box, hence allowing hello load tooperform faster.</span></span>

<span data-ttu-id="73e0e-716">hello minneskrav tooprocess belastningar effektivt beror på hello uppbyggnad hello-tabell som är inlästa och hello mängden data som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="73e0e-716">hello memory needed tooprocess loads efficiently depends on hello nature of hello table loaded and hello amount of data processed.</span></span> <span data-ttu-id="73e0e-717">Läsa in data i CCI tabeller kräver till exempel vissa minne toolet CCI rowgroups nå uppfylldes.</span><span class="sxs-lookup"><span data-stu-id="73e0e-717">For instance, loading data into CCI tables requires some memory toolet CCI rowgroups reach optimality.</span></span> <span data-ttu-id="73e0e-718">Mer information finns i hello Columnstore-index - vägledning för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-718">For more details, see hello Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="73e0e-719">Som bästa praxis, rekommenderar vi att du toouse minst 200MB minne för belastning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-719">As a best practice, we advise you toouse at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="73e0e-720">Frågar bästa praxis</span><span class="sxs-lookup"><span data-stu-id="73e0e-720">Querying best practices</span></span>
<span data-ttu-id="73e0e-721">Frågor har olika krav beroende på deras komplexitet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="73e0e-722">Öka minnesmängd per fråga eller öka hello samtidighet är båda ogiltig sätt tooaugment totala genomflödet beroende på hello frågan behov.</span><span class="sxs-lookup"><span data-stu-id="73e0e-722">Increasing memory per query or increasing hello concurrency are both valid ways tooaugment overall throughput depending on hello query needs.</span></span>
1.  <span data-ttu-id="73e0e-723">Om hello förväntningar regelbundna, komplexa frågor (till exempel toogenerate dagliga och veckovisa rapporter) och behöver inte tootake nytta av samtidighet, dynamiska resursklassen är ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="73e0e-723">If hello expectations are regular, complex queries (for instance, toogenerate daily and weekly reports) and do not need tootake advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="73e0e-724">Om hello system har mer data tooprocess, ger skala upp hello datalagret därför automatiskt mer minne toohello användaren hello frågan körs.</span><span class="sxs-lookup"><span data-stu-id="73e0e-724">If hello system has more data tooprocess, scaling up hello data warehouse will therefore automatically provide more memory toohello user running hello query.</span></span>
2.  <span data-ttu-id="73e0e-725">Om hello förväntningar variabel eller profil över samtidighet mönster (till exempel om hello databasen efterfrågas via ett webbgränssnitt brett tillgänglig), statiska resursklassen är ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="73e0e-725">If hello expectations are variable or diurnal concurrency patterns (for instance if hello database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="73e0e-726">Senare, när du ökar toodata datalager, hello användaren som associerats med hello statiska resursklassen kommer automatiskt att kan toorun flera samtidiga frågor.</span><span class="sxs-lookup"><span data-stu-id="73e0e-726">Later, when scaling up toodata warehouse, hello user associated with hello static resource class will automatically be able toorun more concurrent queries.</span></span>

<span data-ttu-id="73e0e-727">Genom att välja rätt minnestilldelningen beroende på hello behovet av din fråga är icke-trivial som beror på många faktorer, till exempel hello mängden data som efterfrågas, hello uppbyggnad hello tabellscheman och olika koppling, val och grupp-predikat.</span><span class="sxs-lookup"><span data-stu-id="73e0e-727">Selecting proper memory grant depending on hello need of your query is non-trivial, as it depends on many factors, such as hello amount of data queried, hello nature of hello table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="73e0e-728">Från en allmän synvinkel allokera mer minne tillåter frågor toocomplete snabbare, men skulle minska hello övergripande samtidighet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-728">From a general standpoint, allocating more memory will allow queries toocomplete faster, but would reduce hello overall concurrency.</span></span> <span data-ttu-id="73e0e-729">Om samtidighet inte är ett problem, skadas över minnesallokering inte.</span><span class="sxs-lookup"><span data-stu-id="73e0e-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="73e0e-730">toofine finjustera dataflöde, försök olika varianter av resursklasser kan krävas.</span><span class="sxs-lookup"><span data-stu-id="73e0e-730">toofine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="73e0e-731">Du kan använda hello följande lagrade procedur toofigure ut samtidighet och minne bevilja per resursklassen vid en given SLO och hello närmaste bästa resursklassen för minne beräkningsintensiva CCI åtgärder på CCI partitionerade tabellen vid en viss resurs-klass:</span><span class="sxs-lookup"><span data-stu-id="73e0e-731">You can use hello following stored procedure toofigure out concurrency and memory grant per resource class at a given SLO and hello closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="73e0e-732">Beskrivning:</span><span class="sxs-lookup"><span data-stu-id="73e0e-732">Description:</span></span>  
<span data-ttu-id="73e0e-733">Här är hello syftet med den här lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="73e0e-733">Here's hello purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="73e0e-734">toohelp användaren ta reda på samtidighet och minne bevilja per resursklassen vid ett givet Servicenivåmål.</span><span class="sxs-lookup"><span data-stu-id="73e0e-734">toohelp user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="73e0e-735">Användaren måste tooprovide NULL för både schema- och tablename för den här enligt hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="73e0e-735">User needs tooprovide NULL for both schema and tablename for this as shown in hello example below.</span></span>  
2. <span data-ttu-id="73e0e-736">toohelp användaren räkna ut hello närmaste bästa resursklassen för hello minne intensed CCI åtgärder (belastning, kopiera tabellen återskapa index, etc.) på icke partitionerad CCI tabell till en viss resurs-klass.</span><span class="sxs-lookup"><span data-stu-id="73e0e-736">toohelp user figure out hello closest best resource class for hello memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="73e0e-737">hello lagrade procedur använder tabellen schema toofind ut hello krävs minnestilldelningen för detta.</span><span class="sxs-lookup"><span data-stu-id="73e0e-737">hello stored proc uses table schema toofind out hello required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="73e0e-738">Beroenden och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="73e0e-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="73e0e-739">Den här lagrade proceduren är inte utformad toocalculate minnesutrymmet för partitionerad cci tabellen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-739">This stored proc is not designed toocalculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="73e0e-740">Den här lagrade proceduren tar inte minneskravet hänsyn för hello väljer en del av CTAS/INSERT-Välj och förutsätter att den toobe ett enkelt SELECT.</span><span class="sxs-lookup"><span data-stu-id="73e0e-740">This stored proc doesn't take memory requirement into account for hello SELECT part of CTAS/INSERT-SELECT and assumes it toobe a simple SELECT.</span></span>
- <span data-ttu-id="73e0e-741">Den här lagrade proceduren använder en temporär tabell så kan användas i hello session där den här lagrade proceduren har skapats.</span><span class="sxs-lookup"><span data-stu-id="73e0e-741">This stored proc uses a temp table so this can be used in hello session where this stored proc was created.</span></span>    
- <span data-ttu-id="73e0e-742">Den här lagrade proceduren beror på hello aktuella erbjudanden (t.ex. maskinvarukonfiguration, DMS-konfiguration) och om någon av som ändras sedan denna lagrade procedur fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-742">This stored proc depends on hello current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="73e0e-743">Den här lagrade proceduren beror på befintliga erbjudna samtidighet gränsen och om att ändringar sedan den här lagrade proceduren fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="73e0e-744">Denna lagrade procedur beror på befintlig resurs klassen erbjudanden och om att ändringar sedan detta lagrade procedur wuold fungerar inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="73e0e-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="73e0e-745">Om du inte får utdata när du kör lagrad procedur med parametrar som ges det kan vara två fall.</span><span class="sxs-lookup"><span data-stu-id="73e0e-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="73e0e-746">1. Antingen DW-parametern innehåller ett ogiltigt värde för Servicenivåmål</span><span class="sxs-lookup"><span data-stu-id="73e0e-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="73e0e-747">2. ELLER så finns det ingen matchande resursklassen för CCI åtgärd om tabellnamn angavs.</span><span class="sxs-lookup"><span data-stu-id="73e0e-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="73e0e-748">Till exempel på DW100, högsta minnestilldelningen är 400MB och om tabellschemat är bred tillräckligt med toocross hello krav 400 MB.</span><span class="sxs-lookup"><span data-stu-id="73e0e-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough toocross hello requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="73e0e-749">Exempel på användning:</span><span class="sxs-lookup"><span data-stu-id="73e0e-749">Usage example:</span></span>
<span data-ttu-id="73e0e-750">Syntax:</span><span class="sxs-lookup"><span data-stu-id="73e0e-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="73e0e-751">@DWU:Ange antingen en NULL-parameter tooextract hello aktuella DWU från hello databasen för Datalagret eller ange någon stöds DWU i hello form av 'DW100'</span><span class="sxs-lookup"><span data-stu-id="73e0e-751">@DWU: Either provide a NULL parameter tooextract hello current DWU from hello DW DB or provide any supported DWU in hello form of 'DW100'</span></span>
2. <span data-ttu-id="73e0e-752">@SCHEMA_NAME:Ange ett namn på hello tabell</span><span class="sxs-lookup"><span data-stu-id="73e0e-752">@SCHEMA_NAME: Provide a schema name of hello table</span></span>
3. <span data-ttu-id="73e0e-753">@TABLE_NAME:Ange ett tabellnamn hello intressanta</span><span class="sxs-lookup"><span data-stu-id="73e0e-753">@TABLE_NAME: Provide a table name of hello interest</span></span>

<span data-ttu-id="73e0e-754">Exempel köra den här lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="73e0e-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="73e0e-755">Tabell 1 används i hello exemplen ovan kunde skapas enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="73e0e-755">Table1 used in hello above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-hello-stored-procedure-definition"></a><span data-ttu-id="73e0e-756">Här är hello lagrade Procedurdefinition:</span><span class="sxs-lookup"><span data-stu-id="73e0e-756">Here's hello stored procedure definition:</span></span>
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
-- Selecting proper DWU for hello current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need toosupply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) toohold mapping info.
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
-- Creating workload mapping tootheir corresponding slot consumption and default memory grant.
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

## <a name="query-importance"></a><span data-ttu-id="73e0e-757">Vikten av frågan</span><span class="sxs-lookup"><span data-stu-id="73e0e-757">Query importance</span></span>
<span data-ttu-id="73e0e-758">SQL Data Warehouse implementerar resursklasser med hjälp av arbetsbelastningsgrupper.</span><span class="sxs-lookup"><span data-stu-id="73e0e-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="73e0e-759">Det finns totalt åtta arbetsbelastningsgrupper som kontrollerar hello funktionssätt hello resursklasser över hello olika DWU-storlekar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-759">There are a total of eight workload groups that control hello behavior of hello resource classes across hello various DWU sizes.</span></span> <span data-ttu-id="73e0e-760">För alla DWU använder SQL Data Warehouse bara fyra hello åtta arbetsbelastningsgrupper.</span><span class="sxs-lookup"><span data-stu-id="73e0e-760">For any DWU, SQL Data Warehouse uses only four of hello eight workload groups.</span></span> <span data-ttu-id="73e0e-761">Detta är meningsfullt eftersom varje arbetsbelastningsgruppen tilldelas tooone av fyra resursklasser: smallrc mediumrc, largerc, eller xlargerc.</span><span class="sxs-lookup"><span data-stu-id="73e0e-761">This makes sense because each workload group is assigned tooone of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="73e0e-762">hello betydelse förstå hello arbetsbelastningsgrupper är att vissa av dessa arbetsbelastningsgrupper är inställda toohigher *vikten*.</span><span class="sxs-lookup"><span data-stu-id="73e0e-762">hello importance of understanding hello workload groups is that some of these workload groups are set toohigher *importance*.</span></span> <span data-ttu-id="73e0e-763">Vikten används för CPU schemaläggning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="73e0e-764">Frågor som körs med hög prioritet får tre gånger så mycket CPU-cykler än de med medelhög prioritet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="73e0e-765">Därför avgör samtidighet fack mappningar också CPU-prioritet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="73e0e-766">När en fråga förbrukar 16 eller flera platser, körs det som hög prioritet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="73e0e-767">hello visar följande tabell hello vikten mappningar för varje arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-767">hello following table shows hello importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-tooconcurrency-slots-and-importance"></a><span data-ttu-id="73e0e-768">Arbetsbelastningen grupp mappningar tooconcurrency platser och betydelse</span><span class="sxs-lookup"><span data-stu-id="73e0e-768">Workload group mappings tooconcurrency slots and importance</span></span>
| <span data-ttu-id="73e0e-769">Arbetsbelastningsgrupper</span><span class="sxs-lookup"><span data-stu-id="73e0e-769">Workload groups</span></span> | <span data-ttu-id="73e0e-770">Concurrency fack mappning</span><span class="sxs-lookup"><span data-stu-id="73e0e-770">Concurrency slot mapping</span></span> | <span data-ttu-id="73e0e-771">MB / Distribution</span><span class="sxs-lookup"><span data-stu-id="73e0e-771">MB / Distribution</span></span> | <span data-ttu-id="73e0e-772">Vikten mappning</span><span class="sxs-lookup"><span data-stu-id="73e0e-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="73e0e-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="73e0e-773">SloDWGroupC00</span></span> |<span data-ttu-id="73e0e-774">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-774">1</span></span> |<span data-ttu-id="73e0e-775">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-775">100</span></span> |<span data-ttu-id="73e0e-776">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-776">Medium</span></span> |
| <span data-ttu-id="73e0e-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="73e0e-777">SloDWGroupC01</span></span> |<span data-ttu-id="73e0e-778">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-778">2</span></span> |<span data-ttu-id="73e0e-779">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-779">200</span></span> |<span data-ttu-id="73e0e-780">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-780">Medium</span></span> |
| <span data-ttu-id="73e0e-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="73e0e-781">SloDWGroupC02</span></span> |<span data-ttu-id="73e0e-782">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-782">4</span></span> |<span data-ttu-id="73e0e-783">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-783">400</span></span> |<span data-ttu-id="73e0e-784">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-784">Medium</span></span> |
| <span data-ttu-id="73e0e-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-785">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-786">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-786">8</span></span> |<span data-ttu-id="73e0e-787">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-787">800</span></span> |<span data-ttu-id="73e0e-788">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-788">Medium</span></span> |
| <span data-ttu-id="73e0e-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="73e0e-789">SloDWGroupC04</span></span> |<span data-ttu-id="73e0e-790">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-790">16</span></span> |<span data-ttu-id="73e0e-791">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-791">1,600</span></span> |<span data-ttu-id="73e0e-792">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-792">High</span></span> |
| <span data-ttu-id="73e0e-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="73e0e-793">SloDWGroupC05</span></span> |<span data-ttu-id="73e0e-794">32</span><span class="sxs-lookup"><span data-stu-id="73e0e-794">32</span></span> |<span data-ttu-id="73e0e-795">3,200</span><span class="sxs-lookup"><span data-stu-id="73e0e-795">3,200</span></span> |<span data-ttu-id="73e0e-796">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-796">High</span></span> |
| <span data-ttu-id="73e0e-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="73e0e-797">SloDWGroupC06</span></span> |<span data-ttu-id="73e0e-798">64</span><span class="sxs-lookup"><span data-stu-id="73e0e-798">64</span></span> |<span data-ttu-id="73e0e-799">6,400</span><span class="sxs-lookup"><span data-stu-id="73e0e-799">6,400</span></span> |<span data-ttu-id="73e0e-800">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-800">High</span></span> |
| <span data-ttu-id="73e0e-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="73e0e-801">SloDWGroupC07</span></span> |<span data-ttu-id="73e0e-802">128</span><span class="sxs-lookup"><span data-stu-id="73e0e-802">128</span></span> |<span data-ttu-id="73e0e-803">12,800</span><span class="sxs-lookup"><span data-stu-id="73e0e-803">12,800</span></span> |<span data-ttu-id="73e0e-804">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-804">High</span></span> |

<span data-ttu-id="73e0e-805">Från hello **fördelningen och förbrukningen av samtidighet fack** diagram, ser du att en DW500 1, 4, 8 och 16 samtidighet fack för smallrc, mediumrc, largerc och xlargerc, respektive.</span><span class="sxs-lookup"><span data-stu-id="73e0e-805">From hello **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="73e0e-806">Du kan slå dessa värden upp i föregående diagram toofind hello vikten för varje resursklassen hello.</span><span class="sxs-lookup"><span data-stu-id="73e0e-806">You can look those values up in hello preceding chart toofind hello importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-tooimportance"></a><span data-ttu-id="73e0e-807">DW500 mappning av resursen klasser tooimportance</span><span class="sxs-lookup"><span data-stu-id="73e0e-807">DW500 mapping of resource classes tooimportance</span></span>
| <span data-ttu-id="73e0e-808">Resursklass</span><span class="sxs-lookup"><span data-stu-id="73e0e-808">Resource class</span></span> | <span data-ttu-id="73e0e-809">Arbetsbelastningsgruppen</span><span class="sxs-lookup"><span data-stu-id="73e0e-809">Workload group</span></span> | <span data-ttu-id="73e0e-810">Concurrency-platser som används</span><span class="sxs-lookup"><span data-stu-id="73e0e-810">Concurrency slots used</span></span> | <span data-ttu-id="73e0e-811">MB / Distribution</span><span class="sxs-lookup"><span data-stu-id="73e0e-811">MB / Distribution</span></span> | <span data-ttu-id="73e0e-812">Betydelse</span><span class="sxs-lookup"><span data-stu-id="73e0e-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="73e0e-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-813">smallrc</span></span> |<span data-ttu-id="73e0e-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="73e0e-814">SloDWGroupC00</span></span> |<span data-ttu-id="73e0e-815">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-815">1</span></span> |<span data-ttu-id="73e0e-816">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-816">100</span></span> |<span data-ttu-id="73e0e-817">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-817">Medium</span></span> |
| <span data-ttu-id="73e0e-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="73e0e-818">mediumrc</span></span> |<span data-ttu-id="73e0e-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="73e0e-819">SloDWGroupC02</span></span> |<span data-ttu-id="73e0e-820">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-820">4</span></span> |<span data-ttu-id="73e0e-821">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-821">400</span></span> |<span data-ttu-id="73e0e-822">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-822">Medium</span></span> |
| <span data-ttu-id="73e0e-823">largerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-823">largerc</span></span> |<span data-ttu-id="73e0e-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-824">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-825">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-825">8</span></span> |<span data-ttu-id="73e0e-826">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-826">800</span></span> |<span data-ttu-id="73e0e-827">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-827">Medium</span></span> |
| <span data-ttu-id="73e0e-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="73e0e-828">xlargerc</span></span> |<span data-ttu-id="73e0e-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="73e0e-829">SloDWGroupC04</span></span> |<span data-ttu-id="73e0e-830">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-830">16</span></span> |<span data-ttu-id="73e0e-831">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-831">1,600</span></span> |<span data-ttu-id="73e0e-832">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-832">High</span></span> |
| <span data-ttu-id="73e0e-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="73e0e-833">staticrc10</span></span> |<span data-ttu-id="73e0e-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="73e0e-834">SloDWGroupC00</span></span> |<span data-ttu-id="73e0e-835">1</span><span class="sxs-lookup"><span data-stu-id="73e0e-835">1</span></span> |<span data-ttu-id="73e0e-836">100</span><span class="sxs-lookup"><span data-stu-id="73e0e-836">100</span></span> |<span data-ttu-id="73e0e-837">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-837">Medium</span></span> |
| <span data-ttu-id="73e0e-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="73e0e-838">staticrc20</span></span> |<span data-ttu-id="73e0e-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="73e0e-839">SloDWGroupC01</span></span> |<span data-ttu-id="73e0e-840">2</span><span class="sxs-lookup"><span data-stu-id="73e0e-840">2</span></span> |<span data-ttu-id="73e0e-841">200</span><span class="sxs-lookup"><span data-stu-id="73e0e-841">200</span></span> |<span data-ttu-id="73e0e-842">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-842">Medium</span></span> |
| <span data-ttu-id="73e0e-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="73e0e-843">staticrc30</span></span> |<span data-ttu-id="73e0e-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="73e0e-844">SloDWGroupC02</span></span> |<span data-ttu-id="73e0e-845">4</span><span class="sxs-lookup"><span data-stu-id="73e0e-845">4</span></span> |<span data-ttu-id="73e0e-846">400</span><span class="sxs-lookup"><span data-stu-id="73e0e-846">400</span></span> |<span data-ttu-id="73e0e-847">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-847">Medium</span></span> |
| <span data-ttu-id="73e0e-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="73e0e-848">staticrc40</span></span> |<span data-ttu-id="73e0e-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-849">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-850">8</span><span class="sxs-lookup"><span data-stu-id="73e0e-850">8</span></span> |<span data-ttu-id="73e0e-851">800</span><span class="sxs-lookup"><span data-stu-id="73e0e-851">800</span></span> |<span data-ttu-id="73e0e-852">Medel</span><span class="sxs-lookup"><span data-stu-id="73e0e-852">Medium</span></span> |
| <span data-ttu-id="73e0e-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="73e0e-853">staticrc50</span></span> |<span data-ttu-id="73e0e-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-854">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-855">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-855">16</span></span> |<span data-ttu-id="73e0e-856">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-856">1,600</span></span> |<span data-ttu-id="73e0e-857">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-857">High</span></span> |
| <span data-ttu-id="73e0e-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="73e0e-858">staticrc60</span></span> |<span data-ttu-id="73e0e-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-859">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-860">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-860">16</span></span> |<span data-ttu-id="73e0e-861">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-861">1,600</span></span> |<span data-ttu-id="73e0e-862">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-862">High</span></span> |
| <span data-ttu-id="73e0e-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="73e0e-863">staticrc70</span></span> |<span data-ttu-id="73e0e-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-864">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-865">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-865">16</span></span> |<span data-ttu-id="73e0e-866">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-866">1,600</span></span> |<span data-ttu-id="73e0e-867">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-867">High</span></span> |
| <span data-ttu-id="73e0e-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="73e0e-868">staticrc80</span></span> |<span data-ttu-id="73e0e-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="73e0e-869">SloDWGroupC03</span></span> |<span data-ttu-id="73e0e-870">16</span><span class="sxs-lookup"><span data-stu-id="73e0e-870">16</span></span> |<span data-ttu-id="73e0e-871">1,600</span><span class="sxs-lookup"><span data-stu-id="73e0e-871">1,600</span></span> |<span data-ttu-id="73e0e-872">Hög</span><span class="sxs-lookup"><span data-stu-id="73e0e-872">High</span></span> |

<span data-ttu-id="73e0e-873">Du kan använda hello följande DMV-frågan toolook på hello skillnader i minnet resursallokeringen i detalj från hello perspektiv hello resursstyrningen eller tooanalyze aktiva och historisk användning av hello arbetsbelastningsgrupper när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="73e0e-873">You can use hello following DMV query toolook at hello differences in memory resource allocation in detail from hello perspective of hello resource governor, or tooanalyze active and historic usage of hello workload groups when troubleshooting.</span></span>

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

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="73e0e-874">Frågor som följer gränser för samtidig användning</span><span class="sxs-lookup"><span data-stu-id="73e0e-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="73e0e-875">De flesta frågor regleras av resursklasser.</span><span class="sxs-lookup"><span data-stu-id="73e0e-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="73e0e-876">De här frågorna måste få plats inuti både hello samtidiga frågan och samtidighet fack tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="73e0e-876">These queries must fit inside both hello concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="73e0e-877">En användare kan inte välja tooexclude en fråga från hello samtidighet fack modell.</span><span class="sxs-lookup"><span data-stu-id="73e0e-877">A user cannot choose tooexclude a query from hello concurrency slot model.</span></span>

<span data-ttu-id="73e0e-878">tooreiterate, hello följande instruktioner respektera resursklasser:</span><span class="sxs-lookup"><span data-stu-id="73e0e-878">tooreiterate, hello following statements honor resource classes:</span></span>

* <span data-ttu-id="73e0e-879">INFOGA VÄLJER</span><span class="sxs-lookup"><span data-stu-id="73e0e-879">INSERT-SELECT</span></span>
* <span data-ttu-id="73e0e-880">UPPDATERING</span><span class="sxs-lookup"><span data-stu-id="73e0e-880">UPDATE</span></span>
* <span data-ttu-id="73e0e-881">TA BORT</span><span class="sxs-lookup"><span data-stu-id="73e0e-881">DELETE</span></span>
* <span data-ttu-id="73e0e-882">Välj (när du frågar användartabeller)</span><span class="sxs-lookup"><span data-stu-id="73e0e-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="73e0e-883">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="73e0e-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="73e0e-884">ALTER INDEX REORGANIZE</span><span class="sxs-lookup"><span data-stu-id="73e0e-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="73e0e-885">ALTER TABLE REBUILD</span><span class="sxs-lookup"><span data-stu-id="73e0e-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="73e0e-886">SKAPA INDEX</span><span class="sxs-lookup"><span data-stu-id="73e0e-886">CREATE INDEX</span></span>
* <span data-ttu-id="73e0e-887">SKAPA DET GRUPPERADE COLUMNSTORE-INDEX</span><span class="sxs-lookup"><span data-stu-id="73e0e-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="73e0e-888">SKAPA TABLE AS SELECT (CTAS)</span><span class="sxs-lookup"><span data-stu-id="73e0e-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="73e0e-889">Läsa in data</span><span class="sxs-lookup"><span data-stu-id="73e0e-889">Data loading</span></span>
* <span data-ttu-id="73e0e-890">Dataflyttsåtgärderna som utförs av hello Data Movement Service (DMS)</span><span class="sxs-lookup"><span data-stu-id="73e0e-890">Data movement operations conducted by hello Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-tooconcurrency-limits"></a><span data-ttu-id="73e0e-891">Frågan undantag tooconcurrency gränser</span><span class="sxs-lookup"><span data-stu-id="73e0e-891">Query exceptions tooconcurrency limits</span></span>
<span data-ttu-id="73e0e-892">Några frågor följdes inte hello resurs klassen toowhich hello användaren har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="73e0e-892">Some queries do not honor hello resource class toowhich hello user is assigned.</span></span> <span data-ttu-id="73e0e-893">Dessa undantag toohello samtidighet gränser görs när hello minnesresurser som krävs för ett visst kommando är låg, ofta eftersom hello-kommandot är en metadata-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="73e0e-893">These exceptions toohello concurrency limits are made when hello memory resources needed for a particular command are low, often because hello command is a metadata operation.</span></span> <span data-ttu-id="73e0e-894">hello syftet med sådana undantag är tooavoid större minnesallokering för frågor som behöver dem aldrig.</span><span class="sxs-lookup"><span data-stu-id="73e0e-894">hello goal of these exceptions is tooavoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="73e0e-895">I dessa fall tilldelad hello standard små resursklass (smallrc) används alltid oavsett hello faktiska resursklassen toohello användare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-895">In these cases, hello default small resource class (smallrc) is always used regardless of hello actual resource class assigned toohello user.</span></span> <span data-ttu-id="73e0e-896">Till exempel `CREATE LOGIN` körs alltid i smallrc.</span><span class="sxs-lookup"><span data-stu-id="73e0e-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="73e0e-897">hello toofulfill för resurser som krävs för den här åtgärden är mycket låg så att den inte gör meningsfullt tooinclude hello frågan i hello concurrency fack-modell.</span><span class="sxs-lookup"><span data-stu-id="73e0e-897">hello resources required toofulfill this operation are very low, so it does not make sense tooinclude hello query in hello concurrency slot model.</span></span>  <span data-ttu-id="73e0e-898">De här frågorna också begränsas inte av hello 32 användaren samtidighet gränsen, ett obegränsat antal dessa frågor kan köra upp toohello session på högst 1 024 sessioner.</span><span class="sxs-lookup"><span data-stu-id="73e0e-898">These queries are also not limited by hello 32 user concurrency limit, an unlimited number of these queries can run up toohello session limit of 1,024 sessions.</span></span>

<span data-ttu-id="73e0e-899">följande instruktioner hello följdes inte resursklasser:</span><span class="sxs-lookup"><span data-stu-id="73e0e-899">hello following statements do not honor resource classes:</span></span>

* <span data-ttu-id="73e0e-900">Skapa eller ta bort tabell</span><span class="sxs-lookup"><span data-stu-id="73e0e-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="73e0e-901">ALTER TABLE... VÄXELN, dela och slå samman partitionen</span><span class="sxs-lookup"><span data-stu-id="73e0e-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="73e0e-902">ALTER INDEX INAKTIVERA</span><span class="sxs-lookup"><span data-stu-id="73e0e-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="73e0e-903">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="73e0e-903">DROP INDEX</span></span>
* <span data-ttu-id="73e0e-904">Skapa, uppdatera och ta bort statistik</span><span class="sxs-lookup"><span data-stu-id="73e0e-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="73e0e-905">TRUNKERA TABELLEN</span><span class="sxs-lookup"><span data-stu-id="73e0e-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="73e0e-906">ÄNDRA TILLSTÅND</span><span class="sxs-lookup"><span data-stu-id="73e0e-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="73e0e-907">SKAPA INLOGGNING</span><span class="sxs-lookup"><span data-stu-id="73e0e-907">CREATE LOGIN</span></span>
* <span data-ttu-id="73e0e-908">Skapa, ändra eller släppa användaren</span><span class="sxs-lookup"><span data-stu-id="73e0e-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="73e0e-909">Skapa, ändra eller släppa proceduren</span><span class="sxs-lookup"><span data-stu-id="73e0e-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="73e0e-910">Skapa eller ta bort vy</span><span class="sxs-lookup"><span data-stu-id="73e0e-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="73e0e-911">LÄGGA TILL VÄRDEN</span><span class="sxs-lookup"><span data-stu-id="73e0e-911">INSERT VALUES</span></span>
* <span data-ttu-id="73e0e-912">Välj systemvyer och av DMV: er</span><span class="sxs-lookup"><span data-stu-id="73e0e-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="73e0e-913">FÖRKLARAR</span><span class="sxs-lookup"><span data-stu-id="73e0e-913">EXPLAIN</span></span>
* <span data-ttu-id="73e0e-914">DBCC</span><span class="sxs-lookup"><span data-stu-id="73e0e-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="73e0e-915"><a name="changing-user-resource-class-example"></a>Ändra ett exempel på användaren resurs klass</span><span class="sxs-lookup"><span data-stu-id="73e0e-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="73e0e-916">**Skapa inloggning:** öppna en anslutning tooyour **master** databasen på hello SQLServer är värd för SQL Data Warehouse-databas och kör följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="73e0e-916">**Create login:** Open a connection tooyour **master** database on hello SQL server hosting your SQL Data Warehouse database and execute hello following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="73e0e-917">Det är en bra idé toocreate en användare i hello huvuddatabasen för Azure SQL Data Warehouse-användare.</span><span class="sxs-lookup"><span data-stu-id="73e0e-917">It is a good idea toocreate a user in hello master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="73e0e-918">Skapa en användare i master kan en användare toologin med hjälp av verktyg som SSMS utan att ange ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="73e0e-918">Creating a user in master allows a user toologin using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="73e0e-919">Det gör dem också toouse hello object explorer tooview alla databaser på en SQLServer.</span><span class="sxs-lookup"><span data-stu-id="73e0e-919">It also allows them toouse hello object explorer tooview all databases on a SQL server.</span></span>  <span data-ttu-id="73e0e-920">Mer information om att skapa och hantera användare finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="73e0e-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="73e0e-921">**Skapa SQL Data Warehouse användare:** öppna en anslutning toohello **SQL Data Warehouse** databasen och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="73e0e-921">**Create SQL Data Warehouse user:** Open a connection toohello **SQL Data Warehouse** database and execute hello following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="73e0e-922">**Bevilja behörighet:** hello följande exempel ger `CONTROL` på hello **SQL Data Warehouse** databas.</span><span class="sxs-lookup"><span data-stu-id="73e0e-922">**Grant permissions:** hello following example grants `CONTROL` on hello **SQL Data Warehouse** database.</span></span> <span data-ttu-id="73e0e-923">`CONTROL`vid hello är databasnivå hello motsvarar db_owner i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="73e0e-923">`CONTROL` at hello database level is hello equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW toonewperson;
    ```
4. <span data-ttu-id="73e0e-924">**Öka resursklass:** Använd hello följande fråga tooadd en tooa högre arbetsbelastning management användarroll.</span><span class="sxs-lookup"><span data-stu-id="73e0e-924">**Increase resource class:** Use hello following query tooadd a user tooa higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="73e0e-925">**Minska resursklass:** Använd hello följande fråga tooremove en användare från en roll för arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="73e0e-925">**Decrease resource class:** Use hello following query tooremove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="73e0e-926">Det är inte möjligt tooremove en användare från smallrc.</span><span class="sxs-lookup"><span data-stu-id="73e0e-926">It is not possible tooremove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="73e0e-927">Köade frågan identifiering och andra av DMV: er</span><span class="sxs-lookup"><span data-stu-id="73e0e-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="73e0e-928">Du kan använda hello `sys.dm_pdw_exec_requests` DMV tooidentify frågor som väntar i en kö samtidighet.</span><span class="sxs-lookup"><span data-stu-id="73e0e-928">You can use hello `sys.dm_pdw_exec_requests` DMV tooidentify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="73e0e-929">Frågar väntar på en plats för samtidighet har statusen **avbruten**.</span><span class="sxs-lookup"><span data-stu-id="73e0e-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="73e0e-930">Roller för hantering av arbetsbelastning kan visas med `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="73e0e-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="73e0e-931">hello följande fråga visar vilken roll som varje användare har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="73e0e-931">hello following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="73e0e-932">SQL Data Warehouse är hello följande vänta typer:</span><span class="sxs-lookup"><span data-stu-id="73e0e-932">SQL Data Warehouse has hello following wait types:</span></span>

* <span data-ttu-id="73e0e-933">**LocalQueriesConcurrencyResourceType**: frågor som är placerade utanför hello samtidighet fack framework.</span><span class="sxs-lookup"><span data-stu-id="73e0e-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of hello concurrency slot framework.</span></span> <span data-ttu-id="73e0e-934">DMV frågor och system fungerar som `SELECT @@VERSION` är exempel på lokala frågor.</span><span class="sxs-lookup"><span data-stu-id="73e0e-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="73e0e-935">**UserConcurrencyResourceType**: frågor som är placerade i hello samtidighet fack framework.</span><span class="sxs-lookup"><span data-stu-id="73e0e-935">**UserConcurrencyResourceType**: Queries that sit inside hello concurrency slot framework.</span></span> <span data-ttu-id="73e0e-936">Frågor mot tabeller slutanvändarens representerar exempel som använder den här resurstypen.</span><span class="sxs-lookup"><span data-stu-id="73e0e-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="73e0e-937">**DmsConcurrencyResourceType**: Väntar till följd av dataflyttsåtgärderna.</span><span class="sxs-lookup"><span data-stu-id="73e0e-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="73e0e-938">**BackupConcurrencyResourceType**: den här vänta anger att en databas säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="73e0e-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="73e0e-939">hello högsta värdet för den här resurstypen är 1.</span><span class="sxs-lookup"><span data-stu-id="73e0e-939">hello maximum value for this resource type is 1.</span></span> <span data-ttu-id="73e0e-940">Om flera säkerhetskopieringar har begärts på hello samtidigt, hello andra placerar i kö.</span><span class="sxs-lookup"><span data-stu-id="73e0e-940">If multiple backups have been requested at hello same time, hello others will queue.</span></span>

<span data-ttu-id="73e0e-941">Hej `sys.dm_pdw_waits` DMV kan vara används toosee vilka resurser en begäran väntar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-941">hello `sys.dm_pdw_waits` DMV can be used toosee which resources a request is waiting for.</span></span>

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

<span data-ttu-id="73e0e-942">Hej `sys.dm_pdw_resource_waits` DMV visar endast hello resurs väntar används av en given fråga.</span><span class="sxs-lookup"><span data-stu-id="73e0e-942">hello `sys.dm_pdw_resource_waits` DMV shows only hello resource waits consumed by a given query.</span></span> <span data-ttu-id="73e0e-943">Resursen väntetiden endast mäter hello tid väntar på resurser toobe tillhandahålls, som skillnad från toosignal väntetid, vilket är hello tid det tar för hello underliggande SQL-servrar tooschedule hello-fråga till hello CPU.</span><span class="sxs-lookup"><span data-stu-id="73e0e-943">Resource wait time only measures hello time waiting for resources toobe provided, as opposed toosignal wait time, which is hello time it takes for hello underlying SQL servers tooschedule hello query onto hello CPU.</span></span>

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

<span data-ttu-id="73e0e-944">Hej `sys.dm_pdw_wait_stats` DMV kan användas för analys av historiska trender för väntar.</span><span class="sxs-lookup"><span data-stu-id="73e0e-944">hello `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="73e0e-945">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73e0e-945">Next steps</span></span>
<span data-ttu-id="73e0e-946">Mer information om hur du hanterar databasanvändare och säkerhet finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="73e0e-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="73e0e-947">Mer information om hur större resursklasser kan förbättra kvaliteten för grupperade columnstore-index, finns [återskapa index tooimprove segment kvalitet].</span><span class="sxs-lookup"><span data-stu-id="73e0e-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes tooimprove segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[återskapa index tooimprove segment kvalitet]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
