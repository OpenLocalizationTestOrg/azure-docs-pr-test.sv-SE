---
title: "Azure snabbstart – skapa virtuell Windows-dator med PowerShell | Microsoft Docs"
description: "Lär dig att skapa en virtuell Windows-dator med PowerShell"
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
ms.openlocfilehash: 8516cfa2272694496eb353a83eca77c13a516750
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell"></a><span data-ttu-id="8c2b7-103">Skapa en virtuell Windows-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c2b7-103">Create a Windows virtual machine with PowerShell</span></span>

<span data-ttu-id="8c2b7-104">Azure PowerShell-modulen används för att skapa och hantera Azure-resurser från PowerShell-kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-104">The Azure PowerShell module is used to create and manage Azure resources from the PowerShell command line or in scripts.</span></span> <span data-ttu-id="8c2b7-105">Den här guiden beskriver hur man använder PowerShell för att skapa en virtuell Azure-dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-105">This guide details using PowerShell to create and Azure virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="8c2b7-106">När distributionen är klar kan vi ansluta till servern och installera IIS.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-106">Once deployment is complete, we connect to the server and install IIS.</span></span>  

<span data-ttu-id="8c2b7-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="8c2b7-108">Den här snabbstarten kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-108">This quick start requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="8c2b7-109">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-109">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="8c2b7-110">Om du behöver installera eller uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul).</span><span class="sxs-lookup"><span data-stu-id="8c2b7-110">If you need to install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="8c2b7-111">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="8c2b7-111">Log in to Azure</span></span>

<span data-ttu-id="8c2b7-112">Logga in på Azure-prenumerationen med kommandot `Login-AzureRmAccount` och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-112">Log in to your Azure subscription with the `Login-AzureRmAccount` command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="8c2b7-113">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="8c2b7-113">Create resource group</span></span>

<span data-ttu-id="8c2b7-114">Skapa en Azure-resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="8c2b7-114">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="8c2b7-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a><span data-ttu-id="8c2b7-116">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="8c2b7-116">Create networking resources</span></span>

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a><span data-ttu-id="8c2b7-117">Skapa ett virtuellt nätverk, undernät och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-117">Create a virtual network, subnet, and a public IP address.</span></span> 
<span data-ttu-id="8c2b7-118">Dessa resurser används för att skapa nätverksanslutning till den virtuella datorn och ansluta den till internet.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-118">These resources are used to provide network connectivity to the virtual machine and connect it to the internet.</span></span>

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

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a><span data-ttu-id="8c2b7-119">Skapa en nätverkssäkerhetsgrupp och en regel för nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-119">Create a network security group and a network security group rule.</span></span> 
<span data-ttu-id="8c2b7-120">Nätverkssäkerhetsgruppen skyddar den virtuella datorn med hjälp av regler för inkommande och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-120">The network security group secures the virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="8c2b7-121">I detta fall skapas en regel för inkommande trafik för port 3389, som tillåter inkommande anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-121">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span> <span data-ttu-id="8c2b7-122">Vi vill också skapa en regel för inkommande trafik för port 80, som tillåter inkommande webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-122">We also want to create an inbound rule for port 80, which allows incoming web traffic.</span></span>

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

