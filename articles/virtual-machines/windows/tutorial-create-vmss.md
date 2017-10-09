---
title: "aaaCreate en virtuell dator skala uppsättningar för Windows i Azure | Microsoft Docs"
description: "Skapa och distribuera ett program som har hög tillgänglighet på virtuella Windows-datorer med hjälp av en skaluppsättning för virtuell dator"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a3914571005c28a492c069d880992630851afae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a><span data-ttu-id="1e925-103">Skapa en Virtual Machine Scale Set och distribuera en app som har hög tillgänglighet i Windows</span><span class="sxs-lookup"><span data-stu-id="1e925-103">Create a Virtual Machine Scale Set and deploy a highly available app on Windows</span></span>
<span data-ttu-id="1e925-104">En skaluppsättning för virtuell dator kan du toodeploy och hantera en uppsättning identiska, automatisk skalning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1e925-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="1e925-105">Du kan skala hello antal virtuella datorer i hello skaluppsättning manuellt eller definiera regler tooautoscale baserat på CPU-användning, minne begäran eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="1e925-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="1e925-106">I kursen får distribuera du en virtuell dator skala i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e925-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="1e925-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="1e925-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e925-108">Använd hello tillägget för anpassat skript toodefine tooscale en IIS-webbplats</span><span class="sxs-lookup"><span data-stu-id="1e925-108">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="1e925-109">Skapa en belastningsutjämnare för din skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-109">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="1e925-110">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1e925-110">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="1e925-111">Öka eller minska hello antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-111">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="1e925-112">Skapa automatiska regler</span><span class="sxs-lookup"><span data-stu-id="1e925-112">Create autoscale rules</span></span>

