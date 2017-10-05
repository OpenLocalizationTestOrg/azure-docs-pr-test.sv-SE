---
title: "Skriptexempel Azure PowerShell - vägtrafik via en virtuell nätverksenhet | Microsoft Docs"
description: "Azure PowerShell skriptexempel - vägtrafik via en virtuell nätverksenhet för brandväggen."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 883d28dac72a66c2186d222f72b04d68e532cead
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="1a8b9-103">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="1a8b9-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="1a8b9-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="1a8b9-105">Dessutom skapas en virtuell dator med IP-vidarebefordring är aktiverat för att vidarebefordra trafik mellan två undernät.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-105">It also creates a VM with IP forwarding enabled to route traffic between the two subnets.</span></span> <span data-ttu-id="1a8b9-106">Du kan distribuera programvara för nätverk, till exempel en brandvägg för programmet, till den virtuella datorn när skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-106">After running the script you can deploy network software, such as a firewall application, to the VM.</span></span>

<span data-ttu-id="1a8b9-107">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-107">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1a8b9-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1a8b9-108">Sample script</span></span>


<span data-ttu-id="1a8b9-109">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "dirigera trafik via en virtuell nätverksenhet")]</span><span class="sxs-lookup"><span data-stu-id="1a8b9-109">[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1a8b9-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="1a8b9-110">Clean up deployment</span></span> 

<span data-ttu-id="1a8b9-111">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a><span data-ttu-id="1a8b9-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1a8b9-112">Script explanation</span></span>

<span data-ttu-id="1a8b9-113">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-113">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="1a8b9-114">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="1a8b9-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="1a8b9-115">Command</span></span> | <span data-ttu-id="1a8b9-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a8b9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1a8b9-117">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a8b9-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="1a8b9-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1a8b9-119">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="1a8b9-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="1a8b9-120">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-120">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="1a8b9-121">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1a8b9-121">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="1a8b9-122">Skapar backend- och DMZ undernät.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-122">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="1a8b9-123">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="1a8b9-123">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="1a8b9-124">Skapar en offentlig IP-adress för att komma åt den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-124">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="1a8b9-125">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="1a8b9-125">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="1a8b9-126">Skapar ett virtuellt nätverksgränssnitt och aktivera IP-vidarebefordring för den.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-126">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="1a8b9-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="1a8b9-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="1a8b9-128">Skapar en nätverkssäkerhetsgrupp (NSG).</span><span class="sxs-lookup"><span data-stu-id="1a8b9-128">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="1a8b9-129">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="1a8b9-129">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="1a8b9-130">Skapar NSG-regler som tillåter HTTP och HTTPS-portar inkommande till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-130">Creates NSG rules that allow HTTP and HTTPS ports inbound to the VM.</span></span> |
| [<span data-ttu-id="1a8b9-131">Ange AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1a8b9-131">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| <span data-ttu-id="1a8b9-132">Associerar NSG: er och vägtabeller till undernät.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-132">Associates the NSGs and route tables to subnets.</span></span> |
| [<span data-ttu-id="1a8b9-133">Ny AzureRmRouteTable</span><span class="sxs-lookup"><span data-stu-id="1a8b9-133">New-AzureRmRouteTable</span></span>](/powershell/module/azurerm.network/new-azurermroutetable)| <span data-ttu-id="1a8b9-134">Skapar en routningstabell för alla flöden.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-134">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="1a8b9-135">Ny AzureRMRouteConfig</span><span class="sxs-lookup"><span data-stu-id="1a8b9-135">New-AzureRMRouteConfig</span></span>](/powershell/module/azurerm.network/new-azurermrouteconfig)| <span data-ttu-id="1a8b9-136">Skapar vägar för att vidarebefordra trafik mellan undernät och Internet via den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-136">Creates routes to route traffic between subnets and the Internet through the VM.</span></span> |
| [<span data-ttu-id="1a8b9-137">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="1a8b9-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="1a8b9-138">Skapar en virtuell dator och bifogar nätverkskortet till den.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-138">Creates a virtual machine and attaches the NIC to it.</span></span> <span data-ttu-id="1a8b9-139">Det här kommandot anger också avbildning av virtuell dator ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-139">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="1a8b9-140">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a8b9-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | <span data-ttu-id="1a8b9-141">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="1a8b9-141">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1a8b9-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a8b9-142">Next steps</span></span>

<span data-ttu-id="1a8b9-143">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1a8b9-143">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="1a8b9-144">Ytterligare nätverk PowerShell-skript-exempel finns i den [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1a8b9-144">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>