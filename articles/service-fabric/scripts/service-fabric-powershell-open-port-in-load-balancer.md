---
title: "aaaAzure skriptexempel PowerShell - öppna programmet port i belastningsutjämnaren | Microsoft Docs"
description: "Azure PowerShell skriptexempel - öppna en port i hello Azure belastningsutjämnare för ett Service Fabric-program."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 6acb28942851dce63f89f7de362b7cf1dc7b1fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-an-application-port-in-hello-azure-load-balancer"></a><span data-ttu-id="1489a-103">Öppna en port för programmet i hello Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="1489a-103">Open an application port in hello Azure load balancer</span></span>

<span data-ttu-id="1489a-104">Ett Service Fabric-program som körs i Azure är placerad bakom hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="1489a-104">A Service Fabric application running in Azure sits behind hello Azure load balancer.</span></span> <span data-ttu-id="1489a-105">Det här exempelskriptet öppnar en port i en Azure belastningsutjämnare så att ett Service Fabric-program kan kommunicera med externa klienter.</span><span class="sxs-lookup"><span data-stu-id="1489a-105">This sample script opens a port in an Azure load balancer so that a Service Fabric application can communicate with external clients.</span></span> <span data-ttu-id="1489a-106">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="1489a-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="1489a-107">Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1489a-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="1489a-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1489a-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in hello load balancer")]

## <a name="script-explanation"></a><span data-ttu-id="1489a-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1489a-109">Script explanation</span></span>

<span data-ttu-id="1489a-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="1489a-110">This script uses hello following commands.</span></span> <span data-ttu-id="1489a-111">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="1489a-111">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="1489a-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="1489a-112">Command</span></span> | <span data-ttu-id="1489a-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1489a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1489a-114">Get-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="1489a-114">Get-AzureRmResource</span></span>](/powershell/module/azurerm.resources/get-azurermresource) | <span data-ttu-id="1489a-115">Hämtar en Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="1489a-115">Gets an Azure resource.</span></span>  |
| [<span data-ttu-id="1489a-116">Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="1489a-116">Get-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancer) | <span data-ttu-id="1489a-117">Hämtar hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="1489a-117">Gets hello Azure load balancer.</span></span> |
| [<span data-ttu-id="1489a-118">Lägg till AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="1489a-118">Add-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | <span data-ttu-id="1489a-119">Lägger till en belastningsutjämnare för avsökning configuration tooa.</span><span class="sxs-lookup"><span data-stu-id="1489a-119">Adds a probe configuration tooa load balancer.</span></span>|
| [<span data-ttu-id="1489a-120">Get-AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="1489a-120">Get-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | <span data-ttu-id="1489a-121">Hämtar en avsökning konfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="1489a-121">Gets a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="1489a-122">Lägg till AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="1489a-122">Add-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | <span data-ttu-id="1489a-123">Lägger till en belastningsutjämnare för regeln configuration tooa.</span><span class="sxs-lookup"><span data-stu-id="1489a-123">Adds a rule configuration tooa load balancer.</span></span> |
| [<span data-ttu-id="1489a-124">Ange AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="1489a-124">Set-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/set-azurermloadbalancer) | <span data-ttu-id="1489a-125">Anger hello målet tillstånd för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="1489a-125">Sets hello goal state for a load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1489a-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1489a-126">Next steps</span></span>

<span data-ttu-id="1489a-127">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1489a-127">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1489a-128">Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1489a-128">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
