---
title: "aaaCreate och hantera virtuella Windows-datorer i Azure som använder flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate och hantera en virtuell Windows-dator som har flera nätverkskort anslutna tooit med hjälp av Azure PowerShell eller Resource Manager-mallar."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a><span data-ttu-id="3697c-103">Skapa och hantera en virtuell Windows-dator som har flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="3697c-103">Create and manage a Windows virtual machine that has multiple NICs</span></span>
<span data-ttu-id="3697c-104">Virtuella datorer (VM) i Azure kan ha flera virtuella nätverk gränssnittet kort (NIC) som är anslutna toothem.</span><span class="sxs-lookup"><span data-stu-id="3697c-104">Virtual machines (VMs) in Azure can have multiple virtual network interface cards (NICs) attached toothem.</span></span> <span data-ttu-id="3697c-105">Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="3697c-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="3697c-106">Den här artikeln beskrivs hur toocreate en virtuell dator som har flera nätverkskort anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="3697c-106">This article details how toocreate a VM that has multiple NICs attached tooit.</span></span> <span data-ttu-id="3697c-107">Du också lära dig hur tooadd eller ta bort nätverkskort från en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3697c-107">You also learn how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="3697c-108">Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3697c-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="3697c-109">Detaljerad information, inklusive hur toocreate flera nätverkskort i en egen PowerShell-skript, se [distribuera flera NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3697c-109">For detailed information, including how toocreate multiple NICs within your own PowerShell scripts, see [deploying multiple-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3697c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3697c-110">Prerequisites</span></span>
<span data-ttu-id="3697c-111">Se till att du har hello [senaste Azure PowerShell-versionen installerad och konfigurerad](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3697c-111">Make sure that you have hello [latest Azure PowerShell version installed and configured](/powershell/azure/overview).</span></span>

<span data-ttu-id="3697c-112">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="3697c-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="3697c-113">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="3697c-113">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>


## <a name="create-a-vm-with-multiple-nics"></a><span data-ttu-id="3697c-114">Skapa en virtuell dator med flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="3697c-114">Create a VM with multiple NICs</span></span>
<span data-ttu-id="3697c-115">Först skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3697c-115">First, create a resource group.</span></span> <span data-ttu-id="3697c-116">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *EastUs* plats:</span><span class="sxs-lookup"><span data-stu-id="3697c-116">hello following example creates a resource group named *myResourceGroup* in hello *EastUs* location:</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a><span data-ttu-id="3697c-117">Skapa virtuella nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="3697c-117">Create virtual network and subnets</span></span>
<span data-ttu-id="3697c-118">Ett vanligt scenario är för ett virtuellt nätverk toohave två eller flera undernät.</span><span class="sxs-lookup"><span data-stu-id="3697c-118">A common scenario is for a virtual network toohave two or more subnets.</span></span> <span data-ttu-id="3697c-119">Ett undernät kan vara för frontend-trafik, hello andra för backend-trafik.</span><span class="sxs-lookup"><span data-stu-id="3697c-119">One subnet may be for front-end traffic, hello other for back-end traffic.</span></span> <span data-ttu-id="3697c-120">tooconnect tooboth undernät kan du använda flera nätverkskort på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3697c-120">tooconnect tooboth subnets, you then use multiple NICs on your VM.</span></span>

1. <span data-ttu-id="3697c-121">Definiera två undernät för virtuellt nätverk med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="3697c-121">Define two virtual network subnets with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="3697c-122">hello följande exempel definierar hello undernät för *mySubnetFrontEnd* och *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="3697c-122">hello following example defines hello subnets for *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. <span data-ttu-id="3697c-123">Skapa virtuella nätverk och undernät med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="3697c-123">Create your virtual network and subnets with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="3697c-124">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet*:</span><span class="sxs-lookup"><span data-stu-id="3697c-124">hello following example creates a virtual network named *myVnet*:</span></span>

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a><span data-ttu-id="3697c-125">Skapa flera nätverkskort</span><span class="sxs-lookup"><span data-stu-id="3697c-125">Create multiple NICs</span></span>
<span data-ttu-id="3697c-126">Skapa två nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="3697c-126">Create two NICs with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="3697c-127">Koppla en NIC toohello frontend undernät och ett nätverkskort toohello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="3697c-127">Attach one NIC toohello front-end subnet and one NIC toohello back-end subnet.</span></span> <span data-ttu-id="3697c-128">hello följande exempel skapar nätverkskort med namnet *myNic1* och *myNic2*:</span><span class="sxs-lookup"><span data-stu-id="3697c-128">hello following example creates NICs named *myNic1* and *myNic2*:</span></span>

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

<span data-ttu-id="3697c-129">Vanligtvis du också skapa en [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) eller [belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toohelp hantera och distribuera trafik mellan dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3697c-129">Typically you also create a [network security group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="3697c-130">Hej [mer detaljerad flera NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikeln visar hur du skapar en nätverkssäkerhetsgrupp och tilldelar nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="3697c-130">hello [more detailed multiple-NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) article guides you through creating a network security group and assigning NICs.</span></span>

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="3697c-131">Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3697c-131">Create hello virtual machine</span></span>
<span data-ttu-id="3697c-132">Nu vill starta toobuild VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3697c-132">Now start toobuild your VM configuration.</span></span> <span data-ttu-id="3697c-133">Varje VM-storlek har en gräns för hello Totalt antal nätverkskort som du lägger till tooa VM.</span><span class="sxs-lookup"><span data-stu-id="3697c-133">Each VM size has a limit for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="3697c-134">Mer information finns i [Windows VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="3697c-134">For more information, see [Windows VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="3697c-135">Ange dina autentiseringsuppgifter för VM-toohello `$cred` variabeln på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3697c-135">Set your VM credentials toohello `$cred` variable as follows:</span></span>

    ```powershell
    $cred = Get-Credential
    ```

2. <span data-ttu-id="3697c-136">Definiera den virtuella datorn med [nya AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span><span class="sxs-lookup"><span data-stu-id="3697c-136">Define your VM with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span></span> <span data-ttu-id="3697c-137">hello följande exempel definieras en virtuell dator med namnet *myVM* och använder en VM-storlek som har stöd för fler än två nätverkskort (*Standard_DS3_v2*):</span><span class="sxs-lookup"><span data-stu-id="3697c-137">hello following example defines a VM named *myVM* and uses a VM size that supports more than two NICs (*Standard_DS3_v2*):</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. <span data-ttu-id="3697c-138">Skapa hello resten av VM-konfiguration med [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) och [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span><span class="sxs-lookup"><span data-stu-id="3697c-138">Create hello rest of your VM configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) and [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span></span> <span data-ttu-id="3697c-139">hello följande exempel skapar en Windows Server 2016 VM:</span><span class="sxs-lookup"><span data-stu-id="3697c-139">hello following example creates a Windows Server 2016 VM:</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. <span data-ttu-id="3697c-140">Koppla hello två nätverkskort som du skapade tidigare med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="3697c-140">Attach hello two NICs that you previously created with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. <span data-ttu-id="3697c-141">Skapa slutligen den virtuella datorn med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="3697c-141">Finally, create your VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a><span data-ttu-id="3697c-142">Lägg till ett NIC-tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="3697c-142">Add a NIC tooan existing VM</span></span>
<span data-ttu-id="3697c-143">tooadd som en virtuell NIC-tooan befintliga VM frigöra hello VM, lägga till hello virtuella nätverkskort, och starta sedan hello VM.</span><span class="sxs-lookup"><span data-stu-id="3697c-143">tooadd a virtual NIC tooan existing VM, you deallocate hello VM, add hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="3697c-144">Frigöra hello virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3697c-144">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="3697c-145">hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3697c-145">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="3697c-146">Hämta hello befintlig serverkonfiguration hello VM med [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3697c-146">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="3697c-147">hello följande exempel hämtar information för hello virtuella datorn med namnet *myVM* i *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3697c-147">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="3697c-148">hello följande exempel skapas ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) med namnet *myNic3* som är kopplad för*mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="3697c-148">hello following example creates a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) named *myNic3* that is attached too*mySubnetBackEnd*.</span></span> <span data-ttu-id="3697c-149">hello virtuellt nätverkskort är ansluten toohello virtuella datorn med namnet *myVM* i *myResourceGroup* med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="3697c-149">hello virtual NIC is then attached toohello VM named *myVM* in *myResourceGroup* with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a><span data-ttu-id="3697c-150">Primära virtuella nätverkskort</span><span class="sxs-lookup"><span data-stu-id="3697c-150">Primary virtual NICs</span></span>
    <span data-ttu-id="3697c-151">En av hello nätverkskort på en VM med flera nätverkskort måste toobe primära.</span><span class="sxs-lookup"><span data-stu-id="3697c-151">One of hello NICs on a multi-NIC VM needs toobe primary.</span></span> <span data-ttu-id="3697c-152">Om en av hello befintliga virtuella nätverkskort på hello VM har redan angetts som primär, du kan hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="3697c-152">If one of hello existing virtual NICs on hello VM is already set as primary, you can skip this step.</span></span> <span data-ttu-id="3697c-153">hello följande exempel förutsätter att det finns nu två virtuella nätverkskort på en virtuell dator och du vill tooadd hello första NIC (`[0]`) som primär hello:</span><span class="sxs-lookup"><span data-stu-id="3697c-153">hello following example assumes that two virtual NICs are now present on a VM and you wish tooadd hello first NIC (`[0]`) as hello primary:</span></span>
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. <span data-ttu-id="3697c-154">Starta virtuell dator med hello [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="3697c-154">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a><span data-ttu-id="3697c-155">Ta bort ett nätverkskort från en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3697c-155">Remove a NIC from an existing VM</span></span>
<span data-ttu-id="3697c-156">tooremove ett virtuellt nätverkskort från en befintlig virtuell dator frigöra hello VM, ta bort hello virtuella nätverkskortet och sedan starta hello VM.</span><span class="sxs-lookup"><span data-stu-id="3697c-156">tooremove a virtual NIC from an existing VM, you deallocate hello VM, remove hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="3697c-157">Frigöra hello virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3697c-157">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="3697c-158">hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3697c-158">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="3697c-159">Hämta hello befintlig serverkonfiguration hello VM med [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3697c-159">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="3697c-160">hello följande exempel hämtar information för hello virtuella datorn med namnet *myVM* i *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3697c-160">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="3697c-161">Hämta information om hello NIC ta bort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="3697c-161">Get information about hello NIC remove with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span></span> <span data-ttu-id="3697c-162">hello följande exempel hämtar information *myNic3*:</span><span class="sxs-lookup"><span data-stu-id="3697c-162">hello following example gets information about *myNic3*:</span></span>

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. <span data-ttu-id="3697c-163">Ta bort hello nätverkskortet med [ta bort AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) och sedan uppdatera hello virtuell dator med [uppdatering AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3697c-163">Remove hello NIC with [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) and then update hello VM with [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span></span> <span data-ttu-id="3697c-164">hello följande exempel tar bort *myNic3* som erhålls av `$nicId` i hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="3697c-164">hello following example removes *myNic3* as obtained by `$nicId` in hello preceding step:</span></span>

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. <span data-ttu-id="3697c-165">Starta virtuell dator med hello [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="3697c-165">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a><span data-ttu-id="3697c-166">Skapa flera nätverkskort med mallar</span><span class="sxs-lookup"><span data-stu-id="3697c-166">Create multiple NICs with templates</span></span>
<span data-ttu-id="3697c-167">Azure Resource Manager-mallar ger ett sätt toocreate flera instanser av en resurs under distributionen, till exempel skapa flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="3697c-167">Azure Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="3697c-168">Resource Manager-mallar använda deklarativa JSON filer toodefine din miljö.</span><span class="sxs-lookup"><span data-stu-id="3697c-168">Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="3697c-169">Mer information finns i [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3697c-169">For more information, see [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="3697c-170">Du kan använda *kopiera* toospecify hello antal instanser toocreate:</span><span class="sxs-lookup"><span data-stu-id="3697c-170">You can use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="3697c-171">Mer information finns i [skapa flera instanser med *kopiera*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="3697c-171">For more information, see [creating multiple instances by using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="3697c-172">Du kan också använda `copyIndex()` tooappend ett antal tooa resursnamn.</span><span class="sxs-lookup"><span data-stu-id="3697c-172">You can also use `copyIndex()` tooappend a number tooa resource name.</span></span> <span data-ttu-id="3697c-173">Du kan sedan skapa *myNic1*, *MyNic2* och så vidare.</span><span class="sxs-lookup"><span data-stu-id="3697c-173">You can then create *myNic1*, *MyNic2* and so on.</span></span> <span data-ttu-id="3697c-174">hello visar följande kod ett exempel på att lägga till hello indexvärde:</span><span class="sxs-lookup"><span data-stu-id="3697c-174">hello following code shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="3697c-175">Du kan läsa en komplett exempel på [skapa flera nätverkskort med Resource Manager-mallar](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="3697c-175">You can read a complete example of [creating multiple NICs by using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3697c-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3697c-176">Next steps</span></span>
<span data-ttu-id="3697c-177">Granska [Windows VM-storlekar](sizes.md) när du försöker toocreate en virtuell dator som har flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="3697c-177">Review [Windows VM sizes](sizes.md) when you're trying toocreate a VM that has multiple NICs.</span></span> <span data-ttu-id="3697c-178">Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="3697c-178">Pay attention toohello maximum number of NICs that each VM size supports.</span></span> 


