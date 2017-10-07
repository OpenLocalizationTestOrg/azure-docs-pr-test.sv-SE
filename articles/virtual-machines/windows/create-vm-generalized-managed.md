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
# <a name="create-a-vm-from-a-managed-image"></a><span data-ttu-id="75c9a-103">Skapa en virtuell dator från en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="75c9a-103">Create a VM from a managed image</span></span>

<span data-ttu-id="75c9a-104">Du kan skapa flera virtuella datorer från en hanterad VM-avbildning i Azure.</span><span class="sxs-lookup"><span data-stu-id="75c9a-104">You can create multiple VMs from a managed VM image in Azure.</span></span> <span data-ttu-id="75c9a-105">En hanterad VM-avbildning innehåller hello information behövs toocreate en virtuell dator, inklusive hello Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="75c9a-105">A managed VM image contains hello information necessary toocreate a VM, including hello OS and data disks.</span></span> <span data-ttu-id="75c9a-106">hello virtuella hårddiskar som utgör hello-avbildning, inklusive både hello OS-diskar och eventuella hårddiskar, lagras som hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="75c9a-106">hello VHDs that make up hello image, including both hello OS disks and any data disks, are stored as managed disks.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="75c9a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="75c9a-107">Prerequisites</span></span>

<span data-ttu-id="75c9a-108">Du behöver toohave redan [skapas en hanterade VM-avbildning](capture-image-resource.md) toouse för att skapa hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="75c9a-108">You need toohave already [created a managed VM image](capture-image-resource.md) toouse for creating hello new VM.</span></span> 

<span data-ttu-id="75c9a-109">Kontrollera att du har hello senaste versionerna av hello AzureRM.Compute och AzureRM.Network PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="75c9a-109">Make sure that you have hello latest versions of hello AzureRM.Compute and AzureRM.Network PowerShell modules.</span></span> <span data-ttu-id="75c9a-110">Öppna ett PowerShell-Kommandotolken som administratör och kör följande kommando tooinstall hello dem.</span><span class="sxs-lookup"><span data-stu-id="75c9a-110">Open a PowerShell prompt as an Administrator and run hello following command tooinstall them.</span></span>

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
<span data-ttu-id="75c9a-111">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75c9a-111">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>



## <a name="collect-information-about-hello-image"></a><span data-ttu-id="75c9a-112">Samla in information om hello avbildning</span><span class="sxs-lookup"><span data-stu-id="75c9a-112">Collect information about hello image</span></span>

<span data-ttu-id="75c9a-113">Först vi behöver toogather grundläggande information om hello avbildningen och skapa en variabel för hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="75c9a-113">First we need toogather basic information about hello image and create a variable for hello image.</span></span> <span data-ttu-id="75c9a-114">Det här exemplet används en hanterad VM-avbildning med namnet **myImage** som är i hello **myResourceGroup** resursgrupp i hello **Väst centrala oss** plats.</span><span class="sxs-lookup"><span data-stu-id="75c9a-114">This example uses a managed VM image named **myImage** that is in hello **myResourceGroup** resource group in hello **West Central US** location.</span></span> 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="75c9a-115">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="75c9a-115">Create a virtual network</span></span>
<span data-ttu-id="75c9a-116">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="75c9a-116">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="75c9a-117">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="75c9a-117">Create hello subnet.</span></span> <span data-ttu-id="75c9a-118">Det här exemplet skapar ett undernät med namnet **mySubnet** med hello adressprefixet **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-118">This example creates a subnet named **mySubnet** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="75c9a-119">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="75c9a-119">Create hello virtual network.</span></span> <span data-ttu-id="75c9a-120">Det här exemplet skapar ett virtuellt nätverk med namnet **myVnet** med hello adressprefixet **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-120">This example creates a virtual network named **myVnet** with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="75c9a-121">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="75c9a-121">Create a public IP address and network interface</span></span>

