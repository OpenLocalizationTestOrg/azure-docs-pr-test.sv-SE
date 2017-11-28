---
title: Anpassa en Windows-dator i Azure | Microsoft Docs
description: "Lär dig hur du använder tillägget för anpassat skript och Key Vault för att anpassa virtuella Windows-datorer i Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 3be58bf8afbcff018b2b0d69a0e08c2c9ab1fca7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="2fba3-103">Hur du anpassar en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="2fba3-103">How to customize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="2fba3-104">Om du vill konfigurera virtuella datorer (VM) på ett snabbt och konsekvent sätt önskade vanligtvis någon form av automatisering.</span><span class="sxs-lookup"><span data-stu-id="2fba3-104">To configure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="2fba3-105">Ett vanligt sätt att anpassa en virtuell Windows-dator är att använda [anpassade skript tillägget för Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="2fba3-105">A common approach to customize a Windows VM is to use [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="2fba3-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="2fba3-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fba3-107">Använda tillägget för anpassat skript för att installera IIS</span><span class="sxs-lookup"><span data-stu-id="2fba3-107">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="2fba3-108">Skapa en virtuell dator som använder tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2fba3-108">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="2fba3-109">Visa en IIS-webbplats som körs när tillägget används</span><span class="sxs-lookup"><span data-stu-id="2fba3-109">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="2fba3-110">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2fba3-110">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="2fba3-111">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="2fba3-111">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="2fba3-112">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="2fba3-112">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="2fba3-113">Översikt över tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2fba3-113">Custom script extension overview</span></span>
<span data-ttu-id="2fba3-114">Tillägget för anpassat skript hämtar och kör skript på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="2fba3-114">The Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="2fba3-115">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2fba3-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="2fba3-116">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls på Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="2fba3-116">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span>

<span data-ttu-id="2fba3-117">Tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hjälp av Azure CLI, PowerShell, Azure-portalen eller Azure Virtual Machine REST API.</span><span class="sxs-lookup"><span data-stu-id="2fba3-117">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="2fba3-118">Du kan använda tillägget för anpassat skript med Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2fba3-118">You can use the Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="2fba3-119">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2fba3-119">Create virtual machine</span></span>
<span data-ttu-id="2fba3-120">Innan du kan skapa en virtuell dator, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2fba3-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2fba3-121">I följande exempel skapas en resursgrupp med namnet *myResourceGroupAutomate* i den *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="2fba3-121">The following example creates a resource group named *myResourceGroupAutomate* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="2fba3-122">Ange en administratör användarnamn och lösenord för de virtuella datorerna med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="2fba3-122">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="2fba3-123">Nu kan du skapa den virtuella datorn med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="2fba3-123">Now you can create the VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="2fba3-124">I följande exempel skapar krävs virtuella nätverkskomponenter, OS-konfigurationen och sedan skapar en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="2fba3-124">The following example creates the required virtual network components, the OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

<span data-ttu-id="2fba3-125">Det tar några minuter för de resurser och den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="2fba3-125">It takes a few minutes for the resources and VM to be created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="2fba3-126">Automatisera installationen av IIS</span><span class="sxs-lookup"><span data-stu-id="2fba3-126">Automate IIS install</span></span>
<span data-ttu-id="2fba3-127">Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) att installera tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="2fba3-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the Custom Script Extension.</span></span> <span data-ttu-id="2fba3-128">Tillägget körs `powershell Add-WindowsFeature Web-Server` att installera IIS-webbserver och uppdateringar av *Default.htm* sidan för att visa värdnamnet för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="2fba3-128">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the *Default.htm* page to show the hostname of the VM:</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a><span data-ttu-id="2fba3-129">Test-webbplats</span><span class="sxs-lookup"><span data-stu-id="2fba3-129">Test web site</span></span>
<span data-ttu-id="2fba3-130">Hämta offentlig IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2fba3-130">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="2fba3-131">I följande exempel hämtar IP-adressen för *myPublicIP* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="2fba3-131">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="2fba3-132">Du kan sedan ange den offentliga IP-adressen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2fba3-132">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="2fba3-133">Webbplatsen visas, inklusive värdnamnet för den virtuella datorn som belastningsutjämnaren distribuerade trafik till som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fba3-133">The website is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Kör IIS-webbplats](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="2fba3-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fba3-135">Next steps</span></span>

<span data-ttu-id="2fba3-136">I den här självstudiekursen automatiskt installera IIS på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2fba3-136">In this tutorial, you automated the IIS install on a VM.</span></span> <span data-ttu-id="2fba3-137">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="2fba3-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fba3-138">Använda tillägget för anpassat skript för att installera IIS</span><span class="sxs-lookup"><span data-stu-id="2fba3-138">Use the Custom Script Extension to install IIS</span></span>
> * <span data-ttu-id="2fba3-139">Skapa en virtuell dator som använder tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2fba3-139">Create a VM that uses the Custom Script Extension</span></span>
> * <span data-ttu-id="2fba3-140">Visa en IIS-webbplats som körs när tillägget används</span><span class="sxs-lookup"><span data-stu-id="2fba3-140">View a running IIS site after the extension is applied</span></span>

<span data-ttu-id="2fba3-141">Gå vidare till nästa kurs att lära dig hur du skapar anpassade VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="2fba3-141">Advance to the next tutorial to learn how to create custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2fba3-142">Skapa anpassade avbildningar av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2fba3-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
