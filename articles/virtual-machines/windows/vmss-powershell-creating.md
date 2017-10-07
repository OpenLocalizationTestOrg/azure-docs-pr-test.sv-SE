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
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Skapa Skalningsuppsättningar i virtuella datorer med hjälp av PowerShell-cmdlets
Den här artikeln beskriver hur ett exempel på hur toocreate en virtuell dator skaluppsättning (VMSS). Den skapar en skaluppsättning för tre noder, med tillhörande nätverk och lagring.

## <a name="first-steps"></a>Första stegen
Kontrollera du har hello senaste Azure PowerShell-modulen installerad och toomake att du har hello PowerShell-kommandon krävs toomaintain och skapa skaluppsättningar.
Gå toohello kommandoradsverktyg [här](http://aka.ms/webpi-azps) för hello senaste tillgängliga Azure moduler.

toofind VMSS relaterade kommandon använder hello söksträngen \*VMSS\*. Till exempel _gcm *vmss*_

## <a name="creating-a-vmss"></a>Skapa en VMSS
#### <a name="create-resource-group"></a>Skapa resursgrupp
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a>Skapa nätverk (VNET / undernät)
#### <a name="subnet-specification"></a>Undernätspecifikation
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a>VNET-specifikationen
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume hello new subnet is hello only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-tooallow-external-access"></a>Skapa offentlig IP-resurs tooAllow extern åtkomst
Detta kan vara bundna toohello belastningsutjämnaren.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a>Skapa belastningsutjämnare
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

#### <a name="configure-load-balancer"></a>Konfigurera belastningsutjämnare
Skapa Backend adresspoolen Config, detta ska delas av hello nätverkskort hello virtuella datorer i hello skaluppsättning.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Ange belastningen belastningsutjämnade Avsökningsport, ändra hello inställningarna för ditt program.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Skapa en inkommande NAT-pool för routade direktanslutning (inte Utjämning av nätverksbelastning) toohello virtuella datorer i hello skala in via hello belastningsutjämnare. Detta är toodemonstrate med RDP och kan inte behövs i programmet.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Skapa hello ladda belastningsutjämnade regeln det här exemplet visar att läsa in belastningsutjämning port 80 begäranden med hello inställningarna från föregående steg.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Skapa belastningsutjämnaren med konfigurationen.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Kontrollera inställningarna för LB, kontrollera belastningen konfigurationerna för belastningsutjämnade port, Lägg märke till, inte inkommande NAT-regler tills hello virtuella datorer i hello skaluppsättning skapas.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-hello-scale-set"></a>Konfigurera och skapa hello skalan
Observera att den här infrastrukturen exemplet visar hur tooset dig distribuera och skala webbtrafik över hello skala anges, men hello VM-avbildningar som anges här inte har några installerade webbtjänsterna.

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

Binda NIC tooLoad belastningsutjämnare och undernät

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Skapa scale set Config

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

Skapa skala ange konfiguration

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Du har nu skapat hello skaluppsättning. Du kan testa anslutning toohello enskilda VM som använder RDP i det här exemplet:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
