---
title: PowerShell exempel georeplikering redundans grupp enda Azure SQL Database | Microsoft Docs
description: "Azure PowerShell-exempelskript för att ställa in aktiv geo-replikering för en enda Azure SQL-databas"
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
ms.openlocfilehash: 8db8540d9c4caeb53ea8f34d45d9498496d2b8ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a><span data-ttu-id="49909-103">Använd PowerShell för att konfigurera en aktiv geo-replikering redundansväxlingsgrupp för en enda Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="49909-103">Use PowerShell to configure an active geo-replication failover group for a single Azure SQL database</span></span>

<span data-ttu-id="49909-104">Det här exemplet för PowerShell-skriptet konfigurerar en aktiv geo-replikering redundansväxlingsgrupp för en enda Azure SQL-databas och växlar till en sekundär replik av Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="49909-104">This PowerShell script example configures an active geo-replication failover group for a single Azure SQL database and fails it over to a secondary replica of the Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="49909-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="49909-105">Sample scripts</span></span>

<span data-ttu-id="49909-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "konfigurera redundansväxlingsgrupp för enskild databas")]</span><span class="sxs-lookup"><span data-stu-id="49909-106">[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="49909-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="49909-107">Clean up deployment</span></span>

<span data-ttu-id="49909-108">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="49909-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="49909-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="49909-109">Script explanation</span></span>

<span data-ttu-id="49909-110">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="49909-110">This script uses the following commands.</span></span> <span data-ttu-id="49909-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="49909-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="49909-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="49909-112">Command</span></span> | <span data-ttu-id="49909-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="49909-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="49909-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="49909-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="49909-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="49909-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="49909-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="49909-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="49909-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="49909-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="49909-118">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="49909-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="49909-119">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="49909-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="49909-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="49909-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="49909-121">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="49909-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="49909-122">Ny AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="49909-122">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="49909-123">Skapar en sekundär databas för en befintlig databas och startar datareplikeringen.</span><span class="sxs-lookup"><span data-stu-id="49909-123">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="49909-124">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="49909-124">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="49909-125">Hämtar en eller flera databaser.</span><span class="sxs-lookup"><span data-stu-id="49909-125">Gets one or more databases.</span></span> |
| [<span data-ttu-id="49909-126">Ange AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="49909-126">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="49909-127">Växlar en sekundär databas för att vara primär initiera växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="49909-127">Switches a secondary database to be primary to initiate failover.</span></span>|
| [<span data-ttu-id="49909-128">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="49909-128">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="49909-129">Hämtar geo-replikeringslänkar mellan en Azure SQL Database och en resursgrupp eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="49909-129">Gets the geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="49909-130">Ta bort AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="49909-130">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="49909-131">Avbryter datareplikering mellan en SQL-databas och den angivna sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="49909-131">Terminates data replication between a SQL Database and the specified secondary database.</span></span> |
| [<span data-ttu-id="49909-132">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="49909-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="49909-133">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="49909-133">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="49909-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49909-134">Next steps</span></span>

<span data-ttu-id="49909-135">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="49909-135">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="49909-136">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="49909-136">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
