---
title: "aaaAzure PowerShell skriptexempel - tilldela en anpassad domän tooa webbapp | Microsoft Docs"
description: "Azure PowerShell skriptexempel - tilldela en anpassad domän tooa webbapp"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 10224e800588019626ef25cbba4a926096779920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-custom-domain-tooa-web-app"></a><span data-ttu-id="e9219-103">Tilldela en anpassad domän tooa webbapp</span><span class="sxs-lookup"><span data-stu-id="e9219-103">Assign a custom domain tooa web app</span></span>

<span data-ttu-id="e9219-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan mappa `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="e9219-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span> 

<span data-ttu-id="e9219-105">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="e9219-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="e9219-106">Du måste också toohave åtkomst tooyour domän domänregistrators DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="e9219-106">Also, you need toohave access tooyour domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="e9219-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e9219-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="e9219-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="e9219-108">Clean up deployment</span></span> 

<span data-ttu-id="e9219-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e9219-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="e9219-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e9219-110">Script explanation</span></span>

<span data-ttu-id="e9219-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="e9219-111">This script uses hello following commands.</span></span> <span data-ttu-id="e9219-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e9219-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e9219-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="e9219-113">Command</span></span> | <span data-ttu-id="e9219-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9219-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e9219-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e9219-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="e9219-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e9219-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e9219-117">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="e9219-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="e9219-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e9219-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="e9219-119">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="e9219-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="e9219-120">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e9219-120">Creates a web app.</span></span> |
| [<span data-ttu-id="e9219-121">Ange AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="e9219-121">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="e9219-122">Ändrar en App Service-plan toochange dess prisnivå.</span><span class="sxs-lookup"><span data-stu-id="e9219-122">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="e9219-123">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="e9219-123">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="e9219-124">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e9219-124">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e9219-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9219-125">Next steps</span></span>

<span data-ttu-id="e9219-126">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e9219-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="e9219-127">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e9219-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
