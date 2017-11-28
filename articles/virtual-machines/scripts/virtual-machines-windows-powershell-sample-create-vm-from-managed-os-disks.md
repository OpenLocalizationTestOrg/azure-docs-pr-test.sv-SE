---
title: aaaAzure PowerShell skriptexempel - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk | Microsoft Docs
description: Azure PowerShell-skript Sample - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8ae5b14df3977a4af91b92692cb925199cfb8058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a><span data-ttu-id="a7964-103">Skapa en virtuell dator med en befintlig OS-disk hanterade med PowerShell</span><span class="sxs-lookup"><span data-stu-id="a7964-103">Create a virtual machine using an existing managed OS disk with PowerShell</span></span>

<span data-ttu-id="a7964-104">Det här skriptet skapar en virtuell dator genom att koppla en befintlig hanterade disk som OS-disken.</span><span class="sxs-lookup"><span data-stu-id="a7964-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="a7964-105">Använd det här skriptet i föregående scenarier:</span><span class="sxs-lookup"><span data-stu-id="a7964-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="a7964-106">Skapa en virtuell dator från en befintlig hanterade OS-disk som har kopierats från en hanterad disk i en annan prenumeration</span><span class="sxs-lookup"><span data-stu-id="a7964-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="a7964-107">Skapa en virtuell dator från en befintlig hanterade disk som har skapats från en särskild VHD-fil</span><span class="sxs-lookup"><span data-stu-id="a7964-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="a7964-108">Skapa en virtuell dator från en befintlig hanterade OS-disk som har skapats från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="a7964-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a7964-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a7964-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a7964-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a7964-110">Clean up deployment</span></span> 

<span data-ttu-id="a7964-111">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="a7964-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a7964-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a7964-112">Script explanation</span></span>

<span data-ttu-id="a7964-113">Det här skriptet använder hello följande kommandon tooget hanteras diskegenskaper, bifoga en hanterade diskar tooa ny virtuell dator och skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a7964-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="a7964-114">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a7964-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a7964-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="a7964-115">Command</span></span> | <span data-ttu-id="a7964-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a7964-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a7964-117">Get-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="a7964-117">Get-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Get-AzureRmDisk) | <span data-ttu-id="a7964-118">Hämtar disk objekt baserat på hello namn och hello resursgruppen för en disk.</span><span class="sxs-lookup"><span data-stu-id="a7964-118">Gets disk object based on hello name and hello resource group of a disk.</span></span> <span data-ttu-id="a7964-119">ID-egenskapen för hello returnerade diskobjektet är används tooattach hello disk tooa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a7964-119">Id property of hello returned disk object is used tooattach hello disk tooa new VM</span></span> |
| [<span data-ttu-id="a7964-120">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="a7964-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="a7964-121">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7964-121">Creates a VM configuration.</span></span> <span data-ttu-id="a7964-122">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a7964-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="a7964-123">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a7964-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="a7964-124">Ange AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="a7964-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="a7964-125">Bifogar en hanterade diskar med hello Id-egenskapen för hello disk som Operativsystemet disk tooa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a7964-125">Attaches a managed disk using hello Id property of hello disk as OS disk tooa new virtual machine</span></span> |
| [<span data-ttu-id="a7964-126">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="a7964-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="a7964-127">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a7964-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="a7964-128">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="a7964-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="a7964-129">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a7964-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="a7964-130">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="a7964-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="a7964-131">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a7964-131">Create a virtual machine.</span></span> |
|[<span data-ttu-id="a7964-132">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a7964-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="a7964-133">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="a7964-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a7964-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7964-134">Next steps</span></span>

<span data-ttu-id="a7964-135">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a7964-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a7964-136">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a7964-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
