---
title: "Azure PowerShell-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma eller olika prenumeration | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma eller olika prenumeration"
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
ms.openlocfilehash: 728def40a3eb132537decbd099fa71f4544c6b87
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="2012a-103">Skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma eller olika prenumerationen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2012a-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="2012a-104">Det här skriptet skapar en hanterad disk från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2012a-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="2012a-105">Använd det här skriptet för att importera ett speciellt (inte generaliserad/Sysprep) VHD till hanterade OS-disken för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2012a-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="2012a-106">Använda den också för att importera en VHD-data till hanterade datadisk.</span><span class="sxs-lookup"><span data-stu-id="2012a-106">Also, use it to import a data VHD to managed data disk.</span></span> 

<span data-ttu-id="2012a-107">Skapa inte flera identiska hanterade diskar från en VHD-fil i tid.</span><span class="sxs-lookup"><span data-stu-id="2012a-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="2012a-108">Om du vill skapa hanterade diskar från en vhd-fil, blob ögonblicksbild av vhd-filen skapas och den används för att skapa hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="2012a-108">To create managed disks from a vhd file, blob snapshot of the vhd file is created and then it is used to create managed disks.</span></span> <span data-ttu-id="2012a-109">Endast en blob ögonblicksbild kan skapas per minut som orsakar fel när disken skapas på grund av begränsning.</span><span class="sxs-lookup"><span data-stu-id="2012a-109">Only one blob snapshot can be created in a minute that causes disk creation failures due to throttling.</span></span> <span data-ttu-id="2012a-110">För att undvika denna begränsning, skapa en [hanterade ögonblicksbild från vhd-filen](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) och sedan Använd hanterade ögonblicksbilden att skapa flera hanterade diskar kort tid.</span><span class="sxs-lookup"><span data-stu-id="2012a-110">To avoid this throttling, create a [managed snapshot from the vhd file](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use the managed snapshot to create multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2012a-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2012a-111">Sample script</span></span>

<span data-ttu-id="2012a-112">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "skapa hanterade disk från virtuell Hårddisk")]</span><span class="sxs-lookup"><span data-stu-id="2012a-112">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="2012a-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2012a-113">Script explanation</span></span>

<span data-ttu-id="2012a-114">Det här skriptet använder följande kommandon för att skapa en hanterad disk från en virtuell Hårddisk i annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2012a-114">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="2012a-115">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2012a-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2012a-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="2012a-116">Command</span></span> | <span data-ttu-id="2012a-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2012a-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2012a-118">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="2012a-118">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="2012a-119">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="2012a-119">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="2012a-120">Den omfattar lagringstyp, plats, resurs-Id för det lagringskonto där överordnat virtuella Hårddisken lagras VHD-URI för överordnat virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="2012a-120">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="2012a-121">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="2012a-121">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="2012a-122">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="2012a-122">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2012a-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2012a-123">Next steps</span></span>

[<span data-ttu-id="2012a-124">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="2012a-124">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="2012a-125">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2012a-125">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="2012a-126">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2012a-126">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>