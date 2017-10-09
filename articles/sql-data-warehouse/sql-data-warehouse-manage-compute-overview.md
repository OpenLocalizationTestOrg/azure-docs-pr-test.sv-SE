---
title: "aaaManage beräkningskraft i Azure SQL Data Warehouse (översikt) | Microsoft Docs"
description: "Prestanda skala ut funktioner i Azure SQL Data Warehouse. Skala ut genom att justera dwu: er eller pausa och återuppta beräkning resurser toosave kostnader."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a><span data-ttu-id="0c230-104">Hantera datorkraft i Azure SQL Data Warehouse (översikt)</span><span class="sxs-lookup"><span data-stu-id="0c230-104">Manage compute power in Azure SQL Data Warehouse (Overview)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c230-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0c230-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="0c230-106">Portal</span><span class="sxs-lookup"><span data-stu-id="0c230-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="0c230-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c230-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="0c230-108">REST</span><span class="sxs-lookup"><span data-stu-id="0c230-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="0c230-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="0c230-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<span data-ttu-id="0c230-110">hello-arkitekturen för SQL Data Warehouse separerar lagring och beräkning, så att varje tooscale oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="0c230-110">hello architecture of SQL Data Warehouse separates storage and compute, allowing each tooscale independently.</span></span> <span data-ttu-id="0c230-111">Beräkning kan därför vara skalade toomeet prestandakrav oberoende av hello mängden data.</span><span class="sxs-lookup"><span data-stu-id="0c230-111">As a result, compute can be scaled toomeet performance demands independent of hello amount of data.</span></span> <span data-ttu-id="0c230-112">En naturlig följd av den här arkitekturen är att [fakturering] [ billed] för beräkning och lagring är desamma.</span><span class="sxs-lookup"><span data-stu-id="0c230-112">A natural consequence of this architecture is that [billing][billed] for compute and storage is separate.</span></span> 

<span data-ttu-id="0c230-113">Den här översikten beskriver hur skala ut fungerar med SQL Data Warehouse och hur tooutilize hello pausa, återuppta och skala funktionerna för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0c230-113">This overview describes how scale out works with SQL Data Warehouse and how tooutilize hello pause, resume, and scale capabilities of SQL Data Warehouse.</span></span> <span data-ttu-id="0c230-114">Kontakta hello [enheter (dwu: er) för datalager] [ data warehouse units (DWUs)] sidan toolearn hur dwu: er och prestanda är relaterade.</span><span class="sxs-lookup"><span data-stu-id="0c230-114">Consult hello [data warehouse units (DWUs)][data warehouse units (DWUs)] page toolearn how DWUs and performance are related.</span></span> 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a><span data-ttu-id="0c230-115">Beräkna hur hanteringsåtgärder fungerar i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0c230-115">How compute management operations work in SQL Data Warehouse</span></span>
<span data-ttu-id="0c230-116">hello-arkitekturen för SQL Data Warehouse består av en control-noden, datornoder och hello lagringsskikt sprids 60-distributioner.</span><span class="sxs-lookup"><span data-stu-id="0c230-116">hello architecture for SQL Data Warehouse consists of a control node, compute nodes, and hello storage layer spread across 60 distributions.</span></span> 

<span data-ttu-id="0c230-117">Under normala aktiv session i SQL Data Warehouse systemets huvudnod hanterar hello metadata och innehåller hello distribuerad frågeoptimerare.</span><span class="sxs-lookup"><span data-stu-id="0c230-117">During a normal active session in SQL Data Warehouse, your system's head node manages hello metadata and contains hello distributed query optimizer.</span></span> <span data-ttu-id="0c230-118">Under den här huvudnod är compute-noder och lagring-lagret.</span><span class="sxs-lookup"><span data-stu-id="0c230-118">Beneath this head node are your compute nodes and your storage layer.</span></span> <span data-ttu-id="0c230-119">Systemet har en huvudnod fyra datornoder och hello lagringsskikt som består av 60 distributioner för en DWU 400.</span><span class="sxs-lookup"><span data-stu-id="0c230-119">For a DWU 400, your system has one head node, four compute nodes, and hello storage layer, consisting of 60 distributions.</span></span> 

