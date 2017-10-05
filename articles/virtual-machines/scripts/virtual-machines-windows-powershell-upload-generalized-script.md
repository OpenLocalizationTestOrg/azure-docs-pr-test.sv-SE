---
title: "Ladda upp en generaliserad virtuell Hårddisk till Azure PowerShell skriptexempel | Microsoft Docs"
description: "PowerShell-exempelskript att överföra en generaliserad virtuell Hårddisk till Azure och skapa en ny virtuell dator med hjälp av resource manager-distributionsmodellen och hanterade diskar."
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
ms.openlocfilehash: cd3d87bb4384971e28d3330cd5c1a3d351129036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sample-script-to-upload-a-vhd-to-azure-and-create-a-new-vm"></a><span data-ttu-id="adc61-103">Exempelskript för att överföra en virtuell Hårddisk till Azure och skapa en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="adc61-103">Sample script to upload a VHD to Azure and create a new VM</span></span>

<span data-ttu-id="adc61-104">Det här skriptet tar en lokal VHD-fil från en generaliserad virtuell dator och överför den till Azure, skapas en hanterad diskavbildning och använder den att skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-104">This script takes a local .vhd file from a generalized VM and uploads it to Azure, creates a Managed Disk image and uses the to create a new VM.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="adc61-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="adc61-105">Sample script</span></span>

