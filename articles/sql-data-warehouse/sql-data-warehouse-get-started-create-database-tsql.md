---
title: aaaCreate ett SQL Data Warehouse med TSQL | Microsoft Docs
description: "Lär dig hur toocreate en Azure SQL Data Warehouse med TSQL"
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
ms.openlocfilehash: 81ef59a66c61452ff8a2aca29837f155e87d017d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a><span data-ttu-id="dea35-103">Skapa en SQL Data Warehouse-databas med hjälp av Transact-SQL (TSQL)</span><span class="sxs-lookup"><span data-stu-id="dea35-103">Create a SQL Data Warehouse database by using Transact-SQL (TSQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dea35-104">Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dea35-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="dea35-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="dea35-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="dea35-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dea35-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="dea35-107">Den här artikeln visar hur toocreate en SQL Data Warehouse med hjälp av T-SQL.</span><span class="sxs-lookup"><span data-stu-id="dea35-107">This article shows you how toocreate a SQL Data Warehouse using T-SQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dea35-108">Krav</span><span class="sxs-lookup"><span data-stu-id="dea35-108">Prerequisites</span></span>
<span data-ttu-id="dea35-109">tooget igång, behöver du:</span><span class="sxs-lookup"><span data-stu-id="dea35-109">tooget started, you need:</span></span>

* <span data-ttu-id="dea35-110">**Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="dea35-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="dea35-111">**Azure SQL-servern**: Se [skapa en logisk Azure SQL Database-server med hello Azure-portalen] [skapa en logisk Azure SQL Database-server med hello Azure-portalen] eller [skapa en logisk Azure SQL Database-server med PowerShell] [skapa en Azure SQL Logisk Database-server med PowerShell] för mer information.</span><span class="sxs-lookup"><span data-stu-id="dea35-111">**Azure SQL server**:  See [Create an Azure SQL Database logical server with hello Azure Portal][Create an Azure SQL Database logical server with hello Azure Portal] or [Create an Azure SQL Database logical server with PowerShell][Create an Azure SQL Database logical server with PowerShell] for more details.</span></span>
* <span data-ttu-id="dea35-112">**Resursgruppen**: antingen använda hello samma resurs grupp som din Azure SQL-server eller se [hur toocreate en resursgrupp][how toocreate a resource group].</span><span class="sxs-lookup"><span data-stu-id="dea35-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group][how toocreate a resource group].</span></span>
* <span data-ttu-id="dea35-113">**Miljö tooexecute T-SQL**: du kan använda [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], eller [SSMS] [ SSMS] tooexecute T-SQL.</span><span class="sxs-lookup"><span data-stu-id="dea35-113">**Environment tooexecute T-SQL**: You can use [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], or [SSMS][SSMS] tooexecute T-SQL.</span></span>

> [!NOTE]
> <span data-ttu-id="dea35-114">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="dea35-114">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="dea35-115">Mer information om priser finns i [Priser för SQL Data Warehouse][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="dea35-115">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-database-with-visual-studio"></a><span data-ttu-id="dea35-116">Skapa en databas med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dea35-116">Create a database with Visual Studio</span></span>
<span data-ttu-id="dea35-117">Om du är ny tooVisual Studio finns i artikeln hello [frågan Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span><span class="sxs-lookup"><span data-stu-id="dea35-117">If you are new tooVisual Studio, see hello article [Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span></span>  <span data-ttu-id="dea35-118">toostart, Öppna SQL Server Object Explorer i Visual Studio och Anslut toohello-server som är värd för SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="dea35-118">toostart, open SQL Server Object Explorer in Visual Studio and connect toohello server that will host your SQL Data Warehouse database.</span></span>  <span data-ttu-id="dea35-119">När du är ansluten, kan du skapa ett SQL Data Warehouse genom att köra följande SQL-kommando mot hello hello **master** databas.</span><span class="sxs-lookup"><span data-stu-id="dea35-119">Once connected, you can create a SQL Data Warehouse by running hello following SQL command against hello **master** database.</span></span>  <span data-ttu-id="dea35-120">Det här kommandot skapar hello databasen MySqlDwDb med en Tjänstmålet DW400 och Tillåt hello databasen toogrow tooa maximal storlek på 10 TB.</span><span class="sxs-lookup"><span data-stu-id="dea35-120">This command creates hello database MySqlDwDb with a Service Objective of DW400 and allow hello database toogrow tooa maximum size of 10 TB.</span></span>

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a><span data-ttu-id="dea35-121">Skapa en databas med sqlcmd</span><span class="sxs-lookup"><span data-stu-id="dea35-121">Create a database with sqlcmd</span></span>
<span data-ttu-id="dea35-122">Du kan också köra hello samma kommando med sqlcmd genom att köra hello följande i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="dea35-122">Alternatively, you can run hello same command with sqlcmd by running hello following at a command prompt.</span></span>

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

<span data-ttu-id="dea35-123">hello Standardsortering när inte anges är SQL_Latin1_General_CP1_CI_AS för sortering.</span><span class="sxs-lookup"><span data-stu-id="dea35-123">hello default collation when not specified is COLLATE SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="dea35-124">Hej `MAXSIZE` kan vara mellan 250 GB och 240 TB.</span><span class="sxs-lookup"><span data-stu-id="dea35-124">hello `MAXSIZE` can be between 250 GB and 240 TB.</span></span>  <span data-ttu-id="dea35-125">Hej `SERVICE_OBJECTIVE` kan vara mellan DW100 och DW2000 [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="dea35-125">hello `SERVICE_OBJECTIVE` can be between DW100 and DW2000 [DWU][DWU].</span></span>  <span data-ttu-id="dea35-126">En lista över alla giltiga värden finns hello MSDN-dokumentationen för [CREATE DATABASE][CREATE DATABASE].</span><span class="sxs-lookup"><span data-stu-id="dea35-126">For a list of all valid values, see hello MSDN documentation for [CREATE DATABASE][CREATE DATABASE].</span></span>  <span data-ttu-id="dea35-127">Både hello MAXSIZE och SERVICE_OBJECTIVE kan ändras med en [ALTER DATABASE] [ ALTER DATABASE] T-SQL-kommandot.</span><span class="sxs-lookup"><span data-stu-id="dea35-127">Both hello MAXSIZE and SERVICE_OBJECTIVE can be changed with an [ALTER DATABASE][ALTER DATABASE] T-SQL command.</span></span>  <span data-ttu-id="dea35-128">hello sorteringen för en databas kan inte ändras när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="dea35-128">hello collation of a database cannot be changed after creation.</span></span>   <span data-ttu-id="dea35-129">Varning ska användas när ändra hello SERVICE_OBJECTIVE ändras DWU orsakar en omstart av tjänster som avbryter alla frågor som rör sig.</span><span class="sxs-lookup"><span data-stu-id="dea35-129">Caution should be used when changing hello SERVICE_OBJECTIVE as changing DWU causes a restart of services, which cancels all queries in flight.</span></span>  <span data-ttu-id="dea35-130">En ändring av MAXSIZE startar inte om tjänsterna eftersom det bara är en enkel metadata-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dea35-130">Changing MAXSIZE does not restart services as it is just a simple metadata operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dea35-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dea35-131">Next steps</span></span>
<span data-ttu-id="dea35-132">När ditt SQL Data Warehouse är färdigetablerat, kan du [läsa in exempeldata] [ load sample data] eller kolla hur för[utveckla][develop], [ladda][load], eller [migrera][migrate].</span><span class="sxs-lookup"><span data-stu-id="dea35-132">After your SQL Data Warehouse has finished provisioning you can [load sample data][load sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how toocreate a SQL Data Warehouse from hello Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
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
