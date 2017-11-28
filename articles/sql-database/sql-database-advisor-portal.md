---
title: "Tillämpa rekommendationer - Azure SQL Database | Microsoft Docs"
description: "Du kan använda Azure-portalen för att hitta rekommendationer som kan optimera prestanda för din Azure SQL Database eller åtgärda vissa problem som identifieras i din arbetsbelastning."
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
ms.openlocfilehash: 018afaa8b08bd001e55693390e80c8e2c4f33a30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="be60c-103">Hitta och tillämpa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be60c-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="be60c-104">Du kan använda Azure-portalen för att hitta rekommendationer som kan optimera prestanda för din Azure SQL Database eller åtgärda vissa problem som identifieras i din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="be60c-104">You can use the Azure portal to find performance recommendations that can optimize performance of your Azure SQL Database or to correct some issue identified in your workload.</span></span> <span data-ttu-id="be60c-105">**Prestanda rekommendation** sidan i Azure-portalen kan du hitta översta rekommendationer baserat på deras eventuella inverkan.</span><span class="sxs-lookup"><span data-stu-id="be60c-105">**Performance recommendation** page in Azure portal enables you to find the top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="be60c-106">Visa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be60c-106">Viewing recommendations</span></span>

<span data-ttu-id="be60c-107">Om du vill visa och använda rekommendationer, behöver du rätt [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="be60c-107">To view and apply performance recommendations, you need the correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="be60c-108">**Läsaren**, **SQL DB-deltagare** behörigheter som krävs för att visa rekommendationer och **ägare**, **SQL DB-deltagare** behörigheter som krävs för att utföra några åtgärder; Skapa eller drop index och avbryta skapandet av index.</span><span class="sxs-lookup"><span data-stu-id="be60c-108">**Reader**, **SQL DB Contributor** permissions are required to view recommendations, and **Owner**, **SQL DB Contributor** permissions are required to execute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="be60c-109">Använd följande steg för att hitta rekommendationer på Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="be60c-109">Use the following steps to find performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="be60c-110">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="be60c-110">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="be60c-111">Gå till **fler tjänster** > **SQL-databaser**, och välj din databas.</span><span class="sxs-lookup"><span data-stu-id="be60c-111">Go to **More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="be60c-112">Gå till **prestanda rekommendation** att visa tillgängliga rekommendationer för den valda databasen.</span><span class="sxs-lookup"><span data-stu-id="be60c-112">Navigate to **Performance recommendation** to view available recommendations for the selected database.</span></span>

<span data-ttu-id="be60c-113">Rekommendationer är shonw i tabellen som liknar det som visas på följande bild:</span><span class="sxs-lookup"><span data-stu-id="be60c-113">Performance recommendations are shonw in the table similar to the one shown on the following figure:</span></span>

![Rekommendationer](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="be60c-115">Rekommendationer sorteras efter deras eventuella inverkan på prestanda i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="be60c-115">Recommendations are sorted by their potential impact on performance into the following categories:</span></span>

| <span data-ttu-id="be60c-116">Påverkan</span><span class="sxs-lookup"><span data-stu-id="be60c-116">Impact</span></span> | <span data-ttu-id="be60c-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="be60c-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="be60c-118">Hög</span><span class="sxs-lookup"><span data-stu-id="be60c-118">High</span></span> |<span data-ttu-id="be60c-119">Hög inverkan rekommendationer bör ge viktigaste prestandapåverkan.</span><span class="sxs-lookup"><span data-stu-id="be60c-119">High impact recommendations should provide the most significant performance impact.</span></span> |
| <span data-ttu-id="be60c-120">Medel</span><span class="sxs-lookup"><span data-stu-id="be60c-120">Medium</span></span> |<span data-ttu-id="be60c-121">Medelhög påverkan rekommendationer bör förbättra prestanda, men inte avsevärt.</span><span class="sxs-lookup"><span data-stu-id="be60c-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="be60c-122">Låg</span><span class="sxs-lookup"><span data-stu-id="be60c-122">Low</span></span> |<span data-ttu-id="be60c-123">Låg påverkan rekommendationer bör ge bättre prestanda än utan, men kan inte vara betydande förbättringar.</span><span class="sxs-lookup"><span data-stu-id="be60c-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="be60c-124">Azure SQL Database måste att övervaka aktiviteter minst en dag för att identifiera några rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be60c-124">Azure SQL Database needs to monitor activities at least for a day in order to identify some recommendations.</span></span> <span data-ttu-id="be60c-125">Azure SQL-databasen kan enklare att optimera för konsekvent frågemönster än den för slumpmässiga ojämn belastning för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="be60c-125">The Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="be60c-126">Om rekommendationerna inte är tillgängliga, corrently den **prestanda rekommendation** sidan visar ett meddelande som förklarar varför.</span><span class="sxs-lookup"><span data-stu-id="be60c-126">If recommendations are not corrently available, the **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="be60c-127">Du kan också visa status för tidigare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="be60c-127">You can also view the status of the historical operations.</span></span> <span data-ttu-id="be60c-128">Välj en rekommendation eller status för att se mer information.</span><span class="sxs-lookup"><span data-stu-id="be60c-128">Select a recommendation or status to see  more details.</span></span>

<span data-ttu-id="be60c-129">Här är ett exempel på ”Skapa indexet” rekommendation i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be60c-129">Here is an example of "Create index" recommendation in the Azure portal.</span></span>

![Skapa index](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="be60c-131">Tillämpa rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be60c-131">Applying recommendations</span></span>
<span data-ttu-id="be60c-132">Azure SQL Database ger dig fullständig kontroll över hur rekommendationerna är aktiverad med någon av följande tre alternativ:</span><span class="sxs-lookup"><span data-stu-id="be60c-132">Azure SQL Database gives you full control over how recommendations are enabled using any of the following three options:</span></span> 

* <span data-ttu-id="be60c-133">Använda enskilda rekommendationer en i taget.</span><span class="sxs-lookup"><span data-stu-id="be60c-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="be60c-134">Aktivera automatisk inställning för att automatiskt använda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be60c-134">Enable the Automatic tuning to automatically apply recommendations.</span></span>
* <span data-ttu-id="be60c-135">För att implementera en rekommendation manuellt, kör du rekommenderade T-SQL-skript mot databasen.</span><span class="sxs-lookup"><span data-stu-id="be60c-135">To implement a recommendation manually, run the recommended T-SQL script against your database.</span></span>

<span data-ttu-id="be60c-136">Välj en rekommendation att visa information och klickar sedan på **Visa skript** att granska exakt information om hur rekommendationen skapas.</span><span class="sxs-lookup"><span data-stu-id="be60c-136">Select any recommendation to view its details and then click **View script** to review the exact details of how the recommendation is created.</span></span>

<span data-ttu-id="be60c-137">Databasen är online samtidigt rekommendationen tillämpas--med hjälp av prestanda rekommendation eller automatisk justering aldrig tar en databas offline.</span><span class="sxs-lookup"><span data-stu-id="be60c-137">The database remains online while the recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="be60c-138">Använda en enskild rekommendation</span><span class="sxs-lookup"><span data-stu-id="be60c-138">Apply an individual recommendation</span></span>
<span data-ttu-id="be60c-139">Du kan granska och Godkänn rekommendationer en i taget.</span><span class="sxs-lookup"><span data-stu-id="be60c-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="be60c-140">På den **rekommendationer** bladet Välj en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="be60c-140">On the **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="be60c-141">På den **information** bladet och klickar på **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="be60c-141">On the **Details** blade click **Apply** button.</span></span>
   
    ![Tillämpa rekommendation](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="be60c-143">Valda rekommendation tillämpas på databasen.</span><span class="sxs-lookup"><span data-stu-id="be60c-143">Selected recommendation will be applied on the database.</span></span>

### <a name="removing-recommendations-from-the-list"></a><span data-ttu-id="be60c-144">Tar bort rekommendationer från listan</span><span class="sxs-lookup"><span data-stu-id="be60c-144">Removing recommendations from the list</span></span>
<span data-ttu-id="be60c-145">Om din lista över rekommendationer innehåller objekt som du vill ta bort från listan kan du ta bort rekommendationen:</span><span class="sxs-lookup"><span data-stu-id="be60c-145">If your list of recommendations contains items that you want to remove from the list, you can discard the recommendation:</span></span>

1. <span data-ttu-id="be60c-146">Välj en rekommendation i listan över **rekommendationer** att öppna information.</span><span class="sxs-lookup"><span data-stu-id="be60c-146">Select a recommendation in the list of **Recommendations** to open the details.</span></span>
2. <span data-ttu-id="be60c-147">Klicka på **Ignorera** på den **information** bladet.</span><span class="sxs-lookup"><span data-stu-id="be60c-147">Click **Discard** on the **Details** blade.</span></span>

<span data-ttu-id="be60c-148">Om du vill kan du lägga till borttagna objekten tillbaka till den **rekommendationer** lista:</span><span class="sxs-lookup"><span data-stu-id="be60c-148">If desired, you can add discarded items back to the **Recommendations** list:</span></span>

1. <span data-ttu-id="be60c-149">På den **rekommendationer** bladet och klickar på **Visa ignorerade**.</span><span class="sxs-lookup"><span data-stu-id="be60c-149">On the **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="be60c-150">Välj en borttagna objekt från listan för att visa information om den.</span><span class="sxs-lookup"><span data-stu-id="be60c-150">Select a discarded item from the list to view its details.</span></span>
3. <span data-ttu-id="be60c-151">Du kan också klicka på **Ångra Ignorera** att lägga till indexet huvudsakliga listan över **rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="be60c-151">Optionally, click **Undo Discard** to add the index back to the main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="be60c-152">Aktivera automatisk inställning</span><span class="sxs-lookup"><span data-stu-id="be60c-152">Enable automatic tuning</span></span>
<span data-ttu-id="be60c-153">Du kan ställa in Azure SQL-databasen för att implementera rekommendationer automatiskt.</span><span class="sxs-lookup"><span data-stu-id="be60c-153">You can set the Azure SQL Database to implement recommendations automatically.</span></span> <span data-ttu-id="be60c-154">Rekommendationer blir tillgängliga tillämpas de automatiskt.</span><span class="sxs-lookup"><span data-stu-id="be60c-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="be60c-155">Som med alla rekommendationer som hanteras av tjänsten om prestandapåverkan är negativt kommer rekommendationen att återställas.</span><span class="sxs-lookup"><span data-stu-id="be60c-155">As with all recommendations managed by the service if the performance impact is negative the recommendation will be reverted.</span></span>

1. <span data-ttu-id="be60c-156">På den **rekommendationer** bladet, klickar du på **automatisera**:</span><span class="sxs-lookup"><span data-stu-id="be60c-156">On the **Recommendations** blade, click **Automate**:</span></span>
   
    ![Inställningar för klassificering](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="be60c-158">Välj åtgärder för att automatisera:</span><span class="sxs-lookup"><span data-stu-id="be60c-158">Select actions to automate:</span></span>
   
    ![Rekommenderat index](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a><span data-ttu-id="be60c-160">Manuellt köra rekommenderade T-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="be60c-160">Manually run the recommended T-SQL script</span></span>
<span data-ttu-id="be60c-161">Markera varje rekommendation och klicka på **Visa skript**.</span><span class="sxs-lookup"><span data-stu-id="be60c-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="be60c-162">Köra detta skript mot databasen för att använda rekommendationen manuellt.</span><span class="sxs-lookup"><span data-stu-id="be60c-162">Run this script against your database to manually apply the recommendation.</span></span>

<span data-ttu-id="be60c-163">*Index som utförs manuellt inte övervakas och valideras för prestandapåverkan av tjänsten* så vi rekommenderar att du övervaka dessa index när de har skapats för att verifiera de ger prestandavinster och justera eller ta bort dem om det behövs.</span><span class="sxs-lookup"><span data-stu-id="be60c-163">*Indexes that are manually executed are not monitored and validated for performance impact by the service* so it is suggested that you monitor these indexes after creation to verify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="be60c-164">Mer information om hur du skapar index finns [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="be60c-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="be60c-165">Om du avbryter rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be60c-165">Canceling recommendations</span></span>
<span data-ttu-id="be60c-166">Rekommendationer som finns i en **väntande**, **verifiera**, eller **lyckade** status kan avbrytas.</span><span class="sxs-lookup"><span data-stu-id="be60c-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="be60c-167">Rekommendationer med statusen **Executing** kan inte avbrytas.</span><span class="sxs-lookup"><span data-stu-id="be60c-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="be60c-168">Välj en rekommendation i den **justera historik** område för att öppna den **rekommendationsdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="be60c-168">Select a recommendation in the **Tuning History** area to open the **recommendations details** blade.</span></span>
2. <span data-ttu-id="be60c-169">Klicka på **Avbryt** att avbryta en process för tillämpning rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="be60c-169">Click **Cancel** to abort the process of applying the recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="be60c-170">Övervakningsåtgärder</span><span class="sxs-lookup"><span data-stu-id="be60c-170">Monitoring operations</span></span>
<span data-ttu-id="be60c-171">Tillämpa en rekommendation kanske inte omedelbart sker.</span><span class="sxs-lookup"><span data-stu-id="be60c-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="be60c-172">Portalen innehåller information om status för rekommendation.</span><span class="sxs-lookup"><span data-stu-id="be60c-172">The portal provides details regarding the status of recommendation.</span></span> <span data-ttu-id="be60c-173">Följande är möjliga tillstånd att ett index i:</span><span class="sxs-lookup"><span data-stu-id="be60c-173">The following are possible states that an index can be in:</span></span>

| <span data-ttu-id="be60c-174">Status</span><span class="sxs-lookup"><span data-stu-id="be60c-174">Status</span></span> | <span data-ttu-id="be60c-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="be60c-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="be60c-176">Väntande åtgärder</span><span class="sxs-lookup"><span data-stu-id="be60c-176">Pending</span></span> |<span data-ttu-id="be60c-177">Tillämpa rekommendation kommandot har tagits emot och har schemalagts för körning.</span><span class="sxs-lookup"><span data-stu-id="be60c-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="be60c-178">Köra</span><span class="sxs-lookup"><span data-stu-id="be60c-178">Executing</span></span> |<span data-ttu-id="be60c-179">Rekommendationen som används.</span><span class="sxs-lookup"><span data-stu-id="be60c-179">The recommendation is being applied.</span></span> |
| <span data-ttu-id="be60c-180">Verifiera</span><span class="sxs-lookup"><span data-stu-id="be60c-180">Verifying</span></span> |<span data-ttu-id="be60c-181">Rekommendation har tillämpats och tjänsten mäter fördelarna.</span><span class="sxs-lookup"><span data-stu-id="be60c-181">Recommendation was successfully applied and the service is measuring the benefits.</span></span> |
| <span data-ttu-id="be60c-182">Lyckades</span><span class="sxs-lookup"><span data-stu-id="be60c-182">Success</span></span> |<span data-ttu-id="be60c-183">Rekommendation har tillämpats och fördelar har tagits mäts.</span><span class="sxs-lookup"><span data-stu-id="be60c-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="be60c-184">Fel</span><span class="sxs-lookup"><span data-stu-id="be60c-184">Error</span></span> |<span data-ttu-id="be60c-185">Det uppstod ett fel i samband med tillämpandet av rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="be60c-185">An error occurred during the process of applying the recommendation.</span></span> <span data-ttu-id="be60c-186">Detta kan vara ett övergående problem eller eventuellt ett schema ändra i tabellen och skriptet är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="be60c-186">This can be a transient issue, or possibly a schema change to the table and the script is no longer valid.</span></span> |
| <span data-ttu-id="be60c-187">Återställa</span><span class="sxs-lookup"><span data-stu-id="be60c-187">Reverting</span></span> |<span data-ttu-id="be60c-188">Rekommendationen tillämpades, men har bedömts vara icke-performant och återställs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="be60c-188">The recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="be60c-189">Har återställts</span><span class="sxs-lookup"><span data-stu-id="be60c-189">Reverted</span></span> |<span data-ttu-id="be60c-190">Rekommendationen återställdes.</span><span class="sxs-lookup"><span data-stu-id="be60c-190">The recommendation was reverted.</span></span> |

<span data-ttu-id="be60c-191">Klicka på ett pågående rekommendation från listan över att se mer information:</span><span class="sxs-lookup"><span data-stu-id="be60c-191">Click an in-process recommendation from the list to see more details:</span></span>

![Rekommenderat index](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="be60c-193">Återställa en rekommendation</span><span class="sxs-lookup"><span data-stu-id="be60c-193">Reverting a recommendation</span></span>
<span data-ttu-id="be60c-194">Om du använde prestanda rekommendationer för att tillämpa rekommendationerna återställs (vilket innebär att du inte manuellt köra T-SQL-skript) den automatiskt den om den hittar prestandapåverkan vara negativ.</span><span class="sxs-lookup"><span data-stu-id="be60c-194">If you used the performance recommendations to apply the recommendation (meaning you did not manually run the T-SQL script) it will automatically revert it if it finds the performance impact to be negative.</span></span> <span data-ttu-id="be60c-195">Om du av någon anledning som du vill återställa en rekommendation kan du göra följande:</span><span class="sxs-lookup"><span data-stu-id="be60c-195">If for any reason you simply want to revert a recommendation you can do the following:</span></span>

1. <span data-ttu-id="be60c-196">Välj har tillämpade rekommendation i den **justera historik** område.</span><span class="sxs-lookup"><span data-stu-id="be60c-196">Select a successfully applied recommendation in the **Tuning history** area.</span></span>
2. <span data-ttu-id="be60c-197">Klicka på **Återställ** på den **rekommendationsdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="be60c-197">Click **Revert** on the **recommendation details** blade.</span></span>

![Rekommenderat index](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="be60c-199">Övervaka prestandapåverkan index-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be60c-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="be60c-200">När rekommendationerna har implementerats (för närvarande, index-åtgärder och parameterstyra frågor rekommendationer endast) kan du klicka på **frågan insikter** på informationsbladet rekommendation att öppna [fråga Insikter i frågeprestanda](sql-database-query-performance.md) och se påverkan på prestandan för din de vanligaste frågorna.</span><span class="sxs-lookup"><span data-stu-id="be60c-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on the recommendation details blade to open [Query Performance Insights](sql-database-query-performance.md) and see the performance impact of your top queries.</span></span>

![Övervakaren prestandapåverkan](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="be60c-202">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="be60c-202">Summary</span></span>
<span data-ttu-id="be60c-203">Azure SQL-databasen innehåller rekommendationer för att förbättra prestanda för SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="be60c-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="be60c-204">Du får en bra hjälp med att optimera din databas och slutligen förbättra frågeprestanda genom att tillhandahålla T-SQL-skript, samt individuella och fullständigt-automatisk.</span><span class="sxs-lookup"><span data-stu-id="be60c-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be60c-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be60c-205">Next steps</span></span>
<span data-ttu-id="be60c-206">Övervaka dina rekommendationer och fortsätta att använda dem för att förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="be60c-206">Monitor your recommendations and continue to apply them to refine performance.</span></span> <span data-ttu-id="be60c-207">Databasarbetsbelastningar är dynamiska och ändra kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="be60c-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="be60c-208">Azure SQL-databasen fortsätter att övervaka och ger rekommendationer som kan förbättra dina databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="be60c-208">Azure SQL Database will continue to monitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="be60c-209">Se [automatisk justering](sql-database-automatic-tuning.md) att lära dig mer om automatisk inställning i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="be60c-209">See [Automatic tuning](sql-database-automatic-tuning.md) to learn more about the automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="be60c-210">Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be60c-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="be60c-211">Se [insikter i frågeprestanda](sql-database-query-performance.md) mer information om hur du visar påverkan på prestandan för din de vanligaste frågorna.</span><span class="sxs-lookup"><span data-stu-id="be60c-211">See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be60c-212">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="be60c-212">Additional resources</span></span>
* [<span data-ttu-id="be60c-213">Query Store</span><span class="sxs-lookup"><span data-stu-id="be60c-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="be60c-214">SKAPA INDEX</span><span class="sxs-lookup"><span data-stu-id="be60c-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="be60c-215">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="be60c-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

