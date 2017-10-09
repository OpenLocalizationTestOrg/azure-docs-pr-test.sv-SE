---
title: 'Azure-portalen: SQL Database geo-replikering | Microsoft Docs'
description: "Konfigurera geo-replikering för Azure SQL Database i hello Azure-portalen och initiera redundans"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: 09cbbdb040f36c42593e3be87ce6db2238f36656
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a><span data-ttu-id="5b47a-103">Konfigurera aktiv geo-replikering för Azure SQL Database i hello Azure-portalen och initiera redundans</span><span class="sxs-lookup"><span data-stu-id="5b47a-103">Configure active geo-replication for Azure SQL Database in hello Azure portal and initiate failover</span></span>

<span data-ttu-id="5b47a-104">Den här artikeln beskrivs hur du tooconfigure aktiv geo-replikering för SQL-databas i hello [Azure-portalen](http://portal.azure.com) och tooinitiate redundans.</span><span class="sxs-lookup"><span data-stu-id="5b47a-104">This article shows you how tooconfigure active geo-replication for SQL Database in hello [Azure portal](http://portal.azure.com) and tooinitiate failover.</span></span>

<span data-ttu-id="5b47a-105">tooinitiate redundans med hello Azure-portalen finns [starta en planerad eller oplanerad växling för Azure SQL Database med hello Azure-portalen](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5b47a-105">tooinitiate failover with hello Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with hello Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="5b47a-106">tooconfigure aktiv geo-replikering med hjälp av hello Azure-portalen, behöver du hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="5b47a-106">tooconfigure active geo-replication by using hello Azure portal, you need hello following resource:</span></span>

* <span data-ttu-id="5b47a-107">En Azure SQL database: hello primära databasen som du vill tooreplicate tooa olika geografiska region.</span><span class="sxs-lookup"><span data-stu-id="5b47a-107">An Azure SQL database: hello primary database that you want tooreplicate tooa different geographical region.</span></span>

> [!Note]
<span data-ttu-id="5b47a-108">Aktiv geo-replikering måste vara mellan databaser i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5b47a-108">Active geo-replication must be between databases in hello same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="5b47a-109">Lägg till en sekundär databas</span><span class="sxs-lookup"><span data-stu-id="5b47a-109">Add a secondary database</span></span>
<span data-ttu-id="5b47a-110">hello skapa följande en ny sekundär databas i ett partnerskap geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="5b47a-110">hello following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="5b47a-111">tooadd en sekundär databas måste du vara hello prenumerationsägare eller Medägare.</span><span class="sxs-lookup"><span data-stu-id="5b47a-111">tooadd a secondary database, you must be hello subscription owner or co-owner.</span></span>

<span data-ttu-id="5b47a-112">hello sekundära databasen har samma namn som den primära databasen i hello hello och har, som standard hello samma servicenivå.</span><span class="sxs-lookup"><span data-stu-id="5b47a-112">hello secondary database has hello same name as hello primary database and has, by default, hello same service level.</span></span> <span data-ttu-id="5b47a-113">hello sekundära databasen kan vara en enskild databas eller en databas i en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="5b47a-113">hello secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="5b47a-114">Mer information finns i [tjänstnivåer](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="5b47a-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="5b47a-115">Hello sekundära skapas och dirigeras börjar data när replikering från hello primära toohello nya sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="5b47a-115">After hello secondary is created and seeded, data begins replicating from hello primary database toohello new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="5b47a-116">Hello kommandot misslyckas om hello partner databas redan finns (till exempel till följd av avslutar relationen för en tidigare geo-replikering).</span><span class="sxs-lookup"><span data-stu-id="5b47a-116">If hello partner database already exists (for example, as a result of terminating a previous geo-replication relationship) hello command fails.</span></span>
> 

1. <span data-ttu-id="5b47a-117">I hello [Azure-portalen](http://portal.azure.com), bläddra toohello databasen som du vill tooset för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="5b47a-117">In hello [Azure portal](http://portal.azure.com), browse toohello database that you want tooset up for geo-replication.</span></span>
2. <span data-ttu-id="5b47a-118">Hello SQL-databas på sidan Välj **georeplikering**, och välj sedan hello region toocreate hello sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-118">On hello SQL database page, select **geo-replication**, and then select hello region toocreate hello secondary database.</span></span> <span data-ttu-id="5b47a-119">Du kan välja en annan region än hello region värd hello primära databasen, men vi rekommenderar hello [parad region](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="5b47a-119">You can select any region other than hello region hosting hello primary database, but we recommend hello [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Konfigurera geo-replikering](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="5b47a-121">Välj eller konfigurera hello-servern och prisnivå för hello sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-121">Select or configure hello server and pricing tier for hello secondary database.</span></span>
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="5b47a-123">Du kan också kan du lägga till en sekundär tooan elastisk databaspool.</span><span class="sxs-lookup"><span data-stu-id="5b47a-123">Optionally, you can add a secondary database tooan elastic pool.</span></span> <span data-ttu-id="5b47a-124">toocreate hello sekundär databas i en pool, klickar du på **elastisk pool** och välj en pool på hello målserver.</span><span class="sxs-lookup"><span data-stu-id="5b47a-124">toocreate hello secondary database in a pool, click **elastic pool** and select a pool on hello target server.</span></span> <span data-ttu-id="5b47a-125">En pool måste redan finnas på hello målserver.</span><span class="sxs-lookup"><span data-stu-id="5b47a-125">A pool must already exist on hello target server.</span></span> <span data-ttu-id="5b47a-126">Det här arbetsflödet kan inte skapa en pool.</span><span class="sxs-lookup"><span data-stu-id="5b47a-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="5b47a-127">Klicka på **skapa** tooadd hello sekundär.</span><span class="sxs-lookup"><span data-stu-id="5b47a-127">Click **Create** tooadd hello secondary.</span></span>
6. <span data-ttu-id="5b47a-128">hello sekundär databas skapas och hello seeding processen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-128">hello secondary database is created and hello seeding process begins.</span></span>
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="5b47a-130">När hello seeding processen är klar visas dess status hello sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-130">When hello seeding process is complete, hello secondary database displays its status.</span></span>
   
    ![Seeding klar](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="5b47a-132">Initiera redundans</span><span class="sxs-lookup"><span data-stu-id="5b47a-132">Initiate a failover</span></span>

<span data-ttu-id="5b47a-133">hello sekundära databasen kan vara avstängda toobecome hello primära.</span><span class="sxs-lookup"><span data-stu-id="5b47a-133">hello secondary database can be switched toobecome hello primary.</span></span>  

1. <span data-ttu-id="5b47a-134">I hello [Azure-portalen](http://portal.azure.com), bläddra toohello primära databasen hello geo-replikering tillsammans.</span><span class="sxs-lookup"><span data-stu-id="5b47a-134">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="5b47a-135">På bladet för hello SQL-databas, Välj **alla inställningar** > **georeplikering**.</span><span class="sxs-lookup"><span data-stu-id="5b47a-135">On hello SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="5b47a-136">I hello **SEKUNDÄRSERVRAR** listan, Välj hello-databas som du vill toobecome hello nya primära och på **redundans**.</span><span class="sxs-lookup"><span data-stu-id="5b47a-136">In hello **SECONDARIES** list, select hello database you want toobecome hello new primary and click **Failover**.</span></span>
   
    ![växling vid fel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="5b47a-138">Klicka på **Ja** toobegin hello redundans.</span><span class="sxs-lookup"><span data-stu-id="5b47a-138">Click **Yes** toobegin hello failover.</span></span>

<span data-ttu-id="5b47a-139">hello växlar omedelbart hello sekundär databas i hello primära rollen.</span><span class="sxs-lookup"><span data-stu-id="5b47a-139">hello command immediately switches hello secondary database into hello primary role.</span></span> 

<span data-ttu-id="5b47a-140">Det finns en kort period under vilken båda databaserna är inte tillgängliga (på hello ordning 0 too25 sekunder) när hello roller växlas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-140">There is a short period during which both databases are unavailable (on hello order of 0 too25 seconds) while hello roles are switched.</span></span> <span data-ttu-id="5b47a-141">Om hello primära databasen har flera sekundära databaser, hello kommandot automatiskt hello Omkonfigurerar andra sekundärservrar tooconnect toohello nya primära servern.</span><span class="sxs-lookup"><span data-stu-id="5b47a-141">If hello primary database has multiple secondary databases, hello command automatically reconfigures hello other secondaries tooconnect toohello new primary.</span></span> <span data-ttu-id="5b47a-142">hello hela åtgärden bör ta mindre än en minut toocomplete under normala omständigheter.</span><span class="sxs-lookup"><span data-stu-id="5b47a-142">hello entire operation should take less than a minute toocomplete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="5b47a-143">Det här kommandot har utformats för snabb återställning av databasen hello vid ett strömavbrott.</span><span class="sxs-lookup"><span data-stu-id="5b47a-143">This command is designed for quick recovery of hello database in case of an outage.</span></span> <span data-ttu-id="5b47a-144">Utlöser redundans utan datasynkronisering (tvingas redundans).</span><span class="sxs-lookup"><span data-stu-id="5b47a-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="5b47a-145">Om hello primära är online och acceptera transaktioner när hello-kommandot har utfärdats dataförluster kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="5b47a-145">If hello primary is online and committing transactions when hello command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="5b47a-146">Ta bort sekundära databas</span><span class="sxs-lookup"><span data-stu-id="5b47a-146">Remove secondary database</span></span>
<span data-ttu-id="5b47a-147">Den här åtgärden avbryter permanent hello replikering toohello sekundära databas och ändringar hello roll hello sekundära tooa vanlig skrivskyddad databas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-147">This operation permanently terminates hello replication toohello secondary database, and changes hello role of hello secondary tooa regular read-write database.</span></span> <span data-ttu-id="5b47a-148">Om hello anslutningen toohello sekundära databasen är skadad, hello kommando lyckas men hello sekundära har inte blir oskyddad tills när anslutningen har återställts.</span><span class="sxs-lookup"><span data-stu-id="5b47a-148">If hello connectivity toohello secondary database is broken, hello command succeeds but hello secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="5b47a-149">I hello [Azure-portalen](http://portal.azure.com), bläddra toohello primära databasen hello geo-replikering tillsammans.</span><span class="sxs-lookup"><span data-stu-id="5b47a-149">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="5b47a-150">Hello SQL-databas på sidan Välj **georeplikering**.</span><span class="sxs-lookup"><span data-stu-id="5b47a-150">On hello SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="5b47a-151">I hello **SEKUNDÄRSERVRAR** listan, Välj hello-databasen som du vill använda tooremove från hello geo-replikering partnerskap.</span><span class="sxs-lookup"><span data-stu-id="5b47a-151">In hello **SECONDARIES** list, select hello database you want tooremove from hello geo-replication partnership.</span></span>
4. <span data-ttu-id="5b47a-152">Klicka på **Replikeringsstopp**.</span><span class="sxs-lookup"><span data-stu-id="5b47a-152">Click **Stop Replication**.</span></span>
   
    ![Ta bort sekundär](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="5b47a-154">En bekräftelse öppnas.</span><span class="sxs-lookup"><span data-stu-id="5b47a-154">A confirmation window opens.</span></span> <span data-ttu-id="5b47a-155">Klicka på **Ja** tooremove hello-databas från hello geo-replikering partnerskap.</span><span class="sxs-lookup"><span data-stu-id="5b47a-155">Click **Yes** tooremove hello database from hello geo-replication partnership.</span></span> <span data-ttu-id="5b47a-156">(Ange det tooa oskyddad databas inte en del av all replikering.)</span><span class="sxs-lookup"><span data-stu-id="5b47a-156">(Set it tooa read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b47a-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b47a-157">Next steps</span></span>
* <span data-ttu-id="5b47a-158">toolearn mer information om aktiv geo-replikering, se [aktiv geo-replikering](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5b47a-158">toolearn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="5b47a-159">En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="5b47a-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

