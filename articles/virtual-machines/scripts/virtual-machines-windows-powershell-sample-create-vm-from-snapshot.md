---
title: "Azure PowerShell-skript Sample - skapa en virtuell dator från en ögonblicksbild | Microsoft Docs"
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
ms.openlocfilehash: 63d108bbfd0f58f8a40bf1c7c8649e3a1f7ed288
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a><span data-ttu-id="dde58-103">Skapa en virtuell dator från en ögonblicksbild med PowerShell</span><span class="sxs-lookup"><span data-stu-id="dde58-103">Create a virtual machine from a snapshot with PowerShell</span></span>

<span data-ttu-id="dde58-104">Det här skriptet skapar en virtuell dator från en ögonblicksbild av en OS-disk.</span><span class="sxs-lookup"><span data-stu-id="dde58-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="dde58-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="dde58-105">Sample script</span></span>

<span data-ttu-id="dde58-106">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Skapa virtuell dator från hanterade os-disk")]</span><span class="sxs-lookup"><span data-stu-id="dde58-106">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="dde58-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="dde58-107">Clean up deployment</span></span> 

<span data-ttu-id="dde58-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="dde58-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="dde58-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="dde58-109">Script explanation</span></span>

<span data-ttu-id="dde58-110">Det här skriptet använder följande kommandon för att hämta egenskaper för ögonblicksbild, skapar du en hanterad disk från en ögonblicksbild och skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dde58-110">This script uses the following commands to get snapshot properties, create a managed disk from snapshot and create a VM.</span></span> <span data-ttu-id="dde58-111">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="dde58-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="dde58-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="dde58-112">Command</span></span> | <span data-ttu-id="dde58-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dde58-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dde58-114">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="dde58-114">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/get-azurermsnapshot) | <span data-ttu-id="dde58-115">Hämtar en ögonblicksbild med hjälp av namnet på ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="dde58-115">Gets a snapshot using snapshot name.</span></span> |
| [<span data-ttu-id="dde58-116">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="dde58-116">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/new-azurermdiskconfig) | <span data-ttu-id="dde58-117">Skapar en diskkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="dde58-117">Creates a disk configuration.</span></span> <span data-ttu-id="dde58-118">Den här konfigurationen används med disk-processen.</span><span class="sxs-lookup"><span data-stu-id="dde58-118">This configuration is used with the disk creation process.</span></span> |
| [<span data-ttu-id="dde58-119">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="dde58-119">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/new-azurermdisk) | <span data-ttu-id="dde58-120">Skapar en hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="dde58-120">Creates a managed disk.</span></span> |
| [<span data-ttu-id="dde58-121">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="dde58-121">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="dde58-122">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="dde58-122">Creates a VM configuration.</span></span> <span data-ttu-id="dde58-123">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dde58-123">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="dde58-124">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dde58-124">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="dde58-125">Ange AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="dde58-125">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="dde58-126">Bifogar hanterade diskar som OS-disken till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="dde58-126">Attaches the managed disk as OS disk to the virtual machine</span></span> |
| [<span data-ttu-id="dde58-127">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="dde58-127">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="dde58-128">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dde58-128">Creates a public IP address.</span></span> |
| [<span data-ttu-id="dde58-129">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="dde58-129">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="dde58-130">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="dde58-130">Creates a network interface.</span></span> |
| [<span data-ttu-id="dde58-131">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="dde58-131">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="dde58-132">Skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dde58-132">Creates a virtual machine.</span></span> |
|[<span data-ttu-id="dde58-133">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dde58-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="dde58-134">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="dde58-134">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dde58-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dde58-135">Next steps</span></span>

<span data-ttu-id="dde58-136">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dde58-136">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="dde58-137">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dde58-137">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>