---
title: "Azure CLI-skript Sample - skapa en virtuell dator från en ögonblicksbild | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en virtuell dator från en ögonblicksbild"
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
ms.openlocfilehash: 6e47c3baebd5b68ec29d55c43dc00ae7665c81f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a><span data-ttu-id="d9a42-103">Skapa en virtuell dator från en ögonblicksbild med CLI</span><span class="sxs-lookup"><span data-stu-id="d9a42-103">Create a virtual machine from a snapshot with CLI</span></span>

<span data-ttu-id="d9a42-104">Det här skriptet skapar en virtuell dator från en ögonblicksbild av en OS-disk.</span><span class="sxs-lookup"><span data-stu-id="d9a42-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d9a42-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d9a42-105">Sample script</span></span>

<span data-ttu-id="d9a42-106">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Skapa virtuell dator från en ögonblicksbild")]</span><span class="sxs-lookup"><span data-stu-id="d9a42-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d9a42-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d9a42-107">Clean up deployment</span></span> 

<span data-ttu-id="d9a42-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="d9a42-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d9a42-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d9a42-109">Script explanation</span></span>

<span data-ttu-id="d9a42-110">Det här skriptet använder följande kommandon för att skapa en hanterad disk, den virtuella datorn och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="d9a42-110">This script uses the following commands to create a managed disk, virtual machine, and all related resources.</span></span> <span data-ttu-id="d9a42-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d9a42-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d9a42-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="d9a42-112">Command</span></span> | <span data-ttu-id="d9a42-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d9a42-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d9a42-114">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="d9a42-114">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="d9a42-115">Hämtar ögonblicksbild med namnet på ögonblicksbilder och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="d9a42-115">Gets snapshot using snapshot name and resource group name.</span></span> <span data-ttu-id="d9a42-116">ID-egenskapen för det returnerade objektet används för att skapa en hanterad disk.</span><span class="sxs-lookup"><span data-stu-id="d9a42-116">Id property of the returned object is used to create a managed disk.</span></span>  |
| [<span data-ttu-id="d9a42-117">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="d9a42-117">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="d9a42-118">Skapar hanterade diskar från en ögonblicksbild med hjälp av ögonblicksbilder Id, namn på disk, lagringstyp och storlek</span><span class="sxs-lookup"><span data-stu-id="d9a42-118">Creates managed disks from a snapshot using snapshot Id, disk name, storage type, and size</span></span>  |
| [<span data-ttu-id="d9a42-119">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="d9a42-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="d9a42-120">Skapar en virtuell dator med hjälp av en hanterad OS-disk</span><span class="sxs-lookup"><span data-stu-id="d9a42-120">Creates a VM using a managed OS disk</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d9a42-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9a42-121">Next steps</span></span>

<span data-ttu-id="d9a42-122">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d9a42-122">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d9a42-123">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9a42-123">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
