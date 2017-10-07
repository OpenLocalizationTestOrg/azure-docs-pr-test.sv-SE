---
title: aaaHow tooload balansera Windows virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toouse hello Azure läsa in belastningsutjämning toocreate ett program med hög tillgänglighet och säkert över tre virtuella Windows-datorer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Hur balansera tooload Windows-datorer i Azure toocreate ett program med hög tillgänglighet
Belastningsutjämning ger högre tillgänglighet genom att sprida inkommande begäranden över flera virtuella datorer. I kursen får information du om hello olika komponenterna i hello Azure belastningsutjämnare som distribuerar trafik och ger hög tillgänglighet. Lär dig att:

> [!div class="checklist"]
> * Skapa en Azure belastningsutjämnare
> * Skapa en belastningsutjämnaren, hälsoavsökningen
> * Skapa regler för nätverkstrafik för belastningsutjämnare
> * Använd hello tillägget för anpassat skript toocreate en grundläggande IIS-webbplats
> * Skapa virtuella datorer och bifoga tooa belastningsutjämnare
> * Visa en belastningsutjämnare i praktiken
> * Lägga till och ta bort virtuella datorer från en belastningsutjämnare

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Översikt över Azure belastningen belastningsutjämnare
En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer. En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.

Du kan definiera en frontend IP-konfiguration som innehåller en eller flera offentliga IP-adresser. Den här frontend IP-konfiguration kan din load balancer och program toobe tillgänglig via hello Internet. 

Virtuella datorer ansluta tooa belastningsutjämning med hjälp av deras virtuella nätverksgränssnittskortet (NIC). toodistribute trafik toohello virtuella datorer, en backend-adresspool innehåller hello IP-adresser för hello virtuella (NIC) anslutna toohello belastningsutjämnare.

toocontrol hello trafikflödet, definiera regler för inläsning av belastningsutjämning för specifika portar och protokoll som mappar tooyour virtuella datorer.


## <a name="create-azure-load-balancer"></a>Skapa Azure belastningsutjämnare
Det här avsnittet beskrivs hur du kan skapa och konfigurera varje komponent av hello belastningsutjämnare. Innan du kan skapa din belastningsutjämnare, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupLoadBalancer* i hello *EastUS* plats:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress
tooaccess din app på hello Internet, måste en offentlig IP-adress för hello belastningsutjämnaren. Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* i hello *myResourceGroupLoadBalancer* resursgrupp:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Skapa en belastningsutjämnare
Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). hello följande exempel skapar en IP-adress för klientdel som heter *myFrontEndPool*: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). hello följande exempel skapas en backend-adresspool med namnet *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Nu ska du skapa hello belastningsutjämnare med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). hello följande exempel skapas en belastningsutjämnare med namnet *myLoadBalancer* med hello *myPublicIP* adress:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Skapa en hälsoavsökningen
tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen. Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller. En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard. Du skapar en hälsoavsökningen baserat på ett protokoll eller ett visst hälsotillstånd sidan för kontroll för din app. 

hello följande exempel skapar en TCP-avsökning. Du kan också skapa anpassade HTTP-avsökningar mer detaljerade hälsokontroller. När du använder en anpassad HTTP-avsökningen, måste du skapa hello hälsa sidan kontroll som *healthcheck.aspx*. hello avsökning måste returnera ett **HTTP 200 OK** svar för hello belastningen belastningsutjämnaren tookeep hello värd i rotation.

toocreate en TCP-hälsoavsökningen du använder [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). hello följande exempel skapas en hälsoavsökningen med namnet *myHealthProbe* som övervakar varje virtuell dator:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Skapa en regel för belastningsutjämnare
En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer. Du kan definiera hello frontend IP-konfiguration för hello inkommande trafik och hello backend-IP-adresspool tooreceive hello trafik, tillsammans med hello krävs käll- och målport. toomake att endast felfri virtuella datorer ta emot trafik, du också definiera hello hälsa avsökningen toouse.

Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). hello följande exempel skapas en regel för belastningsutjämnare med namnet *myLoadBalancerRule* och balanserar trafik på port *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Konfigurera ett virtuellt nätverk
Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer. Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.

### <a name="create-network-resources"></a>Skapa nätverksresurser
Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Lägg till hello säkerhet grupp toohello undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) och uppdatera sedan hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet *myNetworkSecurityGroup* och omfattar även*mySubnet*:

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuella nätverkskort skapas med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). hello skapas följande exempel tre virtuella nätverkskort. (Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg). Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Skapa virtuella datorer
tooimprove hello hög tillgänglighet för din app, placera dina virtuella datorer i en tillgänglighetsuppsättning.

Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Ange en administratör användarnamn och lösenord för hello virtuella datorer med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nu kan du skapa hello virtuella datorer med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). hello följande exempel skapas tre virtuella datorer:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Det tar några minuter toocreate och konfigurera alla tre virtuella datorer.

### <a name="install-iis-with-custom-script-extension"></a>Installera IIS med tillägget för anpassat skript
I en tidigare självstudiekurs om [hur toocustomize Windows-dator](tutorial-automate-vm-deployment.md), du har lärt dig hur tooautomate VM anpassning med hello anpassade skript tillägget för Windows. Du kan använda hello samma hanterar tooinstall och konfigurera IIS på dina virtuella datorer.

Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript. Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver och uppdateringar hello *Default.htm* sidan tooshow hello värdnamnet för hello VM:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Testa belastningsutjämnare
Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare. hello webbplatsen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:

![Kör IIS-webbplats](./media/tutorial-load-balancer/running-iis-website.png)

toosee hello belastningsutjämnare distribuerar trafik över alla tre virtuella datorer som kör appen, du kan framtvinga uppdatera webbläsaren.


## <a name="add-and-remove-vms"></a>Lägga till och ta bort virtuella datorer
Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS. toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer. Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Ta bort en virtuell dator från hello belastningsutjämnare
Hämta hello nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), sedan ange hello *LoadBalancerBackendAddressPools* -egenskapen för hello virtuella nätverkskortet för*$null*. Slutligen uppdatera hello virtuellt nätverkskort.:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

toosee hello belastningsutjämnare distribuerar trafik över hello återstående två virtuella datorer som kör appen du kan framtvinga uppdatera webbläsaren. Du kan nu utföra underhåll på hello VM, till exempel installera uppdateringar av operativsystem eller utföra en VM-omstart.

### <a name="add-a-vm-toohello-load-balancer"></a>Lägga till en belastningsutjämnare för VM-toohello
Efter utföra VM underhåll eller om du behöver tooexpand kapacitet, ange hello *LoadBalancerBackendAddressPools* -egenskapen för hello virtuella nätverkskort toohello *BackendAddressPool* från [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Hämta hello belastningsutjämnare:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen, skapas en belastningsutjämnare och kopplade tooit för virtuella datorer. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en Azure belastningsutjämnare
> * Skapa en belastningsutjämnaren, hälsoavsökningen
> * Skapa regler för nätverkstrafik för belastningsutjämnare
> * Använd hello tillägget för anpassat skript toocreate en grundläggande IIS-webbplats
> * Skapa virtuella datorer och bifoga tooa belastningsutjämnare
> * Visa en belastningsutjämnare i praktiken
> * Lägga till och ta bort virtuella datorer från en belastningsutjämnare

I förväg toohello nästa självstudiekurs toolearn hur toomanage VM-nätverk.

> [!div class="nextstepaction"]
> [Hantera virtuella datorer och virtuella nätverk](./tutorial-virtual-network.md)
