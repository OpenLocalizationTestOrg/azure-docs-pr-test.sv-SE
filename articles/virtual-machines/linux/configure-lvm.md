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
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="0779d-103">Konfigurera LVM på en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="0779d-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="0779d-104">Det här dokumentet innehåller information om hur tooconfigure logiska volymen Manager (LVM) i ditt virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="0779d-104">This document will discuss how tooconfigure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="0779d-105">Även om det är möjligt tooconfigure LVM på en disk ansluten toohello virtuell dator som standard de flesta molnet bilder inte har konfigurerats LVM på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="0779d-105">While it is feasible tooconfigure LVM on any disk attached toohello virtual machine, by default most cloud images will not have LVM configured on hello OS disk.</span></span> <span data-ttu-id="0779d-106">Detta är tooprevent problem med dubbla volym grupper om hello OS-disken är någonsin kopplade tooanother VM av hello samma distribution och typ, dvs. under en återställningsscenario.</span><span class="sxs-lookup"><span data-stu-id="0779d-106">This is tooprevent problems with duplicate volume groups if hello OS disk is ever attached tooanother VM of hello same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="0779d-107">Därför rekommenderas endast toouse LVM på hello datadiskar.</span><span class="sxs-lookup"><span data-stu-id="0779d-107">Therefore it is recommended only toouse LVM on hello data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="0779d-108">Linjär kontra logiska stripe-volymer</span><span class="sxs-lookup"><span data-stu-id="0779d-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="0779d-109">LVM kan vara används toocombine ett antal fysiska diskar i en enda lagringsvolym.</span><span class="sxs-lookup"><span data-stu-id="0779d-109">LVM can be used toocombine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="0779d-110">Som standard skapar LVM vanligtvis linjär logiska volymer, vilket innebär att hello fysisk lagring sammanfogas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="0779d-110">By default LVM will usually create linear logical volumes, which means that hello physical storage is concatenated together.</span></span> <span data-ttu-id="0779d-111">I det här fallet skickas läs-/ skrivåtgärder vanligtvis bara tooa enskild disk.</span><span class="sxs-lookup"><span data-stu-id="0779d-111">In this case read/write operations will typically only be sent tooa single disk.</span></span> <span data-ttu-id="0779d-112">Däremot kan vi skapa stripe-logiska volymer där läsningar och skrivningar är distribuerade toomultiple diskar i hello volym grupp (d.v.s. liknande tooRAID0).</span><span class="sxs-lookup"><span data-stu-id="0779d-112">In contrast, we can also create striped logical volumes where reads and writes are distributed toomultiple disks contained in hello volume group (i.e. similar tooRAID0).</span></span> <span data-ttu-id="0779d-113">Av prestandaskäl är förmodligen vill toostripe dina logiska volymer så att använda alla anslutna datadiskar läsningar och skrivningar.</span><span class="sxs-lookup"><span data-stu-id="0779d-113">For performance reasons it is likely you will want toostripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="0779d-114">Det här dokumentet beskrivs hur toocombine data flera diskar i en enda volym-grupp och sedan skapa en logisk stripe-volym.</span><span class="sxs-lookup"><span data-stu-id="0779d-114">This document will describe how toocombine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="0779d-115">hello stegen nedan är något generaliserad toowork med de flesta distributioner.</span><span class="sxs-lookup"><span data-stu-id="0779d-115">hello steps below are somewhat generalized toowork with most distributions.</span></span> <span data-ttu-id="0779d-116">I de flesta fall hello verktyg och arbetsflöden för att hantera LVM i Azure är inte helt annorlunda än andra miljöer.</span><span class="sxs-lookup"><span data-stu-id="0779d-116">In most cases hello utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="0779d-117">Som vanligt också kontakta leverantören av Linux för dokumentation och bästa praxis för att använda LVM med en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="0779d-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="0779d-118">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="0779d-118">Attaching data disks</span></span>
<span data-ttu-id="0779d-119">En ska vanligtvis toostart med två eller flera diskar i empty-data när du använder LVM.</span><span class="sxs-lookup"><span data-stu-id="0779d-119">One will usually want toostart with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="0779d-120">Baserat på i/o-behov kan du välja tooattach diskar som är lagrade i vår standardlagring med upp too500 IO/ps per disk eller våra Premium-lagring med upp too5000 IO/ps per disk.</span><span class="sxs-lookup"><span data-stu-id="0779d-120">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="0779d-121">Den här artikeln kommer inte gå in i detalj på hur tooprovision och bifoga data diskar tooa Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="0779d-121">This article will not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span> <span data-ttu-id="0779d-122">Se hello Microsoft Azure artikel [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur tooattach en tom data disk tooa Linux-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="0779d-122">Please see hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-lvm-utilities"></a><span data-ttu-id="0779d-123">Installera hello LVM verktyg</span><span class="sxs-lookup"><span data-stu-id="0779d-123">Install hello LVM utilities</span></span>
* <span data-ttu-id="0779d-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="0779d-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="0779d-125">**RHEL, CentOS och Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="0779d-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="0779d-126">**SLES 12 och openSUSE**</span><span class="sxs-lookup"><span data-stu-id="0779d-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="0779d-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="0779d-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="0779d-128">På SLES11 måste du också redigera `/etc/sysconfig/lvm` och ange `LVM_ACTIVATED_ON_DISCOVERED` för ”aktivera”:</span><span class="sxs-lookup"><span data-stu-id="0779d-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` too"enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="0779d-129">Konfigurera LVM</span><span class="sxs-lookup"><span data-stu-id="0779d-129">Configure LVM</span></span>
<span data-ttu-id="0779d-130">I den här guiden antar vi att du har kopplat tre datadiskar som vi ska refererar tooas `/dev/sdc`, `/dev/sdd` och `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="0779d-130">In this guide we will assume you have attached three data disks, which we'll refer tooas `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="0779d-131">Observera att dessa inte kan alltid vara hello samma sökvägsnamn i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0779d-131">Note that these may not always be hello same path names in your VM.</span></span> <span data-ttu-id="0779d-132">Du kan köra '`sudo fdisk -l`' eller liknande kommandot toolist din tillgängliga diskar.</span><span class="sxs-lookup"><span data-stu-id="0779d-132">You can run '`sudo fdisk -l`' or similar command toolist your available disks.</span></span>

1. <span data-ttu-id="0779d-133">Förbered hello fysiska volymer:</span><span class="sxs-lookup"><span data-stu-id="0779d-133">Prepare hello physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="0779d-134">Skapa en grupp för volymen.</span><span class="sxs-lookup"><span data-stu-id="0779d-134">Create a volume group.</span></span> <span data-ttu-id="0779d-135">I det här exemplet vi ringer hello volym grupp `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="0779d-135">In this example we are calling hello volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="0779d-136">Skapa hello logiska volymerna.</span><span class="sxs-lookup"><span data-stu-id="0779d-136">Create hello logical volume(s).</span></span> <span data-ttu-id="0779d-137">hello-kommandot nedan vi skapar en logisk volym kallas `data-lv01` toospan hello hela volymen gruppera, men Observera att det är också möjligt toocreate flera logiska volymer i hello volym grupp.</span><span class="sxs-lookup"><span data-stu-id="0779d-137">hello command below we will create a single logical volume called `data-lv01` toospan hello entire volume group, but note that it is also feasible toocreate multiple logical volumes in hello volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="0779d-138">Formatera hello logiska volym</span><span class="sxs-lookup"><span data-stu-id="0779d-138">Format hello logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="0779d-139">Med SLES11 använder `-t ext3` i stället för ext4.</span><span class="sxs-lookup"><span data-stu-id="0779d-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="0779d-140">SLES11 stöder bara läsbehörighet tooext4 filsystem.</span><span class="sxs-lookup"><span data-stu-id="0779d-140">SLES11 only supports read-only access tooext4 filesystems.</span></span>

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="0779d-141">Lägg till hello nya filen system för/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="0779d-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0779d-142">Felaktigt redigera hello `/etc/fstab` filen kan resultera i ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="0779d-142">Improperly editing hello `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="0779d-143">Om du är osäker, se toohello distribution dokumentation för information om hur tooproperly redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="0779d-143">If unsure, please refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="0779d-144">Det är också rekommenderar att en säkerhetskopia av hello `/etc/fstab` filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="0779d-144">It is also recommended that a backup of hello `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="0779d-145">Skapa hello önskad monteringspunkt för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0779d-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="0779d-146">Leta upp hello logiska volymsökväg</span><span class="sxs-lookup"><span data-stu-id="0779d-146">Locate hello logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="0779d-147">Öppna `/etc/fstab` i en textredigerare och Lägg till en post för hello nytt filsystem, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0779d-147">Open `/etc/fstab` in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="0779d-148">Spara och Stäng `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="0779d-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="0779d-149">Testa att hello `/etc/fstab` inmatning är korrekt:</span><span class="sxs-lookup"><span data-stu-id="0779d-149">Test that hello `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="0779d-150">Om det här kommandot resulterar i ett felmeddelande du kontrollera hello syntax i hello `/etc/fstab` fil.</span><span class="sxs-lookup"><span data-stu-id="0779d-150">If this command results in an error message please check hello syntax in hello `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="0779d-151">Kör nästa gång hello `mount` kommandot tooensure hello är monterat:</span><span class="sxs-lookup"><span data-stu-id="0779d-151">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="0779d-152">(Valfritt) Felsäker Startparametrar i`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="0779d-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="0779d-153">Många distributioner som innehåller antingen hello `nobootwait` eller `nofail` montera parametrar som kan läggas till toohello `/etc/fstab` fil.</span><span class="sxs-lookup"><span data-stu-id="0779d-153">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello `/etc/fstab` file.</span></span> <span data-ttu-id="0779d-154">Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt hello Linux system toocontinue tooboot även om det är tooproperly montera hello RAID-filsystem.</span><span class="sxs-lookup"><span data-stu-id="0779d-154">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="0779d-155">Mer information om dessa parametrar finns i tooyour distribution-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="0779d-155">Please refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="0779d-156">Exempel (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="0779d-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="0779d-157">Stöd för TRIM/UNMAP</span><span class="sxs-lookup"><span data-stu-id="0779d-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="0779d-158">Vissa Linux kärnor stöd för TRIM/UNMAP operations toodiscard oanvända block på hello disk.</span><span class="sxs-lookup"><span data-stu-id="0779d-158">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="0779d-159">Dessa åtgärder är främst användbart för standardlagring tooinform Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="0779d-159">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="0779d-160">Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="0779d-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="0779d-161">Det finns två sätt tooenable TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="0779d-161">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="0779d-162">Som vanligt, kontakta din distribution för hello rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0779d-162">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="0779d-163">Använd hello `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0779d-163">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="0779d-164">I vissa fall hello `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="0779d-164">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="0779d-165">Du kan också köra hello `fstrim` kommandot manuellt från hello kommandorad, eller lägga till den tooyour crontab toorun regelbundet:</span><span class="sxs-lookup"><span data-stu-id="0779d-165">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="0779d-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="0779d-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="0779d-167">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="0779d-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
