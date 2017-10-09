---
title: "aaaOptimize MySQL prestanda på Linux | Microsoft Docs"
description: "Lär dig hur toooptimize MySQL körs på en Azure-dator (VM) kör Linux."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="f257a-103">Optimera MySQL prestanda på virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="f257a-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="f257a-104">Det finns många faktorer som påverkar prestanda MySQL på Azure, både i val av virtuell maskinvara och konfiguration av programvara.</span><span class="sxs-lookup"><span data-stu-id="f257a-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="f257a-105">Den här artikeln fokuserar på optimera prestanda via lagring, system och konfigurationer för databasen.</span><span class="sxs-lookup"><span data-stu-id="f257a-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f257a-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk.</span><span class="sxs-lookup"><span data-stu-id="f257a-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="f257a-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f257a-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="f257a-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="f257a-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f257a-109">Information om Linux VM optimeringar med hello Resource Manager-modellen finns [optimera din Linux VM på Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f257a-109">For information about Linux VM optimizations with hello Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="f257a-110">Använda RAID på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="f257a-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="f257a-111">Lagring är hello viktiga faktor som påverkar databasprestanda i miljöer i molnet.</span><span class="sxs-lookup"><span data-stu-id="f257a-111">Storage is hello key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="f257a-112">Jämfört med tooa enskild disk RAID ger snabbare åtkomst via samtidighet.</span><span class="sxs-lookup"><span data-stu-id="f257a-112">Compared tooa single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="f257a-113">Mer information finns i [Standard RAID-nivåer](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="f257a-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="f257a-114">I/o-genomflöde och svarstid för i/o i Azure kan förbättras genom RAID.</span><span class="sxs-lookup"><span data-stu-id="f257a-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="f257a-115">Våra tester visar att i/o-genomflöde kan vara dubblerad och i/o-svarstid kan reduceras med hälften i genomsnitt när fördubblas hello antalet RAID-diskar (från två toofour, fyra tooeight osv.).</span><span class="sxs-lookup"><span data-stu-id="f257a-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when hello number of RAID disks is doubled (from two toofour, four tooeight, etc.).</span></span> <span data-ttu-id="f257a-116">Se [bilaga A](#AppendixA) mer information.</span><span class="sxs-lookup"><span data-stu-id="f257a-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="f257a-117">Dessutom toodisk i/o, MySQL prestanda förbättras om du ökar hello RAID-nivå.</span><span class="sxs-lookup"><span data-stu-id="f257a-117">In addition toodisk I/O, MySQL performance improves when you increase hello RAID level.</span></span>  <span data-ttu-id="f257a-118">Se [bilaga B](#AppendixB) mer information.</span><span class="sxs-lookup"><span data-stu-id="f257a-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="f257a-119">Du kan också tooconsider hello segmentstorleken.</span><span class="sxs-lookup"><span data-stu-id="f257a-119">You might also want tooconsider hello chunk size.</span></span> <span data-ttu-id="f257a-120">I allmänhet när du har en större segmentstorlek kan hämta du lägre omkostnader, särskilt för stora skrivningar.</span><span class="sxs-lookup"><span data-stu-id="f257a-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="f257a-121">När hello segmentstorleken är för stor, kan det dock lägga till ytterligare kostnader som hindrar dig från att dra nytta av RAID.</span><span class="sxs-lookup"><span data-stu-id="f257a-121">However, when hello chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="f257a-122">hello aktuella standardstorleken är 512 KB som bevisas toobe som är optimala för mest allmänna produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="f257a-122">hello current default size is 512 KB, which is proven toobe optimal for most general production environments.</span></span> <span data-ttu-id="f257a-123">Se [bilaga C](#AppendixC) mer information.</span><span class="sxs-lookup"><span data-stu-id="f257a-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="f257a-124">Det finns begränsningar på hur många diskar som du kan lägga till för olika virtuella typer.</span><span class="sxs-lookup"><span data-stu-id="f257a-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="f257a-125">Dessa värden beskrivs i [storlekar för virtuella datorer och moln för Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="f257a-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="f257a-126">Du behöver fyra anslutna diskar toofollow hello RAID exempel på data i den här artikeln, men du kan välja tooset in RAID med färre diskar.</span><span class="sxs-lookup"><span data-stu-id="f257a-126">You will need four attached data disks toofollow hello RAID example in this article, although you can choose tooset up RAID with fewer disks.</span></span>  

<span data-ttu-id="f257a-127">Den här artikeln förutsätter att du redan har skapat en virtuell Linux-dator och ha MYSQL installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="f257a-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="f257a-128">Mer information om att komma igång, se hur tooinstall MySQL på Azure.</span><span class="sxs-lookup"><span data-stu-id="f257a-128">For more information on getting started, see How tooinstall MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="f257a-129">Ställ in RAID på Azure</span><span class="sxs-lookup"><span data-stu-id="f257a-129">Set up RAID on Azure</span></span>
<span data-ttu-id="f257a-130">hello följande steg visar hur toocreate RAID på Azure med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f257a-130">hello following steps show how toocreate RAID on Azure by using hello Azure portal.</span></span> <span data-ttu-id="f257a-131">Du kan också ställa in RAID med hjälp av Windows PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="f257a-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="f257a-132">I det här exemplet ska vi konfigurera RAID 0 med fyra diskar.</span><span class="sxs-lookup"><span data-stu-id="f257a-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a><span data-ttu-id="f257a-133">Lägg till en data disk tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f257a-133">Add a data disk tooyour virtual machine</span></span>
<span data-ttu-id="f257a-134">Gå toohello instrumentpanelen och välj hello virtuella toowhich som du vill tooadd en datadisk i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f257a-134">In hello Azure portal, go toohello dashboard and select hello virtual machine toowhich you want tooadd a data disk.</span></span> <span data-ttu-id="f257a-135">I det här exemplet är hello virtuella mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="f257a-135">In this example, hello virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="f257a-136">Klicka på **diskar** och klicka sedan på **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="f257a-136">Click **Disks** and then click **Attach New**.</span></span>

![Virtuella datorer lägger du till disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="f257a-138">Skapa en ny 500 GB disk.</span><span class="sxs-lookup"><span data-stu-id="f257a-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="f257a-139">Se till att **värden Cache inställningar** har angetts för**ingen**.</span><span class="sxs-lookup"><span data-stu-id="f257a-139">Make sure that **Host Cache Preference** is set too**None**.</span></span>  <span data-ttu-id="f257a-140">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f257a-140">When you're finished, click **OK**.</span></span>

![Anslut tom disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="f257a-142">Detta lägger till en tom disk i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f257a-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="f257a-143">Upprepa detta steg tre gånger så att du har fyra datadiskar för RAID.</span><span class="sxs-lookup"><span data-stu-id="f257a-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="f257a-144">Du kan se hello läggas till enheter i hello virtuell dator genom att titta på hello kernel message-loggen.</span><span class="sxs-lookup"><span data-stu-id="f257a-144">You can see hello added drives in hello virtual machine by looking at hello kernel message log.</span></span> <span data-ttu-id="f257a-145">Till exempel toosee detta på Ubuntu, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-145">For example, toosee this on Ubuntu, use hello following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a><span data-ttu-id="f257a-146">Skapa RAID med hello ytterligare diskar</span><span class="sxs-lookup"><span data-stu-id="f257a-146">Create RAID with hello additional disks</span></span>
<span data-ttu-id="f257a-147">hello följande steg beskriver hur för[konfigurera programvarubaserad RAID på Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f257a-147">hello following steps describe how too[configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="f257a-148">Om du använder hello XFS filsystem, köra hello följande steg när du har skapat RAID.</span><span class="sxs-lookup"><span data-stu-id="f257a-148">If you are using hello XFS file system, execute hello following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="f257a-149">tooinstall XFS på Debian och Ubuntu Linux myntverket Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-149">tooinstall XFS on Debian, Ubuntu, or Linux Mint, use hello following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="f257a-150">tooinstall XFS på Fedora, CentOS eller RHEL, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-150">tooinstall XFS on Fedora, CentOS, or RHEL, use hello following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="f257a-151">Ställ in en ny lagringssökväg</span><span class="sxs-lookup"><span data-stu-id="f257a-151">Set up a new storage path</span></span>
<span data-ttu-id="f257a-152">Använd hello följande kommando tooset in sökvägen till en ny lagring:</span><span class="sxs-lookup"><span data-stu-id="f257a-152">Use hello following command tooset up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a><span data-ttu-id="f257a-153">Kopiera hello ursprungliga toohello ny lagring datasökväg</span><span class="sxs-lookup"><span data-stu-id="f257a-153">Copy hello original data toohello new storage path</span></span>
<span data-ttu-id="f257a-154">Använd följande kommando toocopy toohello ny lagring datasökväg hello:</span><span class="sxs-lookup"><span data-stu-id="f257a-154">Use hello following command toocopy data toohello new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a><span data-ttu-id="f257a-155">Ändra behörigheter så att MySQL kan komma åt (läsning och skrivning) hello datadisk</span><span class="sxs-lookup"><span data-stu-id="f257a-155">Modify permissions so MySQL can access (read and write) hello data disk</span></span>
<span data-ttu-id="f257a-156">Använd följande kommando toomodify behörigheter hello:</span><span class="sxs-lookup"><span data-stu-id="f257a-156">Use hello following command toomodify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a><span data-ttu-id="f257a-157">Justera hello disk i/o schemaläggning algoritm</span><span class="sxs-lookup"><span data-stu-id="f257a-157">Adjust hello disk I/O scheduling algorithm</span></span>
<span data-ttu-id="f257a-158">Linux implementerar fyra typer av i/o schemaläggning algoritmer:</span><span class="sxs-lookup"><span data-stu-id="f257a-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="f257a-159">NOOP algoritmen (åtgärden kan Nej)</span><span class="sxs-lookup"><span data-stu-id="f257a-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="f257a-160">Tidsgräns för algoritmen (tidsgräns)</span><span class="sxs-lookup"><span data-stu-id="f257a-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="f257a-161">Helt verkliga MSMQ-algoritmen (CFQ)</span><span class="sxs-lookup"><span data-stu-id="f257a-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="f257a-162">Budget period algoritmen (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="f257a-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="f257a-163">Du kan välja olika i/o-planeringsprogram under olika scenarier toooptimize prestanda.</span><span class="sxs-lookup"><span data-stu-id="f257a-163">You can select different I/O schedulers under different scenarios toooptimize performance.</span></span> <span data-ttu-id="f257a-164">I en miljö med helt direktåtkomst finns det inte en betydande skillnader mellan hello CFQ och tidsgräns algoritmer för prestanda.</span><span class="sxs-lookup"><span data-stu-id="f257a-164">In a completely random access environment, there is not a significant difference between hello CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="f257a-165">Vi rekommenderar att du ställer in hello MySQL-databas miljö tooDeadline för stabilitet.</span><span class="sxs-lookup"><span data-stu-id="f257a-165">We recommend that you set hello MySQL database environment tooDeadline for stability.</span></span> <span data-ttu-id="f257a-166">Om det är mycket sekventiella i/o, kan CFQ minska diskens i/o-prestanda.</span><span class="sxs-lookup"><span data-stu-id="f257a-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="f257a-167">NOOP eller tidsgräns kan få bättre prestanda än hello standard scheduler för SSD och annan utrustning.</span><span class="sxs-lookup"><span data-stu-id="f257a-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than hello default scheduler.</span></span>   

<span data-ttu-id="f257a-168">Tidigare toohello kernel 2.5 hello standard i/o schemaläggning algoritmen är tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="f257a-168">Prior toohello kernel 2.5, hello default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="f257a-169">Från och med hello kernel 2.6.18 blev CFQ hello standard-i/o: schemaläggning-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="f257a-169">Starting with hello kernel 2.6.18, CFQ became hello default I/O scheduling algorithm.</span></span>  <span data-ttu-id="f257a-170">Du kan ange den här inställningen när datorn startas i kernel eller ändra den här inställningen dynamiskt när hello system körs.</span><span class="sxs-lookup"><span data-stu-id="f257a-170">You can specify this setting at kernel boot time or dynamically modify this setting when hello system is running.</span></span>  

<span data-ttu-id="f257a-171">hello som följande exempel visar hur toocheck och ange hello standard scheduler toohello NOOP algoritmen i hello Debian distribution familj.</span><span class="sxs-lookup"><span data-stu-id="f257a-171">hello following example demonstrates how toocheck and set hello default scheduler toohello NOOP algorithm in hello Debian distribution family.</span></span>  

### <a name="view-hello-current-io-scheduler"></a><span data-ttu-id="f257a-172">Visa hello aktuella i/o-Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="f257a-172">View hello current I/O scheduler</span></span>
<span data-ttu-id="f257a-173">tooview hello scheduler kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-173">tooview hello scheduler run hello following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="f257a-174">Du kommer se följande utdata som visar hello aktuella Schemaläggaren:</span><span class="sxs-lookup"><span data-stu-id="f257a-174">You will see following output, which indicates hello current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a><span data-ttu-id="f257a-175">Ändra hello aktuell enhet (/ dev/sda) av schemaläggning hello i/o-algoritm</span><span class="sxs-lookup"><span data-stu-id="f257a-175">Change hello current device (/dev/sda) of hello I/O scheduling algorithm</span></span>
<span data-ttu-id="f257a-176">Kör följande kommandon toochange hello aktuell enhet hello:</span><span class="sxs-lookup"><span data-stu-id="f257a-176">Run hello following commands toochange hello current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="f257a-177">Ange detta för /dev/sda enbart är inte användbar.</span><span class="sxs-lookup"><span data-stu-id="f257a-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="f257a-178">Det måste anges på alla datadiskar där hello databasen finns.</span><span class="sxs-lookup"><span data-stu-id="f257a-178">It must be set on all data disks where hello database resides.</span></span>  
>
>

<span data-ttu-id="f257a-179">Du bör se hello följande utdata, vilket visar att grub.cfg har återskapats och den hello standard Schemaläggaren har uppdaterats tooNOOP:</span><span class="sxs-lookup"><span data-stu-id="f257a-179">You should see hello following output, indicating that grub.cfg has been rebuilt successfully and that hello default scheduler has been updated tooNOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="f257a-180">För hello Red Hat distribution familj, behöver du bara hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-180">For hello Red Hat distribution family, you need only hello following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="f257a-181">Konfigurera systeminställningar filen operations</span><span class="sxs-lookup"><span data-stu-id="f257a-181">Configure system file operations settings</span></span>
<span data-ttu-id="f257a-182">Ett bra tips är toodisable hello *atime* loggningsfunktionen i hello-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="f257a-182">One best practice is toodisable hello *atime* logging feature on hello file system.</span></span> <span data-ttu-id="f257a-183">Atime är hello senaste åtkomsttid för fil.</span><span class="sxs-lookup"><span data-stu-id="f257a-183">Atime is hello last file access time.</span></span> <span data-ttu-id="f257a-184">När en fil används hello hello poster tidsstämpel i hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="f257a-184">Whenever a file is accessed, hello file system records hello timestamp in hello log.</span></span> <span data-ttu-id="f257a-185">Men används den här informationen sällan.</span><span class="sxs-lookup"><span data-stu-id="f257a-185">However, this information is rarely used.</span></span> <span data-ttu-id="f257a-186">Du kan inaktivera den om du inte behöver den, vilket minskar övergripande disktid åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f257a-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="f257a-187">toodisable atime loggning av du behöver toomodify hello filen system configuration filen/etc / fstab och lägga till hello **noatime** alternativet.</span><span class="sxs-lookup"><span data-stu-id="f257a-187">toodisable atime logging, you need toomodify hello file system configuration file /etc/ fstab and add hello **noatime** option.</span></span>  

<span data-ttu-id="f257a-188">Till exempel redigera hello vim /etc/fstab filen till hello noatime som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="f257a-188">For example, edit hello vim /etc/fstab file, adding hello noatime as shown in hello following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="f257a-189">Sedan återansluta hello filsystem med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f257a-189">Then, remount hello file system with hello following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="f257a-190">Testa hello ändrade resultat.</span><span class="sxs-lookup"><span data-stu-id="f257a-190">Test hello modified result.</span></span> <span data-ttu-id="f257a-191">När du ändrar hello testfil uppdateras hello åtkomsttid inte.</span><span class="sxs-lookup"><span data-stu-id="f257a-191">When you modify hello test file, hello access time is not updated.</span></span> <span data-ttu-id="f257a-192">Hej följande exempel visar vilka hello koden ser ut före och efter ändring.</span><span class="sxs-lookup"><span data-stu-id="f257a-192">hello following examples show what hello code looks like before and after modification.</span></span>

<span data-ttu-id="f257a-193">Innan:</span><span class="sxs-lookup"><span data-stu-id="f257a-193">Before:</span></span>        

![Code innan åtkomst ändringar][5]

<span data-ttu-id="f257a-195">Efter:</span><span class="sxs-lookup"><span data-stu-id="f257a-195">After:</span></span>

![Kod efter åtkomst ändring][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="f257a-197">Öka hello maximala tillåtna antalet system handtag för hög samtidighet</span><span class="sxs-lookup"><span data-stu-id="f257a-197">Increase hello maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="f257a-198">MySQL är en hög concurrency-databas.</span><span class="sxs-lookup"><span data-stu-id="f257a-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="f257a-199">hello standardantalet samtidiga referenser är 1024 för Linux som räcker inte alltid.</span><span class="sxs-lookup"><span data-stu-id="f257a-199">hello default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="f257a-200">Använd följande steg tooincrease hello maximala samtidiga i handtag hello system toosupport hög samtidighet på MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="f257a-200">Use hello following steps tooincrease hello maximum concurrent handles of hello system toosupport high concurrency of MySQL.</span></span>

### <a name="modify-hello-limitsconf-file"></a><span data-ttu-id="f257a-201">Ändra hello limits.conf filen</span><span class="sxs-lookup"><span data-stu-id="f257a-201">Modify hello limits.conf file</span></span>
<span data-ttu-id="f257a-202">tooincrease hello högsta tillåtna samtidiga handtag, Lägg till hello följande fyra rader i hello /etc/security/limits.conf fil.</span><span class="sxs-lookup"><span data-stu-id="f257a-202">tooincrease hello maximum allowed concurrent handles, add hello following four lines in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="f257a-203">Observera att 65536 hello maximalt antal hello-system kan stödja.</span><span class="sxs-lookup"><span data-stu-id="f257a-203">Note that 65536 is hello maximum number that hello system can support.</span></span>   

    * <span data-ttu-id="f257a-204">mjuka nofile 65536</span><span class="sxs-lookup"><span data-stu-id="f257a-204">soft nofile 65536</span></span>
    * <span data-ttu-id="f257a-205">hårda nofile 65536</span><span class="sxs-lookup"><span data-stu-id="f257a-205">hard nofile 65536</span></span>
    * <span data-ttu-id="f257a-206">mjuka nproc 65536</span><span class="sxs-lookup"><span data-stu-id="f257a-206">soft nproc 65536</span></span>
    * <span data-ttu-id="f257a-207">hårda nproc 65536</span><span class="sxs-lookup"><span data-stu-id="f257a-207">hard nproc 65536</span></span>

### <a name="update-hello-system-for-hello-new-limits"></a><span data-ttu-id="f257a-208">Uppdatera hello system för hello nya gränser</span><span class="sxs-lookup"><span data-stu-id="f257a-208">Update hello system for hello new limits</span></span>
<span data-ttu-id="f257a-209">tooupdate hello system kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f257a-209">tooupdate hello system, run hello following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a><span data-ttu-id="f257a-210">Se till att hello gränser uppdateras vid start</span><span class="sxs-lookup"><span data-stu-id="f257a-210">Ensure that hello limits are updated at boot time</span></span>
<span data-ttu-id="f257a-211">Placera hello följande startkommandon i hello /etc/rc.local filen så att den börjar gälla när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="f257a-211">Put hello following startup commands in hello /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="f257a-212">MySQL-databas optimering</span><span class="sxs-lookup"><span data-stu-id="f257a-212">MySQL database optimization</span></span>
<span data-ttu-id="f257a-213">tooconfigure MySQL på Azure som du kan använda hello samma prestandajustering strategi som du använder på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="f257a-213">tooconfigure MySQL on Azure, you can use hello same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="f257a-214">hello huvudsakliga i/o optimering reglerna är:</span><span class="sxs-lookup"><span data-stu-id="f257a-214">hello main I/O optimization rules are:</span></span>   

* <span data-ttu-id="f257a-215">Öka hello cachestorlek.</span><span class="sxs-lookup"><span data-stu-id="f257a-215">Increase hello cache size.</span></span>
* <span data-ttu-id="f257a-216">Minska i/o-svarstid.</span><span class="sxs-lookup"><span data-stu-id="f257a-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="f257a-217">toooptimize MySQL-serverinställningar, kan du uppdatera hello my.cnf fil som är hello standardkonfigurationsfilen för både servern och klientdatorerna.</span><span class="sxs-lookup"><span data-stu-id="f257a-217">toooptimize MySQL server settings, you can update hello my.cnf file, which is hello default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="f257a-218">hello är följande konfigurationsobjekt hello viktigaste faktorerna som påverkar prestanda MySQL:</span><span class="sxs-lookup"><span data-stu-id="f257a-218">hello following configuration items are hello main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="f257a-219">**innodb_buffer_pool_size**: hello buffertpool innehåller buffrade data och hello index.</span><span class="sxs-lookup"><span data-stu-id="f257a-219">**innodb_buffer_pool_size**: hello buffer pool contains buffered data and hello index.</span></span> <span data-ttu-id="f257a-220">Det här värdet vanligtvis too70 procent av fysiskt minne.</span><span class="sxs-lookup"><span data-stu-id="f257a-220">This is usually set too70 percent of physical memory.</span></span>
* <span data-ttu-id="f257a-221">**innodb_log_file_size**: Detta är hello gör om loggfilens storlek.</span><span class="sxs-lookup"><span data-stu-id="f257a-221">**innodb_log_file_size**: This is hello redo log size.</span></span> <span data-ttu-id="f257a-222">Du kan använda gör om loggar tooensure att skrivåtgärder är snabb, tillförlitlig och återställas efter en krasch.</span><span class="sxs-lookup"><span data-stu-id="f257a-222">You use redo logs tooensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="f257a-223">Detta är inställt too512 MB, vilket ger tillräckligt med utrymme för att logga skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f257a-223">This is set too512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="f257a-224">**max_connections**: ibland program Stäng inte anslutningar korrekt.</span><span class="sxs-lookup"><span data-stu-id="f257a-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="f257a-225">Ett större värde ger hello server mer tid toorecycle inaktiv anslutningar.</span><span class="sxs-lookup"><span data-stu-id="f257a-225">A larger value will give hello server more time toorecycle idled connections.</span></span> <span data-ttu-id="f257a-226">hello maximalt antal anslutningar är 10 000 men hello rekommenderade maximala antalet är 5 000.</span><span class="sxs-lookup"><span data-stu-id="f257a-226">hello maximum number of connections is 10,000, but hello recommended maximum is 5,000.</span></span>
* <span data-ttu-id="f257a-227">**Innodb_file_per_table**: den här inställningen aktiverar eller inaktiverar hello möjligheten för InnoDB toostore tabeller i separata filer.</span><span class="sxs-lookup"><span data-stu-id="f257a-227">**Innodb_file_per_table**: This setting enables or disables hello ability of InnoDB toostore tables in separate files.</span></span> <span data-ttu-id="f257a-228">Aktivera hello alternativet tooensure att flera avancerade administration-åtgärder kan användas effektivt.</span><span class="sxs-lookup"><span data-stu-id="f257a-228">Turn on hello option tooensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="f257a-229">Ur en prestanda, den påskynda hello tabell utrymme överföring och optimera hello skräp hantering prestanda.</span><span class="sxs-lookup"><span data-stu-id="f257a-229">From a performance point of view, it can speed up hello table space transmission and optimize hello debris management performance.</span></span> <span data-ttu-id="f257a-230">hello bör inställning för det här alternativet är Aktiverat.</span><span class="sxs-lookup"><span data-stu-id="f257a-230">hello recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="f257a-231">Från MySQL 5.6 är hello standardinställningen ON, så ingen åtgärd krävs.</span><span class="sxs-lookup"><span data-stu-id="f257a-231">From MySQL 5.6, hello default setting is ON, so no action is required.</span></span> <span data-ttu-id="f257a-232">För tidigare versioner är hello standardinställningen AVSTÄNGD.</span><span class="sxs-lookup"><span data-stu-id="f257a-232">For earlier versions, hello default setting is OFF.</span></span> <span data-ttu-id="f257a-233">hello inställningen ska ändras innan data har lästs in, eftersom endast nya tabeller påverkas.</span><span class="sxs-lookup"><span data-stu-id="f257a-233">hello setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="f257a-234">**innodb_flush_log_at_trx_commit**: hello standardvärdet är 1, med hello scope too0 ~ 2.</span><span class="sxs-lookup"><span data-stu-id="f257a-234">**innodb_flush_log_at_trx_commit**: hello default value is 1, with hello scope set too0~2.</span></span> <span data-ttu-id="f257a-235">hello standardvärdet är hello alternativ som passar för fristående MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f257a-235">hello default value is hello most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="f257a-236">hello inställningen av 2 aktiverar hello de flesta dataintegritet och lämpar sig för Master i MySQL-kluster.</span><span class="sxs-lookup"><span data-stu-id="f257a-236">hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="f257a-237">hello inställningen 0 dataförluster som kan påverka tillförlitlighet (i vissa fall med bättre prestanda) och lämpar sig för underordnade i MySQL-kluster.</span><span class="sxs-lookup"><span data-stu-id="f257a-237">hello setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="f257a-238">**Innodb_log_buffer_size**: hello loggen buffert kan transaktioner toorun utan tooflush hello loggen toodisk innan hello transaktioner genomförande.</span><span class="sxs-lookup"><span data-stu-id="f257a-238">**Innodb_log_buffer_size**: hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit.</span></span> <span data-ttu-id="f257a-239">Om det finns stora binära objekt eller textfältet, hello cache används snabbt och ofta disk-i/o ska utlösas.</span><span class="sxs-lookup"><span data-stu-id="f257a-239">However, if there is large binary object or text field, hello cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="f257a-240">Det är bättre öka hello buffertstorlek om Innodb_log_waits tillstånd variabeln inte är 0.</span><span class="sxs-lookup"><span data-stu-id="f257a-240">It is better increase hello buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="f257a-241">**query_cache_size**: hello bästa alternativet är toodisable från hello början.</span><span class="sxs-lookup"><span data-stu-id="f257a-241">**query_cache_size**: hello best option is toodisable it from hello outset.</span></span> <span data-ttu-id="f257a-242">Ange query_cache_size too0 (detta är standardinställningen för hello i MySQL 5.6) och använda andra metoder toospeed frågor.</span><span class="sxs-lookup"><span data-stu-id="f257a-242">Set query_cache_size too0 (this is hello default setting in MySQL 5.6) and use other methods toospeed up queries.</span></span>  

<span data-ttu-id="f257a-243">Se [bilaga D](#AppendixD) för en jämförelse av prestanda före och efter hello optimering.</span><span class="sxs-lookup"><span data-stu-id="f257a-243">See [Appendix D](#AppendixD) for a comparison of performance before and after hello optimization.</span></span>

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a><span data-ttu-id="f257a-244">Aktivera hello MySQL långsam frågeloggen för att analysera hello flaskhalsar</span><span class="sxs-lookup"><span data-stu-id="f257a-244">Turn on hello MySQL slow query log for analyzing hello performance bottleneck</span></span>
<span data-ttu-id="f257a-245">hello MySQL långsam fråga loggen kan hjälpa dig identifiera hello långsam frågor för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f257a-245">hello MySQL slow query log can help you identify hello slow queries for MySQL.</span></span> <span data-ttu-id="f257a-246">Du kan använda MySQL-verktyg som när du har aktiverat hello MySQL långsam frågeloggen **mysqldumpslow** tooidentify hello flaskhalsar.</span><span class="sxs-lookup"><span data-stu-id="f257a-246">After enabling hello MySQL slow query log, you can use MySQL tools like **mysqldumpslow** tooidentify hello performance bottleneck.</span></span>  

<span data-ttu-id="f257a-247">Som standard är detta inte aktiverat.</span><span class="sxs-lookup"><span data-stu-id="f257a-247">By default, this is not enabled.</span></span> <span data-ttu-id="f257a-248">Aktivera hello långsam frågeloggen kan använda vissa processorresurser.</span><span class="sxs-lookup"><span data-stu-id="f257a-248">Turning on hello slow query log might consume some CPU resources.</span></span> <span data-ttu-id="f257a-249">Vi rekommenderar att du aktiverar detta tillfälligt för felsökning av flaskhalsar.</span><span class="sxs-lookup"><span data-stu-id="f257a-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="f257a-250">tooturn hello långsam fråga logga in:</span><span class="sxs-lookup"><span data-stu-id="f257a-250">tooturn on hello slow query log:</span></span>

1. <span data-ttu-id="f257a-251">Ändra hello my.cnf-filen genom att lägga till hello efter rader toohello:</span><span class="sxs-lookup"><span data-stu-id="f257a-251">Modify hello my.cnf file by adding hello following lines toohello end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="f257a-252">Starta om hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="f257a-252">Restart hello MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="f257a-253">Kontrollera om hello inställningen börjar gälla med hjälp av hello **visa** kommando.</span><span class="sxs-lookup"><span data-stu-id="f257a-253">Check whether hello setting is taking effect by using hello **show** command.</span></span>

![Långsamma frågeloggen ON][7]   

![Långsamma frågeloggen resultat][8]

<span data-ttu-id="f257a-256">I det här exemplet kan du se hello långsam fråga funktionen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="f257a-256">In this example, you can see that hello slow query feature has been turned on.</span></span> <span data-ttu-id="f257a-257">Du kan sedan använda hello **mysqldumpslow** verktyget toodetermine prestandaflaskhalsar och optimera prestanda, till exempel lägga till index.</span><span class="sxs-lookup"><span data-stu-id="f257a-257">You can then use hello **mysqldumpslow** tool toodetermine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="f257a-258">Tilläggen</span><span class="sxs-lookup"><span data-stu-id="f257a-258">Appendices</span></span>
<span data-ttu-id="f257a-259">hello följande är exempel test prestandadata framställs i labbmiljö riktade.</span><span class="sxs-lookup"><span data-stu-id="f257a-259">hello following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="f257a-260">De ger grundläggande på hello prestanda data trend med olika metoder för prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="f257a-260">They provide general background on hello performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="f257a-261">hello resultat kan variera under olika versioner av miljö eller produkt.</span><span class="sxs-lookup"><span data-stu-id="f257a-261">hello results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="f257a-262"><a name="AppendixA"></a>Bilaga A</span><span class="sxs-lookup"><span data-stu-id="f257a-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="f257a-263">**Diskprestanda (IOPS) med olika RAID-nivåer**</span><span class="sxs-lookup"><span data-stu-id="f257a-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Disk-IOPS med olika RAID-nivåer][9]

<span data-ttu-id="f257a-265">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="f257a-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="f257a-266">hello arbetsbelastningen för det här testet använder 64 trådar, försök tooreach hello övre gränsen för RAID.</span><span class="sxs-lookup"><span data-stu-id="f257a-266">hello workload of this test uses 64 threads, trying tooreach hello upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="f257a-267"><a name="AppendixB"></a>Bilaga B</span><span class="sxs-lookup"><span data-stu-id="f257a-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="f257a-268">**MySQL prestanda (dataflöde) jämförelse med olika RAID-nivåer** </span><span class="sxs-lookup"><span data-stu-id="f257a-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="f257a-269">(XFS filsystem)</span><span class="sxs-lookup"><span data-stu-id="f257a-269">(XFS file system)</span></span>

![MySQL prestanda jämförelse med olika RAID-nivåer][10]  
![MySQL prestanda jämförelse med olika RAID-nivåer][11]

<span data-ttu-id="f257a-272">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="f257a-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="f257a-273">**MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer**</span><span class="sxs-lookup"><span data-stu-id="f257a-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="f257a-274">![MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer][12]</span><span class="sxs-lookup"><span data-stu-id="f257a-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="f257a-275">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="f257a-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="f257a-276"><a name="AppendixC"></a>Bilaga C</span><span class="sxs-lookup"><span data-stu-id="f257a-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="f257a-277">**Jämförelse av disk prestanda (IOPS) för olika segment storlekar**</span><span class="sxs-lookup"><span data-stu-id="f257a-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="f257a-278">(XFS filsystem)</span><span class="sxs-lookup"><span data-stu-id="f257a-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="f257a-279">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="f257a-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="f257a-280">hello filstorlekar som används för detta test är 30 och 1 GB respektive, med RAID 0 (4 diskar) XFS filsystem.</span><span class="sxs-lookup"><span data-stu-id="f257a-280">hello file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="f257a-281"><a name="AppendixD"></a>Bilaga D</span><span class="sxs-lookup"><span data-stu-id="f257a-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="f257a-282">**MySQL prestanda (dataflöde) jämförelse före och efter optimering**</span><span class="sxs-lookup"><span data-stu-id="f257a-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="f257a-283">(XFS File System)</span><span class="sxs-lookup"><span data-stu-id="f257a-283">(XFS File System)</span></span>

![MySQL prestanda (dataflöde) jämförelse före och efter optimering][14]

<span data-ttu-id="f257a-285">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="f257a-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="f257a-286">**hello konfigurationsinställning för standard och optimering är följande:**</span><span class="sxs-lookup"><span data-stu-id="f257a-286">**hello configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="f257a-287">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f257a-287">Parameters</span></span> | <span data-ttu-id="f257a-288">Standard</span><span class="sxs-lookup"><span data-stu-id="f257a-288">Default</span></span> | <span data-ttu-id="f257a-289">Optimering</span><span class="sxs-lookup"><span data-stu-id="f257a-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f257a-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="f257a-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="f257a-291">Ingen</span><span class="sxs-lookup"><span data-stu-id="f257a-291">None</span></span> |<span data-ttu-id="f257a-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="f257a-292">7 GB</span></span> |
| <span data-ttu-id="f257a-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="f257a-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="f257a-294">5 MB</span><span class="sxs-lookup"><span data-stu-id="f257a-294">5 MB</span></span> |<span data-ttu-id="f257a-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="f257a-295">512 MB</span></span> |
| <span data-ttu-id="f257a-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="f257a-296">**max_connections**</span></span> |<span data-ttu-id="f257a-297">100</span><span class="sxs-lookup"><span data-stu-id="f257a-297">100</span></span> |<span data-ttu-id="f257a-298">5000</span><span class="sxs-lookup"><span data-stu-id="f257a-298">5000</span></span> |
| <span data-ttu-id="f257a-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="f257a-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="f257a-300">0</span><span class="sxs-lookup"><span data-stu-id="f257a-300">0</span></span> |<span data-ttu-id="f257a-301">1</span><span class="sxs-lookup"><span data-stu-id="f257a-301">1</span></span> |
| <span data-ttu-id="f257a-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="f257a-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="f257a-303">1</span><span class="sxs-lookup"><span data-stu-id="f257a-303">1</span></span> |<span data-ttu-id="f257a-304">2</span><span class="sxs-lookup"><span data-stu-id="f257a-304">2</span></span> |
| <span data-ttu-id="f257a-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="f257a-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="f257a-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="f257a-306">8 MB</span></span> |<span data-ttu-id="f257a-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="f257a-307">128 MB</span></span> |
| <span data-ttu-id="f257a-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="f257a-308">**query_cache_size**</span></span> |<span data-ttu-id="f257a-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="f257a-309">16 MB</span></span> |<span data-ttu-id="f257a-310">0</span><span class="sxs-lookup"><span data-stu-id="f257a-310">0</span></span> |

<span data-ttu-id="f257a-311">Mer detaljerad [optimering konfigurationsparametrar](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), se toohello [MySQL officiella instruktioner](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="f257a-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer toohello [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="f257a-312">**Testmiljö**</span><span class="sxs-lookup"><span data-stu-id="f257a-312">**Test environment**</span></span>  

| <span data-ttu-id="f257a-313">Maskinvara</span><span class="sxs-lookup"><span data-stu-id="f257a-313">Hardware</span></span> | <span data-ttu-id="f257a-314">Information</span><span class="sxs-lookup"><span data-stu-id="f257a-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="f257a-315">Processor</span><span class="sxs-lookup"><span data-stu-id="f257a-315">CPU</span></span> |<span data-ttu-id="f257a-316">AMD Opteron (TM)-Processor 4171 HAN / 4 kärnor</span><span class="sxs-lookup"><span data-stu-id="f257a-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="f257a-317">Minne</span><span class="sxs-lookup"><span data-stu-id="f257a-317">Memory</span></span> |<span data-ttu-id="f257a-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="f257a-318">14 GB</span></span> |
| <span data-ttu-id="f257a-319">Disk</span><span class="sxs-lookup"><span data-stu-id="f257a-319">Disk</span></span> |<span data-ttu-id="f257a-320">10 GB-disk</span><span class="sxs-lookup"><span data-stu-id="f257a-320">10 GB/disk</span></span> |
| <span data-ttu-id="f257a-321">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="f257a-321">OS</span></span> |<span data-ttu-id="f257a-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="f257a-322">Ubuntu 14.04.1 LTS</span></span> |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

