---
title: "Expandera virtuella hårddiskar på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur du expandera virtuella hårddiskar på en Linux-VM med Azure CLI 2.0"
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
ms.openlocfilehash: b82cc0473c003da767ee230ab485c69b233977d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Så här expanderar virtuella hårddiskar på en Linux-VM med Azure CLI
Standardstorleken för virtuell hårddisk för operativsystem (OS) är vanligtvis 30 GB på en Linux-dator (VM) i Azure. Du kan [lägga till datadiskar](add-disk.md) att tillhandahålla för ytterligare lagringsutrymme, men du kan också expandera en befintlig datadisk. Den här artikeln beskriver hur du expandera hanterade diskar för en Linux-VM med Azure CLI 2.0. Du kan också expandera ohanterade OS-disk med den [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Kontrollera alltid att du säkerhetskopierar dina data innan du utför disk ändra storlek på åtgärder. Mer information finns i [säkerhetskopiera virtuella Linux-datorer i Azure](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Expandera disk
Se till att du har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).

Den här artikeln kräver en befintlig virtuell dator i Azure med minst en datadisk ansluten och förberetts. Om du inte redan har en virtuell dator som du kan använda finns [skapa och förbereda en virtuell dator med datadiskar](tutorial-manage-disks.md#create-and-attach-disks).

Ersätt exempel parameternamn med egna värden i följande exempel. Exempel parameternamn inkluderar *myResourceGroup* och *myVM*.

1. Det går inte att utföra åtgärder på virtuella hårddiskar med den virtuella datorn körs. Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate). I följande exempel tar bort den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop`Frigör inte beräkningsresurserna. Använd om du vill frigöra beräkningsresurser `az vm deallocate`. Den virtuella datorn måste frigöras för att expandera den virtuella hårddisken.

2. Visa en lista över hanterade diskar i en resursgrupp med [az Disklista](/cli/azure/disk#list). I följande exempel visas en lista över hanterade diskar i resursgruppen med namnet *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Expandera diskutrymme med [az disk uppdatering](/cli/azure/disk#update). I följande exempel expanderas hanterade disken med namnet *myDataDisk* ska *200*Gb i storlek:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > När du expanderar en hanterad disk är uppdaterade storleken mappad till den närmaste hantera diskstorleken. En tabell med tillgängliga hanterade diskstorlekar och nivåer finns [hanterade diskar översikt över Azure - priser och fakturering](../windows/managed-disks-overview.md#pricing-and-billing).

3. Starta den virtuella datorn med [az vm start](/cli/azure/vm#start). Följande exempel startar den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH till den virtuella datorn med rätt autentiseringsuppgifter. Du kan hämta den offentliga IP-adressen på den virtuella datorn med [az vm visa](/cli/azure/vm#show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. Om du vill använda den expanderade disken måste du expandera den underliggande partitionen och filsystem.

    a. Om du redan är monterad demontera disken:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Använd `parted` att visa information och ändra storlek på partitionen:

    ```bash
    sudo parted /dev/sdc
    ```

    Visa information om befintliga partitionslayout med `print`. Utdata liknar följande exempel som visar den underliggande disken är 215Gb i storlek:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Expandera partitionen med `resizepart`. Ange partitionsnumret *1*, och en storlek för den nya partitionen:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Om du vill avsluta, ange`quit`

5. Partitionen storlek, verifiera partition konsekvens med `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Nu ändra storlek på filsystemet med `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Montera partitionen till önskad plats som `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. Använd för att kontrollera att OS-disken har ändrats, `df -h`. Följande exempel visas dataenheten, */dev/sdc1*, är nu 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Nästa steg
Om du behöver ytterligare lagringsutrymme du också [lägga till diskar till en Linux-VM](add-disk.md). Mer information om diskkryptering finns [kryptera diskar på en Linux VM som använder Azure CLI](encrypt-disks.md).
