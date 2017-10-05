---
title: "Azure CLI-skript Sample - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en hanterad disk från en ögonblicksbild"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 030fa06bc5819443dac5832172a93c2dca28d1ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="634d0-103">Skapa en hanterad disk från en ögonblicksbild med CLI</span><span class="sxs-lookup"><span data-stu-id="634d0-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="634d0-104">Det här skriptet skapar en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="634d0-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="634d0-105">Du kan använda den för att återställa en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="634d0-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="634d0-106">Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="634d0-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="634d0-107">Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="634d0-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="634d0-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="634d0-108">Sample script</span></span>

<span data-ttu-id="634d0-109">[!code-azurecli[huvudsakliga](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "skapa hanterade diskar från en ögonblicksbild")]</span><span class="sxs-lookup"><span data-stu-id="634d0-109">[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="634d0-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="634d0-110">Script explanation</span></span>

<span data-ttu-id="634d0-111">Det här skriptet använder följande kommandon för att skapa en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="634d0-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="634d0-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="634d0-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="634d0-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="634d0-113">Command</span></span> | <span data-ttu-id="634d0-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="634d0-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="634d0-115">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="634d0-115">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="634d0-116">Hämtar alla egenskaper för en ögonblicksbild med hjälp av namn och egenskaper för resursgrupp av ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="634d0-116">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="634d0-117">ID-egenskapen används för att skapa hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="634d0-117">Id property is used to create managed disk.</span></span>  |
| [<span data-ttu-id="634d0-118">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="634d0-118">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="634d0-119">Skapar en hanterad disk med en ögonblicksbild Id för en hanterad ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="634d0-119">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="634d0-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="634d0-120">Next steps</span></span>

[<span data-ttu-id="634d0-121">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="634d0-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="634d0-122">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="634d0-122">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="634d0-123">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="634d0-123">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
