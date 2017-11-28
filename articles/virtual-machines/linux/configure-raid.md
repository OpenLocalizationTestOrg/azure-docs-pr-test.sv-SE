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
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="8819f-103">Konfigurera programvaru-RAID på Linux</span><span class="sxs-lookup"><span data-stu-id="8819f-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="8819f-104">Det är ett vanligt scenario toouse programvarubaserad RAID på Linux virtuella datorer i Azure toopresent flera bifogade data diskar som en enkel RAID-enhet.</span><span class="sxs-lookup"><span data-stu-id="8819f-104">It's a common scenario toouse software RAID on Linux virtual machines in Azure toopresent multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="8819f-105">Vanligtvis kan vara används tooimprove prestanda och möjliggör bättre genomflöde jämfört med toousing bara en enda disk.</span><span class="sxs-lookup"><span data-stu-id="8819f-105">Typically this can be used tooimprove performance and allow for improved throughput compared toousing just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="8819f-106">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="8819f-106">Attaching data disks</span></span>
<span data-ttu-id="8819f-107">Två eller flera tom datadiskar är nödvändiga tooconfigure en RAID-enhet.</span><span class="sxs-lookup"><span data-stu-id="8819f-107">Two or more empty data disks are needed tooconfigure a RAID device.</span></span>  <span data-ttu-id="8819f-108">hello främsta orsaken för att skapa en RAID-enhet är tooimprove prestanda diskens i/o.</span><span class="sxs-lookup"><span data-stu-id="8819f-108">hello primary reason for creating a RAID device is tooimprove performance of your disk IO.</span></span>  <span data-ttu-id="8819f-109">Baserat på i/o-behov kan du välja tooattach diskar som är lagrade i vår standardlagring med upp too500 IO/ps per disk eller våra Premium-lagring med upp too5000 IO/ps per disk.</span><span class="sxs-lookup"><span data-stu-id="8819f-109">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="8819f-110">Den här artikeln inte gå in i detalj på hur tooprovision och bifoga data diskar tooa Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="8819f-110">This article does not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span>  <span data-ttu-id="8819f-111">Se hello Microsoft Azure artikel [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur tooattach en tom data disk tooa Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="8819f-111">See hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-mdadm-utility"></a><span data-ttu-id="8819f-112">Installera hello mdadm verktyget</span><span class="sxs-lookup"><span data-stu-id="8819f-112">Install hello mdadm utility</span></span>
* <span data-ttu-id="8819f-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="8819f-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="8819f-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="8819f-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="8819f-115">**SLES och openSUSE**</span><span class="sxs-lookup"><span data-stu-id="8819f-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a><span data-ttu-id="8819f-116">Skapa hello diskpartitioner</span><span class="sxs-lookup"><span data-stu-id="8819f-116">Create hello disk partitions</span></span>
<span data-ttu-id="8819f-117">I det här exemplet skapar vi en enskild diskpartition på /dev/sdc.</span><span class="sxs-lookup"><span data-stu-id="8819f-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="8819f-118">hello nya diskpartition kallas /dev/sdc1.</span><span class="sxs-lookup"><span data-stu-id="8819f-118">hello new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="8819f-119">Starta `fdisk` toobegin skapar partitioner</span><span class="sxs-lookup"><span data-stu-id="8819f-119">Start `fdisk` toobegin creating partitions</span></span>

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

2. <span data-ttu-id="8819f-120">Tryck på ”n” på hello fråga toocreate en  **n** y-partition:</span><span class="sxs-lookup"><span data-stu-id="8819f-120">Press 'n' at hello prompt toocreate a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="8819f-121">Tryck sedan på ”p” toocreate en **p**rimär partition:</span><span class="sxs-lookup"><span data-stu-id="8819f-121">Next, press 'p' toocreate a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="8819f-122">Tryck på '1' tooselect partitionsnummer 1:</span><span class="sxs-lookup"><span data-stu-id="8819f-122">Press '1' tooselect partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="8819f-123">Välj hello startpunkten för hello ny partition eller tryck på `<enter>` tooaccept hello tooplace hello standardpartition hello början hello ledigt utrymme på enheten hello:</span><span class="sxs-lookup"><span data-stu-id="8819f-123">Select hello starting point of hello new partition, or press `<enter>` tooaccept hello default tooplace hello partition at hello beginning of hello free space on hello drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="8819f-124">Välj hello storlek för hello partition, till exempel typ '+10G' toocreate en 10 GB-partition.</span><span class="sxs-lookup"><span data-stu-id="8819f-124">Select hello size of hello partition, for example type '+10G' toocreate a 10 gigabyte partition.</span></span> <span data-ttu-id="8819f-125">Eller tryck på `<enter>` skapa en enda partition som sträcker sig över hela enheten för hello:</span><span class="sxs-lookup"><span data-stu-id="8819f-125">Or, press `<enter>` create a single partition that spans hello entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="8819f-126">Ändra hello-ID och **t**typ av hello partition från hello standard ID '83' (Linux) tooID 'fd' (Linux raid auto):</span><span class="sxs-lookup"><span data-stu-id="8819f-126">Next, change hello ID and **t**ype of hello partition from hello default ID '83' (Linux) tooID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. <span data-ttu-id="8819f-127">Slutligen skriva hello partitionera tabellen toohello enhet och avsluta fdisk:</span><span class="sxs-lookup"><span data-stu-id="8819f-127">Finally, write hello partition table toohello drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a><span data-ttu-id="8819f-128">Skapa hello RAID-matris</span><span class="sxs-lookup"><span data-stu-id="8819f-128">Create hello RAID array</span></span>
1. <span data-ttu-id="8819f-129">hello följande exempel kommer ”stripe” (RAID-nivå 0) tre partitioner finns på tre separata hårddiskar (sdc1, sdd1, sde1).</span><span class="sxs-lookup"><span data-stu-id="8819f-129">hello following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="8819f-130">När du kör detta kommando en ny RAID-enhet kallas **/dev/md127** skapas.</span><span class="sxs-lookup"><span data-stu-id="8819f-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="8819f-131">Observera också att det kan vara nödvändigt tooadd hello om dessa data diskar vi tidigare tillhör en annan felaktiga RAID-matris `--force` parametern toohello `mdadm` kommando:</span><span class="sxs-lookup"><span data-stu-id="8819f-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary tooadd hello `--force` parameter toohello `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="8819f-132">Skapa hello filsystemet på hello nya RAID-enhet</span><span class="sxs-lookup"><span data-stu-id="8819f-132">Create hello file system on hello new RAID device</span></span>
   
    <span data-ttu-id="8819f-133">a.</span><span class="sxs-lookup"><span data-stu-id="8819f-133">a.</span></span> <span data-ttu-id="8819f-134">**CentOS, Oracle Linux, SLES 12, openSUSE och Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="8819f-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="8819f-135">b.</span><span class="sxs-lookup"><span data-stu-id="8819f-135">b.</span></span> <span data-ttu-id="8819f-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="8819f-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="8819f-137">c.</span><span class="sxs-lookup"><span data-stu-id="8819f-137">c.</span></span> <span data-ttu-id="8819f-138">**SLES 11** – aktivera boot.md och skapa mdadm.conf</span><span class="sxs-lookup"><span data-stu-id="8819f-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="8819f-139">En omstart kan krävas när du har gjort dessa ändringar på SUSE system.</span><span class="sxs-lookup"><span data-stu-id="8819f-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="8819f-140">Det här steget är *inte* krävs på SLES 12.</span><span class="sxs-lookup"><span data-stu-id="8819f-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="8819f-141">Lägg till hello nya filen system för/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="8819f-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8819f-142">Felaktigt Filredigering hello /etc/fstab kan resultera i ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="8819f-142">Improperly editing hello /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="8819f-143">Om du är osäker, toohello distribution dokumentationen information finns på hur tooproperly redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="8819f-143">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="8819f-144">Vi rekommenderar också att en säkerhetskopia av hello /etc/fstab filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="8819f-144">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="8819f-145">Skapa hello önskad monteringspunkt för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8819f-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="8819f-146">När du redigerar /etc/fstab hello **UUID** ska vara används tooreference hello file system i stället för hello enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="8819f-146">When editing /etc/fstab, hello **UUID** should be used tooreference hello file system rather than hello device name.</span></span>  <span data-ttu-id="8819f-147">Använd hello `blkid` verktyget toodetermine hello UUID för hello nytt filsystem:</span><span class="sxs-lookup"><span data-stu-id="8819f-147">Use hello `blkid` utility toodetermine hello UUID for hello new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="8819f-148">Öppna /etc/fstab i en textredigerare och Lägg till en post för hello nytt filsystem, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8819f-148">Open /etc/fstab in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="8819f-149">Eller på **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="8819f-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="8819f-150">Spara och Stäng /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="8819f-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="8819f-151">Testa att hello/etc/fstab inmatning är korrekt:</span><span class="sxs-lookup"><span data-stu-id="8819f-151">Test that hello /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="8819f-152">Om det här kommandot resulterar i ett felmeddelande, kontrollera hello syntax i hello /etc/fstab-filen.</span><span class="sxs-lookup"><span data-stu-id="8819f-152">If this command results in an error message, please check hello syntax in hello /etc/fstab file.</span></span>
   
    <span data-ttu-id="8819f-153">Kör nästa gång hello `mount` kommandot tooensure hello är monterat:</span><span class="sxs-lookup"><span data-stu-id="8819f-153">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="8819f-154">(Valfritt) Felsäker start-parametrar</span><span class="sxs-lookup"><span data-stu-id="8819f-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="8819f-155">**fstab konfiguration**</span><span class="sxs-lookup"><span data-stu-id="8819f-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="8819f-156">Många distributioner som innehåller antingen hello `nobootwait` eller `nofail` montera parametrar som kan läggas till filen toohello/etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="8819f-156">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello /etc/fstab file.</span></span> <span data-ttu-id="8819f-157">Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt hello Linux system toocontinue tooboot även om det är tooproperly montera hello RAID-filsystem.</span><span class="sxs-lookup"><span data-stu-id="8819f-157">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="8819f-158">Mer information om dessa parametrar finns i tooyour distribution dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8819f-158">Refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="8819f-159">Exempel (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="8819f-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="8819f-160">**Linux-Startparametrar**</span><span class="sxs-lookup"><span data-stu-id="8819f-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="8819f-161">Hej kernel-parametern i tillägg toohello ovan parametrar ”,`bootdegraded=true`” kan tillåta hello system tooboot även om hello RAID uppfattas som skadas eller försämrad, till exempel om en dataenhet oavsiktligt tas bort från hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8819f-161">In addition toohello above parameters, hello kernel parameter "`bootdegraded=true`" can allow hello system tooboot even if hello RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from hello virtual machine.</span></span> <span data-ttu-id="8819f-162">Det kan också leda till ett ej startbar system för som standard.</span><span class="sxs-lookup"><span data-stu-id="8819f-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="8819f-163">Se tooyour distribution dokumentation om hur tooproperly redigera kernel-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8819f-163">Please refer tooyour distribution's documentation on how tooproperly edit kernel parameters.</span></span> <span data-ttu-id="8819f-164">Till exempel i många distributioner (CentOS, Oracle Linux, SLES 11) dessa parametrar kan läggas till manuellt toohello ”`/boot/grub/menu.lst`” filen.</span><span class="sxs-lookup"><span data-stu-id="8819f-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually toohello "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="8819f-165">På Ubuntu den här parametern kan läggas till toohello `GRUB_CMDLINE_LINUX_DEFAULT` variabeln på ”/ etc/standard/grub”.</span><span class="sxs-lookup"><span data-stu-id="8819f-165">On Ubuntu this parameter can be added toohello `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="8819f-166">Stöd för TRIM/UNMAP</span><span class="sxs-lookup"><span data-stu-id="8819f-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="8819f-167">Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk.</span><span class="sxs-lookup"><span data-stu-id="8819f-167">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="8819f-168">Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="8819f-168">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="8819f-169">Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="8819f-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="8819f-170">RAID kan inte utfärda kommandon på Ignorera om hello segmentstorleken för hello matrisen är tooless än hello standardvärdet (512KB).</span><span class="sxs-lookup"><span data-stu-id="8819f-170">RAID may not issue discard commands if hello chunk size for hello array is set tooless than hello default (512KB).</span></span> <span data-ttu-id="8819f-171">Detta beror på att ta bort mappning hello granularitet på hello värden är också 512KB.</span><span class="sxs-lookup"><span data-stu-id="8819f-171">This is because hello unmap granularity on hello Host is also 512KB.</span></span> <span data-ttu-id="8819f-172">Om du har ändrat hello matrisens segmentstorleken via mdadm's `--chunk=` parameter och sedan ta bort TRIMNING/mappning begäranden ignoreras av hello kernel.</span><span class="sxs-lookup"><span data-stu-id="8819f-172">If you modified hello array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by hello kernel.</span></span>

<span data-ttu-id="8819f-173">Det finns två sätt tooenable TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="8819f-173">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="8819f-174">Som vanligt, kontakta din distribution för hello rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8819f-174">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="8819f-175">Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8819f-175">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="8819f-176">I vissa fall hello `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="8819f-176">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="8819f-177">Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:</span><span class="sxs-lookup"><span data-stu-id="8819f-177">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="8819f-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="8819f-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="8819f-179">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="8819f-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
