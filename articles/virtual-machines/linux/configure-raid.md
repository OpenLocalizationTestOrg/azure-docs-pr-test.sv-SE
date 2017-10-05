---
title: "Konfigurera programvarubaserad RAID på en virtuell dator som kör Linux | Microsoft Docs"
description: "Lär dig hur du använder mdadm för att konfigurera RAID på Linux i Azure."
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
ms.openlocfilehash: 12f540a700fbf85e579e8aadc9f6def039299ff7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="5672b-103">Konfigurera programvaru-RAID på Linux</span><span class="sxs-lookup"><span data-stu-id="5672b-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="5672b-104">Det är ett vanligt scenario du använder programvarubaserad RAID på Linux virtuella datorer i Azure för att presentera flera diskar i bifogade data som en enkel RAID-enhet.</span><span class="sxs-lookup"><span data-stu-id="5672b-104">It's a common scenario to use software RAID on Linux virtual machines in Azure to present multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="5672b-105">Vanligtvis kan detta användas för att förbättra prestanda och möjliggöra bättre genomflöde jämfört med bara en enda disk.</span><span class="sxs-lookup"><span data-stu-id="5672b-105">Typically this can be used to improve performance and allow for improved throughput compared to using just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="5672b-106">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="5672b-106">Attaching data disks</span></span>
<span data-ttu-id="5672b-107">Två eller flera tom datadiskar behövs för att konfigurera en RAID-enhet.</span><span class="sxs-lookup"><span data-stu-id="5672b-107">Two or more empty data disks are needed to configure a RAID device.</span></span>  <span data-ttu-id="5672b-108">Den främsta orsaken till att skapa en RAID-enhet är att förbättra prestanda för disken i/o.</span><span class="sxs-lookup"><span data-stu-id="5672b-108">The primary reason for creating a RAID device is to improve performance of your disk IO.</span></span>  <span data-ttu-id="5672b-109">Baserat på i/o-behov kan du välja att koppla diskar som är lagrade i vår standardlagring med upp till 500-i/o/ps per disk eller våra Premium-lagring med upp till 5 000 IO/ps per disk.</span><span class="sxs-lookup"><span data-stu-id="5672b-109">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="5672b-110">Den här artikeln går inte in i detalj att etablera och koppla datadiskar till en virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="5672b-110">This article does not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span>  <span data-ttu-id="5672b-111">Finns i Microsoft Azure-artikel [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur du kopplar en tom datadisk till en virtuell Linux-dator på Azure.</span><span class="sxs-lookup"><span data-stu-id="5672b-111">See the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-mdadm-utility"></a><span data-ttu-id="5672b-112">Installera verktyget mdadm</span><span class="sxs-lookup"><span data-stu-id="5672b-112">Install the mdadm utility</span></span>
* <span data-ttu-id="5672b-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="5672b-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="5672b-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="5672b-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="5672b-115">**SLES och openSUSE**</span><span class="sxs-lookup"><span data-stu-id="5672b-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-the-disk-partitions"></a><span data-ttu-id="5672b-116">Skapa diskpartitioner</span><span class="sxs-lookup"><span data-stu-id="5672b-116">Create the disk partitions</span></span>
<span data-ttu-id="5672b-117">I det här exemplet skapar vi en enskild diskpartition på /dev/sdc.</span><span class="sxs-lookup"><span data-stu-id="5672b-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="5672b-118">Den nya diskpartitionen kallas /dev/sdc1.</span><span class="sxs-lookup"><span data-stu-id="5672b-118">The new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="5672b-119">Starta `fdisk` att börja skapa partitioner</span><span class="sxs-lookup"><span data-stu-id="5672b-119">Start `fdisk` to begin creating partitions</span></span>

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. <span data-ttu-id="5672b-120">Tryck på ”n” i Kommandotolken för att skapa en  **n** y-partition:</span><span class="sxs-lookup"><span data-stu-id="5672b-120">Press 'n' at the prompt to create a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="5672b-121">Tryck sedan på p om du vill skapa en **p**rimär partition:</span><span class="sxs-lookup"><span data-stu-id="5672b-121">Next, press 'p' to create a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="5672b-122">Tryck '1' för att markera partitionsnumret 1:</span><span class="sxs-lookup"><span data-stu-id="5672b-122">Press '1' to select partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="5672b-123">Välj startpunkten för den nya partitionen eller tryck på `<enter>` att acceptera standardvärdet om du vill placera partitionen i början av det lediga utrymmet på enheten:</span><span class="sxs-lookup"><span data-stu-id="5672b-123">Select the starting point of the new partition, or press `<enter>` to accept the default to place the partition at the beginning of the free space on the drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="5672b-124">Välj storleken på partitionen, till exempel skriver ”+10G' om du vill skapa en 10 GB-partition.</span><span class="sxs-lookup"><span data-stu-id="5672b-124">Select the size of the partition, for example type '+10G' to create a 10 gigabyte partition.</span></span> <span data-ttu-id="5672b-125">Eller tryck på `<enter>` skapa en enda partition som sträcker sig över hela enheten:</span><span class="sxs-lookup"><span data-stu-id="5672b-125">Or, press `<enter>` create a single partition that spans the entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="5672b-126">Ändra ID och **t**typ av partitionen från standard-ID '83' (Linux)-ID: t 'fd' (Linux raid auto):</span><span class="sxs-lookup"><span data-stu-id="5672b-126">Next, change the ID and **t**ype of the partition from the default ID '83' (Linux) to ID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

8. <span data-ttu-id="5672b-127">Slutligen skriva partitionstabellen till enheten och avsluta fdisk:</span><span class="sxs-lookup"><span data-stu-id="5672b-127">Finally, write the partition table to the drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a><span data-ttu-id="5672b-128">Skapa RAID-matris</span><span class="sxs-lookup"><span data-stu-id="5672b-128">Create the RAID array</span></span>
1. <span data-ttu-id="5672b-129">I följande exempel startas ”stripe” (RAID-nivå 0) tre partitioner finns på tre separata hårddiskar (sdc1, sdd1, sde1).</span><span class="sxs-lookup"><span data-stu-id="5672b-129">The following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="5672b-130">När du kör detta kommando en ny RAID-enhet kallas **/dev/md127** skapas.</span><span class="sxs-lookup"><span data-stu-id="5672b-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="5672b-131">Observera också att om dessa data diskar vi tidigare tillhör en annan felaktiga RAID-matris det kan vara nödvändigt att lägga till den `--force` parametern till den `mdadm` kommando:</span><span class="sxs-lookup"><span data-stu-id="5672b-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary to add the `--force` parameter to the `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="5672b-132">Skapa filsystemet på den nya RAID-enheten</span><span class="sxs-lookup"><span data-stu-id="5672b-132">Create the file system on the new RAID device</span></span>
   
    <span data-ttu-id="5672b-133">a.</span><span class="sxs-lookup"><span data-stu-id="5672b-133">a.</span></span> <span data-ttu-id="5672b-134">**CentOS, Oracle Linux, SLES 12, openSUSE och Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="5672b-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="5672b-135">b.</span><span class="sxs-lookup"><span data-stu-id="5672b-135">b.</span></span> <span data-ttu-id="5672b-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="5672b-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="5672b-137">c.</span><span class="sxs-lookup"><span data-stu-id="5672b-137">c.</span></span> <span data-ttu-id="5672b-138">**SLES 11** – aktivera boot.md och skapa mdadm.conf</span><span class="sxs-lookup"><span data-stu-id="5672b-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="5672b-139">En omstart kan krävas när du har gjort dessa ändringar på SUSE system.</span><span class="sxs-lookup"><span data-stu-id="5672b-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="5672b-140">Det här steget är *inte* krävs på SLES 12.</span><span class="sxs-lookup"><span data-stu-id="5672b-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="5672b-141">Lägg till det nya filsystemet /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="5672b-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5672b-142">Redigera filen /etc/fstab felaktigt kan det leda till att ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="5672b-142">Improperly editing the /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="5672b-143">Om du är osäker, i den distribution dokumentationen finns information om hur du ska redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="5672b-143">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="5672b-144">Vi rekommenderar också att en säkerhetskopia av /etc/fstab-filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="5672b-144">It is also recommended that a backup of the /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="5672b-145">Skapa den önskade monteringspunkten för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5672b-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="5672b-146">När du redigerar /etc/fstab, den **UUID** ska användas för att referera till filsystemet i stället för namnet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="5672b-146">When editing /etc/fstab, the **UUID** should be used to reference the file system rather than the device name.</span></span>  <span data-ttu-id="5672b-147">Använd den `blkid` verktyg för att fastställa UUID för det nya filsystemet:</span><span class="sxs-lookup"><span data-stu-id="5672b-147">Use the `blkid` utility to determine the UUID for the new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="5672b-148">Öppna /etc/fstab i en textredigerare och Lägg till en post för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5672b-148">Open /etc/fstab in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="5672b-149">Eller på **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="5672b-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="5672b-150">Spara och Stäng /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="5672b-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="5672b-151">Testa att/etc/fstab posten är korrekt:</span><span class="sxs-lookup"><span data-stu-id="5672b-151">Test that the /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="5672b-152">Om det här kommandot resulterar i ett felmeddelande, kontrollera syntax i filen /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="5672b-152">If this command results in an error message, please check the syntax in the /etc/fstab file.</span></span>
   
    <span data-ttu-id="5672b-153">Kör nästa gång den `mount` kommando för att se till att filsystemet är monterad:</span><span class="sxs-lookup"><span data-stu-id="5672b-153">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="5672b-154">(Valfritt) Felsäker start-parametrar</span><span class="sxs-lookup"><span data-stu-id="5672b-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="5672b-155">**fstab konfiguration**</span><span class="sxs-lookup"><span data-stu-id="5672b-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="5672b-156">Många distributioner innehålla antingen den `nobootwait` eller `nofail` montera parametrar som kan läggas till i filen/etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="5672b-156">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the /etc/fstab file.</span></span> <span data-ttu-id="5672b-157">Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt Linux-datorn och fortsätta att starta även om det inte går att montera korrekt RAID-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="5672b-157">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="5672b-158">Finns i dokumentationen för din distribution för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="5672b-158">Refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="5672b-159">Exempel (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="5672b-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="5672b-160">**Linux-Startparametrar**</span><span class="sxs-lookup"><span data-stu-id="5672b-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="5672b-161">Förutom ovanstående parametrar, kernel-parametern ”`bootdegraded=true`” kan tillåta systemet att starta även om RAID uppfattas som skadade eller försämrad för exempel om en dataenhet oavsiktligt tas bort från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5672b-161">In addition to the above parameters, the kernel parameter "`bootdegraded=true`" can allow the system to boot even if the RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from the virtual machine.</span></span> <span data-ttu-id="5672b-162">Det kan också leda till ett ej startbar system för som standard.</span><span class="sxs-lookup"><span data-stu-id="5672b-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="5672b-163">Se dokumentationen för din distribution om hur du redigerar korrekt kernel-parametrar.</span><span class="sxs-lookup"><span data-stu-id="5672b-163">Please refer to your distribution's documentation on how to properly edit kernel parameters.</span></span> <span data-ttu-id="5672b-164">Till exempel i många distributioner (CentOS, Oracle Linux, SLES 11) dessa parametrar kan läggas till manuellt till det ”`/boot/grub/menu.lst`” filen.</span><span class="sxs-lookup"><span data-stu-id="5672b-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually to the "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="5672b-165">På Ubuntu den här parametern kan läggas till i `GRUB_CMDLINE_LINUX_DEFAULT` variabeln på ”/ etc/standard/grub”.</span><span class="sxs-lookup"><span data-stu-id="5672b-165">On Ubuntu this parameter can be added to the `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="5672b-166">Stöd för TRIM/UNMAP</span><span class="sxs-lookup"><span data-stu-id="5672b-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="5672b-167">Vissa Linux kärnor stöd för TRIM/UNMAP åtgärder för att ta bort oanvända block på disken.</span><span class="sxs-lookup"><span data-stu-id="5672b-167">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="5672b-168">Dessa åtgärder är främst användbart för standardlagring att meddela Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="5672b-168">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="5672b-169">Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="5672b-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="5672b-170">RAID kan inte utfärda kommandon på Ignorera om segmentstorleken för matrisen anges till mindre än standardvärdet (512KB).</span><span class="sxs-lookup"><span data-stu-id="5672b-170">RAID may not issue discard commands if the chunk size for the array is set to less than the default (512KB).</span></span> <span data-ttu-id="5672b-171">Detta beror på att unmap granularitet på värden är också 512KB.</span><span class="sxs-lookup"><span data-stu-id="5672b-171">This is because the unmap granularity on the Host is also 512KB.</span></span> <span data-ttu-id="5672b-172">Om du har ändrat matrisens segmentstorleken via mdadm's `--chunk=` parameter och sedan ta bort TRIMNING/mappning begäranden ignoreras av kernel.</span><span class="sxs-lookup"><span data-stu-id="5672b-172">If you modified the array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by the kernel.</span></span>

<span data-ttu-id="5672b-173">Det finns två sätt att aktivera TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="5672b-173">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="5672b-174">Som vanligt, kontakta din distribution för den rekommenderade metoden:</span><span class="sxs-lookup"><span data-stu-id="5672b-174">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="5672b-175">Använd den `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5672b-175">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="5672b-176">I vissa fall den `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="5672b-176">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="5672b-177">Du kan också köra den `fstrim` kommandot manuellt från kommandoraden eller lägga till den i din crontab att köras regelbundet:</span><span class="sxs-lookup"><span data-stu-id="5672b-177">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="5672b-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="5672b-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="5672b-179">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="5672b-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
