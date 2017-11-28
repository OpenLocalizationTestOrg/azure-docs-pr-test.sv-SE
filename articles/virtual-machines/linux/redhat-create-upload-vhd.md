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
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a><span data-ttu-id="ab89b-103">Förbered en Red Hat-baserad virtuell dator för Azure</span><span class="sxs-lookup"><span data-stu-id="ab89b-103">Prepare a Red Hat-based virtual machine for Azure</span></span>
<span data-ttu-id="ab89b-104">I den här artikeln får du lära dig hur tooprepare en virtuell dator för Red Hat Enterprise Linux (RHEL) för användning i Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-104">In this article, you will learn how tooprepare a Red Hat Enterprise Linux (RHEL) virtual machine for use in Azure.</span></span> <span data-ttu-id="ab89b-105">hello versioner av RHEL som beskrivs i den här artikeln är 6.7 + och 7.1 +.</span><span class="sxs-lookup"><span data-stu-id="ab89b-105">hello versions of RHEL that are covered in this article are 6.7+ and 7.1+.</span></span> <span data-ttu-id="ab89b-106">hello hypervisorer för förberedelse som beskrivs i den här artikeln är Hyper-V, kernel-baserad virtuell dator (KVM) och VMware.</span><span class="sxs-lookup"><span data-stu-id="ab89b-106">hello hypervisors for preparation that are covered in this article are Hyper-V, kernel-based virtual machine (KVM), and VMware.</span></span> <span data-ttu-id="ab89b-107">Läs mer om behörighetskraven för deltagande i programmet för Red Hat Molnåtkomst [Red Hat Molnåtkomst webbplats](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) och [kör RHEL på Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="ab89b-107">For more information about eligibility requirements for participating in Red Hat's Cloud Access program, see [Red Hat's Cloud Access website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) and [Running RHEL on Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span></span>

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="ab89b-108">Förbered en Red Hat-baserad virtuell dator från Hyper-V Manager</span><span class="sxs-lookup"><span data-stu-id="ab89b-108">Prepare a Red Hat-based virtual machine from Hyper-V Manager</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ab89b-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ab89b-109">Prerequisites</span></span>
<span data-ttu-id="ab89b-110">Det här avsnittet förutsätter att du redan har fått en ISO-fil från hello Red Hat webbplats och installerade hello RHEL avbildningen tooa virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="ab89b-110">This section assumes that you have already obtained an ISO file from hello Red Hat website and installed hello RHEL image tooa virtual hard disk (VHD).</span></span> <span data-ttu-id="ab89b-111">Mer information om hur toouse Hyper-V Manager tooinstall en operativsystemavbildning finns [installera hello Hyper-V-rollen och konfigurera en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab89b-111">For more details about how toouse Hyper-V Manager tooinstall an operating system image, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="ab89b-112">**RHEL installationsinformation**</span><span class="sxs-lookup"><span data-stu-id="ab89b-112">**RHEL installation notes**</span></span>

* <span data-ttu-id="ab89b-113">Azure stöder inte hello VHDX-format.</span><span class="sxs-lookup"><span data-stu-id="ab89b-113">Azure does not support hello VHDX format.</span></span> <span data-ttu-id="ab89b-114">Azure stöder endast fast virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-114">Azure supports only fixed VHD.</span></span> <span data-ttu-id="ab89b-115">Du kan använda tooVHD för Hyper-V Manager tooconvert hello diskformat eller du kan använda hello convert-vhd-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ab89b-115">You can use Hyper-V Manager tooconvert hello disk tooVHD format, or you can use hello convert-vhd cmdlet.</span></span> <span data-ttu-id="ab89b-116">Om du använder VirtualBox markerar **fast storlek** i motsats toohello standard dynamiskt allokerade alternativet när du skapar hello disk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-116">If you use VirtualBox, select **Fixed size** as opposed toohello default dynamically allocated option when you create hello disk.</span></span>
* <span data-ttu-id="ab89b-117">Azure stöder endast generering 1 för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-117">Azure supports only generation 1 virtual machines.</span></span> <span data-ttu-id="ab89b-118">Du kan konvertera en virtuell dator i generation 1 från VHDX toohello VHD-filformatet och dynamiskt expanderande tooa fast storlek disk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-118">You can convert a generation 1 virtual machine from VHDX toohello VHD file format and from dynamically expanding tooa fixed-size disk.</span></span> <span data-ttu-id="ab89b-119">Du kan inte ändra en virtuell dator generation.</span><span class="sxs-lookup"><span data-stu-id="ab89b-119">You can't change a virtual machine's generation.</span></span> <span data-ttu-id="ab89b-120">Mer information finns i [bör jag skapa en virtuell dator i generation 1 eller 2 i Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span><span class="sxs-lookup"><span data-stu-id="ab89b-120">For more information, see [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span></span>
* <span data-ttu-id="ab89b-121">hello maximala storleken som tillåts för hello VHD är 1,023 GB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-121">hello maximum size that's allowed for hello VHD is 1,023 GB.</span></span>
* <span data-ttu-id="ab89b-122">När du installerar hello Linux-operativsystem, rekommenderar vi att du använder standard partitioner i stället för logisk volym Manager (LVM), vilket ofta är hello standard för många installationer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-122">When you install hello Linux operating system, we recommend that you use standard partitions rather than Logical Volume Manager (LVM), which is often hello default for many installations.</span></span> <span data-ttu-id="ab89b-123">Denna praxis undviker LVM står i konflikt med den klonade virtuella datorer, särskilt om du skulle behöva tooattach ett operativsystem disk tooanother identiska virtuella för felsökning.</span><span class="sxs-lookup"><span data-stu-id="ab89b-123">This practice will avoid LVM name conflicts with cloned virtual machines, particularly if you ever need tooattach an operating system disk tooanother identical virtual machine for troubleshooting.</span></span> <span data-ttu-id="ab89b-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) får användas i datadiskar.</span><span class="sxs-lookup"><span data-stu-id="ab89b-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="ab89b-125">Kernel-stöd för att montera Universal Disk (UDF) filsystem krävs.</span><span class="sxs-lookup"><span data-stu-id="ab89b-125">Kernel support for mounting Universal Disk Format (UDF) file systems is required.</span></span> <span data-ttu-id="ab89b-126">Vid första start på Azure, hello UDF-formaterad media som är skickar bifogade toohello gäst hello etablering configuration toohello virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="ab89b-126">At first boot on Azure, hello UDF-formatted media that is attached toohello guest passes hello provisioning configuration toohello Linux virtual machine.</span></span> <span data-ttu-id="ab89b-127">hello Azure Linux-agenten måste kunna toomount hello UDF filen system tooread dess konfiguration och etablera hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ab89b-127">hello Azure Linux Agent must be able toomount hello UDF file system tooread its configuration and provision hello virtual machine.</span></span>
* <span data-ttu-id="ab89b-128">Hello Linux kernel-versioner som är äldre än 2.6.37 stöder inte icke-enhetlig minnesåtkomst (NUMA) på Hyper-V med större storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-128">Versions of hello Linux kernel that are earlier than 2.6.37 do not support non-uniform memory access (NUMA) on Hyper-V with larger virtual machine sizes.</span></span> <span data-ttu-id="ab89b-129">I detta fall främst påverkar äldre distributioner använder hello uppströms Red Hat 2.6.32 kernel och åtgärdades i RHEL 6.6 (kernel-2.6.32-504).</span><span class="sxs-lookup"><span data-stu-id="ab89b-129">This issue primarily impacts older distributions that use hello upstream Red Hat 2.6.32 kernel and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="ab89b-130">System som kör anpassade kärnor som är äldre än 2.6.37 eller RHEL-baserade kernlar som är äldre än 2.6.32-504 måste ange hello `numa=off` starta parametern på kommandoraden för hello kernel i grub.conf.</span><span class="sxs-lookup"><span data-stu-id="ab89b-130">Systems that run custom kernels that are older than 2.6.37 or RHEL-based kernels that are older than 2.6.32-504 must set hello `numa=off` boot parameter on hello kernel command line in grub.conf.</span></span> <span data-ttu-id="ab89b-131">Mer information finns i Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="ab89b-131">For more information, see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="ab89b-132">Konfigurera inte en byte-partition på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-132">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="ab89b-133">hello Linux-agenten kan vara konfigurerade toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-133">hello Linux Agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="ab89b-134">Mer information om detta finns i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ab89b-134">More information about this can be found in hello following steps.</span></span>
* <span data-ttu-id="ab89b-135">Alla virtuella hårddiskar måste ha storlekar som multiplar av 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-135">All VHDs must have sizes that are multiples of 1 MB.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="ab89b-136">Förbered en RHEL 6 virtuell dator från Hyper-V Manager</span><span class="sxs-lookup"><span data-stu-id="ab89b-136">Prepare a RHEL 6 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="ab89b-137">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ab89b-137">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="ab89b-138">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ab89b-138">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="ab89b-139">I RHEL 6 kan NetworkManager störa hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="ab89b-139">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="ab89b-140">Avinstallera paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-140">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="ab89b-141">Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-141">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="ab89b-142">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-142">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="ab89b-143">Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ab89b-143">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="ab89b-144">Dessa regler orsaka problem när du klonar en virtuell dator i Microsoft Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="ab89b-144">These rules cause problems when you clone a virtual machine in Microsoft Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="ab89b-145">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-145">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

8. <span data-ttu-id="ab89b-146">Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-146">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="ab89b-147">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-147">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-148">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-148">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. <span data-ttu-id="ab89b-149">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-149">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-150">toodo ändringen öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-150">toodo this modification, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="ab89b-151">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-151">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="ab89b-152">Dessutom rekommenderar vi att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-152">In addition, we recommended that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="ab89b-153">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-153">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="ab89b-154">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-154">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-155">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-155">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more.</span></span> <span data-ttu-id="ab89b-156">Den här konfigurationen kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-156">This configuration might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="ab89b-157">RHEL 6.5 och tidigare måste också ange hello `numa=off` kernel-parametern.</span><span class="sxs-lookup"><span data-stu-id="ab89b-157">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="ab89b-158">Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="ab89b-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

11. <span data-ttu-id="ab89b-159">Se till att hello secure shell (SSH)-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard.</span><span class="sxs-lookup"><span data-stu-id="ab89b-159">Ensure that hello secure shell (SSH) server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="ab89b-160">Ändra /etc/ssh/sshd_config tooinclude hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="ab89b-160">Modify /etc/ssh/sshd_config tooinclude hello following line:</span></span>

        ClientAliveInterval 180

12. <span data-ttu-id="ab89b-161">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-161">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    <span data-ttu-id="ab89b-162">Installera hello WALinuxAgent paketet tar bort hello NetworkManager och NetworkManager gör väldigt lätt paket om de inte har redan tagits bort i steg 3.</span><span class="sxs-lookup"><span data-stu-id="ab89b-162">Installing hello WALinuxAgent package removes hello NetworkManager and NetworkManager-gnome packages if they were not already removed in step 3.</span></span>

13. <span data-ttu-id="ab89b-163">Skapa inte växlingsutrymme på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-163">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="ab89b-164">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-164">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-165">Observera lokala resursen är en tillfällig disk och att den kan tömmas när hello virtuella avetableras hello.</span><span class="sxs-lookup"><span data-stu-id="ab89b-165">Note that hello local resource disk is a temporary disk and that it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-166">Ändra hello följande parametrar i /etc/waagent.conf på rätt sätt när du har installerat hello Azure Linux-agenten i hello förra steget:</span><span class="sxs-lookup"><span data-stu-id="ab89b-166">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in /etc/waagent.conf appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. <span data-ttu-id="ab89b-167">Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-167">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

15. <span data-ttu-id="ab89b-168">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-168">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. <span data-ttu-id="ab89b-169">Klicka på **åtgärd** > **Stäng** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ab89b-169">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="ab89b-170">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-170">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="ab89b-171">Förbered en RHEL 7 virtuell dator från Hyper-V Manager</span><span class="sxs-lookup"><span data-stu-id="ab89b-171">Prepare a RHEL 7 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="ab89b-172">Välj hello virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ab89b-172">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="ab89b-173">Klicka på **Anslut** tooopen ett konsolfönster för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ab89b-173">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="ab89b-174">Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-174">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="ab89b-175">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-175">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="ab89b-176">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-176">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="ab89b-177">Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-177">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="ab89b-178">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-178">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-179">toodo ändringen öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter.</span><span class="sxs-lookup"><span data-stu-id="ab89b-179">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="ab89b-180">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ab89b-180">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="ab89b-181">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-181">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="ab89b-182">Den här konfigurationen ger också inaktiverar hello nya RHEL 7 namnkonventioner för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ab89b-182">This configuration also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="ab89b-183">Vi rekommenderar dessutom att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-183">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="ab89b-184">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-184">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="ab89b-185">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-185">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-186">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-186">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

8. <span data-ttu-id="ab89b-187">När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:</span><span class="sxs-lookup"><span data-stu-id="ab89b-187">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. <span data-ttu-id="ab89b-188">Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard.</span><span class="sxs-lookup"><span data-stu-id="ab89b-188">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="ab89b-189">Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="ab89b-189">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

10. <span data-ttu-id="ab89b-190">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-190">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-191">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-191">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. <span data-ttu-id="ab89b-192">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-192">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. <span data-ttu-id="ab89b-193">Skapa inte växlingsutrymme på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-193">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="ab89b-194">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-194">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-195">Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-195">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-196">När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ab89b-196">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="ab89b-197">Om du vill toounregister hello prenumeration kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ab89b-197">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="ab89b-198">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-198">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="ab89b-199">Klicka på **åtgärd** > **Stäng** i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ab89b-199">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="ab89b-200">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-200">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a><span data-ttu-id="ab89b-201">Förbered en Red Hat-baserad virtuell dator från KVM</span><span class="sxs-lookup"><span data-stu-id="ab89b-201">Prepare a Red Hat-based virtual machine from KVM</span></span>
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a><span data-ttu-id="ab89b-202">Förbered en virtuell dator för RHEL 6 från KVM</span><span class="sxs-lookup"><span data-stu-id="ab89b-202">Prepare a RHEL 6 virtual machine from KVM</span></span>

1. <span data-ttu-id="ab89b-203">Hämta hello KVM bild av RHEL 6 från hello Red Hat-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-203">Download hello KVM image of RHEL 6 from hello Red Hat website.</span></span>

2. <span data-ttu-id="ab89b-204">Ange ett rotlösenord.</span><span class="sxs-lookup"><span data-stu-id="ab89b-204">Set a root password.</span></span>

    <span data-ttu-id="ab89b-205">Generera ett krypterat lösenord och kopiera hello utdata från hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="ab89b-205">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="ab89b-206">Ange en rotlösenordet med guestfish:</span><span class="sxs-lookup"><span data-stu-id="ab89b-206">Set a root password with guestfish:</span></span>
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="ab89b-207">Ändra hello andra fält av hello rotanvändaren från ””!</span><span class="sxs-lookup"><span data-stu-id="ab89b-207">Change hello second field of hello root user from "!!"</span></span> <span data-ttu-id="ab89b-208">toohello krypterat lösenord.</span><span class="sxs-lookup"><span data-stu-id="ab89b-208">toohello encrypted password.</span></span>

3. <span data-ttu-id="ab89b-209">Skapa en virtuell dator i KVM från hello qcow2 avbildning.</span><span class="sxs-lookup"><span data-stu-id="ab89b-209">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="ab89b-210">Ange hello disktyp för**qcow2**, och ange hello virtuellt nätverk gränssnittet enhetsmodell för**virtio**.</span><span class="sxs-lookup"><span data-stu-id="ab89b-210">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="ab89b-211">Sedan starta hello virtuella datorn och logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="ab89b-211">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="ab89b-212">Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-212">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="ab89b-213">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-213">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="ab89b-214">Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ab89b-214">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="ab89b-215">Dessa regler orsaka problem när du klonar en virtuell dator i Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="ab89b-215">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="ab89b-216">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-216">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

8. <span data-ttu-id="ab89b-217">Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-217">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="ab89b-218">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-218">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-219">toodo den här konfigurationen, öppna `/boot/grub/menu.lst` i en textredigerare och se till att hello standardkernel innehåller hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-219">toodo this configuration, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="ab89b-220">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-220">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="ab89b-221">Vi rekommenderar dessutom att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-221">In addition, we recommend that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="ab89b-222">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-222">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="ab89b-223">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-223">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-224">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-224">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="ab89b-225">RHEL 6.5 och tidigare måste också ange hello `numa=off` kernel-parametern.</span><span class="sxs-lookup"><span data-stu-id="ab89b-225">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="ab89b-226">Se Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="ab89b-226">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

10. <span data-ttu-id="ab89b-227">Lägg till Hyper-V-moduler tooinitramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-227">Add Hyper-V modules tooinitramfs:</span></span>  

    <span data-ttu-id="ab89b-228">Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="ab89b-228">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="ab89b-229">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-229">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="ab89b-230">Avinstallera molnet init:</span><span class="sxs-lookup"><span data-stu-id="ab89b-230">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="ab89b-231">Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas:</span><span class="sxs-lookup"><span data-stu-id="ab89b-231">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # chkconfig sshd on

    <span data-ttu-id="ab89b-232">Ändra /etc/ssh/sshd_config tooinclude hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="ab89b-232">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="ab89b-233">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-233">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-234">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-234">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. <span data-ttu-id="ab89b-235">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-235">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

        # chkconfig waagent on

15. <span data-ttu-id="ab89b-236">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-236">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-237">Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-237">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-238">När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i **/etc/waagent.conf** på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ab89b-238">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in **/etc/waagent.conf** appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="ab89b-239">Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-239">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="ab89b-240">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-240">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="ab89b-241">Stäng hello virtuell dator i KVM.</span><span class="sxs-lookup"><span data-stu-id="ab89b-241">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="ab89b-242">Konvertera hello qcow2 bild toohello VHD-format.</span><span class="sxs-lookup"><span data-stu-id="ab89b-242">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="ab89b-243">Först konvertera hello bildformat tooraw:</span><span class="sxs-lookup"><span data-stu-id="ab89b-243">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    <span data-ttu-id="ab89b-244">Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-244">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="ab89b-245">Avrunda annars hello storlek tooalign med 1 MB:</span><span class="sxs-lookup"><span data-stu-id="ab89b-245">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="ab89b-246">Konvertera hello rådata disk tooa fast storlek VHD:</span><span class="sxs-lookup"><span data-stu-id="ab89b-246">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a><span data-ttu-id="ab89b-247">Förbered en virtuell dator för RHEL 7 från KVM</span><span class="sxs-lookup"><span data-stu-id="ab89b-247">Prepare a RHEL 7 virtual machine from KVM</span></span>

1. <span data-ttu-id="ab89b-248">Hämta hello KVM bild av RHEL 7 från hello Red Hat-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-248">Download hello KVM image of RHEL 7 from hello Red Hat website.</span></span> <span data-ttu-id="ab89b-249">Den här proceduren använder RHEL 7 hello exempel.</span><span class="sxs-lookup"><span data-stu-id="ab89b-249">This procedure uses RHEL 7 as hello example.</span></span>

2. <span data-ttu-id="ab89b-250">Ange ett rotlösenord.</span><span class="sxs-lookup"><span data-stu-id="ab89b-250">Set a root password.</span></span>

    <span data-ttu-id="ab89b-251">Generera ett krypterat lösenord och kopiera hello utdata från hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="ab89b-251">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="ab89b-252">Ange en rotlösenordet med guestfish:</span><span class="sxs-lookup"><span data-stu-id="ab89b-252">Set a root password with guestfish:</span></span>

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="ab89b-253">Ändra hello andra fält av rotanvändaren från ””!</span><span class="sxs-lookup"><span data-stu-id="ab89b-253">Change hello second field of root user from "!!"</span></span> <span data-ttu-id="ab89b-254">toohello krypterat lösenord.</span><span class="sxs-lookup"><span data-stu-id="ab89b-254">toohello encrypted password.</span></span>

3. <span data-ttu-id="ab89b-255">Skapa en virtuell dator i KVM från hello qcow2 avbildning.</span><span class="sxs-lookup"><span data-stu-id="ab89b-255">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="ab89b-256">Ange hello disktyp för**qcow2**, och ange hello virtuellt nätverk gränssnittet enhetsmodell för**virtio**.</span><span class="sxs-lookup"><span data-stu-id="ab89b-256">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="ab89b-257">Sedan starta hello virtuella datorn och logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="ab89b-257">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="ab89b-258">Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-258">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="ab89b-259">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-259">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. <span data-ttu-id="ab89b-260">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-260">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

7. <span data-ttu-id="ab89b-261">Registrera din Red Hat prenumeration tooenable installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-261">Register your Red Hat subscription tooenable installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. <span data-ttu-id="ab89b-262">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-262">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-263">toodo den här konfigurationen, öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter.</span><span class="sxs-lookup"><span data-stu-id="ab89b-263">toodo this configuration, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="ab89b-264">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ab89b-264">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="ab89b-265">Detta kommando säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-265">This command also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="ab89b-266">hello kommando inaktiverar också på hello nya RHEL 7 namnkonventioner för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ab89b-266">hello command also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="ab89b-267">Vi rekommenderar dessutom att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-267">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="ab89b-268">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-268">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="ab89b-269">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-269">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-270">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-270">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="ab89b-271">När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:</span><span class="sxs-lookup"><span data-stu-id="ab89b-271">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="ab89b-272">Lägg till Hyper-V-modulerna i initramfs.</span><span class="sxs-lookup"><span data-stu-id="ab89b-272">Add Hyper-V modules into initramfs.</span></span>

    <span data-ttu-id="ab89b-273">Redigera `/etc/dracut.conf` och lägga till innehåll:</span><span class="sxs-lookup"><span data-stu-id="ab89b-273">Edit `/etc/dracut.conf` and add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="ab89b-274">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-274">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="ab89b-275">Avinstallera molnet init:</span><span class="sxs-lookup"><span data-stu-id="ab89b-275">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="ab89b-276">Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas:</span><span class="sxs-lookup"><span data-stu-id="ab89b-276">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # systemctl enable sshd

    <span data-ttu-id="ab89b-277">Ändra /etc/ssh/sshd_config tooinclude hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="ab89b-277">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="ab89b-278">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-278">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-279">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-279">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. <span data-ttu-id="ab89b-280">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-280">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

    <span data-ttu-id="ab89b-281">Aktivera hello waagent-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="ab89b-281">Enable hello waagent service:</span></span>

        # systemctl enable waagent.service

15. <span data-ttu-id="ab89b-282">Skapa inte växlingsutrymme på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-282">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="ab89b-283">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-283">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-284">Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-284">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-285">När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ab89b-285">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="ab89b-286">Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-286">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="ab89b-287">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-287">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="ab89b-288">Stäng hello virtuell dator i KVM.</span><span class="sxs-lookup"><span data-stu-id="ab89b-288">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="ab89b-289">Konvertera hello qcow2 bild toohello VHD-format.</span><span class="sxs-lookup"><span data-stu-id="ab89b-289">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="ab89b-290">Först konvertera hello bildformat tooraw:</span><span class="sxs-lookup"><span data-stu-id="ab89b-290">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    <span data-ttu-id="ab89b-291">Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-291">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="ab89b-292">Avrunda annars hello storlek tooalign med 1 MB:</span><span class="sxs-lookup"><span data-stu-id="ab89b-292">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="ab89b-293">Konvertera hello rådata disk tooa fast storlek VHD:</span><span class="sxs-lookup"><span data-stu-id="ab89b-293">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a><span data-ttu-id="ab89b-294">Förbered en Red Hat-baserad virtuell dator från VMware</span><span class="sxs-lookup"><span data-stu-id="ab89b-294">Prepare a Red Hat-based virtual machine from VMware</span></span>
### <a name="prerequisites"></a><span data-ttu-id="ab89b-295">Krav</span><span class="sxs-lookup"><span data-stu-id="ab89b-295">Prerequisites</span></span>
<span data-ttu-id="ab89b-296">Det här avsnittet förutsätter att du redan har installerat en RHEL virtuell dator i VMware.</span><span class="sxs-lookup"><span data-stu-id="ab89b-296">This section assumes that you have already installed a RHEL virtual machine in VMware.</span></span> <span data-ttu-id="ab89b-297">Mer information om hur tooinstall ett operativsystem i VMware, se [VMware gäst operativsystemet installationsguiden](http://partnerweb.vmware.com/GOSIG/home.html).</span><span class="sxs-lookup"><span data-stu-id="ab89b-297">For details about how tooinstall an operating system in VMware, see [VMware Guest Operating System Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).</span></span>

* <span data-ttu-id="ab89b-298">När du installerar hello Linux-operativsystem, rekommenderar vi att du använder standard partitioner i stället för LVM, vilket ofta är hello standard för många installationer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-298">When you install hello Linux operating system, we recommend that you use standard partitions rather than LVM, which is often hello default for many installations.</span></span> <span data-ttu-id="ab89b-299">Detta undviker LVM står i konflikt med den klonade virtuella datorn, särskilt om en operativsystemdisk behövs toobe kopplade tooanother virtuell dator för felsökning.</span><span class="sxs-lookup"><span data-stu-id="ab89b-299">This will avoid LVM name conflicts with cloned virtual machine, particularly if an operating system disk ever needs toobe attached tooanother virtual machine for troubleshooting.</span></span> <span data-ttu-id="ab89b-300">LVM eller RAID kan användas på datadiskar om önskade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-300">LVM or RAID can be used on data disks if preferred.</span></span>
* <span data-ttu-id="ab89b-301">Konfigurera inte en byte-partition på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-301">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="ab89b-302">Du kan konfigurera hello Linux-agenten toocreate en växlingsfil på hello temporär disk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-302">You can configure hello Linux agent toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="ab89b-303">Du hittar mer information om detta i hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-303">You can find more information about this in hello steps that follow.</span></span>
* <span data-ttu-id="ab89b-304">När du skapar hello virtuell hårddisk, Välj **Store virtuell disk som en enda fil**.</span><span class="sxs-lookup"><span data-stu-id="ab89b-304">When you create hello virtual hard disk, select **Store virtual disk as a single file**.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a><span data-ttu-id="ab89b-305">Förbered en virtuell dator för RHEL 6 från VMware</span><span class="sxs-lookup"><span data-stu-id="ab89b-305">Prepare a RHEL 6 virtual machine from VMware</span></span>
1. <span data-ttu-id="ab89b-306">I RHEL 6 kan NetworkManager störa hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="ab89b-306">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="ab89b-307">Avinstallera paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-307">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

2. <span data-ttu-id="ab89b-308">Skapa en fil med namnet **nätverk** hello i hello/etc/sysconfig/katalog som innehåller följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-308">Create a file named **network** in hello /etc/sysconfig/ directory that contains hello following text:</span></span>

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. <span data-ttu-id="ab89b-309">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-309">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. <span data-ttu-id="ab89b-310">Flytta (eller ta bort) hello udev regler tooavoid genererar statiska regler för hello Ethernet-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ab89b-310">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="ab89b-311">Dessa regler orsaka problem när du klonar en virtuell dator i Azure eller Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="ab89b-311">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. <span data-ttu-id="ab89b-312">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-312">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="ab89b-313">Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-313">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="ab89b-314">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-314">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-315">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-315">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. <span data-ttu-id="ab89b-316">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-316">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-317">toodo detta, öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter.</span><span class="sxs-lookup"><span data-stu-id="ab89b-317">toodo this, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="ab89b-318">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ab89b-318">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   <span data-ttu-id="ab89b-319">Detta säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-319">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="ab89b-320">Vi rekommenderar dessutom att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-320">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="ab89b-321">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-321">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="ab89b-322">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-322">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-323">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-323">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="ab89b-324">Lägg till Hyper-V-moduler tooinitramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-324">Add Hyper-V modules tooinitramfs:</span></span>

    <span data-ttu-id="ab89b-325">Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="ab89b-325">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="ab89b-326">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-326">Rebuild initramfs:</span></span>

        # dracut -f -v

10. <span data-ttu-id="ab89b-327">Se till att hello SSH-server är installerat och konfigurerat toostart när datorn startas, vilket vanligtvis är hello standard.</span><span class="sxs-lookup"><span data-stu-id="ab89b-327">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="ab89b-328">Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="ab89b-328">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

    <span data-ttu-id="ab89b-329">ClientAliveInterval 180</span><span class="sxs-lookup"><span data-stu-id="ab89b-329">ClientAliveInterval 180</span></span>

11. <span data-ttu-id="ab89b-330">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-330">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. <span data-ttu-id="ab89b-331">Skapa inte växlingsutrymme på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-331">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="ab89b-332">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-332">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-333">Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-333">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-334">När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ab89b-334">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="ab89b-335">Avregistrera hello prenumeration (vid behov) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-335">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="ab89b-336">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-336">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="ab89b-337">Stäng hello virtuella datorn och konvertera hello VMDK-filen tooa VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-337">Shut down hello virtual machine, and convert hello VMDK file tooa .vhd file.</span></span>

    <span data-ttu-id="ab89b-338">Först konvertera hello bildformat tooraw:</span><span class="sxs-lookup"><span data-stu-id="ab89b-338">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    <span data-ttu-id="ab89b-339">Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-339">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="ab89b-340">Avrunda annars hello storlek tooalign med 1 MB:</span><span class="sxs-lookup"><span data-stu-id="ab89b-340">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="ab89b-341">Konvertera hello rådata disk tooa fast storlek VHD:</span><span class="sxs-lookup"><span data-stu-id="ab89b-341">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a><span data-ttu-id="ab89b-342">Förbered en virtuell dator för RHEL 7 från VMware</span><span class="sxs-lookup"><span data-stu-id="ab89b-342">Prepare a RHEL 7 virtual machine from VMware</span></span>
1. <span data-ttu-id="ab89b-343">Skapa eller redigera hello `/etc/sysconfig/network` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-343">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. <span data-ttu-id="ab89b-344">Skapa eller redigera hello `/etc/sysconfig/network-scripts/ifcfg-eth0` filen och Lägg till hello följande text:</span><span class="sxs-lookup"><span data-stu-id="ab89b-344">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. <span data-ttu-id="ab89b-345">Se till att hello nätverkstjänsten startar när datorn startas genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-345">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

4. <span data-ttu-id="ab89b-346">Registrera din Red Hat prenumeration tooenable hello installation av paket från hello RHEL databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-346">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. <span data-ttu-id="ab89b-347">Ändra hello kernel Start rad i grub configuration tooinclude ytterligare kernel parametrarna för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-347">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="ab89b-348">toodo ändringen öppna `/etc/default/grub` i en textredigerare och redigera hello `GRUB_CMDLINE_LINUX` parameter.</span><span class="sxs-lookup"><span data-stu-id="ab89b-348">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="ab89b-349">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ab89b-349">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="ab89b-350">Den här konfigurationen säkerställer också att alla konsolmeddelanden skickas toohello första serieport, som kan hjälpa Azure support med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="ab89b-350">This configuration also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="ab89b-351">Även stängs av hello nya RHEL 7 namnkonventioner för nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ab89b-351">It also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="ab89b-352">Vi rekommenderar dessutom att du tar bort hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ab89b-352">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="ab89b-353">Grafisk och tyst Start inte är användbara i en molnmiljö där vi vill att alla hello loggar toobe skickas toohello serieport.</span><span class="sxs-lookup"><span data-stu-id="ab89b-353">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="ab89b-354">Du kan lämna hello `crashkernel` alternativet konfigureras om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ab89b-354">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="ab89b-355">Observera att den här parametern minskar hello mängden tillgängligt minne i hello virtuella datorn med 128 MB eller mer, vilket kan vara problematiskt på mindre storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab89b-355">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

6. <span data-ttu-id="ab89b-356">När du är klar med redigering `/etc/default/grub`kör hello följande toorebuild hello grub konfiguration:</span><span class="sxs-lookup"><span data-stu-id="ab89b-356">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. <span data-ttu-id="ab89b-357">Lägg till Hyper-V-moduler tooinitramfs.</span><span class="sxs-lookup"><span data-stu-id="ab89b-357">Add Hyper-V modules tooinitramfs.</span></span>

    <span data-ttu-id="ab89b-358">Redigera `/etc/dracut.conf`, lägga till innehåll:</span><span class="sxs-lookup"><span data-stu-id="ab89b-358">Edit `/etc/dracut.conf`, add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="ab89b-359">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-359">Rebuild initramfs:</span></span>

        # dracut -f -v

8. <span data-ttu-id="ab89b-360">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="ab89b-360">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="ab89b-361">Den här inställningen är vanligtvis hello standard.</span><span class="sxs-lookup"><span data-stu-id="ab89b-361">This setting is usually hello default.</span></span> <span data-ttu-id="ab89b-362">Ändra `/etc/ssh/sshd_config` tooinclude hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="ab89b-362">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

9. <span data-ttu-id="ab89b-363">Hej WALinuxAgent paketet `WALinuxAgent-<version>`, har aviserats toohello Red Hat tillägg databasen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-363">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="ab89b-364">Aktivera hello tillägg databasen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-364">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. <span data-ttu-id="ab89b-365">Installera hello Azure Linux-agenten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab89b-365">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. <span data-ttu-id="ab89b-366">Skapa inte växlingsutrymme på hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="ab89b-366">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="ab89b-367">hello Azure Linux-agenten kan automatiskt konfigurera växlingsutrymme med hello lokala resursen disk som är bifogade toohello virtuell dator efter hello virtuella datorn har etablerats på Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-367">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="ab89b-368">Observera att hello lokala resursen är en tillfällig disk och kan tömmas när hello virtuella datorn är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="ab89b-368">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="ab89b-369">När du installerar hello Azure Linux-agenten i hello föregående steg, ändra hello följande parametrar i `/etc/waagent.conf` på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="ab89b-369">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. <span data-ttu-id="ab89b-370">Om du vill toounregister hello prenumeration kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ab89b-370">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

13. <span data-ttu-id="ab89b-371">Kör hello följande kommandon toodeprovision hello virtuell dator och förbereda den för att etablera i Azure:</span><span class="sxs-lookup"><span data-stu-id="ab89b-371">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. <span data-ttu-id="ab89b-372">Stäng hello virtuella datorn och konvertera hello VMDK-filen toohello VHD-format.</span><span class="sxs-lookup"><span data-stu-id="ab89b-372">Shut down hello virtual machine, and convert hello VMDK file toohello VHD format.</span></span>

    <span data-ttu-id="ab89b-373">Först konvertera hello bildformat tooraw:</span><span class="sxs-lookup"><span data-stu-id="ab89b-373">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    <span data-ttu-id="ab89b-374">Kontrollera att hello storleken på hello raw-bild är i linje med 1 MB.</span><span class="sxs-lookup"><span data-stu-id="ab89b-374">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="ab89b-375">Avrunda annars hello storlek tooalign med 1 MB:</span><span class="sxs-lookup"><span data-stu-id="ab89b-375">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="ab89b-376">Konvertera hello rådata disk tooa fast storlek VHD:</span><span class="sxs-lookup"><span data-stu-id="ab89b-376">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a><span data-ttu-id="ab89b-377">Förbereda en Red Hat-baserad virtuell dator från en ISO med hjälp av en kickstart fil automatiskt</span><span class="sxs-lookup"><span data-stu-id="ab89b-377">Prepare a Red Hat-based virtual machine from an ISO by using a kickstart file automatically</span></span>
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a><span data-ttu-id="ab89b-378">Förbered en RHEL 7 virtuell dator från en kickstart-fil</span><span class="sxs-lookup"><span data-stu-id="ab89b-378">Prepare a RHEL 7 virtual machine from a kickstart file</span></span>

1.  <span data-ttu-id="ab89b-379">Skapa en kickstart-fil som innehåller hello efter innehåll och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ab89b-379">Create a kickstart file that includes hello following content, and save hello file.</span></span> <span data-ttu-id="ab89b-380">Mer information om installation av kickstart finns hello [Kickstart installationsguiden](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span><span class="sxs-lookup"><span data-stu-id="ab89b-380">For details about kickstart installation, see hello [Kickstart Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span></span>

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

2. <span data-ttu-id="ab89b-381">Placera hello kickstart fil där hello installationen system kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="ab89b-381">Place hello kickstart file where hello installation system can access it.</span></span>

3. <span data-ttu-id="ab89b-382">Skapa en ny virtuell dator i Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="ab89b-382">In Hyper-V Manager, create a new virtual machine.</span></span> <span data-ttu-id="ab89b-383">På hello **Anslut virtuell hårddisk** väljer **koppla en virtuell hårddisk senare**, och fullständig hello guiden Ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ab89b-383">On hello **Connect Virtual Hard Disk** page, select **Attach a virtual hard disk later**, and complete hello New Virtual Machine Wizard.</span></span>

4. <span data-ttu-id="ab89b-384">Öppna hello inställningar för virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="ab89b-384">Open hello virtual machine settings:</span></span>

    <span data-ttu-id="ab89b-385">a.</span><span class="sxs-lookup"><span data-stu-id="ab89b-385">a.</span></span>  <span data-ttu-id="ab89b-386">Koppla en ny virtuell hårddisk toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ab89b-386">Attach a new virtual hard disk toohello virtual machine.</span></span> <span data-ttu-id="ab89b-387">Se till att tooselect **VHD-Format** och **med fast storlek**.</span><span class="sxs-lookup"><span data-stu-id="ab89b-387">Make sure tooselect **VHD Format** and **Fixed Size**.</span></span>

    <span data-ttu-id="ab89b-388">b.</span><span class="sxs-lookup"><span data-stu-id="ab89b-388">b.</span></span>  <span data-ttu-id="ab89b-389">Koppla hello installationen ISO toohello DVD-enhet.</span><span class="sxs-lookup"><span data-stu-id="ab89b-389">Attach hello installation ISO toohello DVD drive.</span></span>

    <span data-ttu-id="ab89b-390">c.</span><span class="sxs-lookup"><span data-stu-id="ab89b-390">c.</span></span>  <span data-ttu-id="ab89b-391">Ange hello BIOS tooboot från CD: N.</span><span class="sxs-lookup"><span data-stu-id="ab89b-391">Set hello BIOS tooboot from CD.</span></span>

5. <span data-ttu-id="ab89b-392">Starta hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ab89b-392">Start hello virtual machine.</span></span> <span data-ttu-id="ab89b-393">När installationsguiden för hello visas trycker **fliken** tooconfigure hello startalternativ.</span><span class="sxs-lookup"><span data-stu-id="ab89b-393">When hello installation guide appears, press **Tab** tooconfigure hello boot options.</span></span>

6. <span data-ttu-id="ab89b-394">Ange `inst.ks=<hello location of hello kickstart file>` hello slutet av hello startalternativ och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ab89b-394">Enter `inst.ks=<hello location of hello kickstart file>` at hello end of hello boot options, and press **Enter**.</span></span>

7. <span data-ttu-id="ab89b-395">Vänta tills hello installation toofinish.</span><span class="sxs-lookup"><span data-stu-id="ab89b-395">Wait for hello installation toofinish.</span></span> <span data-ttu-id="ab89b-396">När den är klar, stängs hello virtuella datorn automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ab89b-396">When it's finished, hello virtual machine will be shut down automatically.</span></span> <span data-ttu-id="ab89b-397">Din Linux VHD är nu redo toobe upp tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-397">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="known-issues"></a><span data-ttu-id="ab89b-398">Kända problem</span><span class="sxs-lookup"><span data-stu-id="ab89b-398">Known issues</span></span>
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a><span data-ttu-id="ab89b-399">hello Hyper-V-drivrutinen kan inte ingå i hello inledande RAM-disk när du använder en icke-Hyper-V hypervisor-program</span><span class="sxs-lookup"><span data-stu-id="ab89b-399">hello Hyper-V driver could not be included in hello initial RAM disk when using a non-Hyper-V hypervisor</span></span>

<span data-ttu-id="ab89b-400">I vissa fall kanske Linux installationsprogram inte innehåller hello drivrutiner för Hyper-V i hello inledande RAMDISK (initrd eller initramfs) såvida inte Linux upptäcker att den körs i en miljö med Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="ab89b-400">In some cases, Linux installers might not include hello drivers for Hyper-V in hello initial RAM disk (initrd or initramfs) unless Linux detects that it is running in a Hyper-V environment.</span></span>

<span data-ttu-id="ab89b-401">När du använder en annan virtualisering system (det vill säga Virtualbox, Xen o.s.v.) tooprepare Linux avbildningen kan du behöva toorebuild initrd tooensure som minst hello hv_vmbus och hv_storvsc kernel-moduler är tillgängliga på hello inledande RAM-disk.</span><span class="sxs-lookup"><span data-stu-id="ab89b-401">When you're using a different virtualization system (that is, Virtualbox, Xen, etc.) tooprepare your Linux image, you might need toorebuild initrd tooensure that at least hello hv_vmbus and hv_storvsc kernel modules are available on hello initial RAM disk.</span></span> <span data-ttu-id="ab89b-402">Detta är ett känt problem minst på system som är baserade på hello överordnade Red Hat-distribution.</span><span class="sxs-lookup"><span data-stu-id="ab89b-402">This is a known issue at least on systems that are based on hello upstream Red Hat distribution.</span></span>

<span data-ttu-id="ab89b-403">tooresolve problemet, lägga till Hyper-V-moduler tooinitramfs och återskapa den:</span><span class="sxs-lookup"><span data-stu-id="ab89b-403">tooresolve this issue, add Hyper-V modules tooinitramfs and rebuild it:</span></span>

<span data-ttu-id="ab89b-404">Redigera `/etc/dracut.conf`, och Lägg till hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="ab89b-404">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

<span data-ttu-id="ab89b-405">Återskapa initramfs:</span><span class="sxs-lookup"><span data-stu-id="ab89b-405">Rebuild initramfs:</span></span>

        # dracut -f -v

<span data-ttu-id="ab89b-406">Mer information finns i hello information [återskapa initramfs](https://access.redhat.com/solutions/1958).</span><span class="sxs-lookup"><span data-stu-id="ab89b-406">For more details, see hello information about [rebuilding initramfs](https://access.redhat.com/solutions/1958).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab89b-407">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab89b-407">Next steps</span></span>
<span data-ttu-id="ab89b-408">Du är nu redo toouse Red Hat Enterprise Linux virtuell hårddisk toocreate nya virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="ab89b-408">You're now ready toouse your Red Hat Enterprise Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="ab89b-409">Hello första gången som du ladda upp hello VHD-filen tooAzure, se steg 2 och 3 i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ab89b-409">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="ab89b-410">För mer information om hello hypervisorer som är certifierade toorun Red Hat Enterprise Linux, se [hello Red Hat webbplats](https://access.redhat.com/certified-hypervisors).</span><span class="sxs-lookup"><span data-stu-id="ab89b-410">For more details about hello hypervisors that are certified toorun Red Hat Enterprise Linux, see [hello Red Hat website](https://access.redhat.com/certified-hypervisors).</span></span>
