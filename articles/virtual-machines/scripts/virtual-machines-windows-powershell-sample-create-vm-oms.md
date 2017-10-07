---
title: aaaAzure PowerShell skriptexempel - OMS | Microsoft Docs
description: Azure PowerShell skriptexempel - OMS
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
ms.date: 03/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1eeafbe743013e97bf3fcefb5ce87f72cb503a4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="ffce2-103">Skapa en Operations Management Suite övervakas virtuell dator med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ffce2-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="ffce2-104">Det här skriptet skapar en virtuell dator i Azure, hello Operations Management Suite (OMS) agent installeras och registrerar hello system med en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ffce2-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="ffce2-105">När hello skriptet har körts visas hello virtuell dator i hello OMS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="ffce2-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span> <span data-ttu-id="ffce2-106">Du måste också tooupdate hello OMS arbetsytan ID och arbetsytenyckel.</span><span class="sxs-lookup"><span data-stu-id="ffce2-106">Also, you need tooupdate hello OMS workspace ID and workspace key.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ffce2-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ffce2-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ffce2-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ffce2-108">Clean up deployment</span></span> 

<span data-ttu-id="ffce2-109">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="ffce2-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ffce2-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ffce2-110">Script explanation</span></span>

<span data-ttu-id="ffce2-111">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="ffce2-111">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="ffce2-112">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ffce2-112">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ffce2-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="ffce2-113">Command</span></span> | <span data-ttu-id="ffce2-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ffce2-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ffce2-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ffce2-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="ffce2-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ffce2-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ffce2-117">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="ffce2-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="ffce2-118">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="ffce2-118">Creates a subnet configuration.</span></span> <span data-ttu-id="ffce2-119">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="ffce2-119">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="ffce2-120">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ffce2-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="ffce2-121">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ffce2-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="ffce2-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="ffce2-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="ffce2-123">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ffce2-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="ffce2-124">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="ffce2-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="ffce2-125">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ffce2-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="ffce2-126">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="ffce2-126">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="ffce2-127">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="ffce2-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="ffce2-128">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="ffce2-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="ffce2-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="ffce2-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="ffce2-130">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="ffce2-130">Gets subnet information.</span></span> <span data-ttu-id="ffce2-131">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ffce2-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="ffce2-132">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="ffce2-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="ffce2-133">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ffce2-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="ffce2-134">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="ffce2-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="ffce2-135">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ffce2-135">Creates a VM configuration.</span></span> <span data-ttu-id="ffce2-136">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ffce2-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="ffce2-137">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ffce2-137">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="ffce2-138">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="ffce2-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="ffce2-139">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ffce2-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="ffce2-140">Ange AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="ffce2-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="ffce2-141">Lägg till en VM-tillägget toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ffce2-141">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="ffce2-142">I det här fallet är används tooinstall hello OMS-agent hello Operations Management Suite-agenten tillägget och registrera hello VM i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ffce2-142">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="ffce2-143">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ffce2-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="ffce2-144">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="ffce2-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ffce2-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ffce2-145">Next steps</span></span>

<span data-ttu-id="ffce2-146">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ffce2-146">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="ffce2-147">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ffce2-147">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
