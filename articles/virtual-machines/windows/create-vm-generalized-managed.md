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
# <a name="create-a-vm-from-a-managed-image"></a><span data-ttu-id="b33c1-103">Skapa en virtuell dator från en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="b33c1-103">Create a VM from a managed image</span></span>

<span data-ttu-id="b33c1-104">Du kan skapa flera virtuella datorer från en hanterad VM-avbildning i Azure.</span><span class="sxs-lookup"><span data-stu-id="b33c1-104">You can create multiple VMs from a managed VM image in Azure.</span></span> <span data-ttu-id="b33c1-105">En hanterad VM-avbildning innehåller informationen som behövs för att skapa en virtuell dator, inklusive Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="b33c1-105">A managed VM image contains the information necessary to create a VM, including the OS and data disks.</span></span> <span data-ttu-id="b33c1-106">De virtuella hårddiskar som bilden, inklusive både OS-diskar och eventuella hårddiskar, lagras som hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="b33c1-106">The VHDs that make up the image, including both the OS disks and any data disks, are stored as managed disks.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="b33c1-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b33c1-107">Prerequisites</span></span>

<span data-ttu-id="b33c1-108">Du måste redan ha [skapas en hanterade VM-avbildning](capture-image-resource.md) ska användas för att skapa den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b33c1-108">You need to have already [created a managed VM image](capture-image-resource.md) to use for creating the new VM.</span></span> 

<span data-ttu-id="b33c1-109">Kontrollera att du har de senaste versionerna av AzureRM.Compute och AzureRM.Network PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="b33c1-109">Make sure that you have the latest versions of the AzureRM.Compute and AzureRM.Network PowerShell modules.</span></span> <span data-ttu-id="b33c1-110">Öppna ett PowerShell-Kommandotolken som administratör och kör följande kommando för att installera dem.</span><span class="sxs-lookup"><span data-stu-id="b33c1-110">Open a PowerShell prompt as an Administrator and run the following command to install them.</span></span>

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
<span data-ttu-id="b33c1-111">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b33c1-111">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>



## <a name="collect-information-about-the-image"></a><span data-ttu-id="b33c1-112">Samla in information om bilden</span><span class="sxs-lookup"><span data-stu-id="b33c1-112">Collect information about the image</span></span>

