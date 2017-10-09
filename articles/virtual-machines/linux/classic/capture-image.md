---
title: aaaCapture en bild av Linux VM | Microsoft Docs
description: "Lär dig hur toocapture en avbildning av en Linux-baserade Azure virtuell dator (VM) skapas med hello klassiska distributionsmodellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a><span data-ttu-id="8e830-103">Hur toocapture en klassisk virtuell Linux-dator som en bild</span><span class="sxs-lookup"><span data-stu-id="8e830-103">How toocapture a classic Linux virtual machine as an image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8e830-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8e830-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="8e830-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8e830-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="8e830-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8e830-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="8e830-107">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8e830-107">Learn how too[perform these steps using hello Resource Manager model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="8e830-108">Den här artikeln beskrivs hur du toocapture en klassisk Azure virtuell dator (VM) kör Linux som en bild toocreate andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8e830-108">This article shows you how toocapture a classic Azure virtual machine (VM) running Linux as an image toocreate other virtual machines.</span></span> <span data-ttu-id="8e830-109">Den här avbildningen innehåller hello OS-disken och datadiskar kopplade toohello VM.</span><span class="sxs-lookup"><span data-stu-id="8e830-109">This image includes hello OS disk and data disks attached toohello VM.</span></span> <span data-ttu-id="8e830-110">Det finns inget nätverkskonfigurationen, så du måste tooconfigure som när du skapar hello andra Virtuella från hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="8e830-110">It doesn't include networking configuration, so you need tooconfigure that when you create hello other VM from hello image.</span></span>

<span data-ttu-id="8e830-111">Azure lagrar hello bilden under **bilder**, tillsammans med eventuella avbildningar som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="8e830-111">Azure stores hello image under **Images**, along with any images you've uploaded.</span></span> <span data-ttu-id="8e830-112">Mer information om avbildningar finns [om avbildningar av virtuella datorer i Azure][About Virtual Machine Images in Azure].</span><span class="sxs-lookup"><span data-stu-id="8e830-112">For more information about images, see [About Virtual Machine Images in Azure][About Virtual Machine Images in Azure].</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8e830-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8e830-113">Before you begin</span></span>
<span data-ttu-id="8e830-114">Dessa instruktioner förutsätter att du redan har skapat en virtuell Azure-dator med hjälp av hello klassiska distributionsmodellen och konfigurerade hello operativsystemet, inklusive bifoga datadiskar.</span><span class="sxs-lookup"><span data-stu-id="8e830-114">These steps assume that you've already created an Azure VM using hello Classic deployment model and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="8e830-115">Om du behöver toocreate en virtuell dator kan läsa [hur tooCreate en Linux-dator][How tooCreate a Linux Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="8e830-115">If you need toocreate a VM, read [How tooCreate a Linux Virtual Machine][How tooCreate a Linux Virtual Machine].</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="8e830-116">Avbilda hello virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8e830-116">Capture hello virtual machine</span></span>
1. <span data-ttu-id="8e830-117">[Ansluta toohello VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med hjälp av en SSH-klient av önskat slag.</span><span class="sxs-lookup"><span data-stu-id="8e830-117">[Connect toohello VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) using an SSH client of your choice.</span></span>
2. <span data-ttu-id="8e830-118">Ange följande kommando hello i hello SSH fönstret.</span><span class="sxs-lookup"><span data-stu-id="8e830-118">In hello SSH window, type hello following command.</span></span> <span data-ttu-id="8e830-119">hello utdata från `waagent` kan variera beroende på hello version av det här verktyget:</span><span class="sxs-lookup"><span data-stu-id="8e830-119">hello output from `waagent` may vary slightly depending on hello version of this utility:</span></span>

    ```bash
    sudo waagent -deprovision+user
    ```

    <span data-ttu-id="8e830-120">hello föregående kommando försöker tooclean hello system och gör den lämplig för reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="8e830-120">hello preceding command attempts tooclean hello system and make it suitable for reprovisioning.</span></span> <span data-ttu-id="8e830-121">Den här åtgärden utför hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8e830-121">This operation performs hello following tasks:</span></span>

   * <span data-ttu-id="8e830-122">Tar bort SSH-nycklar för värd (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen för hello)</span><span class="sxs-lookup"><span data-stu-id="8e830-122">Removes SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="8e830-123">Rensar nameserver konfigurationen i /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="8e830-123">Clears nameserver configuration in /etc/resolv.conf</span></span>
   * <span data-ttu-id="8e830-124">Tar bort hello `root` användarlösenord från/etc/shadow (om Provisioning.DeleteRootPassword ”y” i konfigurationsfilen för hello)</span><span class="sxs-lookup"><span data-stu-id="8e830-124">Removes hello `root` user password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="8e830-125">Tar bort cachelagrade lån för DHCP-klienter</span><span class="sxs-lookup"><span data-stu-id="8e830-125">Removes cached DHCP client leases</span></span>
   * <span data-ttu-id="8e830-126">Återställer värd namn toolocalhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="8e830-126">Resets host name toolocalhost.localdomain</span></span>
   * <span data-ttu-id="8e830-127">Tar bort hello senaste etablerade användarkonto (hämtades från /var/lib/waagent) **och tillhörande data**.</span><span class="sxs-lookup"><span data-stu-id="8e830-127">Deletes hello last provisioned user account (obtained from /var/lib/waagent) **and associated data**.</span></span>

     > [!NOTE]
     > <span data-ttu-id="8e830-128">Avetablering tar bort filer och data för ”generalisera” hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="8e830-128">Deprovisioning deletes files and data too"generalize" hello image.</span></span> <span data-ttu-id="8e830-129">Endast köra kommandot på en virtuell dator som du avser toocapture som en ny mall för avbildningen.</span><span class="sxs-lookup"><span data-stu-id="8e830-129">Only run this command on a VM that you intend toocapture as a new image template.</span></span> <span data-ttu-id="8e830-130">Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för Vidaredistribution toothird parter.</span><span class="sxs-lookup"><span data-stu-id="8e830-130">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution toothird parties.</span></span>

3. <span data-ttu-id="8e830-131">Typen **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="8e830-131">Type **y** toocontinue.</span></span> <span data-ttu-id="8e830-132">Du kan lägga till hello `-force` parametern tooavoid åtgärden bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="8e830-132">You can add hello `-force` parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="8e830-133">Typen **avsluta** tooclose hello SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="8e830-133">Type **Exit** tooclose hello SSH client.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8e830-134">hello stegen förutsätter att du redan har [installerat hello Azure CLI](../../../cli-install-nodejs.md) på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="8e830-134">hello remaining steps assume you have already [installed hello Azure CLI](../../../cli-install-nodejs.md) on your client computer.</span></span> <span data-ttu-id="8e830-135">Alla hello följande steg kan även utföras i hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8e830-135">All hello following steps can also be done in hello [Azure portal](http://portal.azure.com).</span></span>

5. <span data-ttu-id="8e830-136">Öppna Azure CLI och inloggningen tooyour Azure-prenumeration från din klientdator.</span><span class="sxs-lookup"><span data-stu-id="8e830-136">From your client computer, open Azure CLI and login tooyour Azure subscription.</span></span> <span data-ttu-id="8e830-137">Mer information läser [ansluta tooan Azure-prenumeration från hello Azure CLI](../../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="8e830-137">For details, read [Connect tooan Azure subscription from hello Azure CLI](../../../xplat-cli-connect.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="8e830-138">Logga in toohello portalen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8e830-138">In hello Azure portal, log in toohello portal.</span></span>

6. <span data-ttu-id="8e830-139">Kontrollera att du befinner dig i Service Management-läge:</span><span class="sxs-lookup"><span data-stu-id="8e830-139">Make sure you are in Service Management mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

7. <span data-ttu-id="8e830-140">Stäng hello virtuell dator som redan avetableras.</span><span class="sxs-lookup"><span data-stu-id="8e830-140">Shut down hello VM that is already deprovisioned.</span></span> <span data-ttu-id="8e830-141">hello följande exempel stängs av hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e830-141">hello following example shuts down hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm shutdown myVM
    ```
   <span data-ttu-id="8e830-142">Om det behövs, kan du visa en lista över alla hello virtuella datorer som skapats i din prenumeration med hjälp av`azure vm list`</span><span class="sxs-lookup"><span data-stu-id="8e830-142">If needed, you can view a list all hello VMs created in your subscription by using `azure vm list`</span></span>

   > [!NOTE]
   > <span data-ttu-id="8e830-143">Om du använder hello Azure-portalen, Välj hello VM och klicka på **stoppa** stänga hello VM.</span><span class="sxs-lookup"><span data-stu-id="8e830-143">If you're using hello Azure portal, select hello VM and click **Stop** to shut down hello VM.</span></span>

8. <span data-ttu-id="8e830-144">När hello VM stoppas avbildning hello.</span><span class="sxs-lookup"><span data-stu-id="8e830-144">When hello VM is stopped, capture hello image.</span></span> <span data-ttu-id="8e830-145">följande exempel insamlingar hello hello virtuella datorn med namnet `myVM` och skapar en generaliserad avbildning med namnet `myNewVM`:</span><span class="sxs-lookup"><span data-stu-id="8e830-145">hello following example captures hello VM named `myVM` and creates a generalized image named `myNewVM`:</span></span>

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    <span data-ttu-id="8e830-146">Hej `-t` underkommandot borttagningar hello ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8e830-146">hello `-t` subcommand deletes hello original virtual machine.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8e830-147">I hello Azure-portalen, kan du göra en avbildning genom att välja **bild** hello hub-menyn.</span><span class="sxs-lookup"><span data-stu-id="8e830-147">In hello Azure portal, you can capture an image by selecting **Image** from hello hub menu.</span></span> <span data-ttu-id="8e830-148">Du behöver toosupply hello följande information för hello bild: namn, resursgruppens namn, plats, typ av operativsystem och blob-sökvägen för lagring.</span><span class="sxs-lookup"><span data-stu-id="8e830-148">You need toosupply hello following information for hello image: name, resource group, location, operating system type, and storage blob path.</span></span>

9. <span data-ttu-id="8e830-149">hello ny avbildning är nu tillgänglig hello listan över avbildningar som kan vara används tooconfigure någon ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8e830-149">hello new image is now available in hello list of images that can be used tooconfigure any new VM.</span></span> <span data-ttu-id="8e830-150">Du kan visa den med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="8e830-150">You can view it with hello command:</span></span>

   ```azurecli
   azure vm image list
   ```

   <span data-ttu-id="8e830-151">På hello [Azure-portalen](http://portal.azure.com), visas hello bilden i hello **VM-avbildningar (klassisk)** som tillhör toohello **Compute** tjänster.</span><span class="sxs-lookup"><span data-stu-id="8e830-151">On hello [Azure portal](http://portal.azure.com), hello new image appears in hello **VM images (classic)** that belongs toohello **Compute** services.</span></span> <span data-ttu-id="8e830-152">Du kan komma åt **VM-avbildningar (klassisk)** genom att klicka på _fler tjänster_ längst hello hello Azure service lista och titta sedan hello **Compute** tjänster.</span><span class="sxs-lookup"><span data-stu-id="8e830-152">You can access **VM images (classic)** by clicking _More services_ at hello bottom of hello Azure service list, and then looking in hello **Compute** services.</span></span>   

   ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="8e830-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e830-154">Next steps</span></span>
<span data-ttu-id="8e830-155">hello bilden är klar toobe används toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8e830-155">hello image is ready toobe used toocreate VMs.</span></span> <span data-ttu-id="8e830-156">Du kan använda hello Azure CLI kommando `azure vm create` och ange hello Avbildningsnamnet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8e830-156">You can use hello Azure CLI command `azure vm create` and supply hello image name you created.</span></span> <span data-ttu-id="8e830-157">Mer information finns i [Using hello Azure CLI med klassiska distributionsmodellen](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8e830-157">For more information, see [Using hello Azure CLI with Classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

<span data-ttu-id="8e830-158">Du kan också använda hello [Azure-portalen](http://portal.azure.com) toocreate en egen virtuell dator med hjälp av hello **bild** metod och välja hello-avbildning som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8e830-158">Alternatively, use hello [Azure portal](http://portal.azure.com) toocreate a custom VM by using hello **Image** method and selecting hello image you created.</span></span> <span data-ttu-id="8e830-159">Mer information finns i [hur tooCreate en anpassad VM][How tooCreate a Custom Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="8e830-159">For more information, see [How tooCreate a Custom VM][How tooCreate a Custom Virtual Machine].</span></span>

<span data-ttu-id="8e830-160">**Se även:** [användarhandboken för Azure Linux-Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="8e830-160">**See also:** [Azure Linux Agent User Guide](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
