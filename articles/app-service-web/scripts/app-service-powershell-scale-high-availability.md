---
title: "aaaAzure PowerShell skriptexempel - skala ett webbprogram över hela världen med en hög tillgänglighet-arkitektur | Microsoft Docs"
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
ms.openlocfilehash: 1fcda23250efe4966d63c5dfa744b76c26f3762a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="8d877-103">Skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="8d877-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="8d877-104">I det här scenariot skapar du en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8d877-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="8d877-105">När hello övningen är klar har du en hög tillgänglighet arkitektur som gör att tillhandahåller globala tillgängligheten hos ditt webbprogram baserat på hello lägsta Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="8d877-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

<span data-ttu-id="8d877-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="8d877-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="8d877-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8d877-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8d877-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8d877-108">Clean up deployment</span></span> 

<span data-ttu-id="8d877-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8d877-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="8d877-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8d877-110">Script explanation</span></span>

<span data-ttu-id="8d877-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="8d877-111">This script uses hello following commands.</span></span> <span data-ttu-id="8d877-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8d877-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8d877-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="8d877-113">Command</span></span> | <span data-ttu-id="8d877-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8d877-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8d877-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8d877-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="8d877-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8d877-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8d877-117">Ny AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="8d877-117">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="8d877-118">Skapar en Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="8d877-118">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="8d877-119">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="8d877-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="8d877-120">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="8d877-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="8d877-121">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="8d877-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="8d877-122">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8d877-122">Creates a web app.</span></span> |
| [<span data-ttu-id="8d877-123">Ny AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="8d877-123">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="8d877-124">Skapar en slutpunkt i Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="8d877-124">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8d877-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d877-125">Next steps</span></span>

<span data-ttu-id="8d877-126">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8d877-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="8d877-127">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8d877-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
