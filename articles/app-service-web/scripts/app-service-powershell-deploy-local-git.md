---
title: "aaaAzure PowerShell skriptexempel - skapa en webbapp och distribuera kod från en lokal Git-lagringsplats | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skapa en webbapp och distribuera kod från en lokal Git-lagringsplats"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a90996658bf0b609315460324d0dcd3a411a6512
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="c1eed-103">Skapa en webbapp och distribuera kod från en lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="c1eed-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="c1eed-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan web app-kod i en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="c1eed-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>

<span data-ttu-id="c1eed-105">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="c1eed-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="c1eed-106">Dessutom måste din programkod toobe som allokerats till en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="c1eed-106">Also, your application code needs toobe committed into a local Git repository.</span></span>

## <a name="sample-script"></a><span data-ttu-id="c1eed-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c1eed-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a><span data-ttu-id="c1eed-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="c1eed-108">Clean up deployment</span></span> 

<span data-ttu-id="c1eed-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c1eed-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="c1eed-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c1eed-110">Script explanation</span></span>

<span data-ttu-id="c1eed-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c1eed-111">This script uses hello following commands.</span></span> <span data-ttu-id="c1eed-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c1eed-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c1eed-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="c1eed-113">Command</span></span> | <span data-ttu-id="c1eed-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c1eed-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c1eed-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c1eed-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c1eed-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c1eed-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c1eed-117">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c1eed-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="c1eed-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="c1eed-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c1eed-119">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="c1eed-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="c1eed-120">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c1eed-120">Creates a web app.</span></span> |
| [<span data-ttu-id="c1eed-121">Ange AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="c1eed-121">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="c1eed-122">Ändrar en resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c1eed-122">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="c1eed-123">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="c1eed-123">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="c1eed-124">Hämta publiceringsprofilen för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c1eed-124">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c1eed-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1eed-125">Next steps</span></span>

<span data-ttu-id="c1eed-126">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1eed-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c1eed-127">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c1eed-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
