---
title: "Skapa och ladda upp en VHD för Oracle Linux | Microsoft Docs"
description: "Lär dig att skapa och ladda upp en Azure virtuell hårddisk (VHD) som innehåller ett Oracle Linux-operativsystem."
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
ms.openlocfilehash: c631ddf3acf6df7364c03eb4691b78be0493e0d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a><span data-ttu-id="773b7-103">Förbered en virtuell Oracle Linux-dator för Azure</span><span class="sxs-lookup"><span data-stu-id="773b7-103">Prepare an Oracle Linux virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="773b7-104">Krav</span><span class="sxs-lookup"><span data-stu-id="773b7-104">Prerequisites</span></span>
<span data-ttu-id="773b7-105">Den här artikeln förutsätter att du redan har installerat ett Oracle Linux-operativsystem till en virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="773b7-105">This article assumes that you have already installed an Oracle Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="773b7-106">Det finns flera verktyg för att skapa VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="773b7-106">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="773b7-107">Instruktioner finns i [installera Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="773b7-107">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="oracle-linux-installation-notes"></a><span data-ttu-id="773b7-108">Installationsinformation för Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="773b7-108">Oracle Linux installation notes</span></span>
* <span data-ttu-id="773b7-109">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="773b7-110">Oracles Red Hat kompatibel kernel och deras UEK3 (Unbreakable Enterprise Kernel) stöds både i Hyper-V och Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-110">Oracle's Red Hat compatible kernel and their UEK3 (Unbreakable Enterprise Kernel) are both supported on Hyper-V and Azure.</span></span> <span data-ttu-id="773b7-111">För bästa resultat bör kontrollera att uppdatera till den senaste kerneln när du förberedde din Oracle Linux VHD.</span><span class="sxs-lookup"><span data-stu-id="773b7-111">For best results, please be sure to update to the latest kernel while preparing your Oracle Linux VHD.</span></span>
* <span data-ttu-id="773b7-112">Oracles UEK2 stöds inte på Hyper-V och Azure som inte innehåller de nödvändiga drivrutinerna.</span><span class="sxs-lookup"><span data-stu-id="773b7-112">Oracle's UEK2 is not supported on Hyper-V and Azure as it does not include the required drivers.</span></span>
* <span data-ttu-id="773b7-113">VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="773b7-113">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="773b7-114">Du kan konvertera disken till VHD-format med hjälp av Hyper-V Manager eller cmdlet convert-vhd.</span><span class="sxs-lookup"><span data-stu-id="773b7-114">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span>
* <span data-ttu-id="773b7-115">När du installerar Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta är standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="773b7-115">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="773b7-116">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om en OS-disk någonsin måste vara kopplad till en annan virtuell dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="773b7-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="773b7-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="773b7-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="773b7-118">NUMA stöds inte för större VM-storlekar på grund av ett fel i Linux kernel-versioner än 2.6.37.</span><span class="sxs-lookup"><span data-stu-id="773b7-118">NUMA is not supported for larger VM sizes due to a bug in Linux kernel versions below 2.6.37.</span></span> <span data-ttu-id="773b7-119">Det här problemet påverkar huvudsakligen distributioner med hjälp av den överordnade Red Hat 2.6.32 kernel.</span><span class="sxs-lookup"><span data-stu-id="773b7-119">This issue primarily impacts distributions using the upstream Red Hat 2.6.32 kernel.</span></span> <span data-ttu-id="773b7-120">Manuell installation av Azure Linux-agenten (waagent) inaktiverar automatiskt NUMA i GRUB konfigurationen för Linux-kärnan.</span><span class="sxs-lookup"><span data-stu-id="773b7-120">Manual installation of the Azure Linux agent (waagent) will automatically disable NUMA in the GRUB configuration for the Linux kernel.</span></span> <span data-ttu-id="773b7-121">Mer information om detta finns i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="773b7-121">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="773b7-122">Konfigurera inte en byte-partition på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="773b7-122">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="773b7-123">Linux-agenten kan konfigureras för att skapa en växlingsfil på tillfällig disken.</span><span class="sxs-lookup"><span data-stu-id="773b7-123">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="773b7-124">Mer information om detta finns i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="773b7-124">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="773b7-125">Alla de virtuella hårddiskarna måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="773b7-125">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>
* <span data-ttu-id="773b7-126">Se till att den `Addons` databasen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="773b7-126">Make sure that the `Addons` repository is enabled.</span></span> <span data-ttu-id="773b7-127">Redigera filen `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) eller `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), och ändrar du raden `enabled=0` till `enabled=1` under **[ol6_addons]** eller **[ol7_addons]** i den här filen.</span><span class="sxs-lookup"><span data-stu-id="773b7-127">Edit the file `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux ), and change the line `enabled=0` to `enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

