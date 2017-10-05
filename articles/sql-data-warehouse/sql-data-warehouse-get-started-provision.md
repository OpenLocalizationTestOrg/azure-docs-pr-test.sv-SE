---
title: Skapa ett SQL Data Warehouse i Azure Portal | Microsoft Docs
description: "Lär dig hur du skapar ett Azure SQL Data Warehouse i Azure Portal"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 24ed2d8bad3090e378acf2a42fb909dee0a8517b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a><span data-ttu-id="48272-103">Skapa ett Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="48272-103">Create an Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48272-104">Azure-portal</span><span class="sxs-lookup"><span data-stu-id="48272-104">Azure portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="48272-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="48272-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="48272-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48272-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="48272-107">I den här självstudien kommer du att använda Azure Portal för att skapa ett SQL Data Warehouse som innehåller en AdventureWorksDW-exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="48272-107">This tutorial uses the Azure portal to create a SQL Data Warehouse that contains an AdventureWorksDW sample database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48272-108">Krav</span><span class="sxs-lookup"><span data-stu-id="48272-108">Prerequisites</span></span>
<span data-ttu-id="48272-109">Du behöver följande för att komma igång:</span><span class="sxs-lookup"><span data-stu-id="48272-109">To get started, you need:</span></span>

* <span data-ttu-id="48272-110">**Azure-konto**: Gå till [Kostnadsfri utvärderingsversion av Azure][Azure Free Trial] eller [MSDN Azure-krediter][MSDN Azure Credits] för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="48272-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] to create an account.</span></span>
* <span data-ttu-id="48272-111">**Azure SQL server**:  Se [Skapa en Azure SQL Database med Azure-portalen][Create an Azure SQL database in the Azure portal] för mer information.</span><span class="sxs-lookup"><span data-stu-id="48272-111">**Azure SQL server**:  See [Create an Azure SQL database with the Azure portal][Create an Azure SQL database in the Azure portal] for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="48272-112">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="48272-112">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="48272-113">Se [SQL Data Warehouse-prissättning][SQL Data Warehouse pricing] för mer information.</span><span class="sxs-lookup"><span data-stu-id="48272-113">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="48272-114">Skapa ett SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="48272-114">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="48272-115">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48272-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="48272-116">Klicka på **+ Nytt** > **Databaser** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="48272-116">Click **+ New** > **Databases** > **SQL Data Warehouse**.</span></span>

    ![Skapa](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. <span data-ttu-id="48272-118">I bladet **SQL Data Warehouse**, anger du informationen som behövs och trycker på Skapa för att skapa.</span><span class="sxs-lookup"><span data-stu-id="48272-118">In the **SQL Data Warehouse** blade, fill in the information needed, then press 'Create' to create.</span></span>

    ![Skapa databas](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * <span data-ttu-id="48272-120">**Server**: Vi rekommenderar att du först väljer server.</span><span class="sxs-lookup"><span data-stu-id="48272-120">**Server**: We recommend you select your server first.</span></span>  
   * <span data-ttu-id="48272-121">**Databasnamn**: Namnet som används för att referera till ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="48272-121">**Database name**: The name that is used to reference the SQL Data Warehouse.</span></span>  <span data-ttu-id="48272-122">Det måste vara unikt för servern.</span><span class="sxs-lookup"><span data-stu-id="48272-122">It must be unique to the server.</span></span>
   * <span data-ttu-id="48272-123">**Prestanda**: Vi rekommenderar att du börjar med 400 [DWU:er][DWU].</span><span class="sxs-lookup"><span data-stu-id="48272-123">**Performance**: We recommend starting with 400 [DWUs][DWU].</span></span> <span data-ttu-id="48272-124">Du kan flytta skjutreglaget till vänster eller höger för att justera prestanda för ditt informationslager, eller skala upp eller ner efter det har skapats.</span><span class="sxs-lookup"><span data-stu-id="48272-124">You can move the slider to the left or right to adjust the performance of your data warehouse, or scale up or down after creation.</span></span>  <span data-ttu-id="48272-125">Läs mer om DWU:er i vår dokumentation om [skalning](sql-data-warehouse-manage-compute-overview.md) eller vår [prissättningssida][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="48272-125">To learn more about DWUs, see our documentation on [scaling](sql-data-warehouse-manage-compute-overview.md) or our [pricing page][SQL Data Warehouse pricing].</span></span>
   * <span data-ttu-id="48272-126">**Prenumeration**: Välj den [prenumeration] som detta SQL Data Warehouse kommer faktureras till.</span><span class="sxs-lookup"><span data-stu-id="48272-126">**Subscription**: Select the [subscription] that this SQL Data Warehouse will bill to.</span></span>
   * <span data-ttu-id="48272-127">**Resursgrupp**: [Resursgrupper][Resource group] är behållare som hjälper dig att hantera en samling med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="48272-127">**Resource group**: [Resource groups][Resource group] are containers designed to help you manage a collection of Azure resources.</span></span> <span data-ttu-id="48272-128">Läs mer om [resursgrupper](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="48272-128">Learn more about [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="48272-129">**Välj källa**: Klicka på **Välj källa** > **Exempel**.</span><span class="sxs-lookup"><span data-stu-id="48272-129">**Select source**: Click **Select source** > **Sample**.</span></span> <span data-ttu-id="48272-130">Azure fyller automatiskt i alternativet **Välj exempel** med AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="48272-130">Azure automatically populates the **Select sample** option with AdventureWorksDW.</span></span>

   > [!NOTE]
   > <span data-ttu-id="48272-131">Standardsortering för ett SQL Data Warehouse är SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="48272-131">The default collation for a SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS.</span></span> <span data-ttu-id="48272-132">Om du behöver en annan sortering, kan [T-SQL][T-SQL] användas för att skapa databasen med en annan sortering.</span><span class="sxs-lookup"><span data-stu-id="48272-132">If a different collation is needed, [T-SQL][T-SQL] can be used to create the database with a different collation.</span></span>
   >
   >

1. <span data-ttu-id="48272-133">Klicka på **Skapa**, för att skapa ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="48272-133">Click **Create** to create your SQL Data Warehouse.</span></span>
2. <span data-ttu-id="48272-134">Vänta några minuter.</span><span class="sxs-lookup"><span data-stu-id="48272-134">Wait for a few minutes.</span></span> <span data-ttu-id="48272-135">När datalagret är klart bör du komma tillbaka till [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48272-135">When your data warehouse is ready, you should be returned to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="48272-136">Du hittar ditt SQL Data Warehouse på instrumentpanelen, listat under dina SQL-databaser, eller i den resursgrupp som du skapade den i.</span><span class="sxs-lookup"><span data-stu-id="48272-136">You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in the resource group that you used to create it.</span></span>

    ![Portal-vy](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="48272-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48272-138">Next steps</span></span>
<span data-ttu-id="48272-139">Nu när du har skapat ett SQL Data Warehouse är du redo att [ansluta](sql-data-warehouse-connect-overview.md) och börja ställa frågor.</span><span class="sxs-lookup"><span data-stu-id="48272-139">Now that you have created a SQL Data Warehouse, you are ready to [Connect](sql-data-warehouse-connect-overview.md) and begin querying.</span></span>

<span data-ttu-id="48272-140">Se [översikt över inläsning](sql-data-warehouse-overview-load.md) för att läsa in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="48272-140">To load data into SQL Data Warehouse, see the [loading overview](sql-data-warehouse-overview-load.md).</span></span>

<span data-ttu-id="48272-141">Om du försöker migrera en befintlig databas till SQL Data Warehouse, kan du se [Migreringsöversikt](sql-data-warehouse-overview-migrate.md) eller använda dig av [Migreringsverktyget](sql-data-warehouse-migrate-migration-utility.md).</span><span class="sxs-lookup"><span data-stu-id="48272-141">If you are trying to migrate an existing database to SQL Data Warehouse, see the [Migration overview](sql-data-warehouse-overview-migrate.md) or use [Migration Utility](sql-data-warehouse-migrate-migration-utility.md).</span></span>

<span data-ttu-id="48272-142">Brandväggsregler kan också konfigureras med hjälp av Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="48272-142">Firewall rules can also be configured using Transact-SQL.</span></span> <span data-ttu-id="48272-143">Mer information finns i [sp_set_firewall_rule][sp_set_firewall_rule] och [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="48272-143">For more information, see [sp_set_firewall_rule][sp_set_firewall_rule] and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="48272-144">Det är också en bra idé att titta på [Metodtips][Best practices].</span><span class="sxs-lookup"><span data-stu-id="48272-144">It's also a great idea to look at the [Best practices][Best practices].</span></span>

<!--Article references-->
[Create an Azure SQL database in the Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
<span data-ttu-id="48272-145">[prenumeration]: ../azure-glossary-cloud-terminology.md#subscription</span><span class="sxs-lookup"><span data-stu-id="48272-145">[subscription]: ../azure-glossary-cloud-terminology.md#subscription</span></span>
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