### <a name="create-a-network-card-for-the-virtual-machine"></a><span data-ttu-id="8c2b7-123">Skapa ett nätverkskort för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-123">Create a network card for the virtual machine.</span></span> 
<span data-ttu-id="8c2b7-124">Skapa ett nätverkskort med [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-124">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for the virtual machine.</span></span> <span data-ttu-id="8c2b7-125">Nätverkskortet ansluter den virtuella datorn till ett undernät, en nätverkssäkerhetsgrupp och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-125">The network card connects the virtual machine to a subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="8c2b7-126">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8c2b7-126">Create virtual machine</span></span>

<span data-ttu-id="8c2b7-127">Skapa en virtuell datorkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-127">Create a virtual machine configuration.</span></span> <span data-ttu-id="8c2b7-128">Den här konfigurationen innehåller de inställningar som används vid distribution av den virtuella datorn, t.ex. den virtuella datorns avbildning, storlek och konfiguration för verifiering.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-128">This configuration includes the settings that are used when deploying the virtual machine such as a virtual machine image, size, and authentication configuration.</span></span> <span data-ttu-id="8c2b7-129">När du kör det här steget uppmanas du att ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-129">When running this step, you are prompted for credentials.</span></span> <span data-ttu-id="8c2b7-130">De värden som du anger konfigureras som användarnamn och lösenord för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-130">The values that you enter are configured as the user name and password for the virtual machine.</span></span>

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

<span data-ttu-id="8c2b7-131">Skapa den virtuella datorn med [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="8c2b7-131">Create the virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a><span data-ttu-id="8c2b7-132">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8c2b7-132">Connect to virtual machine</span></span>

<span data-ttu-id="8c2b7-133">När distributionen har slutförts kan du skapa en fjärrskrivbordsanslutning med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-133">After the deployment has completed, create a remote desktop connection with the virtual machine.</span></span>

<span data-ttu-id="8c2b7-134">Använd kommandot [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) för att returnera den offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-134">Use the [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command to return the public IP address of the virtual machine.</span></span> <span data-ttu-id="8c2b7-135">Anteckna denna IP-adress så att du kan ansluta till den till din webbläsare för att testa webbanslutningen i ett framtida steg.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-135">Take note of this IP Address so you can connect to it with your browser to test web connectivity in a future step.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="8c2b7-136">Använd följande kommando för att skapa en fjärrskrivbordssession med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-136">Use the following command to create a remote desktop session with the virtual machine.</span></span> <span data-ttu-id="8c2b7-137">Ersätt IP-adressen med *publicIPAddress* för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-137">Replace the IP address with the *publicIPAddress* of your virtual machine.</span></span> <span data-ttu-id="8c2b7-138">När du uppmanas att göra det anger du de autentiseringsuppgifter som användes när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-138">When prompted, enter the credentials used when creating the virtual machine.</span></span>

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a><span data-ttu-id="8c2b7-139">Installera IIS via PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c2b7-139">Install IIS via PowerShell</span></span>

<span data-ttu-id="8c2b7-140">Nu när du har loggat in till den virtuella Azure-datorn kan du använda en rad med PowerShell för att installera IIS och aktivera den lokala brandväggsregeln så att del tillåter webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-140">Now that you have logged in to the Azure VM, you can use a single line of PowerShell to install IIS and enable the local firewall rule to allow web traffic.</span></span> <span data-ttu-id="8c2b7-141">Öppna en PowerShell-kommandotolk och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8c2b7-141">Open a PowerShell prompt and run the following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a><span data-ttu-id="8c2b7-142">Visa välkomstsidan för IIS</span><span class="sxs-lookup"><span data-stu-id="8c2b7-142">View the IIS welcome page</span></span>

<span data-ttu-id="8c2b7-143">Du kan använda en webbläsare som du själv väljer för att visa standardvälkomstsidan för IIS när IIS är installerat och port 80 nu är öppen på den virtuella datorn från Internet.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-143">With IIS installed and port 80 now open on your VM from the Internet, you can use a web browser of your choice to view the default IIS welcome page.</span></span> <span data-ttu-id="8c2b7-144">Se till att använda den *publicIpAddress* som du har dokumenterat ovan för att besöka standardsidan.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-144">Be sure to use the *publicIpAddress* you documented above to visit the default page.</span></span> 

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="8c2b7-146">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="8c2b7-146">Clean up resources</span></span>

<span data-ttu-id="8c2b7-147">När den inte längre behövs du använda kommandot [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) för att ta bort resursgruppen, den virtuella datorn och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-147">When no longer needed, you can use the [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="8c2b7-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c2b7-148">Next steps</span></span>

<span data-ttu-id="8c2b7-149">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-149">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="8c2b7-150">Om du vill veta mer om virtuella Azure-datorer fortsätter du till självstudien för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="8c2b7-150">To learn more about Azure virtual machines, continue to the tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8c2b7-151">Självstudier om virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="8c2b7-151">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
