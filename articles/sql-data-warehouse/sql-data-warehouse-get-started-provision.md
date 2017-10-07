---
title: aaaCreate ett SQL Data Warehouse i hello Azure-portalen | Microsoft Docs
description: "Lär dig hur hello toocreate en Azure SQL Data Warehouse i Azure-portalen"
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
ms.openlocfilehash: f5be6e3f2936e3af9d099854a468f8ce66fd8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a><span data-ttu-id="5f674-103">Skapa ett Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5f674-103">Create an Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5f674-104">Azure-portal</span><span class="sxs-lookup"><span data-stu-id="5f674-104">Azure portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="5f674-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="5f674-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="5f674-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f674-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="5f674-107">Den här kursen använder hello Azure portal toocreate ett SQL Data Warehouse som innehåller en AdventureWorksDW som exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="5f674-107">This tutorial uses hello Azure portal toocreate a SQL Data Warehouse that contains an AdventureWorksDW sample database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f674-108">Krav</span><span class="sxs-lookup"><span data-stu-id="5f674-108">Prerequisites</span></span>
<span data-ttu-id="5f674-109">tooget igång, behöver du:</span><span class="sxs-lookup"><span data-stu-id="5f674-109">tooget started, you need:</span></span>

* <span data-ttu-id="5f674-110">**Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="5f674-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="5f674-111">**Azure SQL-servern**: se [skapa en Azure SQL database med hello Azure-portalen] [ Create an Azure SQL database in hello Azure portal] för mer information.</span><span class="sxs-lookup"><span data-stu-id="5f674-111">**Azure SQL server**:  See [Create an Azure SQL database with hello Azure portal][Create an Azure SQL database in hello Azure portal] for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="5f674-112">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="5f674-112">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="5f674-113">Se [SQL Data Warehouse-prissättning][SQL Data Warehouse pricing] för mer information.</span><span class="sxs-lookup"><span data-stu-id="5f674-113">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="5f674-114">Skapa ett SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5f674-114">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="5f674-115">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5f674-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5f674-116">Klicka på **+ Nytt** > **Databaser** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="5f674-116">Click **+ New** > **Databases** > **SQL Data Warehouse**.</span></span>

    ![Skapa](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. <span data-ttu-id="5f674-118">I hello **SQL Data Warehouse** bladet, Fyll i hello information som behövs, och tryck sedan på ”Skapa” toocreate.</span><span class="sxs-lookup"><span data-stu-id="5f674-118">In hello **SQL Data Warehouse** blade, fill in hello information needed, then press 'Create' toocreate.</span></span>

    ![Skapa databas](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * <span data-ttu-id="5f674-120">**Server**: Vi rekommenderar att du först väljer server.</span><span class="sxs-lookup"><span data-stu-id="5f674-120">**Server**: We recommend you select your server first.</span></span>  
   * <span data-ttu-id="5f674-121">**Databasnamnet**: hello-namn som används tooreference hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5f674-121">**Database name**: hello name that is used tooreference hello SQL Data Warehouse.</span></span>  <span data-ttu-id="5f674-122">Den måste vara unika toohello server.</span><span class="sxs-lookup"><span data-stu-id="5f674-122">It must be unique toohello server.</span></span>
   * <span data-ttu-id="5f674-123">**Prestanda**: Vi rekommenderar att du börjar med 400 [DWU:er][DWU].</span><span class="sxs-lookup"><span data-stu-id="5f674-123">**Performance**: We recommend starting with 400 [DWUs][DWU].</span></span> <span data-ttu-id="5f674-124">Du kan flytta hello skjutreglaget toohello åt vänster eller höger tooadjust hello prestanda i datalagret eller skala upp eller ned när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="5f674-124">You can move hello slider toohello left or right tooadjust hello performance of your data warehouse, or scale up or down after creation.</span></span>  <span data-ttu-id="5f674-125">toolearn mer om dwu: er, se vår dokumentation på [skalning](sql-data-warehouse-manage-compute-overview.md) eller vår [sida med priser][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="5f674-125">toolearn more about DWUs, see our documentation on [scaling](sql-data-warehouse-manage-compute-overview.md) or our [pricing page][SQL Data Warehouse pricing].</span></span>
   * <span data-ttu-id="5f674-126">**Prenumerationen**: Välj hello [prenumeration] som den här SQL Data Warehouse kommer faktureras till.</span><span class="sxs-lookup"><span data-stu-id="5f674-126">**Subscription**: Select hello [subscription] that this SQL Data Warehouse will bill to.</span></span>
   * <span data-ttu-id="5f674-127">**Resursgruppen**: [resursgrupper] [ Resource group] är behållare som toohelp du hantera en samling Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5f674-127">**Resource group**: [Resource groups][Resource group] are containers designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="5f674-128">Läs mer om [resursgrupper](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5f674-128">Learn more about [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="5f674-129">**Välj källa**: Klicka på **Välj källa** > **Exempel**.</span><span class="sxs-lookup"><span data-stu-id="5f674-129">**Select source**: Click **Select source** > **Sample**.</span></span> <span data-ttu-id="5f674-130">Azure fyller automatiskt hello **Välj exempel** alternativet med AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5f674-130">Azure automatically populates hello **Select sample** option with AdventureWorksDW.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f674-131">hello standardsortering för en SQL Data Warehouse är SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="5f674-131">hello default collation for a SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS.</span></span> <span data-ttu-id="5f674-132">Om en annan sortering krävs [T-SQL] [ T-SQL] kan vara används toocreate hello databasen med en annan sortering.</span><span class="sxs-lookup"><span data-stu-id="5f674-132">If a different collation is needed, [T-SQL][T-SQL] can be used toocreate hello database with a different collation.</span></span>
   >
   >

1. <span data-ttu-id="5f674-133">Klicka på **skapa** toocreate din SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5f674-133">Click **Create** toocreate your SQL Data Warehouse.</span></span>
2. <span data-ttu-id="5f674-134">Vänta några minuter.</span><span class="sxs-lookup"><span data-stu-id="5f674-134">Wait for a few minutes.</span></span> <span data-ttu-id="5f674-135">När ditt data warehouse är klar, ska du komma tillbaka toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5f674-135">When your data warehouse is ready, you should be returned toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5f674-136">Du kan hitta ditt SQL Data Warehouse på instrumentpanelen, listat under dina SQL-databaser eller grupp som du använde toocreate i hello resursen den.</span><span class="sxs-lookup"><span data-stu-id="5f674-136">You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in hello resource group that you used toocreate it.</span></span>

    ![Portal-vy](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="5f674-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f674-138">Next steps</span></span>
<span data-ttu-id="5f674-139">Nu när du har skapat ett SQL Data Warehouse, är du redo för[Anslut](sql-data-warehouse-connect-overview.md) och börja ställa frågor.</span><span class="sxs-lookup"><span data-stu-id="5f674-139">Now that you have created a SQL Data Warehouse, you are ready too[Connect](sql-data-warehouse-connect-overview.md) and begin querying.</span></span>

<span data-ttu-id="5f674-140">tooload data till SQL Data Warehouse Se hello [översikt över inläsning](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="5f674-140">tooload data into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>

<span data-ttu-id="5f674-141">Om du försöker toomigrate en befintlig databas tooSQL datalagret finns hello [migreringsöversikt](sql-data-warehouse-overview-migrate.md) eller använda [Migreringsverktyget](sql-data-warehouse-migrate-migration-utility.md).</span><span class="sxs-lookup"><span data-stu-id="5f674-141">If you are trying toomigrate an existing database tooSQL Data Warehouse, see hello [Migration overview](sql-data-warehouse-overview-migrate.md) or use [Migration Utility](sql-data-warehouse-migrate-migration-utility.md).</span></span>

<span data-ttu-id="5f674-142">Brandväggsregler kan också konfigureras med hjälp av Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="5f674-142">Firewall rules can also be configured using Transact-SQL.</span></span> <span data-ttu-id="5f674-143">Mer information finns i [sp_set_firewall_rule][sp_set_firewall_rule] och [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="5f674-143">For more information, see [sp_set_firewall_rule][sp_set_firewall_rule] and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="5f674-144">Det är också en bra idé toolook på hello [metodtips][Best practices].</span><span class="sxs-lookup"><span data-stu-id="5f674-144">It's also a great idea toolook at hello [Best practices][Best practices].</span></span>

<!--Article references-->
[Create an Azure SQL database in hello Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
[prenumeration]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
