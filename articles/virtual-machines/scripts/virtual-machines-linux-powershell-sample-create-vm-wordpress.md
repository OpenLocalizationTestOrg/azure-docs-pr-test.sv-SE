---
title: Azure PowerShell skriptexempel - WordPress | Microsoft Docs
description: Azure PowerShell skriptexempel - WordPress
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
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 778a6d5cfc63f80aa66654d682fedb178cfd67a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-wordpress-vm-with-powershell"></a><span data-ttu-id="4b73e-103">Skapa en WordPress virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b73e-103">Create a WordPress VM with PowerShell</span></span>

<span data-ttu-id="4b73e-104">Det här skriptet skapar en virtuell dator och använder tillägget för anpassat skript för Azure virtuell dator för att installera WordPress.</span><span class="sxs-lookup"><span data-stu-id="4b73e-104">This script creates a virtual machine and uses the Azure Virtual Machine custom script extension to install WordPress.</span></span> <span data-ttu-id="4b73e-105">När skriptet har körts, du kan komma åt webbplatsen WordPress konfiguration på `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="4b73e-105">After running the script, you can access the WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4b73e-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4b73e-106">Sample script</span></span>

<span data-ttu-id="4b73e-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.ps1 "skapa VM WordPress")]</span><span class="sxs-lookup"><span data-stu-id="4b73e-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.ps1 "Create VM WordPress")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4b73e-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4b73e-108">Clean up deployment</span></span> 

<span data-ttu-id="4b73e-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="4b73e-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4b73e-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4b73e-110">Script explanation</span></span>

<span data-ttu-id="4b73e-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="4b73e-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="4b73e-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4b73e-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4b73e-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="4b73e-113">Command</span></span> | <span data-ttu-id="4b73e-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4b73e-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4b73e-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4b73e-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="4b73e-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4b73e-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4b73e-117">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="4b73e-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="4b73e-118">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="4b73e-118">Creates a subnet configuration.</span></span> <span data-ttu-id="4b73e-119">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4b73e-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="4b73e-120">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4b73e-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="4b73e-121">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4b73e-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="4b73e-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="4b73e-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="4b73e-123">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4b73e-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="4b73e-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="4b73e-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="4b73e-125">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4b73e-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="4b73e-126">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="4b73e-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="4b73e-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="4b73e-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="4b73e-128">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4b73e-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="4b73e-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="4b73e-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="4b73e-130">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="4b73e-130">Gets subnet information.</span></span> <span data-ttu-id="4b73e-131">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="4b73e-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="4b73e-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="4b73e-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="4b73e-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="4b73e-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="4b73e-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="4b73e-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="4b73e-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4b73e-135">Creates a VM configuration.</span></span> <span data-ttu-id="4b73e-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4b73e-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="4b73e-137">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4b73e-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="4b73e-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="4b73e-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="4b73e-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4b73e-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="4b73e-140">Ange AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="4b73e-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="4b73e-141">Lägg till tillägget för anpassat skript till den virtuella datorn som anropar ett skript för att installera WordPress.</span><span class="sxs-lookup"><span data-stu-id="4b73e-141">Add the Custom Script Extension to the virtual machine, which invokes a script to install WordPress.</span></span> |
|[<span data-ttu-id="4b73e-142">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4b73e-142">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="4b73e-143">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="4b73e-143">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4b73e-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b73e-144">Next steps</span></span>

<span data-ttu-id="4b73e-145">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4b73e-145">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="4b73e-146">Ytterligare virtuella PowerShell-skript-exempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4b73e-146">Additional virtual machine PowerShell script samples can be found in the [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
