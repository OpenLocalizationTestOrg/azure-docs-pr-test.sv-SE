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
# <a name="prepare-a-debian-vhd-for-azure"></a>Förbered en Debian VHD för Azure
## <a name="prerequisites"></a>Krav
Det här avsnittet förutsätter att du redan har installerat en Debian Linux-operativsystem från en ISO-fil som hämtas från hello [Debian webbplats](https://www.debian.org/distrib/) tooa virtuell hårddisk. Det finns flera verktyg toocreate VHD-filer. Hyper-V är bara ett exempel. Instruktioner med Hyper-V finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Installationsinformation
* Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.
* hello senare VHDX-format stöds inte i Azure. Du kan konvertera hello tooVHD diskformat med hjälp av Hyper-V Manager eller hello **convert-vhd** cmdlet.
* När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.
* Konfigurera inte en byte-partition på hello OS-disken. hello Azure Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk. Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.

## <a name="use-azure-manage-toocreate-debian-vhds"></a>Använda Azure-hantera toocreate Debian virtuella hårddiskar
Det finns ett verktyg för att generera Debian virtuella hårddiskar på Azure, till exempel hello [azure-hantera](https://github.com/credativ/azure-manage) skript från [credativ](http://www.credativ.com/). Detta är hello rekommenderat tillvägagångssätt jämfört med att skapa en avbildning från grunden. Toocreate en Debian 8 VHD kör följande kommandon toodownload azure hantera hello (och beroenden) och kör hello azure_build_image skript:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Förbered manuellt en Debian virtuell Hårddisk
1. Välj hello virtuell dator i Hyper-V Manager.
2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.
3. Kommentera hello rad för **deb cdrom** i `/etc/apt/source.list` om du ställer in hello VM mot en ISO-fil.
4. Redigera hello `/etc/default/grub` filen och ändra hello **GRUB_CMDLINE_LINUX** parametern enligt följande tooinclude ytterligare kernel parametrar för Azure.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. Återskapa hello grub och kör:
   
        # sudo update-grub
6. Lägg till Debian's Azure databaser too/etc/apt/sources.list för Debian 7 eller 8:
   
    **Debian 7.x ”Wheezy”**
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    **Debian 8.x ”Jessie”**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. Installera hello Azure Linux-agenten:
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. Debian 7 är det obligatoriska toorun hello 3,16-baserade kernel från hello wheezy backports databasen. Först skapar du en fil med namnet /etc/apt/preferences.d/linux.pref med hello följande innehåll:
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    Kör sedan ”lgh sudo-get installera linux-bild-amd64” tooinstall hello ny kernel.
3. Ta bort etableringen hello virtuell dator och förbereda den för att etablera på Azure och kör:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. Klicka på **åtgärd** i Hyper-V Manager -> stängs ned. Din Linux VHD är nu redo toobe upp tooAzure.

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse Debian virtuell hårddisk toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

