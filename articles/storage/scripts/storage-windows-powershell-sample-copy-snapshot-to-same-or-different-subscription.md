---
title: "aaaAzure skriptexempel PowerShell - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration | Microsoft Docs"
description: "Azure PowerShell skriptexempel - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration"
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
ms.openlocfilehash: 7a3565356f13cb93759dec7ef9d0357e04c3410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="77ea6-103">Kopiera ögonblicksbild av hanterade diskar i samma prenumeration eller annan prenumeration med PowerShell</span><span class="sxs-lookup"><span data-stu-id="77ea6-103">Copy snapshot of a managed disk in same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="77ea6-104">Det här skriptet skapar en kopia av en ögonblicksbild i hello samma samma prenumeration eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="77ea6-104">This script creates a copy of a snapshot in hello same same subscription or different subscription.</span></span> <span data-ttu-id="77ea6-105">Använd det här skriptet toomove en ögonblicksbild toodifferent prenumeration för datalagring.</span><span class="sxs-lookup"><span data-stu-id="77ea6-105">Use this script toomove a snapshot toodifferent subscription for data retention.</span></span> <span data-ttu-id="77ea6-106">Lagra ögonblicksbilder i annan prenumeration för att skydda mot oavsiktlig borttagning av ögonblicksbilder i prenumerationen huvudsakliga.</span><span class="sxs-lookup"><span data-stu-id="77ea6-106">Storing snapshots in different subscription protect you from accidental deletion of snapshots in your main subscription.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="77ea6-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="77ea6-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="77ea6-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="77ea6-108">Script explanation</span></span>

<span data-ttu-id="77ea6-109">Det här skriptet använder följande kommandon toocreate en ögonblicksbild i hello mål en prenumeration med hjälp av hello-Id för hello källa ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="77ea6-109">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="77ea6-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="77ea6-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="77ea6-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="77ea6-111">Command</span></span> | <span data-ttu-id="77ea6-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="77ea6-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="77ea6-113">Ny AzureRmSnapshotConfig</span><span class="sxs-lookup"><span data-stu-id="77ea6-113">New-AzureRmSnapshotConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | <span data-ttu-id="77ea6-114">Skapar ögonblicksbild konfigurationen som används för att skapa en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="77ea6-114">Creates snapshot configuration that is used for snapshot creation.</span></span> <span data-ttu-id="77ea6-115">Den omfattar hello resurs-Id för hello överordnade ögonblicksbild och plats som är samma som hello överordnade ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="77ea6-115">It includes hello resource Id of hello parent snapshot and location that is same as hello parent snapshot.</span></span>  |
| [<span data-ttu-id="77ea6-116">Ny AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="77ea6-116">New-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="77ea6-117">Skapar en ögonblicksbild med hjälp av ögonblicksbilder konfiguration, namnet på ögonblicksbilder och resursgruppens namn som parametrar skickades.</span><span class="sxs-lookup"><span data-stu-id="77ea6-117">Creates a snapshot using snapshot configuration, snapshot name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="77ea6-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77ea6-118">Next steps</span></span>

[<span data-ttu-id="77ea6-119">Skapa en virtuell dator från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="77ea6-119">Create a virtual machine from a snapshot</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="77ea6-120">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="77ea6-120">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="77ea6-121">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77ea6-121">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
