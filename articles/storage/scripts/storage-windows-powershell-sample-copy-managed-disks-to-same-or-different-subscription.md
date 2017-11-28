---
title: Azure PowerShell skriptexempel - kopia (flytta) hanterade diskar till samma eller olika prenumeration | Microsoft Docs
description: Azure PowerShell skriptexempel - kopia (flytta) hanterade diskar till samma eller en annan prenumeration
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
ms.openlocfilehash: 6fa94de0461cc538a60d57ca3518141afd9d0469
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="79fc3-103">Kopiera hanterade diskar i samma prenumeration eller annan prenumeration med PowerShell</span><span class="sxs-lookup"><span data-stu-id="79fc3-103">Copy managed disks in the same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="79fc3-104">Det här skriptet skapar en kopia av en befintlig hanterade disk i samma prenumeration eller annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="79fc3-104">This script creates a copy of an existing managed disk in the same subscription or different subscription.</span></span> <span data-ttu-id="79fc3-105">Den nya disken skapas i samma region som den överordnade hantera disken.</span><span class="sxs-lookup"><span data-stu-id="79fc3-105">The new disk is created in the same region as the parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="79fc3-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="79fc3-106">Sample script</span></span>

<span data-ttu-id="79fc3-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "kopiera hanteras disk")]</span><span class="sxs-lookup"><span data-stu-id="79fc3-107">[!code-powershell[main](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="79fc3-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="79fc3-108">Script explanation</span></span>

<span data-ttu-id="79fc3-109">Det här skriptet använder följande kommandon för att skapa en ny hanterade disk i målprenumerationen med ID: T för den hanterade källdisken.</span><span class="sxs-lookup"><span data-stu-id="79fc3-109">This script uses following commands to create a new managed disk in the target subscription using the Id of the source managed disk.</span></span> <span data-ttu-id="79fc3-110">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="79fc3-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="79fc3-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="79fc3-111">Command</span></span> | <span data-ttu-id="79fc3-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="79fc3-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="79fc3-113">Ny AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="79fc3-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="79fc3-114">Skapar diskkonfigurationen som används för att skapa en disk.</span><span class="sxs-lookup"><span data-stu-id="79fc3-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="79fc3-115">Den innehåller resurs-Id för överordnad disk och plats som är samma som plats för överordnad disk.</span><span class="sxs-lookup"><span data-stu-id="79fc3-115">It includes the resource Id of the parent disk and location that is same as the location of parent disk.</span></span>  |
| [<span data-ttu-id="79fc3-116">Ny AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="79fc3-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="79fc3-117">Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="79fc3-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="79fc3-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79fc3-118">Next steps</span></span>

[<span data-ttu-id="79fc3-119">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="79fc3-119">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="79fc3-120">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="79fc3-120">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="79fc3-121">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79fc3-121">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>