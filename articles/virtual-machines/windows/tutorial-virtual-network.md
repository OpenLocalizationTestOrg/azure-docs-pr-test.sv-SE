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
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a><span data-ttu-id="1c57c-103">Hantera virtuella Azure-nätverk och virtuella Windows-datorer med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1c57c-103">Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell</span></span>

<span data-ttu-id="1c57c-104">Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="1c57c-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="1c57c-105">I kursen får du skapa flera virtuella datorer (VM) i ett virtuellt nätverk och konfigurerar nätverksanslutningen mellan dem.</span><span class="sxs-lookup"><span data-stu-id="1c57c-105">In this tutorial, you create multiple virtual machines (VMs) in a virtual network and configure network connectivity between them.</span></span> <span data-ttu-id="1c57c-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="1c57c-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c57c-107">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1c57c-107">Create a virtual network</span></span>
> * <span data-ttu-id="1c57c-108">Skapa undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1c57c-108">Create virtual network subnets</span></span>
> * <span data-ttu-id="1c57c-109">Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="1c57c-109">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="1c57c-110">Visa regler för nätverkstrafik i praktiken</span><span class="sxs-lookup"><span data-stu-id="1c57c-110">View traffic rules in action</span></span>

<span data-ttu-id="1c57c-111">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1c57c-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="1c57c-112">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="1c57c-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="1c57c-113">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="1c57c-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-vnet"></a><span data-ttu-id="1c57c-114">Skapa VNet</span><span class="sxs-lookup"><span data-stu-id="1c57c-114">Create VNet</span></span>

<span data-ttu-id="1c57c-115">Ett virtuellt nätverk är en representation av ditt eget nätverk i molnet.</span><span class="sxs-lookup"><span data-stu-id="1c57c-115">A VNet is a representation of your own network in the cloud.</span></span> <span data-ttu-id="1c57c-116">Ett virtuellt nätverk är en logisk isolering av Azure-molnet dedikerad till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c57c-116">A VNet is a logical isolation of the Azure cloud dedicated to your subscription.</span></span> <span data-ttu-id="1c57c-117">Du hittar undernät, regler för anslutning till dessa undernät och anslutningar från de virtuella datorerna till undernäten inom ett VNet.</span><span class="sxs-lookup"><span data-stu-id="1c57c-117">Within a VNet, you find subnets, rules for connectivity to those subnets, and connections from the VMs to the subnets.</span></span>

<span data-ttu-id="1c57c-118">Innan du kan skapa andra Azure-resurser, måste du skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="1c57c-118">Before you can create any other Azure resources, you need to create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="1c57c-119">I följande exempel skapas en resursgrupp med namnet *myRGNetwork* i den *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="1c57c-119">The following example creates a resource group named *myRGNetwork* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

<span data-ttu-id="1c57c-120">Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix.</span><span class="sxs-lookup"><span data-stu-id="1c57c-120">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="1c57c-121">Nätverkskorten kan läggas till i undernät och anslutna till virtuella datorer kan man har anslutning för olika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="1c57c-121">NICs can be added to subnets, and connected to VMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="1c57c-122">Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="1c57c-122">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="1c57c-123">Skapa ett VNET med namnet *myVNet* med *myFrontendSubnet* med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="1c57c-123">Create a VNET named *myVNet* using *myFrontendSubnet* with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a><span data-ttu-id="1c57c-124">Skapa frontend VM</span><span class="sxs-lookup"><span data-stu-id="1c57c-124">Create front-end VM</span></span>

<span data-ttu-id="1c57c-125">En virtuell dator, för att kommunicera på ett VNet måste den ha ett virtuellt nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="1c57c-125">For a VM to communicate in a VNet, it needs a virtual network interface (NIC).</span></span> <span data-ttu-id="1c57c-126">Den *myFrontendVM* nås från internet, så måste en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1c57c-126">The *myFrontendVM* is accessed from the internet, so it also needs a public IP address.</span></span> 

<span data-ttu-id="1c57c-127">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="1c57c-127">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

