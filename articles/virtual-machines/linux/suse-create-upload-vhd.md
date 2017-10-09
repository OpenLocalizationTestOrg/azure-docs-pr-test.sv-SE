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
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a><span data-ttu-id="69ff8-103">Förbered en virtuell SLES- eller openSUSE-dator för Azure</span><span class="sxs-lookup"><span data-stu-id="69ff8-103">Prepare a SLES or openSUSE virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="69ff8-104">Krav</span><span class="sxs-lookup"><span data-stu-id="69ff8-104">Prerequisites</span></span>
<span data-ttu-id="69ff8-105">Den här artikeln förutsätter att du redan har installerat en SUSE eller openSUSE Linux operativsystem tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="69ff8-105">This article assumes that you have already installed a SUSE or openSUSE Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="69ff8-106">Flera verktyg finns toocreate VHD-filer, till exempel en virtualiseringslösning som Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="69ff8-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="69ff8-107">Instruktioner finns i [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="69ff8-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="sles--opensuse-installation-notes"></a><span data-ttu-id="69ff8-108">SLES / openSUSE installationsinformation</span><span class="sxs-lookup"><span data-stu-id="69ff8-108">SLES / openSUSE installation notes</span></span>
* <span data-ttu-id="69ff8-109">Se även [allmänna Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer tips om hur du förbereder Linux för Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="69ff8-110">Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="69ff8-110">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="69ff8-111">Du kan konvertera hello tooVHD diskformat med Hyper-V Manager eller hello convert-vhd-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="69ff8-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="69ff8-112">När du installerar hello Linux-system rekommenderas att du använder standard partitioner i stället för LVM (ofta hello standard för många installationer).</span><span class="sxs-lookup"><span data-stu-id="69ff8-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="69ff8-113">Det här undviker LVM står i konflikt med klonade virtuella datorer, särskilt om ett operativsystem någonsin behöver toobe kopplade tooanother VM för felsökning.</span><span class="sxs-lookup"><span data-stu-id="69ff8-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="69ff8-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar som önskade.</span><span class="sxs-lookup"><span data-stu-id="69ff8-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="69ff8-115">Konfigurera inte en byte-partition på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="69ff8-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="69ff8-116">hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="69ff8-116">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="69ff8-117">Mer information om detta finns i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="69ff8-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="69ff8-118">Alla virtuella hårddiskar hello måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="69ff8-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-suse-studio"></a><span data-ttu-id="69ff8-119">Använd SUSE Studio</span><span class="sxs-lookup"><span data-stu-id="69ff8-119">Use SUSE Studio</span></span>
<span data-ttu-id="69ff8-120">[SUSE Studio](http://www.susestudio.com) kan enkelt skapa och hantera dina SLES och openSUSE avbildningar för Azure och Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="69ff8-120">[SUSE Studio](http://www.susestudio.com) can easily create and manage your SLES and openSUSE images for Azure and Hyper-V.</span></span> <span data-ttu-id="69ff8-121">Detta är hello rekommenderat tillvägagångssätt för att anpassa dina egna SLES och openSUSE bilder.</span><span class="sxs-lookup"><span data-stu-id="69ff8-121">This is hello recommended approach for customizing your own SLES and openSUSE images.</span></span>

<span data-ttu-id="69ff8-122">Som ett alternativ toobuilding egna VHD, SUSE publiceras även BYOS (ta med din egen prenumeration) avbildningar för SLES på [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span><span class="sxs-lookup"><span data-stu-id="69ff8-122">As an alternative toobuilding your own VHD, SUSE also publishes BYOS (Bring Your Own Subscription) images for SLES at [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span></span>

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a><span data-ttu-id="69ff8-123">Förbereda SUSE Linux Enterprise Server 11 SP4</span><span class="sxs-lookup"><span data-stu-id="69ff8-123">Prepare SUSE Linux Enterprise Server 11 SP4</span></span>
1. <span data-ttu-id="69ff8-124">Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="69ff8-124">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="69ff8-125">Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="69ff8-125">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="69ff8-126">Registrera din SUSE Linux Enterprise system tooallow den toodownload uppdateringar och installera paket.</span><span class="sxs-lookup"><span data-stu-id="69ff8-126">Register your SUSE Linux Enterprise system tooallow it toodownload updates and install packages.</span></span>
4. <span data-ttu-id="69ff8-127">Uppdatera hello system med hello senaste korrigeringarna:</span><span class="sxs-lookup"><span data-stu-id="69ff8-127">Update hello system with hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="69ff8-128">Installera hello Azure Linux-agenten från hello SLES databasen:</span><span class="sxs-lookup"><span data-stu-id="69ff8-128">Install hello Azure Linux Agent from hello SLES repository:</span></span>
   
        # sudo zypper install WALinuxAgent
6. <span data-ttu-id="69ff8-129">Kontrollera om waagent anges för ”on” i chkconfig och om inte, aktivera den för autostart:</span><span class="sxs-lookup"><span data-stu-id="69ff8-129">Check if waagent is set too"on" in chkconfig, and if not, enable it for autostart:</span></span>
   
        # sudo chkconfig waagent on
7. <span data-ttu-id="69ff8-130">Kontrollera om waagent-tjänsten körs och starta om inte:</span><span class="sxs-lookup"><span data-stu-id="69ff8-130">Check if waagent service is running, and if not, start it:</span></span> 
   
        # sudo service waagent start
8. <span data-ttu-id="69ff8-131">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-131">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="69ff8-132">Det här öppna toodo ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="69ff8-132">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    <span data-ttu-id="69ff8-133">Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="69ff8-133">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
9. <span data-ttu-id="69ff8-134">Bekräfta att /boot/grub/menu.lst och/etc/fstab båda referens hello disk med dess UUID (av uuid) i stället för hello disk-ID (med-id).</span><span class="sxs-lookup"><span data-stu-id="69ff8-134">Confirm that /boot/grub/menu.lst and /etc/fstab both reference hello disk using its UUID (by-uuid) instead of hello disk ID (by-id).</span></span> 
   
    <span data-ttu-id="69ff8-135">Hämta disk-UUID</span><span class="sxs-lookup"><span data-stu-id="69ff8-135">Get disk UUID</span></span>
   
        # ls /dev/disk/by-uuid/
   
    <span data-ttu-id="69ff8-136">Om /dev/disk/by-id är används, uppdatera både /boot/grub/menu.lst och/etc/fstab med hello rätt av uuid värdet</span><span class="sxs-lookup"><span data-stu-id="69ff8-136">If /dev/disk/by-id/ is used, update both /boot/grub/menu.lst and /etc/fstab with hello proper by-uuid value</span></span>
   
    <span data-ttu-id="69ff8-137">Före ändringen</span><span class="sxs-lookup"><span data-stu-id="69ff8-137">Before change</span></span>
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    <span data-ttu-id="69ff8-138">Efter ändring</span><span class="sxs-lookup"><span data-stu-id="69ff8-138">After change</span></span>
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. <span data-ttu-id="69ff8-139">Ändra udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="69ff8-139">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="69ff8-140">De här reglerna kan orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="69ff8-140">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. <span data-ttu-id="69ff8-141">Det rekommenderas tooedit hello filen ”/ etc/sysconfig/nätverk/dhcp” och ändra hello `DHCLIENT_SET_HOSTNAME` parametern toohello följande:</span><span class="sxs-lookup"><span data-stu-id="69ff8-141">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
    
     <span data-ttu-id="69ff8-142">DHCLIENT_SET_HOSTNAME = ”Nej”</span><span class="sxs-lookup"><span data-stu-id="69ff8-142">DHCLIENT_SET_HOSTNAME="no"</span></span>
12. <span data-ttu-id="69ff8-143">Kommentera ut ”/ etc/sudoers”, eller ta bort hello följande rader om de finns:</span><span class="sxs-lookup"><span data-stu-id="69ff8-143">In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
    
     <span data-ttu-id="69ff8-144">Som standard targetpw # uppmana hello lösenordet för hello målanvändare d.v.s. rot alla ALL=(ALL) alla # varning!</span><span class="sxs-lookup"><span data-stu-id="69ff8-144">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="69ff8-145">Använd endast denna tillsammans med 'Standardvärden targetpw'!</span><span class="sxs-lookup"><span data-stu-id="69ff8-145">Only use this together with 'Defaults targetpw'!</span></span>
13. <span data-ttu-id="69ff8-146">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="69ff8-146">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="69ff8-147">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="69ff8-147">This is usually hello default.</span></span>
14. <span data-ttu-id="69ff8-148">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="69ff8-148">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="69ff8-149">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-149">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="69ff8-150">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="69ff8-150">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="69ff8-151">När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="69ff8-151">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="69ff8-152">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Obs: Ange den här toowhatever måste toobe.</span><span class="sxs-lookup"><span data-stu-id="69ff8-152">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
15. <span data-ttu-id="69ff8-153">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="69ff8-153">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="69ff8-154">sudo waagent-force - avetablering</span><span class="sxs-lookup"><span data-stu-id="69ff8-154">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="69ff8-155">Exportera HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="69ff8-155">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="69ff8-156">Logga ut</span><span class="sxs-lookup"><span data-stu-id="69ff8-156">logout</span></span>
16. <span data-ttu-id="69ff8-157">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="69ff8-157">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="69ff8-158">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-158">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="prepare-opensuse-131"></a><span data-ttu-id="69ff8-159">Förbereda openSUSE 13,1 +</span><span class="sxs-lookup"><span data-stu-id="69ff8-159">Prepare openSUSE 13.1+</span></span>
1. <span data-ttu-id="69ff8-160">Välj hello virtuell dator i hello mittersta rutan av Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="69ff8-160">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="69ff8-161">Klicka på **Anslut** tooopen hello fönster för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="69ff8-161">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="69ff8-162">Kör hello-kommando på hello shell '`zypper lr`'.</span><span class="sxs-lookup"><span data-stu-id="69ff8-162">On hello shell, run hello command '`zypper lr`'.</span></span> <span data-ttu-id="69ff8-163">Om det här kommandot returnerar utdata liknande toohello följande och sedan hello databaser är korrekt konfigurerat--krävs inte några justeringar (Observera att versionsnumren kan variera):</span><span class="sxs-lookup"><span data-stu-id="69ff8-163">If this command returns output similar toohello following, then hello repositories are configured as expected--no adjustments are necessary (note that version numbers may vary):</span></span>
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    <span data-ttu-id="69ff8-164">Om hello kommando returnerar ”inga databaser som definierats...” Använd hello följande kommandon tooadd dessa repor:</span><span class="sxs-lookup"><span data-stu-id="69ff8-164">If hello command returns "No repositories defined..." then use hello following commands tooadd these repos:</span></span>
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    <span data-ttu-id="69ff8-165">Du kan kontrollera hello databaser har lagts till genom att köra kommandot hello '`zypper lr`' igen.</span><span class="sxs-lookup"><span data-stu-id="69ff8-165">You can then verify hello repositories have been added by running hello command '`zypper lr`' again.</span></span> <span data-ttu-id="69ff8-166">Om en av hello uppdatering databaser inte är aktiverad, kan du aktivera det med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="69ff8-166">In case one of hello relevant update repositories is not enabled, enable it with following command:</span></span>
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. <span data-ttu-id="69ff8-167">Uppdatera hello kernel toohello senaste tillgängliga versionen:</span><span class="sxs-lookup"><span data-stu-id="69ff8-167">Update hello kernel toohello latest available version:</span></span>
   
        # sudo zypper up kernel-default
   
    <span data-ttu-id="69ff8-168">Eller tooupdate hello system med alla hello senaste korrigeringarna:</span><span class="sxs-lookup"><span data-stu-id="69ff8-168">Or tooupdate hello system with all hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="69ff8-169">Installera hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="69ff8-169">Install hello Azure Linux Agent.</span></span>
   
   # <a name="sudo-zypper-install-walinuxagent"></a><span data-ttu-id="69ff8-170">sudo zypper installera WALinuxAgent</span><span class="sxs-lookup"><span data-stu-id="69ff8-170">sudo zypper install WALinuxAgent</span></span>
6. <span data-ttu-id="69ff8-171">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-171">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="69ff8-172">toodo detta, öppna ”/ boot/grub/menu.lst” i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="69ff8-172">toodo this, open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
     <span data-ttu-id="69ff8-173">konsolen = ttyS0 earlyprintk = ttyS0 rootdelay = 300</span><span class="sxs-lookup"><span data-stu-id="69ff8-173">console=ttyS0 earlyprintk=ttyS0 rootdelay=300</span></span>
   
   <span data-ttu-id="69ff8-174">Detta säkerställer att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="69ff8-174">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="69ff8-175">Dessutom tar du bort hello följande parametrar från hello kernel Start rad om de finns:</span><span class="sxs-lookup"><span data-stu-id="69ff8-175">In addition, remove hello following parameters from hello kernel boot line if they exist:</span></span>
   
     <span data-ttu-id="69ff8-176">reservera libata.atapi_enabled=0 = 0x1f0, 0x8</span><span class="sxs-lookup"><span data-stu-id="69ff8-176">libata.atapi_enabled=0 reserve=0x1f0,0x8</span></span>
7. <span data-ttu-id="69ff8-177">Det rekommenderas tooedit hello filen ”/ etc/sysconfig/nätverk/dhcp” och ändra hello `DHCLIENT_SET_HOSTNAME` parametern toohello följande:</span><span class="sxs-lookup"><span data-stu-id="69ff8-177">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
   
     <span data-ttu-id="69ff8-178">DHCLIENT_SET_HOSTNAME = ”Nej”</span><span class="sxs-lookup"><span data-stu-id="69ff8-178">DHCLIENT_SET_HOSTNAME="no"</span></span>
8. <span data-ttu-id="69ff8-179">**Viktigt:** i ”/ etc/sudoers”, kommentera ut eller ta bort hello följande rader om de finns:</span><span class="sxs-lookup"><span data-stu-id="69ff8-179">**Important:** In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
   
     <span data-ttu-id="69ff8-180">Som standard targetpw # uppmana hello lösenordet för hello målanvändare d.v.s. rot alla ALL=(ALL) alla # varning!</span><span class="sxs-lookup"><span data-stu-id="69ff8-180">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="69ff8-181">Använd endast denna tillsammans med 'Standardvärden targetpw'!</span><span class="sxs-lookup"><span data-stu-id="69ff8-181">Only use this together with 'Defaults targetpw'!</span></span>
9. <span data-ttu-id="69ff8-182">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="69ff8-182">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="69ff8-183">Detta är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="69ff8-183">This is usually hello default.</span></span>
10. <span data-ttu-id="69ff8-184">Skapa inte växlingsutrymme på hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="69ff8-184">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="69ff8-185">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello VM efter etableringen på Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-185">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="69ff8-186">Observera den lokala resurs hello-disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="69ff8-186">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="69ff8-187">När du har installerat hello Azure Linux-agenten (se föregående steg), ändra hello följande parametrar i /etc/waagent.conf på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="69ff8-187">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="69ff8-188">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Obs: Ange den här toowhatever måste toobe.</span><span class="sxs-lookup"><span data-stu-id="69ff8-188">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
11. <span data-ttu-id="69ff8-189">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="69ff8-189">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="69ff8-190">sudo waagent-force - avetablering</span><span class="sxs-lookup"><span data-stu-id="69ff8-190">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="69ff8-191">Exportera HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="69ff8-191">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="69ff8-192">Logga ut</span><span class="sxs-lookup"><span data-stu-id="69ff8-192">logout</span></span>
12. <span data-ttu-id="69ff8-193">Kontrollera hello Azure Linux-agenten körs vid start:</span><span class="sxs-lookup"><span data-stu-id="69ff8-193">Ensure hello Azure Linux Agent runs at startup:</span></span>
    
        # sudo systemctl enable waagent.service
13. <span data-ttu-id="69ff8-194">Klicka på **åtgärd -> stängs ned** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="69ff8-194">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="69ff8-195">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-195">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69ff8-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69ff8-196">Next steps</span></span>
<span data-ttu-id="69ff8-197">Du är nu redo toouse SUSE Linux virtuell hårddisk toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="69ff8-197">You're now ready toouse your SUSE Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="69ff8-198">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="69ff8-198">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

