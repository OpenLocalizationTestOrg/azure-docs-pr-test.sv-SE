---
title: "aaaAzure skriptexempel PowerShell - vägtrafik för hög tillgänglighet för program | Microsoft Docs"
description: "Azure PowerShell skriptexempel - vägtrafik för hög tillgänglighet för program"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 11d15780403b4ed79e85d7b3495bc5d674bfdaee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="73abe-103">Vidarebefordra trafik för hög tillgänglighet för program</span><span class="sxs-lookup"><span data-stu-id="73abe-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="73abe-104">Det här skriptet skapar en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="73abe-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="73abe-105">Traffic Manager dirigerar trafik toohello programmet i en region som hello primär region och toohello sekundär region när programmet hello i hello primära regionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="73abe-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="73abe-106">Innan du kör skriptet hello måste du ändra hello MyWebApp, MyWebAppL1 och MyWebAppL2 värdena toounique värdena i Azure.</span><span class="sxs-lookup"><span data-stu-id="73abe-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="73abe-107">Du kan komma åt hello app i hello primär region med hello URL mywebapp.trafficmanager.net när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="73abe-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

<span data-ttu-id="73abe-108">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="73abe-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="73abe-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="73abe-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


<span data-ttu-id="73abe-110">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="73abe-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a><span data-ttu-id="73abe-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="73abe-111">Script explanation</span></span>

<span data-ttu-id="73abe-112">Det här skriptet använder följande kommandon toocreate en resursgrupp, webbprogram, trafikhanterarprofilen hello och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="73abe-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="73abe-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="73abe-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="73abe-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="73abe-114">Command</span></span> | <span data-ttu-id="73abe-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="73abe-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73abe-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="73abe-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="73abe-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="73abe-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="73abe-118">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="73abe-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="73abe-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="73abe-119">Creates an App Service plan.</span></span> <span data-ttu-id="73abe-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="73abe-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="73abe-121">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="73abe-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="73abe-122">Skapar en Azure webbapp i hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="73abe-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="73abe-123">Ange AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="73abe-123">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/new-azurermresource) | <span data-ttu-id="73abe-124">Skapar en Azure webbapp i hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="73abe-124">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="73abe-125">Ny AzureRmTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="73abe-125">New-AzureRmTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="73abe-126">Skapar en Azure Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="73abe-126">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="73abe-127">Ny AzureRmTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="73abe-127">New-AzureRmTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="73abe-128">Lägger till en slutpunkt tooan Azure Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="73abe-128">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73abe-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73abe-129">Next steps</span></span>

<span data-ttu-id="73abe-130">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73abe-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="73abe-131">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73abe-131">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
