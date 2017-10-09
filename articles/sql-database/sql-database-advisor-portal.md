---
title: "rekommendationer för aaaApply - Azure SQL Database | Microsoft Docs"
description: "Du kan använda hello Azure portal toofind rekommendationer som kan optimera prestanda för din Azure SQL Database eller toocorrect vissa problem som identifieras i din arbetsbelastning."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="2bb57-103">Hitta och tillämpa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2bb57-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="2bb57-104">Du kan använda hello Azure portal toofind rekommendationer som kan optimera prestanda för din Azure SQL Database eller toocorrect vissa problem som identifieras i din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="2bb57-104">You can use hello Azure portal toofind performance recommendations that can optimize performance of your Azure SQL Database or toocorrect some issue identified in your workload.</span></span> <span data-ttu-id="2bb57-105">**Prestanda rekommendation** sidan i Azure-portalen kan du toofind hello översta rekommendationer baserat på deras eventuella inverkan.</span><span class="sxs-lookup"><span data-stu-id="2bb57-105">**Performance recommendation** page in Azure portal enables you toofind hello top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="2bb57-106">Visa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2bb57-106">Viewing recommendations</span></span>

<span data-ttu-id="2bb57-107">tooview och tillämpa rekommendationer, behöver du hello rätt [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="2bb57-107">tooview and apply performance recommendations, you need hello correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="2bb57-108">**Läsaren**, **SQL DB-deltagare** behörigheter som är nödvändiga tooview rekommendationer och **ägare**, **SQL DB-deltagare** behörigheter som krävs tooexecute alla åtgärder. Skapa eller drop index och avbryta skapandet av index.</span><span class="sxs-lookup"><span data-stu-id="2bb57-108">**Reader**, **SQL DB Contributor** permissions are required tooview recommendations, and **Owner**, **SQL DB Contributor** permissions are required tooexecute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="2bb57-109">Använd hello följa steg toofind rekommendationer på Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="2bb57-109">Use hello following steps toofind performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="2bb57-110">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2bb57-110">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2bb57-111">Gå för**fler tjänster** > **SQL-databaser**, och välj din databas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-111">Go too**More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="2bb57-112">Navigera för**prestanda rekommendation** tooview tillgängliga rekommendationer för hello valda databasen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-112">Navigate too**Performance recommendation** tooview available recommendations for hello selected database.</span></span>

<span data-ttu-id="2bb57-113">Rekommendationer är shonw hello tabell liknande toohello som visas på följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="2bb57-113">Performance recommendations are shonw in hello table similar toohello one shown on hello following figure:</span></span>

![Rekommendationer](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="2bb57-115">Rekommendationer sorteras efter deras eventuella inverkan på prestanda i hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="2bb57-115">Recommendations are sorted by their potential impact on performance into hello following categories:</span></span>

| <span data-ttu-id="2bb57-116">Påverkan</span><span class="sxs-lookup"><span data-stu-id="2bb57-116">Impact</span></span> | <span data-ttu-id="2bb57-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2bb57-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2bb57-118">Hög</span><span class="sxs-lookup"><span data-stu-id="2bb57-118">High</span></span> |<span data-ttu-id="2bb57-119">Hög inverkan rekommendationer bör ge hello mest betydande inverkan på prestanda.</span><span class="sxs-lookup"><span data-stu-id="2bb57-119">High impact recommendations should provide hello most significant performance impact.</span></span> |
| <span data-ttu-id="2bb57-120">Medel</span><span class="sxs-lookup"><span data-stu-id="2bb57-120">Medium</span></span> |<span data-ttu-id="2bb57-121">Medelhög påverkan rekommendationer bör förbättra prestanda, men inte avsevärt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="2bb57-122">Låg</span><span class="sxs-lookup"><span data-stu-id="2bb57-122">Low</span></span> |<span data-ttu-id="2bb57-123">Låg påverkan rekommendationer bör ge bättre prestanda än utan, men kan inte vara betydande förbättringar.</span><span class="sxs-lookup"><span data-stu-id="2bb57-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="2bb57-124">Azure SQL Database måste toomonitor aktiviteter minst en dag i ordning tooidentify några rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2bb57-124">Azure SQL Database needs toomonitor activities at least for a day in order tooidentify some recommendations.</span></span> <span data-ttu-id="2bb57-125">hello Azure SQL Database kan enklare att optimera för konsekvent frågemönster än den för slumpmässiga ojämn belastning för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2bb57-125">hello Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="2bb57-126">Om rekommendationerna inte är tillgängliga corrently, hello **prestanda rekommendation** sidan visar ett meddelande som förklarar varför.</span><span class="sxs-lookup"><span data-stu-id="2bb57-126">If recommendations are not corrently available, hello **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="2bb57-127">Du kan också visa hello status för hello tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2bb57-127">You can also view hello status of hello historical operations.</span></span> <span data-ttu-id="2bb57-128">Välj en rekommendation eller status toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="2bb57-128">Select a recommendation or status toosee  more details.</span></span>

<span data-ttu-id="2bb57-129">Här är ett exempel på ”Skapa indexet” rekommendation i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-129">Here is an example of "Create index" recommendation in hello Azure portal.</span></span>

![Skapa index](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="2bb57-131">Tillämpa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2bb57-131">Applying recommendations</span></span>
<span data-ttu-id="2bb57-132">Azure SQL Database ger dig fullständig kontroll över hur rekommendationerna är aktiverad med någon av följande tre alternativ hello:</span><span class="sxs-lookup"><span data-stu-id="2bb57-132">Azure SQL Database gives you full control over how recommendations are enabled using any of hello following three options:</span></span> 

* <span data-ttu-id="2bb57-133">Använda enskilda rekommendationer en i taget.</span><span class="sxs-lookup"><span data-stu-id="2bb57-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="2bb57-134">Aktivera hello automatisk justering tooautomatically gäller rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2bb57-134">Enable hello Automatic tuning tooautomatically apply recommendations.</span></span>
* <span data-ttu-id="2bb57-135">tooimplement en rekommendation köra manuellt, hello rekommenderas T-SQL-skript mot databasen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-135">tooimplement a recommendation manually, run hello recommended T-SQL script against your database.</span></span>

<span data-ttu-id="2bb57-136">Markera varje rekommendation tooview information och klicka på **Visa skript** tooreview hello mer information om hur hello rekommendation skapas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-136">Select any recommendation tooview its details and then click **View script** tooreview hello exact details of how hello recommendation is created.</span></span>

<span data-ttu-id="2bb57-137">hello databasen förblir online medan hello rekommendation används – en databas med hjälp av prestanda rekommendation eller automatisk justering aldrig tar offline.</span><span class="sxs-lookup"><span data-stu-id="2bb57-137">hello database remains online while hello recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="2bb57-138">Använda en enskild rekommendation</span><span class="sxs-lookup"><span data-stu-id="2bb57-138">Apply an individual recommendation</span></span>
<span data-ttu-id="2bb57-139">Du kan granska och Godkänn rekommendationer en i taget.</span><span class="sxs-lookup"><span data-stu-id="2bb57-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="2bb57-140">På hello **rekommendationer** bladet Välj en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="2bb57-140">On hello **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="2bb57-141">På hello **information** bladet och klickar på **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-141">On hello **Details** blade click **Apply** button.</span></span>
   
    ![Tillämpa rekommendation](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="2bb57-143">Valda rekommendation tillämpas på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-143">Selected recommendation will be applied on hello database.</span></span>

### <a name="removing-recommendations-from-hello-list"></a><span data-ttu-id="2bb57-144">Ta bort rekommendationer hello listan</span><span class="sxs-lookup"><span data-stu-id="2bb57-144">Removing recommendations from hello list</span></span>
<span data-ttu-id="2bb57-145">Om din lista över rekommendationer innehåller objekt som du vill tooremove hello listan kan du ta bort hello rekommendation:</span><span class="sxs-lookup"><span data-stu-id="2bb57-145">If your list of recommendations contains items that you want tooremove from hello list, you can discard hello recommendation:</span></span>

1. <span data-ttu-id="2bb57-146">Välj en rekommendation i hello lista över **rekommendationer** tooopen hello information.</span><span class="sxs-lookup"><span data-stu-id="2bb57-146">Select a recommendation in hello list of **Recommendations** tooopen hello details.</span></span>
2. <span data-ttu-id="2bb57-147">Klicka på **Ignorera** på hello **information** bladet.</span><span class="sxs-lookup"><span data-stu-id="2bb57-147">Click **Discard** on hello **Details** blade.</span></span>

<span data-ttu-id="2bb57-148">Om du vill kan du lägga till borttagna objekten tillbaka toohello **rekommendationer** lista:</span><span class="sxs-lookup"><span data-stu-id="2bb57-148">If desired, you can add discarded items back toohello **Recommendations** list:</span></span>

1. <span data-ttu-id="2bb57-149">På hello **rekommendationer** bladet och klickar på **Visa ignorerade**.</span><span class="sxs-lookup"><span data-stu-id="2bb57-149">On hello **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="2bb57-150">Välj en borttagna objekt från hello listan tooview information.</span><span class="sxs-lookup"><span data-stu-id="2bb57-150">Select a discarded item from hello list tooview its details.</span></span>
3. <span data-ttu-id="2bb57-151">Du kan också klicka på **Ångra Ignorera** tooadd hello index tillbaka toohello huvudsakliga lista över **rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="2bb57-151">Optionally, click **Undo Discard** tooadd hello index back toohello main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="2bb57-152">Aktivera automatisk inställning</span><span class="sxs-lookup"><span data-stu-id="2bb57-152">Enable automatic tuning</span></span>
<span data-ttu-id="2bb57-153">Du kan ange hello Azure SQL Database tooimplement rekommendationer automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-153">You can set hello Azure SQL Database tooimplement recommendations automatically.</span></span> <span data-ttu-id="2bb57-154">Rekommendationer blir tillgängliga tillämpas de automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="2bb57-155">Som med alla rekommendationer som hanteras av hello kommer service om hello prestanda påverkas negativt hello rekommendation att återställas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-155">As with all recommendations managed by hello service if hello performance impact is negative hello recommendation will be reverted.</span></span>

1. <span data-ttu-id="2bb57-156">På hello **rekommendationer** bladet, klickar du på **automatisera**:</span><span class="sxs-lookup"><span data-stu-id="2bb57-156">On hello **Recommendations** blade, click **Automate**:</span></span>
   
    ![Inställningar för klassificering](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="2bb57-158">Välj åtgärder tooautomate:</span><span class="sxs-lookup"><span data-stu-id="2bb57-158">Select actions tooautomate:</span></span>
   
    ![Rekommenderat index](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a><span data-ttu-id="2bb57-160">Köra manuellt hello rekommenderas T-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="2bb57-160">Manually run hello recommended T-SQL script</span></span>
<span data-ttu-id="2bb57-161">Markera varje rekommendation och klicka på **Visa skript**.</span><span class="sxs-lookup"><span data-stu-id="2bb57-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="2bb57-162">Köra detta skript mot databasen toomanually gäller hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="2bb57-162">Run this script against your database toomanually apply hello recommendation.</span></span>

<span data-ttu-id="2bb57-163">*Index som utförs manuellt inte övervakas och valideras för prestandapåverkan av hello tjänsten* så rekommenderas du övervaka dessa index efter skapa tooverify de ger prestandavinster och justera eller ta bort dem om nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="2bb57-163">*Indexes that are manually executed are not monitored and validated for performance impact by hello service* so it is suggested that you monitor these indexes after creation tooverify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="2bb57-164">Mer information om hur du skapar index finns [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bb57-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="2bb57-165">Om du avbryter rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2bb57-165">Canceling recommendations</span></span>
<span data-ttu-id="2bb57-166">Rekommendationer som finns i en **väntande**, **verifiera**, eller **lyckade** status kan avbrytas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="2bb57-167">Rekommendationer med statusen **Executing** kan inte avbrytas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="2bb57-168">Välj en rekommendation i hello **justera historik** område tooopen hello **rekommendationsdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="2bb57-168">Select a recommendation in hello **Tuning History** area tooopen hello **recommendations details** blade.</span></span>
2. <span data-ttu-id="2bb57-169">Klicka på **Avbryt** tooabort hello process för tillämpning av hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="2bb57-169">Click **Cancel** tooabort hello process of applying hello recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="2bb57-170">Övervakningsåtgärder</span><span class="sxs-lookup"><span data-stu-id="2bb57-170">Monitoring operations</span></span>
<span data-ttu-id="2bb57-171">Tillämpa en rekommendation kanske inte omedelbart sker.</span><span class="sxs-lookup"><span data-stu-id="2bb57-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="2bb57-172">hello portal innehåller information om hello status för rekommendation.</span><span class="sxs-lookup"><span data-stu-id="2bb57-172">hello portal provides details regarding hello status of recommendation.</span></span> <span data-ttu-id="2bb57-173">hello följande är möjliga tillstånd att ett index i:</span><span class="sxs-lookup"><span data-stu-id="2bb57-173">hello following are possible states that an index can be in:</span></span>

| <span data-ttu-id="2bb57-174">Status</span><span class="sxs-lookup"><span data-stu-id="2bb57-174">Status</span></span> | <span data-ttu-id="2bb57-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2bb57-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2bb57-176">Väntande åtgärder</span><span class="sxs-lookup"><span data-stu-id="2bb57-176">Pending</span></span> |<span data-ttu-id="2bb57-177">Tillämpa rekommendation kommandot har tagits emot och har schemalagts för körning.</span><span class="sxs-lookup"><span data-stu-id="2bb57-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="2bb57-178">Köra</span><span class="sxs-lookup"><span data-stu-id="2bb57-178">Executing</span></span> |<span data-ttu-id="2bb57-179">hello rekommendation tillämpas.</span><span class="sxs-lookup"><span data-stu-id="2bb57-179">hello recommendation is being applied.</span></span> |
| <span data-ttu-id="2bb57-180">Verifiera</span><span class="sxs-lookup"><span data-stu-id="2bb57-180">Verifying</span></span> |<span data-ttu-id="2bb57-181">Rekommendation har tillämpats och hello tjänst mäter hello fördelar.</span><span class="sxs-lookup"><span data-stu-id="2bb57-181">Recommendation was successfully applied and hello service is measuring hello benefits.</span></span> |
| <span data-ttu-id="2bb57-182">Lyckades</span><span class="sxs-lookup"><span data-stu-id="2bb57-182">Success</span></span> |<span data-ttu-id="2bb57-183">Rekommendation har tillämpats och fördelar har tagits mäts.</span><span class="sxs-lookup"><span data-stu-id="2bb57-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="2bb57-184">Fel</span><span class="sxs-lookup"><span data-stu-id="2bb57-184">Error</span></span> |<span data-ttu-id="2bb57-185">Ett fel uppstod under hello process för tillämpning av hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="2bb57-185">An error occurred during hello process of applying hello recommendation.</span></span> <span data-ttu-id="2bb57-186">Detta kan vara ett övergående problem eller eventuellt ett schema ändra toohello tabell och hello skript är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="2bb57-186">This can be a transient issue, or possibly a schema change toohello table and hello script is no longer valid.</span></span> |
| <span data-ttu-id="2bb57-187">Återställa</span><span class="sxs-lookup"><span data-stu-id="2bb57-187">Reverting</span></span> |<span data-ttu-id="2bb57-188">hello rekommendation tillämpades, men har bedömts vara icke-performant och återställs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-188">hello recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="2bb57-189">Har återställts</span><span class="sxs-lookup"><span data-stu-id="2bb57-189">Reverted</span></span> |<span data-ttu-id="2bb57-190">hello rekommendation återställdes.</span><span class="sxs-lookup"><span data-stu-id="2bb57-190">hello recommendation was reverted.</span></span> |

<span data-ttu-id="2bb57-191">Klicka på ett pågående rekommendation från hello listan toosee mer information:</span><span class="sxs-lookup"><span data-stu-id="2bb57-191">Click an in-process recommendation from hello list toosee more details:</span></span>

![Rekommenderat index](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="2bb57-193">Återställa en rekommendation</span><span class="sxs-lookup"><span data-stu-id="2bb57-193">Reverting a recommendation</span></span>
<span data-ttu-id="2bb57-194">Om du använde hello prestanda rekommendationer tooapply hello rekommendation återställs (vilket innebär att du inte manuellt köra hello T-SQL-skript) den automatiskt den om den hittar hello prestanda påverkan toobe negativt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-194">If you used hello performance recommendations tooapply hello recommendation (meaning you did not manually run hello T-SQL script) it will automatically revert it if it finds hello performance impact toobe negative.</span></span> <span data-ttu-id="2bb57-195">Om av någon anledning som du bara vill toorevert en rekommendation kan du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="2bb57-195">If for any reason you simply want toorevert a recommendation you can do hello following:</span></span>

1. <span data-ttu-id="2bb57-196">Välj en har tillämpade rekommendation i hello **justera historik** område.</span><span class="sxs-lookup"><span data-stu-id="2bb57-196">Select a successfully applied recommendation in hello **Tuning history** area.</span></span>
2. <span data-ttu-id="2bb57-197">Klicka på **Återställ** på hello **rekommendationsdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="2bb57-197">Click **Revert** on hello **recommendation details** blade.</span></span>

![Rekommenderat index](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="2bb57-199">Övervaka prestandapåverkan index-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2bb57-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="2bb57-200">När rekommendationerna har implementerats (för närvarande, index-åtgärder och parameterstyra frågor rekommendationer endast) kan du klicka på **frågan insikter** på hello rekommendation information bladet tooopen [fråga Insikter i frågeprestanda](sql-database-query-performance.md) och se hello prestandapåverkan top-frågor.</span><span class="sxs-lookup"><span data-stu-id="2bb57-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on hello recommendation details blade tooopen [Query Performance Insights](sql-database-query-performance.md) and see hello performance impact of your top queries.</span></span>

![Övervakaren prestandapåverkan](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="2bb57-202">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2bb57-202">Summary</span></span>
<span data-ttu-id="2bb57-203">Azure SQL-databasen innehåller rekommendationer för att förbättra prestanda för SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="2bb57-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="2bb57-204">Du får en bra hjälp med att optimera din databas och slutligen förbättra frågeprestanda genom att tillhandahålla T-SQL-skript, samt individuella och fullständigt-automatisk.</span><span class="sxs-lookup"><span data-stu-id="2bb57-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bb57-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bb57-205">Next steps</span></span>
<span data-ttu-id="2bb57-206">Övervaka dina rekommendationer och fortsätta tooapply dem toorefine prestanda.</span><span class="sxs-lookup"><span data-stu-id="2bb57-206">Monitor your recommendations and continue tooapply them toorefine performance.</span></span> <span data-ttu-id="2bb57-207">Databasarbetsbelastningar är dynamiska och ändra kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="2bb57-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="2bb57-208">Azure SQL Database fortsätter toomonitor och ger rekommendationer som kan förbättra dina databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="2bb57-208">Azure SQL Database will continue toomonitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="2bb57-209">Se [automatisk justering](sql-database-automatic-tuning.md) toolearn mer om hello automatisk justering i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2bb57-209">See [Automatic tuning](sql-database-automatic-tuning.md) toolearn more about hello automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="2bb57-210">Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2bb57-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="2bb57-211">Se [insikter i frågeprestanda](sql-database-query-performance.md) toolearn om hur du visar hello prestandapåverkan top-frågor.</span><span class="sxs-lookup"><span data-stu-id="2bb57-211">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bb57-212">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2bb57-212">Additional resources</span></span>
* [<span data-ttu-id="2bb57-213">Query Store</span><span class="sxs-lookup"><span data-stu-id="2bb57-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="2bb57-214">SKAPA INDEX</span><span class="sxs-lookup"><span data-stu-id="2bb57-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="2bb57-215">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="2bb57-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

