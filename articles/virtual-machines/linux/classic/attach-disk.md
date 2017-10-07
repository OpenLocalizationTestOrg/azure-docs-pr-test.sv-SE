---
title: aaaAttach disk-tooa Linux VM i Azure | Microsoft Docs
description: "Lär dig hur tooattach data disk tooa Linux VM med hello klassisk distribution modell och initiera hello disk så att den är redo för användning"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: c76d8479ac2b522d2b6df658cd28f242473f30ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a>Hur tooAttach en datadisk tooa virtuell Linux-dator
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Se hur för[ansluta en datadisk med hello Resource Manager-distributionsmodellen](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Du kan koppla både tom och diskar som innehåller data tooyour virtuella Azure-datorer. Båda typer av diskar är VHD-filer som finns i ett Azure storage-konto. Som med att lägga till en disk tooa Linux-dator, när du har kopplat hello disk du behöver tooinitialize och formatera den så att den är redo för användning. Den här artikeln information som kopplar både tom diskar och diskar som redan innehåller data tooyour virtuella datorer, samt hur toothen initiera och formatera en ny disk.

> [!NOTE]
> Det är en bästa praxis toouse en eller flera olika diskar toostore data för en virtuell dator. När du skapar en virtuell Azure-dator har en operativsystemdisk och en tillfällig disk. **Använd inte hello diskutrymme toostore beständiga data.** Hello namnet antyder ger den tillfälligt lagringsutrymme. Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.
> hello diskutrymme hanteras vanligtvis av hello Azure Linux-agenten och automatiskt monterade för**/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar). Hej på andra sidan, en datadisk kan kallas av hello Linux kernel ungefär `/dev/sdc`, och du behöver toopartition, formatera och montera den här resursen. Se hello [användarhandboken för Azure Linux-agenten] [ Agent] mer information.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initiera en ny datadisk i Linux
1. SSH tooyour VM. Mer information finns i [hur toolog på tooa virtuell dator som kör Linux][Logon].
2. Därefter behöver du toofind hello enhets-ID för hello data disk tooinitialize. Det finns två sätt toodo som:
   
    a) Grep för SCSI-enheter i hello loggar, exempelvis hello följande kommando:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Om du nyligen Ubuntu distributioner måste toouse `sudo grep SCSI /var/log/syslog` eftersom loggar för`/var/log/messages` kan vara inaktiverade som standard.
   
    Du kan hitta hello identifierare för hello senaste datadisk som har lagts till i hello-meddelanden som visas.
   
    ![Hämta hälsningsmeddelande för disk](./media/attach-disk/scsidisklog.png)
   
    ELLER
   
    b) Använd hello `lsscsi` kommandot toofind ut hello enhets-id. `lsscsi` kan installeras antingen `yum install lsscsi` (Red Hat baserad på distributioner) eller `apt-get install lsscsi` (Debian baserad på distributioner). Du kan hitta hello-disk som du letar efter av dess *lun* eller **logiskt enhetsnummer**. Till exempel hello *lun* för hello diskar bifogade kan enkelt se `azure vm disk list <virtual-machine>` som:

    ```azurecli
    azure vm disk list myVM
    ```

    hello-utdata är liknande toohello följande:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Jämföra dessa data med hello utdata från `lsscsi` för hello samma prov virtuell dator:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    hello senaste antalet hello tuppel i varje rad är hello *lun*. Se `man lsscsi` för mer information.
3. I hello kommandotolk, skriver du följande kommando toocreate hello enheten:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. När du uppmanas ange  **n**  toocreate en partition.

    ![Skapa enhet](./media/attach-disk/fdisknewpartition.png)

5. När du uppmanas ange **p** toomake hello partition hello primära partitionen. Typen **1** toomake den hello första partitionen och skriv sedan ange tooaccept hello standardvärde för hello cylinder. Den kan visa hello standardvärdena för hello först och hello senaste sektorer, i stället för hello cylinder på vissa datorer. Du kan välja tooaccept dessa standardinställningar.

    ![Skapa partition](./media/attach-disk/fdisknewpartdetails.png)


6. Typen **p** toosee hello information om hello-disk som är är partitionerad.

    ![Diskinformation för listan](./media/attach-disk/fdiskpartitiondetails.png)


