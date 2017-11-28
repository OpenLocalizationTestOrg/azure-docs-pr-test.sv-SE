---
title: "aaaAzure PowerShell skriptexempel - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
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
ms.openlocfilehash: b99413b4c3e9a662a5fef74b7e4aeb5ecbb26af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="5d41d-103">Skapa en hanterad disk från en ögonblicksbild med PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d41d-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="5d41d-104">Det här skriptet skapar en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5d41d-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="5d41d-105">Använd den toorestore en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="5d41d-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="5d41d-106">Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="5d41d-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="5d41d-107">Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="5d41d-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5d41d-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="5d41d-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="5d41d-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5d41d-109">Script explanation</span></span>

<span data-ttu-id="5d41d-110">Det här skriptet använder följande kommandon toocreate hanterade diskar från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="5d41d-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="5d41d-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5d41d-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5d41d-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="5d41d-112">Command</span></span> | <span data-ttu-id="5d41d-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5d41d-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5d41d-114">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="5d41d-114">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="5d41d-115">Hämtar ögonblicksbild egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5d41d-115">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="5d41d-116">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="5d41d-116">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="5d41d-117">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="5d41d-117">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="5d41d-118">Den omfattar hello resurs-Id för hello överordnade ögonblicksbilden plats som är samma som hello platsen för överordnade ögonblicksbild och hello lagringstyp.</span><span class="sxs-lookup"><span data-stu-id="5d41d-118">It includes hello resource Id of hello parent snapshot, location that is same as hello location of parent snapshot and hello storage type.</span></span>  |
| [<span data-ttu-id="5d41d-119">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="5d41d-119">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="5d41d-120">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="5d41d-120">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="5d41d-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d41d-121">Next steps</span></span>

[<span data-ttu-id="5d41d-122">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="5d41d-122">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="5d41d-123">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5d41d-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="5d41d-124">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5d41d-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
