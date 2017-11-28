---
title: "aaaAzure PowerShell skriptexempel - skapa ett nätverk för flera nivåer program | Microsoft Docs"
description: "Azure PowerShell-skript exempel – skapa ett virtuellt nätverk för program på flera nivåer."
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
ms.openlocfilehash: 46d6d16dc5dbc8be467359f31346f017727b1abe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="24891-103">Skapa ett nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="24891-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="24891-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="24891-105">Trafik toohello frontend undernät är begränsad tooHTTP och SSH, medan trafik toohello backend-undernät är begränsad tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="24891-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="24891-106">Efter körs hello skript har två virtuella datorer i varje undernät som du kan distribuera webbservern och MySQL programvaran till.</span><span class="sxs-lookup"><span data-stu-id="24891-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

<span data-ttu-id="24891-107">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="24891-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="24891-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="24891-108">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="24891-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="24891-109">Clean up deployment</span></span> 

<span data-ttu-id="24891-110">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="24891-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="24891-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="24891-111">Script explanation</span></span>

<span data-ttu-id="24891-112">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="24891-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="24891-113">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="24891-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="24891-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="24891-114">Command</span></span> | <span data-ttu-id="24891-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="24891-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="24891-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="24891-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="24891-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="24891-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="24891-118">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="24891-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="24891-119">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="24891-120">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="24891-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="24891-121">Skapar en backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="24891-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="24891-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="24891-123">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="24891-123">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="24891-124">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="24891-124">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="24891-125">Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-125">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="24891-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="24891-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="24891-127">Skapar nätverkssäkerhetsgrupper (NSG) som är associerade toohello frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-127">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="24891-128">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="24891-128">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |<span data-ttu-id="24891-129">Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät.</span><span class="sxs-lookup"><span data-stu-id="24891-129">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="24891-130">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="24891-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="24891-131">Skapar virtuella datorer och bifogar ett NIC-tooeach VM.</span><span class="sxs-lookup"><span data-stu-id="24891-131">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="24891-132">Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="24891-132">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="24891-133">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="24891-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="24891-134">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="24891-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="24891-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24891-135">Next steps</span></span>

<span data-ttu-id="24891-136">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="24891-136">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="24891-137">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="24891-137">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
