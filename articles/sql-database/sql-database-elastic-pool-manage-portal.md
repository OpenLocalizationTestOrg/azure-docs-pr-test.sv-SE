---
title: 'Azure-portalen: skapa och hantera en SQL Database-elastisk pool | Microsoft Docs'
description: "Lär dig hur du använder Azure portal och SQL-databas inbyggd intelligens att hantera, övervaka och en skalbar elastisk pool för att optimera databasprestanda och hantera kostnad få rätt storlek."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a><span data-ttu-id="52e8c-103">Skapa och hantera en elastisk pool med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="52e8c-103">Create and manage an elastic pool with the Azure portal</span></span>
<span data-ttu-id="52e8c-104">Det här avsnittet beskrivs hur du skapar och hanterar skalbara [elastiska pooler](sql-database-elastic-pool.md) med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-104">This topic shows you how to create and manage scalable [elastic pools](sql-database-elastic-pool.md) with the Azure portal.</span></span> <span data-ttu-id="52e8c-105">Du kan också skapa och hantera en Azure elastisk pool med [PowerShell](sql-database-elastic-pool-manage-powershell.md), REST-API eller [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), the REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="52e8c-106">Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="52e8c-107">Skapa en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-107">Create an elastic pool</span></span> 

<span data-ttu-id="52e8c-108">Det finns två sätt som du kan skapa en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="52e8c-109">Du kan göra det från grunden om du vet vilken typ av pool du vill ha, eller så kan du börja med en rekommendation från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="52e8c-109">You can do it from scratch if you know the pool setup you want, or start with a recommendation from the service.</span></span> <span data-ttu-id="52e8c-110">SQL Database har inbyggd intelligens som rekommenderar en elastisk pool-installationen om det är mer kostnadseffektivt baserat på den senaste användningstelemetrin för dina databaser.</span><span class="sxs-lookup"><span data-stu-id="52e8c-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on the past usage telemetry for your databases.</span></span>

