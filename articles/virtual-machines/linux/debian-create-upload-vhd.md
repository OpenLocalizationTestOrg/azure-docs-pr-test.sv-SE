---
title: "Förbereda en Debian Linux VHD i Azure | Microsoft Docs"
description: "Lär dig hur du skapar Debian 7 och 8 VHD-filer för distribution i Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 63970d162c12984d6476bf0b9fc4ab70160eccdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="6764b-103">Förbered en Debian VHD för Azure</span><span class="sxs-lookup"><span data-stu-id="6764b-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="6764b-104">Krav</span><span class="sxs-lookup"><span data-stu-id="6764b-104">Prerequisites</span></span>
<span data-ttu-id="6764b-105">Det här avsnittet förutsätter att du redan har installerat en Debian Linux-operativsystem från en ISO-fil som hämtas från den [Debian webbplats](https://www.debian.org/distrib/) till en virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="6764b-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from the [Debian website](https://www.debian.org/distrib/) to a virtual hard disk.</span></span> <span data-ttu-id="6764b-106">Det finns flera verktyg för att skapa VHD-filer. Hyper-V är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="6764b-106">Multiple tools exist to create .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="6764b-107">Instruktioner med Hyper-V finns i [installera Hyper-V-rollen och konfigurera en virtuell dator](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="6764b-107">For instructions using Hyper-V, see [Install the Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="6764b-108">Installationsinformation</span><span class="sxs-lookup"><span data-stu-id="6764b-108">Installation notes</span></span>
* <span data-ttu-id="6764b-109">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="6764b-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="6764b-110">Nyare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="6764b-110">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="6764b-111">Du kan konvertera disken till VHD-format med hjälp av Hyper-V Manager eller **convert-vhd** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6764b-111">You can convert the disk to VHD format using Hyper-V Manager or the **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="6764b-112">När du installerar Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta är standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="6764b-112">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="6764b-113">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk någonsin måste vara kopplad till en annan virtuell dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="6764b-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="6764b-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="6764b-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="6764b-115">Konfigurera inte en byte-partition på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="6764b-115">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="6764b-116">Azure Linux-agenten kan konfigureras för att skapa en växlingsfil på tillfällig disken.</span><span class="sxs-lookup"><span data-stu-id="6764b-116">The Azure Linux agent can be configured to create a swap file on the temporary resource disk.</span></span> <span data-ttu-id="6764b-117">Mer information om detta finns i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="6764b-117">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="6764b-118">Alla de virtuella hårddiskarna måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="6764b-118">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-to-create-debian-vhds"></a><span data-ttu-id="6764b-119">Använd hantera i Azure för att skapa Debian virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="6764b-119">Use Azure-Manage to create Debian VHDs</span></span>
<span data-ttu-id="6764b-120">Det finns ett verktyg för att generera Debian virtuella hårddiskar på Azure, som den [azure-hantera](https://github.com/credativ/azure-manage) skript från [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="6764b-120">There are tools available for generating Debian VHDs for Azure, such as the [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="6764b-121">Det här är den rekommenderade metoden jämfört med att skapa en avbildning från grunden.</span><span class="sxs-lookup"><span data-stu-id="6764b-121">This is the recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="6764b-122">Till exempel för att skapa en Debian 8 VHD kör följande kommandon för att hämta azure hantera (och beroenden) och kör skriptet azure_build_image:</span><span class="sxs-lookup"><span data-stu-id="6764b-122">For example, to create a Debian 8 VHD run the following commands to download azure-manage (and dependencies) and run the azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="6764b-123">Förbered manuellt en Debian virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="6764b-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="6764b-124">Välj den virtuella datorn i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="6764b-124">In Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="6764b-125">Klicka på **Anslut** att öppna ett konsolfönster för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6764b-125">Click **Connect** to open a console window for the virtual machine.</span></span>
3. <span data-ttu-id="6764b-126">Kommentera raden för **deb cdrom** i `/etc/apt/source.list` om du ställer in den virtuella datorn mot en ISO-fil.</span><span class="sxs-lookup"><span data-stu-id="6764b-126">Comment out the line for **deb cdrom** in `/etc/apt/source.list` if you set up the VM against an ISO file.</span></span>
4. <span data-ttu-id="6764b-127">Redigera den `/etc/default/grub` filen och ändra den **GRUB_CMDLINE_LINUX** parametern enligt följande för att inkludera ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="6764b-127">Edit the `/etc/default/grub` file and modify the **GRUB_CMDLINE_LINUX** parameter as follows to include additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="6764b-128">Återskapa grub och kör:</span><span class="sxs-lookup"><span data-stu-id="6764b-128">Rebuild the grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="6764b-129">Lägg till Debian's Azure databaser /etc/apt/sources.list för Debian 7 eller 8:</span><span class="sxs-lookup"><span data-stu-id="6764b-129">Add Debian's Azure repositories to /etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="6764b-130">**Debian 7.x ”Wheezy”**</span><span class="sxs-lookup"><span data-stu-id="6764b-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="6764b-131">**Debian 8.x ”Jessie”**</span><span class="sxs-lookup"><span data-stu-id="6764b-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="6764b-132">Installera Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="6764b-132">Install the Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="6764b-133">Det är Debian 7 krävs för att köra kärnan i 3,16 från wheezy backports-databasen.</span><span class="sxs-lookup"><span data-stu-id="6764b-133">For Debian 7, it is required to run the 3.16-based kernel from the wheezy-backports repository.</span></span> <span data-ttu-id="6764b-134">Först skapar du en fil som heter /etc/apt/preferences.d/linux.pref med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="6764b-134">First create a file called /etc/apt/preferences.d/linux.pref with the following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="6764b-135">Kör ”sudo lgh get installera linux-bild-amd64” så här installerar du nya kernel.</span><span class="sxs-lookup"><span data-stu-id="6764b-135">Then run "sudo apt-get install linux-image-amd64" to install the new kernel.</span></span>
3. <span data-ttu-id="6764b-136">Ta bort etableringen av den virtuella datorn och förbereda den för att etablera på Azure och kör:</span><span class="sxs-lookup"><span data-stu-id="6764b-136">Deprovision the virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="6764b-137">Klicka på **åtgärd** i Hyper-V Manager -> stängs ned.</span><span class="sxs-lookup"><span data-stu-id="6764b-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="6764b-138">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="6764b-138">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6764b-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6764b-139">Next steps</span></span>
<span data-ttu-id="6764b-140">Nu är du redo att använda din Debian virtuell hårddisk för att skapa nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="6764b-140">You're now ready to use your Debian virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="6764b-141">Om att du överföra VHD-filen till Azure finns i steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6764b-141">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

