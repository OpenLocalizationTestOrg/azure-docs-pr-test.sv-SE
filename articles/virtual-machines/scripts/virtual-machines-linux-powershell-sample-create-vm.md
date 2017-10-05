---
title: Azure PowerShell-skript Sample - skapa en virtuell Linux-dator | Microsoft Docs
description: Azure PowerShell-skript Sample - skapa en virtuell Linux-dator
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2bf1031e5481bbb662873f57904e889a2908e692
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-powershell"></a><span data-ttu-id="00897-103">Skapa en helt konfigurerade virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="00897-103">Create a fully configured virtual machine with PowerShell</span></span>

<span data-ttu-id="00897-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="00897-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="00897-105">När skriptet har körts kan du komma åt den virtuella datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="00897-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="00897-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="00897-106">Sample script</span></span>

<span data-ttu-id="00897-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.ps1 "skapa VM detaljerad")]</span><span class="sxs-lookup"><span data-stu-id="00897-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.ps1 "Create VM detailed")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="00897-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="00897-108">Clean up deployment</span></span> 

<span data-ttu-id="00897-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="00897-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="00897-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="00897-110">Script explanation</span></span>

<span data-ttu-id="00897-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="00897-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="00897-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="00897-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="00897-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="00897-113">Command</span></span> | <span data-ttu-id="00897-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="00897-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="00897-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00897-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="00897-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="00897-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="00897-117">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="00897-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="00897-118">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="00897-118">Creates a subnet configuration.</span></span> <span data-ttu-id="00897-119">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="00897-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="00897-120">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="00897-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="00897-121">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="00897-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="00897-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="00897-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="00897-123">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="00897-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="00897-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="00897-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="00897-125">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="00897-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="00897-126">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="00897-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="00897-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="00897-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="00897-128">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="00897-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="00897-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="00897-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="00897-130">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="00897-130">Gets subnet information.</span></span> <span data-ttu-id="00897-131">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="00897-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="00897-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="00897-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="00897-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="00897-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="00897-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="00897-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="00897-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="00897-135">Creates a VM configuration.</span></span> <span data-ttu-id="00897-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="00897-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="00897-137">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="00897-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="00897-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="00897-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="00897-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="00897-139">Create a virtual machine.</span></span> |
|[<span data-ttu-id="00897-140">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00897-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="00897-141">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="00897-141">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="00897-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00897-142">Next steps</span></span>

<span data-ttu-id="00897-143">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="00897-143">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="00897-144">Ytterligare virtuella PowerShell-skript-exempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="00897-144">Additional virtual machine PowerShell script samples can be found in the [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
