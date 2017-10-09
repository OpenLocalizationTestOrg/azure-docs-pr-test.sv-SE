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
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Förbered en virtuell Ubuntu-dator för Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Officiell Ubuntu molnet bilder
Ubuntu nu publicerar officiella Azure virtuella hårddiskar för hämtning på [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Om du har måste toobuild specialiserade Ubuntu-avbildningen för Azure i stället för att använda hello manuell proceduren nedan är rekommenderade toostart med de här kända fungerar virtuella hårddiskar och anpassa efter behov. hello senaste avbildningen versioner finns alltid på hello följande platser:

* Ubuntu 12.04/exakt: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/sin: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan har installerat en Ubuntu Linux operativsystem tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V. Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu installationsinformation**

* Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.
* Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.  Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.
* När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.
* Konfigurera inte en byte-partition på hello OS-disken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.

## <a name="manual-steps"></a>Manuella steg
> [!NOTE]
> Innan du försöker toocreate en egen anpassad avbildning Ubuntu på Azure, Överväg att använda hello fördefinierade och testade avbildningar från [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) i stället.
> 
> 

1. Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.

2. Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.

3. Ersätt hello aktuella databaserna i hello avbildningen toouse Ubuntu's Azure repor. hello stegen variera något beroende på hello Ubuntu version.
   
    Innan du redigerar `/etc/apt/sources.list`, är det rekommenderade toomake en säkerhetskopiering:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. hello Ubuntu Azure bilder följer nu hello *maskinvara aktivering* kernel (HWE). Uppdatera hello operativsystemet toohello senaste kernel genom att köra följande kommandon hello:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    **Se även:**
    - [https://Wiki.Ubuntu.com/kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://Wiki.Ubuntu.com/kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Ändra hello kernel Start raden för Grub tooinclude ytterligare kernel parametrar för Azure. Det här öppna toodo `/etc/default/grub` i en textredigerare, söka efter hello variabel med namnet `GRUB_CMDLINE_LINUX_DEFAULT` (eller lägga till det om det behövs) och redigera den tooinclude hello följande parametrar:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Spara och stäng filen och kör sedan `sudo update-grub`. Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure teknisk support med felsökning av problem.

6. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.

7. Installera hello Azure Linux-agenten:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    Hej `walinuxagent` paketet kan ta bort hello `NetworkManager` och `NetworkManager-gnome` paket, om de är installerade.

8. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse Ubuntu Linux virtuell hårddisk toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="references"></a>Referenser
Ubuntu maskinvara aktivering (HWE) kernel:

* [http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [http://Blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

