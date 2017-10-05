---
title: "Hämta en avbildning av en Linux-VM | Microsoft Docs"
description: "Lär dig mer om att hämta en avbildning av en Linux-baserade Azure virtuell dator (VM) skapas med den klassiska distributionsmodellen."
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
ms.openlocfilehash: ecde5dd3211bfbb290e6910d7d55136d079c6cf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a><span data-ttu-id="0302e-103">Spara en klassisk virtuell Linux-dator som en avbildning</span><span class="sxs-lookup"><span data-stu-id="0302e-103">How to capture a classic Linux virtual machine as an image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0302e-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0302e-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0302e-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0302e-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="0302e-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="0302e-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="0302e-107">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0302e-107">Learn how to [perform these steps using the Resource Manager model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="0302e-108">Den här artikeln visar hur du skapar en klassiska Azure-dator (VM) kör Linux som en bild för att skapa andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0302e-108">This article shows you how to capture a classic Azure virtual machine (VM) running Linux as an image to create other virtual machines.</span></span> <span data-ttu-id="0302e-109">Den här avbildningen innehåller operativsystemdisken och datadiskar som är kopplade till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0302e-109">This image includes the OS disk and data disks attached to the VM.</span></span> <span data-ttu-id="0302e-110">Det finns inget nätverkskonfigurationen, så du behöver konfigurera som när du skapar den andra virtuella datorn från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0302e-110">It doesn't include networking configuration, so you need to configure that when you create the other VM from the image.</span></span>

<span data-ttu-id="0302e-111">Azure lagrar bilden under **bilder**, tillsammans med eventuella avbildningar som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="0302e-111">Azure stores the image under **Images**, along with any images you've uploaded.</span></span> <span data-ttu-id="0302e-112">Mer information om avbildningar finns [om avbildningar av virtuella datorer i Azure][About Virtual Machine Images in Azure].</span><span class="sxs-lookup"><span data-stu-id="0302e-112">For more information about images, see [About Virtual Machine Images in Azure][About Virtual Machine Images in Azure].</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0302e-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0302e-113">Before you begin</span></span>
<span data-ttu-id="0302e-114">Dessa instruktioner förutsätter att du redan skapat en virtuell Azure-dator med hjälp av den klassiska distributionsmodellen och konfigurerat operativsystemet, inklusive bifoga datadiskar.</span><span class="sxs-lookup"><span data-stu-id="0302e-114">These steps assume that you've already created an Azure VM using the Classic deployment model and configured the operating system, including attaching any data disks.</span></span> <span data-ttu-id="0302e-115">Om du behöver skapa en virtuell dator kan läsa [hur du skapar en virtuell Linux-dator][How to Create a Linux Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="0302e-115">If you need to create a VM, read [How to Create a Linux Virtual Machine][How to Create a Linux Virtual Machine].</span></span>

