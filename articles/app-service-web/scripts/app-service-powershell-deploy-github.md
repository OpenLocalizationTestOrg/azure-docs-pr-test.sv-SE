---
title: "aaaAzure PowerShell skriptexempel - skapa en webbapp och distribuera kod från GitHub | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skapa en webbapp och distribuera kod från GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 9a28f9cb01b71c86fa0a3f1d0a6761fc3d45d43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a><span data-ttu-id="67f47-103">Skapa en webbapp och distribuera kod från GitHub</span><span class="sxs-lookup"><span data-stu-id="67f47-103">Create a web app and deploy code from GitHub</span></span>

<span data-ttu-id="67f47-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan web app-kod från en offentlig GitHub-databas (utan kontinuerlig distribution).</span><span class="sxs-lookup"><span data-stu-id="67f47-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="67f47-105">GitHub-distribution med kontinuerlig distribution finns [skapa ett webbprogram med kontinuerlig distribution från GitHub](app-service-powershell-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="67f47-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-powershell-continuous-deployment-github.md).</span></span>

<span data-ttu-id="67f47-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67f47-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="67f47-107">Du måste även en länk tooGitHub databas som innehåller hello web app-kod.</span><span class="sxs-lookup"><span data-stu-id="67f47-107">Also, you need a link tooGitHub repository that contains hello web app code.</span></span>

## <a name="sample-script"></a><span data-ttu-id="67f47-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="67f47-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="67f47-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="67f47-109">Clean up deployment</span></span> 

<span data-ttu-id="67f47-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="67f47-110">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="67f47-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="67f47-111">Script explanation</span></span>

<span data-ttu-id="67f47-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="67f47-112">This script uses hello following commands.</span></span> <span data-ttu-id="67f47-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="67f47-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="67f47-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="67f47-114">Command</span></span> | <span data-ttu-id="67f47-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="67f47-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="67f47-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="67f47-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="67f47-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="67f47-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="67f47-118">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="67f47-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="67f47-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="67f47-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="67f47-120">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="67f47-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="67f47-121">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="67f47-121">Creates a web app.</span></span> |
| [<span data-ttu-id="67f47-122">Ange AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="67f47-122">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="67f47-123">Ändrar en resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="67f47-123">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="67f47-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67f47-124">Next steps</span></span>

<span data-ttu-id="67f47-125">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67f47-125">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="67f47-126">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="67f47-126">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
