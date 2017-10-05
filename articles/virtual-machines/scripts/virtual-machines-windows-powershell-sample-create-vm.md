---
title: Azure PowerShell-skript Sample - skapa en Windows VM | Microsoft Docs
description: Azure PowerShell-skript Sample - skapa en Windows VM
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
ms.date: 03/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a9e46aded0cf3792558a7fba6f00e5662166cc0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-powershell"></a><span data-ttu-id="33a21-103">Skapa en helt konfigurerade virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="33a21-103">Create a fully configured virtual machine with PowerShell</span></span>

<span data-ttu-id="33a21-104">Det här skriptet skapar en Azure-dator som kör Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="33a21-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="33a21-105">När skriptet har körts kan du komma åt den virtuella datorn via RDP.</span><span class="sxs-lookup"><span data-stu-id="33a21-105">After running the script, you can access the virtual machine over RDP.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="33a21-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="33a21-106">Sample script</span></span>

<span data-ttu-id="33a21-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.ps1 "skapa VM detaljerad")]</span><span class="sxs-lookup"><span data-stu-id="33a21-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.ps1 "Create VM detailed")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="33a21-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="33a21-108">Clean up deployment</span></span> 

<span data-ttu-id="33a21-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="33a21-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="33a21-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="33a21-110">Script explanation</span></span>

<span data-ttu-id="33a21-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="33a21-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="33a21-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="33a21-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="33a21-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="33a21-113">Command</span></span> | <span data-ttu-id="33a21-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="33a21-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="33a21-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="33a21-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="33a21-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="33a21-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="33a21-117">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="33a21-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="33a21-118">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="33a21-118">Creates a subnet configuration.</span></span> <span data-ttu-id="33a21-119">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="33a21-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="33a21-120">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="33a21-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="33a21-121">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="33a21-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="33a21-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="33a21-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="33a21-123">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="33a21-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="33a21-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="33a21-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="33a21-125">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="33a21-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="33a21-126">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="33a21-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="33a21-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="33a21-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="33a21-128">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="33a21-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="33a21-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="33a21-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="33a21-130">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="33a21-130">Gets subnet information.</span></span> <span data-ttu-id="33a21-131">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="33a21-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="33a21-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="33a21-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="33a21-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="33a21-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="33a21-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="33a21-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="33a21-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="33a21-135">Creates a VM configuration.</span></span> <span data-ttu-id="33a21-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="33a21-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="33a21-137">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="33a21-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="33a21-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="33a21-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="33a21-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="33a21-139">Create a virtual machine.</span></span> |
|[<span data-ttu-id="33a21-140">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="33a21-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="33a21-141">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="33a21-141">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="33a21-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33a21-142">Next steps</span></span>

<span data-ttu-id="33a21-143">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="33a21-143">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="33a21-144">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33a21-144">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
