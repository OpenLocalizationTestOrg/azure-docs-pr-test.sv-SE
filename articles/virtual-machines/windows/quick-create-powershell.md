---
title: aaaAzure Snabbstart - skapa Windows VM PowerShell | Microsoft Docs
description: "Lär dig snabbt toocreate en Windows-datorer med PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5e92435bf7ee443a01c158fed91c356363e2f425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell"></a><span data-ttu-id="aa1c5-103">Skapa en virtuell Windows-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa1c5-103">Create a Windows virtual machine with PowerShell</span></span>

<span data-ttu-id="aa1c5-104">hello Azure PowerShell-modulen är används toocreate och hantera Azure-resurser från hello PowerShell-Kommandotolken eller i skript.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="aa1c5-105">Den här guiden information med hjälp av PowerShell toocreate och virtuella Azure-datorn kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-105">This guide details using PowerShell toocreate and Azure virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="aa1c5-106">När installationen är klar använder vi ansluter toohello server och installera IIS.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>  

<span data-ttu-id="aa1c5-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="aa1c5-108">Denna Snabbstart kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="aa1c5-109">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="aa1c5-110">Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="aa1c5-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="aa1c5-111">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="aa1c5-111">Log in tooAzure</span></span>

<span data-ttu-id="aa1c5-112">Logga in tooyour Azure-prenumeration med hello `Login-AzureRmAccount` kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-112">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="aa1c5-113">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="aa1c5-113">Create resource group</span></span>

<span data-ttu-id="aa1c5-114">Skapa en Azure-resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="aa1c5-114">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="aa1c5-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a><span data-ttu-id="aa1c5-116">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="aa1c5-116">Create networking resources</span></span>

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a><span data-ttu-id="aa1c5-117">Skapa ett virtuellt nätverk, undernät och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-117">Create a virtual network, subnet, and a public IP address.</span></span> 
<span data-ttu-id="aa1c5-118">Dessa resurser är används tooprovide network connectivity toohello virtuell dator och koppla den toohello internet.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-118">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location EastUS `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a><span data-ttu-id="aa1c5-119">Skapa en nätverkssäkerhetsgrupp och en regel för nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-119">Create a network security group and a network security group rule.</span></span> 
<span data-ttu-id="aa1c5-120">Hej nätverkssäkerhetsgruppen säkrar hello virtuell dator med regler för inkommande och utgående.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-120">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="aa1c5-121">I detta fall skapas en regel för inkommande trafik för port 3389, som tillåter inkommande anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-121">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span> <span data-ttu-id="aa1c5-122">Vi vill även toocreate en regel för inkommande trafik för port 80, vilket gör att inkommande webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-122">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location EastUS `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-hello-virtual-machine"></a><span data-ttu-id="aa1c5-123">Skapa ett nätverkskort för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-123">Create a network card for hello virtual machine.</span></span> 
<span data-ttu-id="aa1c5-124">Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-124">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="aa1c5-125">hello nätverkskortet ansluter hello virtuella tooa undernät, nätverkssäkerhetsgrupp och offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-125">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="aa1c5-126">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="aa1c5-126">Create virtual machine</span></span>

<span data-ttu-id="aa1c5-127">Skapa en virtuell datorkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-127">Create a virtual machine configuration.</span></span> <span data-ttu-id="aa1c5-128">Den här konfigurationen innehåller hello-inställningar som används när du distribuerar hello virtuell dator till exempel en bild, storlek och autentisering konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-128">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span> <span data-ttu-id="aa1c5-129">När du kör det här steget uppmanas du att ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-129">When running this step, you are prompted for credentials.</span></span> <span data-ttu-id="aa1c5-130">hello-värden som du anger är konfigurerade som hello användarnamn och lösenord för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-130">hello values that you enter are configured as hello user name and password for hello virtual machine.</span></span>

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

<span data-ttu-id="aa1c5-131">Skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="aa1c5-131">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="aa1c5-132">Ansluta toovirtual datorn</span><span class="sxs-lookup"><span data-stu-id="aa1c5-132">Connect toovirtual machine</span></span>

<span data-ttu-id="aa1c5-133">Skapa en anslutning till fjärrskrivbord med hello virtuella datorn när hello distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-133">After hello deployment has completed, create a remote desktop connection with hello virtual machine.</span></span>

<span data-ttu-id="aa1c5-134">Använd hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) kommandot tooreturn hello offentliga IP-adressen för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-134">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span> <span data-ttu-id="aa1c5-135">Anteckna denna IP-adress så kan du ansluta tooit med din webbläsare tootest webbanslutningen i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-135">Take note of this IP Address so you can connect tooit with your browser tootest web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="aa1c5-136">Använd hello följande kommando toocreate en fjärrskrivbordssession med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-136">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="aa1c5-137">Ersätt hello IP-adress med hello *publicIPAddress* i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-137">Replace hello IP address with hello *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="aa1c5-138">När du uppmanas ange hello-autentiseringsuppgifter som används när du skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-138">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a><span data-ttu-id="aa1c5-139">Installera IIS via PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa1c5-139">Install IIS via PowerShell</span></span>

<span data-ttu-id="aa1c5-140">Nu när du har loggat in toohello Azure VM, kan du använda en rad med PowerShell tooinstall IIS och aktivera hello lokala brandväggen regeln tooallow webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-140">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="aa1c5-141">Öppna en PowerShell-kommandotolk och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="aa1c5-141">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="aa1c5-142">Visa hello välkomstsidan för IIS</span><span class="sxs-lookup"><span data-stu-id="aa1c5-142">View hello IIS welcome page</span></span>

<span data-ttu-id="aa1c5-143">Du kan använda en webbläsare för din choice tooview hello IIS välkomstsidan med IIS installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-143">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="aa1c5-144">Vara säker på att toouse hello *publicIpAddress* du dokumenterade ovan toovisit hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-144">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="aa1c5-146">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="aa1c5-146">Clean up resources</span></span>

<span data-ttu-id="aa1c5-147">När du inte längre behövs kan du använda hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-147">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="aa1c5-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa1c5-148">Next steps</span></span>

<span data-ttu-id="aa1c5-149">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-149">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="aa1c5-150">toolearn mer om Azure-datorer, fortsätta toohello självstudier för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="aa1c5-150">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="aa1c5-151">Självstudier om virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="aa1c5-151">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
