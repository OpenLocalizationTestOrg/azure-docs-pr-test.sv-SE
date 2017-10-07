---
title: aaaCreating virtuella datorn anger med PowerShell-cmdlets | Microsoft Docs
description: "Börja skapa och hantera din första Azure virtuella Skaluppsättningar med hjälp av Azure PowerShell-cmdlets"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 430d9d64-1f35-48f0-a4fd-9b69910ffa59
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: danielsollondon
ms.openlocfilehash: 7979be367d04c904b60d78849c1b751a52cc8caf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a><span data-ttu-id="da752-103">Skapa Skalningsuppsättningar i virtuella datorer med hjälp av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="da752-103">Creating Virtual Machine Scale Sets using PowerShell cmdlets</span></span>
<span data-ttu-id="da752-104">Den här artikeln beskriver hur ett exempel på hur toocreate en virtuell dator skaluppsättning (VMSS).</span><span class="sxs-lookup"><span data-stu-id="da752-104">This article walks through an example of how toocreate a Virtual Machine scale set (VMSS).</span></span> <span data-ttu-id="da752-105">Den skapar en skaluppsättning för tre noder, med tillhörande nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="da752-105">It creates a scale set of three nodes, with associated Networking and Storage.</span></span>

## <a name="first-steps"></a><span data-ttu-id="da752-106">Första stegen</span><span class="sxs-lookup"><span data-stu-id="da752-106">First Steps</span></span>
<span data-ttu-id="da752-107">Kontrollera du har hello senaste Azure PowerShell-modulen installerad och toomake att du har hello PowerShell-kommandon krävs toomaintain och skapa skaluppsättningar.</span><span class="sxs-lookup"><span data-stu-id="da752-107">Ensure you have hello latest Azure PowerShell module installed, toomake sure you have hello PowerShell commandlets needed toomaintain, and create scale sets.</span></span>
<span data-ttu-id="da752-108">Gå toohello kommandoradsverktyg [här](http://aka.ms/webpi-azps) för hello senaste tillgängliga Azure moduler.</span><span class="sxs-lookup"><span data-stu-id="da752-108">Go toohello command line tools [here](http://aka.ms/webpi-azps) for hello latest available Azure Modules.</span></span>

<span data-ttu-id="da752-109">toofind VMSS relaterade kommandon använder hello söksträngen \*VMSS\*.</span><span class="sxs-lookup"><span data-stu-id="da752-109">toofind VMSS related commandlets, use hello search string \*VMSS\*.</span></span> <span data-ttu-id="da752-110">Till exempel _gcm *vmss*_</span><span class="sxs-lookup"><span data-stu-id="da752-110">For example, _gcm *vmss*_</span></span>

## <a name="creating-a-vmss"></a><span data-ttu-id="da752-111">Skapa en VMSS</span><span class="sxs-lookup"><span data-stu-id="da752-111">Creating a VMSS</span></span>
#### <a name="create-resource-group"></a><span data-ttu-id="da752-112">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="da752-112">Create Resource Group</span></span>
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a><span data-ttu-id="da752-113">Skapa nätverk (VNET / undernät)</span><span class="sxs-lookup"><span data-stu-id="da752-113">Create Networking (VNET / Subnet)</span></span>
#### <a name="subnet-specification"></a><span data-ttu-id="da752-114">Undernätspecifikation</span><span class="sxs-lookup"><span data-stu-id="da752-114">Subnet Specification</span></span>
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a><span data-ttu-id="da752-115">VNET-specifikationen</span><span class="sxs-lookup"><span data-stu-id="da752-115">VNET Specification</span></span>
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume hello new subnet is hello only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-tooallow-external-access"></a><span data-ttu-id="da752-116">Skapa offentlig IP-resurs tooAllow extern åtkomst</span><span class="sxs-lookup"><span data-stu-id="da752-116">Create Public IP Resource tooAllow External Access</span></span>
<span data-ttu-id="da752-117">Detta kan vara bundna toohello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="da752-117">This will be bound toohello Load Balancer.</span></span>

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a><span data-ttu-id="da752-118">Skapa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="da752-118">Create Load Balancer</span></span>
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

# Bind Public IP tooLoad Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

#### <a name="configure-load-balancer"></a><span data-ttu-id="da752-119">Konfigurera belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="da752-119">Configure Load Balancer</span></span>
<span data-ttu-id="da752-120">Skapa Backend adresspoolen Config, detta ska delas av hello nätverkskort hello virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="da752-120">Create Backend Address Pool Config, this will be shared by hello NICs of hello VMs in hello scale set.</span></span>

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

<span data-ttu-id="da752-121">Ange belastningen belastningsutjämnade Avsökningsport, ändra hello inställningarna för ditt program.</span><span class="sxs-lookup"><span data-stu-id="da752-121">Set Load Balanced Probe Port, change hello settings as appropriate for your application.</span></span>

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

<span data-ttu-id="da752-122">Skapa en inkommande NAT-pool för routade direktanslutning (inte Utjämning av nätverksbelastning) toohello virtuella datorer i hello skala in via hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="da752-122">Create an inbound NAT pool for direct routed connectivity (not load balanced) toohello VMs in hello scale set via hello Load Balancer.</span></span> <span data-ttu-id="da752-123">Detta är toodemonstrate med RDP och kan inte behövs i programmet.</span><span class="sxs-lookup"><span data-stu-id="da752-123">This is toodemonstrate using RDP and may not be required in your application.</span></span>

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

<span data-ttu-id="da752-124">Skapa hello ladda belastningsutjämnade regeln det här exemplet visar att läsa in belastningsutjämning port 80 begäranden med hello inställningarna från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="da752-124">Create hello Load Balanced Rule, this example shows load balancing port 80 requests, using hello settings from previous steps.</span></span>

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

<span data-ttu-id="da752-125">Skapa belastningsutjämnaren med konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="da752-125">Create Load Balancer with configuration.</span></span>

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

<span data-ttu-id="da752-126">Kontrollera inställningarna för LB, kontrollera belastningen konfigurationerna för belastningsutjämnade port, Lägg märke till, inte inkommande NAT-regler tills hello virtuella datorer i hello skaluppsättning skapas.</span><span class="sxs-lookup"><span data-stu-id="da752-126">Check  LB settings, check load balanced port configs, note, you will not see Inbound NAT rules until hello VMs in hello scale set are created.</span></span>

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-hello-scale-set"></a><span data-ttu-id="da752-127">Konfigurera och skapa hello skalan</span><span class="sxs-lookup"><span data-stu-id="da752-127">Configure and Create hello scale set</span></span>
<span data-ttu-id="da752-128">Observera att den här infrastrukturen exemplet visar hur tooset dig distribuera och skala webbtrafik över hello skala anges, men hello VM-avbildningar som anges här inte har några installerade webbtjänsterna.</span><span class="sxs-lookup"><span data-stu-id="da752-128">Note, this infrastructure example shows how tooset up distribute and scale web traffic across hello scale set, but hello VMs Images specified here do not have any web services installed.</span></span>

```
# specify scale set Name
$vmssName = 'vmss' + $rgname;

## specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

<span data-ttu-id="da752-129">Binda NIC tooLoad belastningsutjämnare och undernät</span><span class="sxs-lookup"><span data-stu-id="da752-129">Bind NIC tooLoad Balancer and Subnet</span></span>

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

<span data-ttu-id="da752-130">Skapa scale set Config</span><span class="sxs-lookup"><span data-stu-id="da752-130">Create scale set Config</span></span>

```
# Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

<span data-ttu-id="da752-131">Skapa skala ange konfiguration</span><span class="sxs-lookup"><span data-stu-id="da752-131">Build scale set configuration</span></span>

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

<span data-ttu-id="da752-132">Du har nu skapat hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="da752-132">Now you have created hello scale set.</span></span> <span data-ttu-id="da752-133">Du kan testa anslutning toohello enskilda VM som använder RDP i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="da752-133">You can test connecting toohello individual VM using RDP in this example:</span></span>

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
