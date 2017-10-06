---
title: "aaaAzure PowerShell skriptexempel - skapa en webbapp och distribuera kod tooa mellanlagring miljö | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skapa en webbapp och distribuera kod tooa mellanlagring miljö"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 5c74b962955770637173f1fd4f49342fec54ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="59912-103">Skapa en webbapp och distribuera kod tooa mellanlagring miljö</span><span class="sxs-lookup"><span data-stu-id="59912-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="59912-104">Det här exempelskriptet skapar en webbapp i App Service med en ytterligare distributionsplatsen som kallas ”mellanlagring” och sedan distribuerar ett exempel app toohello ”mellanlagringsplatsen”.</span><span class="sxs-lookup"><span data-stu-id="59912-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

<span data-ttu-id="59912-105">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="59912-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="59912-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="59912-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code tooa staging environment")]

## <a name="clean-up-deployment"></a><span data-ttu-id="59912-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="59912-107">Clean up deployment</span></span> 

<span data-ttu-id="59912-108">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="59912-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="59912-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="59912-109">Script explanation</span></span>

<span data-ttu-id="59912-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="59912-110">This script uses hello following commands.</span></span> <span data-ttu-id="59912-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="59912-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="59912-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="59912-112">Command</span></span> | <span data-ttu-id="59912-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="59912-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="59912-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="59912-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="59912-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="59912-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="59912-116">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="59912-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="59912-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="59912-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="59912-118">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="59912-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="59912-119">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="59912-119">Creates a web app.</span></span> |
| [<span data-ttu-id="59912-120">Ange AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="59912-120">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="59912-121">Ändrar en App Service-plan toochange dess prisnivå.</span><span class="sxs-lookup"><span data-stu-id="59912-121">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="59912-122">Ny AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="59912-122">New-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/new-azurermwebappslot) | <span data-ttu-id="59912-123">Skapar en distributionsplats för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="59912-123">Creates a deployment slot for a web app.</span></span> |
| [<span data-ttu-id="59912-124">Ange AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="59912-124">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="59912-125">Ändrar en resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="59912-125">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="59912-126">Växlingen AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="59912-126">Swap-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/swap-azurermwebappslot) | <span data-ttu-id="59912-127">Byter ut en webbapp distributionsplatsen till produktionen.</span><span class="sxs-lookup"><span data-stu-id="59912-127">Swaps a web app's deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="59912-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59912-128">Next steps</span></span>

<span data-ttu-id="59912-129">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="59912-129">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="59912-130">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="59912-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
