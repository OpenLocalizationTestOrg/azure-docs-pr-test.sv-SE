---
title: "Azure PowerShell-skript exempel – skala ett webbprogram manuellt | Microsoft Docs"
description: "Azure PowerShell-skript exempel – skala ett webbprogram manuellt"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: e99dfc02b6ab4123cd5f95997285dca5cb686380
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="0b5eb-103">Skala ett webbprogram manuellt</span><span class="sxs-lookup"><span data-stu-id="0b5eb-103">Scale a web app manually</span></span>

<span data-ttu-id="0b5eb-104">I det här scenariot du lär dig att skapa en resursgrupp, en app service-plan och webb-app.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-104">In this scenario you will learn to create a resource group, app service plan and web app.</span></span> <span data-ttu-id="0b5eb-105">Du kommer sedan att skala i App Service-Plan från en enda instans till flera instanser.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-105">You will then scale the App Service Plan from a single instance to multiple instances.</span></span>

<span data-ttu-id="0b5eb-106">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="0b5eb-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0b5eb-107">Sample script</span></span>

<span data-ttu-id="0b5eb-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "skala ett webbprogram manuellt")]</span><span class="sxs-lookup"><span data-stu-id="0b5eb-108">[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="0b5eb-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="0b5eb-109">Clean up deployment</span></span> 

<span data-ttu-id="0b5eb-110">Följande kommando kan användas för att ta bort resursgruppen, webbprogram och alla relaterade resurser efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="0b5eb-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0b5eb-111">Script explanation</span></span>

<span data-ttu-id="0b5eb-112">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-112">This script uses the following commands.</span></span> <span data-ttu-id="0b5eb-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="0b5eb-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="0b5eb-114">Command</span></span> | <span data-ttu-id="0b5eb-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0b5eb-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0b5eb-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0b5eb-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="0b5eb-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0b5eb-118">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="0b5eb-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="0b5eb-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="0b5eb-120">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="0b5eb-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="0b5eb-121">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-121">Creates a web app.</span></span> |
| [<span data-ttu-id="0b5eb-122">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="0b5eb-122">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="0b5eb-123">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0b5eb-123">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0b5eb-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b5eb-124">Next steps</span></span>

<span data-ttu-id="0b5eb-125">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0b5eb-125">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="0b5eb-126">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i den [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0b5eb-126">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
