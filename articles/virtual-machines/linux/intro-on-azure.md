---
title: aaaIntroduction tooLinux i Azure | Microsoft Docs
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
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a><span data-ttu-id="a77c0-103">Introduktion tooLinux på Azure</span><span class="sxs-lookup"><span data-stu-id="a77c0-103">Introduction tooLinux on Azure</span></span>
<span data-ttu-id="a77c0-104">Det här avsnittet innehåller en översikt över vissa aspekter av med hjälp av virtuella Linux-datorer i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="a77c0-104">This topic provides an overview of some aspects of using Linux virtual machines in hello Azure cloud.</span></span> <span data-ttu-id="a77c0-105">Distribuera en virtuell Linux-dator är enkelt att med hjälp av en avbildning från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="a77c0-105">Deploying a Linux virtual machine is a straightforward process using an image from hello gallery.</span></span>

## <a name="authentication-usernames-passwords-and-ssh-keys"></a><span data-ttu-id="a77c0-106">Autentisering: Användarnamn, lösenord och SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="a77c0-106">Authentication: Usernames, Passwords and SSH Keys</span></span>
<span data-ttu-id="a77c0-107">När du skapar en Linux-dator med hjälp av hello Azure-portalen, du uppmanas tooprovide antingen ett användarnamn och lösenord eller en offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="a77c0-107">When creating a Linux virtual machine using hello Azure portal, you are asked tooprovide a either username and password or an SSH public key.</span></span> <span data-ttu-id="a77c0-108">hello valet av ett användarnamn för att distribuera en virtuell Linux-dator i Azure är ämne toohello följande villkor: namnen på Systemkonton (UID < 100) finns redan i hello virtuell dator är inte tillåtna, ”rot” till exempel.</span><span class="sxs-lookup"><span data-stu-id="a77c0-108">hello choice of a username for deploying a Linux virtual machine on Azure is subject toohello following constraint: names of system accounts (UID <100) already present in hello virtual machine are not allowed, 'root' for example.</span></span>

