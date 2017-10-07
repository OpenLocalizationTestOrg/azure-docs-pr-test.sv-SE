---
title: aaaAdd en disk tooLinux VM med hello Azure CLI | Microsoft Docs
description: "Lär dig tooadd beständiga disk tooyour Linux VM i hello Azure CLI 1.0 och 2.0."
keywords: "Linux-dator, Lägg till resurs-disk"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0dc5236be62d96b70dd47a7f621f626a037e22aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-disk-tooa-linux-vm"></a>Lägg till en disk tooa Linux VM
Den här artikeln visar hur tooattach en beständig disk tooyour VM så att du kan behålla dina data - även om den virtuella datorn etableras på grund av toomaintenance eller ändrar storlek på. 

## <a name="quick-commands"></a>Snabbkommandon
Hej följande exempel bifogar en `50`GB disk toohello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

toouse hanterade diskar:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

toouse ohanterad diskar:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a>Koppla in en hanterad disk

Med hjälp av hanterade diskar kan du toofocus på dina virtuella datorer och deras diskar utan att oroa Azure Storage-konton. Du kan snabbt skapa och koppla in en hanterad disk tooa VM med hello samma Azure-resursgrupp eller skapa valfritt antal diskar och sedan koppla dem.


### <a name="attach-a-new-disk-tooa-vm"></a>Koppla en ny disk tooa VM

Om du vill skapa en ny disk på den virtuella datorn, kan du använda hello `az vm disk attach` kommando.

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Ansluta en befintlig disk 

I många fall kan du bifoga diskar som redan har skapats. Du först hitta hello disk-id och sedan skicka denna toohello `az vm disk attach` kommando. hello följande exempel används en disk som skapats med `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

hello utdata ser ut ungefär så hello följande (du kan använda hello `-o table` alternativet tooany tooformat hello kommandoutdata i):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="attach-an-unmanaged-disk"></a>Anslut en ohanterad disk

Koppla en ny disk går snabbt om du inte gör något att skapa en disk i hello samma lagringskonto som den virtuella datorn. Typen `azure vm disk attach-new` toocreate och koppla en ny GB disk för den virtuella datorn. Om du inte identifierar ett lagringskonto, en disk som du skapar placeras i hello samma lagringskonto där OS-disken finns. Hej följande exempel bifogar en `50`GB disk toohello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a>Ansluta toohello Linux VM toomount hello ny disk
> [!NOTE]
> Det här avsnittet ansluter tooa VM med hjälp av användarnamn och lösenord. toouse offentliga och privata nyckelpar toocommunicate med den virtuella datorn finns [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
> 
> 

Du behöver tooSSH i ditt Virtuella Azure-toopartition formatera och montera den nya disken så att din Linux VM kan använda den. Om du inte är bekant med att ansluta med **ssh**, hello kommandot tar emot hello formuläret `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, och ser ut som följande hello:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

Resultat

```bash
hello authenticity of host 'mypublicdns.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) toohello list of known hosts.
ops@mypublicdns.westus.cloudapp.azure.com's password:
Welcome tooUbuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

ops@myVM:~$
```

Nu när du är ansluten tooyour VM, är du redo tooattach en disk.  Hitta först hello disken med `dmesg | grep SCSI` (hello metoden du använder den nya disken kan variera toodiscover). I det här fallet ser ut ungefär så:

```bash
dmesg | grep SCSI
```

Resultat

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

och i det här avsnittet hello fallet hello `sdc` disken är hello som vi vill. Nu hello partitionsdisk med `sudo fdisk /dev/sdc` --förutsatt att i ditt fall hello disken var `sdc`, och att den primära disken på partitionen 1 och acceptera hello standardvärden.

```bash
sudo fdisk /dev/sdc
```

Resultat

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide toowrite them.
After that, of course, hello previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Skapa hello partition genom att skriva `p` hello i Kommandotolken:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
hello partition table has been altered!

Calling ioctl() toore-read partition table.
Syncing disks.
```

Och skriva toohello en filsystempartition med hjälp av hello **mkfs** kommando, anger filsystem typ och hello enhetsnamn. I det här avsnittet använder vi `ext4` och `/dev/sdc1` ovan:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Resultat

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for hello super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Nu skapar vi en directory toomount hello filsystemet med `mkdir`:

```bash
sudo mkdir /datadrive
```

Och du monterar hello directory med `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Hej datadisk är nu redo toouse som `/datadrive`.

```bash
ls
```

Resultat

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

tooensure hello enhet är nytt automatiskt när en omstart måste det vara lagt till toohello /etc/fstab fil. Dessutom rekommenderas att hello UUID (universellt Unique IDentifier) som används i/etc/fstab toorefer toohello-enhet i stället för bara hello enhetens namn (t.ex, `/dev/sdc1`). Om hello OS upptäcker ett diskfel under Start, undviker med hjälp av hello UUID hello felaktig disk som monterade tooa får plats. Återstående datadiskar sedan tilldelas de samma enhets-ID. toofind hello UUID för hello ny enhet använder hello **blkid** verktyget:

```bash
sudo -i blkid
```

hello utdata ser ut ungefär toohello följande:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Felaktigt redigera hello **/etc/fstab** filen kan resultera i ett system som inte kan startas. Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen. Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.
> 
> 

Därefter öppnar hello **/etc/fstab** i en textredigerare:

```bash
sudo vi /etc/fstab
```

I det här exemplet vi använder hello UUID-värdet för hello nya **/dev/sdc1** enheten som skapades i föregående steg i hello och hello monteringspunkt **/datadrive**. Lägg till hello efter raden toohello hello **/etc/fstab** fil:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Tar bort en datadisk senare utan att redigera fstab orsaka hello VM toofail tooboot. De flesta distributioner ange antingen hello `nofail` och/eller `nobootwait` fstab alternativ. Dessa alternativ gör en system-tooboot även om hello disk kraschar toomount vid start. Se dokumentationen för din distribution för mer information om dessa parametrar.
> 
> Hej **nofail** alternativet ser du till att hello VM startar även om hello filsystemet är skadat eller hello disk finns inte vid start. Utan det här alternativet, du kan stöta på problem som beskrivs i [kan SSH tooLinux VM på grund av tooFSTAB fel](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIMNING/UNMAP stöd för Linux i Azure
Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk. Det här är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort. Detta kan spara kostnad om du skapar stora filer och ta bort dem.

Det finns två sätt tooenable TRIMNING stöd i Linux-VM. Som vanligt, kontakta din distribution för hello rekommendationer:

* Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:

    ```bash
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
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nästa steg
* Kom ihåg att den nya disken inte är tillgänglig toohello VM om startas om du skriver den information tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) fil.
* tooensure Linux-VM är korrekt konfigurerade, granska hello [optimera prestanda ditt Linux-datorer](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) rekommendationer.
* Expandera lagringskapaciteten genom att lägga till ytterligare diskar och [konfigurera RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare prestanda.