<span data-ttu-id="1c57c-128">Skapa ett nätverkskort med [nya AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="1c57c-128">Create a NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

<span data-ttu-id="1c57c-129">Ange användarnamn och lösenord behövs för administratören konto på den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="1c57c-129">Set the username and password needed for the administrator account on the VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="1c57c-130">Skapa de virtuella datorerna med [nya AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [lägga till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="1c57c-130">Create the VMs with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> 

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

## <a name="install-web-server"></a><span data-ttu-id="1c57c-131">Installera webbserver</span><span class="sxs-lookup"><span data-stu-id="1c57c-131">Install web server</span></span>

<span data-ttu-id="1c57c-132">Du kan installera IIS på *myFrontendVM* med hjälp av en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="1c57c-132">You can install IIS on *myFrontendVM* by using a remote desktop session.</span></span> <span data-ttu-id="1c57c-133">Du måste hämta offentlig IP-adressen för den virtuella datorn att komma åt den.</span><span class="sxs-lookup"><span data-stu-id="1c57c-133">You need to get the public IP address of the VM to access it.</span></span>

<span data-ttu-id="1c57c-134">Du kan hämta den offentliga IP-adressen för *myFrontendVM* med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="1c57c-134">You can get the public IP address of *myFrontendVM* with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="1c57c-135">I följande exempel hämtar IP-adressen för *myPublicIPAddress* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="1c57c-135">The following example obtains the IP address for *myPublicIPAddress* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

<span data-ttu-id="1c57c-136">Anteckna denna IP-adress så att du kan använda den i kommande steg.</span><span class="sxs-lookup"><span data-stu-id="1c57c-136">Take note of this IP Address so you can use it in future steps.</span></span>

<span data-ttu-id="1c57c-137">Använd följande kommando för att skapa en fjärrskrivbords-session med *myFrontendVM*.</span><span class="sxs-lookup"><span data-stu-id="1c57c-137">Use the following command to create a remote desktop session with *myFrontendVM*.</span></span> <span data-ttu-id="1c57c-138">Ersätt  *<publicIPAddress>*  med adressen som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="1c57c-138">Replace *<publicIPAddress>* with the address that you previously recorded.</span></span> <span data-ttu-id="1c57c-139">När du uppmanas, anger du de autentiseringsuppgifter som används när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1c57c-139">When prompted, enter the credentials used when you created the VM.</span></span>

```
mstsc /v:<publicIpAddress>
``` 

<span data-ttu-id="1c57c-140">Nu när du har loggat in *myFrontendVM*, du kan använda en rad med PowerShell för att installera IIS och aktivera regeln för lokala brandväggen att tillåta webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="1c57c-140">Now that you have logged in to *myFrontendVM*, you can use a single line of PowerShell to install IIS and enable the local firewall rule to allow web traffic.</span></span> <span data-ttu-id="1c57c-141">Öppna en PowerShell-kommandotolk och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1c57c-141">Open a PowerShell prompt and run the following command:</span></span>

<span data-ttu-id="1c57c-142">Använd [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) att köra tillägget för anpassat skript som installerar IIS-webbserver:</span><span class="sxs-lookup"><span data-stu-id="1c57c-142">Use [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) to run the custom script extension that installs the IIS webserver:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="1c57c-143">Nu kan du använda den offentliga IP-adressen och bläddra till den virtuella datorn att se IIS-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="1c57c-143">Now you can use the public IP address to browse to the VM to see the IIS site.</span></span>

![Standardwebbplatsen i IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a><span data-ttu-id="1c57c-145">Hantera intern trafik</span><span class="sxs-lookup"><span data-stu-id="1c57c-145">Manage internal traffic</span></span>

<span data-ttu-id="1c57c-146">En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverkstrafik till resurser som är anslutna till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1c57c-146">A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to a VNet.</span></span> <span data-ttu-id="1c57c-147">NSG: er kan vara kopplad till undernät eller individuella nätverkskort som är kopplade till virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c57c-147">NSGs can be associated to subnets or individual NICs attached to VMs.</span></span> <span data-ttu-id="1c57c-148">Öppna eller stänga åtkomst till virtuella datorer via portar görs med hjälp av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="1c57c-148">Opening or closing access to VMs through ports is done using NSG rules.</span></span> <span data-ttu-id="1c57c-149">När du skapade *myFrontendVM*, inkommande port 3389 öppnas automatiskt för RDP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="1c57c-149">When you created *myFrontendVM*, inbound port 3389 was automatically opened for RDP connectivity.</span></span>

<span data-ttu-id="1c57c-150">Intern kommunikation för virtuella datorer kan konfigureras med en NSG.</span><span class="sxs-lookup"><span data-stu-id="1c57c-150">Internal communication of VMs can be configured using an NSG.</span></span> <span data-ttu-id="1c57c-151">I det här avsnittet kan du lära dig hur du skapar en ytterligare undernät i nätverket och tilldelar en NSG till den för att tillåta en anslutning från *myFrontendVM* till *myBackendVM* på port 1433.</span><span class="sxs-lookup"><span data-stu-id="1c57c-151">In this section, you learn how to create an additional subnet in the network and assign an NSG to it to allow a connection from *myFrontendVM* to *myBackendVM* on port 1433.</span></span> <span data-ttu-id="1c57c-152">Undernätet sedan tilldelas den virtuella datorn när den skapas.</span><span class="sxs-lookup"><span data-stu-id="1c57c-152">The subnet is then assigned to the VM when it is created.</span></span>

<span data-ttu-id="1c57c-153">Du kan begränsa intern trafik till *myBackendVM* från endast *myFrontendVM* genom att skapa en NSG för backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="1c57c-153">You can limit internal traffic to *myBackendVM* from only *myFrontendVM* by creating an NSG for the back-end subnet.</span></span> <span data-ttu-id="1c57c-154">I följande exempel skapas en NSG regeln med namnet *myBackendNSGRule* med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span><span class="sxs-lookup"><span data-stu-id="1c57c-154">The following example creates an NSG rule named *myBackendNSGRule* with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span></span>

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

<span data-ttu-id="1c57c-155">Lägg till en nätverkssäkerhetsgrupp med namnet *myBackendNSG* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="1c57c-155">Add a network security group named *myBackendNSG* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a><span data-ttu-id="1c57c-156">Lägg till backend-undernät</span><span class="sxs-lookup"><span data-stu-id="1c57c-156">Add back-end subnet</span></span>

<span data-ttu-id="1c57c-157">Lägg till *myBackEndSubnet* till *myVNet* med [Lägg till AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="1c57c-157">Add *myBackEndSubnet* to *myVNet* with [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span></span>

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

## <a name="create-back-end-vm"></a><span data-ttu-id="1c57c-158">Skapa backend-VM</span><span class="sxs-lookup"><span data-stu-id="1c57c-158">Create back-end VM</span></span>

<span data-ttu-id="1c57c-159">Det enklaste sättet att skapa den virtuella datorn serverdel är med hjälp av en SQL Server-avbildning.</span><span class="sxs-lookup"><span data-stu-id="1c57c-159">The easiest way to create the back-end VM is by using a SQL Server image.</span></span> <span data-ttu-id="1c57c-160">Den här kursen kan du bara skapar den virtuella datorn med databasservern, men ger inte information om åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="1c57c-160">This tutorial only creates the VM with the database server, but doesn't provide information about accessing the database.</span></span>

<span data-ttu-id="1c57c-161">Skapa *myBackendNic*:</span><span class="sxs-lookup"><span data-stu-id="1c57c-161">Create *myBackendNic*:</span></span>

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

<span data-ttu-id="1c57c-162">Ange användarnamn och lösenord för administratörskontot på den virtuella datorn med Get-Credential:</span><span class="sxs-lookup"><span data-stu-id="1c57c-162">Set the username and password needed for the administrator account on the VM with Get-Credential:</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="1c57c-163">Skapa *myBackendVM*:</span><span class="sxs-lookup"><span data-stu-id="1c57c-163">Create *myBackendVM*:</span></span>

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

<span data-ttu-id="1c57c-164">Den bild som används för SQL Server är installerad men används inte i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="1c57c-164">The image that is used has SQL Server installed, but is not used in this tutorial.</span></span> <span data-ttu-id="1c57c-165">Den ingår för att visa hur du kan konfigurera en virtuell dator för att hantera webbtrafik och en virtuell dator för att hantera databasen.</span><span class="sxs-lookup"><span data-stu-id="1c57c-165">It is included to show you how you can configure a VM to handle web traffic and a VM to handle database management.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c57c-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c57c-166">Next steps</span></span>

<span data-ttu-id="1c57c-167">I den här självstudiekursen skapas och skyddas av Azure-nätverk som är relaterade till virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c57c-167">In this tutorial, you created and secured Azure networks as related to virtual machines.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="1c57c-168">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1c57c-168">Create a virtual network</span></span>
> * <span data-ttu-id="1c57c-169">Skapa undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1c57c-169">Create virtual network subnets</span></span>
> * <span data-ttu-id="1c57c-170">Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="1c57c-170">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="1c57c-171">Visa regler för nätverkstrafik i praktiken</span><span class="sxs-lookup"><span data-stu-id="1c57c-171">View traffic rules in action</span></span>

<span data-ttu-id="1c57c-172">Gå vidare till nästa kurs vill veta mer om övervakning för att skydda data på virtuella datorer med hjälp av Azure-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1c57c-172">Advance to the next tutorial to learn about monitoring securing data on virtual machines using Azure backup.</span></span> <span data-ttu-id="1c57c-173">.</span><span class="sxs-lookup"><span data-stu-id="1c57c-173">.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1c57c-174">Säkerhetskopiera Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="1c57c-174">Back up Windows virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