<span data-ttu-id="52e8c-111">Du kan skapa flera pooler på en server, men du kan inte lägga till databaser från olika servrar i samma pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-111">You can create multiple pools on a server, but you can't add databases from different servers into the same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="52e8c-112">Elastiska pooler är allmänt tillgängliga (GA) i alla Azure-regioner utom Västra Indien där de genomgår förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="52e8c-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="52e8c-113">GA för elastiska pooler i den här regionen inträffar så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="52e8c-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="52e8c-114">Steg 1: Skapa en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="52e8c-115">Skapa en elastisk pool från en befintlig **server** blad i portalen är det enklaste sättet att flytta befintliga databaser till en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-115">Creating an elastic pool from an existing **server** blade in the portal is the easiest way to move existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="52e8c-116">Du kan också skapa en elastisk pool genom att söka **elastiska SQL-poolen** i den **Marketplace** eller klicka på **+ Lägg till** i den **SQL elastiska pooler** Bläddra i bladet.</span><span class="sxs-lookup"><span data-stu-id="52e8c-116">You can also create an elastic pool by searching **SQL elastic pool** in the **Marketplace** or clicking **+Add** in the **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="52e8c-117">Du kan ange en ny eller befintlig server via den här poolen etablering av arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="52e8c-117">You are able to specify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="52e8c-118">I den [Azure-portalen](http://portal.azure.com/), klickar du på **fler tjänster**  **>**  **SQL-servrar**, och klicka sedan på den server som innehåller den databaser som du vill lägga till en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-118">In the [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click the server that contains the databases you want to add to an elastic pool.</span></span>
2. <span data-ttu-id="52e8c-119">Klicka på **Ny pool**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-119">Click **New pool**.</span></span>

    ![Lägg till poolen till en server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="52e8c-121">**-ELLER-**</span><span class="sxs-lookup"><span data-stu-id="52e8c-121">**-OR-**</span></span>

    <span data-ttu-id="52e8c-122">Du kan se ett meddelande om att det finns rekommenderade elastiska pooler för servern.</span><span class="sxs-lookup"><span data-stu-id="52e8c-122">You may see a message saying there are recommended elastic pools for the server.</span></span> <span data-ttu-id="52e8c-123">Klicka på meddelandet för att se de rekommenderade poolerna baserat på historisk telemetri över databasanvändningen och klicka på nivån för att visa mer information och anpassa poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-123">Click the message to see the recommended pools based on historical database usage telemetry, and then click the tier to see more details and customize the pool.</span></span> <span data-ttu-id="52e8c-124">Se [Förstå pool-rekommendationer](#understand-elastic-pool-recommendations) lite senare i ämnet för hur rekommendationen görs.</span><span class="sxs-lookup"><span data-stu-id="52e8c-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how the recommendation is made.</span></span>

    ![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="52e8c-126">Den **elastisk pool** bladet visas, som är där du anger inställningarna för din pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-126">The **elastic pool** blade appears, which is where you specify the settings for your pool.</span></span> <span data-ttu-id="52e8c-127">Om du klickade på **ny pool** i föregående steg, prisnivån är **Standard** som standard och inga databaser har valts.</span><span class="sxs-lookup"><span data-stu-id="52e8c-127">If you clicked **New pool** in the previous step, the pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="52e8c-128">Du kan skapa en tom pool eller ange en uppsättning befintliga databaser från den servern, som du vill flytta till poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-128">You can create an empty pool, or specify a set of existing databases from that server to move into the pool.</span></span> <span data-ttu-id="52e8c-129">Om du skapar en rekommenderad pool rekommenderade prisnivå, inställningar och listan över databaser är förinställd, men du kan ändra dem.</span><span class="sxs-lookup"><span data-stu-id="52e8c-129">If you are creating a recommended pool, the recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="52e8c-131">Ange ett namn för den elastiska poolen, eller lämna den som standard.</span><span class="sxs-lookup"><span data-stu-id="52e8c-131">Specify a name for the elastic pool, or leave it as the default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="52e8c-132">Steg 2: Välj prisnivå.</span><span class="sxs-lookup"><span data-stu-id="52e8c-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="52e8c-133">Prisnivån för poolen avgör vilka funktioner som är tillgängliga för elastics i poolen och maximala antalet edtu: er (eDTU MAX) och lagringsutrymme (GB) är tillgängliga för varje databas.</span><span class="sxs-lookup"><span data-stu-id="52e8c-133">The pool's pricing tier determines the features available to the elastics in the pool, and the maximum number of eDTUs (eDTU MAX), and storage (GBs) available to each database.</span></span> <span data-ttu-id="52e8c-134">Mer information finns i tjänstnivåer.</span><span class="sxs-lookup"><span data-stu-id="52e8c-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="52e8c-135">Om du vill ändra prisnivå för poolen klickar du på **Prisnivå**, klickar på önskad prisnivå och sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-135">To change the pricing tier for the pool, click **Pricing tier**, click the pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="52e8c-136">När du valt prisnivå och bekräftat ditt val genom att klicka på **OK** i det sista steget, kommer du inte kunna ändra prisnivå för poolen igen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-136">After you choose the pricing tier and commit your changes by clicking **OK** in the last step, you won't be able to change the pricing tier of the pool.</span></span> <span data-ttu-id="52e8c-137">Om du vill ändra prisnivå för en befintlig elastisk pool, skapa en elastisk pool i den önskade prisnivån och migrera databaserna till den nya poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-137">To change the pricing tier for an existing elastic pool, create an elastic pool in the desired pricing tier and migrate the databases to this new pool.</span></span>
>

![Välj en prisnivå](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a><span data-ttu-id="52e8c-139">Steg 3: Konfigurera poolen</span><span class="sxs-lookup"><span data-stu-id="52e8c-139">Step 3: Configure the pool</span></span>

<span data-ttu-id="52e8c-140">När du har ställt in prisnivå, klickar du på Konfigurera pool där du lägger till databaser, ange pool-edtu: er och lagring (pool-GB) och när du anger den min och max edtu: er för elastics i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-140">After setting the pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set the min and max eDTUs for the elastics in the pool.</span></span>

1. <span data-ttu-id="52e8c-141">Klicka på **Konfigurera pool**</span><span class="sxs-lookup"><span data-stu-id="52e8c-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="52e8c-142">Välj de databaser som du vill lägga till i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-142">Select the databases you want to add to the pool.</span></span> <span data-ttu-id="52e8c-143">Det här steget är valfritt medan du skapar poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-143">This step is optional while creating the pool.</span></span> <span data-ttu-id="52e8c-144">Databaser kan läggas till efter att poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="52e8c-144">Databases can be added after the pool has been created.</span></span>
    <span data-ttu-id="52e8c-145">För att lägga till databaser, klickar du på **Lägg till databas**, klickar på de databaser som du vill lägga till och klickar sedan på knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-145">To add databases, click **Add database**, click the databases that you want to add, and then click the **Select** button.</span></span>

    ![Lägg till databaser](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="52e8c-147">Om de databaser du arbetar med har tillräckligt med historisk användningstelemetri, kommer stapeldiagrammen **Beräknad eDTU- och GB-användning** och **Faktisk eDTU-användning** att uppdateras för att hjälpa dig att fatta konfigurationsbeslut.</span><span class="sxs-lookup"><span data-stu-id="52e8c-147">If the databases you're working with have enough historical usage telemetry, the **Estimated eDTU and GB usage** graph and the **Actual eDTU usage** bar chart update to help you make configuration decisions.</span></span> <span data-ttu-id="52e8c-148">Tjänsten kan också ge dig ett rekommendationsmeddelande för att hjälpa dig få rätt storlek på poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-148">Also, the service may give you a recommendation message to help you right-size the pool.</span></span> <span data-ttu-id="52e8c-149">Se [Dynamiska rekommendationer](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="52e8c-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="52e8c-150">Använd kontrollerna på sidan **Konfigurera pool** för att kolla igenom inställningarna och konfigurera din pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-150">Use the controls on the **Configure pool** page to explore settings and configure your pool.</span></span> <span data-ttu-id="52e8c-151">Se [begränsningar för elastiska pooler](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för mer information om begränsningarna för varje tjänstnivå och se [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md) för detaljerad vägledning om rätt storlek för en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="52e8c-152">Mer information om poolinställningar finns [elastisk pool egenskaper](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="52e8c-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="52e8c-154">Klicka på **Välj** på bladet **Konfigurera pool**, efter att du har ändrat inställningarna.</span><span class="sxs-lookup"><span data-stu-id="52e8c-154">Click **Select** in the **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="52e8c-155">Klicka på **OK** för att skapa poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-155">Click **OK** to create the pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="52e8c-156">Förstå rekommendationer för elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="52e8c-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="52e8c-157">SQL Database-tjänsten utvärderar användningshistorik och rekommenderar en eller flera pooler när det är mer kostnadseffektivt än att använda enskilda databaser.</span><span class="sxs-lookup"><span data-stu-id="52e8c-157">The SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="52e8c-158">Varje rekommendation är konfigurerad med en unik delmängd av serverns databaser som bäst passar i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-158">Each recommendation is configured with a unique subset of the server's databases that best fit the pool.</span></span>

![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="52e8c-160">Pool-rekommendationerna omfattar:</span><span class="sxs-lookup"><span data-stu-id="52e8c-160">The pool recommendation comprises:</span></span>

- <span data-ttu-id="52e8c-161">En prisnivå för poolen (Basic, Standard, Premium eller Premium RS)</span><span class="sxs-lookup"><span data-stu-id="52e8c-161">A pricing tier for the pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="52e8c-162">Lämpliga **POOL-eDTU:er** (kallas även Max eDTU:er per pool)</span><span class="sxs-lookup"><span data-stu-id="52e8c-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="52e8c-163">**eDTU MAX** och **eDTU Min** per databas</span><span class="sxs-lookup"><span data-stu-id="52e8c-163">The **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="52e8c-164">Listan över rekommenderade databaser för poolen</span><span class="sxs-lookup"><span data-stu-id="52e8c-164">The list of recommended databases for the pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="52e8c-165">Tjänsten tar hänsyn till de senaste 30 dagarnas telemetri vid rekommendation av pooler.</span><span class="sxs-lookup"><span data-stu-id="52e8c-165">The service takes the last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="52e8c-166">Det måste finnas minst 7 dagar för en databas ska anses som en kandidat för en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-166">For a database to be considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="52e8c-167">Databaser som redan finns i en elastisk pool är inte rekommenderade kandidater för elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="52e8c-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="52e8c-168">Tjänsten utvärderar resursbehov och kostnadseffektivitet vid flytt av de enskilda databaserna i varje tjänstnivå till pooler inom samma nivå.</span><span class="sxs-lookup"><span data-stu-id="52e8c-168">The service evaluates resource needs and cost effectiveness of moving the single databases in each service tier into pools of the same tier.</span></span> <span data-ttu-id="52e8c-169">Alla standard-databaser på servern utvärderas exempelvis för hur de skulle passa in i en standard elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="52e8c-170">Det innebär att tjänsten inte göra rekommendationer mellan olika nivåer, som att flytta en standard-databas till en premium-pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-170">This means the service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="52e8c-171">När du lägger till databaserna i poolen, genereras dynamiskt rekommendationer baserat på historisk användning av databaserna som du har valt.</span><span class="sxs-lookup"><span data-stu-id="52e8c-171">After adding databases to the pool, recommendations are dynamically generated based on the historical usage of the databases you have selected.</span></span> <span data-ttu-id="52e8c-172">De här rekommendationerna som visas i eDTU och GB Användningsdiagram och i en rekommendations-banderoll överst i den **konfigurera pool** bladet.</span><span class="sxs-lookup"><span data-stu-id="52e8c-172">These recommendations are shown in the eDTU and GB usage chart and in a recommendation banner at the top of the **Configure pool** blade.</span></span> <span data-ttu-id="52e8c-173">De här rekommendationerna är avsedda att hjälpa dig att skapa en elastisk pool som är optimerade för dina specifika databaser.</span><span class="sxs-lookup"><span data-stu-id="52e8c-173">These recommendations are intended to assist you in creating an elastic pool optimized for your specific databases.</span></span>

![dynamiska rekommendationer](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="52e8c-175">Hantera och övervaka en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="52e8c-176">Du kan använda Azure-portalen för att övervaka och hantera en elastisk pool och databaserna i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-176">You can use the Azure portal to monitor and manage an elastic pool and the databases in the pool.</span></span> <span data-ttu-id="52e8c-177">Du kan övervaka användningen av en elastisk pool och databaserna i poolen från portalen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-177">From the portal, you can monitor the utilization of an elastic pool and the databases within that pool.</span></span> <span data-ttu-id="52e8c-178">Du kan också göra en uppsättning ändringar i den elastiska poolen och skicka alla ändringar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="52e8c-178">You can also make a set of changes to your elastic pool and submit all changes at the same time.</span></span> <span data-ttu-id="52e8c-179">De här förändringarna innefattar att lägga till eller ta bort databaser, ändra inställningarna elastisk pool eller ändra databasinställningarna.</span><span class="sxs-lookup"><span data-stu-id="52e8c-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="52e8c-180">Följande bild visar ett exempel elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-180">The following graphic shows an example elastic pool.</span></span> <span data-ttu-id="52e8c-181">Vyn innehåller:</span><span class="sxs-lookup"><span data-stu-id="52e8c-181">The view includes:</span></span>

*  <span data-ttu-id="52e8c-182">Diagram för övervakning av resursanvändningen för både den elastiska poolen och databaser som ingår i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-182">Charts for monitoring resource usage of both the elastic pool and the databases contained in the pool.</span></span>
*  <span data-ttu-id="52e8c-183">Den **konfigurera** pool för att göra ändringar i den elastiska poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-183">The **Configure** pool button to make changes to the elastic pool.</span></span>
*  <span data-ttu-id="52e8c-184">Den **Skapa databas** knapp som skapar en databas och lägger till den aktuella elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-184">The **Create database** button that creates a database and adds it to the current elastic pool.</span></span>
*  <span data-ttu-id="52e8c-185">Elastiska jobb som hjälper dig hantera stort antal databaser genom att köra Transact-SQL-skript mot alla databaser i en lista.</span><span class="sxs-lookup"><span data-stu-id="52e8c-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Pool-vy][2]

<span data-ttu-id="52e8c-187">Du kan gå till en viss pool för att se dess resursutnyttjande.</span><span class="sxs-lookup"><span data-stu-id="52e8c-187">You can go to a particular pool to see its resource utilization.</span></span> <span data-ttu-id="52e8c-188">Som standard konfigureras poolen om du vill visa lagrings- och eDTU-användning för den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-188">By default, the pool is configured to show storage and eDTU usage for the last hour.</span></span> <span data-ttu-id="52e8c-189">Diagrammet kan konfigureras för att visa olika mätvärden via olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="52e8c-189">The chart can be configured to show different metrics over various time windows.</span></span>

1. <span data-ttu-id="52e8c-190">Välj en elastisk pool att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="52e8c-190">Select an elastic pool to work with.</span></span>
2. <span data-ttu-id="52e8c-191">Under **elastisk Pool övervakning** är ett diagram med etiketten **resursutnyttjande**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="52e8c-192">Klicka på diagrammet.</span><span class="sxs-lookup"><span data-stu-id="52e8c-192">Click the chart.</span></span>

    ![Övervakning av elastisk pool][3]

    <span data-ttu-id="52e8c-194">Den **mått** blad öppnas och visar en detaljerad vy av de angivna mätvärdena via den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="52e8c-194">The **Metric** blade opens, showing a detailed view of the specified metrics over the specified time window.</span></span>   

    ![Bladet Mått][9]

### <a name="to-customize-the-chart-display"></a><span data-ttu-id="52e8c-196">Du kan anpassa diagram</span><span class="sxs-lookup"><span data-stu-id="52e8c-196">To customize the chart display</span></span>

<span data-ttu-id="52e8c-197">Du kan redigera diagrammet och mått bladet för att visa andra mått som CPU-procent, data IO-procent och loggen IO-procent som används.</span><span class="sxs-lookup"><span data-stu-id="52e8c-197">You can edit the chart and the metric blade to display other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="52e8c-198">Klicka på bladet mått **redigera**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-198">On the metric blade, click **Edit**.</span></span>

    ![Klicka på Redigera][6]

2. <span data-ttu-id="52e8c-200">I den **redigera diagram** bladet Välj ett tidsintervall (efter timme, dag, eller föregående vecka), eller klicka på **anpassade** att välja alla datumintervall under de senaste två veckorna.</span><span class="sxs-lookup"><span data-stu-id="52e8c-200">In the **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** to select any date range in the last two weeks.</span></span> <span data-ttu-id="52e8c-201">Välj diagramtyp (stapel- eller rad) och välj sedan resurserna som ska övervaka.</span><span class="sxs-lookup"><span data-stu-id="52e8c-201">Select the chart type (bar or line), then select the resources to monitor.</span></span>

   > [!Note]
   > <span data-ttu-id="52e8c-202">Endast mått med samma enheten kan visas i diagrammet på samma gång.</span><span class="sxs-lookup"><span data-stu-id="52e8c-202">Only metrics with the same unit of measure can be displayed in the chart at the same time.</span></span> <span data-ttu-id="52e8c-203">Till exempel om du väljer ”eDTU i procent” kan du bara välja andra mått med procentandel som enheten.</span><span class="sxs-lookup"><span data-stu-id="52e8c-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as the unit of measure.</span></span>
   >

    ![Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="52e8c-205">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="52e8c-206">Hantera och övervaka databaser i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="52e8c-207">Enskilda databaser kan också övervakas för potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="52e8c-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="52e8c-208">Under **elastisk databas övervakning**, det finns ett diagram som visar mått för fem databaser.</span><span class="sxs-lookup"><span data-stu-id="52e8c-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="52e8c-209">Som standard i diagrammet visas de översta 5 databaserna i poolen efter genomsnittlig eDTU-användning under den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-209">By default, the chart displays the top 5 databases in the pool by average eDTU usage in the past hour.</span></span> <span data-ttu-id="52e8c-210">Klicka på diagrammet.</span><span class="sxs-lookup"><span data-stu-id="52e8c-210">Click the chart.</span></span>

    ![Övervakning av elastisk pool][4]

2. <span data-ttu-id="52e8c-212">Den **databasen resursutnyttjande** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="52e8c-212">The **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="52e8c-213">Detta ger en detaljerad vy av databasanvändningen i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-213">This provides a detailed view of the database usage in the pool.</span></span> <span data-ttu-id="52e8c-214">Med rutnätet i den nedre delen av bladet kan välja du alla databaser i poolen för att visa dess användning i diagrammet (upp till 5 databaser).</span><span class="sxs-lookup"><span data-stu-id="52e8c-214">Using the grid in the lower part of the blade, you can select any databases in the pool to display its usage in the chart (up to 5 databases).</span></span> <span data-ttu-id="52e8c-215">Du kan också anpassa fönstret mått och tid som visas i diagrammet genom att klicka på **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-215">You can also customize the metrics and time window displayed in the chart by clicking **Edit chart**.</span></span>

    ![-Databasresursbladet användning][8]

### <a name="to-customize-the-view"></a><span data-ttu-id="52e8c-217">Anpassa vyn</span><span class="sxs-lookup"><span data-stu-id="52e8c-217">To customize the view</span></span>

1. <span data-ttu-id="52e8c-218">I den **databasen resursutnyttjande** bladet, klickar du på **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-218">In the **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Klicka på Redigera diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="52e8c-220">I den **redigera** diagram bladet väljer du ett tidsintervall (efter timme eller senaste 24 timmarna) eller klicka på **anpassade** att välja en annan dag under de senaste 2 veckorna ska visas.</span><span class="sxs-lookup"><span data-stu-id="52e8c-220">In the **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** to select a different day in the past 2 weeks to display.</span></span>

    ![Klicka på anpassad](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="52e8c-222">Klicka på den **databaser genom att jämföra** listrutan och välj ett annat mått som ska användas vid jämförelse av databaser.</span><span class="sxs-lookup"><span data-stu-id="52e8c-222">Click the **Compare databases by** dropdown to select a different metric to use when comparing databases.</span></span>

    ![Redigera schemat](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a><span data-ttu-id="52e8c-224">Att välja databaser för att övervaka</span><span class="sxs-lookup"><span data-stu-id="52e8c-224">To select databases to monitor</span></span>

<span data-ttu-id="52e8c-225">I databaslistan i den **databasen resursutnyttjande** bladet hittar du viss databaser genom att titta igenom sidorna i listan eller genom att skriva namnet på en databas.</span><span class="sxs-lookup"><span data-stu-id="52e8c-225">In the database list in the **Database Resource Utilization** blade, you can find particular databases by looking through the pages in the list or by typing in the name of a database.</span></span> <span data-ttu-id="52e8c-226">Använd kryssrutan för att välja databasen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-226">Use the checkbox to select the database.</span></span>

![Sök efter databaser för att övervaka][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a><span data-ttu-id="52e8c-228">Lägga till en avisering till en elastisk pool-resurs</span><span class="sxs-lookup"><span data-stu-id="52e8c-228">Add an alert to an elastic pool resource</span></span>

<span data-ttu-id="52e8c-229">Du kan lägga till regler för en elastisk pool som skickar e-post till personer eller avisering strängar till URL: en slutpunkter när den elastiska poolen träffar ett tröskelvärde för användning som du skapat.</span><span class="sxs-lookup"><span data-stu-id="52e8c-229">You can add rules to an elastic pool that send email to people or alert strings to URL endpoints when the elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="52e8c-230">**Lägga till en avisering till en resurs:**</span><span class="sxs-lookup"><span data-stu-id="52e8c-230">**To add an alert to any resource:**</span></span>

1. <span data-ttu-id="52e8c-231">Klicka på den **resursutnyttjande** diagrammet för att öppna den **mått** bladet klickar du på **Lägg till avisering**, och fyller sedan informationen i den **lägga till en varningsregel** bladet (**resurs** har angetts automatiskt upp till att du arbetar med poolen).</span><span class="sxs-lookup"><span data-stu-id="52e8c-231">Click the **Resource utilization** chart to open the **Metric** blade, click **Add alert**, and then fill out the information in the **Add an alert rule** blade (**Resource** is automatically set up to be the pool you're working with).</span></span>
2. <span data-ttu-id="52e8c-232">Ange en **namn** och **beskrivning** som identifierar aviseringen du och mottagare.</span><span class="sxs-lookup"><span data-stu-id="52e8c-232">Type a **Name** and **Description** that identifies the alert to you and the recipients.</span></span>
3. <span data-ttu-id="52e8c-233">Välj en **mått** som du vill Varna från listan.</span><span class="sxs-lookup"><span data-stu-id="52e8c-233">Choose a **Metric** that you want to alert from the list.</span></span>

    <span data-ttu-id="52e8c-234">Diagrammet visar dynamiskt resursanvändningen för den måtten för att välja ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="52e8c-234">The chart dynamically shows resource utilization for that metric to help you choose a threshold.</span></span>

4. <span data-ttu-id="52e8c-235">Välj en **villkoret** (större än mindre än osv) och en **tröskelvärdet**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="52e8c-236">Välj en **Period** som mått regeln måste uppfyllas innan aviseringen utlösare.</span><span class="sxs-lookup"><span data-stu-id="52e8c-236">Choose a **Period** of time that the metric rule must be satisfied before the alert triggers.</span></span>
6. <span data-ttu-id="52e8c-237">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-237">Click **OK**.</span></span>

<span data-ttu-id="52e8c-238">Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="52e8c-239">Flytta en databas till en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="52e8c-240">Du kan lägga till eller ta bort databaser från en befintlig adresspool.</span><span class="sxs-lookup"><span data-stu-id="52e8c-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="52e8c-241">Databaserna kan vara i andra pooler.</span><span class="sxs-lookup"><span data-stu-id="52e8c-241">The databases can be in other pools.</span></span> <span data-ttu-id="52e8c-242">Du kan bara lägga till databaser som finns på samma logiska server.</span><span class="sxs-lookup"><span data-stu-id="52e8c-242">However, you can only add databases that are on the same logical server.</span></span>

1. <span data-ttu-id="52e8c-243">I bladet för i poolen, under **elastiska databaser** klickar du på **konfigurera pool**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-243">In the blade for the pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Klicka på Konfigurera pool][1]

2. <span data-ttu-id="52e8c-245">I den **konfigurera pool** bladet, klickar du på **lägger till poolen**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-245">In the **Configure pool** blade, click **Add to pool**.</span></span>

    ![Klicka på Lägg till pool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="52e8c-247">I den **lägga till databaser** bladet Välj databasen eller databaser att lägga till i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-247">In the **Add databases** blade, select the database or databases to add to the pool.</span></span> <span data-ttu-id="52e8c-248">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-248">Then click **Select**.</span></span>

    ![Välj databaser att lägga till](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="52e8c-250">Den **konfigurera pool** bladet visas nu i databasen som du har valt för att läggas till, med statusen **väntande**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-250">The **Configure pool** blade now lists the database you selected to be added, with its status set to **Pending**.</span></span>

    ![Väntande pool-tillägg](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="52e8c-252">I den **konfigurera pool bladet**, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-252">In the **Configure pool blade**, click **Save**.</span></span>

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="52e8c-254">Flytta en databas från en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="52e8c-255">I den **konfigurera pool** bladet Välj databasen eller databaser att ta bort.</span><span class="sxs-lookup"><span data-stu-id="52e8c-255">In the **Configure pool** blade, select the database or databases to remove.</span></span>

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="52e8c-257">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-257">Click **Remove from pool**.</span></span>

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="52e8c-259">Den **konfigurera pool** bladet visas nu i databasen som du har valt att tas bort med statusen **väntande**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-259">The **Configure pool** blade now lists the database you selected to be removed with its status set to **Pending**.</span></span>

    ![Förhandsgranska databasen tillägg och borttagning](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="52e8c-261">I den **konfigurera pool bladet**, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="52e8c-261">In the **Configure pool blade**, click **Save**.</span></span>

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="52e8c-263">Ändra inställningar för prestanda för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="52e8c-264">När du övervakar resursanvändningen för en elastisk pool kan du märka att några justeringar behövs.</span><span class="sxs-lookup"><span data-stu-id="52e8c-264">As you monitor the resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="52e8c-265">Poolen måste kanske en ändring i gränser prestanda eller lagring.</span><span class="sxs-lookup"><span data-stu-id="52e8c-265">Maybe the pool needs a change in the performance or storage limits.</span></span> <span data-ttu-id="52e8c-266">Kanske vill du ändra databasinställningarna i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-266">Possibly you want to change the database settings in the pool.</span></span> <span data-ttu-id="52e8c-267">Du kan ändra inställningarna för poolen när som helst för att hämta den bästa balansen mellan prestanda och kostnader.</span><span class="sxs-lookup"><span data-stu-id="52e8c-267">You can change the setup of the pool at any time to get the best balance of performance and cost.</span></span> <span data-ttu-id="52e8c-268">Se [när en elastisk pool kan användas?](sql-database-elastic-pool.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="52e8c-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="52e8c-269">Begränsar per pool och edtu: er per databas om du vill ändra edtu: er eller lagring:</span><span class="sxs-lookup"><span data-stu-id="52e8c-269">To change the eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="52e8c-270">Öppna den **konfigurera pool** bladet.</span><span class="sxs-lookup"><span data-stu-id="52e8c-270">Open the **Configure pool** blade.</span></span>

    <span data-ttu-id="52e8c-271">Under **elastisk poolinställningar**, ändra poolinställningarna för med hjälp av antingen skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="52e8c-271">Under **elastic pool settings**, use either slider to change the pool settings.</span></span>

    ![Resursutnyttjande elastisk pool](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="52e8c-273">När inställningen ändras, visas den månatliga uppskattade kostnaden för ändringen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-273">When the setting changes, the display shows the estimated monthly cost of the change.</span></span>

    ![Uppdatera en elastisk pool och nya månadskostnaden](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="52e8c-275">Svarstiden för åtgärder elastisk pool</span><span class="sxs-lookup"><span data-stu-id="52e8c-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="52e8c-276">Ändra min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.</span><span class="sxs-lookup"><span data-stu-id="52e8c-276">Changing the min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="52e8c-277">Ändra edtu: er per pool beror på den totala mängden diskutrymme som används av alla databaser i poolen.</span><span class="sxs-lookup"><span data-stu-id="52e8c-277">Changing the eDTUs per pool depends on the total amount of space used by all databases in the pool.</span></span> <span data-ttu-id="52e8c-278">Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB.</span><span class="sxs-lookup"><span data-stu-id="52e8c-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="52e8c-279">Om det totala utrymmet som används av alla databaser i poolen är exempelvis 200 GB och den förväntade svarstiden för att ändra pool-eDTU per pool är 3 timmar eller mindre.</span><span class="sxs-lookup"><span data-stu-id="52e8c-279">For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for changing the pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52e8c-280">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52e8c-280">Next steps</span></span>

- <span data-ttu-id="52e8c-281">För att förstå vad en elastisk pool är, se [SQL Database-elastisk pool](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-281">To understand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="52e8c-282">Anvisningar om hur du använder elastiska pooler finns [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="52e8c-283">Om du vill använda elastiska jobb för att köra Transact-SQL-skript mot valfritt antal databaser i poolen, se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-283">To use elastic jobs to run Transact-SQL scripts against any number of databases in the pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="52e8c-284">Om du vill fråga på valfritt antal databaser i poolen, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-284">To query across any number of databases in the pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="52e8c-285">Transaktioner valfritt antal databaser i poolen, finns i [elastisk transaktioner](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="52e8c-285">For transactions any number of databases in the pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