<span data-ttu-id="0c230-120">När du genomgå en skala eller pausa åtgärden hello systemet först stoppar alla inkommande förfrågningar och sedan återställer transaktioner tooensure ett konsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0c230-120">When you undergo a scale or pause operation, hello system first kills all incoming queries and then rolls back transactions tooensure a consistent state.</span></span> <span data-ttu-id="0c230-121">För skala utförs skalning endast när den här transaktionella återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0c230-121">For scale operations, scaling will only occur once this transactional rollback has completed.</span></span> <span data-ttu-id="0c230-122">En skala upp åtgärd hello system etablerar hello extra önskat antal datornoder och startar sedan återansluta hello noder toohello lagring beräkningslager.</span><span class="sxs-lookup"><span data-stu-id="0c230-122">For a scale-up operation, hello system provisions hello extra desired number of compute nodes, and then begins reattaching hello compute nodes toohello storage layer.</span></span> <span data-ttu-id="0c230-123">Hello onödiga noder släpps och hello återstående datornoderna återansluta själva toohello lämpligt antal distributioner för en skala ned-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0c230-123">For a scale-down operation, hello unneeded nodes are released and hello remaining compute nodes reattach themselves toohello appropriate number of distributions.</span></span> <span data-ttu-id="0c230-124">För en paus åtgärden beräkna alla noder släpps och datorn kommer att göras en mängd olika metadata operations tooleave slutliga systemet i ett stabilt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0c230-124">For a pause operation, all compute nodes are released and your system will undergo a variety of metadata operations tooleave your final system in a stable state.</span></span>

| <span data-ttu-id="0c230-125">DWU</span><span class="sxs-lookup"><span data-stu-id="0c230-125">DWU</span></span>  | <span data-ttu-id="0c230-126">\#med beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="0c230-126">\#of compute nodes</span></span> | <span data-ttu-id="0c230-127">\#för distributioner per nod</span><span class="sxs-lookup"><span data-stu-id="0c230-127">\# of distributions per node</span></span> |
| ---- | ------------------ | ---------------------------- |
| <span data-ttu-id="0c230-128">100</span><span class="sxs-lookup"><span data-stu-id="0c230-128">100</span></span>  | <span data-ttu-id="0c230-129">1</span><span class="sxs-lookup"><span data-stu-id="0c230-129">1</span></span>                  | <span data-ttu-id="0c230-130">60</span><span class="sxs-lookup"><span data-stu-id="0c230-130">60</span></span>                           |
| <span data-ttu-id="0c230-131">200</span><span class="sxs-lookup"><span data-stu-id="0c230-131">200</span></span>  | <span data-ttu-id="0c230-132">2</span><span class="sxs-lookup"><span data-stu-id="0c230-132">2</span></span>                  | <span data-ttu-id="0c230-133">30</span><span class="sxs-lookup"><span data-stu-id="0c230-133">30</span></span>                           |
| <span data-ttu-id="0c230-134">300</span><span class="sxs-lookup"><span data-stu-id="0c230-134">300</span></span>  | <span data-ttu-id="0c230-135">3</span><span class="sxs-lookup"><span data-stu-id="0c230-135">3</span></span>                  | <span data-ttu-id="0c230-136">20</span><span class="sxs-lookup"><span data-stu-id="0c230-136">20</span></span>                           |
| <span data-ttu-id="0c230-137">400</span><span class="sxs-lookup"><span data-stu-id="0c230-137">400</span></span>  | <span data-ttu-id="0c230-138">4</span><span class="sxs-lookup"><span data-stu-id="0c230-138">4</span></span>                  | <span data-ttu-id="0c230-139">15</span><span class="sxs-lookup"><span data-stu-id="0c230-139">15</span></span>                           |
| <span data-ttu-id="0c230-140">500</span><span class="sxs-lookup"><span data-stu-id="0c230-140">500</span></span>  | <span data-ttu-id="0c230-141">5</span><span class="sxs-lookup"><span data-stu-id="0c230-141">5</span></span>                  | <span data-ttu-id="0c230-142">12</span><span class="sxs-lookup"><span data-stu-id="0c230-142">12</span></span>                           |
| <span data-ttu-id="0c230-143">600</span><span class="sxs-lookup"><span data-stu-id="0c230-143">600</span></span>  | <span data-ttu-id="0c230-144">6</span><span class="sxs-lookup"><span data-stu-id="0c230-144">6</span></span>                  | <span data-ttu-id="0c230-145">10</span><span class="sxs-lookup"><span data-stu-id="0c230-145">10</span></span>                           |
| <span data-ttu-id="0c230-146">1000</span><span class="sxs-lookup"><span data-stu-id="0c230-146">1000</span></span> | <span data-ttu-id="0c230-147">10</span><span class="sxs-lookup"><span data-stu-id="0c230-147">10</span></span>                 | <span data-ttu-id="0c230-148">6</span><span class="sxs-lookup"><span data-stu-id="0c230-148">6</span></span>                            |
| <span data-ttu-id="0c230-149">1200</span><span class="sxs-lookup"><span data-stu-id="0c230-149">1200</span></span> | <span data-ttu-id="0c230-150">12</span><span class="sxs-lookup"><span data-stu-id="0c230-150">12</span></span>                 | <span data-ttu-id="0c230-151">5</span><span class="sxs-lookup"><span data-stu-id="0c230-151">5</span></span>                            |
| <span data-ttu-id="0c230-152">1500</span><span class="sxs-lookup"><span data-stu-id="0c230-152">1500</span></span> | <span data-ttu-id="0c230-153">15</span><span class="sxs-lookup"><span data-stu-id="0c230-153">15</span></span>                 | <span data-ttu-id="0c230-154">4</span><span class="sxs-lookup"><span data-stu-id="0c230-154">4</span></span>                            |
| <span data-ttu-id="0c230-155">2000</span><span class="sxs-lookup"><span data-stu-id="0c230-155">2000</span></span> | <span data-ttu-id="0c230-156">20</span><span class="sxs-lookup"><span data-stu-id="0c230-156">20</span></span>                 | <span data-ttu-id="0c230-157">3</span><span class="sxs-lookup"><span data-stu-id="0c230-157">3</span></span>                            |
| <span data-ttu-id="0c230-158">3000</span><span class="sxs-lookup"><span data-stu-id="0c230-158">3000</span></span> | <span data-ttu-id="0c230-159">30</span><span class="sxs-lookup"><span data-stu-id="0c230-159">30</span></span>                 | <span data-ttu-id="0c230-160">2</span><span class="sxs-lookup"><span data-stu-id="0c230-160">2</span></span>                            |
| <span data-ttu-id="0c230-161">6000</span><span class="sxs-lookup"><span data-stu-id="0c230-161">6000</span></span> | <span data-ttu-id="0c230-162">60</span><span class="sxs-lookup"><span data-stu-id="0c230-162">60</span></span>                 | <span data-ttu-id="0c230-163">1</span><span class="sxs-lookup"><span data-stu-id="0c230-163">1</span></span>                            |

