---
title: "Konvertera en virtuell Linux-dator i Azure från ohanterade diskar till hanterade diskar - hanterade diskar i Azure | Microsoft Docs"
description: "Konvertera en Linux VM från ohanterade diskar till hanterade diskar med hjälp av Azure CLI 2.0 i Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: 94f8e3330fb2d6547811315fcfdb8ced338e0247
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a><span data-ttu-id="02501-103">Konvertera en virtuell Linux-dator från ohanterade diskar till hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="02501-103">Convert a Linux virtual machine from unmanaged disks to managed disks</span></span>

<span data-ttu-id="02501-104">Om du har befintliga virtuella Linux-datorer (VM) som använder ohanterade diskar, kan du konvertera virtuella datorer om du vill använda hanterade diskar via den [Azure hanterade diskar](../windows/managed-disks-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="02501-104">If you have existing Linux virtual machines (VMs) that use unmanaged disks, you can convert the VMs to use managed disks through the [Azure Managed Disks](../windows/managed-disks-overview.md) service.</span></span> <span data-ttu-id="02501-105">Den här processen konverterar både OS-disken och eventuella anslutna hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="02501-105">This process converts both the OS disk and any attached data disks.</span></span>

<span data-ttu-id="02501-106">Den här artikeln visar hur du konverterar virtuella datorer med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="02501-106">This article shows you how to convert VMs by using the Azure CLI.</span></span> <span data-ttu-id="02501-107">Om du behöver installera eller uppgradera den, se [installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="02501-107">If you need to install or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="02501-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="02501-108">Before you begin</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a><span data-ttu-id="02501-109">Konvertera single instance virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="02501-109">Convert single-instance VMs</span></span>
<span data-ttu-id="02501-110">Det här avsnittet beskriver hur du konverterar single instance virtuella Azure-datorer från ohanterade diskar till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="02501-110">This section covers how to convert single-instance Azure VMs from unmanaged disks to managed disks.</span></span> <span data-ttu-id="02501-111">(Om dina virtuella datorer i en tillgänglighetsuppsättning, finns i nästa avsnitt.) Du kan använda den här processen för att konvertera de virtuella datorerna från premium (SSD) ohanterad diskar till hanterade premiumdiskar eller från standard (HDD) ohanterad diskar till hanterade standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="02501-111">(If your VMs are in an availability set, see the next section.) You can use this process to convert the VMs from premium (SSD) unmanaged disks to premium managed disks, or from standard (HDD) unmanaged disks to standard managed disks.</span></span>

1. <span data-ttu-id="02501-112">Frigör den virtuella datorn med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="02501-112">Deallocate the VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="02501-113">I följande exempel tar bort den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="02501-113">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. <span data-ttu-id="02501-114">Konvertera den virtuella datorn till hanterade diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="02501-114">Convert the VM to managed disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="02501-115">Följande process konverterar den virtuella datorn med namnet `myVM`, inklusive OS-disken och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="02501-115">The following process converts the VM named `myVM`, including the OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="02501-116">Starta den virtuella datorn efter konvertering till hanterade diskar med hjälp av [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="02501-116">Start the VM after the conversion to managed disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="02501-117">Följande exempel startar den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="02501-117">The following example starts the VM named `myVM` in the resource group named `myResourceGroup`.</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="02501-118">Konvertera virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="02501-118">Convert VMs in an availability set</span></span>

<span data-ttu-id="02501-119">Om de virtuella datorer som du vill konvertera till hanterade diskar är i en tillgänglighetsuppsättning, måste du först konvertera tillgänglighetsuppsättning till en hanterad tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="02501-119">If the VMs that you want to convert to managed disks are in an availability set, you first need to convert the availability set to a managed availability set.</span></span>

<span data-ttu-id="02501-120">Alla virtuella datorer i tillgänglighetsuppsättningen måste frigöras innan du konverterar tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="02501-120">All VMs in the availability set must be deallocated before you convert the availability set.</span></span> <span data-ttu-id="02501-121">Om du tänker konvertera alla virtuella datorer till hanterade diskar när tillgänglighetsuppsättning själva har konverterats till en hanterad tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="02501-121">Plan to convert all VMs to managed disks after the availability set itself has been converted to a managed availability set.</span></span> <span data-ttu-id="02501-122">Sedan starta de virtuella datorerna och fortsätta fungera som vanligt.</span><span class="sxs-lookup"><span data-stu-id="02501-122">Then, start all the VMs and continue operating as normal.</span></span>

1. <span data-ttu-id="02501-123">Lista alla virtuella datorer i en tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning lista](/cli/azure/vm/availability-set#list).</span><span class="sxs-lookup"><span data-stu-id="02501-123">List all VMs in an availability set by using [az vm availability-set list](/cli/azure/vm/availability-set#list).</span></span> <span data-ttu-id="02501-124">I följande exempel visar en lista över alla virtuella datorer i tillgänglighetsuppsättning namngivna `myAvailabilitySet` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="02501-124">The following example lists all VMs in the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. <span data-ttu-id="02501-125">Ta bort alla virtuella datorer med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="02501-125">Deallocate all the VMs by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="02501-126">I följande exempel tar bort den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="02501-126">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="02501-127">Konvertera tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning konvertera](/cli/azure/vm/availability-set#convert).</span><span class="sxs-lookup"><span data-stu-id="02501-127">Convert the availability set by using [az vm availability-set convert](/cli/azure/vm/availability-set#convert).</span></span> <span data-ttu-id="02501-128">I följande exempel konverterar tillgänglighetsuppsättning namngivna `myAvailabilitySet` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="02501-128">The following example converts the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. <span data-ttu-id="02501-129">Konvertera alla virtuella datorer till hanterade diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="02501-129">Convert all the VMs to managed disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="02501-130">Följande process konverterar den virtuella datorn med namnet `myVM`, inklusive OS-disken och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="02501-130">The following process converts the VM named `myVM`, including the OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. <span data-ttu-id="02501-131">Starta alla virtuella datorer efter konvertering till hanterade diskar med hjälp av [az vm start](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="02501-131">Start all the VMs after the conversion to managed disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="02501-132">Följande exempel startar den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="02501-132">The following example starts the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="02501-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02501-133">Next steps</span></span>
<span data-ttu-id="02501-134">Mer information om lagringsalternativ finns [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="02501-134">For more information about storage options, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>
