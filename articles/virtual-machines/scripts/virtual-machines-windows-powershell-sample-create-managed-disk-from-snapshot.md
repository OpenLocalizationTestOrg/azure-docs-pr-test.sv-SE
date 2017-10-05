---
title: "Azure PowerShell-skript Sample - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en hanterad disk från en ögonblicksbild"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: b475516694d120b7ea05d0892b6789710eec171e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="451f4-103">Skapa en hanterad disk från en ögonblicksbild med PowerShell</span><span class="sxs-lookup"><span data-stu-id="451f4-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="451f4-104">Det här skriptet skapar en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="451f4-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="451f4-105">Du kan använda den för att återställa en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="451f4-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="451f4-106">Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="451f4-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="451f4-107">Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="451f4-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="451f4-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="451f4-108">Sample script</span></span>

<span data-ttu-id="451f4-109">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "skapa hanterade diskar från en ögonblicksbild")]</span><span class="sxs-lookup"><span data-stu-id="451f4-109">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="451f4-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="451f4-110">Script explanation</span></span>

<span data-ttu-id="451f4-111">Det här skriptet använder följande kommandon för att skapa en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="451f4-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="451f4-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="451f4-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="451f4-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="451f4-113">Command</span></span> | <span data-ttu-id="451f4-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="451f4-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="451f4-115">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="451f4-115">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="451f4-116">Hämtar ögonblicksbild egenskaper.</span><span class="sxs-lookup"><span data-stu-id="451f4-116">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="451f4-117">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="451f4-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="451f4-118">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="451f4-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="451f4-119">Den innehåller resurs-Id för överordnad ögonblicksbilden plats som är samma som plats för överordnade ögonblicksbild och lagringstyp.</span><span class="sxs-lookup"><span data-stu-id="451f4-119">It includes the resource Id of the parent snapshot, location that is same as the location of parent snapshot and the storage type.</span></span>  |
| [<span data-ttu-id="451f4-120">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="451f4-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="451f4-121">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="451f4-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="451f4-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="451f4-122">Next steps</span></span>

[<span data-ttu-id="451f4-123">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="451f4-123">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="451f4-124">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="451f4-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="451f4-125">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="451f4-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>