<span data-ttu-id="0c230-164">hello tre primära funktioner för hantering av beräkning är:</span><span class="sxs-lookup"><span data-stu-id="0c230-164">hello three primary functions for managing compute are:</span></span>

1. <span data-ttu-id="0c230-165">Pausa</span><span class="sxs-lookup"><span data-stu-id="0c230-165">Pause</span></span>
2. <span data-ttu-id="0c230-166">Återuppta</span><span class="sxs-lookup"><span data-stu-id="0c230-166">Resume</span></span>
3. <span data-ttu-id="0c230-167">Skala</span><span class="sxs-lookup"><span data-stu-id="0c230-167">Scale</span></span>

<span data-ttu-id="0c230-168">Var och en av dessa åtgärder kan ta flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0c230-168">Each of these operations may take several minutes toocomplete.</span></span> <span data-ttu-id="0c230-169">Om du är skalning/pausa/återupptar automatiskt kan kanske du vill tooimplement logik tooensure att vissa åtgärder har slutförts innan du fortsätter med en annan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0c230-169">If you are scaling/pausing/resuming automatically, you may want tooimplement logic tooensure that certain operations have been completed before proceeding with another action.</span></span> 

<span data-ttu-id="0c230-170">Kontrollera hello databastillståndet via olika slutpunkter kan du toocorrectly implementera automatisering av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0c230-170">Checking hello database state through various endpoints will allow you toocorrectly implement automation of such operations.</span></span> <span data-ttu-id="0c230-171">hello portal ger meddelande vid slutförandet av en åtgärd och hello databaser aktuella tillstånd, men tillåter inte programmässiga kontroll av tillståndet.</span><span class="sxs-lookup"><span data-stu-id="0c230-171">hello portal will provide notification upon completion of an operation and hello databases current state but does not allow for programmatic checking of state.</span></span> 

