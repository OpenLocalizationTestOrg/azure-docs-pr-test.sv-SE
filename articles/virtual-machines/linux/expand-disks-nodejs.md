---
title: "Expandera OS-disken på Linux VM med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du expandera den virtuella disken operativsystem (OS) på en Linux VM som använder Azure CLI 1.0 och Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: 0aedcd70b54c2ed47ec327ccf0529a48351353c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-the-azure-cli-with-the-azure-cli-10"></a><span data-ttu-id="cc936-103">Expandera OS-disk på en Linux VM som använder Azure CLI med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cc936-103">Expand OS disk on a Linux VM using the Azure CLI with the Azure CLI 1.0</span></span>
<span data-ttu-id="cc936-104">Standardstorleken för virtuell hårddisk för operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="cc936-104">The default virtual hard disk size for the operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="cc936-105">Du kan [lägga till datadiskar](add-disk.md) att tillhandahålla för ytterligare lagringsutrymme, men du kan också expandera OS-disk.</span><span class="sxs-lookup"><span data-stu-id="cc936-105">You can [add data disks](add-disk.md) to provide for additional storage space, but you may also wish to expand the OS disk.</span></span> <span data-ttu-id="cc936-106">Den här artikeln beskriver hur du expandera OS-disk för en Linux-VM med hjälp av ohanterade diskar med Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="cc936-106">This article details how to expand the OS disk for a Linux VM using unmanaged disks with the Azure CLI 1.0.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="cc936-107">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="cc936-107">CLI versions to complete the task</span></span>
<span data-ttu-id="cc936-108">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="cc936-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="cc936-109">[Azure CLI 1.0](#prerequisites) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="cc936-109">[Azure CLI 1.0](#prerequisites) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="cc936-110">[Azure CLI 2.0](expand-disks.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="cc936-110">[Azure CLI 2.0](expand-disks.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc936-111">Krav</span><span class="sxs-lookup"><span data-stu-id="cc936-111">Prerequisites</span></span>
<span data-ttu-id="cc936-112">Du behöver den [senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och inloggad på ett [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) med hjälp av hanteraren för filserverresurser på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="cc936-112">You need the [latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in to an [Azure account](https://azure.microsoft.com/pricing/free-trial/) using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="cc936-113">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="cc936-113">In the following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cc936-114">Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="cc936-114">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

## <a name="expand-os-disk"></a><span data-ttu-id="cc936-115">Expandera operativsystemets disk</span><span class="sxs-lookup"><span data-stu-id="cc936-115">Expand OS disk</span></span>

1. <span data-ttu-id="cc936-116">Det går inte att utföra åtgärder på virtuella hårddiskar med den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="cc936-116">Operations on virtual hard disks cannot be performed with the VM running.</span></span> <span data-ttu-id="cc936-117">I följande exempel stoppar och tar bort den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="cc936-117">The following example stops and deallocates the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="cc936-118">`azure vm stop`Frigör inte beräkningsresurserna.</span><span class="sxs-lookup"><span data-stu-id="cc936-118">`azure vm stop` does not release the compute resources.</span></span> <span data-ttu-id="cc936-119">Använd om du vill frigöra beräkningsresurser `azure vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="cc936-119">To release compute resources, use `azure vm deallocate`.</span></span> <span data-ttu-id="cc936-120">Den virtuella datorn måste frigöras för att expandera den virtuella hårddisken.</span><span class="sxs-lookup"><span data-stu-id="cc936-120">The VM must be deallocated to expand the virtual hard disk.</span></span>

2. <span data-ttu-id="cc936-121">Uppdatera storleken på en ohanterad OS disk med hjälp av den `azure vm set` kommando.</span><span class="sxs-lookup"><span data-stu-id="cc936-121">Update the size of the unmanaged OS disk using the `azure vm set` command.</span></span> <span data-ttu-id="cc936-122">I följande exempel uppdateras den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup* ska *50* GB:</span><span class="sxs-lookup"><span data-stu-id="cc936-122">The following example updates the VM named *myVM* in the resource group named *myResourceGroup* to be *50* GB:</span></span>

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. <span data-ttu-id="cc936-123">Starta den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="cc936-123">Start your VM as follows:</span></span>

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="cc936-124">SSH till den virtuella datorn med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cc936-124">SSH to your VM with the appropriate credentials.</span></span> <span data-ttu-id="cc936-125">Använd för att kontrollera att OS-disken har ändrats, `df -h`.</span><span class="sxs-lookup"><span data-stu-id="cc936-125">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="cc936-126">Följande exempel visas den primära partitionen (*/dev/sda1*) är nu 50 GB:</span><span class="sxs-lookup"><span data-stu-id="cc936-126">The following example output shows the primary partition (*/dev/sda1*) is now 50 GB:</span></span>

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a><span data-ttu-id="cc936-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc936-127">Next steps</span></span>
<span data-ttu-id="cc936-128">Om du behöver ytterligare lagringsutrymme du också [lägga till diskar till en Linux-VM](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="cc936-128">If you need additional storage, you also [add data disks to a Linux VM](add-disk.md).</span></span> <span data-ttu-id="cc936-129">Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="cc936-129">For more information about disk encryption, see [Encrypt disks on a Linux VM using the Azure CLI](encrypt-disks.md).</span></span>
