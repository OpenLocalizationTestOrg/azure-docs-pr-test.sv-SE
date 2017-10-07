---
title: "aaaConfigure programvarubaserad RAID på en virtuell dator som kör Linux | Microsoft Docs"
description: "Lär dig hur toouse mdadm tooconfigure RAID på Linux i Azure."
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.openlocfilehash: f06e2679d953faf88ffee9991226cdb3cc1cbdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-software-raid-on-linux"></a>Konfigurera programvaru-RAID på Linux
Det är ett vanligt scenario toouse programvarubaserad RAID på Linux virtuella datorer i Azure toopresent flera bifogade data diskar som en enkel RAID-enhet. Vanligtvis kan vara används tooimprove prestanda och möjliggör bättre genomflöde jämfört med toousing bara en enda disk.

## <a name="attaching-data-disks"></a>Bifoga datadiskar
Två eller flera tom datadiskar är nödvändiga tooconfigure en RAID-enhet.  hello främsta orsaken för att skapa en RAID-enhet är tooimprove prestanda diskens i/o.  Baserat på i/o-behov kan du välja tooattach diskar som är lagrade i vår standardlagring med upp too500 IO/ps per disk eller våra Premium-lagring med upp too5000 IO/ps per disk. Den här artikeln inte gå in i detalj på hur tooprovision och bifoga data diskar tooa Linux-dator.  Se hello Microsoft Azure artikel [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur tooattach en tom data disk tooa Linux-dator i Azure.

## <a name="install-hello-mdadm-utility"></a>Installera hello mdadm verktyget
* **Ubuntu**
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* **CentOS & Oracle Linux**
```bash
sudo yum install mdadm
```

* **SLES och openSUSE**
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a>Skapa hello diskpartitioner
I det här exemplet skapar vi en enskild diskpartition på /dev/sdc. hello nya diskpartition kallas /dev/sdc1.

1. Starta `fdisk` toobegin skapar partitioner

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide toowrite them.
    After that, of course, hello previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off hello mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. Tryck på ”n” på hello fråga toocreate en  **n** y-partition:

    ```bash
    Command (m for help): n
    ```

3. Tryck sedan på ”p” toocreate en **p**rimär partition:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. Tryck på '1' tooselect partitionsnummer 1:

    ```bash
    Partition number (1-4): 1
    ```

5. Välj hello startpunkten för hello ny partition eller tryck på `<enter>` tooaccept hello tooplace hello standardpartition hello början hello ledigt utrymme på enheten hello:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. Välj hello storlek för hello partition, till exempel typ '+10G' toocreate en 10 GB-partition. Eller tryck på `<enter>` skapa en enda partition som sträcker sig över hela enheten för hello:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. Ändra hello-ID och **t**typ av hello partition från hello standard ID '83' (Linux) tooID 'fd' (Linux raid auto):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. Slutligen skriva hello partitionera tabellen toohello enhet och avsluta fdisk:

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a>Skapa hello RAID-matris
1. hello följande exempel kommer ”stripe” (RAID-nivå 0) tre partitioner finns på tre separata hårddiskar (sdc1, sdd1, sde1).  När du kör detta kommando en ny RAID-enhet kallas **/dev/md127** skapas. Observera också att det kan vara nödvändigt tooadd hello om dessa data diskar vi tidigare tillhör en annan felaktiga RAID-matris `--force` parametern toohello `mdadm` kommando:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. Skapa hello filsystemet på hello nya RAID-enhet
   
    a. **CentOS, Oracle Linux, SLES 12, openSUSE och Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** – aktivera boot.md och skapa mdadm.conf

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > En omstart kan krävas när du har gjort dessa ändringar på SUSE system. Det här steget är *inte* krävs på SLES 12.
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a>Lägg till hello nya filen system för/etc/fstab
> [!IMPORTANT]
> Felaktigt Filredigering hello /etc/fstab kan resultera i ett system som inte kan startas. Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen. Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.

1. Skapa hello önskad monteringspunkt för det nya filsystemet, till exempel:

    ```bash
    sudo mkdir /data
    ```
2. När du redigerar /etc/fstab hello **UUID** ska vara används tooreference hello file system i stället för hello enhetsnamn.  Använd hello `blkid` verktyget toodetermine hello UUID för hello nytt filsystem:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. Öppna /etc/fstab i en textredigerare och Lägg till en post för hello nytt filsystem, till exempel:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Eller på **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Spara och Stäng /etc/fstab.

4. Testa att hello/etc/fstab inmatning är korrekt:

    ```bash  
    sudo mount -a
    ```

    Om det här kommandot resulterar i ett felmeddelande, kontrollera hello syntax i hello /etc/fstab-filen.
   
    Kör nästa gång hello `mount` kommandot tooensure hello är monterat:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. (Valfritt) Felsäker start-parametrar
   
    **fstab konfiguration**
   
    Många distributioner som innehåller antingen hello `nobootwait` eller `nofail` montera parametrar som kan läggas till filen toohello/etc/fstab. Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt hello Linux system toocontinue tooboot även om det är tooproperly montera hello RAID-filsystem. Mer information om dessa parametrar finns i tooyour distribution dokumentation.
   
    Exempel (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux-Startparametrar**
   
    Hej kernel-parametern i tillägg toohello ovan parametrar ”,`bootdegraded=true`” kan tillåta hello system tooboot även om hello RAID uppfattas som skadas eller försämrad, till exempel om en dataenhet oavsiktligt tas bort från hello virtuell dator. Det kan också leda till ett ej startbar system för som standard.
   
    Se tooyour distribution dokumentation om hur tooproperly redigera kernel-parametrar. Till exempel i många distributioner (CentOS, Oracle Linux, SLES 11) dessa parametrar kan läggas till manuellt toohello ”`/boot/grub/menu.lst`” filen.  På Ubuntu den här parametern kan läggas till toohello `GRUB_CMDLINE_LINUX_DEFAULT` variabeln på ”/ etc/standard/grub”.


## <a name="trimunmap-support"></a>Stöd för TRIM/UNMAP
Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk. Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort. Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.

> [!NOTE]
> RAID kan inte utfärda kommandon på Ignorera om hello segmentstorleken för hello matrisen är tooless än hello standardvärdet (512KB). Detta beror på att ta bort mappning hello granularitet på hello värden är också 512KB. Om du har ändrat hello matrisens segmentstorleken via mdadm's `--chunk=` parameter och sedan ta bort TRIMNING/mappning begäranden ignoreras av hello kernel.

Det finns två sätt tooenable TRIMNING stöd i Linux-VM. Som vanligt, kontakta din distribution för hello rekommendationer:

- Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- I vissa fall hello `discard` alternativet kanske prestanda. Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL/CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
