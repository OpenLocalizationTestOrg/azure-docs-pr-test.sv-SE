---
title: aaaRestore en Azure SQL Data Warehouse (REST-API) | Microsoft Docs
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
ms.openlocfilehash: cf6678d71aafff71b1ea715f447e41e25f20d1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a><span data-ttu-id="1072d-103">Återställa en Azure SQL Data Warehouse (REST-API)</span><span class="sxs-lookup"><span data-stu-id="1072d-103">Restore an Azure SQL Data Warehouse (REST API)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="1072d-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="1072d-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="1072d-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="1072d-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="1072d-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="1072d-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="1072d-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="1072d-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="1072d-108">I den här artikeln du lära dig hur hello toorestore en Azure SQL Data Warehouse med hjälp av REST API.</span><span class="sxs-lookup"><span data-stu-id="1072d-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using hello REST API.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1072d-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1072d-109">Before you begin</span></span>
<span data-ttu-id="1072d-110">**Kontrollera DTU-kapacitet.**</span><span class="sxs-lookup"><span data-stu-id="1072d-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="1072d-111">Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.</span><span class="sxs-lookup"><span data-stu-id="1072d-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="1072d-112">Innan du kan återställa en SQL Data Warehouse, kontrollera att hello din SQLServer har tillräckligt återstående DTU-kvot för hello databasen som återställs.</span><span class="sxs-lookup"><span data-stu-id="1072d-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="1072d-113">toolearn hur toocalculate DTU behövs eller toorequest mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="1072d-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="1072d-114">Återställa en databas för aktiva eller pausats</span><span class="sxs-lookup"><span data-stu-id="1072d-114">Restore an active or paused database</span></span>
<span data-ttu-id="1072d-115">toorestore en-databas:</span><span class="sxs-lookup"><span data-stu-id="1072d-115">toorestore a database:</span></span>

1. <span data-ttu-id="1072d-116">Hämta hello lista över återställningspunkter i databasen med hjälp av hello Återställningspunkter för Get-databasen igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-116">Get hello list of database restore points using hello Get Database Restore Points operation.</span></span>
2. <span data-ttu-id="1072d-117">Börja din återställning med hjälp av hello [begäran om att skapa databasen återställning] [ Create database restore request] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-117">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
3. <span data-ttu-id="1072d-118">Spåra hello status för återställningspunkten med hjälp av hello [databasen Åtgärdsstatus] [ Database operation status] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-118">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="1072d-119">När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="1072d-119">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="1072d-120">Återställa en borttagen databas</span><span class="sxs-lookup"><span data-stu-id="1072d-120">Restore a deleted database</span></span>
<span data-ttu-id="1072d-121">toorestore en borttagen databas:</span><span class="sxs-lookup"><span data-stu-id="1072d-121">toorestore a deleted database:</span></span>

1. <span data-ttu-id="1072d-122">Lista över alla dina återställningsbara borttagna databaser med hjälp av hello [lista återställningsbara bort databaser] [ List restorable dropped databases] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-122">List all of your restorable deleted databases by using hello [List restorable dropped databases][List restorable dropped databases] operation.</span></span>
2. <span data-ttu-id="1072d-123">Hämta hello information för hello bort databasen du vill använda toorestore med hjälp av hello [Get återställningsbara släppa databasen] [ Get restorable dropped database] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-123">Get hello details for hello deleted database you want toorestore by using hello [Get restorable dropped database][Get restorable dropped database] operation.</span></span>
3. <span data-ttu-id="1072d-124">Börja din återställning med hjälp av hello [begäran om att skapa databasen återställning] [ Create database restore request] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-124">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
4. <span data-ttu-id="1072d-125">Spåra hello status för återställningspunkten med hjälp av hello [databasen Åtgärdsstatus] [ Database operation status] igen.</span><span class="sxs-lookup"><span data-stu-id="1072d-125">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="1072d-126">tooconfigure databasen efter hello återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="1072d-126">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1072d-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1072d-127">Next steps</span></span>
<span data-ttu-id="1072d-128">Läs toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="1072d-128">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
