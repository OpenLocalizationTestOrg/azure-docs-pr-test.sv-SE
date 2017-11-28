---
title: "aaaOpen portarna tooa VM med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig hur tooopen en port / skapa en slutpunkt tooyour Windows VM använder hello Azure resource manager distribution läge och Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c1817a0c447ae4ce7a1ce2a1fc6927bedf2dacb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-and-endpoints-tooa-vm-in-azure-using-powershell"></a><span data-ttu-id="c8fcb-103">Hur tooopen portar och slutpunkter tooa VM i Azure med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8fcb-103">How tooopen ports and endpoints tooa VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="c8fcb-104">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="c8fcb-104">Quick commands</span></span>
<span data-ttu-id="c8fcb-105">toocreate en nätverkssäkerhet och ACL-regler måste [hello senaste versionen av Azure PowerShell installerat](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-105">toocreate a Network Security Group and ACL rules you need [hello latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="c8fcb-106">Du kan också [utföra dessa steg med hello Azure-portalen](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-106">You can also [perform these steps using hello Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="c8fcb-107">Logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-107">Log in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="c8fcb-108">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-108">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="c8fcb-109">Exempel parameternamn ingår *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="c8fcb-110">Skapa en regel med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="c8fcb-111">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* tooallow *tcp* trafik på port *80*:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-111">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow *tcp* traffic on port *80*:</span></span>

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

<span data-ttu-id="c8fcb-112">Skapa sedan ditt nätverk säkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) och tilldela hello HTTP-regel som du just har skapat på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign hello HTTP rule you just created as follows.</span></span> <span data-ttu-id="c8fcb-113">hello följande exempel skapar en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-113">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="c8fcb-114">Nu ska vi tilldela Nätverkssäkerhetsgruppen tooa undernätet.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-114">Now let's assign your Network Security Group tooa subnet.</span></span> <span data-ttu-id="c8fcb-115">hello följande exempel tilldelas ett befintligt virtuellt nätverk med namnet *myVnet* toohello variabeln *$vnet* med [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="c8fcb-115">hello following example assigns an existing virtual network named *myVnet* toohello variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="c8fcb-116">Associera en säkerhetsgrupp för nätverk med ditt undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="c8fcb-117">hello följande exempel associerar hello undernät med namnet *mySubnet* med säkerhetsgrupp för nätverk:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-117">hello following example associates hello subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="c8fcb-118">Slutligen uppdaterar ditt virtuella nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) för dina ändringar tootake gälla:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes tootake effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="c8fcb-119">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="c8fcb-119">More information on Network Security Groups</span></span>
<span data-ttu-id="c8fcb-120">hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-120">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="c8fcb-121">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-121">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="c8fcb-122">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="c8fcb-123">Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="c8fcb-124">hello belastningsutjämnare distribuerar trafik tooVMs med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-124">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="c8fcb-125">Mer information finns i [hur tooload saldo Linux virtuella datorer i Azure toocreate högtillgänglig programmet](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="c8fcb-125">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8fcb-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8fcb-126">Next steps</span></span>
<span data-ttu-id="c8fcb-127">I det här exemplet skapas en enkel regel tooallow HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="c8fcb-127">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="c8fcb-128">Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="c8fcb-128">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="c8fcb-129">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c8fcb-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="c8fcb-130">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="c8fcb-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="c8fcb-131">Översikt av Azure Resource Manager för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c8fcb-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

