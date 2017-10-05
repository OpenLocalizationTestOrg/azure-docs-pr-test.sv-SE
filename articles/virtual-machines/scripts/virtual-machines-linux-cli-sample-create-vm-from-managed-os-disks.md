---
title: Azure CLI-skript Sample - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk | Microsoft Docs
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
ms.openlocfilehash: 18eefee869b243b35d4426c226eff5f48cd490a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="67ea1-103">Skapa en virtuell dator med en befintlig OS-disk hanterade med CLI</span><span class="sxs-lookup"><span data-stu-id="67ea1-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="67ea1-104">Det här skriptet skapar en virtuell dator genom att koppla en befintlig hanterade disk som OS-disken.</span><span class="sxs-lookup"><span data-stu-id="67ea1-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="67ea1-105">Använd det här skriptet i föregående scenarier:</span><span class="sxs-lookup"><span data-stu-id="67ea1-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="67ea1-106">Skapa en virtuell dator från en befintlig hanterade OS-disk som har kopierats från en hanterad disk i en annan prenumeration</span><span class="sxs-lookup"><span data-stu-id="67ea1-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="67ea1-107">Skapa en virtuell dator från en befintlig hanterade disk som har skapats från en särskild VHD-fil</span><span class="sxs-lookup"><span data-stu-id="67ea1-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="67ea1-108">Skapa en virtuell dator från en befintlig hanterade OS-disk som har skapats från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="67ea1-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="67ea1-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="67ea1-109">Sample script</span></span>

<span data-ttu-id="67ea1-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Skapa virtuell dator från en hanterad disk")]</span><span class="sxs-lookup"><span data-stu-id="67ea1-110">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="67ea1-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="67ea1-111">Clean up deployment</span></span> 

<span data-ttu-id="67ea1-112">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="67ea1-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="67ea1-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="67ea1-113">Script explanation</span></span>

<span data-ttu-id="67ea1-114">Det här skriptet använder följande kommandon för att hämta egenskaper för hanterade diskar, ansluter hanterade diskar till en ny virtuell dator och skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="67ea1-114">This script uses the following commands to get managed disk properties, attach a managed disk to a new VM and create a VM.</span></span> <span data-ttu-id="67ea1-115">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="67ea1-115">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="67ea1-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="67ea1-116">Command</span></span> | <span data-ttu-id="67ea1-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="67ea1-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="67ea1-118">AZ disk visa</span><span class="sxs-lookup"><span data-stu-id="67ea1-118">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="67ea1-119">Hämtar egenskaper för hanterade diskar med hjälp av diskens namn och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="67ea1-119">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="67ea1-120">ID-egenskapen används för att koppla en hanterade diskar till en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="67ea1-120">Id property is used to attach a managed disk to a new VM</span></span> |
| [<span data-ttu-id="67ea1-121">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="67ea1-121">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="67ea1-122">Skapar en virtuell dator med hjälp av en hanterad OS-disk</span><span class="sxs-lookup"><span data-stu-id="67ea1-122">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="67ea1-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67ea1-123">Next steps</span></span>

<span data-ttu-id="67ea1-124">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67ea1-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="67ea1-125">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67ea1-125">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
