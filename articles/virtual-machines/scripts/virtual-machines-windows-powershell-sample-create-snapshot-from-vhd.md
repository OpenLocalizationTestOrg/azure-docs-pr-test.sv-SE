---
title: "aaaAzure PowerShell skriptexempel - skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid"
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
ms.openlocfilehash: 5f11793b3669df099b6c31dfdbe906c96ba51786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-snapshot-from-a-vhd-toocreate-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="15c9d-103">Skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid med PowerShell</span><span class="sxs-lookup"><span data-stu-id="15c9d-103">Create a snapshot from a VHD toocreate multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="15c9d-104">Det här skriptet skapar en ögonblicksbild från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15c9d-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="15c9d-105">Använd det här skriptet tooimport en särskild (inte generaliserad/Sysprep) VHD tooa ögonblicksbild och Använd hello ögonblicksbild toocreate flera identiska hanterade diskar i tid.</span><span class="sxs-lookup"><span data-stu-id="15c9d-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD tooa snapshot and then use hello snapshot toocreate multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="15c9d-106">Dessutom använder tooimport en ögonblicksbild VHD tooa och Använd hello ögonblicksbild toocreate flera hanterade diskar i tid.</span><span class="sxs-lookup"><span data-stu-id="15c9d-106">Also, use it tooimport a data VHD tooa snapshot and then use hello snapshot toocreate multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="15c9d-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="15c9d-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="15c9d-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="15c9d-108">Script explanation</span></span>

<span data-ttu-id="15c9d-109">Det här skriptet använder följande kommandon toocreate hanterade diskar från en virtuell Hårddisk i annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15c9d-109">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="15c9d-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="15c9d-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="15c9d-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="15c9d-111">Command</span></span> | <span data-ttu-id="15c9d-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="15c9d-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="15c9d-113">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="15c9d-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="15c9d-114">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="15c9d-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="15c9d-115">Den omfattar lagringstyp, plats, resurs-Id för hello storage-konto där hello överordnade virtuella Hårddisken lagras och VHD-URI för hello överordnade virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="15c9d-115">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, and VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="15c9d-116">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="15c9d-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="15c9d-117">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="15c9d-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="15c9d-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15c9d-118">Next steps</span></span>

[<span data-ttu-id="15c9d-119">Skapa en hanterad disk från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="15c9d-119">Create a managed disk from snapshot</span></span>](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="15c9d-120">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="15c9d-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="15c9d-121">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="15c9d-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="15c9d-122">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="15c9d-122">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
