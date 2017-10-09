---
title: "aaaAzure PowerShell skriptexempel - Filter VM-nätverkstrafik | Microsoft Docs"
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
ms.openlocfilehash: 39eae6a43a8dc7f9fc616ef3ec50f95443fd3547
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="25716-103">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="25716-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="25716-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="25716-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="25716-105">Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP och HTTPS, medan utgående trafik toohello Internet från hello backend-undernät inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="25716-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, and HTTPS, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="25716-106">När du har kört hello skript har du en virtuell dator med två nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="25716-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="25716-107">Varje nätverkskort är anslutet tooa olika undernät.</span><span class="sxs-lookup"><span data-stu-id="25716-107">Each NIC is connected tooa different subnet.</span></span>

<span data-ttu-id="25716-108">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="25716-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="25716-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="25716-109">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="25716-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="25716-110">Clean up deployment</span></span> 

<span data-ttu-id="25716-111">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="25716-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="25716-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="25716-112">Script explanation</span></span>

<span data-ttu-id="25716-113">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="25716-113">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="25716-114">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="25716-114">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="25716-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="25716-115">Command</span></span> | <span data-ttu-id="25716-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="25716-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="25716-117">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="25716-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="25716-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="25716-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="25716-119">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="25716-119">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="25716-120">Skapar ett konfigurationsobjekt för undernätet</span><span class="sxs-lookup"><span data-stu-id="25716-120">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="25716-121">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="25716-121">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="25716-122">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="25716-122">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="25716-123">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="25716-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="25716-124">Skapar säkerhet regler toobe tilldelade tooa nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="25716-124">Creates security rules toobe assigned tooa network security group.</span></span> |
| [<span data-ttu-id="25716-125">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="25716-125">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="25716-126">Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät.</span><span class="sxs-lookup"><span data-stu-id="25716-126">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="25716-127">Ange AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="25716-127">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="25716-128">Associerar NSG: er toosubnets.</span><span class="sxs-lookup"><span data-stu-id="25716-128">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="25716-129">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="25716-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="25716-130">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="25716-130">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="25716-131">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="25716-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="25716-132">Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="25716-132">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="25716-133">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="25716-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="25716-134">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="25716-134">Creates a VM configuration.</span></span> <span data-ttu-id="25716-135">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="25716-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="25716-136">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="25716-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="25716-137">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="25716-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="25716-138">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="25716-138">Create a virtual machine.</span></span> |
|[<span data-ttu-id="25716-139">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="25716-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="25716-140">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="25716-140">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="25716-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25716-141">Next steps</span></span>

<span data-ttu-id="25716-142">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="25716-142">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="25716-143">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="25716-143">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
