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
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a>Expandera OS-disk på en Linux-VM med hello Azure CLI 1.0 hello Azure CLI
hello virtuell hårddisk standardstorlek för hello operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure. Du kan [lägga till datadiskar](add-disk.md) tooprovide för ytterligare lagringsutrymme, men du kan också tooexpand hello OS-disk. Den här artikeln beskrivs hur tooexpand hello OS-disken för en Linux-VM med hjälp av ohanterade diskar med hello Azure CLI 1.0.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#prerequisites) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](expand-disks.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="prerequisites"></a>Krav
Du behöver hello [senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och inloggad tooan [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) läget hello Resource Manager på följande sätt:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.

## <a name="expand-os-disk"></a>Expandera operativsystemets disk

1. Går inte att utföra åtgärder på virtuella hårddiskar med hello VM körs. hello följande exempel stoppar och tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop`Frigör inte hello beräkningsresurser. toorelease beräkningsresurser använder `azure vm deallocate`. hello VM att frigöra tooexpand hello virtuell hårddisk.

2. Uppdatera hello storleken på hello ohanterad OS-disken med hello `azure vm set` kommando. följande exempel uppdateringar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup* toobe *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. Starta den virtuella datorn på följande sätt:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour VM med hello rätt behörighet. storleken har tooverify hello OS-disk, Använd `df -h`. hello följande exempel på utdata som visar hello primära partitionen (*/dev/sda1*) är nu 50 GB:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Nästa steg
Om du behöver ytterligare lagringsutrymme du också [lägga till data diskar tooa Linux VM](add-disk.md). Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder hello Azure CLI](encrypt-disks.md).
