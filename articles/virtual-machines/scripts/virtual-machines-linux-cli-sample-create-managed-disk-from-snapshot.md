---
title: "aaaAzure CLI skriptexempel - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
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
ms.openlocfilehash: 549692f5027b3f50b0dd89fe701ebbf0f51d6031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="d7c0a-103">Skapa en hanterad disk från en ögonblicksbild med CLI</span><span class="sxs-lookup"><span data-stu-id="d7c0a-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="d7c0a-104">Det här skriptet skapar en hanterad disk från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="d7c0a-105">Använd den toorestore en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="d7c0a-106">Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="d7c0a-107">Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d7c0a-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d7c0a-108">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="d7c0a-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d7c0a-109">Script explanation</span></span>

<span data-ttu-id="d7c0a-110">Det här skriptet använder följande kommandon toocreate hanterade diskar från en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="d7c0a-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d7c0a-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="d7c0a-112">Command</span></span> | <span data-ttu-id="d7c0a-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d7c0a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d7c0a-114">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="d7c0a-114">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="d7c0a-115">Hämtar alla hello egenskaperna för en ögonblicksbild med hello namn och egenskaper för resursgrupp hello ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-115">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="d7c0a-116">ID-egenskap är används toocreate hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="d7c0a-116">Id property is used toocreate managed disk.</span></span>  |
| [<span data-ttu-id="d7c0a-117">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="d7c0a-117">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="d7c0a-118">Skapar en hanterad disk med en ögonblicksbild Id för en hanterad ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="d7c0a-118">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d7c0a-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7c0a-119">Next steps</span></span>

[<span data-ttu-id="d7c0a-120">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="d7c0a-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="d7c0a-121">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d7c0a-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d7c0a-122">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7c0a-122">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
