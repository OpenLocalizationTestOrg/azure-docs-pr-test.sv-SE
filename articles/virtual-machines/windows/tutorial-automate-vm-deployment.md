---
title: aaaCustomize en Windows-dator i Azure | Microsoft Docs
description: "Lär dig hur toouse hello tillägget för anpassat skript och Key Vault toocustomize virtuella Windows-datorer i Azure"
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
ms.openlocfilehash: c03b2bb6d70875134c63ea2fe4c2e2c1777c2188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="2c005-103">Hur toocustomize en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="2c005-103">How toocustomize a Windows virtual machine in Azure</span></span>
<span data-ttu-id="2c005-104">är vanligtvis det önskade tooconfigure virtuella maskiner (VMs) på ett snabbt och konsekvent sätt någon form av automatisering.</span><span class="sxs-lookup"><span data-stu-id="2c005-104">tooconfigure virtual machines (VMs) in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="2c005-105">En gemensam metod toocustomize en Windows VM är toouse [anpassade skript tillägget för Windows](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="2c005-105">A common approach toocustomize a Windows VM is toouse [Custom Script Extension for Windows](extensions-customscript.md).</span></span> <span data-ttu-id="2c005-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="2c005-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c005-107">Använd hello tillägget för anpassat skript tooinstall IIS</span><span class="sxs-lookup"><span data-stu-id="2c005-107">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="2c005-108">Skapa en virtuell dator som använder hello tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2c005-108">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="2c005-109">Visa en IIS-webbplats som körs efter hello tillägget används</span><span class="sxs-lookup"><span data-stu-id="2c005-109">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="2c005-110">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2c005-110">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="2c005-111">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="2c005-111">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="2c005-112">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="2c005-112">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="custom-script-extension-overview"></a><span data-ttu-id="2c005-113">Översikt över tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2c005-113">Custom script extension overview</span></span>
<span data-ttu-id="2c005-114">hello tillägget för anpassat skript hämtar och kör skript på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="2c005-114">hello Custom Script Extension downloads and executes scripts on Azure VMs.</span></span> <span data-ttu-id="2c005-115">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c005-115">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="2c005-116">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="2c005-116">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span>

<span data-ttu-id="2c005-117">hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.</span><span class="sxs-lookup"><span data-stu-id="2c005-117">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="2c005-118">Du kan använda hello tillägget för anpassat skript med Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2c005-118">You can use hello Custom Script Extension with both Windows and Linux VMs.</span></span>


## <a name="create-virtual-machine"></a><span data-ttu-id="2c005-119">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2c005-119">Create virtual machine</span></span>
<span data-ttu-id="2c005-120">Innan du kan skapa en virtuell dator, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2c005-120">Before you can create a VM, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2c005-121">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAutomate* i hello *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="2c005-121">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

<span data-ttu-id="2c005-122">Ange en administratör användarnamn och lösenord för hello virtuella datorer med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="2c005-122">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="2c005-123">Nu kan du skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="2c005-123">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="2c005-124">hello följande exempel skapar hello krävs virtuella nätverkskomponenter hello OS-konfiguration, och skapar sedan en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="2c005-124">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="2c005-125">Det tar några minuter för hello resurser och VM-toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="2c005-125">It takes a few minutes for hello resources and VM toobe created.</span></span>


## <a name="automate-iis-install"></a><span data-ttu-id="2c005-126">Automatisera installationen av IIS</span><span class="sxs-lookup"><span data-stu-id="2c005-126">Automate IIS install</span></span>
<span data-ttu-id="2c005-127">Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="2c005-127">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="2c005-128">Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver och uppdateringar hello *Default.htm* sidan tooshow hello värdnamnet för hello VM:</span><span class="sxs-lookup"><span data-stu-id="2c005-128">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

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


## <a name="test-web-site"></a><span data-ttu-id="2c005-129">Test-webbplats</span><span class="sxs-lookup"><span data-stu-id="2c005-129">Test web site</span></span>
<span data-ttu-id="2c005-130">Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2c005-130">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="2c005-131">hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="2c005-131">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

<span data-ttu-id="2c005-132">Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2c005-132">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="2c005-133">hello webbplatsen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2c005-133">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Kör IIS-webbplats](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a><span data-ttu-id="2c005-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c005-135">Next steps</span></span>

<span data-ttu-id="2c005-136">I den här självstudiekursen automatiserad hello IIS installeras på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2c005-136">In this tutorial, you automated hello IIS install on a VM.</span></span> <span data-ttu-id="2c005-137">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="2c005-137">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c005-138">Använd hello tillägget för anpassat skript tooinstall IIS</span><span class="sxs-lookup"><span data-stu-id="2c005-138">Use hello Custom Script Extension tooinstall IIS</span></span>
> * <span data-ttu-id="2c005-139">Skapa en virtuell dator som använder hello tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="2c005-139">Create a VM that uses hello Custom Script Extension</span></span>
> * <span data-ttu-id="2c005-140">Visa en IIS-webbplats som körs efter hello tillägget används</span><span class="sxs-lookup"><span data-stu-id="2c005-140">View a running IIS site after hello extension is applied</span></span>

<span data-ttu-id="2c005-141">I förväg toohello nästa självstudiekurs toolearn hur toocreate anpassade VM-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="2c005-141">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2c005-142">Skapa anpassade avbildningar av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2c005-142">Create custom VM images</span></span>](./tutorial-custom-images.md)
