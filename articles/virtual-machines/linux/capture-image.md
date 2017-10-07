---
title: "aaaCapture en avbildning av en Linux VM i Azure med hjälp av CLI 2.0 | Microsoft Docs"
description: "Hämta en avbildning av en Azure VM toouse för masslagring distributioner med hjälp av hello Azure CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a><span data-ttu-id="dbd16-103">Hur toocreate en avbildning av en virtuell dator eller virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="dbd16-103">How toocreate an image of a virtual machine or VHD</span></span>

<!-- generalize, image - extended version of hello tutorial-->

<span data-ttu-id="dbd16-104">toocreate flera kopior av en virtuell dator (VM) toouse i Azure, en avbildning av hello VM eller hello OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="dbd16-104">toocreate multiple copies of a virtual machine (VM) toouse in Azure, capture an image of hello VM or hello OS VHD.</span></span> <span data-ttu-id="dbd16-105">toocreate en avbildning måste du ta bort personlig information som gör det säkrare toodeploy flera gånger.</span><span class="sxs-lookup"><span data-stu-id="dbd16-105">toocreate an image, you need remove personal account information which makes it safer toodeploy multiple times.</span></span> <span data-ttu-id="dbd16-106">Frigöra i hello följande du ta bort etableringen av en befintlig virtuell dator, och skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-106">In hello following steps you deprovision an existing VM, deallocate and create an image.</span></span> <span data-ttu-id="dbd16-107">Du kan använda den här avbildningen toocreate virtuella datorer i valfri resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dbd16-107">You can use this image toocreate VMs across any resource group within your subscription.</span></span>

<span data-ttu-id="dbd16-108">Om du vill toocreate en kopia av din befintliga Linux-VM för säkerhetskopiering eller felsökning eller ladda upp en särskild Linux VHD från en lokal virtuell dator, se [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="dbd16-108">If you want toocreate a copy of your existing Linux VM for backup or debugging, or upload a specialized Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md).</span></span>  

