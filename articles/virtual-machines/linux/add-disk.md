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
# <a name="add-a-disk-tooa-linux-vm"></a><span data-ttu-id="2365a-104">Lägg till en disk tooa Linux VM</span><span class="sxs-lookup"><span data-stu-id="2365a-104">Add a disk tooa Linux VM</span></span>
<span data-ttu-id="2365a-105">Den här artikeln visar hur tooattach en beständig disk tooyour VM så att du kan behålla dina data - även om den virtuella datorn etableras på grund av toomaintenance eller ändrar storlek på.</span><span class="sxs-lookup"><span data-stu-id="2365a-105">This article shows how tooattach a persistent disk tooyour VM so that you can preserve your data - even if your VM is reprovisioned due toomaintenance or resizing.</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="2365a-106">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="2365a-106">Quick Commands</span></span>
<span data-ttu-id="2365a-107">Hej följande exempel bifogar en `50`GB disk toohello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="2365a-107">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

<span data-ttu-id="2365a-108">toouse hanterade diskar:</span><span class="sxs-lookup"><span data-stu-id="2365a-108">toouse managed disks:</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

<span data-ttu-id="2365a-109">toouse ohanterad diskar:</span><span class="sxs-lookup"><span data-stu-id="2365a-109">toouse unmanaged disks:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a><span data-ttu-id="2365a-110">Koppla in en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="2365a-110">Attach a managed disk</span></span>

<span data-ttu-id="2365a-111">Med hjälp av hanterade diskar kan du toofocus på dina virtuella datorer och deras diskar utan att oroa Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="2365a-111">Using managed disks enables you toofocus on your VMs and their disks without worrying about Azure Storage accounts.</span></span> <span data-ttu-id="2365a-112">Du kan snabbt skapa och koppla in en hanterad disk tooa VM med hello samma Azure-resursgrupp eller skapa valfritt antal diskar och sedan koppla dem.</span><span class="sxs-lookup"><span data-stu-id="2365a-112">You can quickly create and attach a managed disk tooa VM using hello same Azure resource group, or you can create any number of disks and then attach them.</span></span>


### <a name="attach-a-new-disk-tooa-vm"></a><span data-ttu-id="2365a-113">Koppla en ny disk tooa VM</span><span class="sxs-lookup"><span data-stu-id="2365a-113">Attach a new disk tooa VM</span></span>

<span data-ttu-id="2365a-114">Om du vill skapa en ny disk på den virtuella datorn, kan du använda hello `az vm disk attach` kommando.</span><span class="sxs-lookup"><span data-stu-id="2365a-114">If you just need a new disk on your VM, you can use hello `az vm disk attach` command.</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a><span data-ttu-id="2365a-115">Ansluta en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="2365a-115">Attach an existing disk</span></span> 

<span data-ttu-id="2365a-116">I många fall kan du bifoga diskar som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="2365a-116">In many cases you attach disks that have already been created.</span></span> <span data-ttu-id="2365a-117">Du först hitta hello disk-id och sedan skicka denna toohello `az vm disk attach` kommando.</span><span class="sxs-lookup"><span data-stu-id="2365a-117">You first find hello disk id and then pass that toohello `az vm disk attach` command.</span></span> <span data-ttu-id="2365a-118">hello följande exempel används en disk som skapats med `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span><span class="sxs-lookup"><span data-stu-id="2365a-118">hello following example uses a disk created with `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span></span>

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

<span data-ttu-id="2365a-119">hello utdata ser ut ungefär så hello följande (du kan använda hello `-o table` alternativet tooany tooformat hello kommandoutdata i):</span><span class="sxs-lookup"><span data-stu-id="2365a-119">hello output looks something like hello following (you can use hello `-o table` option tooany command tooformat hello output in ):</span></span>

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