## <a name="capture-the-virtual-machine"></a><span data-ttu-id="0302e-116">Avbilda den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="0302e-116">Capture the virtual machine</span></span>
1. <span data-ttu-id="0302e-117">[Ansluta till den virtuella datorn](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med hjälp av en SSH-klient av önskat slag.</span><span class="sxs-lookup"><span data-stu-id="0302e-117">[Connect to the VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) using an SSH client of your choice.</span></span>
2. <span data-ttu-id="0302e-118">Skriv följande kommando i SSH-fönstret.</span><span class="sxs-lookup"><span data-stu-id="0302e-118">In the SSH window, type the following command.</span></span> <span data-ttu-id="0302e-119">Utdata från `waagent` kan variera beroende på vilken version av det här verktyget:</span><span class="sxs-lookup"><span data-stu-id="0302e-119">The output from `waagent` may vary slightly depending on the version of this utility:</span></span>

    ```bash
    sudo waagent -deprovision+user
    ```

    <span data-ttu-id="0302e-120">Föregående kommando försöker att rensa systemet och gör den lämplig för reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="0302e-120">The preceding command attempts to clean the system and make it suitable for reprovisioning.</span></span> <span data-ttu-id="0302e-121">Den här åtgärden utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="0302e-121">This operation performs the following tasks:</span></span>

   * <span data-ttu-id="0302e-122">Tar bort SSH-nycklar för värd (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen)</span><span class="sxs-lookup"><span data-stu-id="0302e-122">Removes SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in the configuration file)</span></span>
   * <span data-ttu-id="0302e-123">Rensar nameserver konfigurationen i /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="0302e-123">Clears nameserver configuration in /etc/resolv.conf</span></span>
   * <span data-ttu-id="0302e-124">Tar bort den `root` användarlösenord från/etc/shadow (om Provisioning.DeleteRootPassword ”y” i konfigurationsfilen)</span><span class="sxs-lookup"><span data-stu-id="0302e-124">Removes the `root` user password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in the configuration file)</span></span>
   * <span data-ttu-id="0302e-125">Tar bort cachelagrade lån för DHCP-klienter</span><span class="sxs-lookup"><span data-stu-id="0302e-125">Removes cached DHCP client leases</span></span>
   * <span data-ttu-id="0302e-126">Återställer värdnamnet till localhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="0302e-126">Resets host name to localhost.localdomain</span></span>
   * <span data-ttu-id="0302e-127">Tar bort sist etablerats användarkonto (hämtades från /var/lib/waagent) **och tillhörande data**.</span><span class="sxs-lookup"><span data-stu-id="0302e-127">Deletes the last provisioned user account (obtained from /var/lib/waagent) **and associated data**.</span></span>

     > [!NOTE]
     > <span data-ttu-id="0302e-128">Avetablering tar bort filer och data att ”generalisera” bilden.</span><span class="sxs-lookup"><span data-stu-id="0302e-128">Deprovisioning deletes files and data to "generalize" the image.</span></span> <span data-ttu-id="0302e-129">Endast köra kommandot på en virtuell dator som du vill samla in som en ny mall för avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0302e-129">Only run this command on a VM that you intend to capture as a new image template.</span></span> <span data-ttu-id="0302e-130">Är det inte säkert att avbildningen är avmarkerad av all känslig information eller lämpar sig för omfördelning till tredje part.</span><span class="sxs-lookup"><span data-stu-id="0302e-130">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution to third parties.</span></span>

