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
# <a name="create-an-azure-sql-data-warehouse"></a>Skapa ett Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Azure-portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Den här kursen använder hello Azure portal toocreate ett SQL Data Warehouse som innehåller en AdventureWorksDW som exempeldatabas.

## <a name="prerequisites"></a>Krav
tooget igång, behöver du:

* **Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.
* **Azure SQL-servern**: se [skapa en Azure SQL database med hello Azure-portalen] [ Create an Azure SQL database in hello Azure portal] för mer information.

> [!NOTE]
> Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.  Se [SQL Data Warehouse-prissättning][SQL Data Warehouse pricing] för mer information.
>
>

## <a name="create-a-sql-data-warehouse"></a>Skapa ett SQL Data Warehouse
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **+ Nytt** > **Databaser** > **SQL Data Warehouse**.

    ![Skapa](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. I hello **SQL Data Warehouse** bladet, Fyll i hello information som behövs, och tryck sedan på ”Skapa” toocreate.

    ![Skapa databas](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * **Server**: Vi rekommenderar att du först väljer server.  
   * **Databasnamnet**: hello-namn som används tooreference hello SQL Data Warehouse.  Den måste vara unika toohello server.
   * **Prestanda**: Vi rekommenderar att du börjar med 400 [DWU:er][DWU]. Du kan flytta hello skjutreglaget toohello åt vänster eller höger tooadjust hello prestanda i datalagret eller skala upp eller ned när den har skapats.  toolearn mer om dwu: er, se vår dokumentation på [skalning](sql-data-warehouse-manage-compute-overview.md) eller vår [sida med priser][SQL Data Warehouse pricing].
   * **Prenumerationen**: Välj hello [prenumeration] som den här SQL Data Warehouse kommer faktureras till.
   * **Resursgruppen**: [resursgrupper] [ Resource group] är behållare som toohelp du hantera en samling Azure-resurser. Läs mer om [resursgrupper](../azure-resource-manager/resource-group-overview.md).
   * **Välj källa**: Klicka på **Välj källa** > **Exempel**. Azure fyller automatiskt hello **Välj exempel** alternativet med AdventureWorksDW.

   > [!NOTE]
   > hello standardsortering för en SQL Data Warehouse är SQL_Latin1_General_CP1_CI_AS. Om en annan sortering krävs [T-SQL] [ T-SQL] kan vara används toocreate hello databasen med en annan sortering.
   >
   >

1. Klicka på **skapa** toocreate din SQL Data Warehouse.
2. Vänta några minuter. När ditt data warehouse är klar, ska du komma tillbaka toohello [Azure-portalen](https://portal.azure.com). Du kan hitta ditt SQL Data Warehouse på instrumentpanelen, listat under dina SQL-databaser eller grupp som du använde toocreate i hello resursen den.

    ![Portal-vy](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett SQL Data Warehouse, är du redo för[Anslut](sql-data-warehouse-connect-overview.md) och börja ställa frågor.

tooload data till SQL Data Warehouse Se hello [översikt över inläsning](sql-data-warehouse-overview-load.md).

Om du försöker toomigrate en befintlig databas tooSQL datalagret finns hello [migreringsöversikt](sql-data-warehouse-overview-migrate.md) eller använda [Migreringsverktyget](sql-data-warehouse-migrate-migration-utility.md).

Brandväggsregler kan också konfigureras med hjälp av Transact-SQL. Mer information finns i [sp_set_firewall_rule][sp_set_firewall_rule] och [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Det är också en bra idé toolook på hello [metodtips][Best practices].

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
