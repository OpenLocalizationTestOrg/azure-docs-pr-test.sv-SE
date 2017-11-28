---
title: "aaaPowerShell exempel-restore-säkerhetskopiering – Azure SQL database | Microsoft Docs"
description: "Azure PowerShell exempel skriptet toorestore en Azure SQL database från säkerhetskopior geo-redundant"
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
ms.openlocfilehash: 68becb89e8a8680aa2efc3de8ad947e674c5fc35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toorestore-an-azure-sql-database-from-backups"></a><span data-ttu-id="8f5ec-103">Använd PowerShell toorestore en Azure SQL database från säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="8f5ec-103">Use PowerShell toorestore an Azure SQL database from backups</span></span>

<span data-ttu-id="8f5ec-104">Exempel på detta PowerShell-skript återställer en Azure SQL-databas från en geo-redundant säkerhetskopia, återställer en borttagen Azure SQL database tooits senaste säkerhetskopiering och återställer en viss punkt för Azure SQL database tooa i tid.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database tooits latest backup, and restores an Azure SQL database tooa specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="8f5ec-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8f5ec-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8f5ec-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8f5ec-106">Clean up deployment</span></span>

<span data-ttu-id="8f5ec-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="8f5ec-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8f5ec-108">Script explanation</span></span>

<span data-ttu-id="8f5ec-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-109">This script uses hello following commands.</span></span> <span data-ttu-id="8f5ec-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8f5ec-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="8f5ec-111">Command</span></span> | <span data-ttu-id="8f5ec-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8f5ec-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8f5ec-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8f5ec-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="8f5ec-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-114">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="8f5ec-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="8f5ec-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="8f5ec-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-116">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="8f5ec-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="8f5ec-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="8f5ec-118">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="8f5ec-119">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="8f5ec-119">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="8f5ec-120">Hämtar en geo-redundant säkerhetskopia av en databas.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-120">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="8f5ec-121">Restore-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="8f5ec-121">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="8f5ec-122">Återställer en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-122">Restores a SQL database.</span></span> |
|[<span data-ttu-id="8f5ec-123">Ta bort-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="8f5ec-123">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="8f5ec-124">Tar bort en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-124">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="8f5ec-125">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="8f5ec-125">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="8f5ec-126">Hämtar en borttagen databas som du kan återställa.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-126">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="8f5ec-127">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8f5ec-127">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="8f5ec-128">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="8f5ec-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8f5ec-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f5ec-129">Next steps</span></span>

<span data-ttu-id="8f5ec-130">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8f5ec-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8f5ec-131">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8f5ec-131">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