3. <span data-ttu-id="0302e-131">Typen **y** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="0302e-131">Type **y** to continue.</span></span> <span data-ttu-id="0302e-132">Du kan lägga till den `-force` parametern för att undvika det här steget bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="0302e-132">You can add the `-force` parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="0302e-133">Typen **avsluta** att stänga SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="0302e-133">Type **Exit** to close the SSH client.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0302e-134">Stegen förutsätter att du redan har [installerade Azure CLI](../../../cli-install-nodejs.md) på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="0302e-134">The remaining steps assume you have already [installed the Azure CLI](../../../cli-install-nodejs.md) on your client computer.</span></span> <span data-ttu-id="0302e-135">Följande steg kan även utföras den [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0302e-135">All the following steps can also be done in the [Azure portal](http://portal.azure.com).</span></span>

5. <span data-ttu-id="0302e-136">Öppna Azure CLI och logga in på Azure-prenumerationen från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="0302e-136">From your client computer, open Azure CLI and login to your Azure subscription.</span></span> <span data-ttu-id="0302e-137">Mer information läser [Anslut till en Azure-prenumeration från Azure CLI](../../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="0302e-137">For details, read [Connect to an Azure subscription from the Azure CLI](../../../xplat-cli-connect.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="0302e-138">Logga in på portalen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0302e-138">In the Azure portal, log in to the portal.</span></span>

6. <span data-ttu-id="0302e-139">Kontrollera att du befinner dig i Service Management-läge:</span><span class="sxs-lookup"><span data-stu-id="0302e-139">Make sure you are in Service Management mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

7. <span data-ttu-id="0302e-140">Stäng av den virtuella datorn som redan avetableras.</span><span class="sxs-lookup"><span data-stu-id="0302e-140">Shut down the VM that is already deprovisioned.</span></span> <span data-ttu-id="0302e-141">I följande exempel stängs den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="0302e-141">The following example shuts down the VM named `myVM`:</span></span>

    ```azurecli
    azure vm shutdown myVM
    ```
   <span data-ttu-id="0302e-142">Om det behövs kan du visa en lista alla de virtuella datorerna i din prenumeration har skapat med hjälp`azure vm list`</span><span class="sxs-lookup"><span data-stu-id="0302e-142">If needed, you can view a list all the VMs created in your subscription by using `azure vm list`</span></span>

   > [!NOTE]
   > <span data-ttu-id="0302e-143">Om du använder Azure portal, Välj den virtuella datorn och klicka på **stoppa** att stänga av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0302e-143">If you're using the Azure portal, select the VM and click **Stop** to shut down the VM.</span></span>

8. <span data-ttu-id="0302e-144">Fånga avbildningen när den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="0302e-144">When the VM is stopped, capture the image.</span></span> <span data-ttu-id="0302e-145">I följande exempel samlar in den virtuella datorn med namnet `myVM` och skapar en generaliserad avbildning med namnet `myNewVM`:</span><span class="sxs-lookup"><span data-stu-id="0302e-145">The following example captures the VM named `myVM` and creates a generalized image named `myNewVM`:</span></span>

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    <span data-ttu-id="0302e-146">Den `-t` underkommandot tar bort den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0302e-146">The `-t` subcommand deletes the original virtual machine.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0302e-147">I Azure-portalen kan du göra en avbildning genom att välja **bild** från navmenyn.</span><span class="sxs-lookup"><span data-stu-id="0302e-147">In the Azure portal, you can capture an image by selecting **Image** from the hub menu.</span></span> <span data-ttu-id="0302e-148">Du måste ange följande information för avbildningen: namn, resursgruppens namn, plats, typ av operativsystem och blob-sökvägen för lagring.</span><span class="sxs-lookup"><span data-stu-id="0302e-148">You need to supply the following information for the image: name, resource group, location, operating system type, and storage blob path.</span></span>

9. <span data-ttu-id="0302e-149">Den nya avbildningen är nu tillgänglig i listan över avbildningar som kan användas för att konfigurera en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0302e-149">The new image is now available in the list of images that can be used to configure any new VM.</span></span> <span data-ttu-id="0302e-150">Du kan visa den med kommandot:</span><span class="sxs-lookup"><span data-stu-id="0302e-150">You can view it with the command:</span></span>

   ```azurecli
   azure vm image list
   ```

   <span data-ttu-id="0302e-151">På den [Azure-portalen](http://portal.azure.com), den nya avbildningen visas i den **VM-avbildningar (klassisk)** som tillhör den **Compute** tjänster.</span><span class="sxs-lookup"><span data-stu-id="0302e-151">On the [Azure portal](http://portal.azure.com), the new image appears in the **VM images (classic)** that belongs to the **Compute** services.</span></span> <span data-ttu-id="0302e-152">Du kan komma åt **VM-avbildningar (klassisk)** genom att klicka på _fler tjänster_ längst ned i Azure service lista och sedan söker den **Compute** tjänster.</span><span class="sxs-lookup"><span data-stu-id="0302e-152">You can access **VM images (classic)** by clicking _More services_ at the bottom of the Azure service list, and then looking in the **Compute** services.</span></span>   

   ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="0302e-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0302e-154">Next steps</span></span>
<span data-ttu-id="0302e-155">Bilden är redo att användas för att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0302e-155">The image is ready to be used to create VMs.</span></span> <span data-ttu-id="0302e-156">Du kan använda kommandot Azure CLI `azure vm create` och ange Avbildningsnamnet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="0302e-156">You can use the Azure CLI command `azure vm create` and supply the image name you created.</span></span> <span data-ttu-id="0302e-157">Mer information finns i [med hjälp av Azure CLI med klassiska distributionsmodellen](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="0302e-157">For more information, see [Using the Azure CLI with Classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

<span data-ttu-id="0302e-158">Du kan också använda den [Azure-portalen](http://portal.azure.com) att skapa en egen virtuell dator med hjälp av den **bild** metoden och välja den bild som du skapade.</span><span class="sxs-lookup"><span data-stu-id="0302e-158">Alternatively, use the [Azure portal](http://portal.azure.com) to create a custom VM by using the **Image** method and selecting the image you created.</span></span> <span data-ttu-id="0302e-159">Mer information finns i [hur du skapar en anpassad VM][How to Create a Custom Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="0302e-159">For more information, see [How to Create a Custom VM][How to Create a Custom Virtual Machine].</span></span>

<span data-ttu-id="0302e-160">**Se även:** [användarhandboken för Azure Linux-Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="0302e-160">**See also:** [Azure Linux Agent User Guide](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk.md
[How to Create a Linux Virtual Machine]:create-custom.md