## <a name="oracle-linux-64"></a><span data-ttu-id="773b7-128">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="773b7-128">Oracle Linux 6.4+</span></span>
<span data-ttu-id="773b7-129">Du måste slutföra specifika konfigurationssteg i operativsystemet för den virtuella datorn körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-129">You must complete specific configuration steps in the operating system for the virtual machine to run in Azure.</span></span>

1. <span data-ttu-id="773b7-130">I rutan i mitten av Hyper-V Manager, Välj den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="773b7-130">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="773b7-131">Klicka på **Anslut** att öppna fönstret för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="773b7-131">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="773b7-132">Avinstallera NetworkManager genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-132">Uninstall NetworkManager by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager
   
    <span data-ttu-id="773b7-133">**Obs:** om paketet inte har installerats, kommandot misslyckas med felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="773b7-133">**Note:** If the package is not already installed, this command will fail with an error message.</span></span> <span data-ttu-id="773b7-134">Detta är förväntat.</span><span class="sxs-lookup"><span data-stu-id="773b7-134">This is expected.</span></span>
4. <span data-ttu-id="773b7-135">Skapa en fil med namnet **nätverk** i den `/etc/sysconfig/` katalog som innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="773b7-135">Create a file named **network** in the `/etc/sysconfig/` directory that contains the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. <span data-ttu-id="773b7-136">Skapa en fil med namnet **ifcfg eth0** i den `/etc/sysconfig/network-scripts/` katalog som innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="773b7-136">Create a file named **ifcfg-eth0** in the `/etc/sysconfig/network-scripts/` directory that contains the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. <span data-ttu-id="773b7-137">Ändra udev regler för att undvika att generera statiska regler för Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="773b7-137">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="773b7-138">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="773b7-138">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. <span data-ttu-id="773b7-139">Kontrollera nätverkstjänsten startar när datorn startas genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-139">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # chkconfig network on
8. <span data-ttu-id="773b7-140">Installera python-pyasn1 genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-140">Install python-pyasn1 by running the following command:</span></span>
   
        # sudo yum install python-pyasn1
