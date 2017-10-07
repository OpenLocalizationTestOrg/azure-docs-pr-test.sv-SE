---
title: aaaCreate och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen | Microsoft Docs
description: "Självstudiekurs – skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 20adcb673ef4de683e6ad82d048a9625a1dc838c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a>Skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen

Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö. Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator. Lär dig att:

> [!div class="checklist"]
> * Skapa och ansluta tooa VM
> * Välj och Använd VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Ändra storlek på en virtuell dator
> * Visa och förstå tillstånd för virtuell dator

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

## <a name="create-resource-group"></a>Skapa resursgrupp

Skapa en resursgrupp med hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando. 

En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas innan en virtuell dator. I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i hello *EastUS* region. 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

hello resursgrupp anges när du skapar eller ändrar en VM som kan ses i hela den här kursen.

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

En virtuell dator måste vara anslutna tooa virtuellt nätverk. Du kan kommunicera med hello virtuell dator med en offentlig IP-adress via ett nätverkskort.

### <a name="create-virtual-network"></a>Skapa det virtuella nätverket

Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Skapa offentlig IP-adress

Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Skapa nätverkskort

Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Skapa nätverkssäkerhetsgrupp

En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer. Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall. De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator. tooaccess hello IIS webbserver som du installerar, måste du lägga till en regel för inkommande NSG.

toocreate en inkommande NSG regel använder [Lägg till AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). hello följande exempel skapas en NSG regeln med namnet *myNSGRule* som öppnar port *3389* för hello virtuell dator:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Skapa hello NSG med *myNSGRule* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Lägg till hello NSG toohello undernät i hello virtuellt nätverk med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Uppdatera hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Skapa en virtuell dator

När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken. I det här exemplet skapas en virtuell dator med namnet *myVM* körs hello senaste versionen av Windows Server 2016 Datacenter.

