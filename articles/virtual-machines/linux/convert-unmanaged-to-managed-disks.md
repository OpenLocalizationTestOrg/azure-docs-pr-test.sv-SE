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
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Konvertera en virtuell Linux-dator från ohanterade diskar till hanterade diskar

Om du har befintliga virtuella Linux-datorer (VM) som använder ohanterade diskar, kan du konvertera virtuella datorer om du vill använda hanterade diskar via den [Azure hanterade diskar](../windows/managed-disks-overview.md) service. Den här processen konverterar både OS-disken och eventuella anslutna hårddiskar.

Den här artikeln visar hur du konverterar virtuella datorer med hjälp av Azure CLI. Om du behöver installera eller uppgradera den, se [installera Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Innan du börjar

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Konvertera single instance virtuella datorer
Det här avsnittet beskriver hur du konverterar single instance virtuella Azure-datorer från ohanterade diskar till hanterade diskar. (Om dina virtuella datorer i en tillgänglighetsuppsättning, finns i nästa avsnitt.) Du kan använda den här processen för att konvertera de virtuella datorerna från premium (SSD) ohanterad diskar till hanterade premiumdiskar eller från standard (HDD) ohanterad diskar till hanterade standarddiskar.

1. Frigör den virtuella datorn med hjälp av [az vm frigöra](/cli/azure/vm#deallocate). I följande exempel tar bort den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Konvertera den virtuella datorn till hanterade diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert). Följande process konverterar den virtuella datorn med namnet `myVM`, inklusive OS-disken och eventuella hårddiskar:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Starta den virtuella datorn efter konvertering till hanterade diskar med hjälp av [az vm start](/cli/azure/vm#start). Följande exempel startar den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Konvertera virtuella datorer i en tillgänglighetsuppsättning

Om de virtuella datorer som du vill konvertera till hanterade diskar är i en tillgänglighetsuppsättning, måste du först konvertera tillgänglighetsuppsättning till en hanterad tillgänglighetsuppsättningen.

Alla virtuella datorer i tillgänglighetsuppsättningen måste frigöras innan du konverterar tillgänglighetsuppsättningen. Om du tänker konvertera alla virtuella datorer till hanterade diskar när tillgänglighetsuppsättning själva har konverterats till en hanterad tillgänglighetsuppsättning. Sedan starta de virtuella datorerna och fortsätta fungera som vanligt.

1. Lista alla virtuella datorer i en tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning lista](/cli/azure/vm/availability-set#list). I följande exempel visar en lista över alla virtuella datorer i tillgänglighetsuppsättning namngivna `myAvailabilitySet` i resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Ta bort alla virtuella datorer med hjälp av [az vm frigöra](/cli/azure/vm#deallocate). I följande exempel tar bort den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Konvertera tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning konvertera](/cli/azure/vm/availability-set#convert). I följande exempel konverterar tillgänglighetsuppsättning namngivna `myAvailabilitySet` i resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Konvertera alla virtuella datorer till hanterade diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert). Följande process konverterar den virtuella datorn med namnet `myVM`, inklusive OS-disken och eventuella hårddiskar:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Starta alla virtuella datorer efter konvertering till hanterade diskar med hjälp av [az vm start](/cli/azure/vm#start). Följande exempel startar den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Nästa steg
Mer information om lagringsalternativ finns [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md).