<span data-ttu-id="b33c1-113">Vi måste först samla in grundläggande information om bilden och skapa en variabel för bilden.</span><span class="sxs-lookup"><span data-stu-id="b33c1-113">First we need to gather basic information about the image and create a variable for the image.</span></span> <span data-ttu-id="b33c1-114">Det här exemplet används en hanterad VM-avbildning med namnet **myImage** som finns i den **myResourceGroup** resursgrupp i den **Väst centrala oss** plats.</span><span class="sxs-lookup"><span data-stu-id="b33c1-114">This example uses a managed VM image named **myImage** that is in the **myResourceGroup** resource group in the **West Central US** location.</span></span> 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="b33c1-115">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b33c1-115">Create a virtual network</span></span>
<span data-ttu-id="b33c1-116">Skapa vNet och undernät för den [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b33c1-116">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="b33c1-117">Skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="b33c1-117">Create the subnet.</span></span> <span data-ttu-id="b33c1-118">Det här exemplet skapar ett undernät med namnet **mySubnet** med adressprefixet för **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-118">This example creates a subnet named **mySubnet** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="b33c1-119">Skapa det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="b33c1-119">Create the virtual network.</span></span> <span data-ttu-id="b33c1-120">Det här exemplet skapar ett virtuellt nätverk med namnet **myVnet** med adressprefixet för **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-120">This example creates a virtual network named **myVnet** with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="b33c1-121">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="b33c1-121">Create a public IP address and network interface</span></span>

<span data-ttu-id="b33c1-122">För att upprätta kommunikation med den virtuella datorn i det virtuella nätverket behöver du en [offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="b33c1-122">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="b33c1-123">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b33c1-123">Create a public IP address.</span></span> <span data-ttu-id="b33c1-124">Det här exemplet skapas en offentlig IP-adress med namnet **myPip**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-124">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="b33c1-125">Skapa nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="b33c1-125">Create the NIC.</span></span> <span data-ttu-id="b33c1-126">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-126">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="b33c1-127">Skapa säkerhetsgrupp för nätverk och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="b33c1-127">Create the network security group and an RDP rule</span></span>

<span data-ttu-id="b33c1-128">Du måste ha en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389 för att kunna logga in på den virtuella datorn med RDP.</span><span class="sxs-lookup"><span data-stu-id="b33c1-128">To be able to log in to your VM using RDP, you need to have a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="b33c1-129">Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="b33c1-129">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="b33c1-130">Mer information om NSG: er finns [öppna portar till en virtuell dator i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b33c1-130">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

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


## <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="b33c1-131">Skapa en variabel för det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="b33c1-131">Create a variable for the virtual network</span></span>

<span data-ttu-id="b33c1-132">Skapa en variabel för det virtuella nätverket som slutförda.</span><span class="sxs-lookup"><span data-stu-id="b33c1-132">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a><span data-ttu-id="b33c1-133">Hämta autentiseringsuppgifterna för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b33c1-133">Get the credentials for the VM</span></span>

<span data-ttu-id="b33c1-134">Följande cmdlet öppnas ett fönster där du ska ange ett nytt användarnamn och lösenord ska användas som ett lokalt administratörskonto för att få fjärråtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b33c1-134">The following cmdlet will open a window where you will enter a new user name and password to use as the local administrator account for remotely accessing the VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a><span data-ttu-id="b33c1-135">Ange variabler för VM-namn, datornamn och storleken på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b33c1-135">Set variables for the VM name, computer name and the size of the VM</span></span>

1. <span data-ttu-id="b33c1-136">Skapa variabler för namn på virtuell dator och datornamn.</span><span class="sxs-lookup"><span data-stu-id="b33c1-136">Create variables for the VM name and computer name.</span></span> <span data-ttu-id="b33c1-137">Det här exemplet anges på det Virtuella datornamnet som **myVM** och datornamn som **den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-137">This example sets the VM name as **myVM** and the computer name as **myComputer**.</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. <span data-ttu-id="b33c1-138">Ange storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b33c1-138">Set the size of the virtual machine.</span></span> <span data-ttu-id="b33c1-139">Det här exemplet skapar **Standard_DS1_v2** storlek VM.</span><span class="sxs-lookup"><span data-stu-id="b33c1-139">This example creates **Standard_DS1_v2** sized VM.</span></span> <span data-ttu-id="b33c1-140">Finns det [VM-storlekar](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="b33c1-140">See the [VM sizes](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentation for more information.</span></span>

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. <span data-ttu-id="b33c1-141">Lägg till VM-namnet och storleken i VM-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b33c1-141">Add the VM name and size to the VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a><span data-ttu-id="b33c1-142">Ange VM-avbildning som källbilden för den nya virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b33c1-142">Set the VM image as source image for the new VM</span></span>

<span data-ttu-id="b33c1-143">Ange källa avbildningen med ID: T för den hanterade VM-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b33c1-143">Set the source image using the ID of the managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a><span data-ttu-id="b33c1-144">Ange Operativsystemets konfiguration och Lägg till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="b33c1-144">Set the OS configuration and add the NIC.</span></span>

<span data-ttu-id="b33c1-145">Ange lagringstypen av (PremiumLRS eller StandardLRS) och storleken på operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="b33c1-145">Enter the storage type (PremiumLRS or StandardLRS) and the size of the OS disk.</span></span> <span data-ttu-id="b33c1-146">Det här exemplet anges kontotypen **PremiumLRS**, diskens storlek till **128 GB** och disk caching **ReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="b33c1-146">This example sets the account type to **PremiumLRS**, the disk size to **128 GB** and disk caching to **ReadWrite**.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a><span data-ttu-id="b33c1-147">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b33c1-147">Create the VM</span></span>

<span data-ttu-id="b33c1-148">Skapa den nya virtuella datorn med hjälp av den konfiguration som vi har skapat och lagras i den **$vm** variabeln.</span><span class="sxs-lookup"><span data-stu-id="b33c1-148">Create the new Vm using the configuration that we have built and stored in the **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="b33c1-149">Kontrollera att den virtuella datorn har skapats</span><span class="sxs-lookup"><span data-stu-id="b33c1-149">Verify that the VM was created</span></span>
<span data-ttu-id="b33c1-150">När du är klar bör du se den nya virtuella datorn i den [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="b33c1-150">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="b33c1-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b33c1-151">Next steps</span></span>
<span data-ttu-id="b33c1-152">För att hantera din nya virtuella datorn med Azure PowerShell, se [skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b33c1-152">To manage your new virtual machine with Azure PowerShell, see [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

