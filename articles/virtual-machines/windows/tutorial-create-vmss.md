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
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Skapa en Virtual Machine Scale Set och distribuera en app som har hög tillgänglighet i Windows
En skaluppsättning för virtuell dator kan du toodeploy och hantera en uppsättning identiska, automatisk skalning virtuella datorer. Du kan skala hello antal virtuella datorer i hello skaluppsättning manuellt eller definiera regler tooautoscale baserat på CPU-användning, minne begäran eller nätverkstrafik. I kursen får distribuera du en virtuell dator skala i Azure. Lär dig att:

> [!div class="checklist"]
> * Använd hello tillägget för anpassat skript toodefine tooscale en IIS-webbplats
> * Skapa en belastningsutjämnare för din skaluppsättning
> * Skapa en skaluppsättning för virtuell dator
> * Öka eller minska hello antalet instanser i en skaluppsättning
> * Skapa automatiska regler

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Skala Set-översikt
Skalningsuppsättningar använda liknande koncept som du lärt dig i hello tidigare självstudierna för[skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md). Virtuella datorer i en skaluppsättning är fördelade på fel och uppdatera domäner precis som virtuella datorer i en tillgänglighetsuppsättning.

Virtuella datorer skapas efter behov i en skaluppsättning. Du kan definiera automatiska regler toocontrol hur och när virtuella datorer läggs till eller tas bort från hello skaluppsättning. De här reglerna kan utlösa baserat på mått som CPU-belastning, minnesanvändning eller nätverkstrafik.

Skala anger stöd upp too1 000 virtuella datorer när du använder en avbildning i Azure-plattformen. För arbetsbelastningar med betydande installation eller VM anpassning krav gärna för[skapa en anpassad VM-avbildning](tutorial-custom-images.md). Du kan skapa upp too100 virtuella datorer i en skala som anges när du använder en anpassad avbildning.


## <a name="create-an-app-tooscale"></a>Skapa en app tooscale
Innan du kan skapa en skalningsuppsättning, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *EastUS* plats:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

I en tidigare kursen får du lärt dig hur för[automatisera VM-konfiguration](tutorial-automate-vm-deployment.md) med hello tillägget för anpassat skript. Skapa en scale set-konfiguration och sedan tillämpa en tooinstall för tillägget för anpassat skript och konfigurera IIS:

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

## <a name="create-scale-load-balancer"></a>Skapa skala belastningsutjämnare
En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer. En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM. Mer information finns i nästa kurs i hello på [hur tooload balansera virtuella Windows-datorer](tutorial-load-balancer.md).

Skapa en belastningsutjämnare som har en offentlig IP-adress och distribuerar Internet-trafik på port 80:

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

## <a name="create-a-scale-set"></a>Skapa en skaluppsättning
Nu skapa en virtuell dator-skala med [ny AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). hello följande exempel skapas en uppsättning med namnet skala *myScaleSet*:

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

Det tar några minuter toocreate och konfigurera alla hello skala uppsättning resurser och virtuella datorer.


## <a name="test-your-app"></a>Testa din app
toosee din IIS-webbplats i åtgärden, hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello följande exempel hämtar hello IP-adress för *myPublicIP* skapas som en del av skaluppsättning för hello:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Ange hello offentliga IP-adressen i tooa webbläsare. hello app visas, inklusive hello värdnamnet för hello VM som hello läsa in belastningsutjämning distribuerade trafik till:

![Kör IIS-webbplats](./media/tutorial-create-vmss/running-iis-site.png)

toosee hello skaluppsättningen i praktiken, du kan kraft uppdatera din webbplats webbläsare toosee hello belastningen belastningsutjämnare distribuerar trafik över alla hello virtuella datorer som kör din app.


## <a name="management-tasks"></a>Administrativa uppgifter
Du kan behöva toorun under hello livscykel för skaluppsättning för hello, en eller flera hanteringsuppgifter. Dessutom kan du toocreate skript som automatiserar olika livscykel-uppgifter. Azure PowerShell innehåller ett snabbt sätt toodo dessa uppgifter. Här följer några vanliga uppgifter.

### <a name="view-vms-in-a-scale-set"></a>Visa virtuella datorer i en skaluppsättning
tooview en lista över virtuella datorer som körs i din skala anges använder [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) på följande sätt:

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


### <a name="increase-or-decrease-vm-instances"></a>Öka eller minska VM-instanser
toosee hello antal förekomster av du för närvarande i en skala har använder [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) och fråga på *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Du kan sedan manuellt öka eller minska hello antalet virtuella datorer i hello skala med [uppdatering AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). hello följande exempel anger hello antal virtuella datorer i din skaluppsättningen för*5*:

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

Om tar några minuter tooupdate hello anges antalet instanser i en skaluppsättning.


### <a name="configure-autoscale-rules"></a>Konfigurera automatiska regler
I stället för att manuellt skalning hello antal instanser för i din skala har angetts kan du definiera automatiska regler. De här reglerna övervaka hello instanser i din skala ange och svarar därefter baserat på mått och tröskelvärden som du definierar. följande exempel hello skalas hello antal instanser av en när hello Genomsnittlig CPU-belastning är större än 60% under en period på 5 minuter. Om hello Genomsnittlig CPU-belastningen sedan sjunker under 30% under en period på 5 minuter, skalas hello instanser i en instans:

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


## <a name="next-steps"></a>Nästa steg
Du har skapat en skaluppsättning för virtuell dator i den här självstudiekursen. Du har lärt dig att:

> [!div class="checklist"]
> * Använd hello tillägget för anpassat skript toodefine tooscale en IIS-webbplats
> * Skapa en belastningsutjämnare för din skaluppsättning
> * Skapa en skaluppsättning för virtuell dator
> * Öka eller minska hello antalet instanser i en skaluppsättning
> * Skapa automatiska regler

Avancera toohello nästa självstudiekurs toolearn mer om koncept för virtuella datorer för belastningsutjämning.

> [!div class="nextstepaction"]
> [Belastningsutjämna virtuella datorer](tutorial-load-balancer.md)
