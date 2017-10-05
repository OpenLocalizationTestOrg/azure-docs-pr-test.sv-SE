---
title: Skapa och ladda upp en Ubuntu Linux VHD i Azure
description: "Lär dig att skapa och ladda upp en Azure virtuell hårddisk (VHD) som innehåller ett Ubuntu Linux-operativsystem."
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
ms.openlocfilehash: 4496b34ff88ca1e08cc74788ae09d787d4399eaf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a><span data-ttu-id="bf0e8-103">Förbered en virtuell Ubuntu-dator för Azure</span><span class="sxs-lookup"><span data-stu-id="bf0e8-103">Prepare an Ubuntu virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a><span data-ttu-id="bf0e8-104">Officiell Ubuntu molnet bilder</span><span class="sxs-lookup"><span data-stu-id="bf0e8-104">Official Ubuntu cloud images</span></span>
<span data-ttu-id="bf0e8-105">Ubuntu nu publicerar officiella Azure virtuella hårddiskar för hämtning på [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="bf0e8-105">Ubuntu now publishes official Azure VHDs for download at [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span></span> <span data-ttu-id="bf0e8-106">Om du behöver skapa särskilda Ubuntu-avbildningen för Azure snarare rekommenderas än att använda manuell proceduren nedan att börja med dessa kända fungerar virtuella hårddiskar och anpassa efter behov.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-106">If you need to build your own specialized Ubuntu image for Azure, rather than use the manual procedure below it is recommended to start with these known working VHDs and customize as needed.</span></span> <span data-ttu-id="bf0e8-107">De senaste versionerna av avbildningen kan alltid hittas på följande platser:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-107">The latest image releases can always be found at the following locations:</span></span>

* <span data-ttu-id="bf0e8-108">Ubuntu 12.04/exakt: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="bf0e8-108">Ubuntu 12.04/Precise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="bf0e8-109">Ubuntu 14.04/sin: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="bf0e8-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="bf0e8-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="bf0e8-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf0e8-111">Krav</span><span class="sxs-lookup"><span data-stu-id="bf0e8-111">Prerequisites</span></span>
<span data-ttu-id="bf0e8-112">Den här artikeln förutsätter att du redan har installerat ett Ubuntu Linux-operativsystem till en virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-112">This article assumes that you have already installed an Ubuntu Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="bf0e8-113">Det finns flera verktyg för att skapa VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-113">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="bf0e8-114">Instruktioner finns i [installera Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf0e8-114">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="bf0e8-115">**Ubuntu installationsinformation**</span><span class="sxs-lookup"><span data-stu-id="bf0e8-115">**Ubuntu installation notes**</span></span>

* <span data-ttu-id="bf0e8-116">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-116">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="bf0e8-117">VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-117">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="bf0e8-118">Du kan konvertera disken till VHD-format med hjälp av Hyper-V Manager eller cmdlet convert-vhd.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-118">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span>
* <span data-ttu-id="bf0e8-119">När du installerar Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta är standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="bf0e8-119">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="bf0e8-120">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk någonsin måste vara kopplad till en annan virtuell dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-120">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="bf0e8-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="bf0e8-122">Konfigurera inte en byte-partition på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-122">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="bf0e8-123">Linux-agenten kan konfigureras för att skapa en växlingsfil på tillfällig disken.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-123">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="bf0e8-124">Mer information om detta finns i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-124">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="bf0e8-125">Alla de virtuella hårddiskarna måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-125">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="manual-steps"></a><span data-ttu-id="bf0e8-126">Manuella steg</span><span class="sxs-lookup"><span data-stu-id="bf0e8-126">Manual steps</span></span>
> [!NOTE]
> <span data-ttu-id="bf0e8-127">Innan du försöker skapa en egen anpassad avbildning Ubuntu för Azure, Överväg att använda fördefinierade och testade avbildningar från [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) i stället.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-127">Before attempting to create your own custom Ubuntu image for Azure, please consider using the pre-built and tested images from [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) instead.</span></span>
> 
> 

1. <span data-ttu-id="bf0e8-128">I rutan i mitten av Hyper-V Manager, Välj den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-128">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="bf0e8-129">Klicka på **Anslut** att öppna fönstret för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-129">Click **Connect** to open the window for the virtual machine.</span></span>

3. <span data-ttu-id="bf0e8-130">Ersätt aktuella databaserna i avbildningen som ska användas Ubuntu's Azure repor.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-130">Replace the current repositories in the image to use Ubuntu's Azure repos.</span></span> <span data-ttu-id="bf0e8-131">Stegen variera beroende på vilken Ubuntu-version.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-131">The steps vary slightly depending on the Ubuntu version.</span></span>
   
    <span data-ttu-id="bf0e8-132">Innan du redigerar `/etc/apt/sources.list`, rekommenderas att säkerhetskopiera:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-132">Before editing `/etc/apt/sources.list`, it is recommended to make a backup:</span></span>
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    <span data-ttu-id="bf0e8-133">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-133">Ubuntu 12.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="bf0e8-134">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-134">Ubuntu 14.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="bf0e8-135">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-135">Ubuntu 16.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. <span data-ttu-id="bf0e8-136">Ubuntu Azure-avbildningar följer nu det *maskinvara aktivering* kernel (HWE).</span><span class="sxs-lookup"><span data-stu-id="bf0e8-136">The Ubuntu Azure images are now following the *hardware enablement* (HWE) kernel.</span></span> <span data-ttu-id="bf0e8-137">Uppdatera operativsystemet för senaste kerneln genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-137">Update the operating system to the latest kernel by running the following commands:</span></span>

    <span data-ttu-id="bf0e8-138">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-138">Ubuntu 12.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    <span data-ttu-id="bf0e8-139">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-139">Ubuntu 14.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    <span data-ttu-id="bf0e8-140">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-140">Ubuntu 16.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    <span data-ttu-id="bf0e8-141">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="bf0e8-141">**See also:**</span></span>
    - [<span data-ttu-id="bf0e8-142">https://Wiki.Ubuntu.com/kernel/LTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="bf0e8-142">https://wiki.ubuntu.com/Kernel/LTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [<span data-ttu-id="bf0e8-143">https://Wiki.Ubuntu.com/kernel/RollingLTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="bf0e8-143">https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. <span data-ttu-id="bf0e8-144">Ändra raden kernel-Start för Grub att inkludera ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-144">Modify the kernel boot line for Grub to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="bf0e8-145">Att göra den här öppna `/etc/default/grub` i en textredigerare, söka efter den variabel som kallas `GRUB_CMDLINE_LINUX_DEFAULT` (eller lägga till det om det behövs) och redigera den för att inkludera följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-145">To do this open `/etc/default/grub` in a text editor, find the variable called `GRUB_CMDLINE_LINUX_DEFAULT` (or add it if needed) and edit it to include the following parameters:</span></span>
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    <span data-ttu-id="bf0e8-146">Spara och stäng filen och kör sedan `sudo update-grub`.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-146">Save and close this file, and then run `sudo update-grub`.</span></span> <span data-ttu-id="bf0e8-147">Detta säkerställer att alla konsolmeddelanden skickas till den första seriella porten som kan hjälpa Azure teknisk support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-147">This will ensure all console messages are sent to the first serial port, which can assist Azure technical support with debugging issues.</span></span>

6. <span data-ttu-id="bf0e8-148">Se till att SSH-server har installerats och konfigurerats för att starta när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-148">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="bf0e8-149">Detta är vanligtvis standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-149">This is usually the default.</span></span>

7. <span data-ttu-id="bf0e8-150">Installera Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-150">Install the Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    <span data-ttu-id="bf0e8-151">Den `walinuxagent` paketet kan ta bort den `NetworkManager` och `NetworkManager-gnome` paket, om de är installerade.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-151">The `walinuxagent` package may remove the `NetworkManager` and `NetworkManager-gnome` packages, if they are installed.</span></span>

8. <span data-ttu-id="bf0e8-152">Kör följande kommandon för att ta bort etableringen av den virtuella datorn och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-152">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. <span data-ttu-id="bf0e8-153">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-153">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="bf0e8-154">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-154">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf0e8-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf0e8-155">Next steps</span></span>
<span data-ttu-id="bf0e8-156">Nu är du redo att använda din virtuella hårddisk Ubuntu Linux för att skapa nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0e8-156">You're now ready to use your Ubuntu Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="bf0e8-157">Om att du överföra VHD-filen till Azure finns i steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bf0e8-157">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="references"></a><span data-ttu-id="bf0e8-158">Referenser</span><span class="sxs-lookup"><span data-stu-id="bf0e8-158">References</span></span>
<span data-ttu-id="bf0e8-159">Ubuntu maskinvara aktivering (HWE) kernel:</span><span class="sxs-lookup"><span data-stu-id="bf0e8-159">Ubuntu hardware enablement (HWE) kernel:</span></span>

* [<span data-ttu-id="bf0e8-160">http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML</span><span class="sxs-lookup"><span data-stu-id="bf0e8-160">http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html</span></span>](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [<span data-ttu-id="bf0e8-161">http://Blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-hwe.HTML</span><span class="sxs-lookup"><span data-stu-id="bf0e8-161">http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html</span></span>](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

