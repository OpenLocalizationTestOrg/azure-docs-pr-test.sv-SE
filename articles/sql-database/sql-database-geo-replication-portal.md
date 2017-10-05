---
title: 'Azure-portalen: SQL Database geo-replikering | Microsoft Docs'
description: "Konfigurera geo-replikering för Azure SQL Database i Azure-portalen och initiera redundans"
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
ms.openlocfilehash: db90fad2fe397f0c8466db6bdc1bd8c8d1cf8f15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a><span data-ttu-id="3123c-103">Konfigurera aktiv geo-replikering för Azure SQL Database i Azure-portalen och initiera redundans</span><span class="sxs-lookup"><span data-stu-id="3123c-103">Configure active geo-replication for Azure SQL Database in the Azure portal and initiate failover</span></span>

<span data-ttu-id="3123c-104">Den här artikeln visar hur du konfigurerar aktiv geo-replikering för SQL-databasen i den [Azure-portalen](http://portal.azure.com) och för att initiera växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3123c-104">This article shows you how to configure active geo-replication for SQL Database in the [Azure portal](http://portal.azure.com) and to initiate failover.</span></span>

<span data-ttu-id="3123c-105">Om du vill initiera redundans med Azure-portalen finns [starta en planerad eller oplanerad växling för Azure SQL Database med Azure-portalen](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3123c-105">To initiate failover with the Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with the Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="3123c-106">Om du vill konfigurera aktiv geo-replikering med hjälp av Azure portal, behöver du följande resurs:</span><span class="sxs-lookup"><span data-stu-id="3123c-106">To configure active geo-replication by using the Azure portal, you need the following resource:</span></span>

* <span data-ttu-id="3123c-107">En Azure SQL database: den primära databasen som du vill replikera till en annan geografisk region.</span><span class="sxs-lookup"><span data-stu-id="3123c-107">An Azure SQL database: The primary database that you want to replicate to a different geographical region.</span></span>

> [!Note]
<span data-ttu-id="3123c-108">Aktiv geo-replikering måste vara mellan databaser i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3123c-108">Active geo-replication must be between databases in the same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="3123c-109">Lägg till en sekundär databas</span><span class="sxs-lookup"><span data-stu-id="3123c-109">Add a secondary database</span></span>
<span data-ttu-id="3123c-110">Följande steg kan du skapa en ny sekundär databas i ett partnerskap geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-110">The following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="3123c-111">Du måste vara prenumerationsägaren eller Medägare för att lägga till en sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="3123c-111">To add a secondary database, you must be the subscription owner or co-owner.</span></span>

<span data-ttu-id="3123c-112">Den sekundära databasen har samma namn som den primära databasen och har samma servicenivåer som standard.</span><span class="sxs-lookup"><span data-stu-id="3123c-112">The secondary database has the same name as the primary database and has, by default, the same service level.</span></span> <span data-ttu-id="3123c-113">Den sekundära databasen kan vara en enskild databas eller en databas i en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="3123c-113">The secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="3123c-114">Mer information finns i [tjänstnivåer](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="3123c-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="3123c-115">När sekundärt skapas och dirigeras, börjar data replikeras från den primära databasen till den nya sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="3123c-115">After the secondary is created and seeded, data begins replicating from the primary database to the new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="3123c-116">Kommandot misslyckas om partner-databas redan finns (till exempel till följd av avslutar relationen för en tidigare geo-replikering).</span><span class="sxs-lookup"><span data-stu-id="3123c-116">If the partner database already exists (for example, as a result of terminating a previous geo-replication relationship) the command fails.</span></span>
> 

1. <span data-ttu-id="3123c-117">I den [Azure-portalen](http://portal.azure.com), bläddra till den databas som du vill konfigurera för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-117">In the [Azure portal](http://portal.azure.com), browse to the database that you want to set up for geo-replication.</span></span>
2. <span data-ttu-id="3123c-118">På sidan SQL-databasen väljer **georeplikering**, och välj sedan regionen för att skapa den sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="3123c-118">On the SQL database page, select **geo-replication**, and then select the region to create the secondary database.</span></span> <span data-ttu-id="3123c-119">Du kan välja en region än den region som är värd för den primära databasen, men vi rekommenderar den [parad region](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="3123c-119">You can select any region other than the region hosting the primary database, but we recommend the [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Konfigurera geo-replikering](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="3123c-121">Välj eller konfigurera servern och prisnivå för den sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="3123c-121">Select or configure the server and pricing tier for the secondary database.</span></span>
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="3123c-123">Alternativt kan du lägga till en sekundär databas till en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="3123c-123">Optionally, you can add a secondary database to an elastic pool.</span></span> <span data-ttu-id="3123c-124">Klicka för att skapa den sekundära databasen i en pool **elastisk pool** och välj en pool på målservern.</span><span class="sxs-lookup"><span data-stu-id="3123c-124">To create the secondary database in a pool, click **elastic pool** and select a pool on the target server.</span></span> <span data-ttu-id="3123c-125">En pool måste redan finnas på målservern.</span><span class="sxs-lookup"><span data-stu-id="3123c-125">A pool must already exist on the target server.</span></span> <span data-ttu-id="3123c-126">Det här arbetsflödet kan inte skapa en pool.</span><span class="sxs-lookup"><span data-stu-id="3123c-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="3123c-127">Klicka på **skapa** att lägga till sekundärt.</span><span class="sxs-lookup"><span data-stu-id="3123c-127">Click **Create** to add the secondary.</span></span>
6. <span data-ttu-id="3123c-128">Den sekundära databasen skapas och seeding-processen börjar.</span><span class="sxs-lookup"><span data-stu-id="3123c-128">The secondary database is created and the seeding process begins.</span></span>
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="3123c-130">När seeding processen är klar visas den sekundära databasen dess status.</span><span class="sxs-lookup"><span data-stu-id="3123c-130">When the seeding process is complete, the secondary database displays its status.</span></span>
   
    ![Seeding klar](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="3123c-132">Initiera redundans</span><span class="sxs-lookup"><span data-stu-id="3123c-132">Initiate a failover</span></span>

<span data-ttu-id="3123c-133">Den sekundära databasen kan växlas till primärt.</span><span class="sxs-lookup"><span data-stu-id="3123c-133">The secondary database can be switched to become the primary.</span></span>  

1. <span data-ttu-id="3123c-134">I den [Azure-portalen](http://portal.azure.com), bläddra till den primära databasen i partnerskap geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-134">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="3123c-135">På bladet SQL-databasen väljer **alla inställningar** > **georeplikering**.</span><span class="sxs-lookup"><span data-stu-id="3123c-135">On the SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="3123c-136">I den **SEKUNDÄRSERVRAR** väljer du den databas du vill bli den nya primärt och på **redundans**.</span><span class="sxs-lookup"><span data-stu-id="3123c-136">In the **SECONDARIES** list, select the database you want to become the new primary and click **Failover**.</span></span>
   
    ![växling vid fel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="3123c-138">Klicka på **Ja** starta redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="3123c-138">Click **Yes** to begin the failover.</span></span>

<span data-ttu-id="3123c-139">Kommandot växlar omedelbart den sekundära databasen till den primära rollen.</span><span class="sxs-lookup"><span data-stu-id="3123c-139">The command immediately switches the secondary database into the primary role.</span></span> 

<span data-ttu-id="3123c-140">Det finns en kort period under vilken båda databaserna är inte tillgänglig (terabyte 0 till 25 sekunder) när rollerna växlas.</span><span class="sxs-lookup"><span data-stu-id="3123c-140">There is a short period during which both databases are unavailable (on the order of 0 to 25 seconds) while the roles are switched.</span></span> <span data-ttu-id="3123c-141">Om den primära databasen har flera sekundära databaser, automatiskt de andra sekundärservrar för att ansluta till den nya primärt automatiskt om kommandot.</span><span class="sxs-lookup"><span data-stu-id="3123c-141">If the primary database has multiple secondary databases, the command automatically reconfigures the other secondaries to connect to the new primary.</span></span> <span data-ttu-id="3123c-142">Hela åtgärden bör ta mindre än en minut att slutföra under normala omständigheter.</span><span class="sxs-lookup"><span data-stu-id="3123c-142">The entire operation should take less than a minute to complete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="3123c-143">Det här kommandot har utformats för snabb återställning av databasen vid ett strömavbrott.</span><span class="sxs-lookup"><span data-stu-id="3123c-143">This command is designed for quick recovery of the database in case of an outage.</span></span> <span data-ttu-id="3123c-144">Utlöser redundans utan datasynkronisering (tvingas redundans).</span><span class="sxs-lookup"><span data-stu-id="3123c-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="3123c-145">Om den primära servern är online och acceptera transaktioner när kommandot utfärdas dataförluster kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="3123c-145">If the primary is online and committing transactions when the command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="3123c-146">Ta bort sekundära databas</span><span class="sxs-lookup"><span data-stu-id="3123c-146">Remove secondary database</span></span>
<span data-ttu-id="3123c-147">Den här åtgärden permanent avslutar replikering till den sekundära databasen och ändras på sekundärt rollen till en vanlig skrivskyddad databas.</span><span class="sxs-lookup"><span data-stu-id="3123c-147">This operation permanently terminates the replication to the secondary database, and changes the role of the secondary to a regular read-write database.</span></span> <span data-ttu-id="3123c-148">Om anslutningen till den sekundära databasen är bruten kommandot lyckas men sekundära har inte blir oskyddad förrän anslutningen har återställts.</span><span class="sxs-lookup"><span data-stu-id="3123c-148">If the connectivity to the secondary database is broken, the command succeeds but the secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="3123c-149">I den [Azure-portalen](http://portal.azure.com), bläddra till den primära databasen i partnerskap geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-149">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="3123c-150">På sidan SQL-databasen väljer **georeplikering**.</span><span class="sxs-lookup"><span data-stu-id="3123c-150">On the SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="3123c-151">I den **SEKUNDÄRSERVRAR** Markera databasen som du vill ta bort från partnerskap för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-151">In the **SECONDARIES** list, select the database you want to remove from the geo-replication partnership.</span></span>
4. <span data-ttu-id="3123c-152">Klicka på **Replikeringsstopp**.</span><span class="sxs-lookup"><span data-stu-id="3123c-152">Click **Stop Replication**.</span></span>
   
    ![Ta bort sekundär](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="3123c-154">En bekräftelse öppnas.</span><span class="sxs-lookup"><span data-stu-id="3123c-154">A confirmation window opens.</span></span> <span data-ttu-id="3123c-155">Klicka på **Ja** att ta bort databasen från partnerskap för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3123c-155">Click **Yes** to remove the database from the geo-replication partnership.</span></span> <span data-ttu-id="3123c-156">(Ange det till en skrivskyddad databas inte en del av all replikering.)</span><span class="sxs-lookup"><span data-stu-id="3123c-156">(Set it to a read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3123c-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3123c-157">Next steps</span></span>
* <span data-ttu-id="3123c-158">Mer information om aktiv geo-replikering finns [aktiv geo-replikering](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3123c-158">To learn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="3123c-159">En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="3123c-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

