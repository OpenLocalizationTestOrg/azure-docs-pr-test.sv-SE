---
title: PowerShell exempel-skapa en Azure SQL database | Microsoft Docs
description: "Azure PowerShell-exempelskript för att skapa en Azure SQL-databas"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1b6809007e6717b7f8847452b2fa5b38d4ab5ccc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="8d204-103">Använda PowerShell för att skapa en Azure SQL-databas och konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="8d204-103">Use PowerShell to create a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="8d204-104">Exempel på detta PowerShell-skript skapar en Azure SQL database och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="8d204-104">This PowerShell script example creates an Azure SQL database and configures a server-level firewall rule.</span></span> <span data-ttu-id="8d204-105">När skriptet har körts utan problem, kan SQL-databasen nås från alla Azure-tjänster och den konfigurerade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="8d204-105">Once the script has been successfully run, the SQL Database can be accessed from all Azure services and the configured IP address.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="8d204-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8d204-106">Sample script</span></span>

<span data-ttu-id="8d204-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "skapa SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="8d204-107">[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8d204-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8d204-108">Clean up deployment</span></span>

<span data-ttu-id="8d204-109">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="8d204-109">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="8d204-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8d204-110">Script explanation</span></span>

<span data-ttu-id="8d204-111">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="8d204-111">This script uses the following commands.</span></span> <span data-ttu-id="8d204-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8d204-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8d204-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="8d204-113">Command</span></span> | <span data-ttu-id="8d204-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8d204-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8d204-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8d204-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8d204-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8d204-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8d204-117">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="8d204-117">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="8d204-118">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="8d204-118">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="8d204-119">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="8d204-119">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="8d204-120">Skapar en brandväggsregel för att tillåta åtkomst till alla SQL-databaser på servern från det angivna IP-adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="8d204-120">Creates a firewall rule to allow access to all SQL Databases on the server from the entered IP address range.</span></span> |
| [<span data-ttu-id="8d204-121">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="8d204-121">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="8d204-122">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="8d204-122">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="8d204-123">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8d204-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="8d204-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="8d204-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="8d204-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d204-125">Next steps</span></span>

<span data-ttu-id="8d204-126">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8d204-126">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8d204-127">Ytterligare exempel för SQL Database PowerShell-skript finns i den [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8d204-127">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>



