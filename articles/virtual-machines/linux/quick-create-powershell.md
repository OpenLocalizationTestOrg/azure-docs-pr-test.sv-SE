---
title: "Azure snabbstart – skapa virtuell dator med PowerShell | Microsoft Docs"
description: "Lär dig att snabbt skapa en virtuell Linux-dator med PowerShell"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adcf492d4c2d935c880167675a1ed97566156ec7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a><span data-ttu-id="c6c6a-103">Skapa en virtuell Linux-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="c6c6a-103">Create a Linux virtual machine with PowerShell</span></span>

<span data-ttu-id="c6c6a-104">Azure PowerShell-modulen används för att skapa och hantera Azure-resurser från PowerShell-kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-104">The Azure PowerShell module is used to create and manage Azure resources from the PowerShell command line or in scripts.</span></span> <span data-ttu-id="c6c6a-105">Den här guiden beskriver hur man använder Azure PowerShell-modul för att distribuera en virtuell dator som kör Ubuntu-servern.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-105">This guide details using the Azure PowerShell module to deploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="c6c6a-106">När servern har distribuerats skapas en SSH-anslutning och en NGINX-webbserver installeras.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-106">Once the server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="c6c6a-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="c6c6a-108">Den här snabbstarten kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-108">This quick start requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="c6c6a-109">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-109">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="c6c6a-110">Om du behöver installera eller uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul).</span><span class="sxs-lookup"><span data-stu-id="c6c6a-110">If you need to install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="c6c6a-111">Slutligen måste du ha en offentlig SSH-nyckel med namnet *id_rsa.pub* i katalogen *.ssh* i din Windows-användarprofil.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-111">Finally, a public SSH key with the name *id_rsa.pub* needs to be stored in the *.ssh* directory of your Windows user profile.</span></span> <span data-ttu-id="c6c6a-112">Mer detaljerad information om hur du skapar SSH-nycklar för Azure finns i [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Skapa SSH-nycklar för Azure).</span><span class="sxs-lookup"><span data-stu-id="c6c6a-112">For detailed information on creating SSH keys for Azure, see [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="c6c6a-113">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="c6c6a-113">Log in to Azure</span></span>

<span data-ttu-id="c6c6a-114">Logga in på Azure-prenumerationen med kommandot `Login-AzureRmAccount` och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-114">Log in to your Azure subscription with the `Login-AzureRmAccount` command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="c6c6a-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c6c6a-115">Create resource group</span></span>

<span data-ttu-id="c6c6a-116">Skapa en Azure-resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="c6c6a-116">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="c6c6a-117">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-117">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a><span data-ttu-id="c6c6a-118">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="c6c6a-118">Create networking resources</span></span>

<span data-ttu-id="c6c6a-119">Skapa ett virtuellt nätverk, undernät och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-119">Create a virtual network, subnet, and a public IP address.</span></span> <span data-ttu-id="c6c6a-120">Dessa resurser används för att skapa nätverksanslutning till den virtuella datorn och ansluta den till internet.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-120">These resources are used to provide network connectivity to the virtual machine and connect it to the internet.</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

<span data-ttu-id="c6c6a-121">Skapa en nätverkssäkerhetsgrupp och en regel för nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-121">Create a network security group and a network security group rule.</span></span> <span data-ttu-id="c6c6a-122">Nätverkssäkerhetsgruppen skyddar den virtuella datorn med hjälp av regler för inkommande och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-122">The network security group secures the virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="c6c6a-123">I detta fall skapas en regel för inkommande trafik för port 22, som tillåter inkommande SSH-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-123">In this case, an inbound rule is created for port 22, which allows incoming SSH connections.</span></span> <span data-ttu-id="c6c6a-124">Vi vill också skapa en regel för inkommande trafik för port 80, som tillåter inkommande webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-124">We also want to create an inbound rule for port 80, which allows incoming web traffic.</span></span>

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

