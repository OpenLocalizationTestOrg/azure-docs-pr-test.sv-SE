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
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a><span data-ttu-id="c5c36-103">Förbered en CentOS-baserad virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="c5c36-103">Prepare a CentOS-based virtual machine for Azure</span></span>
* [<span data-ttu-id="c5c36-104">Förbered en CentOS 6.x virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="c5c36-104">Prepare a CentOS 6.x virtual machine for Azure</span></span>](#centos-6x)
* [<span data-ttu-id="c5c36-105">Förbered en CentOS 7.0 + virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="c5c36-105">Prepare a CentOS 7.0+ virtual machine for Azure</span></span>](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="c5c36-106">Krav</span><span class="sxs-lookup"><span data-stu-id="c5c36-106">Prerequisites</span></span>
<span data-ttu-id="c5c36-107">Den här artikeln förutsätter att du redan har installerat en CentOS (eller liknande derivat) Linux operativsystem tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="c5c36-107">This article assumes that you have already installed a CentOS (or similar derivative) Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="c5c36-108">Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c5c36-108">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="c5c36-109">Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5c36-109">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="c5c36-110">**CentOS installationsinformation**</span><span class="sxs-lookup"><span data-stu-id="c5c36-110">**CentOS installation notes**</span></span>

* <span data-ttu-id="c5c36-111">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-111">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="c5c36-112">Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="c5c36-112">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="c5c36-113">Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c5c36-113">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span> <span data-ttu-id="c5c36-114">Om du använder VirtualBox innebär detta att välja **fast storlek** som motsats toohello standard dynamiskt allokerade när du skapar hello disk.</span><span class="sxs-lookup"><span data-stu-id="c5c36-114">If you are using VirtualBox this means selecting **Fixed size** as opposed toohello default dynamically allocated when creating hello disk.</span></span>
* <span data-ttu-id="c5c36-115">När du installerar hello Linux-system är *rekommenderas* att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="c5c36-115">When installing hello Linux system it is *recommended* that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="c5c36-116">På så sätt undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk behövs toobe kopplade tooanother identiska VM för felsökning.</span><span class="sxs-lookup"><span data-stu-id="c5c36-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother identical VM for troubleshooting.</span></span> <span data-ttu-id="c5c36-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.</span><span class="sxs-lookup"><span data-stu-id="c5c36-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="c5c36-118">Kernel-stöd för att montera UDF-filsystem krävs.</span><span class="sxs-lookup"><span data-stu-id="c5c36-118">Kernel support for mounting UDF file systems is required.</span></span> <span data-ttu-id="c5c36-119">Vid första start på Azure hello skickas etablering configuration toohello Linux VM via UDF-formaterad media som är bifogade toohello gäst.</span><span class="sxs-lookup"><span data-stu-id="c5c36-119">At first boot on Azure hello provisioning configuration is passed toohello Linux VM via UDF-formatted media that is attached toohello guest.</span></span> <span data-ttu-id="c5c36-120">hello Azure Linux-agenten måste kunna toomount hello UDF filen system tooread dess konfiguration och etablera hello VM.</span><span class="sxs-lookup"><span data-stu-id="c5c36-120">hello Azure Linux agent must be able toomount hello UDF file system tooread its configuration and provision hello VM.</span></span>
* <span data-ttu-id="c5c36-121">Linux kernel-versioner än 2.6.37 stöder inte NUMA på Hyper-V med större VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="c5c36-121">Linux kernel versions below 2.6.37 do not support NUMA on Hyper-V with larger VM sizes.</span></span> <span data-ttu-id="c5c36-122">I detta fall främst påverkar äldre distributioner med hello uppströms Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504).</span><span class="sxs-lookup"><span data-stu-id="c5c36-122">This issue primarily impacts older distributions using hello upstream Red Hat 2.6.32 kernel, and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="c5c36-123">System som kör anpassade kernlar som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange hello Start parametern `numa=off` på hello kernel kommandoradsverktyg i grub.conf.</span><span class="sxs-lookup"><span data-stu-id="c5c36-123">Systems running custom kernels older than 2.6.37, or RHEL-based kernels older than 2.6.32-504 must set hello boot parameter `numa=off` on hello kernel command-line in grub.conf.</span></span> <span data-ttu-id="c5c36-124">Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="c5c36-124">For more information see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="c5c36-125">Konfigurera inte en byte-partition på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="c5c36-125">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="c5c36-126">hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="c5c36-126">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="c5c36-127">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="c5c36-127">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="c5c36-128">Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="c5c36-128">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="centos-6x"></a><span data-ttu-id="c5c36-129">CentOS 6.x</span><span class="sxs-lookup"><span data-stu-id="c5c36-129">CentOS 6.x</span></span>

1. <span data-ttu-id="c5c36-130">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="c5c36-130">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="c5c36-131">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c5c36-131">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="c5c36-132">CentOS 6, kan NetworkManager störa hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="c5c36-132">In CentOS 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="c5c36-133">Avinstallera paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c5c36-133">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="c5c36-134">Skapa eller redigera hello filen `/etc/sysconfig/network` och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="c5c36-134">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="c5c36-135">Skapa eller redigera hello filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="c5c36-135">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="c5c36-136">Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c5c36-136">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="c5c36-137">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="c5c36-137">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="c5c36-138">Kontrollera hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c5c36-138">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on

8. <span data-ttu-id="c5c36-139">Om du vill att toouse hello OpenLogic speglar som finns inom hello Azure-datacenter, Ersätt då hello `/etc/yum.repos.d/CentOS-Base.repo` fil med hello följande databaser.</span><span class="sxs-lookup"><span data-stu-id="c5c36-139">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="c5c36-140">Detta lägger också till hello **[openlogic]** databasen som innehåller ytterligare paket, till exempel hello Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="c5c36-140">This will also add hello **[openlogic]** repository that includes additional packages such as hello Azure Linux agent:</span></span>

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
    <span data-ttu-id="c5c36-141">hello resten av den här guiden kommer anta att du använder minst hello `[openlogic]` lagringsplatsen som kommer att använda tooinstall hello Azure Linux-agenten nedan.</span><span class="sxs-lookup"><span data-stu-id="c5c36-141">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>


9. <span data-ttu-id="c5c36-142">Lägg till följande rad too/etc/yum.conf hello:</span><span class="sxs-lookup"><span data-stu-id="c5c36-142">Add hello following line too/etc/yum.conf:</span></span>
    
        http_caching=packages

10. <span data-ttu-id="c5c36-143">Kör hello efter kommandot tooclear hello aktuella yum metadata och uppdateringsfiler hello system med senaste hello-paket:</span><span class="sxs-lookup"><span data-stu-id="c5c36-143">Run hello following command tooclear hello current yum metadata and update hello system with hello latest packages:</span></span>
    
        # yum clean all

    <span data-ttu-id="c5c36-144">Om du skapar en avbildning för en äldre version av CentOS, bör tooupdate alla hello paket toohello senaste:</span><span class="sxs-lookup"><span data-stu-id="c5c36-144">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="c5c36-145">En omstart kan krävas när du har kört kommandot.</span><span class="sxs-lookup"><span data-stu-id="c5c36-145">A reboot may be required after running this command.</span></span>

11. <span data-ttu-id="c5c36-146">(Valfritt) Installera hello drivrutiner för hello Linux Integration Services (LIS).</span><span class="sxs-lookup"><span data-stu-id="c5c36-146">(Optional) Install hello drivers for hello Linux Integration Services (LIS).</span></span>
   
    >[!IMPORTANT]
    <span data-ttu-id="c5c36-147">hello steget är **krävs** för CentOS 6.3 och tidigare och en valfri för senare versioner.</span><span class="sxs-lookup"><span data-stu-id="c5c36-147">hello step is **required** for CentOS 6.3 and earlier, and optional for later releases.</span></span>

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    <span data-ttu-id="c5c36-148">Alternativt, du kan följa hello hello manuell Installationsinstruktioner [LIS hämtningssidan](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c5c36-148">Alternatively, you can follow hello manual installation instructions on hello [LIS download page](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM onto your VM.</span></span>
 
12. <span data-ttu-id="c5c36-149">Installera hello Azure Linux-agenten och beroenden:</span><span class="sxs-lookup"><span data-stu-id="c5c36-149">Install hello Azure Linux Agent and dependencies:</span></span>
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    <span data-ttu-id="c5c36-150">Hej WALinuxAgent paketet tas och bort hello NetworkManager NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 3.</span><span class="sxs-lookup"><span data-stu-id="c5c36-150">hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 3.</span></span>


13. <span data-ttu-id="c5c36-151">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-151">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="c5c36-152">toodo detta, öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="c5c36-152">toodo this, open `/boot/grub/menu.lst` in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="c5c36-153">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="c5c36-153">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="c5c36-154">Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="c5c36-154">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="c5c36-155">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="c5c36-155">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="c5c36-156">Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="c5c36-156">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

    >[!Important]
    <span data-ttu-id="c5c36-157">CentOS 6.5 och tidigare måste också ange hello kernel parametern `numa=off`.</span><span class="sxs-lookup"><span data-stu-id="c5c36-157">CentOS 6.5 and earlier must also set hello kernel parameter `numa=off`.</span></span> <span data-ttu-id="c5c36-158">Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="c5c36-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

14. <span data-ttu-id="c5c36-159">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="c5c36-159">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="c5c36-160">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="c5c36-160">This is usually hello default.</span></span>

15. <span data-ttu-id="c5c36-161">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="c5c36-161">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="c5c36-162">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-162">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="c5c36-163">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="c5c36-163">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="c5c36-164">När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="c5c36-164">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="c5c36-165">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="c5c36-165">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. <span data-ttu-id="c5c36-166">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="c5c36-166">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="c5c36-167">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-167">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


- - -
## <a name="centos-70"></a><span data-ttu-id="c5c36-168">CentOS 7.0 +</span><span class="sxs-lookup"><span data-stu-id="c5c36-168">CentOS 7.0+</span></span>
<span data-ttu-id="c5c36-169">**Ändringar i CentOS 7 (och liknande produkter)**</span><span class="sxs-lookup"><span data-stu-id="c5c36-169">**Changes in CentOS 7 (and similar derivatives)**</span></span>

<span data-ttu-id="c5c36-170">Förbereda en virtuell dator för CentOS 7 för Azure är mycket lik tooCentOS 6, men det finns flera viktiga skillnader noteras:</span><span class="sxs-lookup"><span data-stu-id="c5c36-170">Preparing a CentOS 7 virtual machine for Azure is very similar tooCentOS 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="c5c36-171">Hej NetworkManager paketet inte längre är i konflikt med hello Azure Linux-agent.</span><span class="sxs-lookup"><span data-stu-id="c5c36-171">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="c5c36-172">Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="c5c36-172">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="c5c36-173">GRUB2 används nu som hello standard startprogrammet så hello proceduren för att redigera kernel parametrar har ändrats (se nedan).</span><span class="sxs-lookup"><span data-stu-id="c5c36-173">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="c5c36-174">XFS är nu hello standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="c5c36-174">XFS is now hello default file system.</span></span> <span data-ttu-id="c5c36-175">Hej ext4 filsystem kan fortfarande användas om du vill.</span><span class="sxs-lookup"><span data-stu-id="c5c36-175">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="c5c36-176">**Konfigurationssteg**</span><span class="sxs-lookup"><span data-stu-id="c5c36-176">**Configuration Steps**</span></span>

1. <span data-ttu-id="c5c36-177">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="c5c36-177">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="c5c36-178">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c5c36-178">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="c5c36-179">Skapa eller redigera hello filen `/etc/sysconfig/network` och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="c5c36-179">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="c5c36-180">Skapa eller redigera hello filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="c5c36-180">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="c5c36-181">Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c5c36-181">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="c5c36-182">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="c5c36-182">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. <span data-ttu-id="c5c36-183">Om du vill att toouse hello OpenLogic speglar som finns inom hello Azure-datacenter, Ersätt då hello `/etc/yum.repos.d/CentOS-Base.repo` fil med hello följande databaser.</span><span class="sxs-lookup"><span data-stu-id="c5c36-183">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="c5c36-184">Detta lägger också till hello **[openlogic]** databasen som innehåller paket för hello Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="c5c36-184">This will also add hello **[openlogic]** repository that includes packages for hello Azure Linux agent:</span></span>
   
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
    <span data-ttu-id="c5c36-185">hello resten av den här guiden kommer anta att du använder minst hello `[openlogic]` lagringsplatsen som kommer att använda tooinstall hello Azure Linux-agenten nedan.</span><span class="sxs-lookup"><span data-stu-id="c5c36-185">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>

7. <span data-ttu-id="c5c36-186">Kör följande kommando tooclear hello aktuella yum metadata hello och installera uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="c5c36-186">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all

    <span data-ttu-id="c5c36-187">Om du skapar en avbildning för en äldre version av CentOS, bör tooupdate alla hello paket toohello senaste:</span><span class="sxs-lookup"><span data-stu-id="c5c36-187">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="c5c36-188">En omstart krävs kanske köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="c5c36-188">A reboot maybe required after running this command.</span></span>

8. <span data-ttu-id="c5c36-189">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-189">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="c5c36-190">toodo detta, öppna `/etc/default/grub` i ett text-redigeraren och redigera hello `GRUB_CMDLINE_LINUX` parameter, till exempel:</span><span class="sxs-lookup"><span data-stu-id="c5c36-190">toodo this, open `/etc/default/grub` in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="c5c36-191">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="c5c36-191">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="c5c36-192">Även stängs av hello nya CentOS 7 namnkonventioner för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="c5c36-192">It also turns off hello new CentOS 7 naming conventions for NICs.</span></span> <span data-ttu-id="c5c36-193">Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="c5c36-193">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="c5c36-194">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="c5c36-194">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="c5c36-195">Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="c5c36-195">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

9. <span data-ttu-id="c5c36-196">När du är klar redigera `/etc/default/grub` per ovan, kör hello följande toorebuild hello grub konfiguration:</span><span class="sxs-lookup"><span data-stu-id="c5c36-196">Once you are done editing `/etc/default/grub` per above, run hello following command toorebuild hello grub configuration:</span></span>
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="c5c36-197">Om hello-avbildning från **VMWare, VirtualBox eller KVM:** Kontrollera hello Hyper-V-drivrutiner som ingår i hello initramfs:</span><span class="sxs-lookup"><span data-stu-id="c5c36-197">If building hello image from **VMWare, VirtualBox or KVM:** Ensure hello Hyper-V drivers are included in hello initramfs:</span></span>
   
   <span data-ttu-id="c5c36-198">Redigera `/etc/dracut.conf`, lägga till innehåll:</span><span class="sxs-lookup"><span data-stu-id="c5c36-198">Edit `/etc/dracut.conf`, add content:</span></span>
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   <span data-ttu-id="c5c36-199">Återskapa hello initramfs:</span><span class="sxs-lookup"><span data-stu-id="c5c36-199">Rebuild hello initramfs:</span></span>
   
        # sudo dracut –f -v

11. <span data-ttu-id="c5c36-200">Installera hello Azure Linux-agenten och beroenden:</span><span class="sxs-lookup"><span data-stu-id="c5c36-200">Install hello Azure Linux Agent and dependencies:</span></span>

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. <span data-ttu-id="c5c36-201">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="c5c36-201">Do not create swap space on hello OS disk.</span></span>
   
   <span data-ttu-id="c5c36-202">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-202">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="c5c36-203">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="c5c36-203">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="c5c36-204">När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="c5c36-204">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="c5c36-205">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="c5c36-205">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. <span data-ttu-id="c5c36-206">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="c5c36-206">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="c5c36-207">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-207">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5c36-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5c36-208">Next steps</span></span>
<span data-ttu-id="c5c36-209">Du är nu redo toouse CentOS Linux virtuell hårddisk toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="c5c36-209">You're now ready toouse your CentOS Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="c5c36-210">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c5c36-210">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

