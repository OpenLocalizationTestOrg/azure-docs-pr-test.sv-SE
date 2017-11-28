---
title: "aaaPowerShell-cmdlets för Azure SQL Data Warehouse"
description: "Hitta hello översta PowerShell-cmdlets för Azure SQL Data Warehouse hur toopause och återuppta en databas."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 84353b56131cf856e0724d338d7ed186fd2ceeaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a><span data-ttu-id="18d98-103">PowerShell-cmdletar och REST-API: er för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="18d98-103">PowerShell cmdlets and REST APIs for SQL Data Warehouse</span></span>
<span data-ttu-id="18d98-104">Många administrationsuppgifter för SQL Data Warehouse kan hanteras med hjälp av Azure PowerShell-cmdlets eller REST API: er.</span><span class="sxs-lookup"><span data-stu-id="18d98-104">Many SQL Data Warehouse administration tasks can be managed using either Azure PowerShell cmdlets or REST APIs.</span></span>  <span data-ttu-id="18d98-105">Nedan följer några exempel på hur toouse PowerShell-kommandon tooautomate vanliga aktiviteter i ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="18d98-105">Below are some examples of how toouse PowerShell commands tooautomate common tasks in your SQL Data Warehouse.</span></span>  <span data-ttu-id="18d98-106">Några bra REST-exempel finns hello artikel [hantera skalbarhet med övriga][Manage scalability with REST].</span><span class="sxs-lookup"><span data-stu-id="18d98-106">For some good REST examples, see hello article [Manage scalability with REST][Manage scalability with REST].</span></span>

