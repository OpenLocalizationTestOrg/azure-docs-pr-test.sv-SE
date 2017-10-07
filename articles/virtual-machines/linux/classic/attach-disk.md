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
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a><span data-ttu-id="1e2ea-103">Hur tooAttach en datadisk tooa virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="1e2ea-103">How tooAttach a Data Disk tooa Linux Virtual Machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="1e2ea-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1e2ea-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="1e2ea-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="1e2ea-107">Se hur för[ansluta en datadisk med hello Resource Manager-distributionsmodellen](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-107">See how too[attach a data disk using hello Resource Manager deployment model](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="1e2ea-108">Du kan koppla både tom och diskar som innehåller data tooyour virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-108">You can attach both empty disks and disks that contain data tooyour Azure VMs.</span></span> <span data-ttu-id="1e2ea-109">Båda typer av diskar är VHD-filer som finns i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-109">Both types of disks are .vhd files that reside in an Azure storage account.</span></span> <span data-ttu-id="1e2ea-110">Som med att lägga till en disk tooa Linux-dator, när du har kopplat hello disk du behöver tooinitialize och formatera den så att den är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-110">As with adding any disk tooa Linux machine, after you attach hello disk you need tooinitialize and format it so it's ready for use.</span></span> <span data-ttu-id="1e2ea-111">Den här artikeln information som kopplar både tom diskar och diskar som redan innehåller data tooyour virtuella datorer, samt hur toothen initiera och formatera en ny disk.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-111">This article details attaching both empty disks and disks already containing data tooyour VMs, as well as how toothen initialize and format a new disk.</span></span>

> [!NOTE]
> <span data-ttu-id="1e2ea-112">Det är en bästa praxis toouse en eller flera olika diskar toostore data för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-112">It's a best practice toouse one or more separate disks toostore a virtual machine's data.</span></span> <span data-ttu-id="1e2ea-113">När du skapar en virtuell Azure-dator har en operativsystemdisk och en tillfällig disk.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-113">When you create an Azure virtual machine, it has an operating system disk and a temporary disk.</span></span> <span data-ttu-id="1e2ea-114">**Använd inte hello diskutrymme toostore beständiga data.**</span><span class="sxs-lookup"><span data-stu-id="1e2ea-114">**Do not use hello temporary disk toostore persistent data.**</span></span> <span data-ttu-id="1e2ea-115">Hello namnet antyder ger den tillfälligt lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-115">As hello name implies, it provides temporary storage only.</span></span> <span data-ttu-id="1e2ea-116">Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-116">It offers no redundancy or backup because it doesn't reside in Azure storage.</span></span>
> <span data-ttu-id="1e2ea-117">hello diskutrymme hanteras vanligtvis av hello Azure Linux-agenten och automatiskt monterade för**/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-117">hello temporary disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span> <span data-ttu-id="1e2ea-118">Hej på andra sidan, en datadisk kan kallas av hello Linux kernel ungefär `/dev/sdc`, och du behöver toopartition, formatera och montera den här resursen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-118">On hello other hand, a data disk might be named by hello Linux kernel something like `/dev/sdc`, and you need toopartition, format, and mount this resource.</span></span> <span data-ttu-id="1e2ea-119">Se hello [användarhandboken för Azure Linux-agenten] [ Agent] mer information.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-119">See hello [Azure Linux Agent User Guide][Agent] for details.</span></span>
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a><span data-ttu-id="1e2ea-120">Initiera en ny datadisk i Linux</span><span class="sxs-lookup"><span data-stu-id="1e2ea-120">Initialize a new data disk in Linux</span></span>
1. <span data-ttu-id="1e2ea-121">SSH tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-121">SSH tooyour VM.</span></span> <span data-ttu-id="1e2ea-122">Mer information finns i [hur toolog på tooa virtuell dator som kör Linux][Logon].</span><span class="sxs-lookup"><span data-stu-id="1e2ea-122">For more information, see [How toolog on tooa virtual machine running Linux][Logon].</span></span>
2. <span data-ttu-id="1e2ea-123">Därefter behöver du toofind hello enhets-ID för hello data disk tooinitialize.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-123">Next you need toofind hello device identifier for hello data disk tooinitialize.</span></span> <span data-ttu-id="1e2ea-124">Det finns två sätt toodo som:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-124">There are two ways toodo that:</span></span>
   
    <span data-ttu-id="1e2ea-125">a) Grep för SCSI-enheter i hello loggar, exempelvis hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-125">a) Grep for SCSI devices in hello logs, such as in hello following command:</span></span>
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    <span data-ttu-id="1e2ea-126">Om du nyligen Ubuntu distributioner måste toouse `sudo grep SCSI /var/log/syslog` eftersom loggar för`/var/log/messages` kan vara inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-126">For recent Ubuntu distributions, you may need toouse `sudo grep SCSI /var/log/syslog` because logging too`/var/log/messages` might be disabled by default.</span></span>
   
    <span data-ttu-id="1e2ea-127">Du kan hitta hello identifierare för hello senaste datadisk som har lagts till i hello-meddelanden som visas.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-127">You can find hello identifier of hello last data disk that was added in hello messages that are displayed.</span></span>
   
    ![Hämta hälsningsmeddelande för disk](./media/attach-disk/scsidisklog.png)
   
    <span data-ttu-id="1e2ea-129">ELLER</span><span class="sxs-lookup"><span data-stu-id="1e2ea-129">OR</span></span>
   
    <span data-ttu-id="1e2ea-130">b) Använd hello `lsscsi` kommandot toofind ut hello enhets-id. `lsscsi` kan installeras antingen `yum install lsscsi` (Red Hat baserad på distributioner) eller `apt-get install lsscsi` (Debian baserad på distributioner).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-130">b) Use hello `lsscsi` command toofind out hello device id. `lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="1e2ea-131">Du kan hitta hello-disk som du letar efter av dess *lun* eller **logiskt enhetsnummer**.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-131">You can find hello disk you are looking for by its *lun* or **logical unit number**.</span></span> <span data-ttu-id="1e2ea-132">Till exempel hello *lun* för hello diskar bifogade kan enkelt se `azure vm disk list <virtual-machine>` som:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-132">For example, hello *lun* for hello disks you attached can be easily seen from `azure vm disk list <virtual-machine>` as:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="1e2ea-133">hello-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-133">hello output is similar toohello following:</span></span>

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
   
    <span data-ttu-id="1e2ea-134">Jämföra dessa data med hello utdata från `lsscsi` för hello samma prov virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-134">Compare this data with hello output of `lsscsi` for hello same sample virtual machine:</span></span>
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    <span data-ttu-id="1e2ea-135">hello senaste antalet hello tuppel i varje rad är hello *lun*.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-135">hello last number in hello tuple in each row is hello *lun*.</span></span> <span data-ttu-id="1e2ea-136">Se `man lsscsi` för mer information.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-136">See `man lsscsi` for more information.</span></span>
3. <span data-ttu-id="1e2ea-137">I hello kommandotolk, skriver du följande kommando toocreate hello enheten:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-137">At hello prompt, type hello following command toocreate your device:</span></span>
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. <span data-ttu-id="1e2ea-138">När du uppmanas ange  **n**  toocreate en partition.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-138">When prompted, type **n** toocreate a partition.</span></span>

    ![Skapa enhet](./media/attach-disk/fdisknewpartition.png)

