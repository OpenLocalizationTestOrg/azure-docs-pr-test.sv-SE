---
title: 'Azure-portalen: skapa och hantera en SQL Database-elastisk pool | Microsoft Docs'
description: "Lär dig hur toouse hello Azure-portalen och SQL-databas inbyggd intelligens toomanage, övervaka och rätt storlek en skalbar elastisk pool toooptimize databasens prestanda och hantera kostnaden."
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
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a><span data-ttu-id="06522-103">Skapa och hantera en elastisk pool med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="06522-103">Create and manage an elastic pool with hello Azure portal</span></span>
<span data-ttu-id="06522-104">Det här avsnittet beskrivs hur du toocreate och hantera skalbara [elastiska pooler](sql-database-elastic-pool.md) med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="06522-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with hello Azure portal.</span></span> <span data-ttu-id="06522-105">Du kan också skapa och hantera en Azure elastisk pool med [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API eller [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="06522-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="06522-106">Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="06522-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="06522-107">Skapa en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-107">Create an elastic pool</span></span> 

<span data-ttu-id="06522-108">Det finns två sätt som du kan skapa en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="06522-109">Du kan göra det från grunden om du vet hello pool-installationen du vill eller börja med en rekommendation från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="06522-109">You can do it from scratch if you know hello pool setup you want, or start with a recommendation from hello service.</span></span> <span data-ttu-id="06522-110">SQL Database har inbyggd intelligens som rekommenderar en elastisk pool-installationen om det är mer kostnadseffektivt baserat på hello senaste användningstelemetrin för dina databaser.</span><span class="sxs-lookup"><span data-stu-id="06522-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on hello past usage telemetry for your databases.</span></span>

