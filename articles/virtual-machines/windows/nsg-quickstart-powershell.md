---
title: "Öppna portar till en virtuell dator med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig hur du öppnar en port / skapa en slutpunkt på din Windows-dator med hjälp av Azure resource manager distribution läge och Azure PowerShell"
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
ms.openlocfilehash: e818e3b3c707e1471d6f580f8379a277d3575b89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a><span data-ttu-id="35584-103">Hur du öppnar portar och slutpunkter till en virtuell dator i Azure med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="35584-103">How to open ports and endpoints to a VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="35584-104">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="35584-104">Quick commands</span></span>
<span data-ttu-id="35584-105">Att skapa en säkerhetsgrupp för nätverk och ACL-regler måste [den senaste versionen av Azure PowerShell installerat](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="35584-105">To create a Network Security Group and ACL rules you need [the latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="35584-106">Du kan också [utför dessa steg med hjälp av Azure portal](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="35584-106">You can also [perform these steps using the Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="35584-107">Logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="35584-107">Log in to your Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="35584-108">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="35584-108">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="35584-109">Exempel parameternamn ingår *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="35584-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="35584-110">Skapa en regel med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="35584-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="35584-111">I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* så att *tcp* trafik på port *80*:</span><span class="sxs-lookup"><span data-stu-id="35584-111">The following example creates a rule named *myNetworkSecurityGroupRule* to allow *tcp* traffic on port *80*:</span></span>

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

<span data-ttu-id="35584-112">Skapa sedan ditt nätverk säkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) och tilldela HTTP-regel som du just har skapat på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="35584-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign the HTTP rule you just created as follows.</span></span> <span data-ttu-id="35584-113">I följande exempel skapas en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="35584-113">The following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="35584-114">Nu ska vi tilldela säkerhetsgrupp för nätverk till ett undernät.</span><span class="sxs-lookup"><span data-stu-id="35584-114">Now let's assign your Network Security Group to a subnet.</span></span> <span data-ttu-id="35584-115">I följande exempel tilldelas ett befintligt virtuellt nätverk med namnet *myVnet* till variabeln *$vnet* med [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="35584-115">The following example assigns an existing virtual network named *myVnet* to the variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="35584-116">Associera en säkerhetsgrupp för nätverk med ditt undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="35584-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="35584-117">I följande exempel associerar undernätet med namnet *mySubnet* med säkerhetsgrupp för nätverk:</span><span class="sxs-lookup"><span data-stu-id="35584-117">The following example associates the subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="35584-118">Slutligen uppdaterar ditt virtuella nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) för att ändringarna ska börja gälla:</span><span class="sxs-lookup"><span data-stu-id="35584-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes to take effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="35584-119">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="35584-119">More information on Network Security Groups</span></span>
<span data-ttu-id="35584-120">Snabb kommandon här kan du komma igång med trafik som flödar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="35584-120">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="35584-121">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för att styra åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="35584-121">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="35584-122">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="35584-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="35584-123">Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="35584-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="35584-124">Belastningsutjämnaren distribuerar trafik till virtuella datorer med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="35584-124">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="35584-125">Mer information finns i [hur du läser in balansera Linux-datorer i Azure för att skapa ett program med hög tillgänglighet](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="35584-125">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="35584-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35584-126">Next steps</span></span>
<span data-ttu-id="35584-127">I det här exemplet skapas en enkel regel för att tillåta HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="35584-127">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="35584-128">Du kan hitta information om hur du skapar mer detaljerad miljöer i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="35584-128">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="35584-129">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="35584-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="35584-130">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="35584-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="35584-131">Översikt av Azure Resource Manager för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35584-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