<span data-ttu-id="c6c6a-125">Skapa ett nätverkskort med [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-125">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for the virtual machine.</span></span> <span data-ttu-id="c6c6a-126">Nätverkskortet ansluter den virtuella datorn till ett undernät, en nätverkssäkerhetsgrupp och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-126">The network card connects the virtual machine to a subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="c6c6a-127">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c6c6a-127">Create virtual machine</span></span>

<span data-ttu-id="c6c6a-128">Skapa en virtuell datorkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-128">Create a virtual machine configuration.</span></span> <span data-ttu-id="c6c6a-129">Den här konfigurationen innehåller de inställningar som används vid distribution av den virtuella datorn, t.ex. den virtuella datorns avbildning, storlek och konfiguration för verifiering.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-129">This configuration includes the settings that are used when deploying the virtual machine such as a virtual machine image, size, and authentication configuration.</span></span>

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

<span data-ttu-id="c6c6a-130">Skapa den virtuella datorn med [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="c6c6a-130">Create the virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a><span data-ttu-id="c6c6a-131">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c6c6a-131">Connect to virtual machine</span></span>

<span data-ttu-id="c6c6a-132">När distributionen har slutförts kan du skapa en SSH-anslutning med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-132">After the deployment has completed, create an SSH connection with the virtual machine.</span></span>

<span data-ttu-id="c6c6a-133">Använd kommandot [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) för att returnera den offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-133">Use the [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command to return the public IP address of the virtual machine.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="c6c6a-134">Från en dator med SSH installerat använder du följande kommando för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-134">From a system with SSH installed, used the following command to connect to the virtual machine.</span></span> <span data-ttu-id="c6c6a-135">Om du arbetar med Windows kan du skapa anslutningen med hjälp av [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty).</span><span class="sxs-lookup"><span data-stu-id="c6c6a-135">If working on Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) can be used to create the connection.</span></span> 

```bash 
ssh <Public IP Address>
```

<span data-ttu-id="c6c6a-136">När du får en uppmaning är *azureuser* användarnamnet för inloggningen.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-136">When prompted, the login user name is *azureuser*.</span></span> <span data-ttu-id="c6c6a-137">Om du angav en lösenfras när du skapade SSH-nycklar måste du även ange den.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-137">If a passphrase was entered when creating SSH keys, you need to enter this as well.</span></span>


## <a name="install-nginx"></a><span data-ttu-id="c6c6a-138">Installera NGINX</span><span class="sxs-lookup"><span data-stu-id="c6c6a-138">Install NGINX</span></span>

<span data-ttu-id="c6c6a-139">Använd följande bash-skript för att uppdatera paketkällor och installera det senaste NGINX-paketet.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-139">Use the following bash script to update package sources and install the latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a><span data-ttu-id="c6c6a-140">Visa NGIX-välkomstsidan</span><span class="sxs-lookup"><span data-stu-id="c6c6a-140">View the NGIX welcome page</span></span>

<span data-ttu-id="c6c6a-141">Du kan använda en webbläsare som du väljer för att visa välkomstsidan till NGINX när NGINX är installerat och port 80 nu är öppen på en virtuell dator från Internet.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-141">With NGINX installed and port 80 now open on your VM from the Internet - you can use a web browser of your choice to view the default NGINX welcome page.</span></span> <span data-ttu-id="c6c6a-142">Se till att använda den offentliga IP-adress som du har dokumenterat ovan för att besöka standardsidan.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-142">Be sure to use the public IP address you documented above to visit the default page.</span></span> 

![NGINX-standardwebbplats](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="c6c6a-144">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="c6c6a-144">Clean up resources</span></span>

<span data-ttu-id="c6c6a-145">När den inte längre behövs du använda kommandot [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) för att ta bort resursgruppen, den virtuella datorn och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-145">When no longer needed, you can use the [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c6c6a-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6c6a-146">Next steps</span></span>

<span data-ttu-id="c6c6a-147">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-147">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="c6c6a-148">Om du vill veta mer om virtuella Azure-datorer fortsätter du till självstudien för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="c6c6a-148">To learn more about Azure virtual machines, continue to the tutorial for Linux VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c6c6a-149">Självstudier om virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="c6c6a-149">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
