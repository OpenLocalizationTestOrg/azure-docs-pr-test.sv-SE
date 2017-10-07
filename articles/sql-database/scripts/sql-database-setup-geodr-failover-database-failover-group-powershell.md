---
title: aaaPowerShell exempel georeplikering redundans grupp enda Azure SQL Database | Microsoft Docs
description: "Azure PowerShell exempel skriptet tooset in aktiv geo-replikering för en enda Azure SQL-databas"
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
ms.openlocfilehash: 0ea7afb1125b95370811ef7f80cb9eb7391b2443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a><span data-ttu-id="dca43-103">Använd PowerShell tooconfigure redundansväxlingsgrupp en aktiv geo-replikering för en enda Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="dca43-103">Use PowerShell tooconfigure an active geo-replication failover group for a single Azure SQL database</span></span>

<span data-ttu-id="dca43-104">Det här exemplet för PowerShell-skriptet konfigurerar en aktiv geo-replikering redundansväxlingsgrupp för en enda Azure SQL-databas och växlar tooa sekundär replik av hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="dca43-104">This PowerShell script example configures an active geo-replication failover group for a single Azure SQL database and fails it over tooa secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="dca43-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="dca43-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="dca43-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="dca43-106">Clean up deployment</span></span>

<span data-ttu-id="dca43-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="dca43-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="dca43-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="dca43-108">Script explanation</span></span>

<span data-ttu-id="dca43-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="dca43-109">This script uses hello following commands.</span></span> <span data-ttu-id="dca43-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="dca43-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dca43-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="dca43-111">Command</span></span> | <span data-ttu-id="dca43-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dca43-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dca43-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dca43-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="dca43-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="dca43-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dca43-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="dca43-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="dca43-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="dca43-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="dca43-117">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="dca43-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="dca43-118">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="dca43-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="dca43-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="dca43-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="dca43-120">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="dca43-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="dca43-121">Ny AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="dca43-121">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="dca43-122">Skapar en sekundär databas för en befintlig databas och startar datareplikeringen.</span><span class="sxs-lookup"><span data-stu-id="dca43-122">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="dca43-123">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="dca43-123">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="dca43-124">Hämtar en eller flera databaser.</span><span class="sxs-lookup"><span data-stu-id="dca43-124">Gets one or more databases.</span></span> |
| [<span data-ttu-id="dca43-125">Ange AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="dca43-125">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="dca43-126">Växlar en sekundär databas toobe primära tooinitiate redundans.</span><span class="sxs-lookup"><span data-stu-id="dca43-126">Switches a secondary database toobe primary tooinitiate failover.</span></span>|
| [<span data-ttu-id="dca43-127">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="dca43-127">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="dca43-128">Hämtar hello geo-replikeringslänkar mellan en Azure SQL Database och en resursgrupp eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dca43-128">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="dca43-129">Ta bort AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="dca43-129">Remove-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | <span data-ttu-id="dca43-130">Avbryter datareplikering mellan en SQL-databas och hello angivna sekundära databasen.</span><span class="sxs-lookup"><span data-stu-id="dca43-130">Terminates data replication between a SQL Database and hello specified secondary database.</span></span> |
| [<span data-ttu-id="dca43-131">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dca43-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="dca43-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="dca43-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="dca43-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dca43-133">Next steps</span></span>

<span data-ttu-id="dca43-134">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dca43-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="dca43-135">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dca43-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