## <a name="attach-an-unmanaged-disk"></a><span data-ttu-id="2365a-120">Anslut en ohanterad disk</span><span class="sxs-lookup"><span data-stu-id="2365a-120">Attach an unmanaged disk</span></span>

<span data-ttu-id="2365a-121">Koppla en ny disk går snabbt om du inte gör något att skapa en disk i hello samma lagringskonto som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2365a-121">Attaching a new disk is quick if you do not mind creating a disk in hello same storage account as your VM.</span></span> <span data-ttu-id="2365a-122">Typen `azure vm disk attach-new` toocreate och koppla en ny GB disk för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2365a-122">Type `azure vm disk attach-new` toocreate and attach a new GB disk for your VM.</span></span> <span data-ttu-id="2365a-123">Om du inte identifierar ett lagringskonto, en disk som du skapar placeras i hello samma lagringskonto där OS-disken finns.</span><span class="sxs-lookup"><span data-stu-id="2365a-123">If you do not explicitly identify a storage account, any disk you create is placed in hello same storage account where your OS disk resides.</span></span> <span data-ttu-id="2365a-124">Hej följande exempel bifogar en `50`GB disk toohello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="2365a-124">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a><span data-ttu-id="2365a-125">Ansluta toohello Linux VM toomount hello ny disk</span><span class="sxs-lookup"><span data-stu-id="2365a-125">Connect toohello Linux VM toomount hello new disk</span></span>
> [!NOTE]
> <span data-ttu-id="2365a-126">Det här avsnittet ansluter tooa VM med hjälp av användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="2365a-126">This topic connects tooa VM using usernames and passwords.</span></span> <span data-ttu-id="2365a-127">toouse offentliga och privata nyckelpar toocommunicate med den virtuella datorn finns [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2365a-127">toouse public and private key pairs toocommunicate with your VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
> 
> 

<span data-ttu-id="2365a-128">Du behöver tooSSH i ditt Virtuella Azure-toopartition formatera och montera den nya disken så att din Linux VM kan använda den.</span><span class="sxs-lookup"><span data-stu-id="2365a-128">You need tooSSH into your Azure VM toopartition, format, and mount your new disk so your Linux VM can use it.</span></span> <span data-ttu-id="2365a-129">Om du inte är bekant med att ansluta med **ssh**, hello kommandot tar emot hello formuläret `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, och ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="2365a-129">If you're not familiar with connecting with **ssh**, hello command takes hello form `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, and looks like hello following:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

<span data-ttu-id="2365a-130">Resultat</span><span class="sxs-lookup"><span data-stu-id="2365a-130">Output</span></span>

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

<span data-ttu-id="2365a-131">Nu när du är ansluten tooyour VM, är du redo tooattach en disk.</span><span class="sxs-lookup"><span data-stu-id="2365a-131">Now that you're connected tooyour VM, you're ready tooattach a disk.</span></span>  <span data-ttu-id="2365a-132">Hitta först hello disken med `dmesg | grep SCSI` (hello metoden du använder den nya disken kan variera toodiscover).</span><span class="sxs-lookup"><span data-stu-id="2365a-132">First find hello disk, using `dmesg | grep SCSI` (hello method you use toodiscover your new disk may vary).</span></span> <span data-ttu-id="2365a-133">I det här fallet ser ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="2365a-133">In this case, it looks something like:</span></span>

```bash
dmesg | grep SCSI
```

<span data-ttu-id="2365a-134">Resultat</span><span class="sxs-lookup"><span data-stu-id="2365a-134">Output</span></span>

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

<span data-ttu-id="2365a-135">och i det här avsnittet hello fallet hello `sdc` disken är hello som vi vill.</span><span class="sxs-lookup"><span data-stu-id="2365a-135">and in hello case of this topic, hello `sdc` disk is hello one that we want.</span></span> <span data-ttu-id="2365a-136">Nu hello partitionsdisk med `sudo fdisk /dev/sdc` --förutsatt att i ditt fall hello disken var `sdc`, och att den primära disken på partitionen 1 och acceptera hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="2365a-136">Now partition hello disk with `sudo fdisk /dev/sdc` -- assuming that in your case hello disk was `sdc`, and make it a primary disk on partition 1, and accept hello other defaults.</span></span>

```bash
sudo fdisk /dev/sdc
```

<span data-ttu-id="2365a-137">Resultat</span><span class="sxs-lookup"><span data-stu-id="2365a-137">Output</span></span>

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

<span data-ttu-id="2365a-138">Skapa hello partition genom att skriva `p` hello i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="2365a-138">Create hello partition by typing `p` at hello prompt:</span></span>

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

<span data-ttu-id="2365a-139">Och skriva toohello en filsystempartition med hjälp av hello **mkfs** kommando, anger filsystem typ och hello enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="2365a-139">And write a file system toohello partition by using hello **mkfs** command, specifying your filesystem type and hello device name.</span></span> <span data-ttu-id="2365a-140">I det här avsnittet använder vi `ext4` och `/dev/sdc1` ovan:</span><span class="sxs-lookup"><span data-stu-id="2365a-140">In this topic, we're using `ext4` and `/dev/sdc1` from above:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="2365a-141">Resultat</span><span class="sxs-lookup"><span data-stu-id="2365a-141">Output</span></span>

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

<span data-ttu-id="2365a-142">Nu skapar vi en directory toomount hello filsystemet med `mkdir`:</span><span class="sxs-lookup"><span data-stu-id="2365a-142">Now we create a directory toomount hello file system using `mkdir`:</span></span>

```bash
sudo mkdir /datadrive
```

<span data-ttu-id="2365a-143">Och du monterar hello directory med `mount`:</span><span class="sxs-lookup"><span data-stu-id="2365a-143">And you mount hello directory using `mount`:</span></span>

```bash
sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="2365a-144">Hej datadisk är nu redo toouse som `/datadrive`.</span><span class="sxs-lookup"><span data-stu-id="2365a-144">hello data disk is now ready toouse as `/datadrive`.</span></span>

```bash
ls
```

<span data-ttu-id="2365a-145">Resultat</span><span class="sxs-lookup"><span data-stu-id="2365a-145">Output</span></span>

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

<span data-ttu-id="2365a-146">tooensure hello enhet är nytt automatiskt när en omstart måste det vara lagt till toohello /etc/fstab fil.</span><span class="sxs-lookup"><span data-stu-id="2365a-146">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="2365a-147">Dessutom rekommenderas att hello UUID (universellt Unique IDentifier) som används i/etc/fstab toorefer toohello-enhet i stället för bara hello enhetens namn (t.ex, `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="2365a-147">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (such as, `/dev/sdc1`).</span></span> <span data-ttu-id="2365a-148">Om hello OS upptäcker ett diskfel under Start, undviker med hjälp av hello UUID hello felaktig disk som monterade tooa får plats.</span><span class="sxs-lookup"><span data-stu-id="2365a-148">If hello OS detects a disk error during boot, using hello UUID avoids hello incorrect disk being mounted tooa given location.</span></span> <span data-ttu-id="2365a-149">Återstående datadiskar sedan tilldelas de samma enhets-ID.</span><span class="sxs-lookup"><span data-stu-id="2365a-149">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="2365a-150">toofind hello UUID för hello ny enhet använder hello **blkid** verktyget:</span><span class="sxs-lookup"><span data-stu-id="2365a-150">toofind hello UUID of hello new drive, use hello **blkid** utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="2365a-151">hello utdata ser ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="2365a-151">hello output looks similar toohello following:</span></span>

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> <span data-ttu-id="2365a-152">Felaktigt redigera hello **/etc/fstab** filen kan resultera i ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="2365a-152">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="2365a-153">Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="2365a-153">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="2365a-154">Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="2365a-154">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>
> 
> 

<span data-ttu-id="2365a-155">Därefter öppnar hello **/etc/fstab** i en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="2365a-155">Next, open hello **/etc/fstab** file in a text editor:</span></span>

```bash
sudo vi /etc/fstab
```

<span data-ttu-id="2365a-156">I det här exemplet vi använder hello UUID-värdet för hello nya **/dev/sdc1** enheten som skapades i föregående steg i hello och hello monteringspunkt **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="2365a-156">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="2365a-157">Lägg till hello efter raden toohello hello **/etc/fstab** fil:</span><span class="sxs-lookup"><span data-stu-id="2365a-157">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> <span data-ttu-id="2365a-158">Tar bort en datadisk senare utan att redigera fstab orsaka hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="2365a-158">Later removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="2365a-159">De flesta distributioner ange antingen hello `nofail` och/eller `nobootwait` fstab alternativ.</span><span class="sxs-lookup"><span data-stu-id="2365a-159">Most distributions provide either hello `nofail` and/or `nobootwait` fstab options.</span></span> <span data-ttu-id="2365a-160">Dessa alternativ gör en system-tooboot även om hello disk kraschar toomount vid start.</span><span class="sxs-lookup"><span data-stu-id="2365a-160">These options allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="2365a-161">Se dokumentationen för din distribution för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="2365a-161">Consult your distribution's documentation for more information on these parameters.</span></span>
> 
> <span data-ttu-id="2365a-162">Hej **nofail** alternativet ser du till att hello VM startar även om hello filsystemet är skadat eller hello disk finns inte vid start.</span><span class="sxs-lookup"><span data-stu-id="2365a-162">hello **nofail** option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="2365a-163">Utan det här alternativet, du kan stöta på problem som beskrivs i [kan SSH tooLinux VM på grund av tooFSTAB fel](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span><span class="sxs-lookup"><span data-stu-id="2365a-163">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="2365a-164">TRIMNING/UNMAP stöd för Linux i Azure</span><span class="sxs-lookup"><span data-stu-id="2365a-164">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="2365a-165">Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk.</span><span class="sxs-lookup"><span data-stu-id="2365a-165">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="2365a-166">Det här är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="2365a-166">This is primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="2365a-167">Detta kan spara kostnad om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="2365a-167">This can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="2365a-168">Det finns två sätt tooenable TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="2365a-168">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="2365a-169">Som vanligt, kontakta din distribution för hello rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2365a-169">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="2365a-170">Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="2365a-170">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* <span data-ttu-id="2365a-171">I vissa fall hello `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="2365a-171">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="2365a-172">Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:</span><span class="sxs-lookup"><span data-stu-id="2365a-172">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="2365a-173">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="2365a-173">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="2365a-174">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="2365a-174">**RHEL/CentOS**</span></span>

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="2365a-175">Felsökning</span><span class="sxs-lookup"><span data-stu-id="2365a-175">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="2365a-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2365a-176">Next Steps</span></span>
* <span data-ttu-id="2365a-177">Kom ihåg att den nya disken inte är tillgänglig toohello VM om startas om du skriver den information tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) fil.</span><span class="sxs-lookup"><span data-stu-id="2365a-177">Remember, that your new disk is not available toohello VM if it reboots unless you write that information tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) file.</span></span>
* <span data-ttu-id="2365a-178">tooensure Linux-VM är korrekt konfigurerade, granska hello [optimera prestanda ditt Linux-datorer](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2365a-178">tooensure your Linux VM is configured correctly, review hello [Optimize your Linux machine performance](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) recommendations.</span></span>
* <span data-ttu-id="2365a-179">Expandera lagringskapaciteten genom att lägga till ytterligare diskar och [konfigurera RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare prestanda.</span><span class="sxs-lookup"><span data-stu-id="2365a-179">Expand your storage capacity by adding additional disks and [configure RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional performance.</span></span>

