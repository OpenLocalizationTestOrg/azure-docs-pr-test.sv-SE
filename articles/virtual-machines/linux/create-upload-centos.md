---
title: "Skapa och ladda upp en baserat på CentOS Linux VHD i Azure"
description: "Lär dig att skapa och ladda upp en Azure virtuell hårddisk (VHD) som innehåller en baserat på CentOS Linux-operativsystem."
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
ms.openlocfilehash: 010f4b05b35fa1f31c14f34a5fae9298fcd831e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a><span data-ttu-id="ff914-103">Förbered en CentOS-baserad virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="ff914-103">Prepare a CentOS-based virtual machine for Azure</span></span>
* [<span data-ttu-id="ff914-104">Förbered en CentOS 6.x virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="ff914-104">Prepare a CentOS 6.x virtual machine for Azure</span></span>](#centos-6x)
* [<span data-ttu-id="ff914-105">Förbered en CentOS 7.0 + virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="ff914-105">Prepare a CentOS 7.0+ virtual machine for Azure</span></span>](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="ff914-106">Krav</span><span class="sxs-lookup"><span data-stu-id="ff914-106">Prerequisites</span></span>
<span data-ttu-id="ff914-107">Den här artikeln förutsätter att du redan har installerat en CentOS (eller liknande derivat) Linux-operativsystem till en virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ff914-107">This article assumes that you have already installed a CentOS (or similar derivative) Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="ff914-108">Det finns flera verktyg för att skapa VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="ff914-108">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="ff914-109">Instruktioner finns i [installera Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff914-109">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="ff914-110">**CentOS installationsinformation**</span><span class="sxs-lookup"><span data-stu-id="ff914-110">**CentOS installation notes**</span></span>

* <span data-ttu-id="ff914-111">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-111">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="ff914-112">VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="ff914-112">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="ff914-113">Du kan konvertera disken till VHD-format med hjälp av Hyper-V Manager eller cmdlet convert-vhd.</span><span class="sxs-lookup"><span data-stu-id="ff914-113">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span> <span data-ttu-id="ff914-114">Om du använder VirtualBox innebär detta att välja **fast storlek** till skillnad från standardvärdet dynamiskt allokerade när skapas.</span><span class="sxs-lookup"><span data-stu-id="ff914-114">If you are using VirtualBox this means selecting **Fixed size** as opposed to the default dynamically allocated when creating the disk.</span></span>
* <span data-ttu-id="ff914-115">När du installerar Linux-system är *rekommenderas* att du använder standard partitioner i stället för LVM (ofta är standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="ff914-115">When installing the Linux system it is *recommended* that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="ff914-116">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk någonsin måste vara kopplad till en annan identisk virtuell dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="ff914-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another identical VM for troubleshooting.</span></span> <span data-ttu-id="ff914-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.</span><span class="sxs-lookup"><span data-stu-id="ff914-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="ff914-118">Kernel-stöd för att montera UDF-filsystem krävs.</span><span class="sxs-lookup"><span data-stu-id="ff914-118">Kernel support for mounting UDF file systems is required.</span></span> <span data-ttu-id="ff914-119">Vid första start på Azure skickas etablering konfigurationen för Linux-VM via UDF-formaterad media som är anslutna till gästen.</span><span class="sxs-lookup"><span data-stu-id="ff914-119">At first boot on Azure the provisioning configuration is passed to the Linux VM via UDF-formatted media that is attached to the guest.</span></span> <span data-ttu-id="ff914-120">Azure Linux-agenten måste kunna montera UDF-filsystemet för att läsa konfigurationen och etablera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff914-120">The Azure Linux agent must be able to mount the UDF file system to read its configuration and provision the VM.</span></span>
* <span data-ttu-id="ff914-121">Linux kernel-versioner än 2.6.37 stöder inte NUMA på Hyper-V med större VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="ff914-121">Linux kernel versions below 2.6.37 do not support NUMA on Hyper-V with larger VM sizes.</span></span> <span data-ttu-id="ff914-122">Det här problemet påverkar huvudsakligen äldre distributioner med hjälp av den överordnade Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504).</span><span class="sxs-lookup"><span data-stu-id="ff914-122">This issue primarily impacts older distributions using the upstream Red Hat 2.6.32 kernel, and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="ff914-123">System som kör anpassade kernlar som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange parametern Start `numa=off` på kommandoraden i grub.conf kernel.</span><span class="sxs-lookup"><span data-stu-id="ff914-123">Systems running custom kernels older than 2.6.37, or RHEL-based kernels older than 2.6.32-504 must set the boot parameter `numa=off` on the kernel command-line in grub.conf.</span></span> <span data-ttu-id="ff914-124">Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="ff914-124">For more information see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="ff914-125">Konfigurera inte en byte-partition på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="ff914-125">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="ff914-126">Linux-agenten kan konfigureras för att skapa en växlingsfil på tillfällig disken.</span><span class="sxs-lookup"><span data-stu-id="ff914-126">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="ff914-127">Mer information om detta finns i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="ff914-127">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="ff914-128">Alla de virtuella hårddiskarna måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ff914-128">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="centos-6x"></a><span data-ttu-id="ff914-129">CentOS 6.x</span><span class="sxs-lookup"><span data-stu-id="ff914-129">CentOS 6.x</span></span>

1. <span data-ttu-id="ff914-130">Välj den virtuella datorn i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ff914-130">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="ff914-131">Klicka på **Anslut** att öppna ett konsolfönster för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff914-131">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="ff914-132">CentOS 6, kan NetworkManager störa Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="ff914-132">In CentOS 6, NetworkManager can interfere with the Azure Linux agent.</span></span> <span data-ttu-id="ff914-133">Avinstallera paketet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff914-133">Uninstall this package by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="ff914-134">Skapa eller redigera filen `/etc/sysconfig/network` och Lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="ff914-134">Create or edit the file `/etc/sysconfig/network` and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="ff914-135">Skapa eller redigera filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="ff914-135">Create or edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="ff914-136">Ändra udev regler för att undvika att generera statiska regler för Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ff914-136">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="ff914-137">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="ff914-137">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="ff914-138">Kontrollera nätverkstjänsten startar när datorn startas genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ff914-138">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # sudo chkconfig network on

8. <span data-ttu-id="ff914-139">Om du vill använda OpenLogic speglingen som finns i Azure-datacenter och Ersätt den `/etc/yum.repos.d/CentOS-Base.repo` fil med följande databaser.</span><span class="sxs-lookup"><span data-stu-id="ff914-139">If you would like to use the OpenLogic mirrors that are hosted within the Azure datacenters, then replace the `/etc/yum.repos.d/CentOS-Base.repo` file with the following repositories.</span></span>  <span data-ttu-id="ff914-140">Detta lägger också till den **[openlogic]** databasen som innehåller ytterligare paket, till exempel Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="ff914-140">This will also add the **[openlogic]** repository that includes additional packages such as the Azure Linux agent:</span></span>

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
    <span data-ttu-id="ff914-141">Resten av den här guiden kommer anta att du använder minst `[openlogic]` lagringsplatsen som används för att installera Azure Linux-agenten nedan.</span><span class="sxs-lookup"><span data-stu-id="ff914-141">The rest of this guide will assume you are using at least the `[openlogic]` repo, which will be used to install the Azure Linux agent below.</span></span>


9. <span data-ttu-id="ff914-142">Lägg till följande rad i /etc/yum.conf:</span><span class="sxs-lookup"><span data-stu-id="ff914-142">Add the following line to /etc/yum.conf:</span></span>
    
        http_caching=packages

10. <span data-ttu-id="ff914-143">Kör följande kommando för att rensa aktuella yum metadata och uppdatera systemet med de senaste paket:</span><span class="sxs-lookup"><span data-stu-id="ff914-143">Run the following command to clear the current yum metadata and update the system with the latest packages:</span></span>
    
        # yum clean all

    <span data-ttu-id="ff914-144">Om du skapar en avbildning för en äldre version av CentOS, bör uppdatera alla paket till senast:</span><span class="sxs-lookup"><span data-stu-id="ff914-144">Unless you are creating an image for an older version of CentOS, it is recommended to update all the packages to the latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="ff914-145">En omstart kan krävas när du har kört kommandot.</span><span class="sxs-lookup"><span data-stu-id="ff914-145">A reboot may be required after running this command.</span></span>

11. <span data-ttu-id="ff914-146">(Valfritt) Installera drivrutiner för Linux Integration Services (LIS).</span><span class="sxs-lookup"><span data-stu-id="ff914-146">(Optional) Install the drivers for the Linux Integration Services (LIS).</span></span>
   
    >[!IMPORTANT]
    <span data-ttu-id="ff914-147">Steget är **krävs** för CentOS 6.3 och tidigare och en valfri för senare versioner.</span><span class="sxs-lookup"><span data-stu-id="ff914-147">The step is **required** for CentOS 6.3 and earlier, and optional for later releases.</span></span>

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    <span data-ttu-id="ff914-148">Du kan också följa instruktioner för manuell installation på den [LIS hämtningssidan](https://go.microsoft.com/fwlink/?linkid=403033) att installera RPM på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff914-148">Alternatively, you can follow the manual installation instructions on the [LIS download page](https://go.microsoft.com/fwlink/?linkid=403033) to install the RPM onto your VM.</span></span>
 
12. <span data-ttu-id="ff914-149">Installera Azure Linux-agenten och beroenden:</span><span class="sxs-lookup"><span data-stu-id="ff914-149">Install the Azure Linux Agent and dependencies:</span></span>
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    <span data-ttu-id="ff914-150">Paketets WALinuxAgent tar bort NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 3.</span><span class="sxs-lookup"><span data-stu-id="ff914-150">The WALinuxAgent package will remove the NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 3.</span></span>


13. <span data-ttu-id="ff914-151">Ändra raden kernel Start i grub konfigurationen så att ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-151">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="ff914-152">Det gör du genom att öppna `/boot/grub/menu.lst` i en textredigerare och se till att standard-kärnan innehåller följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ff914-152">To do this, open `/boot/grub/menu.lst` in a text editor and ensure that the default kernel includes the following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="ff914-153">Detta säkerställer också att alla konsolmeddelanden skickas till den första seriella porten som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ff914-153">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="ff914-154">Förutom de som nämns ovan, rekommenderas att *ta bort* följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ff914-154">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="ff914-155">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla loggar som ska skickas till den seriella porten.</span><span class="sxs-lookup"><span data-stu-id="ff914-155">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>  <span data-ttu-id="ff914-156">Den `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar mängden tillgängligt minne på den virtuella datorn med 128 MB eller mer, som kan vara problematiskt på de virtuella datorn är mindre.</span><span class="sxs-lookup"><span data-stu-id="ff914-156">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>

    >[!Important]
    <span data-ttu-id="ff914-157">CentOS 6.5 och tidigare måste också ange parametern kernel `numa=off`.</span><span class="sxs-lookup"><span data-stu-id="ff914-157">CentOS 6.5 and earlier must also set the kernel parameter `numa=off`.</span></span> <span data-ttu-id="ff914-158">Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="ff914-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

14. <span data-ttu-id="ff914-159">Se till att SSH-server har installerats och konfigurerats för att starta när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="ff914-159">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="ff914-160">Detta är vanligtvis standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="ff914-160">This is usually the default.</span></span>

15. <span data-ttu-id="ff914-161">Skapa inte växlingsutrymme på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="ff914-161">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="ff914-162">Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med den lokala resurs disken som är kopplad till den virtuella datorn när du har etablerat på Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-162">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="ff914-163">Observera att den lokala resurs disken är en *tillfälliga* disk och kan tömmas när den virtuella datorn avetableras.</span><span class="sxs-lookup"><span data-stu-id="ff914-163">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="ff914-164">När du har installerat Azure Linux-agenten (se föregående steg), ändra följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ff914-164">After installing the Azure Linux Agent (see previous step), modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. <span data-ttu-id="ff914-165">Kör följande kommandon för att ta bort etableringen av den virtuella datorn och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ff914-165">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. <span data-ttu-id="ff914-166">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ff914-166">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="ff914-167">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-167">Your Linux VHD is now ready to be uploaded to Azure.</span></span>


- - -
## <a name="centos-70"></a><span data-ttu-id="ff914-168">CentOS 7.0 +</span><span class="sxs-lookup"><span data-stu-id="ff914-168">CentOS 7.0+</span></span>
<span data-ttu-id="ff914-169">**Ändringar i CentOS 7 (och liknande produkter)**</span><span class="sxs-lookup"><span data-stu-id="ff914-169">**Changes in CentOS 7 (and similar derivatives)**</span></span>

<span data-ttu-id="ff914-170">Förbereda en virtuell dator för CentOS 7 för Azure liknar CentOS 6, men det finns flera viktiga skillnader noteras:</span><span class="sxs-lookup"><span data-stu-id="ff914-170">Preparing a CentOS 7 virtual machine for Azure is very similar to CentOS 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="ff914-171">NetworkManager paketet inte längre i konflikt med Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="ff914-171">The NetworkManager package no longer conflicts with the Azure Linux agent.</span></span> <span data-ttu-id="ff914-172">Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="ff914-172">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="ff914-173">GRUB2 används nu som standard-startprogrammet så proceduren för att redigera kernel parametrar har ändrats (se nedan).</span><span class="sxs-lookup"><span data-stu-id="ff914-173">GRUB2 is now used as the default bootloader, so the procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="ff914-174">XFS är nu filsystemet.</span><span class="sxs-lookup"><span data-stu-id="ff914-174">XFS is now the default file system.</span></span> <span data-ttu-id="ff914-175">Filsystemet ext4 kan fortfarande användas om du vill.</span><span class="sxs-lookup"><span data-stu-id="ff914-175">The ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="ff914-176">**Konfigurationssteg**</span><span class="sxs-lookup"><span data-stu-id="ff914-176">**Configuration Steps**</span></span>

1. <span data-ttu-id="ff914-177">Välj den virtuella datorn i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ff914-177">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="ff914-178">Klicka på **Anslut** att öppna ett konsolfönster för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff914-178">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="ff914-179">Skapa eller redigera filen `/etc/sysconfig/network` och Lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="ff914-179">Create or edit the file `/etc/sysconfig/network` and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="ff914-180">Skapa eller redigera filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till följande text:</span><span class="sxs-lookup"><span data-stu-id="ff914-180">Create or edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="ff914-181">Ändra udev regler för att undvika att generera statiska regler för Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ff914-181">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="ff914-182">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="ff914-182">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. <span data-ttu-id="ff914-183">Om du vill använda OpenLogic speglingen som finns i Azure-datacenter och Ersätt den `/etc/yum.repos.d/CentOS-Base.repo` fil med följande databaser.</span><span class="sxs-lookup"><span data-stu-id="ff914-183">If you would like to use the OpenLogic mirrors that are hosted within the Azure datacenters, then replace the `/etc/yum.repos.d/CentOS-Base.repo` file with the following repositories.</span></span>  <span data-ttu-id="ff914-184">Detta lägger också till den **[openlogic]** databasen som innehåller paket för Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="ff914-184">This will also add the **[openlogic]** repository that includes packages for the Azure Linux agent:</span></span>
   
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
    <span data-ttu-id="ff914-185">Resten av den här guiden kommer anta att du använder minst `[openlogic]` lagringsplatsen som används för att installera Azure Linux-agenten nedan.</span><span class="sxs-lookup"><span data-stu-id="ff914-185">The rest of this guide will assume you are using at least the `[openlogic]` repo, which will be used to install the Azure Linux agent below.</span></span>

7. <span data-ttu-id="ff914-186">Kör följande kommando för att rensa aktuella yum metadata och installera uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="ff914-186">Run the following command to clear the current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all

    <span data-ttu-id="ff914-187">Om du skapar en avbildning för en äldre version av CentOS, bör uppdatera alla paket till senast:</span><span class="sxs-lookup"><span data-stu-id="ff914-187">Unless you are creating an image for an older version of CentOS, it is recommended to update all the packages to the latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="ff914-188">En omstart krävs kanske köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="ff914-188">A reboot maybe required after running this command.</span></span>

8. <span data-ttu-id="ff914-189">Ändra raden kernel Start i grub konfigurationen så att ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-189">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="ff914-190">Det gör du genom att öppna `/etc/default/grub` i en textredigerare och redigera den `GRUB_CMDLINE_LINUX` parameter, till exempel:</span><span class="sxs-lookup"><span data-stu-id="ff914-190">To do this, open `/etc/default/grub` in a text editor and edit the `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="ff914-191">Detta säkerställer också att alla konsolmeddelanden skickas till den första seriella porten som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ff914-191">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="ff914-192">Även stängs av nya CentOS 7 namnkonventionerna för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ff914-192">It also turns off the new CentOS 7 naming conventions for NICs.</span></span> <span data-ttu-id="ff914-193">Förutom de som nämns ovan, rekommenderas att *ta bort* följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ff914-193">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="ff914-194">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla loggar som ska skickas till den seriella porten.</span><span class="sxs-lookup"><span data-stu-id="ff914-194">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="ff914-195">Den `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar mängden tillgängligt minne på den virtuella datorn med 128 MB eller mer, som kan vara problematiskt på de virtuella datorn är mindre.</span><span class="sxs-lookup"><span data-stu-id="ff914-195">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>

9. <span data-ttu-id="ff914-196">När du är klar redigera `/etc/default/grub` per ovan, kör du följande kommando för att återskapa grub konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="ff914-196">Once you are done editing `/etc/default/grub` per above, run the following command to rebuild the grub configuration:</span></span>
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="ff914-197">Om du skapar bilden från **VMWare, VirtualBox eller KVM:** Kontrollera Hyper-V-drivrutiner som ingår i initramfs:</span><span class="sxs-lookup"><span data-stu-id="ff914-197">If building the image from **VMWare, VirtualBox or KVM:** Ensure the Hyper-V drivers are included in the initramfs:</span></span>
   
   <span data-ttu-id="ff914-198">Redigera `/etc/dracut.conf`, lägga till innehåll:</span><span class="sxs-lookup"><span data-stu-id="ff914-198">Edit `/etc/dracut.conf`, add content:</span></span>
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   <span data-ttu-id="ff914-199">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ff914-199">Rebuild the initramfs:</span></span>
   
        # sudo dracut –f -v

11. <span data-ttu-id="ff914-200">Installera Azure Linux-agenten och beroenden:</span><span class="sxs-lookup"><span data-stu-id="ff914-200">Install the Azure Linux Agent and dependencies:</span></span>

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. <span data-ttu-id="ff914-201">Skapa inte växlingsutrymme på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="ff914-201">Do not create swap space on the OS disk.</span></span>
   
   <span data-ttu-id="ff914-202">Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med den lokala resurs disken som är kopplad till den virtuella datorn när du har etablerat på Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-202">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="ff914-203">Observera att den lokala resurs disken är en *tillfälliga* disk och kan tömmas när den virtuella datorn avetableras.</span><span class="sxs-lookup"><span data-stu-id="ff914-203">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="ff914-204">När du har installerat Azure Linux-agenten (se föregående steg), ändra följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ff914-204">After installing the Azure Linux Agent (see previous step), modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. <span data-ttu-id="ff914-205">Kör följande kommandon för att ta bort etableringen av den virtuella datorn och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ff914-205">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. <span data-ttu-id="ff914-206">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ff914-206">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="ff914-207">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-207">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff914-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff914-208">Next steps</span></span>
<span data-ttu-id="ff914-209">Nu är du redo att använda din virtuella hårddisk CentOS Linux för att skapa nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="ff914-209">You're now ready to use your CentOS Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="ff914-210">Om att du överföra VHD-filen till Azure finns i steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff914-210">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

