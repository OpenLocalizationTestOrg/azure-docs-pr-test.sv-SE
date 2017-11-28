---
title: aaaRestore Azure SQL Data Warehouse (Azure portal) | Microsoft Docs
description: "Azure portal uppgifter för att återställa Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cb225d2a21b61acab70a51b69c266f8d3ffacc9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a><span data-ttu-id="3d329-103">Återställa Azure SQL Data Warehouse (portal)</span><span class="sxs-lookup"><span data-stu-id="3d329-103">Restore Azure SQL Data Warehouse (portal)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3d329-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="3d329-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="3d329-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="3d329-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="3d329-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="3d329-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="3d329-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="3d329-107">[REST][REST]</span></span>
>
>
<span data-ttu-id="3d329-108">I den här artikeln får du lära dig hur toorestore Azure SQL Data Warehouse med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d329-108">In this article, you will learn how toorestore Azure SQL Data Warehouse by using hello Azure portal.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3d329-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3d329-109">Before you begin</span></span>
<span data-ttu-id="3d329-110">**Kontrollera DTU-kapacitet.**</span><span class="sxs-lookup"><span data-stu-id="3d329-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="3d329-111">Varje instans av SQL Data Warehouse finns i en SQL-server (till exempel myserver.database.windows.net) som har en kvot som standard data genomströmning enhet (DTU).</span><span class="sxs-lookup"><span data-stu-id="3d329-111">Each instance of SQL Data Warehouse is hosted by a SQL server (for example, myserver.database.windows.net) which has a default data throughput unit (DTU) quota.</span></span> <span data-ttu-id="3d329-112">Innan du kan återställa SQL Data Warehouse, kan du kontrollera att SQLServer har tillräckligt återstående DTU-kvot för hello-databas som du återställa.</span><span class="sxs-lookup"><span data-stu-id="3d329-112">Before you can restore SQL Data Warehouse, verify that your SQL server has enough remaining DTU quota for hello database that you're restoring.</span></span> <span data-ttu-id="3d329-113">toolearn hur toocalculate DTU-kvot eller toorequest mer dtu: er Se [begär en ändring för DTU-kvot][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="3d329-113">toolearn how toocalculate DTU quota or toorequest more DTUs, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="3d329-114">Återställa en databas för aktiva eller pausats</span><span class="sxs-lookup"><span data-stu-id="3d329-114">Restore an active or paused database</span></span>
<span data-ttu-id="3d329-115">toorestore en-databas:</span><span class="sxs-lookup"><span data-stu-id="3d329-115">toorestore a database:</span></span>

1. <span data-ttu-id="3d329-116">Logga in toohello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="3d329-116">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="3d329-117">I hello vänster och välj **Bläddra**, och välj sedan **SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="3d329-117">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Välj Bläddra > SQL-servrar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="3d329-119">Hitta din server och markerar den.</span><span class="sxs-lookup"><span data-stu-id="3d329-119">Find your server, and then select it.</span></span>

    ![Markera din server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. <span data-ttu-id="3d329-121">Hitta hello instans av SQL Data Warehouse som du vill toorestore från och markerar den.</span><span class="sxs-lookup"><span data-stu-id="3d329-121">Find hello instance of SQL Data Warehouse that you want toorestore from, and then select it.</span></span>

    ![Välj hello instans av SQL Data Warehouse toorestore](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. <span data-ttu-id="3d329-123">Överst hello i hello Data Warehouse-bladet, Välj **återställa**.</span><span class="sxs-lookup"><span data-stu-id="3d329-123">At hello top of hello Data Warehouse blade, select **Restore**.</span></span>

    ![Välj återställning](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. <span data-ttu-id="3d329-125">Ange ett nytt **databasnamnet**.</span><span class="sxs-lookup"><span data-stu-id="3d329-125">Specify a new **Database name**.</span></span>
7. <span data-ttu-id="3d329-126">Välj hello senaste **återställningspunkt**.</span><span class="sxs-lookup"><span data-stu-id="3d329-126">Select hello latest **Restore point**.</span></span>

   <span data-ttu-id="3d329-127">Kontrollera att du väljer hello senaste återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="3d329-127">Make sure you choose hello latest restore point.</span></span> <span data-ttu-id="3d329-128">Eftersom återställningspunkter visas i Coordinated Universal Time (UTC), kanske inte hello standardalternativet hello senaste återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="3d329-128">Because restore points are shown in Coordinated Universal Time (UTC), hello default option might not be hello latest restore point.</span></span>

      ![Välj en återställningspunkt](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. <span data-ttu-id="3d329-130">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d329-130">Select **OK**.</span></span>
9. <span data-ttu-id="3d329-131">hello databasen återställningsprocessen startar och du kan använda **meddelanden** toomonitor hello processen.</span><span class="sxs-lookup"><span data-stu-id="3d329-131">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="3d329-132">När hello återställning har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="3d329-132">After hello restore has finished, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="restore-a-deleted-database"></a><span data-ttu-id="3d329-133">Återställa en borttagen databas</span><span class="sxs-lookup"><span data-stu-id="3d329-133">Restore a deleted database</span></span>
<span data-ttu-id="3d329-134">toorestore en borttagen databas:</span><span class="sxs-lookup"><span data-stu-id="3d329-134">toorestore a deleted database:</span></span>

1. <span data-ttu-id="3d329-135">Logga in toohello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="3d329-135">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="3d329-136">I hello vänster och välj **Bläddra**, och välj sedan **SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="3d329-136">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Välj Bläddra > SQL-servrar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="3d329-138">Hitta din server och markerar den.</span><span class="sxs-lookup"><span data-stu-id="3d329-138">Find your server, and then select it.</span></span>

    ![Markera din server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. <span data-ttu-id="3d329-140">Bläddra nedåt toohello **Operations** avsnitt på bladet för din server.</span><span class="sxs-lookup"><span data-stu-id="3d329-140">Scroll down toohello **Operations** section on your server's blade.</span></span>
5. <span data-ttu-id="3d329-141">Välj hello **bort databaser** panelen.</span><span class="sxs-lookup"><span data-stu-id="3d329-141">Select hello **Deleted databases** tile.</span></span>

    ![Välj hello borttagna databaser sida vid sida](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. <span data-ttu-id="3d329-143">Välj hello bort databasen som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="3d329-143">Select hello deleted database that you want toorestore.</span></span>

    ![Välj en databas toorestore](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. <span data-ttu-id="3d329-145">Ange ett nytt **databasnamnet**.</span><span class="sxs-lookup"><span data-stu-id="3d329-145">Specify a new **Database name**.</span></span>

    ![Lägga till ett namn för hello-databas](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. <span data-ttu-id="3d329-147">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d329-147">Select **OK**.</span></span>
9. <span data-ttu-id="3d329-148">hello databasen återställningsprocessen startar och du kan använda **meddelanden** toomonitor hello processen.</span><span class="sxs-lookup"><span data-stu-id="3d329-148">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="3d329-149">tooconfigure databasen efter hello återställning har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="3d329-149">tooconfigure your database after hello restore has finished, see [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="3d329-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d329-150">Next steps</span></span>
<span data-ttu-id="3d329-151">toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version, läsa hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="3d329-151">toolearn about hello business continuity features of Azure SQL Database editions, read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
