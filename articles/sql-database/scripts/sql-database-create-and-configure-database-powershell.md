---
title: aaaPowerShell exempel-skapa en Azure SQL database | Microsoft Docs
description: Azure PowerShell exempel skriptet toocreate en Azure SQL-databas
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
ms.openlocfilehash: ae57b2018f4a550bf2c6da688d6e49bdadf3d3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="a06da-103">Använd PowerShell toocreate en Azure SQL-databas och konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="a06da-103">Use PowerShell toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="a06da-104">Exempel på detta PowerShell-skript skapar en Azure SQL database och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="a06da-104">This PowerShell script example creates an Azure SQL database and configures a server-level firewall rule.</span></span> <span data-ttu-id="a06da-105">När hello skriptet har körts utan problem, konfigurerade hello SQL-databas kan nås från alla Azure-tjänster och hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a06da-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="a06da-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a06da-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a06da-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a06da-107">Clean up deployment</span></span>

<span data-ttu-id="a06da-108">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="a06da-108">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="a06da-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a06da-109">Script explanation</span></span>

<span data-ttu-id="a06da-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="a06da-110">This script uses hello following commands.</span></span> <span data-ttu-id="a06da-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a06da-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a06da-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="a06da-112">Command</span></span> | <span data-ttu-id="a06da-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a06da-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a06da-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a06da-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="a06da-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a06da-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a06da-116">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="a06da-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="a06da-117">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="a06da-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="a06da-118">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="a06da-118">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="a06da-119">Skapar en brandvägg regeln tooallow åtkomst tooall SQL-databaser på hello-servern från hello angivna IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="a06da-119">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="a06da-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="a06da-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="a06da-121">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="a06da-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="a06da-122">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a06da-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="a06da-123">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="a06da-123">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="a06da-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a06da-124">Next steps</span></span>

<span data-ttu-id="a06da-125">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a06da-125">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a06da-126">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a06da-126">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>



