---
title: Skapa ett SQL Data Warehouse med TSQL | Microsoft Docs
description: "Lär dig hur du skapar ett Azure SQL Data Warehouse med TSQL"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 10d8aa2b3ab8d7d8a9b91e95ffccf03faa89d237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a><span data-ttu-id="2c384-103">Skapa en SQL Data Warehouse-databas med hjälp av Transact-SQL (TSQL)</span><span class="sxs-lookup"><span data-stu-id="2c384-103">Create a SQL Data Warehouse database by using Transact-SQL (TSQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c384-104">Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2c384-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="2c384-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="2c384-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="2c384-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c384-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="2c384-107">I den här artikeln visas hur du skapar ett SQL Data Warehouse med T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2c384-107">This article shows you how to create a SQL Data Warehouse using T-SQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c384-108">Krav</span><span class="sxs-lookup"><span data-stu-id="2c384-108">Prerequisites</span></span>
<span data-ttu-id="2c384-109">Du behöver följande för att komma igång:</span><span class="sxs-lookup"><span data-stu-id="2c384-109">To get started, you need:</span></span>

* <span data-ttu-id="2c384-110">**Azure-konto**: Gå till [Kostnadsfri utvärderingsversion av Azure][Azure Free Trial] eller [MSDN Azure-krediter][MSDN Azure Credits] för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="2c384-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] to create an account.</span></span>
* <span data-ttu-id="2c384-111">**Azure SQL-servern**: Se [skapa en logisk Azure SQL Database-server med Azure-portalen] [skapa en logisk Azure SQL Database-server med Azure Portal] eller [skapa en logisk Azure SQL Database-server med PowerShell] [skapa en logisk Azure SQL Database-server med PowerShell] för mer information.</span><span class="sxs-lookup"><span data-stu-id="2c384-111">**Azure SQL server**:  See [Create an Azure SQL Database logical server with the Azure Portal][Create an Azure SQL Database logical server with the Azure Portal] or [Create an Azure SQL Database logical server with PowerShell][Create an Azure SQL Database logical server with PowerShell] for more details.</span></span>
* <span data-ttu-id="2c384-112">**Resursgrupp**: Använd antingen samma resursgrupp som din Azure SQL-server eller se [hur du skapar en resursgrupp][how to create a resource group].</span><span class="sxs-lookup"><span data-stu-id="2c384-112">**Resource group**: Either use the same resource group as your Azure SQL server or see [how to create a resource group][how to create a resource group].</span></span>
* <span data-ttu-id="2c384-113">**Körningsmiljö för T-SQL**: Du kan använda [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd] eller [SSMS][SSMS] för att köra T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2c384-113">**Environment to execute T-SQL**: You can use [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], or [SSMS][SSMS] to execute T-SQL.</span></span>

> [!NOTE]
> <span data-ttu-id="2c384-114">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="2c384-114">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="2c384-115">Mer information om priser finns i [Priser för SQL Data Warehouse][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="2c384-115">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-database-with-visual-studio"></a><span data-ttu-id="2c384-116">Skapa en databas med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c384-116">Create a database with Visual Studio</span></span>
<span data-ttu-id="2c384-117">Om du precis har kommit igång med Visual Studio läser du artikeln [Fråga SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span><span class="sxs-lookup"><span data-stu-id="2c384-117">If you are new to Visual Studio, see the article [Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span></span>  <span data-ttu-id="2c384-118">För att starta, öppnar du SQL Server Object Explorer i Visual Studio och ansluter till servern där din SQL Data Warehouse-databas kommer ligga.</span><span class="sxs-lookup"><span data-stu-id="2c384-118">To start, open SQL Server Object Explorer in Visual Studio and connect to the server that will host your SQL Data Warehouse database.</span></span>  <span data-ttu-id="2c384-119">När du väl är ansluten, kan du skapa ett SQL Data Warehouse genom att köra följande SQL-kommando mot **huvud**-databasen.</span><span class="sxs-lookup"><span data-stu-id="2c384-119">Once connected, you can create a SQL Data Warehouse by running the following SQL command against the **master** database.</span></span>  <span data-ttu-id="2c384-120">Det här kommandot skapar databasen MySqlDwDb med tjänstmålet DW400 och låter databasen växa till en maximal storlek på 10 TB.</span><span class="sxs-lookup"><span data-stu-id="2c384-120">This command creates the database MySqlDwDb with a Service Objective of DW400 and allow the database to grow to a maximum size of 10 TB.</span></span>

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a><span data-ttu-id="2c384-121">Skapa en databas med sqlcmd</span><span class="sxs-lookup"><span data-stu-id="2c384-121">Create a database with sqlcmd</span></span>
<span data-ttu-id="2c384-122">Alternativt kan du köra samma kommando med sqlcmd genom att köra följande i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="2c384-122">Alternatively, you can run the same command with sqlcmd by running the following at a command prompt.</span></span>

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

<span data-ttu-id="2c384-123">Standardsortering om inte ANNAT anges är COLLATE SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="2c384-123">The default collation when not specified is COLLATE SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="2c384-124">`MAXSIZE` kan vara mellan 250 GB och 240 TB.</span><span class="sxs-lookup"><span data-stu-id="2c384-124">The `MAXSIZE` can be between 250 GB and 240 TB.</span></span>  <span data-ttu-id="2c384-125">`SERVICE_OBJECTIVE` kan vara mellan DW100 och DW2000 [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="2c384-125">The `SERVICE_OBJECTIVE` can be between DW100 and DW2000 [DWU][DWU].</span></span>  <span data-ttu-id="2c384-126">En lista över alla giltiga värden finns i MSDN-dokumentationen för [CREATE DATABASE][CREATE DATABASE].</span><span class="sxs-lookup"><span data-stu-id="2c384-126">For a list of all valid values, see the MSDN documentation for [CREATE DATABASE][CREATE DATABASE].</span></span>  <span data-ttu-id="2c384-127">Både MAXSIZE och SERVICE_OBJECTIVE kan ändras med T-SQL-kommandot [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="2c384-127">Both the MAXSIZE and SERVICE_OBJECTIVE can be changed with an [ALTER DATABASE][ALTER DATABASE] T-SQL command.</span></span>  <span data-ttu-id="2c384-128">Sorteringen för en databas kan inte ändras efter skapandet.</span><span class="sxs-lookup"><span data-stu-id="2c384-128">The collation of a database cannot be changed after creation.</span></span>   <span data-ttu-id="2c384-129">Var försiktig när du ändrar SERVICE_OBJECTIVE eftersom det orsakar en omstart av tjänster som avbryter alla pågående frågor.</span><span class="sxs-lookup"><span data-stu-id="2c384-129">Caution should be used when changing the SERVICE_OBJECTIVE as changing DWU causes a restart of services, which cancels all queries in flight.</span></span>  <span data-ttu-id="2c384-130">En ändring av MAXSIZE startar inte om tjänsterna eftersom det bara är en enkel metadata-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2c384-130">Changing MAXSIZE does not restart services as it is just a simple metadata operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c384-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c384-131">Next steps</span></span>
<span data-ttu-id="2c384-132">När ditt SQL Data Warehouse är färdigetablerat kan du [läsa in exempeldata][load sample data] eller lära dig hur du [utvecklar][develop], [läser in][load] eller [migrerar][migrate].</span><span class="sxs-lookup"><span data-stu-id="2c384-132">After your SQL Data Warehouse has finished provisioning you can [load sample data][load sample data] or check out how to [develop][develop], [load][load], or [migrate][migrate].</span></span>

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
