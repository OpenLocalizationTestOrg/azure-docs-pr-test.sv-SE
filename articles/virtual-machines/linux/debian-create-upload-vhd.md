---
title: aaaPrepare en Debian Linux VHD i Azure | Microsoft Docs
description: "Lär dig hur toocreate Debian 7 och 8 VHD-filer för distribution i Azure."
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
ms.openlocfilehash: e67d126de3db146357a6509aedb5f478576768f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="83e57-103">Förbered en Debian VHD för Azure</span><span class="sxs-lookup"><span data-stu-id="83e57-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="83e57-104">Krav</span><span class="sxs-lookup"><span data-stu-id="83e57-104">Prerequisites</span></span>
<span data-ttu-id="83e57-105">Det här avsnittet förutsätter att du redan har installerat en Debian Linux-operativsystem från en ISO-fil som hämtas från hello [Debian webbplats](https://www.debian.org/distrib/) tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="83e57-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from hello [Debian website](https://www.debian.org/distrib/) tooa virtual hard disk.</span></span> <span data-ttu-id="83e57-106">Det finns flera verktyg toocreate VHD-filer. Hyper-V är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="83e57-106">Multiple tools exist toocreate .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="83e57-107">Instruktioner med Hyper-V finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="83e57-107">For instructions using Hyper-V, see [Install hello Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="83e57-108">Installationsinformation</span><span class="sxs-lookup"><span data-stu-id="83e57-108">Installation notes</span></span>
* <span data-ttu-id="83e57-109">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="83e57-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="83e57-110">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="83e57-110">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="83e57-111">Du kan konvertera hello tooVHD diskformat med hjälp av Hyper-V Manager eller hello **convert-vhd** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="83e57-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="83e57-112">När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="83e57-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="83e57-113">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning.</span><span class="sxs-lookup"><span data-stu-id="83e57-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="83e57-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="83e57-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="83e57-115">Konfigurera inte en byte-partition på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="83e57-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="83e57-116">hello Azure Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="83e57-116">hello Azure Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="83e57-117">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="83e57-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="83e57-118">Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="83e57-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-toocreate-debian-vhds"></a><span data-ttu-id="83e57-119">Använda Azure-hantera toocreate Debian virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="83e57-119">Use Azure-Manage toocreate Debian VHDs</span></span>
<span data-ttu-id="83e57-120">Det finns ett verktyg för att generera Debian virtuella hårddiskar på Azure, till exempel hello [azure-hantera](https://github.com/credativ/azure-manage) skript från [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="83e57-120">There are tools available for generating Debian VHDs for Azure, such as hello [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="83e57-121">Detta är hello rekommenderat tillvägagångssätt jämfört med att skapa en avbildning från grunden.</span><span class="sxs-lookup"><span data-stu-id="83e57-121">This is hello recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="83e57-122">Toocreate en Debian 8 VHD kör följande kommandon toodownload azure hantera hello (och beroenden) och kör hello azure_build_image skript:</span><span class="sxs-lookup"><span data-stu-id="83e57-122">For example, toocreate a Debian 8 VHD run hello following commands toodownload azure-manage (and dependencies) and run hello azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="83e57-123">Förbered manuellt en Debian virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="83e57-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="83e57-124">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="83e57-124">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="83e57-125">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83e57-125">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="83e57-126">Kommentera hello rad för **deb cdrom** i `/etc/apt/source.list` om du ställer in hello VM mot en ISO-fil.</span><span class="sxs-lookup"><span data-stu-id="83e57-126">Comment out hello line for **deb cdrom** in `/etc/apt/source.list` if you set up hello VM against an ISO file.</span></span>
4. <span data-ttu-id="83e57-127">Redigera hello `/etc/default/grub` filen och ändra hello **GRUB_CMDLINE_LINUX** parametern enligt följande tooinclude ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="83e57-127">Edit hello `/etc/default/grub` file and modify hello **GRUB_CMDLINE_LINUX** parameter as follows tooinclude additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="83e57-128">Återskapa hello grub och kör:</span><span class="sxs-lookup"><span data-stu-id="83e57-128">Rebuild hello grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="83e57-129">Lägg till Debian's Azure databaser too/etc/apt/sources.list för Debian 7 eller 8:</span><span class="sxs-lookup"><span data-stu-id="83e57-129">Add Debian's Azure repositories too/etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="83e57-130">**Debian 7.x ”Wheezy”**</span><span class="sxs-lookup"><span data-stu-id="83e57-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="83e57-131">**Debian 8.x ”Jessie”**</span><span class="sxs-lookup"><span data-stu-id="83e57-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="83e57-132">Installera hello Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="83e57-132">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="83e57-133">Debian 7 är det obligatoriska toorun hello 3,16-baserade kernel från hello wheezy backports databasen.</span><span class="sxs-lookup"><span data-stu-id="83e57-133">For Debian 7, it is required toorun hello 3.16-based kernel from hello wheezy-backports repository.</span></span> <span data-ttu-id="83e57-134">Först skapar du en fil med namnet /etc/apt/preferences.d/linux.pref med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="83e57-134">First create a file called /etc/apt/preferences.d/linux.pref with hello following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="83e57-135">Kör sedan ”lgh sudo-get installera linux-bild-amd64” tooinstall hello ny kernel.</span><span class="sxs-lookup"><span data-stu-id="83e57-135">Then run "sudo apt-get install linux-image-amd64" tooinstall hello new kernel.</span></span>
3. <span data-ttu-id="83e57-136">Ta bort etableringen hello virtuell dator och förbereda den för att etablera på Azure och kör:</span><span class="sxs-lookup"><span data-stu-id="83e57-136">Deprovision hello virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="83e57-137">Klicka på **åtgärd** i Hyper-V Manager -> stängs ned.</span><span class="sxs-lookup"><span data-stu-id="83e57-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="83e57-138">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="83e57-138">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83e57-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83e57-139">Next steps</span></span>
<span data-ttu-id="83e57-140">Du är nu redo toouse Debian virtuell hårddisk toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="83e57-140">You're now ready toouse your Debian virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="83e57-141">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83e57-141">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

