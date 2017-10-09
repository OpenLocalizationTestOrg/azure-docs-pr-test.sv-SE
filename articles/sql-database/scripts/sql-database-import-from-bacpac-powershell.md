---
title: aaaPowerShell exempel-import-bacpac filen Azure SQL database | Microsoft Docs
description: Azure PowerShell exempel skriptet tooimport en bacpac panelen till en SQL-databas
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: b85fca1c7fd52037d74254980469f9f53906448e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooimport-a-bacpac-file-into-an-azure-sql-database"></a><span data-ttu-id="23d7c-103">Använd PowerShell tooimport en bacpac-fil till en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="23d7c-103">Use PowerShell tooimport a bacpac file into an Azure SQL database</span></span>

<span data-ttu-id="23d7c-104">Exempel på detta PowerShell-skript importerar en databas från en **bacpac** filen till en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="23d7c-104">This PowerShell script example imports a database from a **bacpac** file into an Azure SQL database.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="23d7c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="23d7c-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="23d7c-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="23d7c-106">Clean up deployment</span></span>

<span data-ttu-id="23d7c-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="23d7c-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="23d7c-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="23d7c-108">Script explanation</span></span>

<span data-ttu-id="23d7c-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="23d7c-109">This script uses hello following commands.</span></span> <span data-ttu-id="23d7c-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="23d7c-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="23d7c-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="23d7c-111">Command</span></span> | <span data-ttu-id="23d7c-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="23d7c-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="23d7c-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="23d7c-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="23d7c-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="23d7c-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="23d7c-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="23d7c-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="23d7c-116">Skapar en logisk server att värdar hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="23d7c-116">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="23d7c-117">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="23d7c-117">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="23d7c-118">Skapar en brandvägg regeln tooallow åtkomst tooall SQL-databaser på hello-servern från hello angivna IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="23d7c-118">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="23d7c-119">Ny AzureRmSqlDatabaseImport</span><span class="sxs-lookup"><span data-stu-id="23d7c-119">New-AzureRmSqlDatabaseImport</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | <span data-ttu-id="23d7c-120">Importerar en .bacpac fil- och skapa en ny databas på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="23d7c-120">Imports a .bacpac file and create a new database on hello server.</span></span> |
| [<span data-ttu-id="23d7c-121">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="23d7c-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="23d7c-122">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="23d7c-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="23d7c-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23d7c-123">Next steps</span></span>

<span data-ttu-id="23d7c-124">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="23d7c-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="23d7c-125">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="23d7c-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
