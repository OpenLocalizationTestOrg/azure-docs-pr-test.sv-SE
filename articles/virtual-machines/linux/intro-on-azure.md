---
title: Introduktion till Linux i Azure | Microsoft Docs
description: "Lär dig mer om hur du använder virtuella Linux-datorer i Azure."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 7bd0c5549a2e1f592681760d5ef464b9570ca4ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-linux-on-azure"></a><span data-ttu-id="c59a2-103">Introduktion till Linux på Azure</span><span class="sxs-lookup"><span data-stu-id="c59a2-103">Introduction to Linux on Azure</span></span>
<span data-ttu-id="c59a2-104">Det här avsnittet innehåller en översikt över vissa aspekter av med hjälp av virtuella Linux-datorer i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="c59a2-104">This topic provides an overview of some aspects of using Linux virtual machines in the Azure cloud.</span></span> <span data-ttu-id="c59a2-105">Distribuera en virtuell Linux-dator är enkelt att använda en bild från galleriet.</span><span class="sxs-lookup"><span data-stu-id="c59a2-105">Deploying a Linux virtual machine is a straightforward process using an image from the gallery.</span></span>

## <a name="authentication-usernames-passwords-and-ssh-keys"></a><span data-ttu-id="c59a2-106">Autentisering: Användarnamn, lösenord och SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="c59a2-106">Authentication: Usernames, Passwords and SSH Keys</span></span>
<span data-ttu-id="c59a2-107">När du skapar en Linux-dator med hjälp av Azure portal, du uppmanas att ange antingen ett användarnamn och lösenord eller en offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c59a2-107">When creating a Linux virtual machine using the Azure portal, you are asked to provide a either username and password or an SSH public key.</span></span> <span data-ttu-id="c59a2-108">Valet av ett användarnamn för att distribuera en virtuell Linux-dator på Azure regleras följande begränsningar: namnen på Systemkonton (UID < 100) finns redan i den virtuella datorn inte är tillåtna, ”rot” till exempel.</span><span class="sxs-lookup"><span data-stu-id="c59a2-108">The choice of a username for deploying a Linux virtual machine on Azure is subject to the following constraint: names of system accounts (UID <100) already present in the virtual machine are not allowed, 'root' for example.</span></span>

