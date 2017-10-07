---
title: "aaaAzure virtuella nätverk och virtuella Windows-datorer | Microsoft Docs"
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
ms.openlocfilehash: ed77d9d5873e849fcb2aaf15e41899d7ad8c781a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a><span data-ttu-id="f8a5f-103">Hantera virtuella Azure-nätverk och virtuella Windows-datorer med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8a5f-103">Manage Azure Virtual Networks and Windows Virtual Machines with Azure PowerShell</span></span>

<span data-ttu-id="f8a5f-104">Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="f8a5f-105">I kursen får du skapa flera virtuella datorer (VM) i ett virtuellt nätverk och konfigurerar nätverksanslutningen mellan dem.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-105">In this tutorial, you create multiple virtual machines (VMs) in a virtual network and configure network connectivity between them.</span></span> <span data-ttu-id="f8a5f-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8a5f-107">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f8a5f-107">Create a virtual network</span></span>
> * <span data-ttu-id="f8a5f-108">Skapa undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f8a5f-108">Create virtual network subnets</span></span>
> * <span data-ttu-id="f8a5f-109">Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="f8a5f-109">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="f8a5f-110">Visa regler för nätverkstrafik i praktiken</span><span class="sxs-lookup"><span data-stu-id="f8a5f-110">View traffic rules in action</span></span>

<span data-ttu-id="f8a5f-111">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f8a5f-112">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f8a5f-113">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f8a5f-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="create-vnet"></a><span data-ttu-id="f8a5f-114">Skapa VNet</span><span class="sxs-lookup"><span data-stu-id="f8a5f-114">Create VNet</span></span>

<span data-ttu-id="f8a5f-115">Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-115">A VNet is a representation of your own network in hello cloud.</span></span> <span data-ttu-id="f8a5f-116">Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-116">A VNet is a logical isolation of hello Azure cloud dedicated tooyour subscription.</span></span> <span data-ttu-id="f8a5f-117">Du hittar undernät, regler för anslutningen toothose undernät och anslutningar från hello VMs toohello undernät inom ett VNet.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-117">Within a VNet, you find subnets, rules for connectivity toothose subnets, and connections from hello VMs toohello subnets.</span></span>

<span data-ttu-id="f8a5f-118">Innan du kan skapa andra Azure-resurser, behöver du toocreate en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="f8a5f-118">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="f8a5f-119">hello följande exempel skapar en resursgrupp med namnet *myRGNetwork* i hello *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-119">hello following example creates a resource group named *myRGNetwork* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

<span data-ttu-id="f8a5f-120">Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-120">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="f8a5f-121">Nätverkskort läggas toosubnets och anslutna tooVMs som man har anslutning för olika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-121">NICs can be added toosubnets, and connected tooVMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="f8a5f-122">Skapa ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-122">Create a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="f8a5f-123">Skapa ett VNET med namnet *myVNet* med *myFrontendSubnet* med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-123">Create a VNET named *myVNet* using *myFrontendSubnet* with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a><span data-ttu-id="f8a5f-124">Skapa frontend VM</span><span class="sxs-lookup"><span data-stu-id="f8a5f-124">Create front-end VM</span></span>

<span data-ttu-id="f8a5f-125">För en VM toocommunicate i ett VNet, måste den ha ett virtuellt nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="f8a5f-125">For a VM toocommunicate in a VNet, it needs a virtual network interface (NIC).</span></span> <span data-ttu-id="f8a5f-126">Hej *myFrontendVM* kan nås från hello internet, så måste en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-126">hello *myFrontendVM* is accessed from hello internet, so it also needs a public IP address.</span></span> 

<span data-ttu-id="f8a5f-127">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-127">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

