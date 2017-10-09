---
title: aaaCreate anpassade VM-avbildningar med hello Azure CLI | Microsoft Docs
description: "Självstudiekurs – skapa en anpassad VM-avbildning med hjälp av hello Azure CLI."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a><span data-ttu-id="0e853-103">Skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI</span><span class="sxs-lookup"><span data-stu-id="0e853-103">Create a custom image of an Azure VM using hello CLI</span></span>

<span data-ttu-id="0e853-104">Anpassade avbildningar liknar marketplace-bilder, men du skapa dem själv.</span><span class="sxs-lookup"><span data-stu-id="0e853-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="0e853-105">Anpassade avbildningar kan vara används toobootstrap konfigurationer, till exempel förinstalleras program, Programinställningar och andra OS-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="0e853-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="0e853-106">I den här självstudiekursen skapar du en egen anpassad avbildning av en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="0e853-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="0e853-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="0e853-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e853-108">Ta bort etableringen och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0e853-108">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="0e853-109">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-109">Create a custom image</span></span>
> * <span data-ttu-id="0e853-110">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="0e853-111">Visa en lista med alla hello bilder i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="0e853-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="0e853-112">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="0e853-112">Delete an image</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0e853-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0e853-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0e853-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="0e853-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0e853-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0e853-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="0e853-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0e853-116">Before you begin</span></span>

<span data-ttu-id="0e853-117">hello stegen nedan i detalj hur tootake en befintlig virtuell dator och aktivera den till en återanvändbara anpassad avbildning som du kan använda toocreate nya VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="0e853-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="0e853-118">toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0e853-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="0e853-119">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="0e853-119">If needed, this [script sample](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) can create one for you.</span></span> <span data-ttu-id="0e853-120">När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.</span><span class="sxs-lookup"><span data-stu-id="0e853-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="create-a-custom-image"></a><span data-ttu-id="0e853-121">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-121">Create a custom image</span></span>

<span data-ttu-id="0e853-122">toocreate en avbildning av en virtuell dator måste tooprepare hello VM genom avetablering det frigjorts och markera hello källa VM som generaliserad.</span><span class="sxs-lookup"><span data-stu-id="0e853-122">toocreate an image of a virtual machine, you need tooprepare hello VM by deprovisioning, deallocating, and then marking hello source VM as generalized.</span></span> <span data-ttu-id="0e853-123">En gång hello VM har förberetts, du kan skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="0e853-123">Once hello VM has been prepared, you can create an image.</span></span>

### <a name="deprovision-hello-vm"></a><span data-ttu-id="0e853-124">Ta bort etableringen hello VM</span><span class="sxs-lookup"><span data-stu-id="0e853-124">Deprovision hello VM</span></span> 

<span data-ttu-id="0e853-125">Avetablering generalisering hello VM genom att ta bort information om datorn.</span><span class="sxs-lookup"><span data-stu-id="0e853-125">Deprovisioning generalizes hello VM by removing machine-specific information.</span></span> <span data-ttu-id="0e853-126">Den här generalisering gör det möjligt toodeploy många virtuella datorer från en enda avbildning.</span><span class="sxs-lookup"><span data-stu-id="0e853-126">This generalization makes it possible toodeploy many VMs from a single image.</span></span> <span data-ttu-id="0e853-127">Under avetablering, återställa hello värdnamn för*localhost.localdomain*.</span><span class="sxs-lookup"><span data-stu-id="0e853-127">During deprovisioning, hello host name is reset too*localhost.localdomain*.</span></span> <span data-ttu-id="0e853-128">SSH-värdnycklar, nameserver konfigurationer, rotlösenordet och cachelagrade DHCP-lån tas också bort.</span><span class="sxs-lookup"><span data-stu-id="0e853-128">SSH host keys, nameserver configurations, root password, and cached DHCP leases are also deleted.</span></span>

