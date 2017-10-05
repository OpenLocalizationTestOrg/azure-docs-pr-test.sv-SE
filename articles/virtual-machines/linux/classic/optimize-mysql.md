---
title: "Optimera prestanda för MySQL på Linux | Microsoft Docs"
description: "Lär dig hur du optimerar MySQL som körs på en Azure-dator (VM) kör Linux."
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
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="cd12c-103">Optimera MySQL prestanda på virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="cd12c-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="cd12c-104">Det finns många faktorer som påverkar prestanda MySQL på Azure, både i val av virtuell maskinvara och konfiguration av programvara.</span><span class="sxs-lookup"><span data-stu-id="cd12c-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="cd12c-105">Den här artikeln fokuserar på optimera prestanda via lagring, system och konfigurationer för databasen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd12c-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk.</span><span class="sxs-lookup"><span data-stu-id="cd12c-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="cd12c-107">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="cd12c-108">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="cd12c-109">Information om Linux VM optimeringar med Resource Manager-modellen finns [optimera din Linux VM på Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cd12c-109">For information about Linux VM optimizations with the Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="cd12c-110">Använda RAID på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="cd12c-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="cd12c-111">Lagring är avgörande som påverkar databasprestanda i miljöer i molnet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-111">Storage is the key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="cd12c-112">Jämfört med en enskild disk ger RAID snabbare åtkomst via samtidighet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-112">Compared to a single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="cd12c-113">Mer information finns i [Standard RAID-nivåer](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="cd12c-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="cd12c-114">I/o-genomflöde och svarstid för i/o i Azure kan förbättras genom RAID.</span><span class="sxs-lookup"><span data-stu-id="cd12c-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="cd12c-115">Våra tester visar att i/o-genomflöde kan vara dubblerad och i/o-svarstid kan reduceras med halvt i genomsnitt när antalet RAID-diskar fördubblas (från två till fyra, fyra till åtta osv.).</span><span class="sxs-lookup"><span data-stu-id="cd12c-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when the number of RAID disks is doubled (from two to four, four to eight, etc.).</span></span> <span data-ttu-id="cd12c-116">Se [bilaga A](#AppendixA) mer information.</span><span class="sxs-lookup"><span data-stu-id="cd12c-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="cd12c-117">Utöver disk-i/o förbättras MySQL prestanda om du ökar RAID-nivå.</span><span class="sxs-lookup"><span data-stu-id="cd12c-117">In addition to disk I/O, MySQL performance improves when you increase the RAID level.</span></span>  <span data-ttu-id="cd12c-118">Se [bilaga B](#AppendixB) mer information.</span><span class="sxs-lookup"><span data-stu-id="cd12c-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="cd12c-119">Du kanske också vill överväga segmentstorleken.</span><span class="sxs-lookup"><span data-stu-id="cd12c-119">You might also want to consider the chunk size.</span></span> <span data-ttu-id="cd12c-120">I allmänhet när du har en större segmentstorlek kan hämta du lägre omkostnader, särskilt för stora skrivningar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="cd12c-121">Om segmentstorleken är för stor, kan det dock lägga till ytterligare kostnader som hindrar dig från att dra nytta av RAID.</span><span class="sxs-lookup"><span data-stu-id="cd12c-121">However, when the chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="cd12c-122">Den aktuella standardstorleken är 512 KB som visat sig vara optimala för mest allmänna produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="cd12c-122">The current default size is 512 KB, which is proven to be optimal for most general production environments.</span></span> <span data-ttu-id="cd12c-123">Se [bilaga C](#AppendixC) mer information.</span><span class="sxs-lookup"><span data-stu-id="cd12c-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="cd12c-124">Det finns begränsningar på hur många diskar som du kan lägga till för olika virtuella typer.</span><span class="sxs-lookup"><span data-stu-id="cd12c-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="cd12c-125">Dessa värden beskrivs i [storlekar för virtuella datorer och moln för Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd12c-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="cd12c-126">Du behöver fyra bifogade datadiskar till RAID-exempel i den här artikeln, men du kan välja att ställa in RAID med färre diskar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-126">You will need four attached data disks to follow the RAID example in this article, although you can choose to set up RAID with fewer disks.</span></span>  

<span data-ttu-id="cd12c-127">Den här artikeln förutsätter att du redan har skapat en virtuell Linux-dator och ha MYSQL installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="cd12c-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="cd12c-128">Mer information om att komma igång finns i hur du installerar MySQL på Azure.</span><span class="sxs-lookup"><span data-stu-id="cd12c-128">For more information on getting started, see How to install MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="cd12c-129">Ställ in RAID på Azure</span><span class="sxs-lookup"><span data-stu-id="cd12c-129">Set up RAID on Azure</span></span>
<span data-ttu-id="cd12c-130">Följande steg visar hur du skapar RAID på Azure med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="cd12c-130">The following steps show how to create RAID on Azure by using the Azure portal.</span></span> <span data-ttu-id="cd12c-131">Du kan också ställa in RAID med hjälp av Windows PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="cd12c-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="cd12c-132">I det här exemplet ska vi konfigurera RAID 0 med fyra diskar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a><span data-ttu-id="cd12c-133">Lägg till en datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="cd12c-133">Add a data disk to your virtual machine</span></span>
<span data-ttu-id="cd12c-134">Gå till instrumentpanelen i Azure-portalen och välj den virtuella dator som du vill lägga till en datadisk.</span><span class="sxs-lookup"><span data-stu-id="cd12c-134">In the Azure portal, go to the dashboard and select the virtual machine to which you want to add a data disk.</span></span> <span data-ttu-id="cd12c-135">I det här exemplet är den virtuella datorn mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="cd12c-135">In this example, the virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="cd12c-136">Klicka på **diskar** och klicka sedan på **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="cd12c-136">Click **Disks** and then click **Attach New**.</span></span>

![Virtuella datorer lägger du till disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="cd12c-138">Skapa en ny 500 GB disk.</span><span class="sxs-lookup"><span data-stu-id="cd12c-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="cd12c-139">Se till att **värden Cache inställningar** är inställd på **ingen**.</span><span class="sxs-lookup"><span data-stu-id="cd12c-139">Make sure that **Host Cache Preference** is set to **None**.</span></span>  <span data-ttu-id="cd12c-140">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd12c-140">When you're finished, click **OK**.</span></span>

![Anslut tom disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="cd12c-142">Detta lägger till en tom disk i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cd12c-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="cd12c-143">Upprepa detta steg tre gånger så att du har fyra datadiskar för RAID.</span><span class="sxs-lookup"><span data-stu-id="cd12c-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="cd12c-144">Du kan se enheterna som har lagts till i den virtuella datorn genom att titta på kernel message-loggen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-144">You can see the added drives in the virtual machine by looking at the kernel message log.</span></span> <span data-ttu-id="cd12c-145">Till exempel om du vill se detta på Ubuntu, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-145">For example, to see this on Ubuntu, use the following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a><span data-ttu-id="cd12c-146">Skapa RAID med ytterligare diskar</span><span class="sxs-lookup"><span data-stu-id="cd12c-146">Create RAID with the additional disks</span></span>
<span data-ttu-id="cd12c-147">Följande steg beskriver hur du [konfigurera programvarubaserad RAID på Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cd12c-147">The following steps describe how to [configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="cd12c-148">Om du använder filsystemet XFS, kör du följande steg när du har skapat RAID.</span><span class="sxs-lookup"><span data-stu-id="cd12c-148">If you are using the XFS file system, execute the following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="cd12c-149">Om du vill installera XFS på Debian och Ubuntu Linux myntverket, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-149">To install XFS on Debian, Ubuntu, or Linux Mint, use the following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="cd12c-150">Om du vill installera XFS på Fedora, CentOS eller RHEL, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-150">To install XFS on Fedora, CentOS, or RHEL, use the following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="cd12c-151">Ställ in en ny lagringssökväg</span><span class="sxs-lookup"><span data-stu-id="cd12c-151">Set up a new storage path</span></span>
<span data-ttu-id="cd12c-152">Använd följande kommando för att ställa in en ny lagringssökväg som:</span><span class="sxs-lookup"><span data-stu-id="cd12c-152">Use the following command to set up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a><span data-ttu-id="cd12c-153">Kopiera den ursprungliga informationen till den nya sökvägen för lagring</span><span class="sxs-lookup"><span data-stu-id="cd12c-153">Copy the original data to the new storage path</span></span>
<span data-ttu-id="cd12c-154">Använd följande kommando för att kopiera data till den nya sökvägen för lagring:</span><span class="sxs-lookup"><span data-stu-id="cd12c-154">Use the following command to copy data to the new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a><span data-ttu-id="cd12c-155">Ändra behörigheter så att MySQL kan komma åt (läsning och skrivning) datadisken</span><span class="sxs-lookup"><span data-stu-id="cd12c-155">Modify permissions so MySQL can access (read and write) the data disk</span></span>
<span data-ttu-id="cd12c-156">Använd följande kommando för att ändra behörigheter:</span><span class="sxs-lookup"><span data-stu-id="cd12c-156">Use the following command to modify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a><span data-ttu-id="cd12c-157">Justera den schemaläggning algoritmen för disk-i/o</span><span class="sxs-lookup"><span data-stu-id="cd12c-157">Adjust the disk I/O scheduling algorithm</span></span>
<span data-ttu-id="cd12c-158">Linux implementerar fyra typer av i/o schemaläggning algoritmer:</span><span class="sxs-lookup"><span data-stu-id="cd12c-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="cd12c-159">NOOP algoritmen (åtgärden kan Nej)</span><span class="sxs-lookup"><span data-stu-id="cd12c-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="cd12c-160">Tidsgräns för algoritmen (tidsgräns)</span><span class="sxs-lookup"><span data-stu-id="cd12c-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="cd12c-161">Helt verkliga MSMQ-algoritmen (CFQ)</span><span class="sxs-lookup"><span data-stu-id="cd12c-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="cd12c-162">Budget period algoritmen (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="cd12c-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="cd12c-163">Du kan välja olika i/o-planeringsprogram under olika scenarier att optimera prestanda.</span><span class="sxs-lookup"><span data-stu-id="cd12c-163">You can select different I/O schedulers under different scenarios to optimize performance.</span></span> <span data-ttu-id="cd12c-164">I en miljö med helt direktåtkomst finns det inte en betydande skillnader mellan CFQ och tidsgräns algoritmer för prestanda.</span><span class="sxs-lookup"><span data-stu-id="cd12c-164">In a completely random access environment, there is not a significant difference between the CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="cd12c-165">Vi rekommenderar att du ställer in MySQL-databasmiljön tidsgräns för stabilitet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-165">We recommend that you set the MySQL database environment to Deadline for stability.</span></span> <span data-ttu-id="cd12c-166">Om det är mycket sekventiella i/o, kan CFQ minska diskens i/o-prestanda.</span><span class="sxs-lookup"><span data-stu-id="cd12c-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="cd12c-167">NOOP eller tidsgräns kan få bättre prestanda än standard Schemaläggaren för SSD och annan utrustning.</span><span class="sxs-lookup"><span data-stu-id="cd12c-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than the default scheduler.</span></span>   

<span data-ttu-id="cd12c-168">Standard-i/o schemaläggning algoritmen är innan kernel 2.5 tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="cd12c-168">Prior to the kernel 2.5, the default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="cd12c-169">Från och med kernel 2.6.18 blev CFQ standard-i/o schemaläggning algoritmen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-169">Starting with the kernel 2.6.18, CFQ became the default I/O scheduling algorithm.</span></span>  <span data-ttu-id="cd12c-170">Du kan ange den här inställningen när datorn startas i kernel eller ändra den här inställningen dynamiskt när systemet körs.</span><span class="sxs-lookup"><span data-stu-id="cd12c-170">You can specify this setting at kernel boot time or dynamically modify this setting when the system is running.</span></span>  

<span data-ttu-id="cd12c-171">Exemplet nedan visar hur du kontrollerar och ange standard Schemaläggaren till algoritmen NOOP i familjen Debian distribution.</span><span class="sxs-lookup"><span data-stu-id="cd12c-171">The following example demonstrates how to check and set the default scheduler to the NOOP algorithm in the Debian distribution family.</span></span>  

### <a name="view-the-current-io-scheduler"></a><span data-ttu-id="cd12c-172">Visa aktuella i/o-Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="cd12c-172">View the current I/O scheduler</span></span>
<span data-ttu-id="cd12c-173">Visa scheduler kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-173">To view the scheduler run the following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="cd12c-174">Visas följande utdata som visar aktuella Schemaläggaren:</span><span class="sxs-lookup"><span data-stu-id="cd12c-174">You will see following output, which indicates the current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a><span data-ttu-id="cd12c-175">Ändra den aktuella enheten (/ dev/sda) schemaläggning i/o-algoritmen</span><span class="sxs-lookup"><span data-stu-id="cd12c-175">Change the current device (/dev/sda) of the I/O scheduling algorithm</span></span>
<span data-ttu-id="cd12c-176">Kör följande kommandon för att ändra den aktuella enheten:</span><span class="sxs-lookup"><span data-stu-id="cd12c-176">Run the following commands to change the current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="cd12c-177">Ange detta för /dev/sda enbart är inte användbar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="cd12c-178">Det måste anges på alla datadiskar där databasen finns.</span><span class="sxs-lookup"><span data-stu-id="cd12c-178">It must be set on all data disks where the database resides.</span></span>  
>
>

<span data-ttu-id="cd12c-179">Du bör se följande utdata, som anger att grub.cfg har återskapats och som standard Schemaläggaren har uppdaterats till NOOP:</span><span class="sxs-lookup"><span data-stu-id="cd12c-179">You should see the following output, indicating that grub.cfg has been rebuilt successfully and that the default scheduler has been updated to NOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="cd12c-180">Red Hat distribution-familjen behöver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-180">For the Red Hat distribution family, you need only the following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="cd12c-181">Konfigurera systeminställningar filen operations</span><span class="sxs-lookup"><span data-stu-id="cd12c-181">Configure system file operations settings</span></span>
<span data-ttu-id="cd12c-182">Ett bra tips är att inaktivera den *atime* loggningsfunktionen i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-182">One best practice is to disable the *atime* logging feature on the file system.</span></span> <span data-ttu-id="cd12c-183">Atime är den senaste åtkomsttid för fil.</span><span class="sxs-lookup"><span data-stu-id="cd12c-183">Atime is the last file access time.</span></span> <span data-ttu-id="cd12c-184">När en fil används registrerar filsystemet i loggen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-184">Whenever a file is accessed, the file system records the timestamp in the log.</span></span> <span data-ttu-id="cd12c-185">Men används den här informationen sällan.</span><span class="sxs-lookup"><span data-stu-id="cd12c-185">However, this information is rarely used.</span></span> <span data-ttu-id="cd12c-186">Du kan inaktivera den om du inte behöver den, vilket minskar övergripande disktid åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cd12c-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="cd12c-187">Om du vill inaktivera atime loggning måste du ändra filen system configuration filen/etc / fstab och lägga till den **noatime** alternativet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-187">To disable atime logging, you need to modify the file system configuration file /etc/ fstab and add the **noatime** option.</span></span>  

<span data-ttu-id="cd12c-188">Till exempel redigera vim /etc/fstab filen, lägga till noatime som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="cd12c-188">For example, edit the vim /etc/fstab file, adding the noatime as shown in the following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="cd12c-189">Sedan återansluta filsystem med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cd12c-189">Then, remount the file system with the following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="cd12c-190">Testa ändrade resultatet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-190">Test the modified result.</span></span> <span data-ttu-id="cd12c-191">När du ändrar Testfilen uppdateras inte den åtkomst-tid.</span><span class="sxs-lookup"><span data-stu-id="cd12c-191">When you modify the test file, the access time is not updated.</span></span> <span data-ttu-id="cd12c-192">Följande exempel visar hur koden ser ut före och efter ändring.</span><span class="sxs-lookup"><span data-stu-id="cd12c-192">The following examples show what the code looks like before and after modification.</span></span>

<span data-ttu-id="cd12c-193">Innan:</span><span class="sxs-lookup"><span data-stu-id="cd12c-193">Before:</span></span>        

![Code innan åtkomst ändringar][5]

<span data-ttu-id="cd12c-195">Efter:</span><span class="sxs-lookup"><span data-stu-id="cd12c-195">After:</span></span>

![Kod efter åtkomst ändring][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="cd12c-197">Öka det maximala antalet system handtag för hög samtidighet</span><span class="sxs-lookup"><span data-stu-id="cd12c-197">Increase the maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="cd12c-198">MySQL är en hög concurrency-databas.</span><span class="sxs-lookup"><span data-stu-id="cd12c-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="cd12c-199">Standardvärdet för antalet samtidiga handtag är 1024 för Linux som räcker inte alltid.</span><span class="sxs-lookup"><span data-stu-id="cd12c-199">The default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="cd12c-200">Använd följande steg för att öka i systemet för att stödja hög samtidighet på MySQL maximala samtidiga handtag.</span><span class="sxs-lookup"><span data-stu-id="cd12c-200">Use the following steps to increase the maximum concurrent handles of the system to support high concurrency of MySQL.</span></span>

### <a name="modify-the-limitsconf-file"></a><span data-ttu-id="cd12c-201">Ändra filen limits.conf</span><span class="sxs-lookup"><span data-stu-id="cd12c-201">Modify the limits.conf file</span></span>
<span data-ttu-id="cd12c-202">Lägg till följande fyra rader i filen /etc/security/limits.conf för att öka de maximala tillåtna samtidiga referenser.</span><span class="sxs-lookup"><span data-stu-id="cd12c-202">To increase the maximum allowed concurrent handles, add the following four lines in the /etc/security/limits.conf file.</span></span> <span data-ttu-id="cd12c-203">Observera att 65536 är det maximala antal som har stöd för systemet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-203">Note that 65536 is the maximum number that the system can support.</span></span>   

    * <span data-ttu-id="cd12c-204">mjuka nofile 65536</span><span class="sxs-lookup"><span data-stu-id="cd12c-204">soft nofile 65536</span></span>
    * <span data-ttu-id="cd12c-205">hårda nofile 65536</span><span class="sxs-lookup"><span data-stu-id="cd12c-205">hard nofile 65536</span></span>
    * <span data-ttu-id="cd12c-206">mjuka nproc 65536</span><span class="sxs-lookup"><span data-stu-id="cd12c-206">soft nproc 65536</span></span>
    * <span data-ttu-id="cd12c-207">hårda nproc 65536</span><span class="sxs-lookup"><span data-stu-id="cd12c-207">hard nproc 65536</span></span>

### <a name="update-the-system-for-the-new-limits"></a><span data-ttu-id="cd12c-208">Uppdatera systemet för de nya gränserna</span><span class="sxs-lookup"><span data-stu-id="cd12c-208">Update the system for the new limits</span></span>
<span data-ttu-id="cd12c-209">Kör följande kommandon för att uppdatera systemet:</span><span class="sxs-lookup"><span data-stu-id="cd12c-209">To update the system, run the following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a><span data-ttu-id="cd12c-210">Se till att gränserna som har uppdaterats vid start</span><span class="sxs-lookup"><span data-stu-id="cd12c-210">Ensure that the limits are updated at boot time</span></span>
<span data-ttu-id="cd12c-211">Placera följande startkommandon i filen /etc/rc.local så börjar gälla när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="cd12c-211">Put the following startup commands in the /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="cd12c-212">MySQL-databas optimering</span><span class="sxs-lookup"><span data-stu-id="cd12c-212">MySQL database optimization</span></span>
<span data-ttu-id="cd12c-213">För att konfigurera MySQL på Azure, kan du använda samma prestandajustering strategin du använder på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="cd12c-213">To configure MySQL on Azure, you can use the same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="cd12c-214">De huvudsakliga i/o optimering reglerna är:</span><span class="sxs-lookup"><span data-stu-id="cd12c-214">The main I/O optimization rules are:</span></span>   

* <span data-ttu-id="cd12c-215">Öka cachestorleken.</span><span class="sxs-lookup"><span data-stu-id="cd12c-215">Increase the cache size.</span></span>
* <span data-ttu-id="cd12c-216">Minska i/o-svarstid.</span><span class="sxs-lookup"><span data-stu-id="cd12c-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="cd12c-217">Du kan uppdatera filen my.cnf, som är standardkonfigurationsfilen för både servern och klientdatorerna för att optimera MySQL-serverinställningar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-217">To optimize MySQL server settings, you can update the my.cnf file, which is the default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="cd12c-218">För följande konfigurationsobjekt är de viktigaste faktorerna som påverkar prestanda MySQL:</span><span class="sxs-lookup"><span data-stu-id="cd12c-218">The following configuration items are the main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="cd12c-219">**innodb_buffer_pool_size**: buffertpoolen innehåller buffrade data och index.</span><span class="sxs-lookup"><span data-stu-id="cd12c-219">**innodb_buffer_pool_size**: The buffer pool contains buffered data and the index.</span></span> <span data-ttu-id="cd12c-220">Det här värdet vanligtvis 70 procent av det fysiska minnet.</span><span class="sxs-lookup"><span data-stu-id="cd12c-220">This is usually set to 70 percent of physical memory.</span></span>
* <span data-ttu-id="cd12c-221">**innodb_log_file_size**: Detta är gör om loggfilens storlek.</span><span class="sxs-lookup"><span data-stu-id="cd12c-221">**innodb_log_file_size**: This is the redo log size.</span></span> <span data-ttu-id="cd12c-222">Du kan använda gör om-loggarna för att säkerställa att skrivåtgärder snabb, tillförlitlig och återställas efter en krasch.</span><span class="sxs-lookup"><span data-stu-id="cd12c-222">You use redo logs to ensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="cd12c-223">Detta är inställt på 512 MB, vilket ger tillräckligt med utrymme för att logga skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="cd12c-223">This is set to 512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="cd12c-224">**max_connections**: ibland program Stäng inte anslutningar korrekt.</span><span class="sxs-lookup"><span data-stu-id="cd12c-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="cd12c-225">Ett större värde får servern längre tid att återvinna inaktiv anslutningar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-225">A larger value will give the server more time to recycle idled connections.</span></span> <span data-ttu-id="cd12c-226">Det maximala antalet anslutningar är 10 000, men det rekommenderade maximala antalet är 5 000.</span><span class="sxs-lookup"><span data-stu-id="cd12c-226">The maximum number of connections is 10,000, but the recommended maximum is 5,000.</span></span>
* <span data-ttu-id="cd12c-227">**Innodb_file_per_table**: den här inställningen aktiverar eller inaktiverar InnoDB möjlighet att lagra tabeller i separata filer.</span><span class="sxs-lookup"><span data-stu-id="cd12c-227">**Innodb_file_per_table**: This setting enables or disables the ability of InnoDB to store tables in separate files.</span></span> <span data-ttu-id="cd12c-228">Aktivera alternativet så att flera avancerade administration-åtgärder kan användas effektivt.</span><span class="sxs-lookup"><span data-stu-id="cd12c-228">Turn on the option to ensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="cd12c-229">Ur en prestanda, den påskynda överföringen tabell utrymme och optimera prestanda för skräp management.</span><span class="sxs-lookup"><span data-stu-id="cd12c-229">From a performance point of view, it can speed up the table space transmission and optimize the debris management performance.</span></span> <span data-ttu-id="cd12c-230">Den rekommenderade inställningen för det här alternativet är Aktiverat.</span><span class="sxs-lookup"><span data-stu-id="cd12c-230">The recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="cd12c-231">Standardinställningen är ON, så ingen åtgärd krävs från MySQL 5.6.</span><span class="sxs-lookup"><span data-stu-id="cd12c-231">From MySQL 5.6, the default setting is ON, so no action is required.</span></span> <span data-ttu-id="cd12c-232">Standardinställningen är för tidigare versioner av.</span><span class="sxs-lookup"><span data-stu-id="cd12c-232">For earlier versions, the default setting is OFF.</span></span> <span data-ttu-id="cd12c-233">Inställningen måste ändras innan data läses eftersom endast nya tabeller påverkas.</span><span class="sxs-lookup"><span data-stu-id="cd12c-233">The setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="cd12c-234">**innodb_flush_log_at_trx_commit**: standardvärdet är 1, med den omfattning som anges som 0 ~ 2.</span><span class="sxs-lookup"><span data-stu-id="cd12c-234">**innodb_flush_log_at_trx_commit**: The default value is 1, with the scope set to 0~2.</span></span> <span data-ttu-id="cd12c-235">Standardvärdet är det lämpligaste alternativet för fristående MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="cd12c-235">The default value is the most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="cd12c-236">Inställningen av 2 kan de flesta dataintegriteten och lämpar sig för Master i MySQL-kluster.</span><span class="sxs-lookup"><span data-stu-id="cd12c-236">The setting of 2 enables the most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="cd12c-237">0 inställningen dataförluster som kan påverka tillförlitlighet (i vissa fall med bättre prestanda) och lämpar sig för underordnade i MySQL-kluster.</span><span class="sxs-lookup"><span data-stu-id="cd12c-237">The setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="cd12c-238">**Innodb_log_buffer_size**: Loggningsbufferten kan transaktioner ska köras utan att rensa loggen på disken innan du genomför transaktionerna.</span><span class="sxs-lookup"><span data-stu-id="cd12c-238">**Innodb_log_buffer_size**: The log buffer allows transactions to run without having to flush the log to disk before the transactions commit.</span></span> <span data-ttu-id="cd12c-239">Om det finns stora binära objekt eller textfältet, cachen används snabbt och ofta disk-i/o ska utlösas.</span><span class="sxs-lookup"><span data-stu-id="cd12c-239">However, if there is large binary object or text field, the cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="cd12c-240">Det är bättre öka buffertstorleken om Innodb_log_waits tillstånd variabeln inte är 0.</span><span class="sxs-lookup"><span data-stu-id="cd12c-240">It is better increase the buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="cd12c-241">**query_cache_size**: det bästa alternativet är att inaktivera redan från början.</span><span class="sxs-lookup"><span data-stu-id="cd12c-241">**query_cache_size**: The best option is to disable it from the outset.</span></span> <span data-ttu-id="cd12c-242">Query_cache_size 0 (detta är standardinställningen i MySQL 5.6) och använda andra metoder för att påskynda frågor.</span><span class="sxs-lookup"><span data-stu-id="cd12c-242">Set query_cache_size to 0 (this is the default setting in MySQL 5.6) and use other methods to speed up queries.</span></span>  

<span data-ttu-id="cd12c-243">Se [bilaga D](#AppendixD) för en jämförelse av prestanda före och efter optimeringen.</span><span class="sxs-lookup"><span data-stu-id="cd12c-243">See [Appendix D](#AppendixD) for a comparison of performance before and after the optimization.</span></span>

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a><span data-ttu-id="cd12c-244">Aktivera MySQL långsam frågeloggen för att analysera flaskhals</span><span class="sxs-lookup"><span data-stu-id="cd12c-244">Turn on the MySQL slow query log for analyzing the performance bottleneck</span></span>
<span data-ttu-id="cd12c-245">MySQL långsam fråga loggen kan hjälpa dig identifiera långsamma frågor för MySQL.</span><span class="sxs-lookup"><span data-stu-id="cd12c-245">The MySQL slow query log can help you identify the slow queries for MySQL.</span></span> <span data-ttu-id="cd12c-246">Du kan använda MySQL-verktyg som när du har aktiverat MySQL långsam frågeloggen **mysqldumpslow** att identifiera flaskhals.</span><span class="sxs-lookup"><span data-stu-id="cd12c-246">After enabling the MySQL slow query log, you can use MySQL tools like **mysqldumpslow** to identify the performance bottleneck.</span></span>  

<span data-ttu-id="cd12c-247">Som standard är detta inte aktiverat.</span><span class="sxs-lookup"><span data-stu-id="cd12c-247">By default, this is not enabled.</span></span> <span data-ttu-id="cd12c-248">Aktivera långsam frågeloggen kan använda vissa processorresurser.</span><span class="sxs-lookup"><span data-stu-id="cd12c-248">Turning on the slow query log might consume some CPU resources.</span></span> <span data-ttu-id="cd12c-249">Vi rekommenderar att du aktiverar detta tillfälligt för felsökning av flaskhalsar.</span><span class="sxs-lookup"><span data-stu-id="cd12c-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="cd12c-250">Aktivera långsam frågeloggen:</span><span class="sxs-lookup"><span data-stu-id="cd12c-250">To turn on the slow query log:</span></span>

1. <span data-ttu-id="cd12c-251">Ändra my.cnf-filen genom att lägga till följande rader i slutet:</span><span class="sxs-lookup"><span data-stu-id="cd12c-251">Modify the my.cnf file by adding the following lines to the end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="cd12c-252">Starta om MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="cd12c-252">Restart the MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="cd12c-253">Kontrollera om inställningen börjar gälla med hjälp av den **visa** kommando.</span><span class="sxs-lookup"><span data-stu-id="cd12c-253">Check whether the setting is taking effect by using the **show** command.</span></span>

![Långsamma frågeloggen ON][7]   

![Långsamma frågeloggen resultat][8]

<span data-ttu-id="cd12c-256">I det här exemplet ser du att funktionen långsam fråga har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cd12c-256">In this example, you can see that the slow query feature has been turned on.</span></span> <span data-ttu-id="cd12c-257">Du kan sedan använda den **mysqldumpslow** verktyg för att fastställa prestandaflaskhalsar och optimera prestanda, till exempel lägga till index.</span><span class="sxs-lookup"><span data-stu-id="cd12c-257">You can then use the **mysqldumpslow** tool to determine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="cd12c-258">Tilläggen</span><span class="sxs-lookup"><span data-stu-id="cd12c-258">Appendices</span></span>
<span data-ttu-id="cd12c-259">Följande är exempel test prestandadata framställs i labbmiljö riktade.</span><span class="sxs-lookup"><span data-stu-id="cd12c-259">The following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="cd12c-260">De ger grundläggande på prestanda data trend med olika metoder för prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="cd12c-260">They provide general background on the performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="cd12c-261">Resultatet kan variera under olika versioner av miljö eller produkt.</span><span class="sxs-lookup"><span data-stu-id="cd12c-261">The results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="cd12c-262"><a name="AppendixA"></a>Bilaga A</span><span class="sxs-lookup"><span data-stu-id="cd12c-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="cd12c-263">**Diskprestanda (IOPS) med olika RAID-nivåer**</span><span class="sxs-lookup"><span data-stu-id="cd12c-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Disk-IOPS med olika RAID-nivåer][9]

<span data-ttu-id="cd12c-265">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="cd12c-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="cd12c-266">Arbetsbelastningen för det här testet använder 64 trådar, försöker komma åt den övre gränsen för RAID.</span><span class="sxs-lookup"><span data-stu-id="cd12c-266">The workload of this test uses 64 threads, trying to reach the upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="cd12c-267"><a name="AppendixB"></a>Bilaga B</span><span class="sxs-lookup"><span data-stu-id="cd12c-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="cd12c-268">**MySQL prestanda (dataflöde) jämförelse med olika RAID-nivåer** </span><span class="sxs-lookup"><span data-stu-id="cd12c-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="cd12c-269">(XFS filsystem)</span><span class="sxs-lookup"><span data-stu-id="cd12c-269">(XFS file system)</span></span>

![MySQL prestanda jämförelse med olika RAID-nivåer][10]  
![MySQL prestanda jämförelse med olika RAID-nivåer][11]

<span data-ttu-id="cd12c-272">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="cd12c-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="cd12c-273">**MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer**</span><span class="sxs-lookup"><span data-stu-id="cd12c-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="cd12c-274">![MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer][12]</span><span class="sxs-lookup"><span data-stu-id="cd12c-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="cd12c-275">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="cd12c-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="cd12c-276"><a name="AppendixC"></a>Bilaga C</span><span class="sxs-lookup"><span data-stu-id="cd12c-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="cd12c-277">**Jämförelse av disk prestanda (IOPS) för olika segment storlekar**</span><span class="sxs-lookup"><span data-stu-id="cd12c-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="cd12c-278">(XFS filsystem)</span><span class="sxs-lookup"><span data-stu-id="cd12c-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="cd12c-279">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="cd12c-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="cd12c-280">Filstorlekarna som används för detta test är 30 och 1 GB respektive, med RAID 0 (4 diskar) XFS filsystem.</span><span class="sxs-lookup"><span data-stu-id="cd12c-280">The file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="cd12c-281"><a name="AppendixD"></a>Bilaga D</span><span class="sxs-lookup"><span data-stu-id="cd12c-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="cd12c-282">**MySQL prestanda (dataflöde) jämförelse före och efter optimering**</span><span class="sxs-lookup"><span data-stu-id="cd12c-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="cd12c-283">(XFS File System)</span><span class="sxs-lookup"><span data-stu-id="cd12c-283">(XFS File System)</span></span>

![MySQL prestanda (dataflöde) jämförelse före och efter optimering][14]

<span data-ttu-id="cd12c-285">**Test-kommandon**</span><span class="sxs-lookup"><span data-stu-id="cd12c-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="cd12c-286">**Konfigurationsinställning för standard och optimering är följande:**</span><span class="sxs-lookup"><span data-stu-id="cd12c-286">**The configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="cd12c-287">Parametrar</span><span class="sxs-lookup"><span data-stu-id="cd12c-287">Parameters</span></span> | <span data-ttu-id="cd12c-288">Standard</span><span class="sxs-lookup"><span data-stu-id="cd12c-288">Default</span></span> | <span data-ttu-id="cd12c-289">Optimering</span><span class="sxs-lookup"><span data-stu-id="cd12c-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd12c-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="cd12c-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="cd12c-291">Ingen</span><span class="sxs-lookup"><span data-stu-id="cd12c-291">None</span></span> |<span data-ttu-id="cd12c-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="cd12c-292">7 GB</span></span> |
| <span data-ttu-id="cd12c-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="cd12c-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="cd12c-294">5 MB</span><span class="sxs-lookup"><span data-stu-id="cd12c-294">5 MB</span></span> |<span data-ttu-id="cd12c-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="cd12c-295">512 MB</span></span> |
| <span data-ttu-id="cd12c-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="cd12c-296">**max_connections**</span></span> |<span data-ttu-id="cd12c-297">100</span><span class="sxs-lookup"><span data-stu-id="cd12c-297">100</span></span> |<span data-ttu-id="cd12c-298">5000</span><span class="sxs-lookup"><span data-stu-id="cd12c-298">5000</span></span> |
| <span data-ttu-id="cd12c-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="cd12c-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="cd12c-300">0</span><span class="sxs-lookup"><span data-stu-id="cd12c-300">0</span></span> |<span data-ttu-id="cd12c-301">1</span><span class="sxs-lookup"><span data-stu-id="cd12c-301">1</span></span> |
| <span data-ttu-id="cd12c-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="cd12c-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="cd12c-303">1</span><span class="sxs-lookup"><span data-stu-id="cd12c-303">1</span></span> |<span data-ttu-id="cd12c-304">2</span><span class="sxs-lookup"><span data-stu-id="cd12c-304">2</span></span> |
| <span data-ttu-id="cd12c-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="cd12c-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="cd12c-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="cd12c-306">8 MB</span></span> |<span data-ttu-id="cd12c-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="cd12c-307">128 MB</span></span> |
| <span data-ttu-id="cd12c-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="cd12c-308">**query_cache_size**</span></span> |<span data-ttu-id="cd12c-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="cd12c-309">16 MB</span></span> |<span data-ttu-id="cd12c-310">0</span><span class="sxs-lookup"><span data-stu-id="cd12c-310">0</span></span> |

<span data-ttu-id="cd12c-311">Mer detaljerad [optimering konfigurationsparametrar](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), referera till den [MySQL officiella instruktioner](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="cd12c-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer to the [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="cd12c-312">**Testmiljö**</span><span class="sxs-lookup"><span data-stu-id="cd12c-312">**Test environment**</span></span>  

| <span data-ttu-id="cd12c-313">Maskinvara</span><span class="sxs-lookup"><span data-stu-id="cd12c-313">Hardware</span></span> | <span data-ttu-id="cd12c-314">Information</span><span class="sxs-lookup"><span data-stu-id="cd12c-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="cd12c-315">Processor</span><span class="sxs-lookup"><span data-stu-id="cd12c-315">CPU</span></span> |<span data-ttu-id="cd12c-316">AMD Opteron (TM)-Processor 4171 HAN / 4 kärnor</span><span class="sxs-lookup"><span data-stu-id="cd12c-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="cd12c-317">Minne</span><span class="sxs-lookup"><span data-stu-id="cd12c-317">Memory</span></span> |<span data-ttu-id="cd12c-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="cd12c-318">14 GB</span></span> |
| <span data-ttu-id="cd12c-319">Disk</span><span class="sxs-lookup"><span data-stu-id="cd12c-319">Disk</span></span> |<span data-ttu-id="cd12c-320">10 GB-disk</span><span class="sxs-lookup"><span data-stu-id="cd12c-320">10 GB/disk</span></span> |
| <span data-ttu-id="cd12c-321">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="cd12c-321">OS</span></span> |<span data-ttu-id="cd12c-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="cd12c-322">Ubuntu 14.04.1 LTS</span></span> |

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