>  [!NOTE]
>
>  <span data-ttu-id="0c230-172">Beräkna hanteringsfunktioner inte finns för alla slutpunkterna.</span><span class="sxs-lookup"><span data-stu-id="0c230-172">Compute management functionality does not exist across all endpoints.</span></span>
>
>  

|              | <span data-ttu-id="0c230-173">Pausa/Fortsätt</span><span class="sxs-lookup"><span data-stu-id="0c230-173">Pause/Resume</span></span> | <span data-ttu-id="0c230-174">Skala</span><span class="sxs-lookup"><span data-stu-id="0c230-174">Scale</span></span> | <span data-ttu-id="0c230-175">Kontrollera databasens status</span><span class="sxs-lookup"><span data-stu-id="0c230-175">Check database state</span></span> |
| ------------ | ------------ | ----- | -------------------- |
| <span data-ttu-id="0c230-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0c230-176">Azure portal</span></span> | <span data-ttu-id="0c230-177">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-177">Yes</span></span>          | <span data-ttu-id="0c230-178">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-178">Yes</span></span>   | <span data-ttu-id="0c230-179">**Nej**</span><span class="sxs-lookup"><span data-stu-id="0c230-179">**No**</span></span>               |
| <span data-ttu-id="0c230-180">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c230-180">PowerShell</span></span>   | <span data-ttu-id="0c230-181">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-181">Yes</span></span>          | <span data-ttu-id="0c230-182">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-182">Yes</span></span>   | <span data-ttu-id="0c230-183">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-183">Yes</span></span>                  |
| <span data-ttu-id="0c230-184">REST API</span><span class="sxs-lookup"><span data-stu-id="0c230-184">REST API</span></span>     | <span data-ttu-id="0c230-185">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-185">Yes</span></span>          | <span data-ttu-id="0c230-186">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-186">Yes</span></span>   | <span data-ttu-id="0c230-187">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-187">Yes</span></span>                  |
| <span data-ttu-id="0c230-188">T-SQL</span><span class="sxs-lookup"><span data-stu-id="0c230-188">T-SQL</span></span>        | <span data-ttu-id="0c230-189">**Nej**</span><span class="sxs-lookup"><span data-stu-id="0c230-189">**No**</span></span>       | <span data-ttu-id="0c230-190">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-190">Yes</span></span>   | <span data-ttu-id="0c230-191">Ja</span><span class="sxs-lookup"><span data-stu-id="0c230-191">Yes</span></span>                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="0c230-192">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="0c230-192">Scale compute</span></span>

<span data-ttu-id="0c230-193">Prestanda i SQL Data Warehouse mäts i [enheter (dwu: er) för datalager] [ data warehouse units (DWUs)] som är en abstracted mått på beräkningsresurser t.ex CPU, minne och i/o-bandbredd.</span><span class="sxs-lookup"><span data-stu-id="0c230-193">Performance in SQL Data Warehouse is measured in [data warehouse units (DWUs)][data warehouse units (DWUs)] which is an abstracted measure of compute resources such as CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="0c230-194">En användare som önskar tooscale sina systemets prestanda kan göra detta på olika sätt som hello portal, T-SQL och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="0c230-194">A user who wishes tooscale their system's performance can do so through various means, such as through hello portal, T-SQL, and REST APIs.</span></span> 

### <a name="how-do-i-scale-compute"></a><span data-ttu-id="0c230-195">Hur skala beräkning?</span><span class="sxs-lookup"><span data-stu-id="0c230-195">How do I scale compute?</span></span>
<span data-ttu-id="0c230-196">Datorkraft hanteras SQL Data Warehouse genom att ändra hello DWU-inställningar.</span><span class="sxs-lookup"><span data-stu-id="0c230-196">Compute power is managed for you SQL Data Warehouse by changing hello DWU setting.</span></span> <span data-ttu-id="0c230-197">Prestandaökningar [linjärt] [ linearly] när du lägger till flera DWU för vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0c230-197">Performance increases [linearly][linearly] as you add more DWU for certain operations.</span></span>  <span data-ttu-id="0c230-198">Vi erbjuder DWU-erbjudanden som säkerställer att prestandan kommer att ändras märkbart när du skalar systemet upp eller ned.</span><span class="sxs-lookup"><span data-stu-id="0c230-198">We offer DWU offerings that ensure that your performance will change noticeably when you scale your system up or down.</span></span> 

