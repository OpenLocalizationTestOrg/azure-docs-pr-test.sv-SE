---
title: aaaCreate och ladda upp en Ubuntu Linux VHD i Azure
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller ett Ubuntu Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: cc546a487f769b32432a7e80ddcd0f6af44e201f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a><span data-ttu-id="24c2e-103">Förbered en virtuell Ubuntu-dator för Azure</span><span class="sxs-lookup"><span data-stu-id="24c2e-103">Prepare an Ubuntu virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a><span data-ttu-id="24c2e-104">Officiell Ubuntu molnet bilder</span><span class="sxs-lookup"><span data-stu-id="24c2e-104">Official Ubuntu cloud images</span></span>
<span data-ttu-id="24c2e-105">Ubuntu nu publicerar officiella Azure virtuella hårddiskar för hämtning på [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="24c2e-105">Ubuntu now publishes official Azure VHDs for download at [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span></span> <span data-ttu-id="24c2e-106">Om du har måste toobuild specialiserade Ubuntu-avbildningen för Azure i stället för att använda hello manuell proceduren nedan är rekommenderade toostart med de här kända fungerar virtuella hårddiskar och anpassa efter behov.</span><span class="sxs-lookup"><span data-stu-id="24c2e-106">If you need toobuild your own specialized Ubuntu image for Azure, rather than use hello manual procedure below it is recommended toostart with these known working VHDs and customize as needed.</span></span> <span data-ttu-id="24c2e-107">hello senaste avbildningen versioner finns alltid på hello följande platser:</span><span class="sxs-lookup"><span data-stu-id="24c2e-107">hello latest image releases can always be found at hello following locations:</span></span>

* <span data-ttu-id="24c2e-108">Ubuntu 12.04/exakt: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="24c2e-108">Ubuntu 12.04/Precise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="24c2e-109">Ubuntu 14.04/sin: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="24c2e-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="24c2e-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="24c2e-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24c2e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="24c2e-111">Prerequisites</span></span>
<span data-ttu-id="24c2e-112">Den här artikeln förutsätter att du redan har installerat en Ubuntu Linux operativsystem tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="24c2e-112">This article assumes that you have already installed an Ubuntu Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="24c2e-113">Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="24c2e-113">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="24c2e-114">Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="24c2e-114">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="24c2e-115">**Ubuntu installationsinformation**</span><span class="sxs-lookup"><span data-stu-id="24c2e-115">**Ubuntu installation notes**</span></span>

* <span data-ttu-id="24c2e-116">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="24c2e-116">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="24c2e-117">Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="24c2e-117">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="24c2e-118">Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="24c2e-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="24c2e-119">När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="24c2e-119">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="24c2e-120">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning.</span><span class="sxs-lookup"><span data-stu-id="24c2e-120">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="24c2e-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="24c2e-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="24c2e-122">Konfigurera inte en byte-partition på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="24c2e-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="24c2e-123">hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="24c2e-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="24c2e-124">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="24c2e-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="24c2e-125">Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="24c2e-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="manual-steps"></a><span data-ttu-id="24c2e-126">Manuella steg</span><span class="sxs-lookup"><span data-stu-id="24c2e-126">Manual steps</span></span>
> [!NOTE]
> <span data-ttu-id="24c2e-127">Innan du försöker toocreate en egen anpassad avbildning Ubuntu på Azure, Överväg att använda hello fördefinierade och testade avbildningar från [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) i stället.</span><span class="sxs-lookup"><span data-stu-id="24c2e-127">Before attempting toocreate your own custom Ubuntu image for Azure, please consider using hello pre-built and tested images from [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) instead.</span></span>
> 
> 

1. <span data-ttu-id="24c2e-128">Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="24c2e-128">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="24c2e-129">Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="24c2e-129">Click **Connect** tooopen hello window for hello virtual machine.</span></span>

3. <span data-ttu-id="24c2e-130">Ersätt hello aktuella databaserna i hello avbildningen toouse Ubuntu's Azure repor.</span><span class="sxs-lookup"><span data-stu-id="24c2e-130">Replace hello current repositories in hello image toouse Ubuntu's Azure repos.</span></span> <span data-ttu-id="24c2e-131">hello stegen variera något beroende på hello Ubuntu version.</span><span class="sxs-lookup"><span data-stu-id="24c2e-131">hello steps vary slightly depending on hello Ubuntu version.</span></span>
   
    <span data-ttu-id="24c2e-132">Innan du redigerar `/etc/apt/sources.list`, är det rekommenderade toomake en säkerhetskopiering:</span><span class="sxs-lookup"><span data-stu-id="24c2e-132">Before editing `/etc/apt/sources.list`, it is recommended toomake a backup:</span></span>
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    <span data-ttu-id="24c2e-133">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-133">Ubuntu 12.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="24c2e-134">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-134">Ubuntu 14.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="24c2e-135">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-135">Ubuntu 16.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. <span data-ttu-id="24c2e-136">hello Ubuntu Azure bilder följer nu hello *maskinvara aktivering* kernel (HWE).</span><span class="sxs-lookup"><span data-stu-id="24c2e-136">hello Ubuntu Azure images are now following hello *hardware enablement* (HWE) kernel.</span></span> <span data-ttu-id="24c2e-137">Uppdatera hello operativsystemet toohello senaste kernel genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="24c2e-137">Update hello operating system toohello latest kernel by running hello following commands:</span></span>

    <span data-ttu-id="24c2e-138">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-138">Ubuntu 12.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    <span data-ttu-id="24c2e-139">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-139">Ubuntu 14.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    <span data-ttu-id="24c2e-140">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="24c2e-140">Ubuntu 16.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    <span data-ttu-id="24c2e-141">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="24c2e-141">**See also:**</span></span>
    - [<span data-ttu-id="24c2e-142">https://Wiki.Ubuntu.com/kernel/LTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="24c2e-142">https://wiki.ubuntu.com/Kernel/LTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [<span data-ttu-id="24c2e-143">https://Wiki.Ubuntu.com/kernel/RollingLTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="24c2e-143">https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. <span data-ttu-id="24c2e-144">Ändra hello kernel Start raden för Grub tooinclude ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="24c2e-144">Modify hello kernel boot line for Grub tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="24c2e-145">Det här öppna toodo `/etc/default/grub` i en textredigerare, söka efter hello variabel med namnet `GRUB_CMDLINE_LINUX_DEFAULT` (eller lägga till det om det behövs) och redigera den tooinclude hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="24c2e-145">toodo this open `/etc/default/grub` in a text editor, find hello variable called `GRUB_CMDLINE_LINUX_DEFAULT` (or add it if needed) and edit it tooinclude hello following parameters:</span></span>
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    <span data-ttu-id="24c2e-146">Spara och stäng filen och kör sedan `sudo update-grub`.</span><span class="sxs-lookup"><span data-stu-id="24c2e-146">Save and close this file, and then run `sudo update-grub`.</span></span> <span data-ttu-id="24c2e-147">Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure teknisk support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="24c2e-147">This will ensure all console messages are sent toohello first serial port, which can assist Azure technical support with debugging issues.</span></span>

6. <span data-ttu-id="24c2e-148">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="24c2e-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="24c2e-149">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="24c2e-149">This is usually hello default.</span></span>

7. <span data-ttu-id="24c2e-150">Installera hello Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="24c2e-150">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    <span data-ttu-id="24c2e-151">Hej `walinuxagent` paketet kan ta bort hello `NetworkManager` och `NetworkManager-gnome` paket, om de är installerade.</span><span class="sxs-lookup"><span data-stu-id="24c2e-151">hello `walinuxagent` package may remove hello `NetworkManager` and `NetworkManager-gnome` packages, if they are installed.</span></span>

8. <span data-ttu-id="24c2e-152">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="24c2e-152">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. <span data-ttu-id="24c2e-153">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="24c2e-153">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="24c2e-154">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="24c2e-154">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24c2e-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24c2e-155">Next steps</span></span>
<span data-ttu-id="24c2e-156">Du är nu redo toouse Ubuntu Linux virtuell hårddisk toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="24c2e-156">You're now ready toouse your Ubuntu Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="24c2e-157">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="24c2e-157">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="references"></a><span data-ttu-id="24c2e-158">Referenser</span><span class="sxs-lookup"><span data-stu-id="24c2e-158">References</span></span>
<span data-ttu-id="24c2e-159">Ubuntu maskinvara aktivering (HWE) kernel:</span><span class="sxs-lookup"><span data-stu-id="24c2e-159">Ubuntu hardware enablement (HWE) kernel:</span></span>

* [<span data-ttu-id="24c2e-160">http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML</span><span class="sxs-lookup"><span data-stu-id="24c2e-160">http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html</span></span>](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [<span data-ttu-id="24c2e-161">http://Blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-hwe.HTML</span><span class="sxs-lookup"><span data-stu-id="24c2e-161">http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html</span></span>](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