<span data-ttu-id="75c9a-122">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="75c9a-122">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="75c9a-123">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="75c9a-123">Create a public IP address.</span></span> <span data-ttu-id="75c9a-124">Det här exemplet skapas en offentlig IP-adress med namnet **myPip**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-124">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="75c9a-125">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="75c9a-125">Create hello NIC.</span></span> <span data-ttu-id="75c9a-126">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-126">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="75c9a-127">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="75c9a-127">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="75c9a-128">toobe kan toolog i tooyour VM som använder RDP måste toohave en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="75c9a-128">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="75c9a-129">Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="75c9a-129">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="75c9a-130">Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="75c9a-130">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

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


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="75c9a-131">Skapa en variabel för hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="75c9a-131">Create a variable for hello virtual network</span></span>

<span data-ttu-id="75c9a-132">Skapa en variabel för hello slutförts virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="75c9a-132">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="75c9a-133">Hämta hello autentiseringsuppgifter för hello VM</span><span class="sxs-lookup"><span data-stu-id="75c9a-133">Get hello credentials for hello VM</span></span>

<span data-ttu-id="75c9a-134">hello öppnar följande cmdlet ett fönster där anger du en ny användare och lösenord toouse som hello lokalt administratörskonto för att få fjärråtkomst till hello VM.</span><span class="sxs-lookup"><span data-stu-id="75c9a-134">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a><span data-ttu-id="75c9a-135">Ange variabler för hello VM namn, datornamn och hello storleken på hello VM</span><span class="sxs-lookup"><span data-stu-id="75c9a-135">Set variables for hello VM name, computer name and hello size of hello VM</span></span>

1. <span data-ttu-id="75c9a-136">Skapa variabler för hello VM namn och datornamn.</span><span class="sxs-lookup"><span data-stu-id="75c9a-136">Create variables for hello VM name and computer name.</span></span> <span data-ttu-id="75c9a-137">Det här exemplet anger hello VM-namn som **myVM** och hello datornamn som **den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-137">This example sets hello VM name as **myVM** and hello computer name as **myComputer**.</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. <span data-ttu-id="75c9a-138">Ange hello storleken på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="75c9a-138">Set hello size of hello virtual machine.</span></span> <span data-ttu-id="75c9a-139">Det här exemplet skapar **Standard_DS1_v2** storlek VM.</span><span class="sxs-lookup"><span data-stu-id="75c9a-139">This example creates **Standard_DS1_v2** sized VM.</span></span> <span data-ttu-id="75c9a-140">Se hello [VM-storlekar](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="75c9a-140">See hello [VM sizes](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentation for more information.</span></span>

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. <span data-ttu-id="75c9a-141">Lägg till hello namn på virtuell dator och storlek toohello VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="75c9a-141">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="75c9a-142">Ange hello VM-avbildning som källbilden för hello ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="75c9a-142">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="75c9a-143">Ange hello Källavbildningen med hello-ID för hello hanterade VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="75c9a-143">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="75c9a-144">Ange hello Operativsystemets konfiguration och Lägg till hello NIC.</span><span class="sxs-lookup"><span data-stu-id="75c9a-144">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="75c9a-145">Ange hello lagringstyp (PremiumLRS eller StandardLRS) och hello hello OS-diskens storlek.</span><span class="sxs-lookup"><span data-stu-id="75c9a-145">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="75c9a-146">Det här exemplet anger hello kontotyp för**PremiumLRS**, hello diskstorleken för**128 GB** och diskcachelagring för**ReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="75c9a-146">This example sets hello account type too**PremiumLRS**, hello disk size too**128 GB** and disk caching too**ReadWrite**.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="75c9a-147">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="75c9a-147">Create hello VM</span></span>

<span data-ttu-id="75c9a-148">Skapa ny virtuell dator med hello-konfiguration som vi har skapat och lagras i hello hello **$vm** variabeln.</span><span class="sxs-lookup"><span data-stu-id="75c9a-148">Create hello new Vm using hello configuration that we have built and stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="75c9a-149">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="75c9a-149">Verify that hello VM was created</span></span>
<span data-ttu-id="75c9a-150">När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="75c9a-150">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="75c9a-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75c9a-151">Next steps</span></span>
<span data-ttu-id="75c9a-152">din nya virtuella datorn med Azure PowerShell finns i toomanage [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="75c9a-152">toomanage your new virtual machine with Azure PowerShell, see [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

