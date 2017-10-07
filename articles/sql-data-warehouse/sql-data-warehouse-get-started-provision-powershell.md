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
# <a name="create-sql-data-warehouse-using-powershell"></a>Skapa ett SQL Data Warehouse med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [Azure-portalen](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Den här artikeln visar hur toocreate en SQL Data Warehouse med hjälp av PowerShell.

## <a name="prerequisites"></a>Krav
tooget igång, behöver du:

* **Azure-konto**: Besök [kostnadsfri utvärderingsversion av Azure] [ Azure Free Trial] eller [MSDN Azure-krediter] [ MSDN Azure Credits] toocreate ett konto.
* **Azure SQL-servern**: se [skapa en Azure SQL database i hello Azure Portal] [ Create an Azure SQL database in hello Azure Portal] eller [skapar en Azure SQL database med PowerShell] [ Create an Azure SQL database with PowerShell] för mer information.
* **Resursgruppen**: antingen använda hello samma resurs grupp som din Azure SQL-server eller se [hur toocreate en resursgrupp](../azure-resource-manager/resource-group-portal.md).
* **PowerShell version 1.0.3 eller senare**: Du kan kontrollera din version genom att köra **Get-Module -ListAvailable -Name Azure**.  hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].

> [!NOTE]
> Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.  Mer information om priser finns i [Priser för SQL Data Warehouse][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>Skapa ett SQL Data Warehouse
1. Öppna Windows PowerShell.
2. Kör denna cmdlet toologin tooAzure Resource Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Välj hello-prenumeration som du vill toouse för den aktuella sessionen.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Skapa databas. Det här exemplet skapar en databas med namnet ”mynewsqldw” med tjänstmål-nivån ”DW400” toohello servern ”sqldwserver1”, som finns i hello resursgrupp med namnet ”mywesteuroperesgp1”.

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

Erfordrade parametrar är:

* **RequestedServiceObjectiveName**: hello mängden [DWU] [ DWU] du begär.  Värden som stöds är: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 och DW6000.
* **DatabaseName**: hello namnet på hello SQL Data Warehouse som du skapar.
* **ServerName**: hello namnet på hello-server som du använder för att skapa (måste vara V12).
* **ResourceGroupName**: Resursgruppen som du använder dig av.  toofind tillgängliga resursgrupper i din prenumeration Använd Get-AzureResource.
* **Edition**: måste vara ”DataWarehouse” toocreate ett SQL Data Warehouse.

Valfria parametrar är:

* **Sorteringsnamnet**: hello Standardsortering om inget annat anges är SQL_Latin1_General_CP1_CI_AS.  Sorteringen kan inte ändras för en databas.
* **MaxSizeBytes**: hello standard maxstorlek för en databas är 10 GB.

Mer information om hello parameteralternativ finns [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] och [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Nästa steg
När ditt SQL Data Warehouse är klar färdigetablerat, kan tootry [läsa in exempeldata] [ loading sample data] eller kolla hur för[utveckla] [ develop] , [ladda][load], eller [migrera][migrate].

Om du vill veta mer om hur toomanage SQL Data Warehouse programmässigt, kolla vår artikel om hur toouse [PowerShell-cmdletar och REST API: er][PowerShell cmdlets and REST APIs].

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
