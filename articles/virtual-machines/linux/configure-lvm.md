---
title: "aaaConfigure LVM på en virtuell dator som kör Linux | Microsoft Docs"
description: "Lär dig hur tooconfigure LVM på Linux i Azure."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 8daf792d87c6bb3d91a2eddcd01cfab34fd28cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurera LVM på en virtuell Linux-dator i Azure
Det här dokumentet innehåller information om hur tooconfigure logiska volymen Manager (LVM) i ditt virtuella Azure-datorn. Även om det är möjligt tooconfigure LVM på en disk ansluten toohello virtuell dator som standard de flesta molnet bilder inte har konfigurerats LVM på hello OS-disken. Detta är tooprevent problem med dubbla volym grupper om hello OS-disken är någonsin kopplade tooanother VM av hello samma distribution och typ, dvs. under en återställningsscenario. Därför rekommenderas endast toouse LVM på hello datadiskar.

## <a name="linear-vs-striped-logical-volumes"></a>Linjär kontra logiska stripe-volymer
LVM kan vara används toocombine ett antal fysiska diskar i en enda lagringsvolym. Som standard skapar LVM vanligtvis linjär logiska volymer, vilket innebär att hello fysisk lagring sammanfogas tillsammans. I det här fallet skickas läs-/ skrivåtgärder vanligtvis bara tooa enskild disk. Däremot kan vi skapa stripe-logiska volymer där läsningar och skrivningar är distribuerade toomultiple diskar i hello volym grupp (d.v.s. liknande tooRAID0). Av prestandaskäl är förmodligen vill toostripe dina logiska volymer så att använda alla anslutna datadiskar läsningar och skrivningar.

Det här dokumentet beskrivs hur toocombine data flera diskar i en enda volym-grupp och sedan skapa en logisk stripe-volym. hello stegen nedan är något generaliserad toowork med de flesta distributioner. I de flesta fall hello verktyg och arbetsflöden för att hantera LVM i Azure är inte helt annorlunda än andra miljöer. Som vanligt också kontakta leverantören av Linux för dokumentation och bästa praxis för att använda LVM med en viss distribution.

## <a name="attaching-data-disks"></a>Bifoga datadiskar
En ska vanligtvis toostart med två eller flera diskar i empty-data när du använder LVM. Baserat på i/o-behov kan du välja tooattach diskar som är lagrade i vår standardlagring med upp too500 IO/ps per disk eller våra Premium-lagring med upp too5000 IO/ps per disk. Den här artikeln kommer inte gå in i detalj på hur tooprovision och bifoga data diskar tooa Linux-dator. Se hello Microsoft Azure artikel [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur tooattach en tom data disk tooa Linux-dator i Azure.

## <a name="install-hello-lvm-utilities"></a>Installera hello LVM verktyg
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS och Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 och openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    På SLES11 måste du också redigera `/etc/sysconfig/lvm` och ange `LVM_ACTIVATED_ON_DISCOVERED` för ”aktivera”:

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>Konfigurera LVM
I den här guiden antar vi att du har kopplat tre datadiskar som vi ska refererar tooas `/dev/sdc`, `/dev/sdd` och `/dev/sde`. Observera att dessa inte kan alltid vara hello samma sökvägsnamn i den virtuella datorn. Du kan köra '`sudo fdisk -l`' eller liknande kommandot toolist din tillgängliga diskar.

1. Förbered hello fysiska volymer:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Skapa en grupp för volymen. I det här exemplet vi ringer hello volym grupp `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Skapa hello logiska volymerna. hello-kommandot nedan vi skapar en logisk volym kallas `data-lv01` toospan hello hela volymen gruppera, men Observera att det är också möjligt toocreate flera logiska volymer i hello volym grupp.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Formatera hello logiska volym

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Med SLES11 använder `-t ext3` i stället för ext4. SLES11 stöder bara läsbehörighet tooext4 filsystem.

## <a name="add-hello-new-file-system-tooetcfstab"></a>Lägg till hello nya filen system för/etc/fstab
> [!IMPORTANT]
> Felaktigt redigera hello `/etc/fstab` filen kan resultera i ett system som inte kan startas. Om du är osäker, se toohello distribution dokumentation för information om hur tooproperly redigera den här filen. Det är också rekommenderar att en säkerhetskopia av hello `/etc/fstab` filen har skapats innan du redigerar.

1. Skapa hello önskad monteringspunkt för det nya filsystemet, till exempel:

    ```bash  
    sudo mkdir /data
    ```

2. Leta upp hello logiska volymsökväg

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Öppna `/etc/fstab` i en textredigerare och Lägg till en post för hello nytt filsystem, till exempel:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Spara och Stäng `/etc/fstab`.

4. Testa att hello `/etc/fstab` inmatning är korrekt:

    ```bash    
    sudo mount -a
    ```

    Om det här kommandot resulterar i ett felmeddelande du kontrollera hello syntax i hello `/etc/fstab` fil.
   
    Kör nästa gång hello `mount` kommandot tooensure hello är monterat:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Valfritt) Felsäker Startparametrar i`/etc/fstab`
   
    Många distributioner som innehåller antingen hello `nobootwait` eller `nofail` montera parametrar som kan läggas till toohello `/etc/fstab` fil. Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt hello Linux system toocontinue tooboot även om det är tooproperly montera hello RAID-filsystem. Mer information om dessa parametrar finns i tooyour distribution-dokumentationen.
   
    Exempel (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>Stöd för TRIM/UNMAP
Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk. Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort. Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.

Det finns två sätt tooenable TRIMNING stöd i Linux-VM. Som vanligt, kontakta din distribution för hello rekommendationer:

- Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- I vissa fall hello `discard` alternativet kanske prestanda. Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
