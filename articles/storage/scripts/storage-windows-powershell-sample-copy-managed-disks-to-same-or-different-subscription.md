---
title: aaaAzure PowerShell skriptexempel - kopia (flytta) hanteras diskar toosame eller annan prenumeration | Microsoft Docs
description: Azure PowerShell skriptexempel - kopia (flytta) hanteras diskar toosame eller en annan prenumeration
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
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 5a92118e10a14615e5b1713f1b90188b37b05305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-in-hello-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="856c8-103">Kopiera hanterade diskar i hello samma prenumeration eller annan prenumeration med PowerShell</span><span class="sxs-lookup"><span data-stu-id="856c8-103">Copy managed disks in hello same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="856c8-104">Det här skriptet skapar en kopia av en befintlig hanterade disk i hello samma prenumeration eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="856c8-104">This script creates a copy of an existing managed disk in hello same subscription or different subscription.</span></span> <span data-ttu-id="856c8-105">hello ny disk skapas i hello samma region som hello överordnade hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="856c8-105">hello new disk is created in hello same region as hello parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="856c8-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="856c8-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="856c8-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="856c8-107">Script explanation</span></span>

<span data-ttu-id="856c8-108">Det här skriptet använder följande kommandon toocreate en ny hanterade disk i hello mål en prenumeration med hjälp av hello-Id för hello källa hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="856c8-108">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="856c8-109">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="856c8-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="856c8-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="856c8-110">Command</span></span> | <span data-ttu-id="856c8-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="856c8-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="856c8-112">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="856c8-112">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="856c8-113">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="856c8-113">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="856c8-114">Den omfattar hello resurs-Id för hello överordnad disk och plats som är samma som hello platsen för överordnad disk.</span><span class="sxs-lookup"><span data-stu-id="856c8-114">It includes hello resource Id of hello parent disk and location that is same as hello location of parent disk.</span></span>  |
| [<span data-ttu-id="856c8-115">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="856c8-115">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="856c8-116">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="856c8-116">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="856c8-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="856c8-117">Next steps</span></span>

[<span data-ttu-id="856c8-118">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="856c8-118">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="856c8-119">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="856c8-119">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="856c8-120">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="856c8-120">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
