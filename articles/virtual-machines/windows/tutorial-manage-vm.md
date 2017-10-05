---
title: Skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen | Microsoft Docs
description: "Självstudiekurs – skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen"
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
ms.openlocfilehash: ec1bb7834beb66dc28dd5b1db764bd358243292c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a><span data-ttu-id="24b20-103">Skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="24b20-103">Create and Manage Windows VMs with the Azure PowerShell module</span></span>

<span data-ttu-id="24b20-104">Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö.</span><span class="sxs-lookup"><span data-stu-id="24b20-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="24b20-105">Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="24b20-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="24b20-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24b20-107">Skapa och ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-107">Create and connect to a VM</span></span>
> * <span data-ttu-id="24b20-108">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="24b20-108">Select and use VM images</span></span>
> * <span data-ttu-id="24b20-109">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="24b20-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="24b20-110">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-110">Resize a VM</span></span>
> * <span data-ttu-id="24b20-111">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-111">View and understand VM state</span></span>

<span data-ttu-id="24b20-112">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="24b20-112">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="24b20-113">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="24b20-113">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="24b20-114">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="24b20-114">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="24b20-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="24b20-115">Create resource group</span></span>

<span data-ttu-id="24b20-116">Skapa en resursgrupp med kommandot [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="24b20-116">Create a resource group with the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> 

<span data-ttu-id="24b20-117">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="24b20-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="24b20-118">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="24b20-119">I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i den *EastUS* region.</span><span class="sxs-lookup"><span data-stu-id="24b20-119">In this example, a resource group named *myResourceGroupVM* is created in the *EastUS* region.</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

<span data-ttu-id="24b20-120">Resursgruppen har angetts när du skapar eller ändrar en VM som kan ses i hela den här kursen.</span><span class="sxs-lookup"><span data-stu-id="24b20-120">The resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="24b20-121">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-121">Create virtual machine</span></span>

<span data-ttu-id="24b20-122">En virtuell dator måste vara ansluten till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="24b20-122">A virtual machine must be connected to a virtual network.</span></span> <span data-ttu-id="24b20-123">Du kan kommunicera med den virtuella datorn med en offentlig IP-adress via ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="24b20-123">You communicate with the virtual machine using a public IP address through a network interface card.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="24b20-124">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="24b20-124">Create virtual network</span></span>

<span data-ttu-id="24b20-125">Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="24b20-125">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="24b20-126">Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="24b20-126">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a><span data-ttu-id="24b20-127">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="24b20-127">Create public IP address</span></span>

<span data-ttu-id="24b20-128">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="24b20-128">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a><span data-ttu-id="24b20-129">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="24b20-129">Create network interface card</span></span>

<span data-ttu-id="24b20-130">Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="24b20-130">Create a network interface card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a><span data-ttu-id="24b20-131">Skapa nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="24b20-131">Create network security group</span></span>

<span data-ttu-id="24b20-132">En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="24b20-132">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="24b20-133">Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="24b20-133">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="24b20-134">De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-134">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span> <span data-ttu-id="24b20-135">För att komma åt IIS-webbserver som du installerar måste du lägga till en regel för inkommande NSG.</span><span class="sxs-lookup"><span data-stu-id="24b20-135">To access the IIS webserver that you are installing, you must add an inbound NSG rule.</span></span>

<span data-ttu-id="24b20-136">Så här skapar du en regel för inkommande NSG [Lägg till AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="24b20-136">To create an inbound NSG rule, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="24b20-137">I följande exempel skapas en NSG regeln med namnet *myNSGRule* som öppnar port *3389* för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="24b20-137">The following example creates an NSG rule named *myNSGRule* that opens port *3389* for the virtual machine:</span></span>

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

<span data-ttu-id="24b20-138">Skapa en NSG med hjälp av *myNSGRule* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="24b20-138">Create the NSG using *myNSGRule* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

<span data-ttu-id="24b20-139">Lägg till NSG: N till undernät i det virtuella nätverket med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="24b20-139">Add the NSG to the subnet in the virtual network with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="24b20-140">Uppdatera det virtuella nätverket med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="24b20-140">Update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a><span data-ttu-id="24b20-141">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-141">Create virtual machine</span></span>

<span data-ttu-id="24b20-142">När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken.</span><span class="sxs-lookup"><span data-stu-id="24b20-142">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="24b20-143">I det här exemplet skapas en virtuell dator med namnet *myVM* kör den senaste versionen av Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="24b20-143">In this example, a virtual machine is created with a name of *myVM* running the latest version of Windows Server 2016 Datacenter.</span></span>

<span data-ttu-id="24b20-144">Ange användarnamn och lösenord för administratörskontot på den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="24b20-144">Set the username and password needed for the administrator account on the virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="24b20-145">Skapa den första konfigurationen för den virtuella datorn med [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span><span class="sxs-lookup"><span data-stu-id="24b20-145">Create the initial configuration for the virtual machine with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

<span data-ttu-id="24b20-146">Lägg till information om operativsystemet i konfigurationen av virtuella datorn med [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span><span class="sxs-lookup"><span data-stu-id="24b20-146">Add the operating system information to the virtual machine configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span></span>

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="24b20-147">Lägga till bilden i konfigurationen av virtuella datorn med [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span><span class="sxs-lookup"><span data-stu-id="24b20-147">Add the image information to the virtual machine configuration with [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

<span data-ttu-id="24b20-148">Lägg till disk operativsysteminställningar i konfigurationen av virtuella datorn med [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span><span class="sxs-lookup"><span data-stu-id="24b20-148">Add the operating system disk settings to the virtual machine configuration with [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

<span data-ttu-id="24b20-149">Lägg till nätverkskortet som du skapade tidigare till konfigurationen av virtuella datorn med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="24b20-149">Add the network interface card that you previously created to the virtual machine configuration with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="24b20-150">Skapa den virtuella datorn med [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="24b20-150">Create the virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a><span data-ttu-id="24b20-151">Ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-151">Connect to VM</span></span>

<span data-ttu-id="24b20-152">När distributionen har slutförts kan du skapa en fjärrskrivbordsanslutning med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-152">After the deployment has completed, create a remote desktop connection with the virtual machine.</span></span>

<span data-ttu-id="24b20-153">Kör följande kommandon för att returnera den offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-153">Run the following commands to return the public IP address of the virtual machine.</span></span> <span data-ttu-id="24b20-154">Anteckna denna IP-adress så att du kan ansluta till den till din webbläsare för att testa webbanslutningen i ett framtida steg.</span><span class="sxs-lookup"><span data-stu-id="24b20-154">Take note of this IP Address so you can connect to it with your browser to test web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

<span data-ttu-id="24b20-155">Använd följande kommando för att skapa en fjärrskrivbordssession med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-155">Use the following command to create a remote desktop session with the virtual machine.</span></span> <span data-ttu-id="24b20-156">Ersätt IP-adressen med *publicIPAddress* för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-156">Replace the IP address with the *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="24b20-157">När du uppmanas att göra det anger du de autentiseringsuppgifter som användes när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-157">When prompted, enter the credentials used when creating the virtual machine.</span></span>

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a><span data-ttu-id="24b20-158">Förstå VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="24b20-158">Understand VM images</span></span>

<span data-ttu-id="24b20-159">Azure marketplace innehåller många avbildningar av virtuella datorer som kan användas för att skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-159">The Azure marketplace includes many virtual machine images that can be used to create a new virtual machine.</span></span> <span data-ttu-id="24b20-160">I de föregående stegen skapades en virtuell dator med Windows Server 2016-Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="24b20-160">In the previous steps, a virtual machine was created using the Windows Server 2016-Datacenter image.</span></span> <span data-ttu-id="24b20-161">I det här steget används i PowerShell-modulen för att söka på marketplace för andra Windows-avbildningar kan också som bas för nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="24b20-161">In this step, the PowerShell module is used to search the marketplace for other Windows images, which can also as a base for new VMs.</span></span> <span data-ttu-id="24b20-162">Den här processen består av att söka efter utgivare, erbjudande och Avbildningsnamnet (Sku).</span><span class="sxs-lookup"><span data-stu-id="24b20-162">This process consists of finding the publisher, offer, and the image name (Sku).</span></span> 

<span data-ttu-id="24b20-163">Använd den [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) kommando för att returnera en lista över avbildningen utgivare.</span><span class="sxs-lookup"><span data-stu-id="24b20-163">Use the [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) command to return a list of image publishers.</span></span>  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

<span data-ttu-id="24b20-164">Använd den [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) att returnera en lista med bild erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="24b20-164">Use the [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) to return a list of image offers.</span></span> <span data-ttu-id="24b20-165">Den returnerade listan filtreras på den angivna utgivaren med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="24b20-165">With this command, the returned list is filtered on the specified publisher.</span></span> 

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

<span data-ttu-id="24b20-166">Den [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) kommandot filtrerar sedan på namnet på utgivaren och erbjudandet att returnera en lista över namn på bilder.</span><span class="sxs-lookup"><span data-stu-id="24b20-166">The [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) command will then filter on the publisher and offer name to return a list of image names.</span></span>

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

<span data-ttu-id="24b20-167">Den här informationen kan användas för att distribuera en virtuell dator med en viss bild.</span><span class="sxs-lookup"><span data-stu-id="24b20-167">This information can be used to deploy a VM with a specific image.</span></span> <span data-ttu-id="24b20-168">Det här exemplet anger avbildningens namn på det Virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="24b20-168">This example sets the image name on the VM object.</span></span> <span data-ttu-id="24b20-169">I föregående exempel finns i den här självstudiekursen fullständig distributionssteg.</span><span class="sxs-lookup"><span data-stu-id="24b20-169">Refer to the previous examples in this tutorial for complete deployment steps.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="24b20-170">Förstå VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="24b20-170">Understand VM sizes</span></span>

<span data-ttu-id="24b20-171">Storlek på en virtuell dator bestämmer hur mycket av beräkningsresurser som Processorn och GPU-minne som är tillgängliga för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="24b20-171">A virtual machine size determines the amount of compute resources such as CPU, GPU, and memory that are made available to the virtual machine.</span></span> <span data-ttu-id="24b20-172">Virtuella datorer måste skapas med lämplig storlek för förväntat arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="24b20-172">Virtual machines need to be created with a size appropriate for the expect work load.</span></span> <span data-ttu-id="24b20-173">Om belastningen ökar, kan en befintlig virtuell dator ändras.</span><span class="sxs-lookup"><span data-stu-id="24b20-173">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="24b20-174">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="24b20-174">VM Sizes</span></span>

<span data-ttu-id="24b20-175">I följande tabell kategoriserar storlekar i användningsfall.</span><span class="sxs-lookup"><span data-stu-id="24b20-175">The following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="24b20-176">Typ</span><span class="sxs-lookup"><span data-stu-id="24b20-176">Type</span></span>                     | <span data-ttu-id="24b20-177">Storlekar</span><span class="sxs-lookup"><span data-stu-id="24b20-177">Sizes</span></span>           |    <span data-ttu-id="24b20-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="24b20-178">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="24b20-179">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="24b20-179">General purpose</span></span>         |<span data-ttu-id="24b20-180">DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="24b20-180">DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="24b20-181">Belastningsutjämnade CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="24b20-181">Balanced CPU-to-memory.</span></span> <span data-ttu-id="24b20-182">Idealiskt för dev / test och små till medelstora lösningar för program och data.</span><span class="sxs-lookup"><span data-stu-id="24b20-182">Ideal for dev / test and small to medium applications and data solutions.</span></span>  |
| <span data-ttu-id="24b20-183">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="24b20-183">Compute optimized</span></span>      | <span data-ttu-id="24b20-184">FS, F</span><span class="sxs-lookup"><span data-stu-id="24b20-184">Fs, F</span></span>             | <span data-ttu-id="24b20-185">Hög CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="24b20-185">High CPU-to-memory.</span></span> <span data-ttu-id="24b20-186">Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.</span><span class="sxs-lookup"><span data-stu-id="24b20-186">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| <span data-ttu-id="24b20-187">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="24b20-187">Memory optimized</span></span>       | <span data-ttu-id="24b20-188">GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="24b20-188">GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="24b20-189">Hög minne-till-core.</span><span class="sxs-lookup"><span data-stu-id="24b20-189">High memory-to-core.</span></span> <span data-ttu-id="24b20-190">Perfekt för relationsdatabaser, medelstora till stora cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="24b20-190">Great for relational databases, medium to large caches, and in-memory analytics.</span></span>                 |
| <span data-ttu-id="24b20-191">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="24b20-191">Storage optimized</span></span>       | <span data-ttu-id="24b20-192">Ls</span><span class="sxs-lookup"><span data-stu-id="24b20-192">Ls</span></span>                | <span data-ttu-id="24b20-193">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="24b20-193">High disk throughput and IO.</span></span> <span data-ttu-id="24b20-194">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="24b20-194">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| <span data-ttu-id="24b20-195">GPU</span><span class="sxs-lookup"><span data-stu-id="24b20-195">GPU</span></span>           | <span data-ttu-id="24b20-196">NV NC</span><span class="sxs-lookup"><span data-stu-id="24b20-196">NV, NC</span></span>            | <span data-ttu-id="24b20-197">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="24b20-197">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| <span data-ttu-id="24b20-198">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="24b20-198">High performance</span></span> | <span data-ttu-id="24b20-199">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="24b20-199">H, A8-11</span></span>          | <span data-ttu-id="24b20-200">Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA).</span><span class="sxs-lookup"><span data-stu-id="24b20-200">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="24b20-201">Hitta tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="24b20-201">Find available VM sizes</span></span>

<span data-ttu-id="24b20-202">Om du vill se en lista över storlekar på VM tillgängliga i en viss region, Använd den [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando.</span><span class="sxs-lookup"><span data-stu-id="24b20-202">To see a list of VM sizes available in a particular region, use the [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command.</span></span>

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a><span data-ttu-id="24b20-203">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-203">Resize a VM</span></span>

<span data-ttu-id="24b20-204">När en virtuell dator har distribuerats, kan den ändras för att öka eller minska resursallokering.</span><span class="sxs-lookup"><span data-stu-id="24b20-204">After a VM has been deployed, it can be resized to increase or decrease resource allocation.</span></span>

<span data-ttu-id="24b20-205">Kontrollera om önskad storlek är tillgängligt på den aktuella virtuella datorns klustret innan du ändrar storlek på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-205">Before resizing a VM, check if the desired size is available on the current VM cluster.</span></span> <span data-ttu-id="24b20-206">Den [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando returnerar en lista över storlekar.</span><span class="sxs-lookup"><span data-stu-id="24b20-206">The [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command returns a list of sizes.</span></span> 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

<span data-ttu-id="24b20-207">Om önskad storlek är tillgänglig, kan den virtuella datorn ändras från ett slås på tillstånd, men den startas under åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24b20-207">If the desired size is available, the VM can be resized from a powered-on state, however it is rebooted during the operation.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

<span data-ttu-id="24b20-208">Om önskad storlek inte är i det aktuella klustret måste den virtuella datorn frigörs innan åtgärden Ändra storlek kan ske.</span><span class="sxs-lookup"><span data-stu-id="24b20-208">If the desired size is not on the current cluster, the VM needs to be deallocated before the resize operation can occur.</span></span> <span data-ttu-id="24b20-209">Observera att när den virtuella datorn är påslagen tillbaka alla data på disken för temporär tas bort och den offentliga IP-ändra såvida inte en statisk IP-adress används.</span><span class="sxs-lookup"><span data-stu-id="24b20-209">Note, when the VM is powered back on, any data on the temp disk are removed, and the public IP address change unless a static IP address is being used.</span></span> 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a><span data-ttu-id="24b20-210">Energisparfunktioner för VM</span><span class="sxs-lookup"><span data-stu-id="24b20-210">VM power states</span></span>

<span data-ttu-id="24b20-211">En virtuell Azure-dator kan ha en av många energisparfunktioner.</span><span class="sxs-lookup"><span data-stu-id="24b20-211">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="24b20-212">Det här tillståndet representerar det aktuella tillståndet för den virtuella datorn för hypervisor-programmet.</span><span class="sxs-lookup"><span data-stu-id="24b20-212">This state represents the current state of the VM from the standpoint of the hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="24b20-213">Energisparfunktioner</span><span class="sxs-lookup"><span data-stu-id="24b20-213">Power states</span></span>

| <span data-ttu-id="24b20-214">Energisparläge</span><span class="sxs-lookup"><span data-stu-id="24b20-214">Power State</span></span> | <span data-ttu-id="24b20-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="24b20-215">Description</span></span>
|----|----|
| <span data-ttu-id="24b20-216">Startar</span><span class="sxs-lookup"><span data-stu-id="24b20-216">Starting</span></span> | <span data-ttu-id="24b20-217">Anger den virtuella datorn startas.</span><span class="sxs-lookup"><span data-stu-id="24b20-217">Indicates the virtual machine is being started.</span></span> |
| <span data-ttu-id="24b20-218">Körs</span><span class="sxs-lookup"><span data-stu-id="24b20-218">Running</span></span> | <span data-ttu-id="24b20-219">Anger att den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="24b20-219">Indicates that the virtual machine is running.</span></span> |
| <span data-ttu-id="24b20-220">Stoppas</span><span class="sxs-lookup"><span data-stu-id="24b20-220">Stopping</span></span> | <span data-ttu-id="24b20-221">Anger att den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="24b20-221">Indicates that the virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="24b20-222">Stoppad</span><span class="sxs-lookup"><span data-stu-id="24b20-222">Stopped</span></span> | <span data-ttu-id="24b20-223">Anger att den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="24b20-223">Indicates that the virtual machine is stopped.</span></span> <span data-ttu-id="24b20-224">Observera att virtuella datorer i ett stoppat tillstånd fortfarande avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="24b20-224">Note that virtual machines in the stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="24b20-225">Det har frigjorts</span><span class="sxs-lookup"><span data-stu-id="24b20-225">Deallocating</span></span> | <span data-ttu-id="24b20-226">Anger att den virtuella datorn har flyttats.</span><span class="sxs-lookup"><span data-stu-id="24b20-226">Indicates that the virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="24b20-227">Frigöra</span><span class="sxs-lookup"><span data-stu-id="24b20-227">Deallocated</span></span> | <span data-ttu-id="24b20-228">Anger att den virtuella datorn är helt tas bort från hypervisor-programmet men fortfarande tillgängliga i kontrollplan.</span><span class="sxs-lookup"><span data-stu-id="24b20-228">Indicates that the virtual machine is completely removed from the hypervisor but still available in the control plane.</span></span> <span data-ttu-id="24b20-229">Virtuella datorer med tillståndet Deallocated inte avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="24b20-229">Virtual machines in the Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="24b20-230">Anger att energisparläge för den virtuella datorn är okänt.</span><span class="sxs-lookup"><span data-stu-id="24b20-230">Indicates that the power state of the virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="24b20-231">Hitta energiläge</span><span class="sxs-lookup"><span data-stu-id="24b20-231">Find power state</span></span>

<span data-ttu-id="24b20-232">Använd för att hämta tillståndet för en viss virtuell dator i [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando.</span><span class="sxs-lookup"><span data-stu-id="24b20-232">To retrieve the state of a particular VM, use the [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span> <span data-ttu-id="24b20-233">Se till att ange ett giltigt namn för en virtuell dator och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="24b20-233">Be sure to specify a valid name for a virtual machine and resource group.</span></span> 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

<span data-ttu-id="24b20-234">Resultat:</span><span class="sxs-lookup"><span data-stu-id="24b20-234">Output:</span></span>

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a><span data-ttu-id="24b20-235">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="24b20-235">Management tasks</span></span>

<span data-ttu-id="24b20-236">Under livscykeln för en virtuell dator kan du vill köra hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24b20-236">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="24b20-237">Dessutom kanske du vill skapa skript för att automatisera repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="24b20-237">Additionally, you may want to create scripts to automate repetitive or complex tasks.</span></span> <span data-ttu-id="24b20-238">Med hjälp av Azure PowerShell, kan många vanliga administrativa uppgifter köras från kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="24b20-238">Using Azure PowerShell, many common management tasks can be run from the command line or in scripts.</span></span>

### <a name="stop-virtual-machine"></a><span data-ttu-id="24b20-239">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="24b20-239">Stop virtual machine</span></span>

<span data-ttu-id="24b20-240">Stoppa och ta bort en virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="24b20-240">Stop and deallocate a virtual machine with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

<span data-ttu-id="24b20-241">Använd parametern - StayProvisioned om du vill behålla den virtuella datorn i ett etablerat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="24b20-241">If you want to keep the virtual machine in a provisioned state, use the -StayProvisioned parameter.</span></span>

### <a name="start-virtual-machine"></a><span data-ttu-id="24b20-242">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="24b20-242">Start virtual machine</span></span>

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="24b20-243">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="24b20-243">Delete resource group</span></span>

<span data-ttu-id="24b20-244">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="24b20-244">Deleting a resource group also deletes all resources contained within.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a><span data-ttu-id="24b20-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24b20-245">Next steps</span></span>

<span data-ttu-id="24b20-246">I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:</span><span class="sxs-lookup"><span data-stu-id="24b20-246">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24b20-247">Skapa och ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-247">Create and connect to a VM</span></span>
> * <span data-ttu-id="24b20-248">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="24b20-248">Select and use VM images</span></span>
> * <span data-ttu-id="24b20-249">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="24b20-249">View and use specific VM sizes</span></span>
> * <span data-ttu-id="24b20-250">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-250">Resize a VM</span></span>
> * <span data-ttu-id="24b20-251">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="24b20-251">View and understand VM state</span></span>

<span data-ttu-id="24b20-252">Gå vidare till nästa kurs vill veta mer om Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="24b20-252">Advance to the next tutorial to learn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="24b20-253">Skapa och hantera Virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="24b20-253">Create and Manage VM disks</span></span>](./tutorial-manage-data-disk.md)
