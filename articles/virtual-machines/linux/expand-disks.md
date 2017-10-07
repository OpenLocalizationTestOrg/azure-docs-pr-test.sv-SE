---
title: "aaaExpand virtuella hårddiskar på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur tooexpand virtuella hårddiskar på en Linux-VM med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 7c09a682cb4322c027e57667640e8f1f8e6612f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a>Hur tooexpand virtuella hårddiskar på en Linux-VM med hello Azure CLI
hello virtuell hårddisk standardstorlek för hello operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure. Du kan [lägga till datadiskar](add-disk.md) tooprovide för ytterligare lagringsutrymme, men du kan också tooexpand en befintlig datadisk. Den här artikeln beskrivs hur tooexpand hanterade diskar för en Linux-VM med hello Azure CLI 2.0. Du kan också expandera hello ohanterad OS-disken med hello [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Kontrollera alltid att du säkerhetskopierar dina data innan du utför disk ändra storlek på åtgärder. Mer information finns i [säkerhetskopiera virtuella Linux-datorer i Azure](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Expandera disk
Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Den här artikeln kräver en befintlig virtuell dator i Azure med minst en datadisk ansluten och förberetts. Om du inte redan har en virtuell dator som du kan använda finns [skapa och förbereda en virtuell dator med datadiskar](tutorial-manage-disks.md#create-and-attach-disks).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.

1. Går inte att utföra åtgärder på virtuella hårddiskar med hello VM körs. Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop`Frigör inte hello beräkningsresurser. toorelease beräkningsresurser använder `az vm deallocate`. hello VM att frigöra tooexpand hello virtuell hårddisk.

2. Visa en lista över hanterade diskar i en resursgrupp med [az Disklista](/cli/azure/disk#list). hello följande exempel visar en lista över hanterade diskar i hello resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Expandera hello diskutrymme med [az disk uppdatering](/cli/azure/disk#update). hello följande exempel expanderas hello hanterade disk med namnet *myDataDisk* toobe *200*Gb i storlek:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > När du expanderar en hanterad disk är hello uppdateras storleken mappade toohello närmsta hanterade diskstorleken. En tabell med hello tillgängliga hanterade diskstorlekar och nivåer finns [hanterade diskar översikt över Azure - priser och fakturering](../windows/managed-disks-overview.md#pricing-and-billing).

3. Starta den virtuella datorn med [az vm start](/cli/azure/vm#start). följande exempel startar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour VM med hello rätt behörighet. Du kan hämta hello offentliga IP-adressen för den virtuella datorn med [az vm visa](/cli/azure/vm#show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. toouse hello expanderas disk måste du tooexpand hello underliggande partition och filsystem.

    a. Om du redan är monterad demontera hello disk:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Använd `parted` tooview disk information och ändra storlek på hello-partition:

    ```bash
    sudo parted /dev/sdc
    ```

    Visa information om hello befintliga partitionslayout med `print`. hello utdata är liknande toohello följande exempel som visar hello underliggande disk är 215Gb i storlek:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome tooGNU Parted! Type 'help' tooview a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Expandera hello partitionen med `resizepart`. Ange hello partitionsnumret *1*, och en storlek för hello ny partition:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. tooexit, ange`quit`

5. Hello partition storlek, verifiera hello partition konsekvens med `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Nu ändra storlek på hello filsystem med `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Montera hello partition toohello önskad plats, t.ex `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. storleken har tooverify hello OS-disk, Använd `df -h`. hello följande exempel på utdata som visar hello dataenhet */dev/sdc1*, är nu 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Nästa steg
Om du behöver ytterligare lagringsutrymme du också [lägga till data diskar tooa Linux VM](add-disk.md). Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder hello Azure CLI](encrypt-disks.md).
