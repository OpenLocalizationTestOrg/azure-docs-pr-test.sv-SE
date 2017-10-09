---
title: "aaaExpand OS-disk på Linux VM med hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooexpand hello operativsystem (OS) virtuell disk på en Linux VM som använder hello Azure CLI 1.0 och hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0db78c0b86b48b2c5358611e11bb0b7ad781a559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a><span data-ttu-id="0fb28-103">Expandera OS-disk på en Linux-VM med hello Azure CLI 1.0 hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0fb28-103">Expand OS disk on a Linux VM using hello Azure CLI with hello Azure CLI 1.0</span></span>
<span data-ttu-id="0fb28-104">hello virtuell hårddisk standardstorlek för hello operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="0fb28-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="0fb28-105">Du kan [lägga till datadiskar](add-disk.md) tooprovide för ytterligare lagringsutrymme, men du kan också tooexpand hello OS-disk.</span><span class="sxs-lookup"><span data-stu-id="0fb28-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand hello OS disk.</span></span> <span data-ttu-id="0fb28-106">Den här artikeln beskrivs hur tooexpand hello OS-disken för en Linux-VM med hjälp av ohanterade diskar med hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="0fb28-106">This article details how tooexpand hello OS disk for a Linux VM using unmanaged disks with hello Azure CLI 1.0.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="0fb28-107">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="0fb28-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="0fb28-108">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="0fb28-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="0fb28-109">[Azure CLI 1.0](#prerequisites) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="0fb28-109">[Azure CLI 1.0](#prerequisites) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="0fb28-110">[Azure CLI 2.0](expand-disks.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="0fb28-110">[Azure CLI 2.0](expand-disks.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fb28-111">Krav</span><span class="sxs-lookup"><span data-stu-id="0fb28-111">Prerequisites</span></span>
<span data-ttu-id="0fb28-112">Du behöver hello [senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och inloggad tooan [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) läget hello Resource Manager på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0fb28-112">You need hello [latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in tooan [Azure account](https://azure.microsoft.com/pricing/free-trial/) using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="0fb28-113">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0fb28-113">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="0fb28-114">Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="0fb28-114">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

## <a name="expand-os-disk"></a><span data-ttu-id="0fb28-115">Expandera operativsystemets disk</span><span class="sxs-lookup"><span data-stu-id="0fb28-115">Expand OS disk</span></span>

1. <span data-ttu-id="0fb28-116">Går inte att utföra åtgärder på virtuella hårddiskar med hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="0fb28-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="0fb28-117">hello följande exempel stoppar och tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="0fb28-117">hello following example stops and deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="0fb28-118">`azure vm stop`Frigör inte hello beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="0fb28-118">`azure vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="0fb28-119">toorelease beräkningsresurser använder `azure vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="0fb28-119">toorelease compute resources, use `azure vm deallocate`.</span></span> <span data-ttu-id="0fb28-120">hello VM att frigöra tooexpand hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="0fb28-120">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="0fb28-121">Uppdatera hello storleken på hello ohanterad OS-disken med hello `azure vm set` kommando.</span><span class="sxs-lookup"><span data-stu-id="0fb28-121">Update hello size of hello unmanaged OS disk using hello `azure vm set` command.</span></span> <span data-ttu-id="0fb28-122">följande exempel uppdateringar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup* toobe *50* GB:</span><span class="sxs-lookup"><span data-stu-id="0fb28-122">hello following example updates hello VM named *myVM* in hello resource group named *myResourceGroup* toobe *50* GB:</span></span>

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. <span data-ttu-id="0fb28-123">Starta den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0fb28-123">Start your VM as follows:</span></span>

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="0fb28-124">SSH tooyour VM med hello rätt behörighet.</span><span class="sxs-lookup"><span data-stu-id="0fb28-124">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="0fb28-125">storleken har tooverify hello OS-disk, Använd `df -h`.</span><span class="sxs-lookup"><span data-stu-id="0fb28-125">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="0fb28-126">hello följande exempel på utdata som visar hello primära partitionen (*/dev/sda1*) är nu 50 GB:</span><span class="sxs-lookup"><span data-stu-id="0fb28-126">hello following example output shows hello primary partition (*/dev/sda1*) is now 50 GB:</span></span>

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a><span data-ttu-id="0fb28-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0fb28-127">Next steps</span></span>
<span data-ttu-id="0fb28-128">Om du behöver ytterligare lagringsutrymme du också [lägga till data diskar tooa Linux VM](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="0fb28-128">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="0fb28-129">Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder hello Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="0fb28-129">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>
