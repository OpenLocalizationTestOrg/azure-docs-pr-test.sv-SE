---
title: "Återställa en Azure SQL Data Warehouse (REST-API) | Microsoft Docs"
description: "REST API-uppgifterna för att återställa en Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 8656607611e7518e42b51b91774f55abec15c228
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a><span data-ttu-id="7279d-103">Återställa en Azure SQL Data Warehouse (REST-API)</span><span class="sxs-lookup"><span data-stu-id="7279d-103">Restore an Azure SQL Data Warehouse (REST API)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="7279d-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="7279d-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="7279d-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="7279d-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="7279d-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="7279d-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="7279d-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="7279d-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="7279d-108">I den här artikeln får du lära dig hur du återställer en Azure SQL Data Warehouse med hjälp av REST-API.</span><span class="sxs-lookup"><span data-stu-id="7279d-108">In this article you will learn how to restore an Azure SQL Data Warehouse using the REST API.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7279d-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7279d-109">Before you begin</span></span>
<span data-ttu-id="7279d-110">**Kontrollera DTU-kapacitet.**</span><span class="sxs-lookup"><span data-stu-id="7279d-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="7279d-111">Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.</span><span class="sxs-lookup"><span data-stu-id="7279d-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="7279d-112">Innan du kan återställa en SQL Data Warehouse, kontrollera att den SQL-servern har tillräckligt med återstående DTU-kvot för databasen som återställs.</span><span class="sxs-lookup"><span data-stu-id="7279d-112">Before you can restore a SQL Data Warehouse, verify that the your SQL server has enough remaining DTU quota for the database being restored.</span></span> <span data-ttu-id="7279d-113">Information om hur du beräkna DTU behövs eller begära mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="7279d-113">To learn how to calculate DTU needed or to request more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="7279d-114">Återställa en databas för aktiva eller pausats</span><span class="sxs-lookup"><span data-stu-id="7279d-114">Restore an active or paused database</span></span>
<span data-ttu-id="7279d-115">Att återställa en databas:</span><span class="sxs-lookup"><span data-stu-id="7279d-115">To restore a database:</span></span>

1. <span data-ttu-id="7279d-116">Hämta listan över återställningspunkter i databasen med hjälp av återställningspunkter för Get-databasen igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-116">Get the list of database restore points using the Get Database Restore Points operation.</span></span>
2. <span data-ttu-id="7279d-117">Börja din återställning med hjälp av den [skapandebegäran om återställning av databas] [ Create database restore request] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-117">Begin your restore by using the [Create database restore request][Create database restore request] operation.</span></span>
3. <span data-ttu-id="7279d-118">Spåra statusen för återställningspunkten med hjälp av den [databasen Åtgärdsstatus] [ Database operation status] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-118">Track the status of your restore by using the [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="7279d-119">När återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="7279d-119">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="7279d-120">Återställa en borttagen databas</span><span class="sxs-lookup"><span data-stu-id="7279d-120">Restore a deleted database</span></span>
<span data-ttu-id="7279d-121">Att återställa en borttagen databas:</span><span class="sxs-lookup"><span data-stu-id="7279d-121">To restore a deleted database:</span></span>

1. <span data-ttu-id="7279d-122">Lista över alla dina återställningsbara borttagna databaser med hjälp av den [lista återställningsbara bort databaser] [ List restorable dropped databases] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-122">List all of your restorable deleted databases by using the [List restorable dropped databases][List restorable dropped databases] operation.</span></span>
2. <span data-ttu-id="7279d-123">Hämta information för den borttagna databasen som du vill återställa med hjälp av den [Get återställningsbara släppa databasen] [ Get restorable dropped database] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-123">Get the details for the deleted database you want to restore by using the [Get restorable dropped database][Get restorable dropped database] operation.</span></span>
3. <span data-ttu-id="7279d-124">Börja din återställning med hjälp av den [skapandebegäran om återställning av databas] [ Create database restore request] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-124">Begin your restore by using the [Create database restore request][Create database restore request] operation.</span></span>
4. <span data-ttu-id="7279d-125">Spåra statusen för återställningspunkten med hjälp av den [databasen Åtgärdsstatus] [ Database operation status] igen.</span><span class="sxs-lookup"><span data-stu-id="7279d-125">Track the status of your restore by using the [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="7279d-126">För att konfigurera databasen efter återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="7279d-126">To configure your database after the restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7279d-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7279d-127">Next steps</span></span>
<span data-ttu-id="7279d-128">Mer information om funktionerna för verksamhetskontinuitet Azure SQL Database version, Läs den [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="7279d-128">To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