> [!NOTE]
> <span data-ttu-id="18d98-107">I ordning toouse Azure PowerShell med SQL Data Warehouse, behöver du Azure PowerShell version 1.0.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="18d98-107">In order toouse Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="18d98-108">Du kan kontrollera din version genom att köra **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="18d98-108">You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="18d98-109">hello senaste versionen kan installeras från [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="18d98-109">hello latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="18d98-110">Mer information om hur du installerar hello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="18d98-110">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="18d98-111">Kom igång med Azure PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="18d98-111">Get started with Azure PowerShell cmdlets</span></span>
1. <span data-ttu-id="18d98-112">Öppna Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="18d98-112">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="18d98-113">I PowerShell-Kommandotolken hello köra dessa kommandon toosign i toohello Azure Resource Manager och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="18d98-113">At hello PowerShell prompt, run these commands toosign in toohello Azure Resource Manager and select your subscription.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a><span data-ttu-id="18d98-114">Exempel på paus SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="18d98-114">Pause SQL Data Warehouse Example</span></span>
<span data-ttu-id="18d98-115">Pausa en databas med namnet ”Database02” finns på en server med namnet ”Server01”.</span><span class="sxs-lookup"><span data-stu-id="18d98-115">Pause a database named "Database02" hosted on a server named "Server01."</span></span>  <span data-ttu-id="18d98-116">hello-servern är i ett Azure-resursgrupp med namnet ”ResourceGroup1”.</span><span class="sxs-lookup"><span data-stu-id="18d98-116">hello server is in an Azure resource group named "ResourceGroup1."</span></span>

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="18d98-117">En variant det här exemplet kommer hello hämta objekt för[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="18d98-117">A variation, this example pipes hello retrieved object too[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span>  <span data-ttu-id="18d98-118">Därför har hello databasen pausats.</span><span class="sxs-lookup"><span data-stu-id="18d98-118">As a result, hello database is paused.</span></span> <span data-ttu-id="18d98-119">hello kommandot visar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="18d98-119">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a><span data-ttu-id="18d98-120">Starta SQL Data Warehouse-exempel</span><span class="sxs-lookup"><span data-stu-id="18d98-120">Start SQL Data Warehouse Example</span></span>
<span data-ttu-id="18d98-121">Åtgärden återuppta av en databas med namnet ”Database02” finns på en server med namnet ”Server01”.</span><span class="sxs-lookup"><span data-stu-id="18d98-121">Resume operation of a database named "Database02" hosted on a server named "Server01."</span></span> <span data-ttu-id="18d98-122">hello-server ingår i en resursgrupp med namnet ”ResourceGroup1”.</span><span class="sxs-lookup"><span data-stu-id="18d98-122">hello server is contained in a resource group named "ResourceGroup1."</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="18d98-123">En variant det här exemplet hämtas en databas med namnet ”Database02” från en server med namnet ”Server01” som ingår i en resursgrupp med namnet ”ResourceGroup1”.</span><span class="sxs-lookup"><span data-stu-id="18d98-123">A variation, this example retrieves a database named "Database02" from a server named "Server01" that is contained in a resource group named "ResourceGroup1."</span></span> <span data-ttu-id="18d98-124">Det kommer hello hämta objekt för[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="18d98-124">It pipes hello retrieved object too[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> <span data-ttu-id="18d98-125">Observera att om servern är foo.database.windows.net, Använd ”foo” som hello - servernamn i hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="18d98-125">Note that if your server is foo.database.windows.net, use "foo" as hello -ServerName in hello PowerShell cmdlets.</span></span>
> 
> 

## <a name="other-supported-powershell-cmdlets"></a><span data-ttu-id="18d98-126">Andra stöd för PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="18d98-126">Other supported PowerShell cmdlets</span></span>
<span data-ttu-id="18d98-127">Dessa PowerShell cmdlets stöds med Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="18d98-127">These PowerShell cmdlets are supported with Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="18d98-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span><span class="sxs-lookup"><span data-stu-id="18d98-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span></span>
* <span data-ttu-id="18d98-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span><span class="sxs-lookup"><span data-stu-id="18d98-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span></span>
* <span data-ttu-id="18d98-131">[Ny AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-131">[New-AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-132">[Ta bort-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-133">[Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-133">[Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-135">[SELECT-AzureRmSubscription][Select-AzureRmSubscription]</span><span class="sxs-lookup"><span data-stu-id="18d98-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span></span>
* <span data-ttu-id="18d98-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="18d98-137">[Pausa AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="18d98-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span></span>

## <a name="next-steps"></a><span data-ttu-id="18d98-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18d98-138">Next steps</span></span>
<span data-ttu-id="18d98-139">Flera PowerShell-exemplen finns:</span><span class="sxs-lookup"><span data-stu-id="18d98-139">For more PowerShell examples, see:</span></span>

* <span data-ttu-id="18d98-140">[Skapa ett SQL Data Warehouse med hjälp av PowerShell][Create a SQL Data Warehouse using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="18d98-140">[Create a SQL Data Warehouse using PowerShell][Create a SQL Data Warehouse using PowerShell]</span></span>
* <span data-ttu-id="18d98-141">[Återställning av databasen][Database restore]</span><span class="sxs-lookup"><span data-stu-id="18d98-141">[Database restore][Database restore]</span></span>

<span data-ttu-id="18d98-142">Andra uppgifter som kan automatiseras med PowerShell finns i [Cmdlets för Azure SQL Database][Azure SQL Database Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="18d98-142">For other tasks which can be automated with PowerShell, see [Azure SQL Database Cmdlets][Azure SQL Database Cmdlets].</span></span> <span data-ttu-id="18d98-143">Observera att stöds inte alla Azure SQL Database-cmdlets för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="18d98-143">Note that not all Azure SQL Database cmdlets are supported for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="18d98-144">En lista över aktiviteter som kan automatiseras med övriga, se [åtgärder för Azure SQL-databaser][Operations for Azure SQL Databases].</span><span class="sxs-lookup"><span data-stu-id="18d98-144">For a list of tasks which can be automated with REST, see [Operations for Azure SQL Databases][Operations for Azure SQL Databases].</span></span>

<!--Image references-->

<!--Article references-->
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points tooSelect-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
