---
title: Azure PowerShell skriptexempel - OMS | Microsoft Docs
description: Azure PowerShell skriptexempel - OMS
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
ms.openlocfilehash: cd23f71221efb62d2547b2b683ca8e2218403a2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="129b9-103">Skapa en Operations Management Suite övervakas virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="129b9-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="129b9-104">Det här skriptet skapar en virtuell dator i Azure, installerar du agenten med Operations Management Suite (OMS) och registrerar system med en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="129b9-104">This script creates an Azure Virtual Machine, installs the Operations Management Suite (OMS) agent, and enrolls the system with an OMS workspace.</span></span> <span data-ttu-id="129b9-105">När skriptet har körts, visas den virtuella datorn i OMS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="129b9-105">Once the script has run, the virtual machine will be visible in the OMS console.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="129b9-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="129b9-106">Sample script</span></span>

<span data-ttu-id="129b9-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.ps1 "skapa VM OMS")]</span><span class="sxs-lookup"><span data-stu-id="129b9-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.ps1 "Create VM OMS")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="129b9-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="129b9-108">Clean up deployment</span></span> 

<span data-ttu-id="129b9-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="129b9-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="129b9-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="129b9-110">Script explanation</span></span>

<span data-ttu-id="129b9-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="129b9-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="129b9-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="129b9-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="129b9-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="129b9-113">Command</span></span> | <span data-ttu-id="129b9-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="129b9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="129b9-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="129b9-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="129b9-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="129b9-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="129b9-117">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="129b9-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="129b9-118">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="129b9-118">Creates a subnet configuration.</span></span> <span data-ttu-id="129b9-119">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="129b9-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="129b9-120">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="129b9-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="129b9-121">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="129b9-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="129b9-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="129b9-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="129b9-123">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="129b9-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="129b9-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="129b9-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="129b9-125">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="129b9-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="129b9-126">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="129b9-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="129b9-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="129b9-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="129b9-128">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="129b9-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="129b9-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="129b9-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="129b9-130">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="129b9-130">Gets subnet information.</span></span> <span data-ttu-id="129b9-131">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="129b9-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="129b9-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="129b9-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="129b9-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="129b9-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="129b9-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="129b9-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="129b9-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="129b9-135">Creates a VM configuration.</span></span> <span data-ttu-id="129b9-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="129b9-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="129b9-137">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="129b9-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="129b9-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="129b9-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="129b9-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="129b9-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="129b9-140">Ange AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="129b9-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="129b9-141">Lägg till en VM-tillägget till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="129b9-141">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="129b9-142">I det här fallet används filnamnstillägget Operations Management Suite-agenten för att installera OMS-agenten och registrera den virtuella datorn i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="129b9-142">In this case, the Operations Management Suite agent extension is used to install the OMS agent and enroll the VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="129b9-143">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="129b9-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="129b9-144">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="129b9-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="129b9-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="129b9-145">Next steps</span></span>

<span data-ttu-id="129b9-146">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="129b9-146">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="129b9-147">Ytterligare virtuella PowerShell-skript-exempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="129b9-147">Additional virtual machine PowerShell script samples can be found in the [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
