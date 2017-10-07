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
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a><span data-ttu-id="f484d-103">Skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="f484d-103">Create and Manage Windows VMs with hello Azure PowerShell module</span></span>

<span data-ttu-id="f484d-104">Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö.</span><span class="sxs-lookup"><span data-stu-id="f484d-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="f484d-105">Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="f484d-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="f484d-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f484d-107">Skapa och ansluta tooa VM</span><span class="sxs-lookup"><span data-stu-id="f484d-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="f484d-108">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="f484d-108">Select and use VM images</span></span>
> * <span data-ttu-id="f484d-109">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="f484d-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="f484d-110">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-110">Resize a VM</span></span>
> * <span data-ttu-id="f484d-111">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-111">View and understand VM state</span></span>

<span data-ttu-id="f484d-112">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f484d-112">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f484d-113">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="f484d-113">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f484d-114">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f484d-114">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="f484d-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f484d-115">Create resource group</span></span>

<span data-ttu-id="f484d-116">Skapa en resursgrupp med hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando.</span><span class="sxs-lookup"><span data-stu-id="f484d-116">Create a resource group with hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> 

<span data-ttu-id="f484d-117">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="f484d-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="f484d-118">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="f484d-119">I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i hello *EastUS* region.</span><span class="sxs-lookup"><span data-stu-id="f484d-119">In this example, a resource group named *myResourceGroupVM* is created in hello *EastUS* region.</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

<span data-ttu-id="f484d-120">hello resursgrupp anges när du skapar eller ändrar en VM som kan ses i hela den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f484d-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="f484d-121">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-121">Create virtual machine</span></span>

<span data-ttu-id="f484d-122">En virtuell dator måste vara anslutna tooa virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f484d-122">A virtual machine must be connected tooa virtual network.</span></span> <span data-ttu-id="f484d-123">Du kan kommunicera med hello virtuell dator med en offentlig IP-adress via ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="f484d-123">You communicate with hello virtual machine using a public IP address through a network interface card.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="f484d-124">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="f484d-124">Create virtual network</span></span>

<span data-ttu-id="f484d-125">Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f484d-125">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="f484d-126">Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="f484d-126">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a><span data-ttu-id="f484d-127">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="f484d-127">Create public IP address</span></span>

<span data-ttu-id="f484d-128">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="f484d-128">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a><span data-ttu-id="f484d-129">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="f484d-129">Create network interface card</span></span>

<span data-ttu-id="f484d-130">Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="f484d-130">Create a network interface card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a><span data-ttu-id="f484d-131">Skapa nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="f484d-131">Create network security group</span></span>

<span data-ttu-id="f484d-132">En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f484d-132">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="f484d-133">Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="f484d-133">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="f484d-134">De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-134">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span> <span data-ttu-id="f484d-135">tooaccess hello IIS webbserver som du installerar, måste du lägga till en regel för inkommande NSG.</span><span class="sxs-lookup"><span data-stu-id="f484d-135">tooaccess hello IIS webserver that you are installing, you must add an inbound NSG rule.</span></span>

