---
title: "Självstudiekurs – skapa ett program som har hög tillgänglighet på Azure Virtual Machines | Microsoft Docs"
description: "Lär dig hur du skapar ett program med hög tillgänglighet och säkert över tre virtuella Windows-datorer med en belastningsutjämnare i Azure"
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
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Skapa en belastningsutjämnad, hög tillgänglighet program på Windows-datorer i Azure

I den här kursen kan du skapa ett program för hög tillgänglighet som är motståndskraftiga mot underhållshändelser. Appen använder en belastningsutjämnare, en tillgänglighetsuppsättning och tre virtuella Windows-datorer (VM). Den här självstudiekursen installerar IIS, även om du kan använda den här självstudiekursen för att distribuera ett programramverk för olika som använder samma komponenter för hög tillgänglighet och riktlinjer. 

## <a name="step-1---azure-prerequisites"></a>Steg 1 – krav för Azure

För att slutföra den här självstudiekursen måste du kontrollera att du har installerat senast [Azure PowerShell](/powershell/azure/overview) modul.

Logga in på Azure-prenumerationen med kommandot Login-AzureRmAccount först och följ de på skärmen riktningar.

```powershell
Login-AzureRmAccount
```

En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. Innan du kan skapa andra Azure-resurser, måste du skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westeurope` region: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>Steg 2 – Skapa tillgänglighetsuppsättning

Virtuella datorer kan skapa logiska fel och uppdatera domäner. Varje logiska domän representerar en del av maskinvara i det underliggande Azure-datacentret. När du skapar två eller flera virtuella datorer distribueras resurserna beräkning och lagring i dessa domäner. Den här distributionen underhåller tillgängligheten för din app om en maskinvarukomponent måste underhåll. Tillgänglighetsuppsättningar kan du definiera dessa logiska fel- och update-domäner.

Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). I följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:

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

En Azure belastningsutjämnare distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren. En hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik till en virtuell dator i drift.

### <a name="create-public-ip-address"></a>Skapa offentlig IP-adress

Tilldela en offentlig IP-adress till belastningsutjämnaren för att komma åt din app på Internet. Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). I följande exempel skapas en offentlig IP-adress med namnet `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Skapa belastningsutjämnare

Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). I följande exempel skapas en IP-adress för klientdel som heter `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). I följande exempel skapas en backend-adresspool som heter `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Nu ska du skapa belastningsutjämnaren med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). I följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer` med hjälp av den `myPublicIP` adress:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Skapa hälsoavsökningen

Om du vill att belastningsutjämnaren ska övervaka status för din app kan du använda en hälsoavsökningen. Avsökningen hälsa dynamiskt lägger till eller tar bort virtuella datorer från belastningen belastningsutjämnaren rotationen baserat på deras svar på hälsokontroller. En virtuell dator tas bort från belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.

Skapa en hälsoavsökningen med [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). I följande exempel skapas en hälsoavsökningen med namnet `myHealthProbe` som övervakar varje virtuell dator:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Skapa regel för belastningsutjämnare

En regel för belastningsutjämnare används för att definiera hur trafiken distribueras till de virtuella datorerna.

Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). I följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRule` och balanserar trafik på port `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Uppdaterar belastningsutjämnaren med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>Steg 4 – Konfigurera nätverk

Varje virtuell dator har en eller flera virtuella nätverkskort (NIC) som ansluter till ett virtuellt nätverk. Det här virtuella nätverket skyddas för att filtrera trafik baserat på definierade åtkomstregler.

### <a name="create-virtual-network"></a>Skapa det virtuella nätverket

Konfigurera först ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). I följande exempel skapas ett undernät med namnet `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

För att ge nätverksanslutning till dina virtuella datorer, skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). I följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med `mySubnet`:

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

För att tillåta webbtrafik att nå din app, skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). I följande exempel skapas en grupp nätverkssäkerhetsregeln med namnet `myNetworkSecurityGroupRule`:

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

Skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). I följande exempel skapas en NSG som heter `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Lägga till nätverkssäkerhetsgruppen till undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Uppdatera det virtuella nätverket med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Skapa virtuellt nätverkskort

Läsa in belastningsutjämning funktionen med virtuell NIC-resurs i stället för verkliga VM. Det virtuella nätverkskortet är anslutet till belastningsutjämnaren och bifogas till en virtuell dator.

Skapa ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). I följande exempel skapas tre virtuella nätverkskort. (Ett virtuellt nätverkskort för varje virtuell du skapa för din app nedan):


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

Du kan nu skapa högtillgängliga virtuella datorer om du vill köra appen med alla de underliggande komponenterna på plats. 

Hämta användarnamn och lösenord för administratörskontot på den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Skapa de virtuella datorerna med [nya AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [lägga till AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). I följande exempel skapas tre virtuella datorer:

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

Det tar flera minuter att skapa och konfigurera alla tre virtuella datorer. I belastningsutjämnaren, hälsoavsökningen identifierar automatiskt när appen körs på varje virtuell dator. När appen körs börjar belastningsutjämningsregeln distribuera trafiken.

### <a name="install-the-app"></a>Installera appen 

Tillägg för virtuella Azure-datorn används för att automatisera åtgärder för konfiguration av virtuell dator, till exempel installera program och konfigurera operativsystemet. Den [tillägget för anpassat skript för Windows](./../virtual-machines-windows-extensions-customscript.md) används för att köra PowerShell-skript på den virtuella datorn. Skriptet kan lagras i Azure storage, valfri tillgänglig HTTP-slutpunkt och inbäddade i konfigurationen för anpassat skript för tillägget. När du använder tillägget för anpassat skript, hanterar Virtuella Azure-agenten körningen av skriptet.

Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) att installera tillägget för anpassat skript. Tillägget körs `powershell Add-WindowsFeature Web-Server` att installera IIS-webbserver:

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

Hämta offentlig IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). I följande exempel hämtar IP-adressen för `myPublicIP` skapade tidigare:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Ange den offentliga IP-adressen i en webbläsare. IIS-standardwebbplatsen visas med NSG-regeln på plats. 

![Standardwebbplatsen i IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>Steg 6 – administrativa uppgifter

Du kan behöva utföra underhåll på virtuella datorer som kör appen, till exempel installera uppdateringar av OS. Du kan behöva lägga till ytterligare virtuella datorer för att hantera den ökade trafiken till din app. Det här avsnittet visar hur du tar bort eller lägga till en virtuell dator från belastningsutjämnaren. 

### <a name="remove-a-vm-from-the-load-balancer"></a>Ta bort en virtuell dator från belastningsutjämnaren

Ta bort en virtuell dator från backend-adresspool genom att återställa egenskapen LoadBalancerBackendAddressPools för nätverkskortet.

Hämta nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Egenskapen LoadBalancerBackendAddressPools för nätverkskortet till $null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Uppdatera nätverkskortet:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a>Lägga till en virtuell dator till belastningsutjämnaren

När du utför VM underhåll eller om du behöver Utöka kapaciteten att lägga till nätverkskort på en virtuell dator serverdelsadresspool för belastningsutjämnaren.

Hämta belastningsutjämnaren:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Lägg till backend-adresspool för belastningsutjämnaren nätverkskortet:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Uppdatera nätverkskortet:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nästa steg

Exempel – [exempel Azure virtuella PowerShell-skript](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