<span data-ttu-id="0c230-199">tooadjust dwu: er, du kan använda någon av dessa enskilda metoder.</span><span class="sxs-lookup"><span data-stu-id="0c230-199">tooadjust DWUs, you can use any of these individual methods.</span></span>

* <span data-ttu-id="0c230-200">[Skala datorkraft med Azure-portalen][Scale compute power with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="0c230-200">[Scale compute power with Azure portal][Scale compute power with Azure portal]</span></span>
* <span data-ttu-id="0c230-201">[Skala datorkraft med PowerShell][Scale compute power with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="0c230-201">[Scale compute power with PowerShell][Scale compute power with PowerShell]</span></span>
* <span data-ttu-id="0c230-202">[Skala datorkraft med REST API: er][Scale compute power with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="0c230-202">[Scale compute power with REST APIs][Scale compute power with REST APIs]</span></span>
* <span data-ttu-id="0c230-203">[Skala datorkraft med TSQL][Scale compute power with TSQL]</span><span class="sxs-lookup"><span data-stu-id="0c230-203">[Scale compute power with TSQL][Scale compute power with TSQL]</span></span>

### <a name="how-many-dwus-should-i-use"></a><span data-ttu-id="0c230-204">Hur många dwu: er ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="0c230-204">How many DWUs should I use?</span></span>

<span data-ttu-id="0c230-205">toounderstand vad ditt perfekta DWU-värde är, försök att skala upp och ner och köra några frågor efter att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="0c230-205">toounderstand what your ideal DWU value is, try scaling up and down, and running a few queries after loading your data.</span></span> <span data-ttu-id="0c230-206">Eftersom det går snabbt att skala, kan du försöka olika prestandanivåer i en timme eller mindre.</span><span class="sxs-lookup"><span data-stu-id="0c230-206">Since scaling is quick, you can try various performance levels in an hour or less.</span></span> 

> [!Note] 
> <span data-ttu-id="0c230-207">SQL Data Warehouse är utformad tooprocess stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="0c230-207">SQL Data Warehouse is designed tooprocess large amounts of data.</span></span> <span data-ttu-id="0c230-208">toosee dess true funktioner för skalning, speciellt vid större dwu: er som du vill använda toouse stora datamängder som närmar sig eller större än 1 TB.</span><span class="sxs-lookup"><span data-stu-id="0c230-208">toosee its true capabilities for scaling, especially at larger DWUs, you want toouse a large data set which approaches or exceeds 1 TB.</span></span>

<span data-ttu-id="0c230-209">Rekommendationer för att hitta hello bästa DWU för din arbetsbelastning:</span><span class="sxs-lookup"><span data-stu-id="0c230-209">Recommendations for finding hello best DWU for your workload:</span></span>

1. <span data-ttu-id="0c230-210">Börja genom att välja en mindre DWU prestandanivå för ett datalager under utveckling.</span><span class="sxs-lookup"><span data-stu-id="0c230-210">For a data warehouse in development, begin by selecting a smaller DWU performance level.</span></span>  <span data-ttu-id="0c230-211">En bra utgångspunkt är DW400 eller DW200.</span><span class="sxs-lookup"><span data-stu-id="0c230-211">A good starting point is DW400 or DW200.</span></span>
2. <span data-ttu-id="0c230-212">Övervakare för programmets prestanda, sett hello antalet dwu: er som valts jämfört med toohello prestanda du se.</span><span class="sxs-lookup"><span data-stu-id="0c230-212">Monitor your application performance, observing hello number of DWUs selected compared toohello performance you observe.</span></span>
3. <span data-ttu-id="0c230-213">Avgör hur mycket snabbare eller långsammare prestanda bör vara för du tooreach hello optimala prestandanivån för dina krav genom att överta linjär skala.</span><span class="sxs-lookup"><span data-stu-id="0c230-213">Determine how much faster or slower performance should be for you tooreach hello optimum performance level for your requirements by assuming linear scale.</span></span>
4. <span data-ttu-id="0c230-214">Öka eller minska antalet hello dwu: er i andel toohow mycket snabbare eller långsammare som du vill tooperform din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="0c230-214">Increase or decrease hello number of DWUs in proportion toohow much faster or slower you want your workload tooperform.</span></span> 
5. <span data-ttu-id="0c230-215">Fortsätt att göra justeringar tills du når nivån optimal prestanda för dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="0c230-215">Continue making adjustments until you reach an optimum performance level for your business requirements.</span></span>

> [!NOTE]
>
> <span data-ttu-id="0c230-216">Frågeprestanda ökar bara med flera parallellisering om hello arbete delas mellan beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="0c230-216">Query performance only increases with more parallelization if hello work can be split between compute nodes.</span></span> <span data-ttu-id="0c230-217">Om du upptäcker att skalning inte är ändra din prestanda, finns våra artiklar toocheck om dina data fördelas ojämnt eller om du introducerar en stor mängd data movement för prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="0c230-217">If you find that scaling is not changing your performance, please check out our performance tuning articles toocheck whether your data is unevenly distributed or if you are introducing a large amount of data movement.</span></span> 

### <a name="when-should-i-scale-dwus"></a><span data-ttu-id="0c230-218">När bör jag skala dwu: er?</span><span class="sxs-lookup"><span data-stu-id="0c230-218">When should I scale DWUs?</span></span>
<span data-ttu-id="0c230-219">Skalning dwu: er ändrar hello följande viktiga scenarier:</span><span class="sxs-lookup"><span data-stu-id="0c230-219">Scaling DWUs alters hello following important scenarios:</span></span>

1. <span data-ttu-id="0c230-220">Ändra linjärt prestanda för hello för sökningar, aggregeringar och CTAS uttryck</span><span class="sxs-lookup"><span data-stu-id="0c230-220">Linearly changing performance of hello system for scans, aggregations, and CTAS statements</span></span>
2. <span data-ttu-id="0c230-221">Öka hello antal läsare och skrivare vid inläsning av med PolyBase</span><span class="sxs-lookup"><span data-stu-id="0c230-221">Increasing hello number of readers and writers when loading with PolyBase</span></span>
3. <span data-ttu-id="0c230-222">Maximalt antal samtidiga frågor och samtidighet fack</span><span class="sxs-lookup"><span data-stu-id="0c230-222">Maximum number of concurrent queries and concurrency slots</span></span>

<span data-ttu-id="0c230-223">Rekommendationer för när tooscale dwu: er:</span><span class="sxs-lookup"><span data-stu-id="0c230-223">Recommendations for when tooscale DWUs:</span></span>

1. <span data-ttu-id="0c230-224">Innan du utför en tung datainläsnings- eller omvandlingsåtgärd åtgärd skala upp dwu: er så att dina data blir tillgängliga snabbare.</span><span class="sxs-lookup"><span data-stu-id="0c230-224">Before you perform a heavy data loading or transformation operation, scale up DWUs so that your data is available more quickly.</span></span>
2. <span data-ttu-id="0c230-225">Skala tooaccommodate större antal samtidiga frågor under belastning kontorstid.</span><span class="sxs-lookup"><span data-stu-id="0c230-225">During peak business hours, scale tooaccommodate larger numbers of concurrent queries.</span></span> 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="0c230-226">Pausa beräkning</span><span class="sxs-lookup"><span data-stu-id="0c230-226">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="0c230-227">toopause en databas med någon av metoderna enskilda.</span><span class="sxs-lookup"><span data-stu-id="0c230-227">toopause a database, use any of these individual methods.</span></span>

* <span data-ttu-id="0c230-228">[Pausa beräkning med Azure-portalen][Pause compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="0c230-228">[Pause compute with Azure portal][Pause compute with Azure portal]</span></span>
* <span data-ttu-id="0c230-229">[Pausa beräkning med PowerShell][Pause compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="0c230-229">[Pause compute with PowerShell][Pause compute with PowerShell]</span></span>
* <span data-ttu-id="0c230-230">[Pausa beräkning med REST API: er][Pause compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="0c230-230">[Pause compute with REST APIs][Pause compute with REST APIs]</span></span>

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="0c230-231">Återuppta beräkning</span><span class="sxs-lookup"><span data-stu-id="0c230-231">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="0c230-232">tooresume en databas med någon av metoderna enskilda.</span><span class="sxs-lookup"><span data-stu-id="0c230-232">tooresume a database, use any of these individual methods.</span></span>

* <span data-ttu-id="0c230-233">[Återuppta beräkning med Azure-portalen][Resume compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="0c230-233">[Resume compute with Azure portal][Resume compute with Azure portal]</span></span>
* <span data-ttu-id="0c230-234">[Återuppta beräkning med PowerShell][Resume compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="0c230-234">[Resume compute with PowerShell][Resume compute with PowerShell]</span></span>
* <span data-ttu-id="0c230-235">[Återuppta beräkning med REST API: er][Resume compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="0c230-235">[Resume compute with REST APIs][Resume compute with REST APIs]</span></span>

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="0c230-236">Kontrollera databasens status</span><span class="sxs-lookup"><span data-stu-id="0c230-236">Check database state</span></span> 

<span data-ttu-id="0c230-237">tooresume en databas med någon av metoderna enskilda.</span><span class="sxs-lookup"><span data-stu-id="0c230-237">tooresume a database, use any of these individual methods.</span></span>

- <span data-ttu-id="0c230-238">[Kontrollera databasens status med T-SQL][Check database state with T-SQL]</span><span class="sxs-lookup"><span data-stu-id="0c230-238">[Check database state with T-SQL][Check database state with T-SQL]</span></span>
- <span data-ttu-id="0c230-239">[Kontrollera databasens status med PowerShell][Check database state with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="0c230-239">[Check database state with PowerShell][Check database state with PowerShell]</span></span>
- <span data-ttu-id="0c230-240">[Kontrollera databasens status med REST API: er][Check database state with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="0c230-240">[Check database state with REST APIs][Check database state with REST APIs]</span></span>

## <a name="permissions"></a><span data-ttu-id="0c230-241">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="0c230-241">Permissions</span></span>

<span data-ttu-id="0c230-242">Skalning hello-databasen kräver hello behörigheterna som beskrivs i [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="0c230-242">Scaling hello database requires hello permissions described in [ALTER DATABASE][ALTER DATABASE].</span></span>  <span data-ttu-id="0c230-243">Pausa och återuppta kräver hello [SQL DB-deltagare] [ SQL DB Contributor] behörighet, särskilt Microsoft.Sql/servers/databases/action.</span><span class="sxs-lookup"><span data-stu-id="0c230-243">Pause and Resume require hello [SQL DB Contributor][SQL DB Contributor] permission, specifically Microsoft.Sql/servers/databases/action.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="0c230-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c230-244">Next steps</span></span>
<span data-ttu-id="0c230-245">Se följande artiklar toohelp du känner till vissa ytterligare KPI begreppen toohello:</span><span class="sxs-lookup"><span data-stu-id="0c230-245">Refer toohello following articles toohelp you understand some additional key performance concepts:</span></span>

* <span data-ttu-id="0c230-246">[Hantering av arbetsbelastning och samtidighet][Workload and concurrency management]</span><span class="sxs-lookup"><span data-stu-id="0c230-246">[Workload and concurrency management][Workload and concurrency management]</span></span>
* <span data-ttu-id="0c230-247">[Översikt över tabellen design][Table design overview]</span><span class="sxs-lookup"><span data-stu-id="0c230-247">[Table design overview][Table design overview]</span></span>
* <span data-ttu-id="0c230-248">[Tabell-distribution][Table distribution]</span><span class="sxs-lookup"><span data-stu-id="0c230-248">[Table distribution][Table distribution]</span></span>
* <span data-ttu-id="0c230-249">[Tabell indexering][Table indexing]</span><span class="sxs-lookup"><span data-stu-id="0c230-249">[Table indexing][Table indexing]</span></span>
* <span data-ttu-id="0c230-250">[Tabellpartitionering][Table partitioning]</span><span class="sxs-lookup"><span data-stu-id="0c230-250">[Table partitioning][Table partitioning]</span></span>
* <span data-ttu-id="0c230-251">[Tabellstatistik][Table statistics]</span><span class="sxs-lookup"><span data-stu-id="0c230-251">[Table statistics][Table statistics]</span></span>
* <span data-ttu-id="0c230-252">[Bästa praxis][Best practices]</span><span class="sxs-lookup"><span data-stu-id="0c230-252">[Best practices][Best practices]</span></span>

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
