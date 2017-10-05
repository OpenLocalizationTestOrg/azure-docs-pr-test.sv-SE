---
title: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid"
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
ms.openlocfilehash: 8588a33a1f0b4cce0059a40239301587cdc28920
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="598be-103">Skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid med PowerShell</span><span class="sxs-lookup"><span data-stu-id="598be-103">Create a snapshot from a VHD to create multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="598be-104">Det här skriptet skapar en ögonblicksbild från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="598be-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="598be-105">Använd det här skriptet för att importera ett speciellt (inte generaliserad/Sysprep) virtuell Hårddisk till en ögonblicksbild och använder sedan ögonblicksbilden för att skapa flera identiska hanteras diskarna i tid.</span><span class="sxs-lookup"><span data-stu-id="598be-105">Use this script to import a specialized (not generalized/sysprepped) VHD to a snapshot and then use the snapshot to create multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="598be-106">Använd det att importera data VHD till en ögonblicksbild och sedan använda ögonblicksbilden för att skapa flera hanterade diskar i tid.</span><span class="sxs-lookup"><span data-stu-id="598be-106">Also, use it to import a data VHD to a snapshot and then use the snapshot to create multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="598be-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="598be-107">Sample script</span></span>

<span data-ttu-id="598be-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "skapa ögonblicksbilder från VHD")]</span><span class="sxs-lookup"><span data-stu-id="598be-108">[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="598be-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="598be-109">Script explanation</span></span>

<span data-ttu-id="598be-110">Det här skriptet använder följande kommandon för att skapa en hanterad disk från en virtuell Hårddisk i annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="598be-110">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="598be-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="598be-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="598be-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="598be-112">Command</span></span> | <span data-ttu-id="598be-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="598be-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="598be-114">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="598be-114">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="598be-115">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="598be-115">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="598be-116">Den omfattar lagringstyp, plats, resurs-Id för det lagringskonto där överordnat virtuella Hårddisken lagras och VHD-URI för överordnat virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="598be-116">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, and VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="598be-117">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="598be-117">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="598be-118">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="598be-118">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="598be-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="598be-119">Next steps</span></span>

[<span data-ttu-id="598be-120">Skapa en hanterad disk från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="598be-120">Create a managed disk from snapshot</span></span>](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="598be-121">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="598be-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="598be-122">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="598be-122">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="598be-123">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="598be-123">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>