---
title: "aaaUpload en generaliserad virtuell Hårddisk tooAzure PowerShell skriptexempel | Microsoft Docs"
description: "PowerShell-exempel på skript tooupload en generaliserad virtuell Hårddisk tooAzure och skapa en ny virtuell dator med hjälp av hello resource manager-distributionsmodellen och hanterade diskar."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/18/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 72b4ff09eb7a6ba1604b9d5cbac51e60eded7545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-script-tooupload-a-vhd-tooazure-and-create-a-new-vm"></a>Exempel på skript tooupload en VHD-tooAzure och skapa en ny virtuell dator

Det här skriptet tar en lokal VHD-fil från en generaliserad virtuell dator och överför det tooAzure, skapar en hanterad diskavbildning och hello toocreate en ny virtuell dator.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

```powershell
# Provide values for hello variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get hello username and password toobe used for hello administrators account on hello VM. 
# This is used when connecting toohello VM using RDP.

$cred = Get-Credential

# Upload hello VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading hello VHD may take awhile!

# Create a managed image from hello uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create hello networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building hello VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set hello VM image as source image for hello new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish hello VM configuration and add hello NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create hello VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that hello VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate hello distribution. Varje objekt i hello tabellen länkar toocommand viss dokumentation.

| Kommando                                                                                                             | Anteckningar                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | Skapar en resursgrupp som är lagrade i alla resurser.                                                                                                                          |
| [Ny AzureRmStorageAccount](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | Skapar ett lagringskonto.                                                                                                                                                           |
| [Lägg till AzureRmVhd](/powershell/module/azurerm.resources/add-azurermvhd)                                               | Överför en virtuell hårddisk från en lokal virtuell dator tooa blob i ett moln-lagringskonto i Azure.                                                                       |
| [Ny AzureRmImageConfig](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | Skapar en konfigurerbar image-objekt.                                                                                                                                                 |
| [Ange AzureRmImageOsDisk](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | Anger hello operativsystemet diskegenskaper i ett image-objekt.                                                                                                                        |
| [Ny AzureRmImage](/powershell/module/azurerm.resources/new-azurermimage)                                           | Skapar en ny avbildning.                                                                                                                                                                 |
| [Ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | Skapar en konfiguration av undernät. Den här konfigurationen används med hello processen för att skapa virtuella nätverk.                                                                                |
| [Ny AzureRmVirtualNetwork](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | Skapar ett virtuellt nätverk.                                                                                                                                                           |
| [Ny AzureRmPublicIpAddress](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | Skapar en offentlig IP-adress.                                                                                                                                                         |
| [Ny AzureRmNetworkInterface](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | Skapar ett nätverksgränssnitt.                                                                                                                                                         |
| [Ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | Skapar en grupp regeln nätverkssäkerhetskonfigurationen. Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.                                                       |
| [Ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | Skapar en nätverkssäkerhetsgrupp.                                                                                                                                                    |
| [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | Hämtar ett virtuellt nätverk i en resursgrupp.                                                                                                                                          |
| [Ny AzureRmVMConfig](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | Skapar en VM-konfiguration. Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter. hello konfiguration används under skapande av virtuell dator. |
| [Ange AzureRmVMSourceImage](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | Anger en avbildning för en virtuell dator.                                                                                                                                            |
| [Ange AzureRmVMOSDisk](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | Anger hello operativsystemet diskegenskaper på en virtuell dator.                                                                                                                      |
| [Ange AzureRmVMOperatingSystem](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | Anger hello operativsystemet diskegenskaper på en virtuell dator.                                                                                                                      |
| [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | Lägger till en network interface tooa virtuell dator.                                                                                                                                       |
| [Ny AzureRmVM](/powershell/module/azurerm.resources/new-azurermvm)                                                 | Skapa en virtuell dator.                                                                                                                                                            |
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | Tar bort en resursgrupp och alla resurser som ingår i.                                                                                                                         |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