<span data-ttu-id="dbd16-109">Du kan också använda **förpackaren** toocreate anpassade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dbd16-109">You can also use **Packer** toocreate your custom configuration.</span></span> <span data-ttu-id="dbd16-110">Mer information om hur du använder förpackaren finns [hur toouse förpackaren toocreate Linux virtuella bilder i Azure](build-image-with-packer.md).</span><span class="sxs-lookup"><span data-stu-id="dbd16-110">For more information on using Packer, see [How toouse Packer toocreate Linux virtual machine images in Azure](build-image-with-packer.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="dbd16-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="dbd16-111">Before you begin</span></span>
<span data-ttu-id="dbd16-112">Se till att du uppfyller hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="dbd16-112">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="dbd16-113">Du behöver en Azure VM som skapats i hello Resource Manager-distributionsmodellen med hjälp av hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="dbd16-113">You need an Azure VM created in hello Resource Manager deployment model using managed disks.</span></span> <span data-ttu-id="dbd16-114">Om du inte har skapat en Linux VM, kan du använda hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), eller [Resource Manager-mallar](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="dbd16-114">If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> <span data-ttu-id="dbd16-115">Konfigurera hello VM efter behov.</span><span class="sxs-lookup"><span data-stu-id="dbd16-115">Configure hello VM as needed.</span></span> <span data-ttu-id="dbd16-116">Till exempel [lägga till datadiskar](add-disk.md), tillämpa uppdateringar och installera program.</span><span class="sxs-lookup"><span data-stu-id="dbd16-116">For example, [add data disks](add-disk.md), apply updates, and install applications.</span></span> 

* <span data-ttu-id="dbd16-117">Du måste också toohave hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och loggas i tooan Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="dbd16-117">You also need toohave hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and be logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="dbd16-118">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="dbd16-118">Quick commands</span></span>

<span data-ttu-id="dbd16-119">En förenklad version av det här avsnittet för att testa, utvärderar eller Lär dig mer om virtuella datorer i Azure finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="dbd16-119">For a simplified version of this topic, for testing, evaluating or learning about VMs in Azure, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>


## <a name="step-1-deprovision-hello-vm"></a><span data-ttu-id="dbd16-120">Steg 1: Ta bort etableringen hello VM</span><span class="sxs-lookup"><span data-stu-id="dbd16-120">Step 1: Deprovision hello VM</span></span>
<span data-ttu-id="dbd16-121">Du ta bort etableringen hello VM som använder hello Azure VM-agenten, toodelete datorn specifika filer och data.</span><span class="sxs-lookup"><span data-stu-id="dbd16-121">You deprovision hello VM, using hello Azure VM agent, toodelete machine specific files and data.</span></span> <span data-ttu-id="dbd16-122">Använd hello `waagent` med hello *-deprovision + användaren* parameter på käll-Linux VM.</span><span class="sxs-lookup"><span data-stu-id="dbd16-122">Use hello `waagent` command with hello *-deprovision+user* parameter on your source Linux VM.</span></span> <span data-ttu-id="dbd16-123">Mer information finns i hello [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="dbd16-123">For more information, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md).</span></span>

1. <span data-ttu-id="dbd16-124">Ansluta tooyour Linux VM som använder en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="dbd16-124">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="dbd16-125">Ange följande kommando hello i hello SSH fönstret:</span><span class="sxs-lookup"><span data-stu-id="dbd16-125">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > <span data-ttu-id="dbd16-126">Endast köra kommandot på en virtuell dator som du avser toocapture som en bild.</span><span class="sxs-lookup"><span data-stu-id="dbd16-126">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="dbd16-127">Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-127">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span> <span data-ttu-id="dbd16-128">Hej *+ användaren* parametern tar också bort hello senaste etablerade användarkonto.</span><span class="sxs-lookup"><span data-stu-id="dbd16-128">hello *+user* parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="dbd16-129">Om du vill tookeep kontoautentiseringsuppgifter hello VM kan bara använda *-deprovision* tooleave hello användarkonto på plats.</span><span class="sxs-lookup"><span data-stu-id="dbd16-129">If you want tookeep account credentials in hello VM, just use *-deprovision* tooleave hello user account in place.</span></span>
 
3. <span data-ttu-id="dbd16-130">Typen **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="dbd16-130">Type **y** toocontinue.</span></span> <span data-ttu-id="dbd16-131">Du kan lägga till hello **-tvinga** parametern tooavoid åtgärden bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="dbd16-131">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="dbd16-132">När hello-kommandot har slutförts genom att skriva **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="dbd16-132">After hello command completes, type **exit**.</span></span> <span data-ttu-id="dbd16-133">Det här steget stänger hello SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="dbd16-133">This step closes hello SSH client.</span></span>

## <a name="step-2-create-vm-image"></a><span data-ttu-id="dbd16-134">Steg 2: Skapa VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="dbd16-134">Step 2: Create VM image</span></span>
<span data-ttu-id="dbd16-135">Använd hello Azure CLI 2.0 toomark hello VM som generaliserad och hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-135">Use hello Azure CLI 2.0 toomark hello VM as generalized and capture hello image.</span></span> <span data-ttu-id="dbd16-136">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="dbd16-136">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="dbd16-137">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="dbd16-137">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

1. <span data-ttu-id="dbd16-138">Frigöra hello virtuell dator som du avetableras med [az vm frigöra](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="dbd16-138">Deallocate hello VM that you deprovisioned with [az vm deallocate](/cli//azure/vm#deallocate).</span></span> <span data-ttu-id="dbd16-139">hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="dbd16-139">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. <span data-ttu-id="dbd16-140">Markera hello VM som generaliserad med [az vm generalisera](/cli//azure/vm#generalize).</span><span class="sxs-lookup"><span data-stu-id="dbd16-140">Mark hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize).</span></span> <span data-ttu-id="dbd16-141">Hej följande exempel märken hello hello virtuell dator med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup* som generaliserad:</span><span class="sxs-lookup"><span data-stu-id="dbd16-141">hello following example marks hello hello VM named *myVM* in hello resource group named *myResourceGroup* as generalized:</span></span>
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. <span data-ttu-id="dbd16-142">Nu skapa en avbildning av hello Virtuella datorresursen med [az bild skapa](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="dbd16-142">Now create an image of hello VM resource with [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="dbd16-143">hello följande exempel skapas en bild med namnet *myImage* i hello resursgrupp med namnet *myResourceGroup* med hello VM resurs med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="dbd16-143">hello following example creates an image named *myImage* in hello resource group named *myResourceGroup* using hello VM resource named *myVM*:</span></span>
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > <span data-ttu-id="dbd16-144">hello avbildningen skapas i hello samma resursgrupp som ditt Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="dbd16-144">hello image is created in hello same resource group as your source VM.</span></span> <span data-ttu-id="dbd16-145">Du kan skapa virtuella datorer i valfri resursgrupp i din prenumeration från den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="dbd16-145">You can create VMs in any resource group within your subscription from this image.</span></span> <span data-ttu-id="dbd16-146">Ur hantering av du toocreate en specifik resursgrupp för VM-resurser och bilder.</span><span class="sxs-lookup"><span data-stu-id="dbd16-146">From a management perspective, you may wish toocreate a specific resource group for your VM resources and images.</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="dbd16-147">Steg 3: Skapa en virtuell dator från hello avbildas avbildning</span><span class="sxs-lookup"><span data-stu-id="dbd16-147">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="dbd16-148">Skapa en virtuell dator med hjälp av hello-avbildning som du skapat med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="dbd16-148">Create a VM using hello image you created with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="dbd16-149">hello följande exempel skapas en virtuell dator med namnet *myVMDeployed* från hello bild med namnet *myImage*:</span><span class="sxs-lookup"><span data-stu-id="dbd16-149">hello following example creates a VM named *myVMDeployed* from hello image named *myImage*:</span></span>

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a><span data-ttu-id="dbd16-150">Skapa hello VM i en annan resursgrupp</span><span class="sxs-lookup"><span data-stu-id="dbd16-150">Creating hello VM in another resource group</span></span> 

<span data-ttu-id="dbd16-151">Du kan skapa virtuella datorer från en avbildning i valfri resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dbd16-151">You can create VMs from an image in any resource group within your subscription.</span></span> <span data-ttu-id="dbd16-152">toocreate en virtuell dator i en annan resursgrupp än hello bilden ange hello fullständiga resurs-ID tooyour bild.</span><span class="sxs-lookup"><span data-stu-id="dbd16-152">toocreate a VM in a different resource group than hello image, specify hello full resource ID tooyour image.</span></span> <span data-ttu-id="dbd16-153">Använd [az bildlista](/cli/azure/image#list) tooview en lista över avbildningar.</span><span class="sxs-lookup"><span data-stu-id="dbd16-153">Use [az image list](/cli/azure/image#list) tooview a list of images.</span></span> <span data-ttu-id="dbd16-154">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dbd16-154">hello output is similar toohello following example:</span></span>

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

<span data-ttu-id="dbd16-155">hello följande exempel används [az vm skapa](/cli/azure/vm#create) toocreate en virtuell dator i en annan resursgrupp än hello Källavbildningen genom att ange hello Bildresursen:</span><span class="sxs-lookup"><span data-stu-id="dbd16-155">hello following example uses [az vm create](/cli/azure/vm#create) toocreate a VM in a different resource group than hello source image by specifying hello image resource ID:</span></span>

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a><span data-ttu-id="dbd16-156">Steg 4: Verifiera hello-distribution</span><span class="sxs-lookup"><span data-stu-id="dbd16-156">Step 4: Verify hello deployment</span></span>

<span data-ttu-id="dbd16-157">Nu SSH toohello virtuella datorn du har skapat tooverify hello distribution och börjar använda hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dbd16-157">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="dbd16-158">tooconnect via SSH, hitta hello IP-adress eller FQDN för den virtuella datorn med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="dbd16-158">tooconnect via SSH, find hello IP address or FQDN of your VM with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a><span data-ttu-id="dbd16-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dbd16-159">Next steps</span></span>
<span data-ttu-id="dbd16-160">Du kan skapa flera virtuella datorer från din datakälla VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-160">You can create multiple VMs from your source VM image.</span></span> <span data-ttu-id="dbd16-161">Om du behöver toomake ändringar tooyour avbildningen:</span><span class="sxs-lookup"><span data-stu-id="dbd16-161">If you need toomake changes tooyour image:</span></span> 

- <span data-ttu-id="dbd16-162">Skapa en virtuell dator från en avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-162">Create a VM from your image.</span></span>
- <span data-ttu-id="dbd16-163">Se några uppdateringar eller ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dbd16-163">Make any updates or configuration changes.</span></span>
- <span data-ttu-id="dbd16-164">Följ hello steg igen toodeprovision, frigöra, generalisera och skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-164">Follow hello steps again toodeprovision, deallocate, generalize, and create an image.</span></span>
- <span data-ttu-id="dbd16-165">Använd den här nya avbildningen för framtida distributioner.</span><span class="sxs-lookup"><span data-stu-id="dbd16-165">Use this new image for future deployments.</span></span> <span data-ttu-id="dbd16-166">Om du vill ta bort hello ursprungliga avbildning.</span><span class="sxs-lookup"><span data-stu-id="dbd16-166">If desired, delete hello original image.</span></span>

<span data-ttu-id="dbd16-167">Mer information om hur du hanterar dina virtuella datorer med hello CLI finns [Azure CLI 2.0](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dbd16-167">For more information on managing your VMs with hello CLI, see [Azure CLI 2.0](/cli/azure/overview).</span></span>