7. Typen **w** toowrite hello inställningarna för hello disk.

    ![Skriva hello disk ändringar](./media/attach-disk/fdiskwritedisk.png)

8. Du kan nu skapa hello filsystemet på hello ny partition. Lägg till hello partition nummer toohello enhets-ID (i följande exempel hello `/dev/sdc1`). hello följande exempel skapas en ext4 partition på /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Skapa filsystem](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 system stöder bara läsbehörighet för ext4 filsystem. För de här systemen bör tooformat hello nytt filsystem som ext3 i stället för ext4.

9. Gör ett directory toomount hello nytt filsystem, enligt följande:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Slutligen kan du montera hello enheten enligt följande:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    Hej datadisk är nu redo toouse som **/datadrive**.
   
    ![Skapa hello katalogen och montera hello-disk](./media/attach-disk/mkdirandmount.png)

11. Lägg till hello ny enhet för/etc/fstab:
   
    tooensure hello enhet är nytt automatiskt när en omstart måste det vara lagt till toohello /etc/fstab fil. Dessutom kan rekommenderar vi starkt att hello UUID (universellt Unique IDentifier) som används i /etc/fstab toorefer toohello enhet i stället för bara hello enhetsnamn (d.v.s. /dev/sdc1). Med hjälp av hello undviker UUID hello felaktig disk som monterade tooa får plats om hello OS upptäcker ett diskfel under start och eventuella återstående hårddiskar sedan som tilldelats de enhets-ID. toofind hello UUID för hello ny enhet kan du använda hello **blkid** verktyget:
   
    ```bash
    sudo -i blkid
    ```
   
    hello utdata ser ut ungefär toohello följande exempel:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Felaktigt redigera hello **/etc/fstab** filen kan resultera i ett system som inte kan startas. Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen. Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.

    Därefter öppnar hello **/etc/fstab** i en textredigerare:

    ```bash
    sudo vi /etc/fstab
    ```

    I det här exemplet vi använder hello UUID-värdet för hello nya **/dev/sdc1** enheten som skapades i föregående steg i hello och hello monteringspunkt **/datadrive**. Lägg till hello efter raden toohello hello **/etc/fstab** fil:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Eller på system baserat på SuSE Linux behöva toouse ett något annorlunda format:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > Hej `nofail` alternativet ser du till att hello VM startar även om hello filsystemet är skadat eller hello disk finns inte vid start. Utan det här alternativet, du kan stöta på problem som beskrivs i [kan SSH tooLinux VM på grund av fel tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Du kan nu testa att hello är monterat genom demontering och ommontering hello filsystem, d.v.s. med hello exempel monteringspunkt `/datadrive` skapas i hello tidigare steg:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Om hello `mount` kommandot genererar ett fel, kontrollera hello filen/etc/fstab korrekt syntax. Om ytterligare dataenheter eller partitioner har skapats, anger du dem i/etc/fstab samt separat.

    Göra hello enhet skrivbar med hjälp av det här kommandot:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Därefter tar du bort en datadisk utan att redigera fstab orsaka hello VM toofail tooboot. Om detta är vanligt förekommande de flesta distributioner ange antingen hello `nofail` och/eller `nobootwait` fstab alternativ som gör att en system-tooboot även om hello disk kraschar toomount vid start. Se dokumentationen för din distribution för mer information om dessa parametrar.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIMNING/UNMAP stöd för Linux i Azure
Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk. Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort. Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.

Det finns två sätt tooenable TRIMNING stöd i Linux-VM. Som vanligt, kontakta din distribution för hello rekommendationer:

* Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* I vissa fall hello `discard` alternativet kanske prestanda. Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nästa steg
Du kan läsa mer om hur du använder din Linux VM i hello följande artiklar:

* [Hur toolog på tooa virtuell dator som kör Linux][Logon]
* [Hur toodetach en disk från en virtuell Linux-dator](detach-disk.md)
* [Med hello klassiska distributionsmodellen hello Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Konfigurera RAID på en virtuell Linux-dator i Azure](../configure-raid.md)
* [Konfigurera LVM på en virtuell Linux-dator i Azure](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
