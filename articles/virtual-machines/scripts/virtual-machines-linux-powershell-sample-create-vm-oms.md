---
title: aaaAzure PowerShell skriptexempel - OMS | Microsoft Docs
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
ms.openlocfilehash: 75bfa42b538d8f5f01bb24d507797470b9491602
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="d1963-103">Skapa en Operations Management Suite övervakas virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1963-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="d1963-104">Det här skriptet skapar en virtuell dator i Azure, hello Operations Management Suite (OMS) agent installeras och registrerar hello system med en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d1963-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="d1963-105">När hello skriptet har körts visas hello virtuell dator i hello OMS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d1963-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d1963-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d1963-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d1963-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d1963-107">Clean up deployment</span></span> 

<span data-ttu-id="d1963-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="d1963-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d1963-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d1963-109">Script explanation</span></span>

<span data-ttu-id="d1963-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="d1963-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="d1963-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d1963-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d1963-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="d1963-112">Command</span></span> | <span data-ttu-id="d1963-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d1963-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d1963-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1963-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d1963-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d1963-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d1963-116">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="d1963-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="d1963-117">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="d1963-117">Creates a subnet configuration.</span></span> <span data-ttu-id="d1963-118">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d1963-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="d1963-119">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="d1963-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="d1963-120">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d1963-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="d1963-121">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="d1963-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="d1963-122">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d1963-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="d1963-123">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="d1963-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="d1963-124">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d1963-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="d1963-125">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="d1963-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="d1963-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="d1963-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="d1963-127">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="d1963-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="d1963-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="d1963-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="d1963-129">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="d1963-129">Gets subnet information.</span></span> <span data-ttu-id="d1963-130">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d1963-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="d1963-131">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="d1963-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="d1963-132">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d1963-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="d1963-133">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="d1963-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="d1963-134">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d1963-134">Creates a VM configuration.</span></span> <span data-ttu-id="d1963-135">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d1963-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="d1963-136">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d1963-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="d1963-137">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="d1963-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="d1963-138">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d1963-138">Create a virtual machine.</span></span> |
| [<span data-ttu-id="d1963-139">Ange AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="d1963-139">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="d1963-140">Lägg till en VM-tillägget toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d1963-140">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="d1963-141">I det här fallet är används tooinstall hello OMS-agent hello Operations Management Suite-agenten tillägget och registrera hello VM i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d1963-141">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="d1963-142">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1963-142">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="d1963-143">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="d1963-143">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d1963-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1963-144">Next steps</span></span>

<span data-ttu-id="d1963-145">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d1963-145">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d1963-146">Ytterligare virtuella PowerShell-skript-exempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d1963-146">Additional virtual machine PowerShell script samples can be found in hello [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
