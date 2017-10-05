---
title: "Skapa virtuell dator från en hanterad VM-avbildning i Azure | Microsoft Docs"
description: "Skapa en virtuell Windows-dator från en generaliserad hanterade VM-avbildning med hjälp av Azure PowerShell i Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 2bb2d66271178a64ec0f4642e46b23f5618a56d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Skapa en virtuell dator från en hanterad avbildning

Du kan skapa flera virtuella datorer från en hanterad VM-avbildning i Azure. En hanterad VM-avbildning innehåller informationen som behövs för att skapa en virtuell dator, inklusive Operativsystemet och datadiskarna. De virtuella hårddiskar som bilden, inklusive både OS-diskar och eventuella hårddiskar, lagras som hanterade diskar. 


## <a name="prerequisites"></a>Krav

Du måste redan ha [skapas en hanterade VM-avbildning](capture-image-resource.md) ska användas för att skapa den nya virtuella datorn. 

Kontrollera att du har de senaste versionerna av AzureRM.Compute och AzureRM.Network PowerShell-moduler. Öppna ett PowerShell-Kommandotolken som administratör och kör följande kommando för att installera dem.

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).



## <a name="collect-information-about-the-image"></a>Samla in information om bilden

Vi måste först samla in grundläggande information om bilden och skapa en variabel för bilden. Det här exemplet används en hanterad VM-avbildning med namnet **myImage** som finns i den **myResourceGroup** resursgrupp i den **Väst centrala oss** plats. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

1. Skapa undernätet. Det här exemplet skapar ett undernät med namnet **mySubnet** med adressprefixet för **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Skapa det virtuella nätverket. Det här exemplet skapar ett virtuellt nätverk med namnet **myVnet** med adressprefixet för **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Skapa en offentlig IP-adress och gränssnitt

För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

1. Skapa en offentlig IP-adress. Det här exemplet skapas en offentlig IP-adress med namnet **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Skapa nätverkskortet. Det här exemplet skapar ett nätverkskort med namnet **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Skapa säkerhetsgrupp för nätverk och en RDP-regel

Du måste ha en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP. 

Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389. Mer information om NSG: er finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Skapa en variabel för det virtuella nätverket

Skapa en variabel för det virtuella nätverket som slutförda. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>Hämta autentiseringsuppgifterna för den virtuella datorn

Följande cmdlet öppnas ett fönster där du ska ange ett nytt användarnamn och lösenord ska användas som ett lokalt administratörskonto för att få fjärråtkomst till den virtuella datorn. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Ange variabler för VM-namn, datornamn och storleken på den virtuella datorn

1. Skapa variabler för namn på virtuell dator och datornamn. Det här exemplet anges på det Virtuella datornamnet som **myVM** och datornamn som **den här datorn**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Ange storleken på den virtuella datorn. Det här exemplet skapar **Standard_DS1_v2** storlek VM. Finns det [VM-storlekar](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentationen för mer information.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Lägg till VM-namnet och storleken i VM-konfigurationen.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Ange VM-avbildning som källbilden för den nya virtuella datorn

Ange källa avbildningen med ID: T för den hanterade VM-avbildningen.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Ange Operativsystemets konfiguration och Lägg till nätverkskortet.

Ange lagringstypen av (PremiumLRS eller StandardLRS) och storleken på operativsystemdisken. Det här exemplet anges kontotypen **PremiumLRS**, diskens storlek till **128 GB** och disk caching **ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Skapa den virtuella datorn

Skapa den nya virtuella datorn med hjälp av den konfiguration som vi har skapat och lagras i den **$vm** variabeln.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Kontrollera att den virtuella datorn har skapats
När du är klar bör du se den nya virtuella datorn i den [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nästa steg
För att hantera din nya virtuella datorn med Azure PowerShell, se [skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

