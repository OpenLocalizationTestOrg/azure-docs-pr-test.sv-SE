---
title: "PowerShell exempel-restore-säkerhetskopiering – Azure SQL database | Microsoft Docs"
description: "Azure PowerShell-exempelskript för att återställa en Azure SQL database från geo-redundant säkerhetskopieringar"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: ae1d0c828ae1e7e1e7e07dcc7d6157187a3859d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a><span data-ttu-id="e038c-103">Använd PowerShell för att återställa en Azure SQL database från säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="e038c-103">Use PowerShell to restore an Azure SQL database from backups</span></span>

<span data-ttu-id="e038c-104">Exempel på detta PowerShell-skript återställer en Azure SQL-databas från en geo-redundant säkerhetskopia, återställer en borttagen Azure SQL-databas till den senaste säkerhetskopian och återställer en Azure SQL database till en specifik tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="e038c-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database to its latest backup, and restores an Azure SQL database to a specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="e038c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e038c-105">Sample script</span></span>

<span data-ttu-id="e038c-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "skapa SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="e038c-106">[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e038c-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="e038c-107">Clean up deployment</span></span>

<span data-ttu-id="e038c-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="e038c-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="e038c-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e038c-109">Script explanation</span></span>

<span data-ttu-id="e038c-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="e038c-110">This script uses the following commands.</span></span> <span data-ttu-id="e038c-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e038c-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e038c-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="e038c-112">Command</span></span> | <span data-ttu-id="e038c-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e038c-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e038c-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e038c-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="e038c-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e038c-115">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="e038c-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="e038c-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="e038c-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="e038c-117">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="e038c-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="e038c-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="e038c-119">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="e038c-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="e038c-120">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="e038c-120">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="e038c-121">Hämtar en geo-redundant säkerhetskopia av en databas.</span><span class="sxs-lookup"><span data-stu-id="e038c-121">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="e038c-122">Restore-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="e038c-122">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="e038c-123">Återställer en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e038c-123">Restores a SQL database.</span></span> |
|[<span data-ttu-id="e038c-124">Ta bort-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="e038c-124">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="e038c-125">Tar bort en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="e038c-125">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="e038c-126">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="e038c-126">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="e038c-127">Hämtar en borttagen databas som du kan återställa.</span><span class="sxs-lookup"><span data-stu-id="e038c-127">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="e038c-128">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e038c-128">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="e038c-129">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="e038c-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e038c-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e038c-130">Next steps</span></span>

<span data-ttu-id="e038c-131">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e038c-131">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="e038c-132">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e038c-132">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
