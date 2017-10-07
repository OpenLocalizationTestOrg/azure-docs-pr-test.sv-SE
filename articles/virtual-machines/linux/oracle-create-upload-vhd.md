---
title: "aaaCreate och ladda upp en VHD för Oracle Linux | Microsoft Docs"
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller ett Oracle Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: be9cf284d7f5e7122a106506a343e53e9f1ac56e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Förbered en virtuell Oracle Linux-dator för Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan har installerat en Oracle Linux operativsystem tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V. Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Installationsinformation för Oracle Linux
* Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.
* Oracles Red Hat kompatibel kernel och deras UEK3 (Unbreakable Enterprise Kernel) stöds både i Hyper-V och Azure. För bästa resultat bör vara säker på att tooupdate toohello senaste kernel när du förberedde din Oracle Linux VHD.
* Oracles UEK2 stöds inte på Hyper-V och Azure som inte omfattar hello krävs drivrutiner.
* Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.  Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.
* När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.
* NUMA stöds inte för större VM-storlekar på grund av tooa programfel i Linux kernel-versioner än 2.6.37. I detta fall främst påverkar distributioner med hello överordnade Red Hat 2.6.32 kernel. Manuell installation av hello Azure Linux-agenten (waagent) inaktiverar automatiskt NUMA i hello GRUB konfiguration för hello Linux kernel. Mer information om detta finns i hello stegen nedan.
* Konfigurera inte en byte-partition på hello OS-disken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.
* Kontrollera att hello `Addons` databasen är aktiverad. Redigera filen hello `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) eller `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), och ändra hello rad `enabled=0` för`enabled=1` under **[ol6_addons]** eller **[ol7_addons]** på denna filen.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Du måste slutföra specifika konfigurationssteg i hello operativsystemet för hello virtuella toorun i Azure.

1. Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.
2. Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.
3. Avinstallera NetworkManager genom att köra följande kommando hello:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Obs:** om hello paketet inte har installerats, kommandot misslyckas med felmeddelandet. Detta är förväntat.
4. Skapa en fil med namnet **nätverk** i hello `/etc/sysconfig/` katalog som innehåller hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Skapa en fil med namnet **ifcfg eth0** i hello `/etc/sysconfig/network-scripts/` katalog som innehåller hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Kontrollera hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:
   
        # chkconfig network on
8. Installera python-pyasn1 genom att köra följande kommando hello:
   
        # sudo yum install python-pyasn1
9. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. Det här öppna toodo ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Detta inaktiverar NUMA på grund av tooa programfel i Oracles Red Hat kompatibel kernel.
   
   Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
   Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.
   
   Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.
10. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.
11. Installera hello Azure Linux-agenten genom att köra följande kommando hello. hello senaste versionen är 2.0.15.
    
        # sudo yum install WALinuxAgent
    
    Observera att installera hello WALinuxAgent paketet tas bort hello NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 2.
12. Skapa inte växlingsutrymme på hello OS-disken.
    
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Ändringar i Oracle Linux 7**

Förbereda en virtuell dator för Oracle Linux 7 för Azure är mycket lik tooOracle Linux 6, men det finns flera viktiga skillnader noteras:

* Både hello Red Hat kompatibel kernel och Oracles UEK3 stöds i Azure.  Hej UEK3 kernel rekommenderas.
* Hej NetworkManager paketet inte längre är i konflikt med hello Azure Linux-agent. Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.
* GRUB2 används nu som hello standard startprogrammet så hello proceduren för att redigera kernel parametrar har ändrats (se nedan).
* XFS är nu hello standardfilsystem. Hej ext4 filsystem kan fortfarande användas om du vill.

**Konfigurationssteg**

1. Välj hello virtuell dator i Hyper-V Manager.
2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.
3. Skapa en fil med namnet **nätverk** i hello `/etc/sysconfig/` katalog som innehåller hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Skapa en fil med namnet **ifcfg eth0** i hello `/etc/sysconfig/network-scripts/` katalog som innehåller hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Kontrollera hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:
   
        # sudo chkconfig network on
7. Installera hello python pyasn1 paketet genom att köra följande kommando hello:
   
        # sudo yum install python-pyasn1
8. Kör följande kommando tooclear hello aktuella yum metadata hello och installera uppdateringar:
   
        # sudo yum clean all
        # sudo yum -y update
9. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo detta öppna ”/ etc/standard/grub” i en text-redigeraren och redigera hello `GRUB_CMDLINE_LINUX` parameter, till exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Även stängs av hello nya OEL 7 namnkonventioner för nätverkskort. Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:
   
       rhgb quiet crashkernel=auto
   
   Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.
   
   Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.
10. När du är klar för redigering ”/ etc/standard/grub” per ovan, kör hello följande toorebuild hello grub konfiguration:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.
12. Installera hello Azure Linux-agenten genom att köra följande kommando hello:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Skapa inte växlingsutrymme på hello OS-disken.
    
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se hello föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse Oracle Linux VHD toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

