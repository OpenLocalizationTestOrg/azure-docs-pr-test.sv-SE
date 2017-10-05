---
title: "Virtuella Azure-nätverk och virtuella Windows-datorer | Microsoft Docs"
description: "Självstudiekurs – hantera virtuella Azure-nätverk och virtuella Windows-datorer med Azure PowerShell"
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
ms.date: 05/02/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: c71c07f8ecd123a7e27848ba5043d46e315fcf03
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Hantera virtuella Azure-nätverk och virtuella Windows-datorer med Azure PowerShell

Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation. I kursen får du skapa flera virtuella datorer (VM) i ett virtuellt nätverk och konfigurerar nätverksanslutningen mellan dem. Lär dig att:

> [!div class="checklist"]
> * Skapa ett virtuellt nätverk
> * Skapa undernät för virtuellt nätverk
> * Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper
> * Visa regler för nätverkstrafik i praktiken

Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen. Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

## <a name="create-vnet"></a>Skapa VNet

Ett virtuellt nätverk är en representation av ditt eget nätverk i molnet. Ett virtuellt nätverk är en logisk isolering av Azure-molnet dedikerad till din prenumeration. Du hittar undernät, regler för anslutning till dessa undernät och anslutningar från de virtuella datorerna till undernäten inom ett VNet.

Innan du kan skapa andra Azure-resurser, måste du skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). I följande exempel skapas en resursgrupp med namnet *myRGNetwork* i den *EastUS* plats:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix. Nätverkskorten kan läggas till i undernät och anslutna till virtuella datorer kan man har anslutning för olika arbetsbelastningar.

Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Skapa ett VNET med namnet *myVNet* med *myFrontendSubnet* med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a>Skapa frontend VM

En virtuell dator, för att kommunicera på ett VNet måste den ha ett virtuellt nätverksgränssnitt (NIC). Den *myFrontendVM* nås från internet, så måste en offentlig IP-adress. 

Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

Skapa ett nätverkskort med [nya AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Ange användarnamn och lösenord behövs för administratören konto på den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Skapa de virtuella datorerna med [nya AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [lägga till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). 

```powershell
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="install-web-server"></a>Installera webbserver

Du kan installera IIS på *myFrontendVM* med hjälp av en fjärrskrivbordssession. Du måste hämta offentlig IP-adressen för den virtuella datorn att komma åt den.

Du kan hämta den offentliga IP-adressen för *myFrontendVM* med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). I följande exempel hämtar IP-adressen för *myPublicIPAddress* skapade tidigare:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

Anteckna denna IP-adress så att du kan använda den i kommande steg.

Använd följande kommando för att skapa en fjärrskrivbords-session med *myFrontendVM*. Ersätt  *<publicIPAddress>*  med adressen som du antecknade tidigare. När du uppmanas, anger du de autentiseringsuppgifter som används när du skapade den virtuella datorn.

```
mstsc /v:<publicIpAddress>
``` 

Nu när du har loggat in *myFrontendVM*, du kan använda en rad med PowerShell för att installera IIS och aktivera regeln för lokala brandväggen att tillåta webbtrafik. Öppna en PowerShell-kommandotolk och kör följande kommando:

Använd [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) att köra tillägget för anpassat skript som installerar IIS-webbserver:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Nu kan du använda den offentliga IP-adressen och bläddra till den virtuella datorn att se IIS-webbplatsen.

![Standardwebbplatsen i IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Hantera intern trafik

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverkstrafik till resurser som är anslutna till ett virtuellt nätverk. NSG: er kan vara kopplad till undernät eller individuella nätverkskort som är kopplade till virtuella datorer. Öppna eller stänga åtkomst till virtuella datorer via portar görs med hjälp av NSG-regler. När du skapade *myFrontendVM*, inkommande port 3389 öppnas automatiskt för RDP-anslutning.

Intern kommunikation för virtuella datorer kan konfigureras med en NSG. I det här avsnittet kan du lära dig hur du skapar en ytterligare undernät i nätverket och tilldelar en NSG till den för att tillåta en anslutning från *myFrontendVM* till *myBackendVM* på port 1433. Undernätet sedan tilldelas den virtuella datorn när den skapas.

Du kan begränsa intern trafik till *myBackendVM* från endast *myFrontendVM* genom att skapa en NSG för backend-undernät. I följande exempel skapas en NSG regeln med namnet *myBackendNSGRule* med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```powershell
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Lägg till en nätverkssäkerhetsgrupp med namnet *myBackendNSG* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a>Lägg till backend-undernät

Lägg till *myBackEndSubnet* till *myVNet* med [Lägg till AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```powershell
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
```

## <a name="create-back-end-vm"></a>Skapa backend-VM

Det enklaste sättet att skapa den virtuella datorn serverdel är med hjälp av en SQL Server-avbildning. Den här kursen kan du bara skapar den virtuella datorn med databasservern, men ger inte information om åtkomst till databasen.

Skapa *myBackendNic*:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Ange användarnamn och lösenord för administratörskontot på den virtuella datorn med Get-Credential:

```powershell
$cred = Get-Credential
```

Skapa *myBackendVM*:

```powershell
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

Den bild som används för SQL Server är installerad men används inte i den här kursen. Den ingår för att visa hur du kan konfigurera en virtuell dator för att hantera webbtrafik och en virtuell dator för att hantera databasen.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen skapas och skyddas av Azure-nätverk som är relaterade till virtuella datorer. 

> [!div class="checklist"]
> * Skapa ett virtuellt nätverk
> * Skapa undernät för virtuellt nätverk
> * Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper
> * Visa regler för nätverkstrafik i praktiken

Gå vidare till nästa kurs vill veta mer om övervakning för att skydda data på virtuella datorer med hjälp av Azure-säkerhetskopiering. .

> [!div class="nextstepaction"]
> [Säkerhetskopiera Windows-datorer i Azure](./tutorial-backup-vms.md)
