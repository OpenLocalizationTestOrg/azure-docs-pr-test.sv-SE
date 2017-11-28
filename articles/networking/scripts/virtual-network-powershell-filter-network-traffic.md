---
title: "Skriptexempel Azure PowerShell - Filter VM-nätverkstrafik | Microsoft Docs"
description: "Azure PowerShell-skript exempel – filtrera inkommande och utgående VM-nätverkstrafik."
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
ms.openlocfilehash: e871ba2f370157936c2aaabc804dc9f5aea6d7ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="27033-103">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="27033-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="27033-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="27033-105">Inkommande nätverkstrafik till undernätet frontend är begränsad till HTTP och HTTPS, medan utgående trafik till Internet från backend-undernät inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="27033-105">Inbound network traffic to the front-end subnet is limited to HTTP, and HTTPS, while outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> <span data-ttu-id="27033-106">När du har kört skriptet har du en virtuell dator med två nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="27033-106">After running the script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="27033-107">Varje nätverkskort är anslutet till ett annat undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-107">Each NIC is connected to a different subnet.</span></span>

<span data-ttu-id="27033-108">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="27033-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="27033-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="27033-109">Sample script</span></span>


<span data-ttu-id="27033-110">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM-nätverkstrafik")]</span><span class="sxs-lookup"><span data-stu-id="27033-110">[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="27033-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="27033-111">Clean up deployment</span></span> 

<span data-ttu-id="27033-112">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="27033-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="27033-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="27033-113">Script explanation</span></span>

<span data-ttu-id="27033-114">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="27033-114">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="27033-115">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="27033-115">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="27033-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="27033-116">Command</span></span> | <span data-ttu-id="27033-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="27033-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="27033-118">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="27033-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="27033-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="27033-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="27033-120">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="27033-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="27033-121">Skapar ett konfigurationsobjekt för undernätet</span><span class="sxs-lookup"><span data-stu-id="27033-121">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="27033-122">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="27033-122">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="27033-123">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-123">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="27033-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="27033-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="27033-125">Skapar säkerhetsregler kan tilldelas till en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="27033-125">Creates security rules to be assigned to a network security group.</span></span> |
| [<span data-ttu-id="27033-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="27033-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="27033-127">Skapar NSG-regler som tillåter eller blockerar specifika portar till specifika undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-127">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="27033-128">Ange AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="27033-128">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="27033-129">Associerar NSG: er till undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-129">Associates NSGs to subnets.</span></span> |
| [<span data-ttu-id="27033-130">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="27033-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="27033-131">Skapar en offentlig IP-adress för att komma åt den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="27033-131">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="27033-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="27033-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="27033-133">Skapar virtuella nätverksgränssnitt och kopplar dem till det virtuella nätverket frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="27033-133">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="27033-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="27033-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="27033-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="27033-135">Creates a VM configuration.</span></span> <span data-ttu-id="27033-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="27033-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="27033-137">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="27033-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="27033-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="27033-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="27033-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="27033-139">Create a virtual machine.</span></span> |
|[<span data-ttu-id="27033-140">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="27033-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="27033-141">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="27033-141">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="27033-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27033-142">Next steps</span></span>

<span data-ttu-id="27033-143">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="27033-143">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="27033-144">Ytterligare nätverk PowerShell-skript-exempel finns i den [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27033-144">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>