---
title: aaaPowerShell exempel granskning threat detection-Azure SQL Database | Microsoft Docs
description: Azure PowerShell exempel skriptet tooconfigure auditing & threat detection i en Azure SQL Database
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 97e057ac6efe5e730404ae796bc01e7e5c70df35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-sql-database-auditing-and-threat-detection"></a><span data-ttu-id="0626c-103">Använd PowerShell tooconfigure SQL-databas granskning och hotidentifiering identifiering</span><span class="sxs-lookup"><span data-stu-id="0626c-103">Use PowerShell tooconfigure SQL Database auditing and threat detection</span></span>

<span data-ttu-id="0626c-104">Exempel på detta PowerShell-skript konfigurerar gransknings- och hot identifiering av SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0626c-104">This PowerShell script example configures SQL Database auditing and threat detection.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="0626c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0626c-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "Configure auditing and threat detection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0626c-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="0626c-106">Clean up deployment</span></span>

<span data-ttu-id="0626c-107">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="0626c-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="0626c-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0626c-108">Script explanation</span></span>

<span data-ttu-id="0626c-109">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0626c-109">This script uses hello following commands.</span></span> <span data-ttu-id="0626c-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0626c-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0626c-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="0626c-111">Command</span></span> | <span data-ttu-id="0626c-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0626c-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0626c-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0626c-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="0626c-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="0626c-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0626c-115">Ny AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="0626c-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="0626c-116">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="0626c-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="0626c-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="0626c-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="0626c-118">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="0626c-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="0626c-119">Ny AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="0626c-119">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="0626c-120">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0626c-120">Creates a Storage account.</span></span> |
| [<span data-ttu-id="0626c-121">Ange AzureRmSqlDatabaseAuditingPolicy</span><span class="sxs-lookup"><span data-stu-id="0626c-121">Set-AzureRmSqlDatabaseAuditingPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | <span data-ttu-id="0626c-122">Anger hello granskningsprincip för en databas.</span><span class="sxs-lookup"><span data-stu-id="0626c-122">Sets hello auditing policy for a database.</span></span> |
| [<span data-ttu-id="0626c-123">Ange AzureRmSqlDatabaseThreatDetectionPolicy</span><span class="sxs-lookup"><span data-stu-id="0626c-123">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | <span data-ttu-id="0626c-124">Anger en princip för identifiering av hotet för en databas.</span><span class="sxs-lookup"><span data-stu-id="0626c-124">Sets a threat detection policy on a database.</span></span> |
| [<span data-ttu-id="0626c-125">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0626c-125">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="0626c-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="0626c-126">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="0626c-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0626c-127">Next steps</span></span>

<span data-ttu-id="0626c-128">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0626c-128">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="0626c-129">Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0626c-129">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
