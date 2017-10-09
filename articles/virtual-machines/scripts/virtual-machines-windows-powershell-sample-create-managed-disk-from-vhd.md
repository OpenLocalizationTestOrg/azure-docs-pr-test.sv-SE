---
title: "aaaAzure PowerShell skriptexempel - skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma eller olika prenumerationen | Microsoft Docs"
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
ms.openlocfilehash: 47acff274cdf79d6fc3cd685cda01cad3d14ca8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="19e2a-103">Skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma eller olika prenumerationen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="19e2a-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="19e2a-104">Det här skriptet skapar en hanterad disk från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="19e2a-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="19e2a-105">Använd det här skriptet tooimport en särskild (inte generaliserad/Sysprep) VHD toomanaged OS disk toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="19e2a-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="19e2a-106">Använd den tooimport en VHD toomanaged data datadisk.</span><span class="sxs-lookup"><span data-stu-id="19e2a-106">Also, use it tooimport a data VHD toomanaged data disk.</span></span> 

<span data-ttu-id="19e2a-107">Skapa inte flera identiska hanterade diskar från en VHD-fil i tid.</span><span class="sxs-lookup"><span data-stu-id="19e2a-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="19e2a-108">toocreate hanterade diskar från en vhd-fil, blob ögonblicksbild av hello vhd-filen har skapats och är det använda toocreate hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="19e2a-108">toocreate managed disks from a vhd file, blob snapshot of hello vhd file is created and then it is used toocreate managed disks.</span></span> <span data-ttu-id="19e2a-109">Endast en blob ögonblicksbild kan skapas i en minut som orsakar fel när disken skapas på grund av toothrottling.</span><span class="sxs-lookup"><span data-stu-id="19e2a-109">Only one blob snapshot can be created in a minute that causes disk creation failures due toothrottling.</span></span> <span data-ttu-id="19e2a-110">tooavoid denna begränsning, skapa en [hanterade ögonblicksbild från hello vhd-filen](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) och sedan använda hello hanteras ögonblicksbild toocreate flera hanterade diskar på kort tid.</span><span class="sxs-lookup"><span data-stu-id="19e2a-110">tooavoid this throttling, create a [managed snapshot from hello vhd file](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use hello managed snapshot toocreate multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="19e2a-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="19e2a-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="19e2a-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="19e2a-112">Script explanation</span></span>

<span data-ttu-id="19e2a-113">Det här skriptet använder följande kommandon toocreate hanterade diskar från en virtuell Hårddisk i annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="19e2a-113">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="19e2a-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="19e2a-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="19e2a-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="19e2a-115">Command</span></span> | <span data-ttu-id="19e2a-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="19e2a-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="19e2a-117">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="19e2a-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="19e2a-118">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="19e2a-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="19e2a-119">Den omfattar lagringstyp, plats, resurs-Id för hello storage-konto där hello överordnade virtuella Hårddisken lagras VHD-URI för hello överordnade virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="19e2a-119">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="19e2a-120">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="19e2a-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="19e2a-121">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="19e2a-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="19e2a-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19e2a-122">Next steps</span></span>

[<span data-ttu-id="19e2a-123">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="19e2a-123">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="19e2a-124">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="19e2a-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="19e2a-125">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19e2a-125">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
