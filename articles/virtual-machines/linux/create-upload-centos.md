---
title: "aaaCreate och ladda upp en baserat på CentOS Linux VHD i Azure"
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller en baserat på CentOS Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 78f36be1d36e3d44cb836c912d143f2c9a72aab5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Förbered en CentOS-baserad virtuell dator för Azure
* [Förbered en CentOS 6.x virtuell dator för Azure](#centos-6x)
* [Förbered en CentOS 7.0 + virtuell dator för Azure](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan har installerat en CentOS (eller liknande derivat) Linux operativsystem tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V. Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

**CentOS installationsinformation**

* Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.
* Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.  Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet. Om du använder VirtualBox innebär detta att välja **fast storlek** som motsats toohello standard dynamiskt allokerade när du skapar hello disk.
* När du installerar hello Linux-system är *rekommenderas* att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). På så sätt undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk behövs toobe kopplade tooanother identiska VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.
* Kernel-stöd för att montera UDF-filsystem krävs. Vid första start på Azure hello skickas etablering configuration toohello Linux VM via UDF-formaterad media som är bifogade toohello gäst. hello Azure Linux-agenten måste kunna toomount hello UDF filen system tooread dess konfiguration och etablera hello VM.
* Linux kernel-versioner än 2.6.37 stöder inte NUMA på Hyper-V med större VM-storlekar. I detta fall främst påverkar äldre distributioner med hello uppströms Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504). System som kör anpassade kernlar som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange hello Start parametern `numa=off` på hello kernel kommandoradsverktyg i grub.conf. Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Konfigurera inte en byte-partition på hello OS-disken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.

## <a name="centos-6x"></a>CentOS 6.x

1. Välj hello virtuell dator i Hyper-V Manager.

2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.

3. CentOS 6, kan NetworkManager störa hello Azure Linux-agenten. Avinstallera paketet genom att köra följande kommando hello:
   
        # sudo rpm -e --nodeps NetworkManager

4. Skapa eller redigera hello filen `/etc/sysconfig/network` och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Skapa eller redigera hello filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande text:
   
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
   
        # sudo chkconfig network on

8. Om du vill att toouse hello OpenLogic speglar som finns inom hello Azure-datacenter, Ersätt då hello `/etc/yum.repos.d/CentOS-Base.repo` fil med hello följande databaser.  Detta lägger också till hello **[openlogic]** databasen som innehåller ytterligare paket, till exempel hello Azure Linux-agenten:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    hello resten av den här guiden kommer anta att du använder minst hello `[openlogic]` lagringsplatsen som kommer att använda tooinstall hello Azure Linux-agenten nedan.


9. Lägg till följande rad too/etc/yum.conf hello:
    
        http_caching=packages

10. Kör hello efter kommandot tooclear hello aktuella yum metadata och uppdateringsfiler hello system med senaste hello-paket:
    
        # yum clean all

    Om du skapar en avbildning för en äldre version av CentOS, bör tooupdate alla hello paket toohello senaste:

        # sudo yum -y update

    En omstart kan krävas när du har kört kommandot.

11. (Valfritt) Installera hello drivrutiner för hello Linux Integration Services (LIS).
   
    >[!IMPORTANT]
    hello steget är **krävs** för CentOS 6.3 och tidigare och en valfri för senare versioner.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    Alternativt, du kan följa hello hello manuell Installationsinstruktioner [LIS hämtningssidan](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM på den virtuella datorn.
 
12. Installera hello Azure Linux-agenten och beroenden:
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    Hej WALinuxAgent paketet tas och bort hello NetworkManager NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 3.


13. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo detta, öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.
    
    Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:
    
        rhgb quiet crashkernel=auto
    
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.  Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.

    >[!Important]
    CentOS 6.5 och tidigare måste också ange hello kernel parametern `numa=off`. Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

14. Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.

15. Skapa inte växlingsutrymme på hello OS-disken.
    
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klicka på **åtgärd -> stängs ned** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.


- - -
## <a name="centos-70"></a>CentOS 7.0 +
**Ändringar i CentOS 7 (och liknande produkter)**

Förbereda en virtuell dator för CentOS 7 för Azure är mycket lik tooCentOS 6, men det finns flera viktiga skillnader noteras:

* Hej NetworkManager paketet inte längre är i konflikt med hello Azure Linux-agent. Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.
* GRUB2 används nu som hello standard startprogrammet så hello proceduren för att redigera kernel parametrar har ändrats (se nedan).
* XFS är nu hello standardfilsystem. Hej ext4 filsystem kan fortfarande användas om du vill.

**Konfigurationssteg**

1. Välj hello virtuell dator i Hyper-V Manager.

2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.

3. Skapa eller redigera hello filen `/etc/sysconfig/network` och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Skapa eller redigera hello filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Om du vill att toouse hello OpenLogic speglar som finns inom hello Azure-datacenter, Ersätt då hello `/etc/yum.repos.d/CentOS-Base.repo` fil med hello följande databaser.  Detta lägger också till hello **[openlogic]** databasen som innehåller paket för hello Azure Linux-agenten:
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    hello resten av den här guiden kommer anta att du använder minst hello `[openlogic]` lagringsplatsen som kommer att använda tooinstall hello Azure Linux-agenten nedan.

7. Kör följande kommando tooclear hello aktuella yum metadata hello och installera uppdateringar:
   
        # sudo yum clean all

    Om du skapar en avbildning för en äldre version av CentOS, bör tooupdate alla hello paket toohello senaste:

        # sudo yum -y update

    En omstart krävs kanske köra det här kommandot.

8. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo detta, öppna `/etc/default/grub` i ett text-redigeraren och redigera hello `GRUB_CMDLINE_LINUX` parameter, till exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Även stängs av hello nya CentOS 7 namnkonventioner för nätverkskort. Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.

9. När du är klar redigera `/etc/default/grub` per ovan, kör hello följande toorebuild hello grub konfiguration:
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Om hello-avbildning från **VMWare, VirtualBox eller KVM:** Kontrollera hello Hyper-V-drivrutiner som ingår i hello initramfs:
   
   Redigera `/etc/dracut.conf`, lägga till innehåll:
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   Återskapa hello initramfs:
   
        # sudo dracut –f -v

11. Installera hello Azure Linux-agenten och beroenden:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Skapa inte växlingsutrymme på hello OS-disken.
   
   hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:
   
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

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse CentOS Linux virtuell hårddisk toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