Ange hello användarnamn och lösenord som krävs för hello administratörskontot på hello virtuell dator med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Skapa hello inledande konfiguration för hello virtuell dator med [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Lägg till hello operativsystemet information toohello konfigurationen virtuella datorn med [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Lägg till hello avbildningen information toohello konfiguration av virtuell dator med [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Lägg till hello operativsystemet inställningar toohello virtuella diskkonfigurationen med [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Lägg till hello-nätverkskort som du skapade tidigare toohello konfiguration av virtuell dator med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a>Ansluta tooVM

Skapa en anslutning till fjärrskrivbord med hello virtuella datorn när hello distributionen har slutförts.

Kör hello följande kommandon tooreturn hello offentliga IP-adress för hello virtuell dator. Anteckna denna IP-adress så kan du ansluta tooit med din webbläsare tootest webbanslutningen i ett kommande steg.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Använd hello följande kommando toocreate en fjärrskrivbordssession med hello virtuell dator. Ersätt hello IP-adress med hello *publicIPAddress* i den virtuella datorn. När du uppmanas ange hello-autentiseringsuppgifter som används när du skapar hello virtuell dator.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Förstå VM-avbildningar

hello Azure marketplace innehåller många avbildningar av virtuella datorer som kan använda toocreate en ny virtuell dator. I föregående steg hello skapades en virtuell dator med hello Windows Server 2016-Datacenter-avbildning. I det här steget är hello PowerShell-modulen används toosearch hello marketplace för andra Windows-avbildningar kan också som bas för nya virtuella datorer. Den här processen består av att hitta hello utgivare, erbjudande och hello avbildningsnamn (Sku). 

Använd hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) kommandot tooreturn en lista över avbildningen utgivare.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Använd hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn en lista med bild erbjudanden. Med det här kommandot returnerade hello listan filtreras på hello angiven utgivare. 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

Hej [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) kommandot kommer sedan filtrera hello utgivare och erbjudandet namnet tooreturn en lista över namn på bilder.

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer EastUS  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer EastUS
```

Den här informationen kan vara används toodeploy en virtuell dator med en viss bild. Det här exemplet anger hello avbildningens namn på hello VM-objekt. Läs toohello föregående exempel i den här självstudiekursen fullständig distributionssteg.

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Förstå VM-storlekar

Storlek på en virtuell dator bestäms hello beräkningsresurser som Processorn och GPU-minne som görs tillgängliga toohello virtuella datorn. Virtuella datorer måste toobe som skapats med en lämplig för hello räknar arbetsbelastning. Om belastningen ökar, kan en befintlig virtuell dator ändras.

### <a name="vm-sizes"></a>VM-storlekar

i den följande tabellen hello kategoriserar storlekar i användningsfall.  

| Typ                     | Storlekar           |    Beskrivning       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Generellt syfte         |DSv2, Dv2, DS, D, Av2, A0 7| Belastningsutjämnade CPU-till-minne. Idealiskt för dev / test och lösningar för små toomedium program och data.  |
| Beräkningsoptimerad      | FS, F             | Hög CPU-till-minne. Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.        |
| Minnesoptimerad       | GS, G, DSv2, DS, Dv2, D   | Hög minne-till-core. Perfekt för relationsdatabaser, medelhög toolarge cacheminnen och analyser i minnet.                 |
| Lagringsoptimerad       | Ls                | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser.                                                         |
| GPU           | NV NC            | Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.       |
| Hög prestanda | H, A8-11          | Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA). 


### <a name="find-available-vm-sizes"></a>Hitta tillgängliga storlekar på VM

toosee en lista över Virtuella storlekar tillgängliga i en viss region, Använd hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando.

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Ändra storlek på en virtuell dator

När en virtuell dator har distribuerats kan storleksändrade tooincrease eller minska resursallokering.

Innan du ändrar storlek på en virtuell dator, kontrollera om hello önskad storlek är tillgänglig på hello aktuella virtuella datorns kluster. Hej [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando returnerar en lista över storlekar. 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Eventuellt hello storleken är tillgänglig hello VM storlek kan ändras från ett slås på tillstånd, men den startas under hello-åtgärd.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Om hello önskad storlek är inte på hello aktuella klustret hello VM behov toobe frigjorts innan hello att ändra storlek på kan uppstå. Observera att när hello VM är påslagen tillbaka, några data på hello temp disken har tagits bort och hello offentliga IP-adressen ändras såvida inte en statisk IP-adress används. 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Energisparfunktioner för VM

En virtuell Azure-dator kan ha en av många energisparfunktioner. Det här tillståndet representerar hello aktuell status för hello VM från hello synvinkel av hello hypervisor. 

### <a name="power-states"></a>Energisparfunktioner

| Energisparläge | Beskrivning
|----|----|
| Startar | Anger hello virtuella datorn startas. |
| Körs | Anger att hello den virtuella datorn körs. |
| Stoppas | Anger att hello den virtuella datorn stoppas. | 
| Stoppad | Anger att hello den virtuella datorn har stoppats. Observera att virtuella datorer i hello stoppats tillstånd fortfarande avgifter beräkning.  |
| Det har frigjorts | Anger att hello den virtuella datorn har flyttats. |
| Frigöra | Anger att hello den virtuella datorn bort helt från hello hypervisor men är fortfarande tillgänglig i hello kontrollplan. Virtuella datorer i hello Deallocated tillstånd inte avgifter beräkning. |
| - | Anger att hello energisparläge för hello virtuell dator är okänt. |

### <a name="find-power-state"></a>Hitta energiläge

tooretrieve hello tillståndet för en viss virtuell dator, Använd hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando. Vara säker på att toospecify ett giltigt namn för en virtuell dator och resursgruppen. 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Resultat:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Administrativa uppgifter

Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator. Dessutom kan du toocreate skript tooautomate repetitiva och komplicerade uppgifter. Med hjälp av Azure PowerShell, kan många vanliga administrativa uppgifter köras från hello kommandoraden eller i skript.

### <a name="stop-virtual-machine"></a>Stoppa den virtuella datorn

Stoppa och ta bort en virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Om du vill tookeep hello virtuell dator i ett etablerat tillstånd, använder du hello - StayProvisioned-parametern.

### <a name="start-virtual-machine"></a>Starta den virtuella datorn

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Ta bort resursgruppen

En resursgrupp också tar du bort alla resurser som ingår i.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:

> [!div class="checklist"]
> * Skapa och ansluta tooa VM
> * Välj och Använd VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Ändra storlek på en virtuell dator
> * Visa och förstå tillstånd för virtuell dator

Avancera toohello nästa självstudiekurs toolearn om Virtuella diskar.  

> [!div class="nextstepaction"]
> [Skapa och hantera Virtuella diskar](./tutorial-manage-data-disk.md)
