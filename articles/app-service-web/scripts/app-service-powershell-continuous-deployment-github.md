---
title: "aaaAzure PowerShell skriptexempel - skapa en webbapp med kontinuerlig distribution från GitHub | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en webbapp med kontinuerlig distribution från GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c2f260a06bce9af6d11ad4033931d3dc18da8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="bf681-103">Skapa en webbapp med kontinuerlig distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="bf681-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="bf681-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan konfigurerar kontinuerlig distribution från en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="bf681-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="bf681-105">GitHub distribution utan kontinuerlig distribution finns [skapa en webbapp och distribuera kod från GitHub](app-service-powershell-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="bf681-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-powershell-deploy-github.md).</span></span>

<span data-ttu-id="bf681-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf681-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="bf681-107">Se också till att:</span><span class="sxs-lookup"><span data-stu-id="bf681-107">Also, ensure that:</span></span>

- <span data-ttu-id="bf681-108">En anslutning med Azure har skapats med hjälp av hello `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="bf681-108">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="bf681-109">hello programkod är i ett offentligt eller privat GitHub-databas som du äger.</span><span class="sxs-lookup"><span data-stu-id="bf681-109">hello application code is in a public or private GitHub repository that you own.</span></span>
- <span data-ttu-id="bf681-110">Du har [skapas en åtkomst-token i GitHub-konto](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span><span class="sxs-lookup"><span data-stu-id="bf681-110">You have [created an access token in your GitHub account](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).</span></span>

## <a name="sample-script"></a><span data-ttu-id="bf681-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="bf681-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="bf681-112">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="bf681-112">Clean up deployment</span></span> 

<span data-ttu-id="bf681-113">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="bf681-113">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="bf681-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="bf681-114">Script explanation</span></span>

<span data-ttu-id="bf681-115">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="bf681-115">This script uses hello following commands.</span></span> <span data-ttu-id="bf681-116">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bf681-116">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bf681-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="bf681-117">Command</span></span> | <span data-ttu-id="bf681-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bf681-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bf681-119">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="bf681-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="bf681-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="bf681-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bf681-121">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="bf681-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="bf681-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="bf681-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="bf681-123">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="bf681-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="bf681-124">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bf681-124">Creates a web app.</span></span> |
| [<span data-ttu-id="bf681-125">Ange AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="bf681-125">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="bf681-126">Ändrar en resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bf681-126">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bf681-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf681-127">Next steps</span></span>

<span data-ttu-id="bf681-128">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf681-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bf681-129">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bf681-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
