---
title: "aaaAzure PowerShell skriptexempel - skapa en virtuell dator från en ögonblicksbild | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en virtuell dator från en ögonblicksbild"
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
ms.openlocfilehash: 89c65171b55bff0582c4a26df0b0f29f556845fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a><span data-ttu-id="449f9-103">Skapa en virtuell dator från en ögonblicksbild med PowerShell</span><span class="sxs-lookup"><span data-stu-id="449f9-103">Create a virtual machine from a snapshot with PowerShell</span></span>

<span data-ttu-id="449f9-104">Det här skriptet skapar en virtuell dator från en ögonblicksbild av en OS-disk.</span><span class="sxs-lookup"><span data-stu-id="449f9-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="449f9-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="449f9-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="449f9-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="449f9-106">Clean up deployment</span></span> 

<span data-ttu-id="449f9-107">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="449f9-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="449f9-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="449f9-108">Script explanation</span></span>

<span data-ttu-id="449f9-109">Det här skriptet använder hello följande kommandon tooget ögonblicksbild egenskaper, skapar du en hanterad disk från en ögonblicksbild och skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="449f9-109">This script uses hello following commands tooget snapshot properties, create a managed disk from snapshot and create a VM.</span></span> <span data-ttu-id="449f9-110">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="449f9-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="449f9-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="449f9-111">Command</span></span> | <span data-ttu-id="449f9-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="449f9-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="449f9-113">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="449f9-113">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/get-azurermsnapshot) | <span data-ttu-id="449f9-114">Hämtar en ögonblicksbild med hjälp av namnet på ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="449f9-114">Gets a snapshot using snapshot name.</span></span> |
| [<span data-ttu-id="449f9-115">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="449f9-115">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/new-azurermdiskconfig) | <span data-ttu-id="449f9-116">Skapar en diskkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="449f9-116">Creates a disk configuration.</span></span> <span data-ttu-id="449f9-117">Den här konfigurationen används med hello disken skapas.</span><span class="sxs-lookup"><span data-stu-id="449f9-117">This configuration is used with hello disk creation process.</span></span> |
| [<span data-ttu-id="449f9-118">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="449f9-118">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/new-azurermdisk) | <span data-ttu-id="449f9-119">Skapar en hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="449f9-119">Creates a managed disk.</span></span> |
| [<span data-ttu-id="449f9-120">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="449f9-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="449f9-121">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="449f9-121">Creates a VM configuration.</span></span> <span data-ttu-id="449f9-122">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="449f9-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="449f9-123">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="449f9-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="449f9-124">Ange AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="449f9-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="449f9-125">Bifogar hello hanterade diskar som OS disk toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="449f9-125">Attaches hello managed disk as OS disk toohello virtual machine</span></span> |
| [<span data-ttu-id="449f9-126">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="449f9-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="449f9-127">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="449f9-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="449f9-128">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="449f9-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="449f9-129">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="449f9-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="449f9-130">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="449f9-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="449f9-131">Skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="449f9-131">Creates a virtual machine.</span></span> |
|[<span data-ttu-id="449f9-132">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="449f9-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="449f9-133">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="449f9-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="449f9-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="449f9-134">Next steps</span></span>

<span data-ttu-id="449f9-135">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="449f9-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="449f9-136">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="449f9-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
