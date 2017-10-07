---
title: "aaaCreate och ladda upp en Red Hat Enterprise Linux VHD för användning i Azure | Microsoft Docs"
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller en Red Hat Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: szark
ms.openlocfilehash: bdd1bbfbee55b5cc61d69a09b06b6bd2c3de362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Förbered en Red Hat-baserad virtuell dator för Azure
I den här artikeln får du lära dig hur tooprepare en virtuell dator för Red Hat Enterprise Linux (RHEL) för användning i Azure. hello versioner av RHEL som beskrivs i den här artikeln är 6.7 + och 7.1 +. hello hypervisorer för förberedelse som beskrivs i den här artikeln är Hyper-V, kernel-baserad virtuell dator (KVM) och VMware. Läs mer om behörighetskraven för deltagande i programmet för Red Hat Molnåtkomst [Red Hat Molnåtkomst webbplats](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) och [kör RHEL på Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Förbered en Red Hat-baserad virtuell dator från Hyper-V Manager

### <a name="prerequisites"></a>Krav
Det här avsnittet förutsätter att du redan har fått en ISO-fil från hello Red Hat webbplats och installerade hello RHEL avbildningen tooa virtuell hårddisk (VHD). Mer information om hur toouse Hyper-V Manager tooinstall en operativsystemavbildning finns [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL installationsinformation**

* Azure stöder inte hello VHDX-format. Azure stöder endast fast virtuell Hårddisk. Du kan använda tooVHD för Hyper-V Manager tooconvert hello diskformat eller du kan använda hello convert-vhd-cmdlet. Om du använder VirtualBox markerar **fast storlek** i motsats toohello standard dynamiskt allokerade alternativet när du skapar hello disk.
* Azure stöder endast generering 1 för virtuella datorer. Du kan konvertera en virtuell dator i generation 1 från VHDX toohello VHD-filformatet och dynamiskt expanderande tooa fast storlek disk. Du kan inte ändra en virtuell dator generation. Mer information finns i [bör jag skapa en virtuell dator i generation 1 eller 2 i Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* hello maximala storleken som tillåts för hello VHD är 1,023 GB.
* När du installerar hello Linux-operativsystem, rekommenderar vi att du använder standard partitioner i stället för logisk volym Manager (LVM), vilket ofta är hello standard för många installationer. Denna praxis undviker LVM står i konflikt med den klonade virtuella datorer, särskilt om du skulle behöva tooattach ett operativsystem disk tooanother identiska virtuella för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.
* Kernel-stöd för att montera Universal Disk (UDF) filsystem krävs. Vid första start på Azure, hello UDF-formaterad media som är skickar bifogade toohello gäst hello etablering configuration toohello virtuell Linux-dator. hello Azure Linux-agenten måste kunna toomount hello UDF filen system tooread dess konfiguration och etablera hello virtuell dator.
* Hello Linux kernel-versioner som är äldre än 2.6.37 stöder inte icke-enhetlig minnesåtkomst (NUMA) på Hyper-V med större storlekar för virtuella datorer. I detta fall främst påverkar äldre distributioner använder hello uppströms Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504). System som kör anpassade kärnor som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange hello `numa=off` starta parametern på kommandoraden för hello kernel i grub.conf. Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Konfigurera inte en byte-partition på hello operativsystemdisken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello följande steg.
* Alla virtuella hårddiskar måste ha storlekar som multiplar av 1 MB.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Förbered en RHEL 6 virtuell dator från Hyper-V Manager

1. Välj hello virtuell dator i Hyper-V Manager.

2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.

3. I RHEL 6 kan NetworkManager störa hello Azure Linux-agenten. Avinstallera paketet genom att köra följande kommando hello:
   
        # sudo rpm -e --nodeps NetworkManager

4. Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. Dessa regler orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # sudo chkconfig network on

8. Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo ändringen öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.
    
    Dessutom rekommenderar vi att du tar bort hello följande parametrar:
    
        rhgb quiet crashkernel=auto
    
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.  Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer. Den här konfigurationen kan vara problematiskt på mindre storlekar för virtuella datorer.

    >[!Important]
    RHEL 6.5 och tidigare måste också ange hello `numa=off` kernel-parametern. Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

11. Se till att hello secure shell (SSH)-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard. Ändra /etc/ssh/sshd_config tooinclude hello följande rad:

        ClientAliveInterval 180

12. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    Installera hello WALinuxAgent paketet tar bort hello NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort i steg 3.

13. Skapa inte växlingsutrymme på hello operativsystemdisken.

    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera lokala resursen är en tillfällig disk och att den kan tömmas när hello virtuella avetableras hello. Ändra hello följande parametrar i /etc/waagent.conf på rätt sätt när du har installerat hello Azure Linux-agenten i hello förra steget:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:

        # sudo subscription-manager unregister

15. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. Klicka på **åtgärd** > **Stäng** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Förbered en RHEL 7 virtuell dator från Hyper-V Manager

1. Välj hello virtuell dator i Hyper-V Manager.

2. Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.

3. Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # sudo chkconfig network on

6. Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo ändringen öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter. Exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Den här konfigurationen ger också inaktiverar hello nya RHEL 7 namnkonventioner för nätverkskort. Vi rekommenderar dessutom att du tar bort hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.

8. När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard. Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:

        ClientAliveInterval 180

10. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. Skapa inte växlingsutrymme på hello operativsystemdisken.

    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade. När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Om du vill toounregister hello prenumeration kör hello följande kommando:

        # sudo subscription-manager unregister

14. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Klicka på **åtgärd** > **Stäng** i Hyper-V Manager. Din Linux VHD är nu redo toobe upp tooAzure.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Förbered en Red Hat-baserad virtuell dator från KVM
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>Förbered en virtuell dator för RHEL 6 från KVM

1. Hämta hello KVM bild av RHEL 6 från hello Red Hat-webbplatsen.

2. Ange ett rotlösenord.

    Generera ett krypterat lösenord och kopiera hello utdata från hello-kommandot:

        # openssl passwd -1 changeme

    Ange en rotlösenordet med guestfish:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Ändra hello andra fält av hello rotanvändaren från ””! toohello krypterat lösenord.

3. Skapa en virtuell dator i KVM från hello qcow2 avbildning. Ange hello disktyp för**qcow2**, och ange hello virtuellt nätverk gränssnittet enhetsmodell för**virtio**. Sedan starta hello virtuella datorn och logga in som rot.

4. Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. Dessa regler orsaka problem när du klonar en virtuell dator i Azure eller Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # chkconfig network on

8. Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo den här konfigurationen, öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.
    
    Vi rekommenderar dessutom att du tar bort hello följande parametrar:
    
        rhgb quiet crashkernel=auto
    
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.

    >[!Important]
    RHEL 6.5 och tidigare måste också ange hello `numa=off` kernel-parametern. Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

10. Lägg till Hyper-V-moduler tooinitramfs:  

    Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Återskapa initramfs:

        # dracut -f -v

11. Avinstallera molnet init:

        # yum remove cloud-init

12. Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas:

        # chkconfig sshd on

    Ändra /etc/ssh/sshd_config tooinclude hello följande rader:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # yum install WALinuxAgent

        # chkconfig waagent on

15. hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade. När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i **/etc/waagent.conf** på rätt sätt:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:

        # subscription-manager unregister

17. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Stäng hello virtuell dator i KVM.

19. Konvertera hello qcow2 bild toohello VHD-format.

    Först konvertera hello bildformat tooraw:

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB. Avrunda annars hello storlek tooalign med 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertera hello rådata disk tooa fast storlek VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>Förbered en virtuell dator för RHEL 7 från KVM

1. Hämta hello KVM bild av RHEL 7 från hello Red Hat-webbplatsen. Den här proceduren använder RHEL 7 hello exempel.

2. Ange ett rotlösenord.

    Generera ett krypterat lösenord och kopiera hello utdata från hello-kommandot:

        # openssl passwd -1 changeme

    Ange en rotlösenordet med guestfish:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Ändra hello andra fält av rotanvändaren från ””! toohello krypterat lösenord.

3. Skapa en virtuell dator i KVM från hello qcow2 avbildning. Ange hello disktyp för**qcow2**, och ange hello virtuellt nätverk gränssnittet enhetsmodell för**virtio**. Sedan starta hello virtuella datorn och logga in som rot.

4. Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # chkconfig network on

7. Registrera din Red Hat prenumeration tooenable installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo den här konfigurationen, öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter. Exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Detta kommando säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. hello kommando inaktiverar också på hello nya RHEL 7 namnkonventioner för nätverkskort. Vi rekommenderar dessutom att du tar bort hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.

9. När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Lägg till Hyper-V-modulerna i initramfs.

    Redigera `/etc/dracut.conf` och lägga till innehåll:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Återskapa initramfs:

        # dracut -f -v

11. Avinstallera molnet init:

        # yum remove cloud-init

12. Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas:

        # systemctl enable sshd

    Ändra /etc/ssh/sshd_config tooinclude hello följande rader:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # yum install WALinuxAgent

    Aktivera hello waagent-tjänsten:

        # systemctl enable waagent.service

15. Skapa inte växlingsutrymme på hello operativsystemdisken.

    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade. När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:

        # subscription-manager unregister

17. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Stäng hello virtuell dator i KVM.

19. Konvertera hello qcow2 bild toohello VHD-format.

    Först konvertera hello bildformat tooraw:

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB. Avrunda annars hello storlek tooalign med 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertera hello rådata disk tooa fast storlek VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Förbered en Red Hat-baserad virtuell dator från VMware
### <a name="prerequisites"></a>Krav
Det här avsnittet förutsätter att du redan har installerat en RHEL virtuell dator i VMware. Mer information om hur tooinstall ett operativsystem i VMware, se [VMware gäst operativsystemet installationsguiden](http://partnerweb.vmware.com/GOSIG/home.html).

* När du installerar hello Linux-operativsystem, rekommenderar vi att du använder standard partitioner i stället för LVM, vilket ofta är hello standard för många installationer. Detta undviker LVM står i konflikt med den klonade virtuella datorn, särskilt om en operativsystemdisk behövs toobe kopplade tooanother virtuell dator för felsökning. LVM eller RAID kan användas på datadiskar om önskade.
* Konfigurera inte en byte-partition på hello operativsystemdisken. Du kan konfigurera hello Linux-agenten toocreate en växlingsfil på hello temporär disk. Du hittar mer information om detta i hello steg som följer.
* När du skapar hello virtuell hårddisk, Välj **Store virtuell disk som en enda fil**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>Förbered en virtuell dator för RHEL 6 från VMware
1. I RHEL 6 kan NetworkManager störa hello Azure Linux-agenten. Avinstallera paketet genom att köra följande kommando hello:
   
        # sudo rpm -e --nodeps NetworkManager

2. Skapa en fil med namnet **nätverk** hello i hello/etc/sysconfig/katalog som innehåller följande text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt. Dessa regler orsaka problem när du klonar en virtuell dator i Azure eller Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # sudo chkconfig network on

6. Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo detta, öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter. Exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Vi rekommenderar dessutom att du tar bort hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.

9. Lägg till Hyper-V-moduler tooinitramfs:

    Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Återskapa initramfs:

        # dracut -f -v

10. Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard. Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:

    ClientAliveInterval 180

11. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. Skapa inte växlingsutrymme på hello operativsystemdisken.

    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade. När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:

        # sudo subscription-manager unregister

14. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Stäng hello virtuella datorn och konvertera hello VMDK-filen tooa VHD-filen.

    Först konvertera hello bildformat tooraw:

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB. Avrunda annars hello storlek tooalign med 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertera hello rådata disk tooa fast storlek VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Förbered en virtuell dator för RHEL 7 från VMware
1. Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:

        # sudo chkconfig network on

4. Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure. toodo ändringen öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter. Exempel:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Den här konfigurationen säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem. Även stängs av hello nya RHEL 7 namnkonventioner för nätverkskort. Vi rekommenderar dessutom att du tar bort hello följande parametrar:
   
        rhgb quiet crashkernel=auto
   
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas. Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.

6. När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Lägg till Hyper-V-moduler tooinitramfs.

    Redigera `/etc/dracut.conf`, lägga till innehåll:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Återskapa initramfs:

        # dracut -f -v

8. Se till att hello SSH-server är installerat och konfigurerat toostart vid start. Den här inställningen är vanligtvis hello standard. Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:

        ClientAliveInterval 180

9. Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen. Aktivera hello tillägg databasen genom att köra följande kommando hello:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installera hello Azure Linux-agenten genom att köra följande kommando hello:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. Skapa inte växlingsutrymme på hello operativsystemdisken.

    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure. Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade. När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. Om du vill toounregister hello prenumeration kör hello följande kommando:

        # sudo subscription-manager unregister

13. Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. Stäng hello virtuella datorn och konvertera hello VMDK-filen toohello VHD-format.

    Först konvertera hello bildformat tooraw:

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB. Avrunda annars hello storlek tooalign med 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Konvertera hello rådata disk tooa fast storlek VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Förbereda en Red Hat-baserad virtuell dator från en ISO med hjälp av en kickstart fil automatiskt
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Förbered en RHEL 7 virtuell dator från en kickstart-fil

1.  Skapa en kickstart-fil som innehåller hello efter innehåll och spara hello-filen. Mer information om installation av kickstart finns hello [Kickstart installationsguiden](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run hello Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear hello MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down hello machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable hello root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set hello cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build hello grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. Placera hello kickstart fil där hello installationen system kan komma åt den.

3. Skapa en ny virtuell dator i Hyper-V Manager. På hello **Anslut virtuell hårddisk** väljer **koppla en virtuell hårddisk senare**, och fullständig hello guiden Ny virtuell dator.

4. Öppna hello inställningar för virtuell dator:

    a.  Koppla en ny virtuell hårddisk toohello virtuell dator. Se till att tooselect **VHD-Format** och **med fast storlek**.

    b.  Koppla hello installationen ISO toohello DVD-enhet.

    c.  Ange hello BIOS tooboot från CD: N.

5. Starta hello virtuell dator. När installationsguiden för hello visas trycker **fliken** tooconfigure hello startalternativ.

6. Ange `inst.ks=<hello location of hello kickstart file>` hello slutet av hello startalternativ och tryck på **RETUR**.

7. Vänta tills hello installation toofinish. När den är klar, stängs hello virtuella datorn automatiskt. Din Linux VHD är nu redo toobe upp tooAzure.

## <a name="known-issues"></a>Kända problem
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>hello Hyper-V-drivrutinen kan inte ingå i hello inledande RAM-disk när du använder en icke-Hyper-V hypervisor-program

I vissa fall kanske Linux installationsprogram inte innehåller hello drivrutiner för Hyper-V i hello inledande RAMDISK (initrd eller initramfs) såvida inte Linux upptäcker att den körs i en miljö med Hyper-V.

När du använder en annan virtualisering system (det vill säga Virtualbox, Xen o.s.v.) tooprepare Linux avbildningen kan du behöva toorebuild initrd tooensure som minst hello hv_vmbus och hv_storvsc kernel-moduler är tillgängliga på hello inledande RAM-disk. Detta är ett känt problem minst på system som är baserade på hello överordnade Red Hat-distribution.

tooresolve problemet, lägga till Hyper-V-moduler tooinitramfs och återskapa den:

Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

Återskapa initramfs:

        # dracut -f -v

Mer information finns i hello information [återskapa initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Nästa steg
Du är nu redo toouse Red Hat Enterprise Linux virtuell hårddisk toocreate nya virtuella datorer i Azure. Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

För mer information om hello hypervisorer som är certifierade toorun Red Hat Enterprise Linux, se [hello Red Hat webbplats](https://access.redhat.com/certified-hypervisors).
