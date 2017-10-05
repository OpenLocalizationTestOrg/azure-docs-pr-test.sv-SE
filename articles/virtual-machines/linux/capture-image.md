---
title: "Hämta en avbildning av en Linux VM i Azure med hjälp av CLI 2.0 | Microsoft Docs"
description: "Hämta en avbildning av en Azure VM ska användas för masslagring distributioner som använder Azure CLI 2.0."
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
ms.openlocfilehash: 19b573f77f2ee84600955d00d30bdb16c84e3623
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a><span data-ttu-id="fb939-103">Så här skapar du en avbildning av en virtuell dator eller virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="fb939-103">How to create an image of a virtual machine or VHD</span></span>

<!-- generalize, image - extended version of the tutorial-->

<span data-ttu-id="fb939-104">Om du vill skapa flera kopior av en virtuell dator (VM) ska användas i Azure, en avbildning av den virtuella datorn eller OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="fb939-104">To create multiple copies of a virtual machine (VM) to use in Azure, capture an image of the VM or the OS VHD.</span></span> <span data-ttu-id="fb939-105">Om du vill skapa en avbildning måste du ta bort personlig information som gör det säkrare att distribuera flera gånger.</span><span class="sxs-lookup"><span data-stu-id="fb939-105">To create an image, you need remove personal account information which makes it safer to deploy multiple times.</span></span> <span data-ttu-id="fb939-106">I följande steg du ta bort etableringen av en befintlig virtuell dator frigöra och skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="fb939-106">In the following steps you deprovision an existing VM, deallocate and create an image.</span></span> <span data-ttu-id="fb939-107">Du kan använda den här avbildningen för att skapa virtuella datorer på en resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fb939-107">You can use this image to create VMs across any resource group within your subscription.</span></span>

<span data-ttu-id="fb939-108">Om du vill skapa en kopia av din befintliga Linux-VM för säkerhetskopiering eller felsökning, eller överför en särskild Linux VHD från en lokal virtuell dator, se [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="fb939-108">If you want to create a copy of your existing Linux VM for backup or debugging, or upload a specialized Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md).</span></span>  

<span data-ttu-id="fb939-109">Du kan också använda **förpackaren** att skapa en anpassad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fb939-109">You can also use **Packer** to create your custom configuration.</span></span> <span data-ttu-id="fb939-110">Mer information om hur du använder förpackaren finns [använda förpackaren för att skapa avbildningar av Linux virtuella datorer i Azure](build-image-with-packer.md).</span><span class="sxs-lookup"><span data-stu-id="fb939-110">For more information on using Packer, see [How to use Packer to create Linux virtual machine images in Azure](build-image-with-packer.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="fb939-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fb939-111">Before you begin</span></span>
<span data-ttu-id="fb939-112">Se till att du uppfyller följande krav:</span><span class="sxs-lookup"><span data-stu-id="fb939-112">Ensure that you meet the following prerequisites:</span></span>

* <span data-ttu-id="fb939-113">Du behöver en Azure VM som skapats i Resource Manager-distributionsmodellen med hjälp av hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="fb939-113">You need an Azure VM created in the Resource Manager deployment model using managed disks.</span></span> <span data-ttu-id="fb939-114">Om du inte har skapat en Linux VM, kan du använda den [portal](quick-create-portal.md), [Azure CLI](quick-create-cli.md), eller [Resource Manager-mallar](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="fb939-114">If you haven't created a Linux VM, you can use the [portal](quick-create-portal.md), the [Azure CLI](quick-create-cli.md), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> <span data-ttu-id="fb939-115">Konfigurera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb939-115">Configure the VM as needed.</span></span> <span data-ttu-id="fb939-116">Till exempel [lägga till datadiskar](add-disk.md), tillämpa uppdateringar och installera program.</span><span class="sxs-lookup"><span data-stu-id="fb939-116">For example, [add data disks](add-disk.md), apply updates, and install applications.</span></span> 

* <span data-ttu-id="fb939-117">Du måste också har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="fb939-117">You also need to have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and be logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="fb939-118">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="fb939-118">Quick commands</span></span>

<span data-ttu-id="fb939-119">En förenklad version av det här avsnittet för att testa, utvärderar eller Lär dig mer om virtuella datorer i Azure finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av CLI](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="fb939-119">For a simplified version of this topic, for testing, evaluating or learning about VMs in Azure, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md).</span></span>