5. <span data-ttu-id="1e2ea-140">När du uppmanas ange **p** toomake hello partition hello primära partitionen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-140">When prompted, type **p** toomake hello partition hello primary partition.</span></span> <span data-ttu-id="1e2ea-141">Typen **1** toomake den hello första partitionen och skriv sedan ange tooaccept hello standardvärde för hello cylinder.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-141">Type **1** toomake it hello first partition, and then type enter tooaccept hello default value for hello cylinder.</span></span> <span data-ttu-id="1e2ea-142">Den kan visa hello standardvärdena för hello först och hello senaste sektorer, i stället för hello cylinder på vissa datorer.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-142">On some systems, it can show hello default values of hello first and hello last sectors, instead of hello cylinder.</span></span> <span data-ttu-id="1e2ea-143">Du kan välja tooaccept dessa standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-143">You can choose tooaccept these defaults.</span></span>

    ![Skapa partition](./media/attach-disk/fdisknewpartdetails.png)


6. <span data-ttu-id="1e2ea-145">Typen **p** toosee hello information om hello-disk som är är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-145">Type **p** toosee hello details about hello disk that is being partitioned.</span></span>

    ![Diskinformation för listan](./media/attach-disk/fdiskpartitiondetails.png)


7. <span data-ttu-id="1e2ea-147">Typen **w** toowrite hello inställningarna för hello disk.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-147">Type **w** toowrite hello settings for hello disk.</span></span>

    ![Skriva hello disk ändringar](./media/attach-disk/fdiskwritedisk.png)

