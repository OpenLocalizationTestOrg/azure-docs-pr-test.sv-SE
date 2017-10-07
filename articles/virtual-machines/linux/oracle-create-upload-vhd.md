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
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a><span data-ttu-id="2ce52-103">Förbered en virtuell Oracle Linux-dator för Azure</span><span class="sxs-lookup"><span data-stu-id="2ce52-103">Prepare an Oracle Linux virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="2ce52-104">Krav</span><span class="sxs-lookup"><span data-stu-id="2ce52-104">Prerequisites</span></span>
<span data-ttu-id="2ce52-105">Den här artikeln förutsätter att du redan har installerat en Oracle Linux operativsystem tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="2ce52-105">This article assumes that you have already installed an Oracle Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="2ce52-106">Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="2ce52-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="2ce52-107">Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="2ce52-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="oracle-linux-installation-notes"></a><span data-ttu-id="2ce52-108">Installationsinformation för Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="2ce52-108">Oracle Linux installation notes</span></span>
* <span data-ttu-id="2ce52-109">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="2ce52-110">Oracles Red Hat kompatibel kernel och deras UEK3 (Unbreakable Enterprise Kernel) stöds både i Hyper-V och Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-110">Oracle's Red Hat compatible kernel and their UEK3 (Unbreakable Enterprise Kernel) are both supported on Hyper-V and Azure.</span></span> <span data-ttu-id="2ce52-111">För bästa resultat bör vara säker på att tooupdate toohello senaste kernel när du förberedde din Oracle Linux VHD.</span><span class="sxs-lookup"><span data-stu-id="2ce52-111">For best results, please be sure tooupdate toohello latest kernel while preparing your Oracle Linux VHD.</span></span>
* <span data-ttu-id="2ce52-112">Oracles UEK2 stöds inte på Hyper-V och Azure som inte omfattar hello krävs drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="2ce52-112">Oracle's UEK2 is not supported on Hyper-V and Azure as it does not include hello required drivers.</span></span>
* <span data-ttu-id="2ce52-113">Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="2ce52-113">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="2ce52-114">Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="2ce52-114">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="2ce52-115">När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="2ce52-115">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="2ce52-116">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning.</span><span class="sxs-lookup"><span data-stu-id="2ce52-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="2ce52-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="2ce52-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="2ce52-118">NUMA stöds inte för större VM-storlekar på grund av tooa programfel i Linux kernel-versioner än 2.6.37.</span><span class="sxs-lookup"><span data-stu-id="2ce52-118">NUMA is not supported for larger VM sizes due tooa bug in Linux kernel versions below 2.6.37.</span></span> <span data-ttu-id="2ce52-119">I detta fall främst påverkar distributioner med hello överordnade Red Hat 2.6.32 kernel.</span><span class="sxs-lookup"><span data-stu-id="2ce52-119">This issue primarily impacts distributions using hello upstream Red Hat 2.6.32 kernel.</span></span> <span data-ttu-id="2ce52-120">Manuell installation av hello Azure Linux-agenten (waagent) inaktiverar automatiskt NUMA i hello GRUB konfiguration för hello Linux kernel.</span><span class="sxs-lookup"><span data-stu-id="2ce52-120">Manual installation of hello Azure Linux agent (waagent) will automatically disable NUMA in hello GRUB configuration for hello Linux kernel.</span></span> <span data-ttu-id="2ce52-121">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="2ce52-121">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="2ce52-122">Konfigurera inte en byte-partition på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="2ce52-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="2ce52-123">hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="2ce52-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="2ce52-124">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="2ce52-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="2ce52-125">Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="2ce52-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>
* <span data-ttu-id="2ce52-126">Kontrollera att hello `Addons` databasen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="2ce52-126">Make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="2ce52-127">Redigera filen hello `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) eller `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), och ändra hello rad `enabled=0` för`enabled=1` under **[ol6_addons]** eller **[ol7_addons]** på denna filen.</span><span class="sxs-lookup"><span data-stu-id="2ce52-127">Edit hello file `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux ), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

## <a name="oracle-linux-64"></a><span data-ttu-id="2ce52-128">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="2ce52-128">Oracle Linux 6.4+</span></span>
<span data-ttu-id="2ce52-129">Du måste slutföra specifika konfigurationssteg i hello operativsystemet för hello virtuella toorun i Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-129">You must complete specific configuration steps in hello operating system for hello virtual machine toorun in Azure.</span></span>

1. <span data-ttu-id="2ce52-130">Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="2ce52-130">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="2ce52-131">Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2ce52-131">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="2ce52-132">Avinstallera NetworkManager genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-132">Uninstall NetworkManager by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager
   
    <span data-ttu-id="2ce52-133">**Obs:** om hello paketet inte har installerats, kommandot misslyckas med felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="2ce52-133">**Note:** If hello package is not already installed, this command will fail with an error message.</span></span> <span data-ttu-id="2ce52-134">Detta är förväntat.</span><span class="sxs-lookup"><span data-stu-id="2ce52-134">This is expected.</span></span>
4. <span data-ttu-id="2ce52-135">Skapa en fil med namnet **nätverk** i hello `/etc/sysconfig/` katalog som innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2ce52-135">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. <span data-ttu-id="2ce52-136">Skapa en fil med namnet **ifcfg eth0** i hello `/etc/sysconfig/network-scripts/` katalog som innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2ce52-136">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. <span data-ttu-id="2ce52-137">Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="2ce52-137">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="2ce52-138">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="2ce52-138">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. <span data-ttu-id="2ce52-139">Kontrollera hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-139">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # chkconfig network on
8. <span data-ttu-id="2ce52-140">Installera python-pyasn1 genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-140">Install python-pyasn1 by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
9. <span data-ttu-id="2ce52-141">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-141">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="2ce52-142">Det här öppna toodo ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2ce52-142">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   <span data-ttu-id="2ce52-143">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="2ce52-143">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="2ce52-144">Detta inaktiverar NUMA på grund av tooa programfel i Oracles Red Hat kompatibel kernel.</span><span class="sxs-lookup"><span data-stu-id="2ce52-144">This will disable NUMA due tooa bug in Oracle's Red Hat compatible kernel.</span></span>
   
   <span data-ttu-id="2ce52-145">Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2ce52-145">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
   <span data-ttu-id="2ce52-146">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="2ce52-146">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="2ce52-147">Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="2ce52-147">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="2ce52-148">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="2ce52-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="2ce52-149">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="2ce52-149">This is usually hello default.</span></span>
11. <span data-ttu-id="2ce52-150">Installera hello Azure Linux-agenten genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="2ce52-150">Install hello Azure Linux Agent by running hello following command.</span></span> <span data-ttu-id="2ce52-151">hello senaste versionen är 2.0.15.</span><span class="sxs-lookup"><span data-stu-id="2ce52-151">hello latest version is 2.0.15.</span></span>
    
        # sudo yum install WALinuxAgent
    
    <span data-ttu-id="2ce52-152">Observera att installera hello WALinuxAgent paketet tas bort hello NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort enligt beskrivningen i steg 2.</span><span class="sxs-lookup"><span data-stu-id="2ce52-152">Note that installing hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.</span></span>
12. <span data-ttu-id="2ce52-153">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="2ce52-153">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="2ce52-154">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-154">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="2ce52-155">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="2ce52-155">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="2ce52-156">När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="2ce52-156">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. <span data-ttu-id="2ce52-157">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="2ce52-157">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. <span data-ttu-id="2ce52-158">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="2ce52-158">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="2ce52-159">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-159">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="oracle-linux-70"></a><span data-ttu-id="2ce52-160">Oracle Linux 7.0 +</span><span class="sxs-lookup"><span data-stu-id="2ce52-160">Oracle Linux 7.0+</span></span>
<span data-ttu-id="2ce52-161">**Ändringar i Oracle Linux 7**</span><span class="sxs-lookup"><span data-stu-id="2ce52-161">**Changes in Oracle Linux 7**</span></span>

<span data-ttu-id="2ce52-162">Förbereda en virtuell dator för Oracle Linux 7 för Azure är mycket lik tooOracle Linux 6, men det finns flera viktiga skillnader noteras:</span><span class="sxs-lookup"><span data-stu-id="2ce52-162">Preparing an Oracle Linux 7 virtual machine for Azure is very similar tooOracle Linux 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="2ce52-163">Både hello Red Hat kompatibel kernel och Oracles UEK3 stöds i Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-163">Both hello Red Hat compatible kernel and Oracle's UEK3 are supported in Azure.</span></span>  <span data-ttu-id="2ce52-164">Hej UEK3 kernel rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="2ce52-164">hello UEK3 kernel is recommended.</span></span>
* <span data-ttu-id="2ce52-165">Hej NetworkManager paketet inte längre är i konflikt med hello Azure Linux-agent.</span><span class="sxs-lookup"><span data-stu-id="2ce52-165">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="2ce52-166">Det här paketet installeras som standard och vi rekommenderar att den inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="2ce52-166">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="2ce52-167">GRUB2 används nu som hello standard startprogrammet så hello proceduren för att redigera kernel parametrar har ändrats (se nedan).</span><span class="sxs-lookup"><span data-stu-id="2ce52-167">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="2ce52-168">XFS är nu hello standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="2ce52-168">XFS is now hello default file system.</span></span> <span data-ttu-id="2ce52-169">Hej ext4 filsystem kan fortfarande användas om du vill.</span><span class="sxs-lookup"><span data-stu-id="2ce52-169">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="2ce52-170">**Konfigurationssteg**</span><span class="sxs-lookup"><span data-stu-id="2ce52-170">**Configuration steps**</span></span>

1. <span data-ttu-id="2ce52-171">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="2ce52-171">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="2ce52-172">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2ce52-172">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="2ce52-173">Skapa en fil med namnet **nätverk** i hello `/etc/sysconfig/` katalog som innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2ce52-173">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. <span data-ttu-id="2ce52-174">Skapa en fil med namnet **ifcfg eth0** i hello `/etc/sysconfig/network-scripts/` katalog som innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2ce52-174">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. <span data-ttu-id="2ce52-175">Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="2ce52-175">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="2ce52-176">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="2ce52-176">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. <span data-ttu-id="2ce52-177">Kontrollera hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-177">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on
7. <span data-ttu-id="2ce52-178">Installera hello python pyasn1 paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-178">Install hello python-pyasn1 package by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
8. <span data-ttu-id="2ce52-179">Kör följande kommando tooclear hello aktuella yum metadata hello och installera uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="2ce52-179">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all
        # sudo yum -y update
9. <span data-ttu-id="2ce52-180">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-180">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="2ce52-181">toodo detta öppna ”/ etc/standard/grub” i en text-redigeraren och redigera hello `GRUB_CMDLINE_LINUX` parameter, till exempel:</span><span class="sxs-lookup"><span data-stu-id="2ce52-181">toodo this open "/etc/default/grub" in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="2ce52-182">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="2ce52-182">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="2ce52-183">Även stängs av hello nya OEL 7 namnkonventioner för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="2ce52-183">It also turns off hello new OEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="2ce52-184">Dessutom toohello ovan, rekommenderas för*ta bort* hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2ce52-184">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
       rhgb quiet crashkernel=auto
   
   <span data-ttu-id="2ce52-185">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="2ce52-185">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="2ce52-186">Hej `crashkernel` alternativet kan vara left konfigureras om så önskas, men Observera att den här parametern minskar hello mängden tillgängligt minne i hello VM 128 MB eller mer, som kan vara problematiskt på hello mindre VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="2ce52-186">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="2ce52-187">När du är klar för redigering ”/ etc/standard/grub” per ovan, kör hello följande toorebuild hello grub konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2ce52-187">Once you are done editing "/etc/default/grub" per above, run hello following command toorebuild hello grub configuration:</span></span>
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. <span data-ttu-id="2ce52-188">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="2ce52-188">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="2ce52-189">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="2ce52-189">This is usually hello default.</span></span>
12. <span data-ttu-id="2ce52-190">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2ce52-190">Install hello Azure Linux Agent by running hello following command:</span></span>
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. <span data-ttu-id="2ce52-191">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="2ce52-191">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="2ce52-192">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-192">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="2ce52-193">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="2ce52-193">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="2ce52-194">När du har installerat hello Azure Linux-agenten (se hello föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="2ce52-194">After installing hello Azure Linux Agent (see hello previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. <span data-ttu-id="2ce52-195">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="2ce52-195">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. <span data-ttu-id="2ce52-196">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="2ce52-196">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="2ce52-197">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-197">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ce52-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ce52-198">Next steps</span></span>
<span data-ttu-id="2ce52-199">Du är nu redo toouse Oracle Linux VHD toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce52-199">You're now ready toouse your Oracle Linux .vhd toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="2ce52-200">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ce52-200">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