## <a name="step-1-deprovision-the-vm"></a><span data-ttu-id="fb939-120">Steg 1: Ta bort etableringen av den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="fb939-120">Step 1: Deprovision the VM</span></span>
<span data-ttu-id="fb939-121">Du ta bort etableringen av den virtuella datorn med Virtuella Azure-agenten att ta bort specifika filer och data.</span><span class="sxs-lookup"><span data-stu-id="fb939-121">You deprovision the VM, using the Azure VM agent, to delete machine specific files and data.</span></span> <span data-ttu-id="fb939-122">Använd den `waagent` kommandot med de *-deprovision + användare* parameter på käll-Linux VM.</span><span class="sxs-lookup"><span data-stu-id="fb939-122">Use the `waagent` command with the *-deprovision+user* parameter on your source Linux VM.</span></span> <span data-ttu-id="fb939-123">Mer information finns i [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="fb939-123">For more information, see the [Azure Linux Agent user guide](../windows/agent-user-guide.md).</span></span>

1. <span data-ttu-id="fb939-124">Ansluta till din Linux VM som använder en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="fb939-124">Connect to your Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="fb939-125">I fönstret SSH skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fb939-125">In the SSH window, type the following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > <span data-ttu-id="fb939-126">Endast köra kommandot på en virtuell dator som du vill samla in som en bild.</span><span class="sxs-lookup"><span data-stu-id="fb939-126">Only run this command on a VM that you intend to capture as an image.</span></span> <span data-ttu-id="fb939-127">Är det inte säkert att avbildningen är avmarkerad av all känslig information eller lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="fb939-127">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution.</span></span> <span data-ttu-id="fb939-128">Den *+ användaren* parametern tar också bort det senaste kontot för etablerad användare.</span><span class="sxs-lookup"><span data-stu-id="fb939-128">The *+user* parameter also removes the last provisioned user account.</span></span> <span data-ttu-id="fb939-129">Om du vill behålla autentiseringsuppgifter i den virtuella datorn bara använda *-deprovision* lämna användarkontot på plats.</span><span class="sxs-lookup"><span data-stu-id="fb939-129">If you want to keep account credentials in the VM, just use *-deprovision* to leave the user account in place.</span></span>
 
3. <span data-ttu-id="fb939-130">Typen **y** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="fb939-130">Type **y** to continue.</span></span> <span data-ttu-id="fb939-131">Du kan lägga till den **-tvinga** parametern för att undvika det här steget bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="fb939-131">You can add the **-force** parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="fb939-132">När kommandot har slutförts genom att skriva **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="fb939-132">After the command completes, type **exit**.</span></span> <span data-ttu-id="fb939-133">Det här steget stänger SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="fb939-133">This step closes the SSH client.</span></span>

## <a name="step-2-create-vm-image"></a><span data-ttu-id="fb939-134">Steg 2: Skapa VM-avbildning</span><span class="sxs-lookup"><span data-stu-id="fb939-134">Step 2: Create VM image</span></span>
<span data-ttu-id="fb939-135">Använda Azure CLI 2.0 för att markera den virtuella datorn som generaliserad och spara avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fb939-135">Use the Azure CLI 2.0 to mark the VM as generalized and capture the image.</span></span> <span data-ttu-id="fb939-136">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fb939-136">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="fb939-137">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="fb939-137">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

1. <span data-ttu-id="fb939-138">Frigör den virtuella datorn som du avetableras med [az vm frigöra](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="fb939-138">Deallocate the VM that you deprovisioned with [az vm deallocate](/cli//azure/vm#deallocate).</span></span> <span data-ttu-id="fb939-139">I följande exempel tar bort den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="fb939-139">The following example deallocates the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. <span data-ttu-id="fb939-140">Markera den virtuella datorn som generaliserad med [az vm generalisera](/cli//azure/vm#generalize).</span><span class="sxs-lookup"><span data-stu-id="fb939-140">Mark the VM as generalized with [az vm generalize](/cli//azure/vm#generalize).</span></span> <span data-ttu-id="fb939-141">Följande exempel märken på den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup* som generaliserad:</span><span class="sxs-lookup"><span data-stu-id="fb939-141">The following example marks the the VM named *myVM* in the resource group named *myResourceGroup* as generalized:</span></span>
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. <span data-ttu-id="fb939-142">Nu skapa en avbildning av den Virtuella datorresursen med [az bild skapa](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="fb939-142">Now create an image of the VM resource with [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="fb939-143">I följande exempel skapas en bild med namnet *myImage* i resursgrupp med namnet *myResourceGroup* med hjälp av den Virtuella datorresursen med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="fb939-143">The following example creates an image named *myImage* in the resource group named *myResourceGroup* using the VM resource named *myVM*:</span></span>
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > <span data-ttu-id="fb939-144">Bilden har skapats i samma resursgrupp som ditt Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="fb939-144">The image is created in the same resource group as your source VM.</span></span> <span data-ttu-id="fb939-145">Du kan skapa virtuella datorer i valfri resursgrupp i din prenumeration från den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fb939-145">You can create VMs in any resource group within your subscription from this image.</span></span> <span data-ttu-id="fb939-146">Ur management kan du skapa en viss resursgrupp för VM-resurser och bilder.</span><span class="sxs-lookup"><span data-stu-id="fb939-146">From a management perspective, you may wish to create a specific resource group for your VM resources and images.</span></span>

## <a name="step-3-create-a-vm-from-the-captured-image"></a><span data-ttu-id="fb939-147">Steg 3: Skapa en virtuell dator från avbildningen</span><span class="sxs-lookup"><span data-stu-id="fb939-147">Step 3: Create a VM from the captured image</span></span>
<span data-ttu-id="fb939-148">Skapa en virtuell dator med hjälp av den avbildning som du skapat med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="fb939-148">Create a VM using the image you created with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="fb939-149">I följande exempel skapas en virtuell dator med namnet *myVMDeployed* från avbildningen med namnet *myImage*:</span><span class="sxs-lookup"><span data-stu-id="fb939-149">The following example creates a VM named *myVMDeployed* from the image named *myImage*:</span></span>

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a><span data-ttu-id="fb939-150">Skapa den virtuella datorn i en annan resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fb939-150">Creating the VM in another resource group</span></span> 

<span data-ttu-id="fb939-151">Du kan skapa virtuella datorer från en avbildning i valfri resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fb939-151">You can create VMs from an image in any resource group within your subscription.</span></span> <span data-ttu-id="fb939-152">Ange fullständiga resurs-ID i avbildningen för att skapa en virtuell dator i en annan resursgrupp än avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fb939-152">To create a VM in a different resource group than the image, specify the full resource ID to your image.</span></span> <span data-ttu-id="fb939-153">Använd [az bildlista](/cli/azure/image#list) att visa en lista över avbildningar.</span><span class="sxs-lookup"><span data-stu-id="fb939-153">Use [az image list](/cli/azure/image#list) to view a list of images.</span></span> <span data-ttu-id="fb939-154">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="fb939-154">The output is similar to the following example:</span></span>

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

<span data-ttu-id="fb939-155">I följande exempel används [az vm skapa](/cli/azure/vm#create) att skapa en virtuell dator i en annan resursgrupp än källbilden genom att ange Bildresursen:</span><span class="sxs-lookup"><span data-stu-id="fb939-155">The following example uses [az vm create](/cli/azure/vm#create) to create a VM in a different resource group than the source image by specifying the image resource ID:</span></span>

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a><span data-ttu-id="fb939-156">Steg 4: Verifiera distributionen</span><span class="sxs-lookup"><span data-stu-id="fb939-156">Step 4: Verify the deployment</span></span>

<span data-ttu-id="fb939-157">Nu SSH till den virtuella datorn som du skapade för att kontrollera distributionen och börja använda den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb939-157">Now SSH to the virtual machine you created to verify the deployment and start using the new VM.</span></span> <span data-ttu-id="fb939-158">För att ansluta via SSH, hitta IP-adress eller FQDN för den virtuella datorn med [az vm visa](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="fb939-158">To connect via SSH, find the IP address or FQDN of your VM with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a><span data-ttu-id="fb939-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb939-159">Next steps</span></span>
<span data-ttu-id="fb939-160">Du kan skapa flera virtuella datorer från din datakälla VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="fb939-160">You can create multiple VMs from your source VM image.</span></span> <span data-ttu-id="fb939-161">Om du behöver göra ändringar i avbildningen:</span><span class="sxs-lookup"><span data-stu-id="fb939-161">If you need to make changes to your image:</span></span> 

- <span data-ttu-id="fb939-162">Skapa en virtuell dator från en avbildning.</span><span class="sxs-lookup"><span data-stu-id="fb939-162">Create a VM from your image.</span></span>
- <span data-ttu-id="fb939-163">Se några uppdateringar eller ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fb939-163">Make any updates or configuration changes.</span></span>
- <span data-ttu-id="fb939-164">Följ stegen igen för att ta bort etableringen, frigöra, generalisera och skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="fb939-164">Follow the steps again to deprovision, deallocate, generalize, and create an image.</span></span>
- <span data-ttu-id="fb939-165">Använd den här nya avbildningen för framtida distributioner.</span><span class="sxs-lookup"><span data-stu-id="fb939-165">Use this new image for future deployments.</span></span> <span data-ttu-id="fb939-166">Om du vill ta bort den ursprungliga avbildningen.</span><span class="sxs-lookup"><span data-stu-id="fb939-166">If desired, delete the original image.</span></span>

<span data-ttu-id="fb939-167">Mer information om hur du hanterar dina virtuella datorer med CLI finns [Azure CLI 2.0](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb939-167">For more information on managing your VMs with the CLI, see [Azure CLI 2.0](/cli/azure/overview).</span></span>
