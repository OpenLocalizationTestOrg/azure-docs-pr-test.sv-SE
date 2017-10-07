---
title: aaaAzure PowerShell skriptexempel - IIS | Microsoft Docs
description: Azure PowerShell skriptexempel - IIS
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 707563e3cce32e667c41ab9ec2392c8f12ef116c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iis-vm-with-powershell"></a><span data-ttu-id="0d7f9-103">Skapa en virtuell dator i IIS med PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d7f9-103">Create an IIS VM with PowerShell</span></span>

<span data-ttu-id="0d7f9-104">Det här skriptet skapar en Azure-dator som kör Windows Server 2016 och använder sedan hello Azure virtuella tillägget för anpassat skript tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-104">This script creates an Azure Virtual Machine running Windows Server 2016, and then uses hello Azure Virtual Machine Custom Script Extension tooinstall IIS.</span></span> <span data-ttu-id="0d7f9-105">Du kan komma åt hello IIS-standardwebbplatsen på hello offentliga IP-adress hello virtuella datorn när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-105">After running hello script, you can access hello default IIS website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="0d7f9-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="0d7f9-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0d7f9-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="0d7f9-107">Clean up deployment</span></span> 

<span data-ttu-id="0d7f9-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="0d7f9-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0d7f9-109">Script explanation</span></span>

<span data-ttu-id="0d7f9-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="0d7f9-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0d7f9-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="0d7f9-112">Command</span></span> | <span data-ttu-id="0d7f9-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0d7f9-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0d7f9-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0d7f9-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="0d7f9-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0d7f9-116">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="0d7f9-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="0d7f9-117">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-117">Creates a subnet configuration.</span></span> <span data-ttu-id="0d7f9-118">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="0d7f9-119">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="0d7f9-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="0d7f9-120">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="0d7f9-121">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="0d7f9-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="0d7f9-122">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="0d7f9-123">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="0d7f9-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="0d7f9-124">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="0d7f9-125">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="0d7f9-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="0d7f9-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="0d7f9-127">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="0d7f9-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="0d7f9-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="0d7f9-129">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-129">Gets subnet information.</span></span> <span data-ttu-id="0d7f9-130">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="0d7f9-131">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="0d7f9-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="0d7f9-132">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="0d7f9-133">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="0d7f9-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="0d7f9-134">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-134">Creates a VM configuration.</span></span> <span data-ttu-id="0d7f9-135">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="0d7f9-136">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="0d7f9-137">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="0d7f9-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="0d7f9-138">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-138">Create a virtual machine.</span></span> |
| [<span data-ttu-id="0d7f9-139">Ange AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="0d7f9-139">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="0d7f9-140">Lägg till en VM-tillägget toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-140">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="0d7f9-141">I det här exemplet är hello tillägget för anpassat skript används tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-141">In this sample, hello custom script extension is used tooinstall IIS.</span></span> |
|[<span data-ttu-id="0d7f9-142">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0d7f9-142">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="0d7f9-143">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="0d7f9-143">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0d7f9-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d7f9-144">Next steps</span></span>

<span data-ttu-id="0d7f9-145">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0d7f9-145">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="0d7f9-146">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0d7f9-146">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
