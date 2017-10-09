---
title: "aaaConvert en Linux-dator i Azure från ohanterade diskar toomanaged diskar - hanterade diskar i Azure | Microsoft Docs"
description: "Hur tooconvert en Linux VM från ohanterade diskar toomanaged diskar med hjälp av Azure CLI 2.0 i hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 1b94da11deab46f344e28ab4491cf220506b6347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="1f3a9-103">Konvertera en virtuell Linux-dator från ohanterade diskar toomanaged diskar</span><span class="sxs-lookup"><span data-stu-id="1f3a9-103">Convert a Linux virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="1f3a9-104">Om du har befintliga virtuella Linux-datorer (VM) som använder ohanterade diskar, kan du konvertera hello VMs toouse hanterade diskar via hello [Azure hanterade diskar](../windows/managed-disks-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-104">If you have existing Linux virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](../windows/managed-disks-overview.md) service.</span></span> <span data-ttu-id="1f3a9-105">Den här processen konverterar hello OS-disk- och eventuella anslutna hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="1f3a9-106">Den här artikeln visar hur tooconvert virtuella datorer med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-106">This article shows you how tooconvert VMs by using hello Azure CLI.</span></span> <span data-ttu-id="1f3a9-107">Om du behöver tooinstall eller uppgradera den [installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-107">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="1f3a9-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1f3a9-108">Before you begin</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a><span data-ttu-id="1f3a9-109">Konvertera single instance virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1f3a9-109">Convert single-instance VMs</span></span>
<span data-ttu-id="1f3a9-110">Det här avsnittet beskriver hur tooconvert single instance Azure virtuella datorer från ohanterade diskar toomanaged diskar.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-110">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="1f3a9-111">(Om dina virtuella datorer i en tillgänglighetsuppsättning, se hello nästa avsnitt.) Du kan använda den här processen tooconvert hello virtuella datorer från premium (SSD) ohanterad diskar toopremium hanterade diskar, eller från standard (HDD) ohanterad diskar toostandard hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-111">(If your VMs are in an availability set, see hello next section.) You can use this process tooconvert hello VMs from premium (SSD) unmanaged disks toopremium managed disks, or from standard (HDD) unmanaged disks toostandard managed disks.</span></span>

1. <span data-ttu-id="1f3a9-112">Frigöra hello VM med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-112">Deallocate hello VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="1f3a9-113">hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-113">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. <span data-ttu-id="1f3a9-114">Konvertera hello VM toomanaged diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-114">Convert hello VM toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="1f3a9-115">följande process konverterar hello hello virtuella datorn med namnet `myVM`, inklusive hello OS-disk- och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-115">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="1f3a9-116">Starta hello VM efter hello konvertering toomanaged diskar med hjälp av [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-116">Start hello VM after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="1f3a9-117">följande exempel startar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-117">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="1f3a9-118">Konvertera virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="1f3a9-118">Convert VMs in an availability set</span></span>

<span data-ttu-id="1f3a9-119">Om hello virtuella datorer som du vill tooconvert toomanaged diskar är i en tillgänglighetsuppsättning, måste du först tooconvert hello tillgänglighet set tooa hanteras tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-119">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

<span data-ttu-id="1f3a9-120">Alla virtuella datorer i tillgänglighetsuppsättningen hello måste frigöras innan du konverterar hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-120">All VMs in hello availability set must be deallocated before you convert hello availability set.</span></span> <span data-ttu-id="1f3a9-121">Planera tooconvert alla toomanaged diskar för virtuella datorer efter hello tillgänglighet satt har konverterade tooa hanteras tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-121">Plan tooconvert all VMs toomanaged disks after hello availability set itself has been converted tooa managed availability set.</span></span> <span data-ttu-id="1f3a9-122">Starta alla virtuella datorer hello sedan och fortsätta fungera som vanligt.</span><span class="sxs-lookup"><span data-stu-id="1f3a9-122">Then, start all hello VMs and continue operating as normal.</span></span>

1. <span data-ttu-id="1f3a9-123">Lista alla virtuella datorer i en tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning lista](/cli/azure/vm/availability-set#list).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-123">List all VMs in an availability set by using [az vm availability-set list](/cli/azure/vm/availability-set#list).</span></span> <span data-ttu-id="1f3a9-124">hello följande exempel visar en lista över alla virtuella datorer i hello tillgänglighetsuppsättning namngivna `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-124">hello following example lists all VMs in hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. <span data-ttu-id="1f3a9-125">Frigör alla hello virtuella datorer med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-125">Deallocate all hello VMs by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="1f3a9-126">hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-126">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="1f3a9-127">Konvertera hello tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning konvertera](/cli/azure/vm/availability-set#convert).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-127">Convert hello availability set by using [az vm availability-set convert](/cli/azure/vm/availability-set#convert).</span></span> <span data-ttu-id="1f3a9-128">hello följande exempel konverterar hello tillgänglighetsuppsättning namngivna `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-128">hello following example converts hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. <span data-ttu-id="1f3a9-129">Konvertera alla hello VMs toomanaged diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-129">Convert all hello VMs toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="1f3a9-130">följande process konverterar hello hello virtuella datorn med namnet `myVM`, inklusive hello OS-disk- och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-130">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. <span data-ttu-id="1f3a9-131">Starta alla virtuella datorer hello efter hello konvertering toomanaged diskar med hjälp av [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-131">Start all hello VMs after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="1f3a9-132">följande exempel startar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="1f3a9-132">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="1f3a9-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f3a9-133">Next steps</span></span>
<span data-ttu-id="1f3a9-134">Mer information om lagringsalternativ finns [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f3a9-134">For more information about storage options, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>
