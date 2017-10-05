---
title: "Azure PowerShell skriptexempel - tilldela en anpassad domän till en webbapp | Microsoft Docs"
description: "Azure PowerShell skriptexempel - tilldela en anpassad domän till en webbapp"
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
ms.openlocfilehash: 6d25fe8098848fc69470c77e3200bee554c1f875
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="assign-a-custom-domain-to-a-web-app"></a><span data-ttu-id="4fc26-103">Tilldela en anpassad domän till en webbapp</span><span class="sxs-lookup"><span data-stu-id="4fc26-103">Assign a custom domain to a web app</span></span>

<span data-ttu-id="4fc26-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan mappa `www.<yourdomain>` till den.</span><span class="sxs-lookup"><span data-stu-id="4fc26-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span> 

<span data-ttu-id="4fc26-105">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc26-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> <span data-ttu-id="4fc26-106">Dessutom måste ha åtkomst till din domänregistrator DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="4fc26-106">Also, you need to have access to your domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="4fc26-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4fc26-107">Sample script</span></span>

<span data-ttu-id="4fc26-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "tilldela en anpassad domän till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="4fc26-108">[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4fc26-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4fc26-109">Clean up deployment</span></span> 

<span data-ttu-id="4fc26-110">Följande kommando kan användas för att ta bort resursgruppen, webbprogram och alla relaterade resurser efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="4fc26-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="4fc26-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4fc26-111">Script explanation</span></span>

<span data-ttu-id="4fc26-112">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="4fc26-112">This script uses the following commands.</span></span> <span data-ttu-id="4fc26-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4fc26-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4fc26-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="4fc26-114">Command</span></span> | <span data-ttu-id="4fc26-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4fc26-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4fc26-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4fc26-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="4fc26-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4fc26-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4fc26-118">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="4fc26-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="4fc26-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4fc26-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4fc26-120">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="4fc26-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="4fc26-121">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4fc26-121">Creates a web app.</span></span> |
| [<span data-ttu-id="4fc26-122">Ange AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="4fc26-122">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="4fc26-123">Ändrar en App Service-plan för att ändra dess prisnivå.</span><span class="sxs-lookup"><span data-stu-id="4fc26-123">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="4fc26-124">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="4fc26-124">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="4fc26-125">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4fc26-125">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4fc26-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fc26-126">Next steps</span></span>

<span data-ttu-id="4fc26-127">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4fc26-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4fc26-128">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i den [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