8. <span data-ttu-id="1e2ea-149">Du kan nu skapa hello filsystemet på hello ny partition.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-149">Now you can create hello file system on hello new partition.</span></span> <span data-ttu-id="1e2ea-150">Lägg till hello partition nummer toohello enhets-ID (i följande exempel hello `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-150">Append hello partition number toohello device ID (in hello following example `/dev/sdc1`).</span></span> <span data-ttu-id="1e2ea-151">hello följande exempel skapas en ext4 partition på /dev/sdc1:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-151">hello following example creates an ext4 partition on /dev/sdc1:</span></span>
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Skapa filsystem](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > <span data-ttu-id="1e2ea-153">SuSE Linux Enterprise 11 system stöder bara läsbehörighet för ext4 filsystem.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-153">SuSE Linux Enterprise 11 systems only support read-only access for ext4 file systems.</span></span> <span data-ttu-id="1e2ea-154">För de här systemen bör tooformat hello nytt filsystem som ext3 i stället för ext4.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-154">For these systems, it is recommended tooformat hello new file system as ext3 rather than ext4.</span></span>

9. <span data-ttu-id="1e2ea-155">Gör ett directory toomount hello nytt filsystem, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-155">Make a directory toomount hello new file system, as follows:</span></span>
   
    ```bash
    sudo mkdir /datadrive
    ```

10. <span data-ttu-id="1e2ea-156">Slutligen kan du montera hello enheten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-156">Finally you can mount hello drive, as follows:</span></span>
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    <span data-ttu-id="1e2ea-157">Hej datadisk är nu redo toouse som **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-157">hello data disk is now ready toouse as **/datadrive**.</span></span>
   
    ![Skapa hello katalogen och montera hello-disk](./media/attach-disk/mkdirandmount.png)

11. <span data-ttu-id="1e2ea-159">Lägg till hello ny enhet för/etc/fstab:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-159">Add hello new drive too/etc/fstab:</span></span>
   
    <span data-ttu-id="1e2ea-160">tooensure hello enhet är nytt automatiskt när en omstart måste det vara lagt till toohello /etc/fstab fil.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-160">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="1e2ea-161">Dessutom kan rekommenderar vi starkt att hello UUID (universellt Unique IDentifier) som används i /etc/fstab toorefer toohello enhet i stället för bara hello enhetsnamn (d.v.s. /dev/sdc1).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-161">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (i.e. /dev/sdc1).</span></span> <span data-ttu-id="1e2ea-162">Med hjälp av hello undviker UUID hello felaktig disk som monterade tooa får plats om hello OS upptäcker ett diskfel under start och eventuella återstående hårddiskar sedan som tilldelats de enhets-ID.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-162">Using hello UUID avoids hello incorrect disk being mounted tooa given location if hello OS detects a disk error during boot and any remaining data disks then being assigned those device IDs.</span></span> <span data-ttu-id="1e2ea-163">toofind hello UUID för hello ny enhet kan du använda hello **blkid** verktyget:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-163">toofind hello UUID of hello new drive, you can use hello **blkid** utility:</span></span>
   
    ```bash
    sudo -i blkid
    ```
   
    <span data-ttu-id="1e2ea-164">hello utdata ser ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-164">hello output looks similar toohello following example:</span></span>
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > <span data-ttu-id="1e2ea-165">Felaktigt redigera hello **/etc/fstab** filen kan resultera i ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-165">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="1e2ea-166">Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-166">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="1e2ea-167">Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-167">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

    <span data-ttu-id="1e2ea-168">Därefter öppnar hello **/etc/fstab** i en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-168">Next, open hello **/etc/fstab** file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

    <span data-ttu-id="1e2ea-169">I det här exemplet vi använder hello UUID-värdet för hello nya **/dev/sdc1** enheten som skapades i föregående steg i hello och hello monteringspunkt **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-169">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="1e2ea-170">Lägg till hello efter raden toohello hello **/etc/fstab** fil:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-170">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    <span data-ttu-id="1e2ea-171">Eller på system baserat på SuSE Linux behöva toouse ett något annorlunda format:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-171">Or, on systems based on SuSE Linux you may need toouse a slightly different format:</span></span>

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > <span data-ttu-id="1e2ea-172">Hej `nofail` alternativet ser du till att hello VM startar även om hello filsystemet är skadat eller hello disk finns inte vid start.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-172">hello `nofail` option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="1e2ea-173">Utan det här alternativet, du kan stöta på problem som beskrivs i [kan SSH tooLinux VM på grund av fel tooFSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-173">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span></span>

    <span data-ttu-id="1e2ea-174">Du kan nu testa att hello är monterat genom demontering och ommontering hello filsystem, d.v.s. med hello exempel monteringspunkt `/datadrive` skapas i hello tidigare steg:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-174">You can now test that hello file system is mounted properly by unmounting and then remounting hello file system, i.e. using hello example mount point `/datadrive` created in hello earlier steps:</span></span>

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    <span data-ttu-id="1e2ea-175">Om hello `mount` kommandot genererar ett fel, kontrollera hello filen/etc/fstab korrekt syntax.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-175">If hello `mount` command produces an error, check hello /etc/fstab file for correct syntax.</span></span> <span data-ttu-id="1e2ea-176">Om ytterligare dataenheter eller partitioner har skapats, anger du dem i/etc/fstab samt separat.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-176">If additional data drives or partitions are created, enter them into /etc/fstab separately as well.</span></span>

    <span data-ttu-id="1e2ea-177">Göra hello enhet skrivbar med hjälp av det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-177">Make hello drive writable by using this command:</span></span>

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > <span data-ttu-id="1e2ea-178">Därefter tar du bort en datadisk utan att redigera fstab orsaka hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-178">Subsequently removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="1e2ea-179">Om detta är vanligt förekommande de flesta distributioner ange antingen hello `nofail` och/eller `nobootwait` fstab alternativ som gör att en system-tooboot även om hello disk kraschar toomount vid start.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-179">If this is a common occurrence, most distributions provide either hello `nofail` and/or `nobootwait` fstab options that allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="1e2ea-180">Se dokumentationen för din distribution för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-180">Consult your distribution's documentation for more information on these parameters.</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="1e2ea-181">TRIMNING/UNMAP stöd för Linux i Azure</span><span class="sxs-lookup"><span data-stu-id="1e2ea-181">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="1e2ea-182">Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-182">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="1e2ea-183">Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-183">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="1e2ea-184">Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-184">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="1e2ea-185">Det finns två sätt tooenable TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-185">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="1e2ea-186">Som vanligt, kontakta din distribution för hello rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-186">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="1e2ea-187">Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-187">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* <span data-ttu-id="1e2ea-188">I vissa fall hello `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-188">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="1e2ea-189">Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-189">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="1e2ea-190">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="1e2ea-190">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="1e2ea-191">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="1e2ea-191">**RHEL/CentOS**</span></span>
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="1e2ea-192">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1e2ea-192">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="1e2ea-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e2ea-193">Next Steps</span></span>
<span data-ttu-id="1e2ea-194">Du kan läsa mer om hur du använder din Linux VM i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-194">You can read more about using your Linux VM in hello following articles:</span></span>

* <span data-ttu-id="1e2ea-195">[Hur toolog på tooa virtuell dator som kör Linux][Logon]</span><span class="sxs-lookup"><span data-stu-id="1e2ea-195">[How toolog on tooa virtual machine running Linux][Logon]</span></span>
* [<span data-ttu-id="1e2ea-196">Hur toodetach en disk från en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="1e2ea-196">How toodetach a disk from a Linux virtual machine</span></span>](detach-disk.md)
* [<span data-ttu-id="1e2ea-197">Med hello klassiska distributionsmodellen hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1e2ea-197">Using hello Azure CLI with hello Classic deployment model</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="1e2ea-198">Konfigurera RAID på en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="1e2ea-198">Configure RAID on a Linux VM in Azure</span></span>](../configure-raid.md)
* [<span data-ttu-id="1e2ea-199">Konfigurera LVM på en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="1e2ea-199">Configure LVM on a Linux VM in Azure</span></span>](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
