---
title: "aaaTutorial - skapa ett program som har hög tillgänglighet på Azure Virtual Machines | Microsoft Docs"
description: "Lär dig hur toocreate ett program för hög tillgänglighet och säkert över tre virtuella Windows-datorer med en belastningsutjämnare i Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Skapa en belastningsutjämnad, hög tillgänglighet program på Windows-datorer i Azure

I den här kursen kan du skapa ett program för hög tillgänglighet som är flexibel toomaintenance händelser. hello appen använder en belastningsutjämnare, en tillgänglighetsuppsättning och tre virtuella Windows-datorer (VM). Den här självstudiekursen installerar IIS, även om du kan använda den här självstudiekursen toodeploy med ett annat program framework hello samma komponenter för hög tillgänglighet och riktlinjer. 

## <a name="step-1---azure-prerequisites"></a>Steg 1 – krav för Azure

toocomplete självstudiekursen, se till att du har installerat hello senaste [Azure PowerShell](/powershell/azure/overview) modul.

Först logga in tooyour Azure-prenumeration med hello Login-AzureRmAccount kommandot och följ hello på skärmen riktningar.

```powershell
Login-AzureRmAccount
```

En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. Innan du kan skapa andra Azure-resurser, behöver du toocreate en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` region: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>Steg 2 – Skapa tillgänglighetsuppsättning

Virtuella datorer kan skapa logiska fel och uppdatera domäner. Varje logiska domän representerar en del av maskinvaran i hello underliggande Azure-datacenter. När du skapar två eller flera virtuella datorer distribueras resurserna beräkning och lagring i dessa domäner. Den här distributionen underhåller hello tillgängligheten för din app om en maskinvarukomponent måste underhåll. Tillgänglighetsuppsättningar kan du definiera dessa logiska fel- och update-domäner.

Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a>Steg 3 – skapa belastningen belastningsutjämnare

En Azure belastningsutjämnare distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren. En hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.

### <a name="create-public-ip-address"></a>Skapa offentlig IP-adress

tooaccess din app på hello Internet, tilldela offentliga IP-adress toohello belastningsutjämning. Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Skapa belastningsutjämnare

Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). hello följande exempel skapar en IP-adress för klientdel som heter `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). hello följande exempel skapas en backend-adresspool som heter `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Nu ska du skapa hello belastningsutjämnare med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer` med hello `myPublicIP` adress:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Skapa hälsoavsökningen

tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen. Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller. En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.

Skapa en hälsoavsökningen med [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). hello följande exempel skapas en hälsoavsökningen med namnet `myHealthProbe` som övervakar varje virtuell dator:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Skapa regel för belastningsutjämnare

En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer.

Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). hello följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRule` och balanserar trafik på port `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>Steg 4 – Konfigurera nätverk

Varje virtuell dator har en eller flera virtuella nätverkskort (NIC) som ansluter tooa virtuellt nätverk. Det här virtuella nätverket skyddas toofilter trafik baserat på definierade åtkomstregler.

### <a name="create-virtual-network"></a>Skapa det virtuella nätverket

Konfigurera först ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). hello följande exempel skapas ett undernät med namnet `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

tooprovide network connectivity tooyour virtuella datorer, skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med `mySubnet`:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a>Konfigurera nätverkssäkerhet

En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer. Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall. De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator.

tooallow trafik tooreach din webbapp, skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet `myNetworkSecurityGroupRule`:

```powershell
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
```

Skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). hello följande exempel skapas en NSG som heter `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Lägg till hello säkerhet grupp toohello undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Uppdatera hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Skapa virtuellt nätverkskort

Läsa in belastningsutjämning funktionen med hello virtuell NIC-resurs i stället för hello faktiska VM. hello virtuella nätverkskortet är anslutet toohello belastningsutjämnare och kopplade tooa VM.

Skapa ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). hello skapas följande exempel tre virtuella nätverkskort. (Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg):


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a>Steg 5 – Skapa virtuella datorer

Med alla hello underliggande komponenter på plats, kan du nu skapa högtillgängliga virtuella datorer toorun din app. 

Hämta hello användarnamn och lösenord för administratörskontot för hello på hello virtuell dator med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Skapa hello virtuella datorer med [ny AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Lägga till AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). hello följande exempel skapas tre virtuella datorer:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

Det tar flera minuter toocreate och konfigurera alla tre virtuella datorer. hello identifierar belastningsutjämnaren, hälsoavsökningen automatiskt när hello appen körs på varje virtuell dator. När hello appen körs startar hello-regel för belastningsutjämnare toodistribute trafik.

### <a name="install-hello-app"></a>Installera hello app 

Tillägg för virtuell dator i Azure är används tooautomate virtuella konfigurationsåtgärder, till exempel installera program och konfigurera hello-operativsystemet. Hej [tillägget för anpassat skript för Windows](./../virtual-machines-windows-extensions-customscript.md) är används toorun alla PowerShell-skript på hello virtuella datorn. hello skript kan lagras i Azure storage, alla tillgängliga HTTP-slutpunkten eller inbäddat i hello konfiguration för anpassat skript. När du använder tillägget för anpassat skript för hello hanterar hello skriptkörningen hello Azure VM-agenten.

Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript. Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a>Testa din app

Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello följande exempel hämtar hello IP-adress för `myPublicIP` skapade tidigare:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Ange hello offentliga IP-adressen i tooa webbläsare. Hello IIS-standardwebbplatsen visas med hello NSG regeln på plats. 

![Standardwebbplatsen i IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>Steg 6 – administrativa uppgifter

Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS. toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer. Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren. 

### <a name="remove-a-vm-from-hello-load-balancer"></a>Ta bort en virtuell dator från hello belastningsutjämnare

Ta bort en virtuell dator från hello serverdelen för adresspoolen genom att återställa hello LoadBalancerBackendAddressPools-egenskapen för hello-nätverkskort.

Hämta hello nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Egenskapen hello LoadBalancerBackendAddressPools för hello nätverkskort för$ null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Uppdatera hello nätverkskort:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a>Lägga till en belastningsutjämnare för VM-toohello

När du utför VM underhåll eller om du behöver tooexpand kapacitet, lägger du till hello nätverkskort på en VM toohello serverdelsadresspool för belastningsutjämnare hello.

Hämta hello belastningsutjämnare:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Lägg till hello serverdelsadresspool för hello belastningen belastningsutjämnaren toohello nätverkskort:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Uppdatera hello nätverkskort:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nästa steg

Exempel – [exempel Azure virtuella PowerShell-skript](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
