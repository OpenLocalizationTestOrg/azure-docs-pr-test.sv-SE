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
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>Konvertera en virtuell Linux-dator från ohanterade diskar toomanaged diskar

Om du har befintliga virtuella Linux-datorer (VM) som använder ohanterade diskar, kan du konvertera hello VMs toouse hanterade diskar via hello [Azure hanterade diskar](../windows/managed-disks-overview.md) service. Den här processen konverterar hello OS-disk- och eventuella anslutna hårddiskar.

Den här artikeln visar hur tooconvert virtuella datorer med hjälp av hello Azure CLI. Om du behöver tooinstall eller uppgradera den [installera Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Innan du börjar

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Konvertera single instance virtuella datorer
Det här avsnittet beskriver hur tooconvert single instance Azure virtuella datorer från ohanterade diskar toomanaged diskar. (Om dina virtuella datorer i en tillgänglighetsuppsättning, se hello nästa avsnitt.) Du kan använda den här processen tooconvert hello virtuella datorer från premium (SSD) ohanterad diskar toopremium hanterade diskar, eller från standard (HDD) ohanterad diskar toostandard hanterade diskar.

1. Frigöra hello VM med hjälp av [az vm frigöra](/cli/azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Konvertera hello VM toomanaged diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert). följande process konverterar hello hello virtuella datorn med namnet `myVM`, inklusive hello OS-disk- och eventuella hårddiskar:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Starta hello VM efter hello konvertering toomanaged diskar med hjälp av [az vm start](/cli/azure/vm#start). följande exempel startar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Konvertera virtuella datorer i en tillgänglighetsuppsättning

Om hello virtuella datorer som du vill tooconvert toomanaged diskar är i en tillgänglighetsuppsättning, måste du först tooconvert hello tillgänglighet set tooa hanteras tillgänglighetsuppsättning.

Alla virtuella datorer i tillgänglighetsuppsättningen hello måste frigöras innan du konverterar hello tillgänglighetsuppsättning. Planera tooconvert alla toomanaged diskar för virtuella datorer efter hello tillgänglighet satt har konverterade tooa hanteras tillgänglighetsuppsättning. Starta alla virtuella datorer hello sedan och fortsätta fungera som vanligt.

1. Lista alla virtuella datorer i en tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning lista](/cli/azure/vm/availability-set#list). hello följande exempel visar en lista över alla virtuella datorer i hello tillgänglighetsuppsättning namngivna `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Frigör alla hello virtuella datorer med hjälp av [az vm frigöra](/cli/azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Konvertera hello tillgänglighetsuppsättning med hjälp av [az vm-tillgänglighetsuppsättning konvertera](/cli/azure/vm/availability-set#convert). hello följande exempel konverterar hello tillgänglighetsuppsättning namngivna `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Konvertera alla hello VMs toomanaged diskar med hjälp av [az vm konvertera](/cli/azure/vm#convert). följande process konverterar hello hello virtuella datorn med namnet `myVM`, inklusive hello OS-disk- och eventuella hårddiskar:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Starta alla virtuella datorer hello efter hello konvertering toomanaged diskar med hjälp av [az vm start](/cli/azure/vm#start). följande exempel startar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Nästa steg
Mer information om lagringsalternativ finns [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md).