9. <span data-ttu-id="773b7-141">Ändra raden kernel Start i grub konfigurationen så att ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-141">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="773b7-142">Att göra den här öppna ”/ boot/grub/menu.lst” i en textredigerare och se till att standard-kärnan innehåller följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="773b7-142">To do this open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   <span data-ttu-id="773b7-143">Detta säkerställer också att alla konsolmeddelanden skickas till den första seriella porten som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="773b7-143">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="773b7-144">Detta inaktiverar NUMA på grund av ett fel i Oracles Red Hat kompatibel kernel.</span><span class="sxs-lookup"><span data-stu-id="773b7-144">This will disable NUMA due to a bug in Oracle's Red Hat compatible kernel.</span></span>
   
   <span data-ttu-id="773b7-145">Förutom de som nämns ovan, rekommenderas att *ta bort* följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="773b7-145">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
   <span data-ttu-id="773b7-146">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla loggar som ska skickas till den seriella porten.</span><span class="sxs-lookup"><span data-stu-id="773b7-146">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>
   
   <span data-ttu-id="773b7-147">Den `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar mängden tillgängligt minne på den virtuella datorn med 128 MB eller mer, som kan vara problematiskt på de virtuella datorn är mindre.</span><span class="sxs-lookup"><span data-stu-id="773b7-147">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>
10. <span data-ttu-id="773b7-148">Se till att SSH-server har installerats och konfigurerats för att starta när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="773b7-148">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="773b7-149">Detta är vanligtvis standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="773b7-149">This is usually the default.</span></span>
11. <span data-ttu-id="773b7-150">Installera Azure Linux-agenten genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="773b7-150">Install the Azure Linux Agent by running the following command.</span></span> <span data-ttu-id="773b7-151">Den senaste versionen är 2.0.15.</span><span class="sxs-lookup"><span data-stu-id="773b7-151">The latest version is 2.0.15.</span></span>
    
        # sudo yum install WALinuxAgent
    
    <span data-ttu-id="773b7-152">Observera att installera paketet WALinuxAgent tar bort NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 2.</span><span class="sxs-lookup"><span data-stu-id="773b7-152">Note that installing the WALinuxAgent package will remove the NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.</span></span>
12. <span data-ttu-id="773b7-153">Skapa inte växlingsutrymme på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="773b7-153">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="773b7-154">Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med den lokala resurs disken som är kopplad till den virtuella datorn när du har etablerat på Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-154">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="773b7-155">Observera att den lokala resurs disken är en *tillfälliga* disk och kan tömmas när den virtuella datorn avetableras.</span><span class="sxs-lookup"><span data-stu-id="773b7-155">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="773b7-156">När du har installerat Azure Linux-agenten (se föregående steg), ändra följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="773b7-156">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
13. <span data-ttu-id="773b7-157">Kör följande kommandon för att ta bort etableringen av den virtuella datorn och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="773b7-157">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. <span data-ttu-id="773b7-158">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="773b7-158">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="773b7-159">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-159">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

- - -
## <a name="oracle-linux-70"></a><span data-ttu-id="773b7-160">Oracle Linux 7.0 +</span><span class="sxs-lookup"><span data-stu-id="773b7-160">Oracle Linux 7.0+</span></span>
<span data-ttu-id="773b7-161">**Ändringar i Oracle Linux 7**</span><span class="sxs-lookup"><span data-stu-id="773b7-161">**Changes in Oracle Linux 7**</span></span>

<span data-ttu-id="773b7-162">Förbereda en virtuell dator för Oracle Linux 7 för Azure liknar Oracle Linux 6, men det finns flera viktiga skillnader noteras:</span><span class="sxs-lookup"><span data-stu-id="773b7-162">Preparing an Oracle Linux 7 virtual machine for Azure is very similar to Oracle Linux 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="773b7-163">Både kompatibla kernel Red Hat och Oracles UEK3 stöds i Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-163">Both the Red Hat compatible kernel and Oracle's UEK3 are supported in Azure.</span></span>  <span data-ttu-id="773b7-164">UEK3 kernel rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="773b7-164">The UEK3 kernel is recommended.</span></span>
* <span data-ttu-id="773b7-165">NetworkManager paketet inte längre i konflikt med Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="773b7-165">The NetworkManager package no longer conflicts with the Azure Linux agent.</span></span> <span data-ttu-id="773b7-166">Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="773b7-166">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="773b7-167">GRUB2 används nu som standard-startprogrammet så proceduren för att redigera kernel parametrar har ändrats (se nedan).</span><span class="sxs-lookup"><span data-stu-id="773b7-167">GRUB2 is now used as the default bootloader, so the procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="773b7-168">XFS är nu filsystemet.</span><span class="sxs-lookup"><span data-stu-id="773b7-168">XFS is now the default file system.</span></span> <span data-ttu-id="773b7-169">Filsystemet ext4 kan fortfarande användas om du vill.</span><span class="sxs-lookup"><span data-stu-id="773b7-169">The ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="773b7-170">**Konfigurationssteg**</span><span class="sxs-lookup"><span data-stu-id="773b7-170">**Configuration steps**</span></span>

1. <span data-ttu-id="773b7-171">Välj den virtuella datorn i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="773b7-171">In Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="773b7-172">Klicka på **Anslut** att öppna ett konsolfönster för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="773b7-172">Click **Connect** to open a console window for the virtual machine.</span></span>
3. <span data-ttu-id="773b7-173">Skapa en fil med namnet **nätverk** i den `/etc/sysconfig/` katalog som innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="773b7-173">Create a file named **network** in the `/etc/sysconfig/` directory that contains the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. <span data-ttu-id="773b7-174">Skapa en fil med namnet **ifcfg eth0** i den `/etc/sysconfig/network-scripts/` katalog som innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="773b7-174">Create a file named **ifcfg-eth0** in the `/etc/sysconfig/network-scripts/` directory that contains the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. <span data-ttu-id="773b7-175">Ändra udev regler för att undvika att generera statiska regler för Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="773b7-175">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="773b7-176">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="773b7-176">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. <span data-ttu-id="773b7-177">Kontrollera nätverkstjänsten startar när datorn startas genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-177">Ensure the network service will start at boot time by running the following command:</span></span>
   
        # sudo chkconfig network on
7. <span data-ttu-id="773b7-178">Installera python-pyasn1 paketet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-178">Install the python-pyasn1 package by running the following command:</span></span>
   
        # sudo yum install python-pyasn1
8. <span data-ttu-id="773b7-179">Kör följande kommando för att rensa aktuella yum metadata och installera uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="773b7-179">Run the following command to clear the current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all
        # sudo yum -y update
9. <span data-ttu-id="773b7-180">Ändra raden kernel Start i grub konfigurationen så att ytterligare kernel parametrar för Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-180">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="773b7-181">Om du vill öppna ”/ etc/standard/grub” i en textredigerare och redigera den `GRUB_CMDLINE_LINUX` parameter, till exempel:</span><span class="sxs-lookup"><span data-stu-id="773b7-181">To do this open "/etc/default/grub" in a text editor and edit the `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="773b7-182">Detta säkerställer också att alla konsolmeddelanden skickas till den första seriella porten som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="773b7-182">This will also ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="773b7-183">Även stängs av nya OEL 7 namnkonventionerna för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="773b7-183">It also turns off the new OEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="773b7-184">Förutom de som nämns ovan, rekommenderas att *ta bort* följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="773b7-184">In addition to the above, it is recommended to *remove* the following parameters:</span></span>
   
       rhgb quiet crashkernel=auto
   
   <span data-ttu-id="773b7-185">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla loggar som ska skickas till den seriella porten.</span><span class="sxs-lookup"><span data-stu-id="773b7-185">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>
   
   <span data-ttu-id="773b7-186">Den `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar mängden tillgängligt minne på den virtuella datorn med 128 MB eller mer, som kan vara problematiskt på de virtuella datorn är mindre.</span><span class="sxs-lookup"><span data-stu-id="773b7-186">The `crashkernel` option may be left configured if desired, but note that this parameter will reduce the amount of available memory in the VM by 128MB or more, which may be problematic on the smaller VM sizes.</span></span>
10. <span data-ttu-id="773b7-187">När du är klar för redigering ”/ etc/standard/grub” per ovan, kör du följande kommando för att återskapa grub konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="773b7-187">Once you are done editing "/etc/default/grub" per above, run the following command to rebuild the grub configuration:</span></span>
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. <span data-ttu-id="773b7-188">Se till att SSH-server har installerats och konfigurerats för att starta när datorn startas.</span><span class="sxs-lookup"><span data-stu-id="773b7-188">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="773b7-189">Detta är vanligtvis standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="773b7-189">This is usually the default.</span></span>
12. <span data-ttu-id="773b7-190">Installera Azure Linux-agenten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="773b7-190">Install the Azure Linux Agent by running the following command:</span></span>
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. <span data-ttu-id="773b7-191">Skapa inte växlingsutrymme på OS-disk.</span><span class="sxs-lookup"><span data-stu-id="773b7-191">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="773b7-192">Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med den lokala resurs disken som är kopplad till den virtuella datorn när du har etablerat på Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-192">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="773b7-193">Observera att den lokala resurs disken är en *tillfälliga* disk och kan tömmas när den virtuella datorn avetableras.</span><span class="sxs-lookup"><span data-stu-id="773b7-193">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="773b7-194">När du har installerat Azure Linux-agenten (se föregående steg), ändra följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="773b7-194">After installing the Azure Linux Agent (see the previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. <span data-ttu-id="773b7-195">Kör följande kommandon för att ta bort etableringen av den virtuella datorn och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="773b7-195">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. <span data-ttu-id="773b7-196">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="773b7-196">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="773b7-197">Din Linux VHD är nu redo att överföras till Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-197">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="773b7-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="773b7-198">Next steps</span></span>
<span data-ttu-id="773b7-199">Nu är du redo att använda Oracle Linux-VHD för att skapa nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="773b7-199">You're now ready to use your Oracle Linux .vhd to create new virtual machines in Azure.</span></span> <span data-ttu-id="773b7-200">Om att du överföra VHD-filen till Azure finns i steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="773b7-200">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