<span data-ttu-id="06522-111">Du kan skapa flera pooler på en server, men du kan inte lägga till databaser från olika servrar i hello samma pool.</span><span class="sxs-lookup"><span data-stu-id="06522-111">You can create multiple pools on a server, but you can't add databases from different servers into hello same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="06522-112">Elastiska pooler är allmänt tillgängliga (GA) i alla Azure-regioner utom Västra Indien där de genomgår förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="06522-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="06522-113">GA för elastiska pooler i den här regionen inträffar så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="06522-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="06522-114">Steg 1: Skapa en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="06522-115">Skapa en elastisk pool från en befintlig **server** bladet i hello portal är hello enklaste sättet toomove befintliga databaser till en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-115">Creating an elastic pool from an existing **server** blade in hello portal is hello easiest way toomove existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="06522-116">Du kan också skapa en elastisk pool genom att söka **elastiska SQL-poolen** i hello **Marketplace** eller klicka på **+ Lägg till** i hello **SQL elastiska pooler**Bläddra bladet.</span><span class="sxs-lookup"><span data-stu-id="06522-116">You can also create an elastic pool by searching **SQL elastic pool** in hello **Marketplace** or clicking **+Add** in hello **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="06522-117">Du kan kan toospecify en ny eller befintlig server via den här poolen etablering av arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="06522-117">You are able toospecify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="06522-118">I hello [Azure-portalen](http://portal.azure.com/), klickar du på **fler tjänster**  **>**  **SQL-servrar**, och klicka sedan på hello-server som innehåller hello databaser som du vill tooadd tooan elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-118">In hello [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click hello server that contains hello databases you want tooadd tooan elastic pool.</span></span>
2. <span data-ttu-id="06522-119">Klicka på **Ny pool**.</span><span class="sxs-lookup"><span data-stu-id="06522-119">Click **New pool**.</span></span>

    ![Lägg till pool tooa server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="06522-121">**-ELLER-**</span><span class="sxs-lookup"><span data-stu-id="06522-121">**-OR-**</span></span>

    <span data-ttu-id="06522-122">Du kan se ett meddelande om att det finns rekommenderade elastiska pooler för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="06522-122">You may see a message saying there are recommended elastic pools for hello server.</span></span> <span data-ttu-id="06522-123">Klicka på hello meddelandet toosee hello rekommenderade poolerna baserat på historisk telemetri, och på hello nivå toosee mer information och anpassa hello poolen.</span><span class="sxs-lookup"><span data-stu-id="06522-123">Click hello message toosee hello recommended pools based on historical database usage telemetry, and then click hello tier toosee more details and customize hello pool.</span></span> <span data-ttu-id="06522-124">Se [förstå poolrekommendationer](#understand-elastic-pool-recommendations) senare i det här avsnittet för hur hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="06522-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how hello recommendation is made.</span></span>

    ![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="06522-126">Hej **elastisk pool** bladet visas, som är där du anger hello inställningarna för din pool.</span><span class="sxs-lookup"><span data-stu-id="06522-126">hello **elastic pool** blade appears, which is where you specify hello settings for your pool.</span></span> <span data-ttu-id="06522-127">Om du klickade på **ny pool** i föregående steg hello hello prisnivån är **Standard** som standard och inga databaser har valts.</span><span class="sxs-lookup"><span data-stu-id="06522-127">If you clicked **New pool** in hello previous step, hello pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="06522-128">Du kan skapa en tom pool eller ange en uppsättning befintliga databaser från denna server toomove till hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-128">You can create an empty pool, or specify a set of existing databases from that server toomove into hello pool.</span></span> <span data-ttu-id="06522-129">Om du skapar en rekommenderad pool hello rekommenderas prisnivån, prestandainställningar för och listan över databaser är förinställd, men du kan ändra dem.</span><span class="sxs-lookup"><span data-stu-id="06522-129">If you are creating a recommended pool, hello recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="06522-131">Ange ett namn för hello elastisk pool eller lämna den som standard hello.</span><span class="sxs-lookup"><span data-stu-id="06522-131">Specify a name for hello elastic pool, or leave it as hello default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="06522-132">Steg 2: Välj prisnivå.</span><span class="sxs-lookup"><span data-stu-id="06522-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="06522-133">hello avgör prisnivån för poolen hello funktioner tillgängliga toohello elastics i hello poolen och hello maximala antalet edtu: er (eDTU MAX) och lagringsutrymme (GB) tillgängliga tooeach databas.</span><span class="sxs-lookup"><span data-stu-id="06522-133">hello pool's pricing tier determines hello features available toohello elastics in hello pool, and hello maximum number of eDTUs (eDTU MAX), and storage (GBs) available tooeach database.</span></span> <span data-ttu-id="06522-134">Mer information finns i tjänstnivåer.</span><span class="sxs-lookup"><span data-stu-id="06522-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="06522-135">toochange hello prisnivå för poolen hello, klickar du på **prisnivå**, klicka på hello prisnivån och klickar sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="06522-135">toochange hello pricing tier for hello pool, click **Pricing tier**, click hello pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06522-136">När du väljer hello prisnivån och genomför ändringarna genom att klicka på **OK** hello sista steget, inte kan toochange hello prisnivån för hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-136">After you choose hello pricing tier and commit your changes by clicking **OK** in hello last step, you won't be able toochange hello pricing tier of hello pool.</span></span> <span data-ttu-id="06522-137">toochange hello prisnivå för en befintlig elastisk pool, skapa en elastisk pool i hello önskade prisnivån och migrera hello databaser toothis ny pool.</span><span class="sxs-lookup"><span data-stu-id="06522-137">toochange hello pricing tier for an existing elastic pool, create an elastic pool in hello desired pricing tier and migrate hello databases toothis new pool.</span></span>
>

![Välj en prisnivå](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a><span data-ttu-id="06522-139">Steg 3: Konfigurera hello pool</span><span class="sxs-lookup"><span data-stu-id="06522-139">Step 3: Configure hello pool</span></span>

<span data-ttu-id="06522-140">Klicka på Konfigurera pool när du har angett hello prisnivån, där du lägger till databaser, ange pool-edtu: er och lagring (pool-GB) och när du anger hello min och max edtu: er för hello elastics i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-140">After setting hello pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set hello min and max eDTUs for hello elastics in hello pool.</span></span>

1. <span data-ttu-id="06522-141">Klicka på **Konfigurera pool**</span><span class="sxs-lookup"><span data-stu-id="06522-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="06522-142">Välj hello-databaser som du vill tooadd toohello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-142">Select hello databases you want tooadd toohello pool.</span></span> <span data-ttu-id="06522-143">Det här steget är valfritt medan du skapar hello poolen.</span><span class="sxs-lookup"><span data-stu-id="06522-143">This step is optional while creating hello pool.</span></span> <span data-ttu-id="06522-144">Databaser kan läggas till efter hello poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="06522-144">Databases can be added after hello pool has been created.</span></span>
    <span data-ttu-id="06522-145">tooadd databaserna klickar du på **Lägg till databas**, klickar du på hello databaser du vill tooadd och klicka sedan på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="06522-145">tooadd databases, click **Add database**, click hello databases that you want tooadd, and then click hello **Select** button.</span></span>

    ![Lägg till databaser](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="06522-147">Om hello databaser du arbetar med har tillräckligt med historisk användningstelemetri, hello **beräknad eDTU och GB-användning** diagram och hello **faktisk eDTU-användning** stapeldiagram uppdatering toohelp du göra konfiguration beslut.</span><span class="sxs-lookup"><span data-stu-id="06522-147">If hello databases you're working with have enough historical usage telemetry, hello **Estimated eDTU and GB usage** graph and hello **Actual eDTU usage** bar chart update toohelp you make configuration decisions.</span></span> <span data-ttu-id="06522-148">Dessutom du hello-tjänsten kan ge en rekommendation meddelandet toohelp du rätt storlek hello poolen.</span><span class="sxs-lookup"><span data-stu-id="06522-148">Also, hello service may give you a recommendation message toohelp you right-size hello pool.</span></span> <span data-ttu-id="06522-149">Se [Dynamiska rekommendationer](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="06522-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="06522-150">Använda hello kontroller i hello **konfigurera pool** sidan tooexplore inställningar och konfigurera din pool.</span><span class="sxs-lookup"><span data-stu-id="06522-150">Use hello controls on hello **Configure pool** page tooexplore settings and configure your pool.</span></span> <span data-ttu-id="06522-151">Se [begränsningar för elastiska pooler](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för mer information om begränsningarna för varje tjänstnivå och se [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md) för detaljerad vägledning om rätt storlek för en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="06522-152">Mer information om poolinställningar finns [elastisk pool egenskaper](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="06522-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="06522-154">Klicka på **Välj** i hello **konfigurera Pool** bladet när du har ändrat inställningarna.</span><span class="sxs-lookup"><span data-stu-id="06522-154">Click **Select** in hello **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="06522-155">Klicka på **OK** toocreate hello poolen.</span><span class="sxs-lookup"><span data-stu-id="06522-155">Click **OK** toocreate hello pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="06522-156">Förstå rekommendationer för elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="06522-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="06522-157">hello SQL Database-tjänsten utvärderar Användningshistorik och rekommenderar en eller flera pooler när det är mer kostnadseffektivt än att använda enskilda databaser.</span><span class="sxs-lookup"><span data-stu-id="06522-157">hello SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="06522-158">Varje rekommendation är konfigurerad med en unik delmängd av hello server-databaser som bäst passar hello poolen.</span><span class="sxs-lookup"><span data-stu-id="06522-158">Each recommendation is configured with a unique subset of hello server's databases that best fit hello pool.</span></span>

![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="06522-160">hello pool-rekommendationerna omfattar:</span><span class="sxs-lookup"><span data-stu-id="06522-160">hello pool recommendation comprises:</span></span>

- <span data-ttu-id="06522-161">En prisnivå för hello poolen (Basic, Standard, Premium eller Premium RS)</span><span class="sxs-lookup"><span data-stu-id="06522-161">A pricing tier for hello pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="06522-162">Lämpliga **POOL-eDTU:er** (kallas även Max eDTU:er per pool)</span><span class="sxs-lookup"><span data-stu-id="06522-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="06522-163">Hej **eDTU MAX** och **eDTU Min** per databas</span><span class="sxs-lookup"><span data-stu-id="06522-163">hello **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="06522-164">hello listan över rekommenderade databaser för hello-pool</span><span class="sxs-lookup"><span data-stu-id="06522-164">hello list of recommended databases for hello pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06522-165">hello service beaktas hello senaste 30 dagarnas telemetri vid rekommendation av pooler.</span><span class="sxs-lookup"><span data-stu-id="06522-165">hello service takes hello last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="06522-166">För en databas toobe betraktas som en kandidat för en elastisk pool, måste det finnas minst 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="06522-166">For a database toobe considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="06522-167">Databaser som redan finns i en elastisk pool är inte rekommenderade kandidater för elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="06522-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="06522-168">hello tjänsten utvärderar resursbehov och kostnadseffektivitet av glidande hello enda databaser i varje tjänstnivå till pooler inom hello samma nivå.</span><span class="sxs-lookup"><span data-stu-id="06522-168">hello service evaluates resource needs and cost effectiveness of moving hello single databases in each service tier into pools of hello same tier.</span></span> <span data-ttu-id="06522-169">Alla standard-databaser på servern utvärderas exempelvis för hur de skulle passa in i en standard elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="06522-170">Det innebär att hello-tjänsten inte göra rekommendationer mellan olika nivåer, till exempel flytta en Standard-databas till en Premium-pool.</span><span class="sxs-lookup"><span data-stu-id="06522-170">This means hello service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="06522-171">När du lägger till databaser toohello pool genereras dynamiskt rekommendationer baserat på hello historisk användning av hello-databaser som du har valt.</span><span class="sxs-lookup"><span data-stu-id="06522-171">After adding databases toohello pool, recommendations are dynamically generated based on hello historical usage of hello databases you have selected.</span></span> <span data-ttu-id="06522-172">De här rekommendationerna som visas i hello eDTU och GB och i en rekommendations-banderoll överst hello i hello **konfigurera pool** bladet.</span><span class="sxs-lookup"><span data-stu-id="06522-172">These recommendations are shown in hello eDTU and GB usage chart and in a recommendation banner at hello top of hello **Configure pool** blade.</span></span> <span data-ttu-id="06522-173">De här rekommendationerna är avsedda tooassist som du skapar en elastisk pool optimerad för dina specifika databaser.</span><span class="sxs-lookup"><span data-stu-id="06522-173">These recommendations are intended tooassist you in creating an elastic pool optimized for your specific databases.</span></span>

![dynamiska rekommendationer](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="06522-175">Hantera och övervaka en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="06522-176">Du kan använda hello Azure portal toomonitor och hantera en elastisk pool och hello-databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-176">You can use hello Azure portal toomonitor and manage an elastic pool and hello databases in hello pool.</span></span> <span data-ttu-id="06522-177">Du kan övervaka hello-belastningen för en elastisk pool och hello databaser i poolen från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="06522-177">From hello portal, you can monitor hello utilization of an elastic pool and hello databases within that pool.</span></span> <span data-ttu-id="06522-178">Du kan också göra en uppsättning ändringar tooyour elastisk pool och skicka alla ändringar på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="06522-178">You can also make a set of changes tooyour elastic pool and submit all changes at hello same time.</span></span> <span data-ttu-id="06522-179">De här förändringarna innefattar att lägga till eller ta bort databaser, ändra inställningarna elastisk pool eller ändra databasinställningarna.</span><span class="sxs-lookup"><span data-stu-id="06522-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="06522-180">hello följande bild visar ett exempel elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-180">hello following graphic shows an example elastic pool.</span></span> <span data-ttu-id="06522-181">hello-vyn innehåller:</span><span class="sxs-lookup"><span data-stu-id="06522-181">hello view includes:</span></span>

*  <span data-ttu-id="06522-182">Diagram för övervakning av resursanvändningen för både hello elastisk pool och hello databaser som ingår i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-182">Charts for monitoring resource usage of both hello elastic pool and hello databases contained in hello pool.</span></span>
*  <span data-ttu-id="06522-183">Hej **konfigurera** pool knappen toomake ändras toohello elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-183">hello **Configure** pool button toomake changes toohello elastic pool.</span></span>
*  <span data-ttu-id="06522-184">Hej **Skapa databas** skapar du en databas och lägger till den toohello aktuella elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="06522-184">hello **Create database** button that creates a database and adds it toohello current elastic pool.</span></span>
*  <span data-ttu-id="06522-185">Elastiska jobb som hjälper dig hantera stort antal databaser genom att köra Transact-SQL-skript mot alla databaser i en lista.</span><span class="sxs-lookup"><span data-stu-id="06522-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Pool-vy][2]

<span data-ttu-id="06522-187">Du kan gå tooa viss poolen toosee dess resursutnyttjande.</span><span class="sxs-lookup"><span data-stu-id="06522-187">You can go tooa particular pool toosee its resource utilization.</span></span> <span data-ttu-id="06522-188">Hello poolen är som standard konfigurerade tooshow lagring och eDTU-användning för hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="06522-188">By default, hello pool is configured tooshow storage and eDTU usage for hello last hour.</span></span> <span data-ttu-id="06522-189">hello diagrammet kan vara konfigurerade tooshow olika mätvärden via olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="06522-189">hello chart can be configured tooshow different metrics over various time windows.</span></span>

1. <span data-ttu-id="06522-190">Välj en elastisk pool toowork med.</span><span class="sxs-lookup"><span data-stu-id="06522-190">Select an elastic pool toowork with.</span></span>
2. <span data-ttu-id="06522-191">Under **elastisk Pool övervakning** är ett diagram med etiketten **resursutnyttjande**.</span><span class="sxs-lookup"><span data-stu-id="06522-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="06522-192">Klicka på hello diagram.</span><span class="sxs-lookup"><span data-stu-id="06522-192">Click hello chart.</span></span>

    ![Övervakning av elastisk pool][3]

    <span data-ttu-id="06522-194">Hej **mått** blad öppnas, visar en detaljerad vy av hello anges mätvärden via hello angivna tidsfönstret.</span><span class="sxs-lookup"><span data-stu-id="06522-194">hello **Metric** blade opens, showing a detailed view of hello specified metrics over hello specified time window.</span></span>   

    ![Bladet Mått][9]

### <a name="toocustomize-hello-chart-display"></a><span data-ttu-id="06522-196">toocustomize hello diagramvyn</span><span class="sxs-lookup"><span data-stu-id="06522-196">toocustomize hello chart display</span></span>

<span data-ttu-id="06522-197">Du kan redigera hello diagram och hello mått bladet toodisplay andra mått som CPU-procent, data IO-procent och loggen IO-procent som används.</span><span class="sxs-lookup"><span data-stu-id="06522-197">You can edit hello chart and hello metric blade toodisplay other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="06522-198">På mått hello-bladet klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="06522-198">On hello metric blade, click **Edit**.</span></span>

    ![Klicka på Redigera][6]

2. <span data-ttu-id="06522-200">I hello **redigera diagram** bladet Välj ett tidsintervall (efter timme, dag, eller föregående vecka), eller klicka på **anpassade** tooselect i hello vara ett datum senaste två veckorna.</span><span class="sxs-lookup"><span data-stu-id="06522-200">In hello **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** tooselect any date range in hello last two weeks.</span></span> <span data-ttu-id="06522-201">Välj hello diagramtyp (stapel- eller rad), och välj sedan hello resurser toomonitor.</span><span class="sxs-lookup"><span data-stu-id="06522-201">Select hello chart type (bar or line), then select hello resources toomonitor.</span></span>

   > [!Note]
   > <span data-ttu-id="06522-202">Endast mått med samma enhet som kan visas i hello hello diagram på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="06522-202">Only metrics with hello same unit of measure can be displayed in hello chart at hello same time.</span></span> <span data-ttu-id="06522-203">Till exempel om du väljer ”eDTU i procent” kan du bara välja andra mått med procentandel som hello enhet.</span><span class="sxs-lookup"><span data-stu-id="06522-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as hello unit of measure.</span></span>
   >

    ![Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="06522-205">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="06522-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="06522-206">Hantera och övervaka databaser i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="06522-207">Enskilda databaser kan också övervakas för potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="06522-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="06522-208">Under **elastisk databas övervakning**, det finns ett diagram som visar mått för fem databaser.</span><span class="sxs-lookup"><span data-stu-id="06522-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="06522-209">Som standard hello diagrammet visar hello övre 5 databaser i poolen hello av genomsnittlig eDTU-användning i hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="06522-209">By default, hello chart displays hello top 5 databases in hello pool by average eDTU usage in hello past hour.</span></span> <span data-ttu-id="06522-210">Klicka på hello diagram.</span><span class="sxs-lookup"><span data-stu-id="06522-210">Click hello chart.</span></span>

    ![Övervakning av elastisk pool][4]

2. <span data-ttu-id="06522-212">Hej **databasen resursutnyttjande** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="06522-212">hello **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="06522-213">Detta ger en detaljerad vy av hello databasanvändningen i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-213">This provides a detailed view of hello database usage in hello pool.</span></span> <span data-ttu-id="06522-214">Använder hello rutnät i hello nedre delen av hello-bladet, kan du välja alla databaser i hello poolen toodisplay användningen i hello diagram (upp too5 databaser).</span><span class="sxs-lookup"><span data-stu-id="06522-214">Using hello grid in hello lower part of hello blade, you can select any databases in hello pool toodisplay its usage in hello chart (up too5 databases).</span></span> <span data-ttu-id="06522-215">Du kan också anpassa hello mått och tid fönster som visas i diagrammet hello genom att klicka på **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="06522-215">You can also customize hello metrics and time window displayed in hello chart by clicking **Edit chart**.</span></span>

    ![-Databasresursbladet användning][8]

### <a name="toocustomize-hello-view"></a><span data-ttu-id="06522-217">toocustomize hello vy</span><span class="sxs-lookup"><span data-stu-id="06522-217">toocustomize hello view</span></span>

1. <span data-ttu-id="06522-218">I hello **databasen resursutnyttjande** bladet, klickar du på **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="06522-218">In hello **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Klicka på Redigera diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="06522-220">I hello **redigera** diagram bladet väljer du ett tidsintervall (efter timme eller senaste 24 timmarna) eller klicka på **anpassade** tooselect en annan dag i hello efter två veckor toodisplay.</span><span class="sxs-lookup"><span data-stu-id="06522-220">In hello **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** tooselect a different day in hello past 2 weeks toodisplay.</span></span>

    ![Klicka på anpassad](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="06522-222">Klicka på hello **databaser genom att jämföra** listrutan tooselect olika mått toouse när databaser.</span><span class="sxs-lookup"><span data-stu-id="06522-222">Click hello **Compare databases by** dropdown tooselect a different metric toouse when comparing databases.</span></span>

    ![Redigera hello diagram](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a><span data-ttu-id="06522-224">tooselect databaser toomonitor</span><span class="sxs-lookup"><span data-stu-id="06522-224">tooselect databases toomonitor</span></span>

<span data-ttu-id="06522-225">I listan för hello databasen hello **databasen resursutnyttjande** bladet hittar du viss databaser genom att titta igenom hello sidorna i hello listan eller genom att skriva hello namnet på en databas.</span><span class="sxs-lookup"><span data-stu-id="06522-225">In hello database list in hello **Database Resource Utilization** blade, you can find particular databases by looking through hello pages in hello list or by typing in hello name of a database.</span></span> <span data-ttu-id="06522-226">Använd hello kryssrutan tooselect hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="06522-226">Use hello checkbox tooselect hello database.</span></span>

![Sök efter databaser toomonitor][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="06522-228">Lägg till en resurs för avisering tooan elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-228">Add an alert tooan elastic pool resource</span></span>

<span data-ttu-id="06522-229">Du kan lägga till regler tooan elastisk pool som skickar e-post toopeople eller varning strängar tooURL slutpunkter när hello elastisk pool träffar ett tröskelvärde för användning som du skapat.</span><span class="sxs-lookup"><span data-stu-id="06522-229">You can add rules tooan elastic pool that send email toopeople or alert strings tooURL endpoints when hello elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="06522-230">**tooadd en avisering tooany resurs:**</span><span class="sxs-lookup"><span data-stu-id="06522-230">**tooadd an alert tooany resource:**</span></span>

1. <span data-ttu-id="06522-231">Klicka på hello **resursutnyttjande** diagram tooopen hello **mått** bladet, klickar du på **Lägg till avisering**, och fyller sedan hello informationen i hello **lägga till en avisering regeln** bladet (**resurs** konfigureras automatiskt toobe hello pool du arbetar med).</span><span class="sxs-lookup"><span data-stu-id="06522-231">Click hello **Resource utilization** chart tooopen hello **Metric** blade, click **Add alert**, and then fill out hello information in hello **Add an alert rule** blade (**Resource** is automatically set up toobe hello pool you're working with).</span></span>
2. <span data-ttu-id="06522-232">Ange en **namn** och **beskrivning** som identifierar hello avisering tooyou och hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="06522-232">Type a **Name** and **Description** that identifies hello alert tooyou and hello recipients.</span></span>
3. <span data-ttu-id="06522-233">Välj en **mått** som du vill tooalert hello-listan.</span><span class="sxs-lookup"><span data-stu-id="06522-233">Choose a **Metric** that you want tooalert from hello list.</span></span>

    <span data-ttu-id="06522-234">hello diagrammet visar dynamiskt resursanvändningen för den mått toohelp du väljer ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="06522-234">hello chart dynamically shows resource utilization for that metric toohelp you choose a threshold.</span></span>

4. <span data-ttu-id="06522-235">Välj en **villkoret** (större än mindre än osv) och en **tröskelvärdet**.</span><span class="sxs-lookup"><span data-stu-id="06522-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="06522-236">Välj en **Period** som hello mått regeln måste uppfyllas innan hello avisering utlösare.</span><span class="sxs-lookup"><span data-stu-id="06522-236">Choose a **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span>
6. <span data-ttu-id="06522-237">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="06522-237">Click **OK**.</span></span>

<span data-ttu-id="06522-238">Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="06522-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="06522-239">Flytta en databas till en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="06522-240">Du kan lägga till eller ta bort databaser från en befintlig adresspool.</span><span class="sxs-lookup"><span data-stu-id="06522-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="06522-241">hello databaser kan vara i andra pooler.</span><span class="sxs-lookup"><span data-stu-id="06522-241">hello databases can be in other pools.</span></span> <span data-ttu-id="06522-242">Men du kan bara lägga till databaser som finns på hello samma logiska server.</span><span class="sxs-lookup"><span data-stu-id="06522-242">However, you can only add databases that are on hello same logical server.</span></span>

1. <span data-ttu-id="06522-243">Hello-bladet för hello poolen, under **elastiska databaser** klickar du på **konfigurera pool**.</span><span class="sxs-lookup"><span data-stu-id="06522-243">In hello blade for hello pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Klicka på Konfigurera pool][1]

2. <span data-ttu-id="06522-245">I hello **konfigurera pool** bladet, klickar du på **lägga till toopool**.</span><span class="sxs-lookup"><span data-stu-id="06522-245">In hello **Configure pool** blade, click **Add toopool**.</span></span>

    ![Klicka på Lägg till toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="06522-247">I hello **lägga till databaser** bladet, Välj hello databasen eller databaser tooadd toohello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-247">In hello **Add databases** blade, select hello database or databases tooadd toohello pool.</span></span> <span data-ttu-id="06522-248">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="06522-248">Then click **Select**.</span></span>

    ![Välj databaser tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="06522-250">Hej **konfigurera pool** bladet nu visar hello markerade toobe som lagts till med statusen för databasen**väntande**.</span><span class="sxs-lookup"><span data-stu-id="06522-250">hello **Configure pool** blade now lists hello database you selected toobe added, with its status set too**Pending**.</span></span>

    ![Väntande pool-tillägg](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="06522-252">I hello **konfigurera pool bladet**, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="06522-252">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="06522-254">Flytta en databas från en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="06522-255">I hello **konfigurera pool** bladet, Välj hello databasen eller databaser tooremove.</span><span class="sxs-lookup"><span data-stu-id="06522-255">In hello **Configure pool** blade, select hello database or databases tooremove.</span></span>

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="06522-257">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="06522-257">Click **Remove from pool**.</span></span>

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="06522-259">Hej **konfigurera pool** bladet nu visar hello markerade toobe bort med statusen för databasen**väntande**.</span><span class="sxs-lookup"><span data-stu-id="06522-259">hello **Configure pool** blade now lists hello database you selected toobe removed with its status set too**Pending**.</span></span>

    ![Förhandsgranska databasen tillägg och borttagning](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="06522-261">I hello **konfigurera pool bladet**, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="06522-261">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="06522-263">Ändra inställningar för prestanda för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="06522-264">När du övervakar hello resursanvändningen för en elastisk pool kan du märka att några justeringar behövs.</span><span class="sxs-lookup"><span data-stu-id="06522-264">As you monitor hello resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="06522-265">Hello poolen behöver kanske en ändring i hello prestanda eller lagring gränser.</span><span class="sxs-lookup"><span data-stu-id="06522-265">Maybe hello pool needs a change in hello performance or storage limits.</span></span> <span data-ttu-id="06522-266">Du vill eventuellt toochange hello databasinställningarna i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-266">Possibly you want toochange hello database settings in hello pool.</span></span> <span data-ttu-id="06522-267">Du kan ändra hello installationen av hello poolen på varje gång tooget hello bästa balansen mellan prestanda och kostnad.</span><span class="sxs-lookup"><span data-stu-id="06522-267">You can change hello setup of hello pool at any time tooget hello best balance of performance and cost.</span></span> <span data-ttu-id="06522-268">Se [när en elastisk pool kan användas?](sql-database-elastic-pool.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="06522-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="06522-269">toochange hello edtu: er eller lagring gränser per pool och edtu: er per databas:</span><span class="sxs-lookup"><span data-stu-id="06522-269">toochange hello eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="06522-270">Öppna hello **konfigurera pool** bladet.</span><span class="sxs-lookup"><span data-stu-id="06522-270">Open hello **Configure pool** blade.</span></span>

    <span data-ttu-id="06522-271">Under **elastisk poolinställningar**, använda antingen skjutreglaget toochange hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="06522-271">Under **elastic pool settings**, use either slider toochange hello pool settings.</span></span>

    ![Resursutnyttjande elastisk pool](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="06522-273">När hello inställningen ändras, visar hello visa hello beräknade månadskostnaden för hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="06522-273">When hello setting changes, hello display shows hello estimated monthly cost of hello change.</span></span>

    ![Uppdatera en elastisk pool och nya månadskostnaden](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="06522-275">Svarstiden för åtgärder elastisk pool</span><span class="sxs-lookup"><span data-stu-id="06522-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="06522-276">Ändra hello min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.</span><span class="sxs-lookup"><span data-stu-id="06522-276">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="06522-277">Ändra hello edtu: er per pool beror på hello totala mängden diskutrymme som används av alla databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="06522-277">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="06522-278">Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB.</span><span class="sxs-lookup"><span data-stu-id="06522-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="06522-279">Om hello totalt utrymme som används av alla databaser i poolen hello är exempelvis 200 GB, och sedan hello förväntade svarstid för att ändra hello pool-eDTU per pool är tre timmar eller mindre.</span><span class="sxs-lookup"><span data-stu-id="06522-279">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06522-280">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06522-280">Next steps</span></span>

- <span data-ttu-id="06522-281">toounderstand är, finns i en elastisk pool [SQL Database-elastisk pool](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="06522-281">toounderstand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="06522-282">Anvisningar om hur du använder elastiska pooler finns [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="06522-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="06522-283">toouse elastiska jobb toorun Transact-SQL-skript mot valfritt antal databaser i hello pool, se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="06522-283">toouse elastic jobs toorun Transact-SQL scripts against any number of databases in hello pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="06522-284">tooquery över valfritt antal databaser i hello pool, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="06522-284">tooquery across any number of databases in hello pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="06522-285">Transaktioner valfritt antal databaser i hello pool finns [elastisk transaktioner](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="06522-285">For transactions any number of databases in hello pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


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
