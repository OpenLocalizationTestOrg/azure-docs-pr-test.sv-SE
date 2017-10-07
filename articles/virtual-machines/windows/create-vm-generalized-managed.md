---
title: "aaaCreate virtuell dator från en hanterad VM-avbildning i Azure | Microsoft Docs"
description: "Skapa en virtuell Windows-dator från en generaliserad hanterade VM-avbildning med hjälp av Azure PowerShell i hello Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: 5036ef1533c144a9a328e94599b359e0166f337d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Skapa en virtuell dator från en hanterad avbildning

Du kan skapa flera virtuella datorer från en hanterad VM-avbildning i Azure. En hanterad VM-avbildning innehåller hello information behövs toocreate en virtuell dator, inklusive hello Operativsystemet och datadiskarna. hello virtuella hårddiskar som utgör hello-avbildning, inklusive både hello OS-diskar och eventuella hårddiskar, lagras som hanterade diskar. 


## <a name="prerequisites"></a>Krav

Du behöver toohave redan [skapas en hanterade VM-avbildning](capture-image-resource.md) toouse för att skapa hello ny virtuell dator. 

Kontrollera att du har hello senaste versionerna av hello AzureRM.Compute och AzureRM.Network PowerShell-moduler. Öppna ett PowerShell-Kommandotolken som administratör och kör följande kommando tooinstall hello dem.

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).



## <a name="collect-information-about-hello-image"></a>Samla in information om hello avbildning

Först vi behöver toogather grundläggande information om hello avbildningen och skapa en variabel för hello avbildningen. Det här exemplet används en hanterad VM-avbildning med namnet **myImage** som är i hello **myResourceGroup** resursgrupp i hello **Väst centrala oss** plats. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

1. Skapa hello undernät. Det här exemplet skapar ett undernät med namnet **mySubnet** med hello adressprefixet **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Skapa hello virtuellt nätverk. Det här exemplet skapar ett virtuellt nätverk med namnet **myVnet** med hello adressprefixet **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Skapa en offentlig IP-adress och gränssnitt

tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

1. Skapa en offentlig IP-adress. Det här exemplet skapas en offentlig IP-adress med namnet **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Skapa hello NIC. Det här exemplet skapar ett nätverkskort med namnet **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Skapa hello nätverkssäkerhetsgruppen och en RDP-regel

toobe kan toolog i tooyour VM som använder RDP måste toohave en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389. 

Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389. Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

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


## <a name="create-a-variable-for-hello-virtual-network"></a>Skapa en variabel för hello virtuellt nätverk

Skapa en variabel för hello slutförts virtuellt nätverk. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a>Hämta hello autentiseringsuppgifter för hello VM

hello öppnar följande cmdlet ett fönster där anger du en ny användare och lösenord toouse som hello lokalt administratörskonto för att få fjärråtkomst till hello VM. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a>Ange variabler för hello VM namn, datornamn och hello storleken på hello VM

1. Skapa variabler för hello VM namn och datornamn. Det här exemplet anger hello VM-namn som **myVM** och hello datornamn som **den här datorn**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Ange hello storleken på hello virtuella datorn. Det här exemplet skapar **Standard_DS1_v2** storlek VM. Se hello [VM-storlekar](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentationen för mer information.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Lägg till hello namn på virtuell dator och storlek toohello VM-konfiguration.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Ange hello VM-avbildning som källbilden för hello ny virtuell dator

Ange hello Källavbildningen med hello-ID för hello hanterade VM-avbildning.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Ange hello Operativsystemets konfiguration och Lägg till hello NIC.

Ange hello lagringstyp (PremiumLRS eller StandardLRS) och hello hello OS-diskens storlek. Det här exemplet anger hello kontotyp för**PremiumLRS**, hello diskstorleken för**128 GB** och diskcachelagring för**ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Skapa hello VM

Skapa ny virtuell dator med hello-konfiguration som vi har skapat och lagras i hello hello **$vm** variabeln.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a>Kontrollera att hello VM skapades
När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nästa steg
din nya virtuella datorn med Azure PowerShell finns i toomanage [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