<span data-ttu-id="0e853-129">toodeprovision hello VM, använda hello Azure VM-agenten (waagent).</span><span class="sxs-lookup"><span data-stu-id="0e853-129">toodeprovision hello VM, use hello Azure VM agent (waagent).</span></span> <span data-ttu-id="0e853-130">hello Azure VM-agenten är installerad på hello VM och hanterar etablering och interagerar med hello Azure-Infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="0e853-130">hello Azure VM agent is installed on hello VM and manages provisioning and interacting with hello Azure Fabric Controller.</span></span> <span data-ttu-id="0e853-131">Mer information finns i hello [Azure Linux-agenten användarhandboken](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="0e853-131">For more information, see hello [Azure Linux Agent user guide](agent-user-guide.md).</span></span>

<span data-ttu-id="0e853-132">Ansluta tooyour VM med SSH och kör hello kommandot toodeprovision hello VM.</span><span class="sxs-lookup"><span data-stu-id="0e853-132">Connect tooyour VM using SSH and run hello command toodeprovision hello VM.</span></span> <span data-ttu-id="0e853-133">Med hello `+user` argument, hello senaste kontot för etablerad användare och alla associerade data tas också bort.</span><span class="sxs-lookup"><span data-stu-id="0e853-133">With hello `+user` argument, hello last provisioned user account and any associated data are also deleted.</span></span> <span data-ttu-id="0e853-134">Ersätt hello IP-adressen med hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0e853-134">Replace hello example IP address with hello public IP address of your VM.</span></span>

<span data-ttu-id="0e853-135">SSH toohello VM.</span><span class="sxs-lookup"><span data-stu-id="0e853-135">SSH toohello VM.</span></span>
```bash
ssh azureuser@52.174.34.95
```
<span data-ttu-id="0e853-136">Ta bort etableringen hello VM.</span><span class="sxs-lookup"><span data-stu-id="0e853-136">Deprovision hello VM.</span></span>

```bash
sudo waagent -deprovision+user -force
```
<span data-ttu-id="0e853-137">Stäng hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="0e853-137">Close hello SSH session.</span></span>

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="0e853-138">Frigöra och markera hello VM som generaliserad</span><span class="sxs-lookup"><span data-stu-id="0e853-138">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="0e853-139">toocreate en avbildning måste hello VM toobe frigjorts.</span><span class="sxs-lookup"><span data-stu-id="0e853-139">toocreate an image, hello VM needs toobe deallocated.</span></span> <span data-ttu-id="0e853-140">Frigöra hello VM med hjälp av [az vm frigöra](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="0e853-140">Deallocate hello VM using [az vm deallocate](/cli//azure/vm#deallocate).</span></span> 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="0e853-141">Ange slutligen hello tillstånd för hello VM som generaliserad med [az vm generalisera](/cli//azure/vm#generalize) så hello Azure-plattformen vet hello VM har generaliserats.</span><span class="sxs-lookup"><span data-stu-id="0e853-141">Finally, set hello state of hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize) so hello Azure platform knows hello VM has been generalized.</span></span> <span data-ttu-id="0e853-142">Du kan bara skapa en avbildning från en generaliserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0e853-142">You can only create an image from a generalized VM.</span></span>
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a><span data-ttu-id="0e853-143">Skapa hello avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-143">Create hello image</span></span>

<span data-ttu-id="0e853-144">Nu kan du skapa en avbildning av hello VM med hjälp av [az bild skapa](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="0e853-144">Now you can create an image of hello VM by using [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="0e853-145">hello följande exempel skapas en bild med namnet *myImage* från en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="0e853-145">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="0e853-146">Skapa virtuella datorer från hello-bild</span><span class="sxs-lookup"><span data-stu-id="0e853-146">Create VMs from hello image</span></span>

<span data-ttu-id="0e853-147">Nu när du har skapat en avbildning kan du skapa en eller flera nya virtuella datorer från hello avbildning med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0e853-147">Now that you have an image, you can create one or more new VMs from hello image using [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="0e853-148">hello följande exempel skapas en virtuell dator med namnet *myVMfromImage* från hello bild med namnet *myImage*.</span><span class="sxs-lookup"><span data-stu-id="0e853-148">hello following example creates a VM named *myVMfromImage* from hello image named *myImage*.</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a><span data-ttu-id="0e853-149">Avbildningshantering</span><span class="sxs-lookup"><span data-stu-id="0e853-149">Image management</span></span> 

<span data-ttu-id="0e853-150">Här följer några exempel på vanliga hanteringsuppgifter för avbildningen och hur toocomplete dem med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0e853-150">Here are some examples of common image management tasks and how toocomplete them using hello Azure CLI.</span></span>

<span data-ttu-id="0e853-151">Visa alla avbildningar av namnet i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="0e853-151">List all images by name in a table format.</span></span>

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

<span data-ttu-id="0e853-152">Ta bort en bild.</span><span class="sxs-lookup"><span data-stu-id="0e853-152">Delete an image.</span></span> <span data-ttu-id="0e853-153">Det här exemplet tar bort hello bild med namnet *myOldImage* från hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="0e853-153">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="0e853-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e853-154">Next steps</span></span>

<span data-ttu-id="0e853-155">I kursen får skapat du en anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="0e853-155">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="0e853-156">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="0e853-156">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e853-157">Ta bort etableringen och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0e853-157">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="0e853-158">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-158">Create a custom image</span></span>
> * <span data-ttu-id="0e853-159">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="0e853-159">Create a VM from a custom image</span></span>
> * <span data-ttu-id="0e853-160">Visa en lista med alla hello bilder i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="0e853-160">List all hello images in your subscription</span></span>
> * <span data-ttu-id="0e853-161">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="0e853-161">Delete an image</span></span>

<span data-ttu-id="0e853-162">Avancera toohello nästa självstudiekurs toolearn om virtuella datorer med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="0e853-162">Advance toohello next tutorial toolearn about highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="0e853-163">[Skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="0e853-163">[Create highly available VMs](tutorial-availability-sets.md).</span></span>

