---
title: "Konfigurera LVM på en virtuell dator som kör Linux | Microsoft Docs"
description: "Lär dig hur du konfigurerar LVM på Linux i Azure."
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
ms.openlocfilehash: 7926627aaa3f0da935131f491d927ab5cb4b35c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="90ec1-103">Konfigurera LVM på en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="90ec1-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="90ec1-104">Det här dokumentet innehåller information om hur du konfigurerar logiska volymen Manager (LVM) i ditt virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="90ec1-104">This document will discuss how to configure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="90ec1-105">Det är möjligt att konfigurera LVM på en disk ansluten till den virtuella datorn, som standard de flesta molnet bilder inte LVM som konfigurerats på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="90ec1-105">While it is feasible to configure LVM on any disk attached to the virtual machine, by default most cloud images will not have LVM configured on the OS disk.</span></span> <span data-ttu-id="90ec1-106">Detta är att förhindra problem med dubbla volym grupper om OS-disken någonsin kopplas till en annan virtuell dator med samma distribution och typ, dvs. under en återställningsscenario.</span><span class="sxs-lookup"><span data-stu-id="90ec1-106">This is to prevent problems with duplicate volume groups if the OS disk is ever attached to another VM of the same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="90ec1-107">Därför rekommenderas endast för att använda LVM på datadiskar med.</span><span class="sxs-lookup"><span data-stu-id="90ec1-107">Therefore it is recommended only to use LVM on the data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="90ec1-108">Linjär kontra logiska stripe-volymer</span><span class="sxs-lookup"><span data-stu-id="90ec1-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="90ec1-109">LVM kan användas för att kombinera flera fysiska diskar till en enda lagringsvolym.</span><span class="sxs-lookup"><span data-stu-id="90ec1-109">LVM can be used to combine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="90ec1-110">Som standard skapar LVM vanligtvis linjär logiska volymer, vilket innebär att den fysiska lagringsplatsen sammanfogas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="90ec1-110">By default LVM will usually create linear logical volumes, which means that the physical storage is concatenated together.</span></span> <span data-ttu-id="90ec1-111">I det här fallet skickas läs-/ skrivåtgärder vanligtvis bara till en enskild disk.</span><span class="sxs-lookup"><span data-stu-id="90ec1-111">In this case read/write operations will typically only be sent to a single disk.</span></span> <span data-ttu-id="90ec1-112">Däremot kan vi skapa stripe-logiska volymer där läsningar och skrivningar distribueras till flera diskar som ingår i gruppen volym (d.v.s. liknar RAID0).</span><span class="sxs-lookup"><span data-stu-id="90ec1-112">In contrast, we can also create striped logical volumes where reads and writes are distributed to multiple disks contained in the volume group (i.e. similar to RAID0).</span></span> <span data-ttu-id="90ec1-113">Av prestandaskäl troligtvis kommer du vilja stripe-dina logiska volymer så att använda alla anslutna datadiskar läsningar och skrivningar.</span><span class="sxs-lookup"><span data-stu-id="90ec1-113">For performance reasons it is likely you will want to stripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="90ec1-114">Det här dokumentet beskrivs hur du kombinera flera datadiskar till en enda volym-grupp och sedan skapa en logisk stripe-volym.</span><span class="sxs-lookup"><span data-stu-id="90ec1-114">This document will describe how to combine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="90ec1-115">De här stegen är ganska generaliserade för att fungera med de flesta distributioner.</span><span class="sxs-lookup"><span data-stu-id="90ec1-115">The steps below are somewhat generalized to work with most distributions.</span></span> <span data-ttu-id="90ec1-116">I de flesta fall är verktyg och arbetsflöden för att hantera LVM i Azure inte helt annorlunda än andra miljöer.</span><span class="sxs-lookup"><span data-stu-id="90ec1-116">In most cases the utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="90ec1-117">Som vanligt också kontakta leverantören av Linux för dokumentation och bästa praxis för att använda LVM med en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="90ec1-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="90ec1-118">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="90ec1-118">Attaching data disks</span></span>
<span data-ttu-id="90ec1-119">En vill vanligtvis börja med minst två tomma datadiskar när du använder LVM.</span><span class="sxs-lookup"><span data-stu-id="90ec1-119">One will usually want to start with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="90ec1-120">Baserat på i/o-behov kan du välja att koppla diskar som är lagrade i vår standardlagring med upp till 500-i/o/ps per disk eller våra Premium-lagring med upp till 5 000 IO/ps per disk.</span><span class="sxs-lookup"><span data-stu-id="90ec1-120">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="90ec1-121">Den här artikeln kommer inte gå in i detalj att etablera och koppla datadiskar till en virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="90ec1-121">This article will not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span> <span data-ttu-id="90ec1-122">Finns i Microsoft Azure-artikeln [ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) detaljerade anvisningar om hur du kopplar en tom datadisk till en virtuell Linux-dator på Azure.</span><span class="sxs-lookup"><span data-stu-id="90ec1-122">Please see the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-lvm-utilities"></a><span data-ttu-id="90ec1-123">Installera LVM-verktyg</span><span class="sxs-lookup"><span data-stu-id="90ec1-123">Install the LVM utilities</span></span>
* <span data-ttu-id="90ec1-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="90ec1-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="90ec1-125">**RHEL, CentOS och Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="90ec1-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="90ec1-126">**SLES 12 och openSUSE**</span><span class="sxs-lookup"><span data-stu-id="90ec1-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="90ec1-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="90ec1-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="90ec1-128">På SLES11 måste du också redigera `/etc/sysconfig/lvm` och ange `LVM_ACTIVATED_ON_DISCOVERED` ”aktivera”:</span><span class="sxs-lookup"><span data-stu-id="90ec1-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` to "enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="90ec1-129">Konfigurera LVM</span><span class="sxs-lookup"><span data-stu-id="90ec1-129">Configure LVM</span></span>
<span data-ttu-id="90ec1-130">I den här guiden antar vi att du har kopplat tre datadiskar som vi ska kallar `/dev/sdc`, `/dev/sdd` och `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="90ec1-130">In this guide we will assume you have attached three data disks, which we'll refer to as `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="90ec1-131">Observera att de inte alltid samma sökvägsnamn i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="90ec1-131">Note that these may not always be the same path names in your VM.</span></span> <span data-ttu-id="90ec1-132">Du kan köra '`sudo fdisk -l`' eller liknande kommando för att visa en lista över tillgängliga diskar.</span><span class="sxs-lookup"><span data-stu-id="90ec1-132">You can run '`sudo fdisk -l`' or similar command to list your available disks.</span></span>

