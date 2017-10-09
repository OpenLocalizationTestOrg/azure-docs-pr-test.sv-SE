---
title: aaaAzure PowerShell skriptexempel - skapa en Linux VM | Microsoft Docs
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
ms.openlocfilehash: 8ee2ee42aa68ee135a859b7eaead25811cf54095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-powershell"></a><span data-ttu-id="7c232-103">Skapa en helt konfigurerade virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c232-103">Create a fully configured virtual machine with PowerShell</span></span>

<span data-ttu-id="7c232-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="7c232-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="7c232-105">Du kan komma åt hello virtuella datorn när du har kört hello skript via SSH.</span><span class="sxs-lookup"><span data-stu-id="7c232-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7c232-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7c232-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.ps1 "Create VM detailed")]

## <a name="clean-up-deployment"></a><span data-ttu-id="7c232-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="7c232-107">Clean up deployment</span></span> 

<span data-ttu-id="7c232-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="7c232-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="7c232-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7c232-109">Script explanation</span></span>

<span data-ttu-id="7c232-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="7c232-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="7c232-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7c232-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7c232-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="7c232-112">Command</span></span> | <span data-ttu-id="7c232-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7c232-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7c232-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7c232-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="7c232-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="7c232-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7c232-116">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="7c232-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="7c232-117">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="7c232-117">Creates a subnet configuration.</span></span> <span data-ttu-id="7c232-118">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7c232-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="7c232-119">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="7c232-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="7c232-120">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7c232-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="7c232-121">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="7c232-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="7c232-122">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7c232-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="7c232-123">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="7c232-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="7c232-124">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7c232-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="7c232-125">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="7c232-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="7c232-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="7c232-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="7c232-127">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="7c232-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="7c232-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="7c232-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="7c232-129">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="7c232-129">Gets subnet information.</span></span> <span data-ttu-id="7c232-130">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7c232-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="7c232-131">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="7c232-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="7c232-132">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7c232-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="7c232-133">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="7c232-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="7c232-134">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7c232-134">Creates a VM configuration.</span></span> <span data-ttu-id="7c232-135">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7c232-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="7c232-136">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7c232-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="7c232-137">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="7c232-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="7c232-138">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7c232-138">Create a virtual machine.</span></span> |
|[<span data-ttu-id="7c232-139">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7c232-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="7c232-140">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="7c232-140">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7c232-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c232-141">Next steps</span></span>

<span data-ttu-id="7c232-142">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7c232-142">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7c232-143">Ytterligare virtuella PowerShell-skript-exempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7c232-143">Additional virtual machine PowerShell script samples can be found in hello [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