* <span data-ttu-id="c59a2-109">Se [skapa en virtuell dator som kör Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-109">See [Create a Virtual Machine Running Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="c59a2-110">Se [hur du använder SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-110">See [How to Use SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="obtaining-superuser-privileges-using-sudo"></a><span data-ttu-id="c59a2-111">Hämta superanvändare använder`sudo`</span><span class="sxs-lookup"><span data-stu-id="c59a2-111">Obtaining Superuser Privileges Using `sudo`</span></span>
<span data-ttu-id="c59a2-112">Det användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier.</span><span class="sxs-lookup"><span data-stu-id="c59a2-112">The user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="c59a2-113">Det här kontot konfigureras med Azure Linux-agenten för att kunna upphöja behörighet till rot (superanvändare konto) med hjälp av den `sudo` verktyget.</span><span class="sxs-lookup"><span data-stu-id="c59a2-113">This account is configured by the Azure Linux Agent to be able to elevate privileges to root (superuser account) using the `sudo` utility.</span></span> <span data-ttu-id="c59a2-114">När du loggade in med det här användarkontot, kan du köra kommandon som rot med hjälp av Kommandosyntax:</span><span class="sxs-lookup"><span data-stu-id="c59a2-114">Once logged in using this user account, you will be able to run commands as root using the command syntax:</span></span>

    # sudo <COMMAND>

<span data-ttu-id="c59a2-115">Du kan eventuellt hämta ett rot-gränssnittet med **sudo -s**.</span><span class="sxs-lookup"><span data-stu-id="c59a2-115">You can optionally obtain a root shell using **sudo -s**.</span></span>

* <span data-ttu-id="c59a2-116">Se [med rotprivilegier på Linux-datorer i Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-116">See [Using root privileges on Linux virtual machines in Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="firewall-configuration"></a><span data-ttu-id="c59a2-117">Brandväggskonfiguration</span><span class="sxs-lookup"><span data-stu-id="c59a2-117">Firewall Configuration</span></span>
<span data-ttu-id="c59a2-118">Azure tillhandahåller ett inkommande paketfilter som begränsar anslutning till portar som anges i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c59a2-118">Azure provides an inbound packet filter that restricts connectivity to ports specified in the Azure portal.</span></span> <span data-ttu-id="c59a2-119">Som standard är endast tillåtna port SSH.</span><span class="sxs-lookup"><span data-stu-id="c59a2-119">By default, the only allowed port is SSH.</span></span> <span data-ttu-id="c59a2-120">Du kan öppna konfigurerar åtkomst till ytterligare portar på den virtuella Linux-datorn genom att konfigurera slutpunkter i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="c59a2-120">You may open up access to additional ports on your Linux virtual machine by configuring endpoints in the Azure portal:</span></span>

* <span data-ttu-id="c59a2-121">Se: [konfigurera slutpunkter till en virtuell dator](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-121">See: [How to Set Up Endpoints to a Virtual Machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="c59a2-122">Linux-avbildningar i Azure-galleriet inte aktiverar den *iptables* brandväggen som standard.</span><span class="sxs-lookup"><span data-stu-id="c59a2-122">The Linux images in the Azure Gallery do not enable the *iptables* firewall by default.</span></span> <span data-ttu-id="c59a2-123">Om du vill kan brandväggen konfigureras för att tillhandahålla ytterligare filtrering.</span><span class="sxs-lookup"><span data-stu-id="c59a2-123">If desired, the firewall may be configured to provide additional filtering.</span></span>

## <a name="hostname-changes"></a><span data-ttu-id="c59a2-124">Hostname ändringar</span><span class="sxs-lookup"><span data-stu-id="c59a2-124">Hostname Changes</span></span>
<span data-ttu-id="c59a2-125">När du distribuerar en instans av en Linux-avbildning från början, måste du ange ett värdnamn för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c59a2-125">When you initially deploy an instance of a Linux image, you are required to provide a host name for the virtual machine.</span></span> <span data-ttu-id="c59a2-126">När den virtuella datorn körs, har värdnamnet publicerats till DNS-servrar plattform så att flera virtuella datorer som är anslutna till varandra kan utföra IP-adress sökningar med värdnamn.</span><span class="sxs-lookup"><span data-stu-id="c59a2-126">Once the virtual machine is running, this hostname is published to the platform DNS servers so that multiple virtual machines connected to each other can perform IP address lookups using hostnames.</span></span>

<span data-ttu-id="c59a2-127">Använd kommandot eventuellt värdnamn ändringar är när en virtuell dator har distribuerats</span><span class="sxs-lookup"><span data-stu-id="c59a2-127">If hostname changes are desired after a virtual machine has been deployed, please use the command</span></span>

    # sudo hostname <newname>

<span data-ttu-id="c59a2-128">Azure Linux-agenten innehåller funktioner för att automatiskt identifiera den här namnändringen och konfigureras på rätt sätt den virtuella datorn för att spara den här ändringen och publicera den här ändringen till plattformen DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="c59a2-128">The Azure Linux Agent includes functionality to automatically detect this name change and appropriately configure the virtual machine to persist this change and publish this change to the platform DNS servers.</span></span>

* [<span data-ttu-id="c59a2-129">Användarhandboken för Azure Linux-Agent</span><span class="sxs-lookup"><span data-stu-id="c59a2-129">Azure Linux Agent User Guide</span></span>](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a><span data-ttu-id="c59a2-130">Molnet initiering</span><span class="sxs-lookup"><span data-stu-id="c59a2-130">Cloud-Init</span></span>
<span data-ttu-id="c59a2-131">**Ubuntu** och **virtuell CoreOS** bilder använda molnet initiering på Azure, vilket ger ytterligare funktioner för att starta en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c59a2-131">**Ubuntu** and **CoreOS** images utilize cloud-init on Azure, which provides additional capabilities for bootstrapping a virtual machine.</span></span>

* [<span data-ttu-id="c59a2-132">Hur att mata in anpassade Data</span><span class="sxs-lookup"><span data-stu-id="c59a2-132">How to Inject Custom Data</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="c59a2-133">Anpassade Data och initiering molnet i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c59a2-133">Custom Data and Cloud-Init on Microsoft Azure</span></span>](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [<span data-ttu-id="c59a2-134">Skapa Azure växlingen partitioner som använder molnet initiering</span><span class="sxs-lookup"><span data-stu-id="c59a2-134">Create Azure Swap Partitions Using Cloud-Init</span></span>](https://wiki.ubuntu.com/AzureSwapPartitions)
* [<span data-ttu-id="c59a2-135">Använda virtuell CoreOS på Azure</span><span class="sxs-lookup"><span data-stu-id="c59a2-135">How to Use CoreOS on Azure</span></span>](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a><span data-ttu-id="c59a2-136">Avbildningen för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c59a2-136">Virtual Machine Image Capture</span></span>
<span data-ttu-id="c59a2-137">Azure tillhandahåller möjligheten att samla in tillståndet för en befintlig virtuell dator till en avbildning som senare kan användas för att distribuera ytterligare virtuella instanser.</span><span class="sxs-lookup"><span data-stu-id="c59a2-137">Azure provides the ability to capture the state of an existing virtual machine into an image that can subsequently be used to deploy additional virtual machine instances.</span></span> <span data-ttu-id="c59a2-138">Azure Linux-agenten kan vara används för att återställa några av de anpassningar som utfördes under etableringen.</span><span class="sxs-lookup"><span data-stu-id="c59a2-138">The Azure Linux Agent may be used to rollback some of the customization that was performed during the provisioning process.</span></span> <span data-ttu-id="c59a2-139">Du kan följa stegen nedan för att avbilda en virtuell dator som en bild:</span><span class="sxs-lookup"><span data-stu-id="c59a2-139">You may follow the steps below to capture a virtual machine as an image:</span></span>

1. <span data-ttu-id="c59a2-140">Kör **waagent-deprovision** Ångra allokering anpassning.</span><span class="sxs-lookup"><span data-stu-id="c59a2-140">Run **waagent -deprovision** to undo provisioning customization.</span></span> <span data-ttu-id="c59a2-141">Eller **waagent-deprovision + användaren** om du vill ta bort användarkontot som anges under etablering och alla associerade data.</span><span class="sxs-lookup"><span data-stu-id="c59a2-141">Or **waagent -deprovision+user** to optionally delete the user account specified during provisioning and all associated data.</span></span>
2. <span data-ttu-id="c59a2-142">Stänga ned/stänga av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c59a2-142">Shut down/power off the virtual machine.</span></span>
3. <span data-ttu-id="c59a2-143">Klicka på **avbilda** i Azure-portalen eller Använd PowerShell eller CLI verktyg för att avbilda den virtuella datorn som en bild.</span><span class="sxs-lookup"><span data-stu-id="c59a2-143">Click **Capture** in the Azure portal or use the PowerShell or CLI tools to capture the virtual machine as an image.</span></span>
   
   * <span data-ttu-id="c59a2-144">Se: [så här skapar du en virtuell Linux-dator för användning som en mall](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-144">See: [How to Capture a Linux Virtual Machine to Use as a Template](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="attaching-disks"></a><span data-ttu-id="c59a2-145">Bifoga diskar</span><span class="sxs-lookup"><span data-stu-id="c59a2-145">Attaching Disks</span></span>
<span data-ttu-id="c59a2-146">Varje virtuell dator har en tillfällig lokala *resurs disk* ansluten.</span><span class="sxs-lookup"><span data-stu-id="c59a2-146">Each virtual machine has a temporary, local *resource disk* attached.</span></span> <span data-ttu-id="c59a2-147">Eftersom data på en resurs kan vara beständig mellan omstarter, det används ofta av program och processer som körs på den virtuella datorn för tillfälligt och **tillfälliga** lagring av data.</span><span class="sxs-lookup"><span data-stu-id="c59a2-147">Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in the virtual machine for transient and **temporary** storage of data.</span></span> <span data-ttu-id="c59a2-148">Den används också för att lagra sidan eller växla filer för operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c59a2-148">It is also used to store the page or swap files for the operating system.</span></span>

<span data-ttu-id="c59a2-149">På Linux, resurs disken är vanligtvis hanteras av Azure Linux-agenten och automatiskt monteras **/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar).</span><span class="sxs-lookup"><span data-stu-id="c59a2-149">On Linux, the resource disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images).</span></span>

> [!NOTE]
> <span data-ttu-id="c59a2-150">Observera att disken som resursen är en **tillfälliga** disk, och som kan tas bort och formateras om när den virtuella datorn har startats om.</span><span class="sxs-lookup"><span data-stu-id="c59a2-150">Note that the resource disk is a **temporary** disk, and might be deleted and reformatted when the VM is rebooted.</span></span>
> 
> 

<span data-ttu-id="c59a2-151">På Linux datadisken kan kallas kerneln som `/dev/sdc`, och användarna måste partitionera, formatera och montera den här resursen.</span><span class="sxs-lookup"><span data-stu-id="c59a2-151">On Linux the data disk might be named by the kernel as `/dev/sdc`, and users will need to partition, format and mount that resource.</span></span> <span data-ttu-id="c59a2-152">Detta beskrivs steg för steg i självstudiekursen: [hur du kopplar en datadisk till en virtuell dator](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c59a2-152">This is covered step-by-step in the tutorial: [How to Attach a Data Disk to a Virtual Machine](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

* <span data-ttu-id="c59a2-153">**Se även:** [konfigurera programvarubaserad RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurera LVM på en virtuell Linux-dator i Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c59a2-153">**See also:** [Configure Software RAID on Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Configure LVM on a Linux VM in Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

