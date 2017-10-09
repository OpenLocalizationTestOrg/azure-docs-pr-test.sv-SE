---
title: aaaAzure Snabbstart - skapa VM PowerShell | Microsoft Docs
description: "Lär dig snabbt toocreate en Linux virtuella datorer med PowerShell"
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
ms.openlocfilehash: f05ea7fedafe4fda660dc6084ae57ebf9dced473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a><span data-ttu-id="3edf3-103">Skapa en virtuell Linux-dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="3edf3-103">Create a Linux virtual machine with PowerShell</span></span>

<span data-ttu-id="3edf3-104">hello Azure PowerShell-modulen är används toocreate och hantera Azure-resurser från hello PowerShell-Kommandotolken eller i skript.</span><span class="sxs-lookup"><span data-stu-id="3edf3-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="3edf3-105">Den här guiden information med hjälp av hello Azure PowerShell-modulen toodeploy en virtuell dator som kör Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="3edf3-105">This guide details using hello Azure PowerShell module toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="3edf3-106">När hello server har distribuerats, en SSH-anslutning skapas och en NGINX-webbserver är installerad.</span><span class="sxs-lookup"><span data-stu-id="3edf3-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="3edf3-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="3edf3-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="3edf3-108">Denna Snabbstart kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3edf3-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="3edf3-109">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="3edf3-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="3edf3-110">Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="3edf3-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="3edf3-111">Slutligen en offentlig SSH-nyckel med namnet hello *id_rsa.pub* måste lagras i hello toobe *.ssh* på din Windows-användarprofil.</span><span class="sxs-lookup"><span data-stu-id="3edf3-111">Finally, a public SSH key with hello name *id_rsa.pub* needs toobe stored in hello *.ssh* directory of your Windows user profile.</span></span> <span data-ttu-id="3edf3-112">Mer detaljerad information om hur du skapar SSH-nycklar för Azure finns i [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Skapa SSH-nycklar för Azure).</span><span class="sxs-lookup"><span data-stu-id="3edf3-112">For detailed information on creating SSH keys for Azure, see [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="3edf3-113">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="3edf3-113">Log in tooAzure</span></span>

<span data-ttu-id="3edf3-114">Logga in tooyour Azure-prenumeration med hello `Login-AzureRmAccount` kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="3edf3-114">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="3edf3-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3edf3-115">Create resource group</span></span>

<span data-ttu-id="3edf3-116">Skapa en Azure-resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="3edf3-116">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="3edf3-117">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="3edf3-117">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a><span data-ttu-id="3edf3-118">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="3edf3-118">Create networking resources</span></span>

<span data-ttu-id="3edf3-119">Skapa ett virtuellt nätverk, undernät och offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="3edf3-119">Create a virtual network, subnet, and a public IP address.</span></span> <span data-ttu-id="3edf3-120">Dessa resurser är används tooprovide network connectivity toohello virtuell dator och koppla den toohello internet.</span><span class="sxs-lookup"><span data-stu-id="3edf3-120">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

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

<span data-ttu-id="3edf3-121">Skapa en nätverkssäkerhetsgrupp och en regel för nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="3edf3-121">Create a network security group and a network security group rule.</span></span> <span data-ttu-id="3edf3-122">Hej nätverkssäkerhetsgruppen säkrar hello virtuell dator med regler för inkommande och utgående.</span><span class="sxs-lookup"><span data-stu-id="3edf3-122">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="3edf3-123">I detta fall skapas en regel för inkommande trafik för port 22, som tillåter inkommande SSH-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="3edf3-123">In this case, an inbound rule is created for port 22, which allows incoming SSH connections.</span></span> <span data-ttu-id="3edf3-124">Vi vill även toocreate en regel för inkommande trafik för port 80, vilket gör att inkommande webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="3edf3-124">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

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

<span data-ttu-id="3edf3-125">Skapa ett nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3edf3-125">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="3edf3-126">hello nätverkskortet ansluter hello virtuella tooa undernät, nätverkssäkerhetsgrupp och offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="3edf3-126">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="3edf3-127">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3edf3-127">Create virtual machine</span></span>

<span data-ttu-id="3edf3-128">Skapa en virtuell datorkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="3edf3-128">Create a virtual machine configuration.</span></span> <span data-ttu-id="3edf3-129">Den här konfigurationen innehåller hello-inställningar som används när du distribuerar hello virtuell dator till exempel en bild, storlek och autentisering konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3edf3-129">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span>

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

<span data-ttu-id="3edf3-130">Skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="3edf3-130">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="3edf3-131">Ansluta toovirtual datorn</span><span class="sxs-lookup"><span data-stu-id="3edf3-131">Connect toovirtual machine</span></span>

<span data-ttu-id="3edf3-132">Skapa en SSH-anslutning med hello virtuella datorn när hello distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3edf3-132">After hello deployment has completed, create an SSH connection with hello virtual machine.</span></span>

<span data-ttu-id="3edf3-133">Använd hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) kommandot tooreturn hello offentliga IP-adressen för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3edf3-133">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="3edf3-134">Använda hello följande kommando från ett system med SSH installerat tooconnect toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3edf3-134">From a system with SSH installed, used hello following command tooconnect toohello virtual machine.</span></span> <span data-ttu-id="3edf3-135">Om du arbetar med Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) kan vara används toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="3edf3-135">If working on Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) can be used toocreate hello connection.</span></span> 

```bash 
ssh <Public IP Address>
```

<span data-ttu-id="3edf3-136">När du uppmanas hello användarnamn är *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="3edf3-136">When prompted, hello login user name is *azureuser*.</span></span> <span data-ttu-id="3edf3-137">Om en lösenfras angavs när du skapar SSH-nycklar måste det här samt tooenter.</span><span class="sxs-lookup"><span data-stu-id="3edf3-137">If a passphrase was entered when creating SSH keys, you need tooenter this as well.</span></span>


## <a name="install-nginx"></a><span data-ttu-id="3edf3-138">Installera NGINX</span><span class="sxs-lookup"><span data-stu-id="3edf3-138">Install NGINX</span></span>

<span data-ttu-id="3edf3-139">Använd hello följande bash skriptet tooupdate paketet källor och installera hello senaste NGINX-paketet.</span><span class="sxs-lookup"><span data-stu-id="3edf3-139">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a><span data-ttu-id="3edf3-140">Visa hello NGIX välkomstsidan</span><span class="sxs-lookup"><span data-stu-id="3edf3-140">View hello NGIX welcome page</span></span>

<span data-ttu-id="3edf3-141">Du kan använda en webbläsare för dina val tooview hello NGINX välkomstsidan med NGINX installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="3edf3-141">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="3edf3-142">Vara säker på att toouse hello offentliga IP-adressen du dokumenterade över toovisit hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="3edf3-142">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![NGINX-standardwebbplats](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="3edf3-144">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="3edf3-144">Clean up resources</span></span>

<span data-ttu-id="3edf3-145">När du inte längre behövs kan du använda hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3edf3-145">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="3edf3-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3edf3-146">Next steps</span></span>

<span data-ttu-id="3edf3-147">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="3edf3-147">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="3edf3-148">toolearn mer om Azure-datorer, fortsätta toohello självstudier för Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3edf3-148">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3edf3-149">Självstudier om virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="3edf3-149">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
