---
title: "aaaCreate SQL Data Warehouse med hjälp av PowerShell | Microsoft Docs"
description: "Skapa ett SQL Data Warehouse med hjälp av PowerShell"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d8af29ec285a11285785ab5474e4dfc8c36bc3ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a><span data-ttu-id="29925-103">Skapa ett SQL Data Warehouse med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="29925-103">Create SQL Data Warehouse using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29925-104">Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="29925-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="29925-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="29925-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="29925-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29925-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="29925-107">Den här artikeln visar hur toocreate en SQL Data Warehouse med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="29925-107">This article shows you how toocreate a SQL Data Warehouse using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29925-108">Krav</span><span class="sxs-lookup"><span data-stu-id="29925-108">Prerequisites</span></span>
<span data-ttu-id="29925-109">tooget igång, behöver du:</span><span class="sxs-lookup"><span data-stu-id="29925-109">tooget started, you need:</span></span>

* <span data-ttu-id="29925-110">**Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="29925-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="29925-111">**Azure SQL-servern**: se [skapa en Azure SQL database i hello Azure Portal] [ Create an Azure SQL database in hello Azure Portal] eller [skapar en Azure SQL database med PowerShell] [ Create an Azure SQL database with PowerShell] för mer information.</span><span class="sxs-lookup"><span data-stu-id="29925-111">**Azure SQL server**:  See [Create an Azure SQL database in hello Azure Portal][Create an Azure SQL database in hello Azure Portal] or [Create an Azure SQL database with PowerShell][Create an Azure SQL database with PowerShell] for more details.</span></span>
* <span data-ttu-id="29925-112">**Resursgruppen**: antingen använda hello samma resurs grupp som din Azure SQL-server eller se [hur toocreate en resursgrupp](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="29925-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="29925-113">**PowerShell version 1.0.3 eller senare**: Du kan kontrollera din version genom att köra **Get-Module -ListAvailable -Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="29925-113">**PowerShell version 1.0.3 or greater**:  You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="29925-114">hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="29925-114">hello latest version can be installed from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="29925-115">Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="29925-115">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

> [!NOTE]
> <span data-ttu-id="29925-116">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="29925-116">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="29925-117">Mer information om priser finns i [Priser för SQL Data Warehouse][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="29925-117">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="29925-118">Skapa ett SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="29925-118">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="29925-119">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="29925-119">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="29925-120">Kör denna cmdlet toologin tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="29925-120">Run this cmdlet toologin tooAzure Resource Manager.</span></span>

    ```Powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="29925-121">Välj hello-prenumeration som du vill toouse för den aktuella sessionen.</span><span class="sxs-lookup"><span data-stu-id="29925-121">Select hello subscription you want toouse for your current session.</span></span>

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. <span data-ttu-id="29925-122">Skapa databas.</span><span class="sxs-lookup"><span data-stu-id="29925-122">Create database.</span></span> <span data-ttu-id="29925-123">Det här exemplet skapar en databas med namnet ”mynewsqldw” med tjänstmål-nivån ”DW400” toohello servern ”sqldwserver1”, som finns i hello resursgrupp med namnet ”mywesteuroperesgp1”.</span><span class="sxs-lookup"><span data-stu-id="29925-123">This example creates a database named "mynewsqldw", with service objective level "DW400", toohello server named "sqldwserver1", which is in hello resource group named "mywesteuroperesgp1".</span></span>

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

<span data-ttu-id="29925-124">Erfordrade parametrar är:</span><span class="sxs-lookup"><span data-stu-id="29925-124">Required Parameters are:</span></span>

* <span data-ttu-id="29925-125">**RequestedServiceObjectiveName**: hello mängden [DWU] [ DWU] du begär.</span><span class="sxs-lookup"><span data-stu-id="29925-125">**RequestedServiceObjectiveName**: hello amount of [DWU][DWU] you are requesting.</span></span>  <span data-ttu-id="29925-126">Värden som stöds är: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 och DW6000.</span><span class="sxs-lookup"><span data-stu-id="29925-126">Supported values are: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000, and DW6000.</span></span>
* <span data-ttu-id="29925-127">**DatabaseName**: hello namnet på hello SQL Data Warehouse som du skapar.</span><span class="sxs-lookup"><span data-stu-id="29925-127">**DatabaseName**: hello name of hello SQL Data Warehouse that you are creating.</span></span>
* <span data-ttu-id="29925-128">**ServerName**: hello namnet på hello-server som du använder för att skapa (måste vara V12).</span><span class="sxs-lookup"><span data-stu-id="29925-128">**ServerName**: hello name of hello server that you are using for creation (must be V12).</span></span>
* <span data-ttu-id="29925-129">**ResourceGroupName**: Resursgruppen som du använder dig av.</span><span class="sxs-lookup"><span data-stu-id="29925-129">**ResourceGroupName**: Resource group you are using.</span></span>  <span data-ttu-id="29925-130">toofind tillgängliga resursgrupper i din prenumeration Använd Get-AzureResource.</span><span class="sxs-lookup"><span data-stu-id="29925-130">toofind available resource groups in your subscription use Get-AzureResource.</span></span>
* <span data-ttu-id="29925-131">**Edition**: måste vara ”DataWarehouse” toocreate ett SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="29925-131">**Edition**: Must be "DataWarehouse" toocreate a SQL Data Warehouse.</span></span>

<span data-ttu-id="29925-132">Valfria parametrar är:</span><span class="sxs-lookup"><span data-stu-id="29925-132">Optional Parameters are:</span></span>

* <span data-ttu-id="29925-133">**Sorteringsnamnet**: hello Standardsortering om inget annat anges är SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="29925-133">**CollationName**: hello default collation if not specified is SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="29925-134">Sorteringen kan inte ändras för en databas.</span><span class="sxs-lookup"><span data-stu-id="29925-134">Collation cannot be changed on a database.</span></span>
* <span data-ttu-id="29925-135">**MaxSizeBytes**: hello standard maxstorlek för en databas är 10 GB.</span><span class="sxs-lookup"><span data-stu-id="29925-135">**MaxSizeBytes**: hello default max size of a database is 10 GB.</span></span>

<span data-ttu-id="29925-136">Mer information om hello parameteralternativ finns [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] och [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="29925-136">For more details on hello parameter options, see [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] and [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="29925-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29925-137">Next steps</span></span>
<span data-ttu-id="29925-138">När ditt SQL Data Warehouse är klar färdigetablerat, kan tootry [läsa in exempeldata] [ loading sample data] eller kolla hur för[utveckla] [ develop] , [ladda][load], eller [migrera][migrate].</span><span class="sxs-lookup"><span data-stu-id="29925-138">After your SQL Data Warehouse has finished provisioning you may want tootry [loading sample data][loading sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

<span data-ttu-id="29925-139">Om du vill veta mer om hur toomanage SQL Data Warehouse programmässigt, kolla vår artikel om hur toouse [PowerShell-cmdletar och REST API: er][PowerShell cmdlets and REST APIs].</span><span class="sxs-lookup"><span data-stu-id="29925-139">If you're interested in more on how toomanage SQL Data Warehouse programmatically, check out our article on how toouse [PowerShell cmdlets and REST APIs][PowerShell cmdlets and REST APIs].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how toocreate a SQL Data Warehouse from hello Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
