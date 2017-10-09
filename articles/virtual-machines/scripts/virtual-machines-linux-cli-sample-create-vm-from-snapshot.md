---
title: "aaaAzure CLI skriptexempel - skapa en virtuell dator från en ögonblicksbild | Microsoft Docs"
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
ms.openlocfilehash: ddc95289dcb8a0ca7c7854d969983f96b8f4613f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a><span data-ttu-id="20089-103">Skapa en virtuell dator från en ögonblicksbild med CLI</span><span class="sxs-lookup"><span data-stu-id="20089-103">Create a virtual machine from a snapshot with CLI</span></span>

<span data-ttu-id="20089-104">Det här skriptet skapar en virtuell dator från en ögonblicksbild av en OS-disk.</span><span class="sxs-lookup"><span data-stu-id="20089-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="20089-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="20089-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="20089-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="20089-106">Clean up deployment</span></span> 

<span data-ttu-id="20089-107">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="20089-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="20089-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="20089-108">Script explanation</span></span>

<span data-ttu-id="20089-109">Det här skriptet använder hello följande kommandon toocreate hanterade diskar, virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="20089-109">This script uses hello following commands toocreate a managed disk, virtual machine, and all related resources.</span></span> <span data-ttu-id="20089-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="20089-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="20089-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="20089-111">Command</span></span> | <span data-ttu-id="20089-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="20089-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="20089-113">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="20089-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="20089-114">Hämtar ögonblicksbild med namnet på ögonblicksbilder och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="20089-114">Gets snapshot using snapshot name and resource group name.</span></span> <span data-ttu-id="20089-115">ID-egenskapen för hello returnerade objekt är används toocreate hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="20089-115">Id property of hello returned object is used toocreate a managed disk.</span></span>  |
| [<span data-ttu-id="20089-116">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="20089-116">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="20089-117">Skapar hanterade diskar från en ögonblicksbild med hjälp av ögonblicksbilder Id, namn på disk, lagringstyp och storlek</span><span class="sxs-lookup"><span data-stu-id="20089-117">Creates managed disks from a snapshot using snapshot Id, disk name, storage type, and size</span></span>  |
| [<span data-ttu-id="20089-118">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="20089-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="20089-119">Skapar en virtuell dator med hjälp av en hanterad OS-disk</span><span class="sxs-lookup"><span data-stu-id="20089-119">Creates a VM using a managed OS disk</span></span> |

## <a name="next-steps"></a><span data-ttu-id="20089-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20089-120">Next steps</span></span>

<span data-ttu-id="20089-121">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="20089-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="20089-122">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="20089-122">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