<span data-ttu-id="1e925-113">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1e925-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="1e925-114">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="1e925-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="1e925-115">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="1e925-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="scale-set-overview"></a><span data-ttu-id="1e925-116">Skala Set-översikt</span><span class="sxs-lookup"><span data-stu-id="1e925-116">Scale Set overview</span></span>
<span data-ttu-id="1e925-117">Skalningsuppsättningar använda liknande koncept som du lärt dig i hello tidigare självstudierna för[skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="1e925-117">Scale sets use similar concepts as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="1e925-118">Virtuella datorer i en skaluppsättning är fördelade på fel och uppdatera domäner precis som virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1e925-118">VMs in a scale set are distributed across fault and update domains just like VMs in an availability set.</span></span>

<span data-ttu-id="1e925-119">Virtuella datorer skapas efter behov i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="1e925-119">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="1e925-120">Du kan definiera automatiska regler toocontrol hur och när virtuella datorer läggs till eller tas bort från hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="1e925-120">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="1e925-121">De här reglerna kan utlösa baserat på mått som CPU-belastning, minnesanvändning eller nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="1e925-121">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="1e925-122">Skala anger stöd upp too1 000 virtuella datorer när du använder en avbildning i Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1e925-122">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="1e925-123">För arbetsbelastningar med betydande installation eller VM anpassning krav gärna för[skapa en anpassad VM-avbildning](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="1e925-123">For workloads with significant installation or VM customization requirements, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="1e925-124">Du kan skapa upp too100 virtuella datorer i en skala som anges när du använder en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="1e925-124">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="1e925-125">Skapa en app tooscale</span><span class="sxs-lookup"><span data-stu-id="1e925-125">Create an app tooscale</span></span>
<span data-ttu-id="1e925-126">Innan du kan skapa en skalningsuppsättning, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="1e925-126">Before you can create a scale set, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="1e925-127">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="1e925-127">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

<span data-ttu-id="1e925-128">I en tidigare kursen får du lärt dig hur för[automatisera VM-konfiguration](tutorial-automate-vm-deployment.md) med hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="1e925-128">In an earlier tutorial, you learned how too[Automate VM configuration](tutorial-automate-vm-deployment.md) using hello Custom Script Extension.</span></span> <span data-ttu-id="1e925-129">Skapa en scale set-konfiguration och sedan tillämpa en tooinstall för tillägget för anpassat skript och konfigurera IIS:</span><span class="sxs-lookup"><span data-stu-id="1e925-129">Create a scale set configuration then apply a Custom Script Extension tooinstall and configure IIS:</span></span>

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define hello script for your Custom Script Extension toorun
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension tooinstall IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a><span data-ttu-id="1e925-130">Skapa skala belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="1e925-130">Create scale load balancer</span></span>
<span data-ttu-id="1e925-131">En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1e925-131">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="1e925-132">En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.</span><span class="sxs-lookup"><span data-stu-id="1e925-132">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span> <span data-ttu-id="1e925-133">Mer information finns i nästa kurs i hello på [hur tooload balansera virtuella Windows-datorer](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="1e925-133">For more information, see hello next tutorial on [How tooload balance Windows virtual machines](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="1e925-134">Skapa en belastningsutjämnare som har en offentlig IP-adress och distribuerar Internet-trafik på port 80:</span><span class="sxs-lookup"><span data-stu-id="1e925-134">Create a load balancer that has a public IP address and distributes web traffic on port 80:</span></span>

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create hello load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule toodistribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update hello load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a><span data-ttu-id="1e925-135">Skapa en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-135">Create a scale set</span></span>
<span data-ttu-id="1e925-136">Nu skapa en virtuell dator-skala med [ny AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="1e925-136">Now create a virtual machine scale set with [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="1e925-137">hello följande exempel skapas en uppsättning med namnet skala *myScaleSet*:</span><span class="sxs-lookup"><span data-stu-id="1e925-137">hello following example creates a scale set named *myScaleSet*:</span></span>

```powershell
# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create hello virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach hello virtual network toohello config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

<span data-ttu-id="1e925-138">Det tar några minuter toocreate och konfigurera alla hello skala uppsättning resurser och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1e925-138">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span>


## <a name="test-your-app"></a><span data-ttu-id="1e925-139">Testa din app</span><span class="sxs-lookup"><span data-stu-id="1e925-139">Test your app</span></span>
<span data-ttu-id="1e925-140">toosee din IIS-webbplats i åtgärden, hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="1e925-140">toosee your IIS website in action, obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="1e925-141">hello följande exempel hämtar hello IP-adress för *myPublicIP* skapas som en del av skaluppsättning för hello:</span><span class="sxs-lookup"><span data-stu-id="1e925-141">hello following example obtains hello IP address for *myPublicIP* created as part of hello scale set:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

<span data-ttu-id="1e925-142">Ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1e925-142">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="1e925-143">hello app visas, inklusive hello värdnamnet för hello VM som hello läsa in belastningsutjämning distribuerade trafik till:</span><span class="sxs-lookup"><span data-stu-id="1e925-143">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Kör IIS-webbplats](./media/tutorial-create-vmss/running-iis-site.png)

<span data-ttu-id="1e925-145">toosee hello skaluppsättningen i praktiken, du kan kraft uppdatera din webbplats webbläsare toosee hello belastningen belastningsutjämnare distribuerar trafik över alla hello virtuella datorer som kör din app.</span><span class="sxs-lookup"><span data-stu-id="1e925-145">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="1e925-146">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="1e925-146">Management tasks</span></span>
<span data-ttu-id="1e925-147">Du kan behöva toorun under hello livscykel för skaluppsättning för hello, en eller flera hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1e925-147">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="1e925-148">Dessutom kan du toocreate skript som automatiserar olika livscykel-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="1e925-148">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="1e925-149">Azure PowerShell innehåller ett snabbt sätt toodo dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="1e925-149">Azure PowerShell provides a quick way toodo those tasks.</span></span> <span data-ttu-id="1e925-150">Här följer några vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="1e925-150">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="1e925-151">Visa virtuella datorer i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-151">View VMs in a scale set</span></span>
<span data-ttu-id="1e925-152">tooview en lista över virtuella datorer som körs i din skala anges använder [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1e925-152">tooview a list of VMs running in your scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) as follows:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through hello instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="1e925-153">Öka eller minska VM-instanser</span><span class="sxs-lookup"><span data-stu-id="1e925-153">Increase or decrease VM instances</span></span>
<span data-ttu-id="1e925-154">toosee hello antal förekomster av du för närvarande i en skala har använder [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) och fråga på *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="1e925-154">toosee hello number of instances you currently have in a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) and query on *sku.capacity*:</span></span>

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

<span data-ttu-id="1e925-155">Du kan sedan manuellt öka eller minska hello antalet virtuella datorer i hello skala med [uppdatering AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span><span class="sxs-lookup"><span data-stu-id="1e925-155">You can then manually increase or decrease hello number of virtual machines in hello scale set with [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span></span> <span data-ttu-id="1e925-156">hello följande exempel anger hello antal virtuella datorer i din skaluppsättningen för*5*:</span><span class="sxs-lookup"><span data-stu-id="1e925-156">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update hello capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

<span data-ttu-id="1e925-157">Om tar några minuter tooupdate hello anges antalet instanser i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="1e925-157">If takes a few minutes tooupdate hello specified number of instances in your scale set.</span></span>


### <a name="configure-autoscale-rules"></a><span data-ttu-id="1e925-158">Konfigurera automatiska regler</span><span class="sxs-lookup"><span data-stu-id="1e925-158">Configure autoscale rules</span></span>
<span data-ttu-id="1e925-159">I stället för att manuellt skalning hello antal instanser för i din skala har angetts kan du definiera automatiska regler.</span><span class="sxs-lookup"><span data-stu-id="1e925-159">Rather than manually scaling hello number of instances in your scale set, you define autoscale rules.</span></span> <span data-ttu-id="1e925-160">De här reglerna övervaka hello instanser i din skala ange och svarar därefter baserat på mått och tröskelvärden som du definierar.</span><span class="sxs-lookup"><span data-stu-id="1e925-160">These rules monitor hello instances in your scale set and respond accordingly based on metrics and thresholds you define.</span></span> <span data-ttu-id="1e925-161">följande exempel hello skalas hello antal instanser av en när hello Genomsnittlig CPU-belastning är större än 60% under en period på 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="1e925-161">hello following example scales out hello number of instances by one when hello average CPU load is greater than 60% over a 5 minute period.</span></span> <span data-ttu-id="1e925-162">Om hello Genomsnittlig CPU-belastningen sedan sjunker under 30% under en period på 5 minuter, skalas hello instanser i en instans:</span><span class="sxs-lookup"><span data-stu-id="1e925-162">If hello average CPU load then drops below 30% over a 5 minute period, hello instances are scaled in by one instance:</span></span>

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule tooincrease hello number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule toodecrease hello number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply hello autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a><span data-ttu-id="1e925-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e925-163">Next steps</span></span>
<span data-ttu-id="1e925-164">Du har skapat en skaluppsättning för virtuell dator i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="1e925-164">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="1e925-165">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="1e925-165">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e925-166">Använd hello tillägget för anpassat skript toodefine tooscale en IIS-webbplats</span><span class="sxs-lookup"><span data-stu-id="1e925-166">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="1e925-167">Skapa en belastningsutjämnare för din skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-167">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="1e925-168">Skapa en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1e925-168">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="1e925-169">Öka eller minska hello antalet instanser i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1e925-169">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="1e925-170">Skapa automatiska regler</span><span class="sxs-lookup"><span data-stu-id="1e925-170">Create autoscale rules</span></span>

<span data-ttu-id="1e925-171">Avancera toohello nästa självstudiekurs toolearn mer om koncept för virtuella datorer för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="1e925-171">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1e925-172">Belastningsutjämna virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1e925-172">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
