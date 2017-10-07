---
title: aaaAzure CLI skriptexempel - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk | Microsoft Docs
description: Azure CLI-skript Sample - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 71fc5c6a577c64b913cfa35e99b2b9b75dea0c31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="7063f-103">Skapa en virtuell dator med en befintlig OS-disk hanterade med CLI</span><span class="sxs-lookup"><span data-stu-id="7063f-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="7063f-104">Det här skriptet skapar en virtuell dator genom att koppla en befintlig hanterade disk som OS-disken.</span><span class="sxs-lookup"><span data-stu-id="7063f-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="7063f-105">Använd det här skriptet i föregående scenarier:</span><span class="sxs-lookup"><span data-stu-id="7063f-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="7063f-106">Skapa en virtuell dator från en befintlig hanterade OS-disk som har kopierats från en hanterad disk i en annan prenumeration</span><span class="sxs-lookup"><span data-stu-id="7063f-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="7063f-107">Skapa en virtuell dator från en befintlig hanterade disk som har skapats från en särskild VHD-fil</span><span class="sxs-lookup"><span data-stu-id="7063f-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="7063f-108">Skapa en virtuell dator från en befintlig hanterade OS-disk som har skapats från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="7063f-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7063f-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7063f-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="7063f-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="7063f-110">Clean up deployment</span></span> 

<span data-ttu-id="7063f-111">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="7063f-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="7063f-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7063f-112">Script explanation</span></span>

<span data-ttu-id="7063f-113">Det här skriptet använder hello följande kommandon tooget hanteras diskegenskaper, bifoga en hanterade diskar tooa ny virtuell dator och skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7063f-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="7063f-114">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7063f-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7063f-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="7063f-115">Command</span></span> | <span data-ttu-id="7063f-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7063f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7063f-117">AZ disk visa</span><span class="sxs-lookup"><span data-stu-id="7063f-117">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="7063f-118">Hämtar egenskaper för hanterade diskar med hjälp av diskens namn och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="7063f-118">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="7063f-119">ID-egenskap är används tooattach en hanterade diskar tooa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7063f-119">Id property is used tooattach a managed disk tooa new VM</span></span> |
| [<span data-ttu-id="7063f-120">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="7063f-120">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="7063f-121">Skapar en virtuell dator med hjälp av en hanterad OS-disk</span><span class="sxs-lookup"><span data-stu-id="7063f-121">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="7063f-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7063f-122">Next steps</span></span>

<span data-ttu-id="7063f-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7063f-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7063f-124">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7063f-124">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