<span data-ttu-id="f8a5f-128">Skapa ett nätverkskort med [nya AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-128">Create a NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):</span></span>


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

<span data-ttu-id="f8a5f-129">Ange hello användarnamn och lösenord som krävs för hello administratörskontot på hello VM med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-129">Set hello username and password needed for hello administrator account on hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f8a5f-130">Skapa hello virtuella datorer med [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Lägga till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f8a5f-130">Create hello VMs with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> 

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

## <a name="install-web-server"></a><span data-ttu-id="f8a5f-131">Installera webbserver</span><span class="sxs-lookup"><span data-stu-id="f8a5f-131">Install web server</span></span>

<span data-ttu-id="f8a5f-132">Du kan installera IIS på *myFrontendVM* med hjälp av en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-132">You can install IIS on *myFrontendVM* by using a remote desktop session.</span></span> <span data-ttu-id="f8a5f-133">Du behöver tooget hello offentliga IP-adress hello VM tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-133">You need tooget hello public IP address of hello VM tooaccess it.</span></span>

<span data-ttu-id="f8a5f-134">Du kan hämta hello offentliga IP-adressen för *myFrontendVM* med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="f8a5f-134">You can get hello public IP address of *myFrontendVM* with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="f8a5f-135">hello följande exempel hämtar hello IP-adress för *myPublicIPAddress* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-135">hello following example obtains hello IP address for *myPublicIPAddress* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

<span data-ttu-id="f8a5f-136">Anteckna denna IP-adress så att du kan använda den i kommande steg.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-136">Take note of this IP Address so you can use it in future steps.</span></span>

<span data-ttu-id="f8a5f-137">Använd hello följande kommando toocreate en fjärrskrivbordssession med *myFrontendVM*.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-137">Use hello following command toocreate a remote desktop session with *myFrontendVM*.</span></span> <span data-ttu-id="f8a5f-138">Ersätt  *<publicIPAddress>*  med hello-adress som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-138">Replace *<publicIPAddress>* with hello address that you previously recorded.</span></span> <span data-ttu-id="f8a5f-139">När du uppmanas ange hello-autentiseringsuppgifter som används när du skapade hello VM.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-139">When prompted, enter hello credentials used when you created hello VM.</span></span>

```
mstsc /v:<publicIpAddress>
``` 

<span data-ttu-id="f8a5f-140">Nu när du har loggat in för*myFrontendVM*, kan du använda en rad med PowerShell tooinstall IIS och aktivera hello lokala brandväggen regeln tooallow webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-140">Now that you have logged in too*myFrontendVM*, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="f8a5f-141">Öppna en PowerShell-kommandotolk och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-141">Open a PowerShell prompt and run hello following command:</span></span>

<span data-ttu-id="f8a5f-142">Använd [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello tillägget för anpassat skript som installerar hello IIS-webbserver:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-142">Use [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello custom script extension that installs hello IIS webserver:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="f8a5f-143">Du kan nu använda hello offentliga IP-adress toobrowse toohello VM toosee hello IIS-plats.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-143">Now you can use hello public IP address toobrowse toohello VM toosee hello IIS site.</span></span>

![Standardwebbplatsen i IIS](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a><span data-ttu-id="f8a5f-145">Hantera intern trafik</span><span class="sxs-lookup"><span data-stu-id="f8a5f-145">Manage internal traffic</span></span>

<span data-ttu-id="f8a5f-146">En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources anslutna tooa VNet.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-146">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooa VNet.</span></span> <span data-ttu-id="f8a5f-147">NSG: er kan vara associerade toosubnets eller individuella nätverkskort kopplade tooVMs.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-147">NSGs can be associated toosubnets or individual NICs attached tooVMs.</span></span> <span data-ttu-id="f8a5f-148">Öppna eller stänga åtkomst tooVMs via portar görs med hjälp av NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-148">Opening or closing access tooVMs through ports is done using NSG rules.</span></span> <span data-ttu-id="f8a5f-149">När du skapade *myFrontendVM*, inkommande port 3389 öppnas automatiskt för RDP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-149">When you created *myFrontendVM*, inbound port 3389 was automatically opened for RDP connectivity.</span></span>

<span data-ttu-id="f8a5f-150">Intern kommunikation för virtuella datorer kan konfigureras med en NSG.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-150">Internal communication of VMs can be configured using an NSG.</span></span> <span data-ttu-id="f8a5f-151">I det här avsnittet lär du dig hur toocreate ett ytterligare undernät i hello nätverks- och tilldela en NSG tooit tooallow en anslutning från *myFrontendVM* för*myBackendVM* på port 1433.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-151">In this section, you learn how toocreate an additional subnet in hello network and assign an NSG tooit tooallow a connection from *myFrontendVM* too*myBackendVM* on port 1433.</span></span> <span data-ttu-id="f8a5f-152">hello undernät tilldelas sedan toohello VM när den skapas.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-152">hello subnet is then assigned toohello VM when it is created.</span></span>

<span data-ttu-id="f8a5f-153">Du kan begränsa intern trafik för*myBackendVM* från endast *myFrontendVM* genom att skapa en NSG för hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-153">You can limit internal traffic too*myBackendVM* from only *myFrontendVM* by creating an NSG for hello back-end subnet.</span></span> <span data-ttu-id="f8a5f-154">hello följande exempel skapas en NSG regeln med namnet *myBackendNSGRule* med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-154">hello following example creates an NSG rule named *myBackendNSGRule* with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):</span></span>

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

<span data-ttu-id="f8a5f-155">Lägg till en nätverkssäkerhetsgrupp med namnet *myBackendNSG* med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-155">Add a network security group named *myBackendNSG* with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):</span></span>

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a><span data-ttu-id="f8a5f-156">Lägg till backend-undernät</span><span class="sxs-lookup"><span data-stu-id="f8a5f-156">Add back-end subnet</span></span>

<span data-ttu-id="f8a5f-157">Lägg till *myBackEndSubnet* för*myVNet* med [Lägg till AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="f8a5f-157">Add *myBackEndSubnet* too*myVNet* with [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):</span></span>

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

## <a name="create-back-end-vm"></a><span data-ttu-id="f8a5f-158">Skapa backend-VM</span><span class="sxs-lookup"><span data-stu-id="f8a5f-158">Create back-end VM</span></span>

<span data-ttu-id="f8a5f-159">hello enklaste sättet toocreate hello backend-VM är med hjälp av en SQL Server-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-159">hello easiest way toocreate hello back-end VM is by using a SQL Server image.</span></span> <span data-ttu-id="f8a5f-160">Den här självstudiekursen bara skapar hello VM med hello databasserver, men det innehåller inte information om åtkomst till hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-160">This tutorial only creates hello VM with hello database server, but doesn't provide information about accessing hello database.</span></span>

<span data-ttu-id="f8a5f-161">Skapa *myBackendNic*:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-161">Create *myBackendNic*:</span></span>

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

<span data-ttu-id="f8a5f-162">Ange hello användarnamn och lösenord för administratörskontot för hello på hello virtuell dator med Get-Credential:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-162">Set hello username and password needed for hello administrator account on hello VM with Get-Credential:</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f8a5f-163">Skapa *myBackendVM*:</span><span class="sxs-lookup"><span data-stu-id="f8a5f-163">Create *myBackendVM*:</span></span>

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

<span data-ttu-id="f8a5f-164">hello-bild som används SQL Server är installerad men används inte i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-164">hello image that is used has SQL Server installed, but is not used in this tutorial.</span></span> <span data-ttu-id="f8a5f-165">Det är inkluderade tooshow du hur du kan konfigurera en VM toohandle webbtrafik och en VM toohandle databashantering.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-165">It is included tooshow you how you can configure a VM toohandle web traffic and a VM toohandle database management.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8a5f-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8a5f-166">Next steps</span></span>

<span data-ttu-id="f8a5f-167">I den här självstudiekursen skapas och skyddas av Azure-nätverk som relaterade toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-167">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="f8a5f-168">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f8a5f-168">Create a virtual network</span></span>
> * <span data-ttu-id="f8a5f-169">Skapa undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f8a5f-169">Create virtual network subnets</span></span>
> * <span data-ttu-id="f8a5f-170">Kontrollera nätverkstrafik med Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="f8a5f-170">Control network traffic with Network Security Groups</span></span>
> * <span data-ttu-id="f8a5f-171">Visa regler för nätverkstrafik i praktiken</span><span class="sxs-lookup"><span data-stu-id="f8a5f-171">View traffic rules in action</span></span>

<span data-ttu-id="f8a5f-172">Avancera toohello nästa självstudiekurs toolearn om övervakning för att skydda data på virtuella datorer med hjälp av Azure-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-172">Advance toohello next tutorial toolearn about monitoring securing data on virtual machines using Azure backup.</span></span> <span data-ttu-id="f8a5f-173">.</span><span class="sxs-lookup"><span data-stu-id="f8a5f-173">.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f8a5f-174">Säkerhetskopiera Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="f8a5f-174">Back up Windows virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