<span data-ttu-id="f484d-136">toocreate en inkommande NSG regel använder [Lägg till AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="f484d-136">toocreate an inbound NSG rule, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="f484d-137">hello följande exempel skapas en NSG regeln med namnet *myNSGRule* som öppnar port *3389* för hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="f484d-137">hello following example creates an NSG rule named *myNSGRule* that opens port *3389* for hello virtual machine:</span></span>

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

<span data-ttu-id="f484d-138">Skapa hello NSG med *myNSGRule* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="f484d-138">Create hello NSG using *myNSGRule* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

<span data-ttu-id="f484d-139">Lägg till hello NSG toohello undernät i hello virtuellt nätverk med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f484d-139">Add hello NSG toohello subnet in hello virtual network with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="f484d-140">Uppdatera hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="f484d-140">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a><span data-ttu-id="f484d-141">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-141">Create virtual machine</span></span>

<span data-ttu-id="f484d-142">När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken.</span><span class="sxs-lookup"><span data-stu-id="f484d-142">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="f484d-143">I det här exemplet skapas en virtuell dator med namnet *myVM* körs hello senaste versionen av Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="f484d-143">In this example, a virtual machine is created with a name of *myVM* running hello latest version of Windows Server 2016 Datacenter.</span></span>

<span data-ttu-id="f484d-144">Ange hello användarnamn och lösenord som krävs för hello administratörskontot på hello virtuell dator med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="f484d-144">Set hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f484d-145">Skapa hello inledande konfiguration för hello virtuell dator med [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span><span class="sxs-lookup"><span data-stu-id="f484d-145">Create hello initial configuration for hello virtual machine with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

<span data-ttu-id="f484d-146">Lägg till hello operativsystemet information toohello konfigurationen virtuella datorn med [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span><span class="sxs-lookup"><span data-stu-id="f484d-146">Add hello operating system information toohello virtual machine configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):</span></span>

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="f484d-147">Lägg till hello avbildningen information toohello konfiguration av virtuell dator med [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span><span class="sxs-lookup"><span data-stu-id="f484d-147">Add hello image information toohello virtual machine configuration with [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

<span data-ttu-id="f484d-148">Lägg till hello operativsystemet inställningar toohello virtuella diskkonfigurationen med [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span><span class="sxs-lookup"><span data-stu-id="f484d-148">Add hello operating system disk settings toohello virtual machine configuration with [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

<span data-ttu-id="f484d-149">Lägg till hello-nätverkskort som du skapade tidigare toohello konfiguration av virtuell dator med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="f484d-149">Add hello network interface card that you previously created toohello virtual machine configuration with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="f484d-150">Skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f484d-150">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a><span data-ttu-id="f484d-151">Ansluta tooVM</span><span class="sxs-lookup"><span data-stu-id="f484d-151">Connect tooVM</span></span>

<span data-ttu-id="f484d-152">Skapa en anslutning till fjärrskrivbord med hello virtuella datorn när hello distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f484d-152">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="f484d-153">Kör hello följande kommandon tooreturn hello offentliga IP-adress för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-153">Run hello following commands tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="f484d-154">Anteckna denna IP-adress så kan du ansluta tooit med din webbläsare tootest webbanslutningen i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="f484d-154">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

<span data-ttu-id="f484d-155">Använd hello följande kommando toocreate en fjärrskrivbordssession med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-155">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="f484d-156">Ersätt hello IP-adress med hello *publicIPAddress* i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f484d-156">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="f484d-157">När du uppmanas ange hello-autentiseringsuppgifter som används när du skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-157">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a><span data-ttu-id="f484d-158">Förstå VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="f484d-158">Understand VM images</span></span>

<span data-ttu-id="f484d-159">hello Azure marketplace innehåller många avbildningar av virtuella datorer som kan använda toocreate en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-159">hello Azure marketplace includes many virtual machine images that can be used toocreate a new virtual machine.</span></span> <span data-ttu-id="f484d-160">I föregående steg hello skapades en virtuell dator med hello Windows Server 2016-Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f484d-160">In hello previous steps, a virtual machine was created using hello Windows Server 2016-Datacenter image.</span></span> <span data-ttu-id="f484d-161">I det här steget är hello PowerShell-modulen används toosearch hello marketplace för andra Windows-avbildningar kan också som bas för nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f484d-161">In this step, hello PowerShell module is used toosearch hello marketplace for other Windows images, which can also as a base for new VMs.</span></span> <span data-ttu-id="f484d-162">Den här processen består av att hitta hello utgivare, erbjudande och hello avbildningsnamn (Sku).</span><span class="sxs-lookup"><span data-stu-id="f484d-162">This process consists of finding hello publisher, offer, and hello image name (Sku).</span></span> 

<span data-ttu-id="f484d-163">Använd hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) kommandot tooreturn en lista över avbildningen utgivare.</span><span class="sxs-lookup"><span data-stu-id="f484d-163">Use hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) command tooreturn a list of image publishers.</span></span>  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

<span data-ttu-id="f484d-164">Använd hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn en lista med bild erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="f484d-164">Use hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn a list of image offers.</span></span> <span data-ttu-id="f484d-165">Med det här kommandot returnerade hello listan filtreras på hello angiven utgivare.</span><span class="sxs-lookup"><span data-stu-id="f484d-165">With this command, hello returned list is filtered on hello specified publisher.</span></span> 

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

<span data-ttu-id="f484d-166">Hej [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) kommandot kommer sedan filtrera hello utgivare och erbjudandet namnet tooreturn en lista över namn på bilder.</span><span class="sxs-lookup"><span data-stu-id="f484d-166">hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) command will then filter on hello publisher and offer name tooreturn a list of image names.</span></span>

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

<span data-ttu-id="f484d-167">Den här informationen kan vara används toodeploy en virtuell dator med en viss bild.</span><span class="sxs-lookup"><span data-stu-id="f484d-167">This information can be used toodeploy a VM with a specific image.</span></span> <span data-ttu-id="f484d-168">Det här exemplet anger hello avbildningens namn på hello VM-objekt.</span><span class="sxs-lookup"><span data-stu-id="f484d-168">This example sets hello image name on hello VM object.</span></span> <span data-ttu-id="f484d-169">Läs toohello föregående exempel i den här självstudiekursen fullständig distributionssteg.</span><span class="sxs-lookup"><span data-stu-id="f484d-169">Refer toohello previous examples in this tutorial for complete deployment steps.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="f484d-170">Förstå VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="f484d-170">Understand VM sizes</span></span>

<span data-ttu-id="f484d-171">Storlek på en virtuell dator bestäms hello beräkningsresurser som Processorn och GPU-minne som görs tillgängliga toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f484d-171">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="f484d-172">Virtuella datorer måste toobe som skapats med en lämplig för hello räknar arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="f484d-172">Virtual machines need toobe created with a size appropriate for hello expect work load.</span></span> <span data-ttu-id="f484d-173">Om belastningen ökar, kan en befintlig virtuell dator ändras.</span><span class="sxs-lookup"><span data-stu-id="f484d-173">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="f484d-174">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="f484d-174">VM Sizes</span></span>

<span data-ttu-id="f484d-175">i den följande tabellen hello kategoriserar storlekar i användningsfall.</span><span class="sxs-lookup"><span data-stu-id="f484d-175">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="f484d-176">Typ</span><span class="sxs-lookup"><span data-stu-id="f484d-176">Type</span></span>                     | <span data-ttu-id="f484d-177">Storlekar</span><span class="sxs-lookup"><span data-stu-id="f484d-177">Sizes</span></span>           |    <span data-ttu-id="f484d-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f484d-178">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f484d-179">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="f484d-179">General purpose</span></span>         |<span data-ttu-id="f484d-180">DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="f484d-180">DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="f484d-181">Belastningsutjämnade CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="f484d-181">Balanced CPU-to-memory.</span></span> <span data-ttu-id="f484d-182">Idealiskt för dev / test och lösningar för små toomedium program och data.</span><span class="sxs-lookup"><span data-stu-id="f484d-182">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| <span data-ttu-id="f484d-183">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="f484d-183">Compute optimized</span></span>      | <span data-ttu-id="f484d-184">FS, F</span><span class="sxs-lookup"><span data-stu-id="f484d-184">Fs, F</span></span>             | <span data-ttu-id="f484d-185">Hög CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="f484d-185">High CPU-to-memory.</span></span> <span data-ttu-id="f484d-186">Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.</span><span class="sxs-lookup"><span data-stu-id="f484d-186">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| <span data-ttu-id="f484d-187">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="f484d-187">Memory optimized</span></span>       | <span data-ttu-id="f484d-188">GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="f484d-188">GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="f484d-189">Hög minne-till-core.</span><span class="sxs-lookup"><span data-stu-id="f484d-189">High memory-to-core.</span></span> <span data-ttu-id="f484d-190">Perfekt för relationsdatabaser, medelhög toolarge cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="f484d-190">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| <span data-ttu-id="f484d-191">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="f484d-191">Storage optimized</span></span>       | <span data-ttu-id="f484d-192">Ls</span><span class="sxs-lookup"><span data-stu-id="f484d-192">Ls</span></span>                | <span data-ttu-id="f484d-193">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="f484d-193">High disk throughput and IO.</span></span> <span data-ttu-id="f484d-194">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="f484d-194">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| <span data-ttu-id="f484d-195">GPU</span><span class="sxs-lookup"><span data-stu-id="f484d-195">GPU</span></span>           | <span data-ttu-id="f484d-196">NV NC</span><span class="sxs-lookup"><span data-stu-id="f484d-196">NV, NC</span></span>            | <span data-ttu-id="f484d-197">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="f484d-197">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| <span data-ttu-id="f484d-198">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="f484d-198">High performance</span></span> | <span data-ttu-id="f484d-199">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="f484d-199">H, A8-11</span></span>          | <span data-ttu-id="f484d-200">Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA).</span><span class="sxs-lookup"><span data-stu-id="f484d-200">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="f484d-201">Hitta tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="f484d-201">Find available VM sizes</span></span>

<span data-ttu-id="f484d-202">toosee en lista över Virtuella storlekar tillgängliga i en viss region, Använd hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando.</span><span class="sxs-lookup"><span data-stu-id="f484d-202">toosee a list of VM sizes available in a particular region, use hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command.</span></span>

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a><span data-ttu-id="f484d-203">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-203">Resize a VM</span></span>

<span data-ttu-id="f484d-204">När en virtuell dator har distribuerats kan storleksändrade tooincrease eller minska resursallokering.</span><span class="sxs-lookup"><span data-stu-id="f484d-204">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="f484d-205">Innan du ändrar storlek på en virtuell dator, kontrollera om hello önskad storlek är tillgänglig på hello aktuella virtuella datorns kluster.</span><span class="sxs-lookup"><span data-stu-id="f484d-205">Before resizing a VM, check if hello desired size is available on hello current VM cluster.</span></span> <span data-ttu-id="f484d-206">Hej [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) kommando returnerar en lista över storlekar.</span><span class="sxs-lookup"><span data-stu-id="f484d-206">hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) command returns a list of sizes.</span></span> 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

<span data-ttu-id="f484d-207">Eventuellt hello storleken är tillgänglig hello VM storlek kan ändras från ett slås på tillstånd, men den startas under hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f484d-207">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

<span data-ttu-id="f484d-208">Om hello önskad storlek är inte på hello aktuella klustret hello VM behov toobe frigjorts innan hello att ändra storlek på kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="f484d-208">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="f484d-209">Observera att när hello VM är påslagen tillbaka, några data på hello temp disken har tagits bort och hello offentliga IP-adressen ändras såvida inte en statisk IP-adress används.</span><span class="sxs-lookup"><span data-stu-id="f484d-209">Note, when hello VM is powered back on, any data on hello temp disk are removed, and hello public IP address change unless a static IP address is being used.</span></span> 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a><span data-ttu-id="f484d-210">Energisparfunktioner för VM</span><span class="sxs-lookup"><span data-stu-id="f484d-210">VM power states</span></span>

<span data-ttu-id="f484d-211">En virtuell Azure-dator kan ha en av många energisparfunktioner.</span><span class="sxs-lookup"><span data-stu-id="f484d-211">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="f484d-212">Det här tillståndet representerar hello aktuell status för hello VM från hello synvinkel av hello hypervisor.</span><span class="sxs-lookup"><span data-stu-id="f484d-212">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="f484d-213">Energisparfunktioner</span><span class="sxs-lookup"><span data-stu-id="f484d-213">Power states</span></span>

| <span data-ttu-id="f484d-214">Energisparläge</span><span class="sxs-lookup"><span data-stu-id="f484d-214">Power State</span></span> | <span data-ttu-id="f484d-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f484d-215">Description</span></span>
|----|----|
| <span data-ttu-id="f484d-216">Startar</span><span class="sxs-lookup"><span data-stu-id="f484d-216">Starting</span></span> | <span data-ttu-id="f484d-217">Anger hello virtuella datorn startas.</span><span class="sxs-lookup"><span data-stu-id="f484d-217">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="f484d-218">Körs</span><span class="sxs-lookup"><span data-stu-id="f484d-218">Running</span></span> | <span data-ttu-id="f484d-219">Anger att hello den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="f484d-219">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="f484d-220">Stoppas</span><span class="sxs-lookup"><span data-stu-id="f484d-220">Stopping</span></span> | <span data-ttu-id="f484d-221">Anger att hello den virtuella datorn stoppas.</span><span class="sxs-lookup"><span data-stu-id="f484d-221">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="f484d-222">Stoppad</span><span class="sxs-lookup"><span data-stu-id="f484d-222">Stopped</span></span> | <span data-ttu-id="f484d-223">Anger att hello den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="f484d-223">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="f484d-224">Observera att virtuella datorer i hello stoppats tillstånd fortfarande avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="f484d-224">Note that virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="f484d-225">Det har frigjorts</span><span class="sxs-lookup"><span data-stu-id="f484d-225">Deallocating</span></span> | <span data-ttu-id="f484d-226">Anger att hello den virtuella datorn har flyttats.</span><span class="sxs-lookup"><span data-stu-id="f484d-226">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="f484d-227">Frigöra</span><span class="sxs-lookup"><span data-stu-id="f484d-227">Deallocated</span></span> | <span data-ttu-id="f484d-228">Anger att hello den virtuella datorn bort helt från hello hypervisor men är fortfarande tillgänglig i hello kontrollplan.</span><span class="sxs-lookup"><span data-stu-id="f484d-228">Indicates that hello virtual machine is completely removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="f484d-229">Virtuella datorer i hello Deallocated tillstånd inte avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="f484d-229">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="f484d-230">Anger att hello energisparläge för hello virtuell dator är okänt.</span><span class="sxs-lookup"><span data-stu-id="f484d-230">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="f484d-231">Hitta energiläge</span><span class="sxs-lookup"><span data-stu-id="f484d-231">Find power state</span></span>

<span data-ttu-id="f484d-232">tooretrieve hello tillståndet för en viss virtuell dator, Använd hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando.</span><span class="sxs-lookup"><span data-stu-id="f484d-232">tooretrieve hello state of a particular VM, use hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span> <span data-ttu-id="f484d-233">Vara säker på att toospecify ett giltigt namn för en virtuell dator och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f484d-233">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

<span data-ttu-id="f484d-234">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f484d-234">Output:</span></span>

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a><span data-ttu-id="f484d-235">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="f484d-235">Management tasks</span></span>

<span data-ttu-id="f484d-236">Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f484d-236">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="f484d-237">Dessutom kan du toocreate skript tooautomate repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f484d-237">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="f484d-238">Med hjälp av Azure PowerShell, kan många vanliga administrativa uppgifter köras från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="f484d-238">Using Azure PowerShell, many common management tasks can be run from hello command line or in scripts.</span></span>

### <a name="stop-virtual-machine"></a><span data-ttu-id="f484d-239">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f484d-239">Stop virtual machine</span></span>

<span data-ttu-id="f484d-240">Stoppa och ta bort en virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="f484d-240">Stop and deallocate a virtual machine with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

<span data-ttu-id="f484d-241">Om du vill tookeep hello virtuell dator i ett etablerat tillstånd, använder du hello - StayProvisioned-parametern.</span><span class="sxs-lookup"><span data-stu-id="f484d-241">If you want tookeep hello virtual machine in a provisioned state, use hello -StayProvisioned parameter.</span></span>

### <a name="start-virtual-machine"></a><span data-ttu-id="f484d-242">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f484d-242">Start virtual machine</span></span>

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="f484d-243">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="f484d-243">Delete resource group</span></span>

<span data-ttu-id="f484d-244">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="f484d-244">Deleting a resource group also deletes all resources contained within.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a><span data-ttu-id="f484d-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f484d-245">Next steps</span></span>

<span data-ttu-id="f484d-246">I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:</span><span class="sxs-lookup"><span data-stu-id="f484d-246">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f484d-247">Skapa och ansluta tooa VM</span><span class="sxs-lookup"><span data-stu-id="f484d-247">Create and connect tooa VM</span></span>
> * <span data-ttu-id="f484d-248">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="f484d-248">Select and use VM images</span></span>
> * <span data-ttu-id="f484d-249">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="f484d-249">View and use specific VM sizes</span></span>
> * <span data-ttu-id="f484d-250">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-250">Resize a VM</span></span>
> * <span data-ttu-id="f484d-251">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f484d-251">View and understand VM state</span></span>

<span data-ttu-id="f484d-252">Avancera toohello nästa självstudiekurs toolearn om Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="f484d-252">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="f484d-253">Skapa och hantera Virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="f484d-253">Create and Manage VM disks</span></span>](./tutorial-manage-data-disk.md)
