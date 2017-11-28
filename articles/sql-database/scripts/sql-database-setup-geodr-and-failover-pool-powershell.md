---
title: aaaPowerShell exempel aktiv geo-replikering-pooler Azure SQL database | Microsoft Docs
description: "Azure PowerShell exempel skriptet tooset in aktiv geo-replikering för en pool Azure SQL-databas"
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/25/2017
ms.author: carlrab
ms.openlocfilehash: 9d183f08dcc07ba864e42fe70a562fef8bd572f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-active-geo-replication-for-a-pooled-azure-sql-database"></a><span data-ttu-id="c7b04-103">Använd PowerShell tooconfigure aktiv geo-replikering för en pool Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c7b04-103">Use PowerShell tooconfigure active geo-replication for a pooled Azure SQL database</span></span>

<span data-ttu-id="c7b04-104">Det här exemplet för PowerShell-skriptet konfigurerar aktiv geo-replikering för en Azure SQL database i en elastisk pool och växlar toohello sekundär replik av hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c7b04-104">This PowerShell script example configures active geo-replication for an Azure SQL database in an elastic pool and fails it over toohello secondary replica of hello Azure SQL database.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a><span data-ttu-id="c7b04-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c7b04-105">Sample scripts</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-pool/setup-geodr-and-failover-pool.ps1?highlight=16-19 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a><span data-ttu-id="c7b04-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="c7b04-106">Clean up deployment</span></span>

<span data-ttu-id="c7b04-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="c7b04-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="c7b04-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c7b04-108">Script explanation</span></span>

<span data-ttu-id="c7b04-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c7b04-109">This script uses hello following commands.</span></span> <span data-ttu-id="c7b04-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c7b04-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c7b04-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="c7b04-111">Command</span></span> | <span data-ttu-id="c7b04-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c7b04-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c7b04-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c7b04-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c7b04-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c7b04-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c7b04-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="c7b04-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="c7b04-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="c7b04-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="c7b04-117">Ny AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="c7b04-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="c7b04-118">Skapar en elastisk pool i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="c7b04-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="c7b04-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c7b04-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="c7b04-120">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="c7b04-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="c7b04-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c7b04-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="c7b04-122">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="c7b04-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="c7b04-123">Ny AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="c7b04-123">New-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| <span data-ttu-id="c7b04-124">Skapar en sekundär databas för en befintlig databas och startar datareplikeringen.</span><span class="sxs-lookup"><span data-stu-id="c7b04-124">Creates a secondary database for an existing database and starts data replication.</span></span> |
| [<span data-ttu-id="c7b04-125">Get-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="c7b04-125">Get-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabase)| <span data-ttu-id="c7b04-126">Hämtar en eller flera databaser.</span><span class="sxs-lookup"><span data-stu-id="c7b04-126">Gets one or more databases.</span></span> |
| [<span data-ttu-id="c7b04-127">Ange AzureRmSqlDatabaseSecondary</span><span class="sxs-lookup"><span data-stu-id="c7b04-127">Set-AzureRmSqlDatabaseSecondary</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| <span data-ttu-id="c7b04-128">Växlar en sekundär databas toobe primär i ordning tooinitiate redundans.</span><span class="sxs-lookup"><span data-stu-id="c7b04-128">Switches a secondary database toobe primary in order tooinitiate failover.</span></span>|
| [<span data-ttu-id="c7b04-129">Get-AzureRmSqlDatabaseReplicationLink</span><span class="sxs-lookup"><span data-stu-id="c7b04-129">Get-AzureRmSqlDatabaseReplicationLink</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | <span data-ttu-id="c7b04-130">Hämtar hello geo-replikeringslänkar mellan en Azure SQL Database och en resursgrupp eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c7b04-130">Gets hello geo-replication links between an Azure SQL Database and a resource group or SQL Server.</span></span> |
| [<span data-ttu-id="c7b04-131">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c7b04-131">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="c7b04-132">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="c7b04-132">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="c7b04-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7b04-133">Next steps</span></span>

<span data-ttu-id="c7b04-134">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c7b04-134">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c7b04-135">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c7b04-135">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
