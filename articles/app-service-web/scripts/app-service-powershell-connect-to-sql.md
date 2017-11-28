---
title: aaaAzure PowerShell skriptexempel - ansluta en web app tooa SQL-databas | Microsoft Docs
description: Azure PowerShell-skript Sample - Anslut en web app tooa SQL-databas
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8f80b940378d020cbcaec2c1bbc28bae1a3ef35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="d4969-103">Ansluta en web app tooa SQL-databas</span><span class="sxs-lookup"><span data-stu-id="d4969-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="d4969-104">I det här scenariot du lära dig hur toocreate Azure SQL-databas och ett Azure web app.</span><span class="sxs-lookup"><span data-stu-id="d4969-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="d4969-105">Du kan länka hello SQL-databasen toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="d4969-105">Then you will link hello SQL database toohello web app using app settings.</span></span>

<span data-ttu-id="d4969-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="d4969-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d4969-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d4969-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app tooa SQL database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d4969-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d4969-108">Clean up deployment</span></span> 

<span data-ttu-id="d4969-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="d4969-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="d4969-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d4969-110">Script explanation</span></span>

<span data-ttu-id="d4969-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="d4969-111">This script uses hello following commands.</span></span> <span data-ttu-id="d4969-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d4969-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d4969-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="d4969-113">Command</span></span> | <span data-ttu-id="d4969-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d4969-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d4969-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d4969-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d4969-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d4969-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d4969-117">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="d4969-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="d4969-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="d4969-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="d4969-119">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d4969-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="d4969-120">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d4969-120">Creates a web app.</span></span> |
| [<span data-ttu-id="d4969-121">Ny AzureRMSQLServer</span><span class="sxs-lookup"><span data-stu-id="d4969-121">New-AzureRMSQLServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="d4969-122">Skapar en SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="d4969-122">Creates a SQL Database server.</span></span> |
| [<span data-ttu-id="d4969-123">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="d4969-123">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="d4969-124">Skapar en brandväggsregel för en SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="d4969-124">Creates a firewall rule for a SQL Database server.</span></span> |
| [<span data-ttu-id="d4969-125">Ny AzureRMSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="d4969-125">New-AzureRMSQLDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="d4969-126">Skapar en databas eller en elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="d4969-126">Creates a database or an elastic database.</span></span> |
| [<span data-ttu-id="d4969-127">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d4969-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="d4969-128">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d4969-128">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d4969-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4969-129">Next steps</span></span>

<span data-ttu-id="d4969-130">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d4969-130">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d4969-131">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d4969-131">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
