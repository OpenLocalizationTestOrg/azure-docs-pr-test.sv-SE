---
title: "Azure PowerShell skriptexempel - skala en webbapp över hela världen med en hög tillgänglighet-arkitektur | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skala en webbapp över hela världen med en arkitektur med hög tillgänglighet"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 9acd1cf4d1a5705811c4dedc545505ec0ac55fc7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="0b984-103">Skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="0b984-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="0b984-104">I det här scenariot skapar du en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="0b984-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="0b984-105">När den här övningen är klar har du en hög tillgänglighet arkitektur som gör att tillhandahåller globala tillgängligheten hos ditt webbprogram baserat på den lägsta Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="0b984-105">Once the exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on the lowest network latency.</span></span>

<span data-ttu-id="0b984-106">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="0b984-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="0b984-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0b984-107">Sample script</span></span>

<span data-ttu-id="0b984-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet")]</span><span class="sxs-lookup"><span data-stu-id="0b984-108">[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="0b984-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="0b984-109">Clean up deployment</span></span> 

<span data-ttu-id="0b984-110">Följande kommando kan användas för att ta bort resursgruppen, webbprogram och alla relaterade resurser efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="0b984-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="0b984-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0b984-111">Script explanation</span></span>

<span data-ttu-id="0b984-112">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0b984-112">This script uses the following commands.</span></span> <span data-ttu-id="0b984-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0b984-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="0b984-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="0b984-114">Command</span></span> | <span data-ttu-id="0b984-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0b984-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0b984-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0b984-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="0b984-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="0b984-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0b984-118">Ny AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="0b984-118">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="0b984-119">Skapar en Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="0b984-119">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="0b984-120">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="0b984-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="0b984-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0b984-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="0b984-122">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="0b984-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="0b984-123">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0b984-123">Creates a web app.</span></span> |
| [<span data-ttu-id="0b984-124">Ny AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="0b984-124">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="0b984-125">Skapar en slutpunkt i Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="0b984-125">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0b984-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b984-126">Next steps</span></span>

<span data-ttu-id="0b984-127">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0b984-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="0b984-128">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i den [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0b984-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