1. <span data-ttu-id="90ec1-133">Förbereda fysiska volymer:</span><span class="sxs-lookup"><span data-stu-id="90ec1-133">Prepare the physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="90ec1-134">Skapa en grupp för volymen.</span><span class="sxs-lookup"><span data-stu-id="90ec1-134">Create a volume group.</span></span> <span data-ttu-id="90ec1-135">I det här exemplet vi ringer gruppen volym `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="90ec1-135">In this example we are calling the volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="90ec1-136">Skapa logiska volymerna.</span><span class="sxs-lookup"><span data-stu-id="90ec1-136">Create the logical volume(s).</span></span> <span data-ttu-id="90ec1-137">Kommandot nedan vi skapar en logisk volym kallas `data-lv01` span gruppen hela volymen, men Observera att det är också möjligt att skapa flera logiska volymer i gruppen volym.</span><span class="sxs-lookup"><span data-stu-id="90ec1-137">The command below we will create a single logical volume called `data-lv01` to span the entire volume group, but note that it is also feasible to create multiple logical volumes in the volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="90ec1-138">Formatera den logiska</span><span class="sxs-lookup"><span data-stu-id="90ec1-138">Format the logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="90ec1-139">Med SLES11 använder `-t ext3` i stället för ext4.</span><span class="sxs-lookup"><span data-stu-id="90ec1-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="90ec1-140">SLES11 stöder endast skrivskyddad åtkomst till ext4 filsystem.</span><span class="sxs-lookup"><span data-stu-id="90ec1-140">SLES11 only supports read-only access to ext4 filesystems.</span></span>

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="90ec1-141">Lägg till det nya filsystemet /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="90ec1-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="90ec1-142">Felaktigt redigerar den `/etc/fstab` filen kan resultera i ett system som inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="90ec1-142">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="90ec1-143">Om du är osäker, se den fördelningen dokumentationen för information om hur du ska redigera den här filen.</span><span class="sxs-lookup"><span data-stu-id="90ec1-143">If unsure, please refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="90ec1-144">Det är också rekommenderar att en säkerhetskopia av den `/etc/fstab` filen har skapats innan du redigerar.</span><span class="sxs-lookup"><span data-stu-id="90ec1-144">It is also recommended that a backup of the `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="90ec1-145">Skapa den önskade monteringspunkten för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="90ec1-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="90ec1-146">Leta upp de logiska volymsökväg</span><span class="sxs-lookup"><span data-stu-id="90ec1-146">Locate the logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="90ec1-147">Öppna `/etc/fstab` i en textredigerare och Lägg till en post för det nya filsystemet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="90ec1-147">Open `/etc/fstab` in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="90ec1-148">Spara och Stäng `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="90ec1-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="90ec1-149">Testa den `/etc/fstab` posten är korrekt:</span><span class="sxs-lookup"><span data-stu-id="90ec1-149">Test that the `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="90ec1-150">Om det här kommandot resulterar i ett felmeddelande Kontrollera syntax den `/etc/fstab` filen.</span><span class="sxs-lookup"><span data-stu-id="90ec1-150">If this command results in an error message please check the syntax in the `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="90ec1-151">Kör nästa gång den `mount` kommando för att se till att filsystemet är monterad:</span><span class="sxs-lookup"><span data-stu-id="90ec1-151">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="90ec1-152">(Valfritt) Felsäker Startparametrar i`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="90ec1-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="90ec1-153">Många distributioner innehålla antingen den `nobootwait` eller `nofail` montera parametrar som kan läggas till i `/etc/fstab` fil.</span><span class="sxs-lookup"><span data-stu-id="90ec1-153">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="90ec1-154">Dessa parametrar tillåta fel när du monterar en viss filsystemet och Tillåt Linux-datorn och fortsätta att starta även om det inte går att montera korrekt RAID-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="90ec1-154">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="90ec1-155">Se dokumentationen för din distribution för mer information om dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="90ec1-155">Please refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="90ec1-156">Exempel (Ubuntu):</span><span class="sxs-lookup"><span data-stu-id="90ec1-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="90ec1-157">Stöd för TRIM/UNMAP</span><span class="sxs-lookup"><span data-stu-id="90ec1-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="90ec1-158">Vissa Linux kärnor stöd för TRIM/UNMAP åtgärder för att ta bort oanvända block på disken.</span><span class="sxs-lookup"><span data-stu-id="90ec1-158">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="90ec1-159">Dessa åtgärder är främst användbart för standardlagring att meddela Azure som bort sidor som inte längre är giltig och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="90ec1-159">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="90ec1-160">Ignorera sidor kan du spara kostnader om du skapar stora filer och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="90ec1-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="90ec1-161">Det finns två sätt att aktivera TRIMNING stöd i Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="90ec1-161">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="90ec1-162">Som vanligt, kontakta din distribution för den rekommenderade metoden:</span><span class="sxs-lookup"><span data-stu-id="90ec1-162">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="90ec1-163">Använd den `discard` montera alternativet i `/etc/fstab`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="90ec1-163">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="90ec1-164">I vissa fall den `discard` alternativet kanske prestanda.</span><span class="sxs-lookup"><span data-stu-id="90ec1-164">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="90ec1-165">Du kan också köra den `fstrim` kommandot manuellt från kommandoraden eller lägga till den i din crontab att köras regelbundet:</span><span class="sxs-lookup"><span data-stu-id="90ec1-165">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="90ec1-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="90ec1-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="90ec1-167">**RHEL/CentOS**</span><span class="sxs-lookup"><span data-stu-id="90ec1-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
