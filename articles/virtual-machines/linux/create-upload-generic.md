---
title: aaaCreate och ladda upp en Linux-VHD i Azure
description: "Lär dig toocreate och överför en Azure virtuell hårddisk (VHD) som innehåller en Linux-operativsystem."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 208e15c035edb5520fc29ba8e4e71c42b041864d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="information-for-non-endorsed-distributions"></a>Information om icke-godkända distributioner
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

hello Azure-plattformen SLA gäller toovirtual datorer som kör hello Linux OS bara när en av hello [-godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) används. Alla Linux-distributioner som finns angivna i hello Azure avbildningen galleriet är godkända distributioner med hello nödvändig konfiguration.

* [Linux på Azure - godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Stöd för Linux-avbildningar i Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alla distributioner som körs på Azure måste toomeet ett antal krav toohave en chans tooproperly köras på hello-plattformar.  Den här artikeln är inte menat omfattande som varje distribution är olika. och det är fullt möjligt att även om du uppfyller alla hello villkoren nedan du måste fortfarande toosignificantly justera tooensure ditt Linux-system som körs korrekt på hello-plattformen.

Det är därför som vi rekommenderar att du börjar med en av våra [Linux på Azure-godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) när det är möjligt. hello följande artiklar får du stegvisa anvisningar hur tooprepare hello olika godkända Linux-distributioner som stöds i Azure:

* **[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

hello resten av den här artikeln fokuserar på allmänna riktlinjer för att köra Linux-distribution på Azure.

## <a name="general-linux-installation-notes"></a>Allmän Linux installationsinformation
* Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.  Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet. Om du använder VirtualBox innebär detta att välja **fast storlek** som motsats toohello standard dynamiskt allokerade när du skapar hello disk.
* Azure har endast stöd för virtuella datorer i generation 1. Du kan konvertera en generation 1 virtuell dator från VHDX toohello VHD-filformatet och dynamiskt expanderande tooa fast storlek disk. Men du kan inte ändra en virtuell dator generation. Mer information finns i [bör jag skapa en virtuell dator i generation 1 eller 2 i Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* Maximal storlek för hello tillåts för hello VHD 1,023 GB.
* När du installerar hello Linux-system är *rekommenderas* att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer). På så sätt undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk behövs toobe kopplade tooanother identiska VM för felsökning. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.
* Kernel-stöd för att montera UDF-filsystem krävs. Vid första start på Azure hello skickas etablering configuration toohello Linux VM via UDF-formaterad media som är bifogade toohello gäst. hello Azure Linux-agenten måste kunna toomount hello UDF filen system tooread dess konfiguration och etablera hello VM.
* Linux kernel-versioner än 2.6.37 stöder inte NUMA på Hyper-V med större VM-storlekar. I detta fall främst påverkar äldre distributioner med hello uppströms Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504). System som kör anpassade kernlar som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange hello Start parametern `numa=off` på hello kernel kommandoradsverktyg i grub.conf. Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Konfigurera inte en byte-partition på hello OS-disken. hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.  Mer information om detta finns i hello stegen nedan.
* Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.

### <a name="installing-kernel-modules-without-hyper-v"></a>Installera kernel moduler utan Hyper-V
Azure körs på hello Hyper-V hypervisor-programmet så Linux kräver att vissa kernel-moduler är installerade i ordning toorun i Azure. Om du har en virtuell dator som har skapats utanför Hyper-V hello Linux installationsprogram får inte innehålla hello drivrutiner för Hyper-V i hello inledande ramdisk (initrd eller initramfs) om den identifierar att den körs en Hyper-V-miljö. När du använder en annan virtualisering system (d.v.s. Virtualbox, KVM o.s.v.) tooprepare Linux avbildningen måste du kanske måste toorebuild hello initrd tooensure minst hello `hv_vmbus` och `hv_storvsc` kernel-moduler är tillgängliga på hello inledande ramdisk.  Detta är ett känt problem minst på system baserat på hello överordnade Red Hat-distribution.

hello mekanism för att återskapa hello initrd eller initramfs avbildningen kan variera beroende på hello-distribution. Dokumentationen för din distribution eller stöd för hello rätt proceduren.  Här är ett exempel på hur toorebuild hello initrd med hello `mkinitrd` verktyget:

Säkerhetskopiera först hello befintliga initrd avbildningen:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Därefter återskapa hello initrd med hello `hv_vmbus` och `hv_storvsc` kernel moduler:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Ändra storlek på virtuella hårddiskar
VHD-avbildningar i Azure måste ha en virtuell storlek justerad too1MB.  Normalt ska redan virtuella hårddiskar som skapats med hjälp av Hyper-V justeras korrekt.  Om hello VHD inte justeras korrekt och att du får ett fel meddelande liknande toohello följande när du försöker toocreate en *bild* från den virtuella Hårddisken:

    "hello VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. hello size must be a whole number (in MBs).”

tooremedy detta du kan ändra storlek hello VM som använder hello Hyper-V Manager-konsolen eller hello [storleksändring VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-cmdlet.  Om du inte körs i en Windows-miljö är det rekommenderade toouse qemu img tooconvert (om det behövs) och ändra storlek på hello VHD.

> [!NOTE]
> Det finns ett känt fel i qemu img versioner > = 2.2.1 som resulterar i en felaktigt formaterad virtuell Hårddisk. hello problemet är åtgärdat i QEMU 2.6. Det rekommenderas toouse qemu img 2.2.0 eller lägre eller uppdatering too2.6 eller högre. Referens: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Ändra storlek på hello VHD direkt med hjälp av verktyg som `qemu-img` eller `vbox-manage` kan resultera i en VHD som inte kan startas.  Därför rekommenderas toofirst konvertera hello VHD tooa RAW diskavbildning.  Om hello VM-avbildning har redan skapats som RÅDATA diskavbildning (hello standard för vissa hypervisor-program, till exempel KVM) kan du hoppa över det här steget:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Beräkna hello storlek som krävs av hello disk image tooensure som hello virtuell storlek är justerade too1MB.  hello följande bash shell-skript kan hjälpa till med den här.  hello-skript som använder ”`qemu-img info`” toodetermine hello virtuell storlek på hello diskavbildning och beräknar sedan hello storlek toohello nästa 1 MB:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Ändra storlek på hello rådata disk med hjälp av $rounded_size som angetts i hello ovan skript:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Nu kan konvertera hello RÅDATA disk tillbaka tooa VHD med fast storlek:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Eller med qemu version **2.6 +** inkluderar hello `force_size` alternativ:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux-Kernel-krav
hello Linux Integration Services (LIS) drivrutiner för Hyper-V och Azure tillhandahålls direkt toohello överordnade Linux kernel. Många distributioner som innehåller de senaste Linux kernel-version (d.v.s. 3.x) redan har dessa drivrutiner eller annat sätt tillhandahålla anpassats versioner av drivrutinerna med deras kärnor.  De här drivrutinerna som uppdateras kontinuerligt i hello överordnade kernel med nya korrigeringar och funktioner, så när det är möjligt bör toorun en [godkända distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som innehåller dessa korrigeringar och uppdateringar.

Om du använder en variant av Red Hat Enterprise Linux versioner **6.0 6.3**, behöver du tooinstall hello senaste LIS drivrutiner för Hyper-V. hello drivrutinerna finns [på den här platsen](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Från och med RHEL **6.4 +** (och derivat) hello LIS drivrutiner ingår redan i hello kernel och därför inga ytterligare Installationspaketen är nödvändiga toorun dessa system på Azure.

Om en anpassad kernel krävs, är det rekommenderade toouse en nyare version av kernel (d.v.s. **3.8 +**). För dessa distributioner eller leverantörer som hanterar sina egna kernel blir möda nödvändiga tooregularly backport hello LIS drivrutiner från hello överordnade kernel tooyour anpassade kernel.  Även om du redan kör en relativt nya kernel-version, rekommenderas tookeep reda på någon uppströms åtgärdas i hello LIS drivrutiner och backport dem efter behov. hello hello LIS drivrutinens källfiler finns tillgängliga i hello [sköter underhåll SJÄLVA](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) filen i hello Linux kernel källa trädet:

    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/

Vid en mycket minsta hello frånvaron av hello följande korrigeringsfiler har rapporterats toocause problem på Azure och så de måste inkluderas i hello kernel. Den här listan är inte menat fullständig eller fullständig för alla distributioner:

* [ata_piix: skjuta upp diskar toohello Hyper-V-drivrutiner som standard](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: kontot för transit paket i hello Återställ sökväg](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: undvika användningen av WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: inaktivera skriva samma för RAID och drivrutiner för nätverkskort för virtuell värddator](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL-pekare avreferera-korrigering](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ring buffer fel kan resultera i i/o-låsning](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: skydda dig mot dubbla körning av __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="hello-azure-linux-agent"></a>hello Azure Linux-agenten
Hej [Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (waagent) krävs tooproperly etablera en virtuell Linux-dator i Azure. Du kan hämta hello senaste versionen, filen problem eller skicka pull-förfrågningar på hello [Linux-agenten GitHub-repo-](https://github.com/Azure/WALinuxAgent).

* hello Linux-agenten släpps under hello Apache 2.0-licens. Många distributioner ange redan RPM eller deb paket för hello agent och så i vissa fall detta kan du installera och uppdatera visningsläge.
* hello Azure Linux-agenten kräver Python v2.6 +.
* hello agent kräver också hello python pyasn1 modul. De flesta distributioner ger detta som ett separat paket som kan installeras.
* Hello Azure Linux-agenten kanske inte kompatibelt med NetworkManager i vissa fall. Många av hello RPM/Deb-paket som tillhandahålls av distributioner konfigurera NetworkManager som en konflikt toohello waagent paket och därmed kommer att avinstallera NetworkManager när du installerar hello Linux-agenten.

## <a name="general-linux-system-requirements"></a>Systemkrav för allmänna Linux

* Ändra hello kernel Start rad i GRUB eller GRUB2 tooinclude hello följande parametrar. Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.
  
    Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar om de finns:
  
        rhgb quiet crashkernel=auto
  
    Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport. Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.

* Installera hello Azure Linux-agenten
  
    hello Azure Linux-agenten krävs för att etablera en Linux-avbildning på Azure.  Många distributioner ange hello agent som ett RPM eller Deb (hello paketet kallas vanligtvis 'WALinuxAgent' eller 'walinuxagent').  hello agent kan även installeras manuellt genom att följa stegen hello i hello [Linux-agenten Guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Se till att hello SSH-server är installerat och konfigurerat toostart vid start.  Detta är vanligtvis hello standard.

* Skapa inte växlingsutrymme på hello OS-disk
  
    hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure. Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade. När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

* Kör hello följande kommandon toodeprovision hello virtuell dator som ett sista steg:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > På Virtualbox kan du se följande fel när du har kört hello ' waagent-force - deprovision': `[Errno 5] Input/output error`. Det här felmeddelandet är inte viktigt och kan ignoreras.
  > 
  > 

* Du behöver tooshut ned hello virtuella datorn och därefter överföra hello VHD tooAzure.