```powershell
# Provide values for the variables
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

# Get the username and password to be used for the administrators account on the VM. 
# This is used when connecting to the VM using RDP.

$cred = Get-Credential

# Upload the VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading the VHD may take awhile!

# Create a managed image from the uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create the networking resources
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

# Start building the VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set the VM image as source image for the new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish the VM configuration and add the NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create the VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that the VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a><span data-ttu-id="adc61-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="adc61-106">Clean up deployment</span></span> 

<span data-ttu-id="adc61-107">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="adc61-107">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="adc61-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="adc61-108">Script explanation</span></span>

<span data-ttu-id="adc61-109">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="adc61-109">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="adc61-110">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="adc61-110">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="adc61-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="adc61-111">Command</span></span>                                                                                                             | <span data-ttu-id="adc61-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="adc61-112">Notes</span></span>                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="adc61-113">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="adc61-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | <span data-ttu-id="adc61-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="adc61-114">Creates a resource group in which all resources are stored.</span></span>                                                                                                                          |
| [<span data-ttu-id="adc61-115">Ny AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="adc61-115">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | <span data-ttu-id="adc61-116">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="adc61-116">Creates a storage account.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="adc61-117">Lägg till AzureRmVhd</span><span class="sxs-lookup"><span data-stu-id="adc61-117">Add-AzureRmVhd</span></span>](/powershell/module/azurerm.resources/add-azurermvhd)                                               | <span data-ttu-id="adc61-118">Överför en virtuell hårddisk från en lokal virtuell dator till en blobb i ett moln-lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="adc61-118">Uploads a virtual hard disk from an on-premises virtual machine to a blob in a cloud storage account in Azure.</span></span>                                                                       |
| [<span data-ttu-id="adc61-119">Ny AzureRmImageConfig</span><span class="sxs-lookup"><span data-stu-id="adc61-119">New-AzureRmImageConfig</span></span>](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | <span data-ttu-id="adc61-120">Skapar en konfigurerbar image-objekt.</span><span class="sxs-lookup"><span data-stu-id="adc61-120">Creates a configurable image object.</span></span>                                                                                                                                                 |
| [<span data-ttu-id="adc61-121">Ange AzureRmImageOsDisk</span><span class="sxs-lookup"><span data-stu-id="adc61-121">Set-AzureRmImageOsDisk</span></span>](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | <span data-ttu-id="adc61-122">Anger operativsystemet diskegenskaper i ett image-objekt.</span><span class="sxs-lookup"><span data-stu-id="adc61-122">Sets the operating system disk properties on an image object.</span></span>                                                                                                                        |
| [<span data-ttu-id="adc61-123">Ny AzureRmImage</span><span class="sxs-lookup"><span data-stu-id="adc61-123">New-AzureRmImage</span></span>](/powershell/module/azurerm.resources/new-azurermimage)                                           | <span data-ttu-id="adc61-124">Skapar en ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="adc61-124">Creates a new image.</span></span>                                                                                                                                                                 |
| [<span data-ttu-id="adc61-125">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="adc61-125">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="adc61-126">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="adc61-126">Creates a subnet configuration.</span></span> <span data-ttu-id="adc61-127">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="adc61-127">This configuration is used with the virtual network creation process.</span></span>                                                                                |
| [<span data-ttu-id="adc61-128">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="adc61-128">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | <span data-ttu-id="adc61-129">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="adc61-129">Creates a virtual network.</span></span>                                                                                                                                                           |
| [<span data-ttu-id="adc61-130">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="adc61-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | <span data-ttu-id="adc61-131">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="adc61-131">Creates a public IP address.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="adc61-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="adc61-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | <span data-ttu-id="adc61-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="adc61-133">Creates a network interface.</span></span>                                                                                                                                                         |
| [<span data-ttu-id="adc61-134">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="adc61-134">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | <span data-ttu-id="adc61-135">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="adc61-135">Creates a network security group rule configuration.</span></span> <span data-ttu-id="adc61-136">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="adc61-136">This configuration is used to create an NSG rule when the NSG is created.</span></span>                                                       |
| [<span data-ttu-id="adc61-137">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="adc61-137">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | <span data-ttu-id="adc61-138">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="adc61-138">Creates a network security group.</span></span>                                                                                                                                                    |
| [<span data-ttu-id="adc61-139">Get-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="adc61-139">Get-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | <span data-ttu-id="adc61-140">Hämtar ett virtuellt nätverk i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="adc61-140">Gets a virtual network in a resource group.</span></span>                                                                                                                                          |
| [<span data-ttu-id="adc61-141">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="adc61-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | <span data-ttu-id="adc61-142">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="adc61-142">Creates a VM configuration.</span></span> <span data-ttu-id="adc61-143">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="adc61-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="adc61-144">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-144">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="adc61-145">Ange AzureRmVMSourceImage</span><span class="sxs-lookup"><span data-stu-id="adc61-145">Set-AzureRmVMSourceImage</span></span>](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | <span data-ttu-id="adc61-146">Anger en avbildning för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-146">Specifies an image for a virtual machine.</span></span>                                                                                                                                            |
| [<span data-ttu-id="adc61-147">Ange AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="adc61-147">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | <span data-ttu-id="adc61-148">Anger operativsystemet diskegenskaper på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-148">Sets the operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="adc61-149">Ange AzureRmVMOperatingSystem</span><span class="sxs-lookup"><span data-stu-id="adc61-149">Set-AzureRmVMOperatingSystem</span></span>](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | <span data-ttu-id="adc61-150">Anger operativsystemet diskegenskaper på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-150">Sets the operating system disk properties on a virtual machine.</span></span>                                                                                                                      |
| [<span data-ttu-id="adc61-151">Lägg till AzureRmVMNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="adc61-151">Add-AzureRmVMNetworkInterface</span></span>](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | <span data-ttu-id="adc61-152">Lägger till ett nätverksgränssnitt i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-152">Adds a network interface to a virtual machine.</span></span>                                                                                                                                       |
| [<span data-ttu-id="adc61-153">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="adc61-153">New-AzureRmVM</span></span>](/powershell/module/azurerm.resources/new-azurermvm)                                                 | <span data-ttu-id="adc61-154">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="adc61-154">Create a virtual machine.</span></span>                                                                                                                                                            |
| [<span data-ttu-id="adc61-155">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="adc61-155">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | <span data-ttu-id="adc61-156">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="adc61-156">Removes a resource group and all resources contained within.</span></span>                                                                                                                         |

## <a name="next-steps"></a><span data-ttu-id="adc61-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adc61-157">Next steps</span></span>

<span data-ttu-id="adc61-158">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="adc61-158">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="adc61-159">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="adc61-159">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
