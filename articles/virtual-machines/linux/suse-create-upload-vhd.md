---
title: aaaCreate och ladda upp en SUSE Linux VHD i Azure
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller en SUSE Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: 9185c7e67279357f00db0f43e944e96c58f0dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Förbered en virtuell SLES- eller openSUSE-dator för Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan har installerat en SUSE eller openSUSE Linux operativsystem tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V. Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE installationsinformation
* Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.
* Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.  Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.
* När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.
* Konfigurera inte en byte-partition på hello OS-disken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.

## <a name="use-suse-studio"></a>Använd SUSE Studio
[SUSE Studio](http://www.susestudio.com) kan enkelt skapa och hantera dina SLES och openSUSE avbildningar för Azure och Hyper-V. Detta är hello rekommenderat tillvägagångssätt för att anpassa dina egna SLES och openSUSE bilder.

Som ett alternativ toobuilding egna VHD, SUSE publiceras även BYOS (ta med din egen prenumeration) avbildningar för SLES på [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Förbereda SUSE Linux Enterprise Server 11 SP4
1. Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.
2. Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.
3. Registrera din SUSE Linux Enterprise system tooallow den toodownload uppdateringar och installera paket.
4. Uppdatera hello system med hello senaste korrigeringarna:
   
        # sudo zypper update
5. Installera hello Azure Linux-agenten från hello SLES databasen:
   
        # sudo zypper install WALinuxAgent
6. Kontrollera om waagent anges för ”on” i chkconfig och om inte, aktivera den för autostart:
   
        # sudo chkconfig waagent on
7. Kontrollera om waagent-tjänsten körs och starta om inte: 
   
        # sudo service waagent start
8. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. Det här öppna toodo ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.
9. Bekräfta att /boot/grub/menu.lst och/etc/fstab båda referens hello disk med dess UUID (av uuid) i stället för hello disk-ID (med-id). 
   
    Hämta disk-UUID
   
        # ls /dev/disk/by-uuid/
   
    Om /dev/disk/by-id är används, uppdatera både /boot/grub/menu.lst och/etc/fstab med hello rätt av uuid värdet
   
    Före ändringen
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Efter ändring
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Det rekommenderas tooedit hello filen ”/ etc/sysconfig/nätverk/dhcp” och ändra hello `DHCLIENT_SET_HOSTNAME` parametern toohello följande:
    
     DHCLIENT_SET_HOSTNAME = ”Nej”
12. Kommentera ut ”/ etc/sudoers”, eller ta bort hello följande rader om de finns:
    
     Som standard targetpw # uppmana hello lösenordet för hello målanvändare d.v.s. rot alla ALL=(ALL) alla # varning! Använd endast denna tillsammans med 'Standardvärden targetpw'!
13. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.
14. Skapa inte växlingsutrymme på hello OS-disken.
    
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Obs: Ange den här toowhatever måste toobe.
15. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - avetablering
    # <a name="export-histsize0"></a>Exportera HISTSIZE = 0
    # <a name="logout"></a>Logga ut
16. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.

- - -
## <a name="prepare-opensuse-131"></a>Förbereda openSUSE 13,1 +
1. Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.
2. Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.
3. Kör hello-kommando på hello shell '`zypper lr`'. Om det här kommandot returnerar utdata liknande toohello följande och sedan hello databaser är korrekt konfigurerat--krävs inte några justeringar (Observera att versionsnumren kan variera):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Om hello kommando returnerar ”inga databaser som definierats...” Använd hello följande kommandon tooadd dessa repor:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Du kan kontrollera hello databaser har lagts till genom att köra kommandot hello '`zypper lr`' igen. Om en av hello uppdatering databaser inte är aktiverad, kan du aktivera det med följande kommando:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Uppdatera hello kernel toohello senaste tillgängliga versionen:
   
        # sudo zypper up kernel-default
   
    Eller tooupdate hello system med alla hello senaste korrigeringarna:
   
        # sudo zypper update
5. Installera hello Azure Linux-agenten.
   
   # <a name="sudo-zypper-install-walinuxagent"></a>sudo zypper installera WALinuxAgent
6. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo detta, öppna ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
   
     konsolen = ttyS0 earlyprintk = ttyS0 rootdelay = 300
   
   Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Dessutom tar du bort hello följande parametrar från hello kernel Start rad om de finns:
   
     reservera libata.atapi_enabled=0 = 0x1f0, 0x8
7. Det rekommenderas tooedit hello filen ”/ etc/sysconfig/nätverk/dhcp” och ändra hello `DHCLIENT_SET_HOSTNAME` parametern toohello följande:
   
     DHCLIENT_SET_HOSTNAME = ”Nej”
8. **Viktigt:** i ”/ etc/sudoers”, kommentera ut eller ta bort hello följande rader om de finns:
   
     Som standard targetpw # uppmana hello lösenordet för hello målanvändare d.v.s. rot alla ALL=(ALL) alla # varning! Använd endast denna tillsammans med 'Standardvärden targetpw'!
9. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.
10. Skapa inte växlingsutrymme på hello OS-disken.
    
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Obs: Ange den här toowhatever måste toobe.
11. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - avetablering
    # <a name="export-histsize0"></a>Exportera HISTSIZE = 0
    # <a name="logout"></a>Logga ut
12. Kontrollera hello Azure Linux-agenten körs vid start:
    
        # sudo systemctl enable waagent.service
13. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse SUSE Linux virtuell hårddisk toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