* <span data-ttu-id="a77c0-109">Se [skapa en virtuell dator som kör Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-109">See [Create a Virtual Machine Running Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="a77c0-110">Se [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-110">See [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="obtaining-superuser-privileges-using-sudo"></a><span data-ttu-id="a77c0-111">Hämta superanvändare använder`sudo`</span><span class="sxs-lookup"><span data-stu-id="a77c0-111">Obtaining Superuser Privileges Using `sudo`</span></span>
<span data-ttu-id="a77c0-112">hello-användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier.</span><span class="sxs-lookup"><span data-stu-id="a77c0-112">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="a77c0-113">Det här kontot konfigureras genom att använda hello hello Azure Linux-agenten toobe kan tooelevate privilegier tooroot (superanvändare konto) `sudo` verktyget.</span><span class="sxs-lookup"><span data-stu-id="a77c0-113">This account is configured by hello Azure Linux Agent toobe able tooelevate privileges tooroot (superuser account) using hello `sudo` utility.</span></span> <span data-ttu-id="a77c0-114">När du loggade in med det här användarkontot, kommer du att kan toorun kommandon som rot med hello Kommandosyntax:</span><span class="sxs-lookup"><span data-stu-id="a77c0-114">Once logged in using this user account, you will be able toorun commands as root using hello command syntax:</span></span>

    # sudo <COMMAND>

<span data-ttu-id="a77c0-115">Du kan eventuellt hämta ett rot-gränssnittet med **sudo -s**.</span><span class="sxs-lookup"><span data-stu-id="a77c0-115">You can optionally obtain a root shell using **sudo -s**.</span></span>

* <span data-ttu-id="a77c0-116">Se [med rotprivilegier på Linux-datorer i Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-116">See [Using root privileges on Linux virtual machines in Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="firewall-configuration"></a><span data-ttu-id="a77c0-117">Brandväggskonfiguration</span><span class="sxs-lookup"><span data-stu-id="a77c0-117">Firewall Configuration</span></span>
<span data-ttu-id="a77c0-118">Azure tillhandahåller ett inkommande paketfilter som begränsar anslutningen tooports anges i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a77c0-118">Azure provides an inbound packet filter that restricts connectivity tooports specified in hello Azure portal.</span></span> <span data-ttu-id="a77c0-119">Som standard hello endast tillåtna porten är SSH.</span><span class="sxs-lookup"><span data-stu-id="a77c0-119">By default, hello only allowed port is SSH.</span></span> <span data-ttu-id="a77c0-120">Du kan öppna in åtkomst tooadditional portar på den virtuella Linux-datorn genom att konfigurera slutpunkter i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="a77c0-120">You may open up access tooadditional ports on your Linux virtual machine by configuring endpoints in hello Azure portal:</span></span>

* <span data-ttu-id="a77c0-121">Se: [hur tooSet in slutpunkter tooa virtuell dator](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-121">See: [How tooSet Up Endpoints tooa Virtual Machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="a77c0-122">hello Linux bilder i hello Azure-galleriet inte aktiverar hello *iptables* brandväggen som standard.</span><span class="sxs-lookup"><span data-stu-id="a77c0-122">hello Linux images in hello Azure Gallery do not enable hello *iptables* firewall by default.</span></span> <span data-ttu-id="a77c0-123">Om du vill hello brandväggen kan konfigureras tooprovide ytterligare filtrering.</span><span class="sxs-lookup"><span data-stu-id="a77c0-123">If desired, hello firewall may be configured tooprovide additional filtering.</span></span>

## <a name="hostname-changes"></a><span data-ttu-id="a77c0-124">Hostname ändringar</span><span class="sxs-lookup"><span data-stu-id="a77c0-124">Hostname Changes</span></span>
<span data-ttu-id="a77c0-125">När du distribuerar en instans av en Linux-avbildning från början, är obligatoriska tooprovide ett värdnamn för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a77c0-125">When you initially deploy an instance of a Linux image, you are required tooprovide a host name for hello virtual machine.</span></span> <span data-ttu-id="a77c0-126">När hello virtuella datorn körs, är det här värdnamnet publicerade toohello plattform DNS-servrar så att flera virtuella datorer anslutna tooeach andra kan utföra IP-adress sökningar med värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a77c0-126">Once hello virtual machine is running, this hostname is published toohello platform DNS servers so that multiple virtual machines connected tooeach other can perform IP address lookups using hostnames.</span></span>

<span data-ttu-id="a77c0-127">Om värdnamnet ändringar önskas när en virtuell dator har distribuerats, Använd hello kommandot</span><span class="sxs-lookup"><span data-stu-id="a77c0-127">If hostname changes are desired after a virtual machine has been deployed, please use hello command</span></span>

    # sudo hostname <newname>

<span data-ttu-id="a77c0-128">hello Azure Linux-agenten innehåller funktioner tooautomatically identifiera den här namnändringen korrekt hello virtuella toopersist ändringen och konfigurera publicera den här ändringen toohello plattform DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="a77c0-128">hello Azure Linux Agent includes functionality tooautomatically detect this name change and appropriately configure hello virtual machine toopersist this change and publish this change toohello platform DNS servers.</span></span>

* [<span data-ttu-id="a77c0-129">Användarhandboken för Azure Linux-Agent</span><span class="sxs-lookup"><span data-stu-id="a77c0-129">Azure Linux Agent User Guide</span></span>](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a><span data-ttu-id="a77c0-130">Molnet initiering</span><span class="sxs-lookup"><span data-stu-id="a77c0-130">Cloud-Init</span></span>
<span data-ttu-id="a77c0-131">**Ubuntu** och **virtuell CoreOS** bilder använda molnet initiering på Azure, vilket ger ytterligare funktioner för att starta en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a77c0-131">**Ubuntu** and **CoreOS** images utilize cloud-init on Azure, which provides additional capabilities for bootstrapping a virtual machine.</span></span>

* [<span data-ttu-id="a77c0-132">Hur tooInject anpassade Data</span><span class="sxs-lookup"><span data-stu-id="a77c0-132">How tooInject Custom Data</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="a77c0-133">Anpassade Data och initiering molnet i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a77c0-133">Custom Data and Cloud-Init on Microsoft Azure</span></span>](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [<span data-ttu-id="a77c0-134">Skapa Azure växlingen partitioner som använder molnet initiering</span><span class="sxs-lookup"><span data-stu-id="a77c0-134">Create Azure Swap Partitions Using Cloud-Init</span></span>](https://wiki.ubuntu.com/AzureSwapPartitions)
* [<span data-ttu-id="a77c0-135">Hur tooUse virtuell CoreOS på Azure</span><span class="sxs-lookup"><span data-stu-id="a77c0-135">How tooUse CoreOS on Azure</span></span>](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a><span data-ttu-id="a77c0-136">Avbildningen för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a77c0-136">Virtual Machine Image Capture</span></span>
<span data-ttu-id="a77c0-137">Azure tillhandahåller hello möjlighet toocapture hello tillståndet för en befintlig virtuell dator till en bild som kan senare använda toodeploy ytterligare virtuella instanser.</span><span class="sxs-lookup"><span data-stu-id="a77c0-137">Azure provides hello ability toocapture hello state of an existing virtual machine into an image that can subsequently be used toodeploy additional virtual machine instances.</span></span> <span data-ttu-id="a77c0-138">hello Azure Linux-agenten kanske används toorollback några av hello anpassning som utfördes under hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="a77c0-138">hello Azure Linux Agent may be used toorollback some of hello customization that was performed during hello provisioning process.</span></span> <span data-ttu-id="a77c0-139">Du kan följa hello stegen nedan toocapture en virtuell dator som en bild:</span><span class="sxs-lookup"><span data-stu-id="a77c0-139">You may follow hello steps below toocapture a virtual machine as an image:</span></span>

1. <span data-ttu-id="a77c0-140">Kör **waagent-deprovision** tooundo etablering anpassning.</span><span class="sxs-lookup"><span data-stu-id="a77c0-140">Run **waagent -deprovision** tooundo provisioning customization.</span></span> <span data-ttu-id="a77c0-141">Eller **waagent-deprovision + användaren** toooptionally ta bort hello-användarkonto som anges under etablering och alla associerade data.</span><span class="sxs-lookup"><span data-stu-id="a77c0-141">Or **waagent -deprovision+user** toooptionally delete hello user account specified during provisioning and all associated data.</span></span>
2. <span data-ttu-id="a77c0-142">Stänga ned/hello virtuella datorn stängas.</span><span class="sxs-lookup"><span data-stu-id="a77c0-142">Shut down/power off hello virtual machine.</span></span>
3. <span data-ttu-id="a77c0-143">Klicka på **avbilda** i hello Azure-portalen eller Använd hello PowerShell eller CLI verktygen toocapture hello virtuell dator som en bild.</span><span class="sxs-lookup"><span data-stu-id="a77c0-143">Click **Capture** in hello Azure portal or use hello PowerShell or CLI tools toocapture hello virtual machine as an image.</span></span>
   
   * <span data-ttu-id="a77c0-144">Se: [hur tooCapture en virtuell Linux-dator tooUse som en mall](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-144">See: [How tooCapture a Linux Virtual Machine tooUse as a Template](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="attaching-disks"></a><span data-ttu-id="a77c0-145">Bifoga diskar</span><span class="sxs-lookup"><span data-stu-id="a77c0-145">Attaching Disks</span></span>
<span data-ttu-id="a77c0-146">Varje virtuell dator har en tillfällig lokala *resurs disk* ansluten.</span><span class="sxs-lookup"><span data-stu-id="a77c0-146">Each virtual machine has a temporary, local *resource disk* attached.</span></span> <span data-ttu-id="a77c0-147">Eftersom data på en resurs kan vara beständig mellan omstarter, det används ofta av program och processer som körs i hello virtuell dator för tillfällig och **tillfälliga** lagring av data.</span><span class="sxs-lookup"><span data-stu-id="a77c0-147">Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in hello virtual machine for transient and **temporary** storage of data.</span></span> <span data-ttu-id="a77c0-148">Det är också används toostore hello sidan eller växla filer för hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="a77c0-148">It is also used toostore hello page or swap files for hello operating system.</span></span>

<span data-ttu-id="a77c0-149">På Linux, hello resurs disk hanteras vanligtvis av hello Azure Linux-agenten och monterade automatiskt för**/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar).</span><span class="sxs-lookup"><span data-stu-id="a77c0-149">On Linux, hello resource disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span>

> [!NOTE]
> <span data-ttu-id="a77c0-150">Observera att hello resurs disken är en **tillfälliga** disk, och som kan tas bort och formateras om när hello VM startas.</span><span class="sxs-lookup"><span data-stu-id="a77c0-150">Note that hello resource disk is a **temporary** disk, and might be deleted and reformatted when hello VM is rebooted.</span></span>
> 
> 

<span data-ttu-id="a77c0-151">På Linux hello datadisk kan namnges med hello-kernel `/dev/sdc`, och användarna måste toopartition, formatera och montera den här resursen.</span><span class="sxs-lookup"><span data-stu-id="a77c0-151">On Linux hello data disk might be named by hello kernel as `/dev/sdc`, and users will need toopartition, format and mount that resource.</span></span> <span data-ttu-id="a77c0-152">Detta beskrivs steg för steg i självstudiekursen hello: [hur tooAttach en datadisk tooa virtuella](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a77c0-152">This is covered step-by-step in hello tutorial: [How tooAttach a Data Disk tooa Virtual Machine](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

* <span data-ttu-id="a77c0-153">**Se även:** [konfigurera programvarubaserad RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurera LVM på en virtuell Linux-dator i Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a77c0-153">**See also:** [Configure Software RAID on Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Configure LVM on a Linux VM in Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

