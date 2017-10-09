---
title: "aaaDifferent sätt toocreate en Linux VM i Azure | Microsoft Azure"
description: "Lär dig hello olika sätt toocreate en Linux-dator i Azure, inklusive länkar tootools och självstudier för varje metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a><span data-ttu-id="bc2a5-103">Olika sätt toocreate en Linux VM</span><span class="sxs-lookup"><span data-stu-id="bc2a5-103">Different ways toocreate a Linux VM</span></span>
<span data-ttu-id="bc2a5-104">Har du hello flexibilitet i Azure toocreate en Linux-dator (VM) med hjälp av verktyg och arbetsflöden föredrar tooyou.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-104">You have hello flexibility in Azure toocreate a Linux virtual machine (VM) using tools and workflows comfortable tooyou.</span></span> <span data-ttu-id="bc2a5-105">Den här artikeln sammanfattar dessa skillnader och exempel för att skapa din virtuella Linux-datorer, inklusive hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-105">This article summarizes these differences and examples for creating your Linux VMs, including hello Azure CLI 2.0.</span></span> <span data-ttu-id="bc2a5-106">Du kan också visa skapa alternativ, inklusive hello [Azure CLI 1.0](creation-choices-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-106">You can also view creation choices including hello [Azure CLI 1.0](creation-choices-nodejs.md).</span></span>

<span data-ttu-id="bc2a5-107">Hej [Azure CLI 2.0](/cli/azure/install-az-cli2) är tillgänglig mellan plattformar via ett npm-paket, som tillhandahålls av distro paket eller dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-107">hello [Azure CLI 2.0](/cli/azure/install-az-cli2) is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="bc2a5-108">Installera hello lämpligaste build för din miljö och logga in tooan Azure-konto med [az inloggning](/cli/azure/#login)</span><span class="sxs-lookup"><span data-stu-id="bc2a5-108">Install hello most appropriate build for your environment and log in tooan Azure account using [az login](/cli/azure/#login)</span></span>

* [<span data-ttu-id="bc2a5-109">Skapa en Linux VM med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bc2a5-109">Create a Linux VM with hello Azure CLI 2.0</span></span>](quick-create-cli.md)
  
  * <span data-ttu-id="bc2a5-110">Skapa en resursgrupp med [az group create](/cli/azure/group#create) med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-110">Create a resource group with [az group create](/cli/azure/group#create) named *myResourceGroup*:</span></span> 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * <span data-ttu-id="bc2a5-111">Skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create) med namnet *myVM* med hello senaste *UbuntuLTS* avbildningen och generera SSH-nycklar, om de inte redan finns i *~/.ssh*:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-111">Create a VM with [az vm create](/cli/azure/vm#create) named *myVM* using hello latest *UbuntuLTS* image and generate SSH keys if they do not already exist in *~/.ssh*:</span></span>

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [<span data-ttu-id="bc2a5-112">Skapa en virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="bc2a5-112">Create a Linux VM with an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="bc2a5-113">hello följande exempel används [az distribution skapa](/cli/azure/group/deployment#create) toocreate en virtuell dator från en mall som lagras på GitHub:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-113">hello following example uses [az group deployment create](/cli/azure/group/deployment#create) toocreate a VM from a template stored on GitHub:</span></span>
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [<span data-ttu-id="bc2a5-114">Skapa en virtuell Linux-dator och anpassa med cloud-init</span><span class="sxs-lookup"><span data-stu-id="bc2a5-114">Create a Linux VM and customize with cloud-init</span></span>](tutorial-automate-vm-deployment.md)

* [<span data-ttu-id="bc2a5-115">Skapa en belastningsutjämnat och mycket tillgängligt program på flera virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="bc2a5-115">Create a load balanced and highly available application on multiple Linux VMs</span></span>](tutorial-load-balancer.md)


## <a name="azure-portal"></a><span data-ttu-id="bc2a5-116">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bc2a5-116">Azure portal</span></span>
<span data-ttu-id="bc2a5-117">Hej [Azure-portalen](https://portal.azure.com) kan du tooquickly skapa en virtuell dator eftersom det inte finns något tooinstall på datorn.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-117">hello [Azure portal](https://portal.azure.com) allows you tooquickly create a VM since there is nothing tooinstall on your system.</span></span> <span data-ttu-id="bc2a5-118">Använd hello Azure portal toocreate hello VM:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-118">Use hello Azure portal toocreate hello VM:</span></span>

* [<span data-ttu-id="bc2a5-119">Skapa en Linux VM som använder hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bc2a5-119">Create a Linux VM using hello Azure portal</span></span>](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a><span data-ttu-id="bc2a5-120">Alternativ för operativsystem och avbildning</span><span class="sxs-lookup"><span data-stu-id="bc2a5-120">Operating system and image choices</span></span>
<span data-ttu-id="bc2a5-121">När du skapar en virtuell dator kan välja du en avbildning baserat på operativsystem som du vill toorun hello.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-121">When creating a VM, you choose an image based on hello operating system you want toorun.</span></span> <span data-ttu-id="bc2a5-122">Azure och dess samarbetspartner erbjuder många avbildningar, varav några innehåller förinstallerade program och verktyg.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-122">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="bc2a5-123">Eller ladda upp en av dina egna avbildningar (se [hello följande avsnitt](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-123">Or, upload one of your own images (see [hello following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="bc2a5-124">Azure-avbildningar</span><span class="sxs-lookup"><span data-stu-id="bc2a5-124">Azure images</span></span>
<span data-ttu-id="bc2a5-125">Använd hello [az vm-avbildning](/cli/azure/vm/image) kommandon toosee vad som är tillgängligt genom utgivare, distro versionen och versioner.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-125">Use hello [az vm image](/cli/azure/vm/image) commands toosee what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="bc2a5-126">Lista tillgängliga utgivare:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-126">List available publishers:</span></span>

```azurecli
az vm image list-publishers --location eastus
```

<span data-ttu-id="bc2a5-127">Lista tillgängliga produkter (erbjudanden) för en viss utgivare:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-127">List available products (offers) for a given publisher:</span></span>

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

<span data-ttu-id="bc2a5-128">Lista tillgängliga SKU:er (distributionsutgåvor) för ett givet erbjudande:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-128">List available SKUs (distro releases) of a given offer:</span></span>

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

<span data-ttu-id="bc2a5-129">Lista alla tillgängliga bilder för en viss version:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-129">List all available images for a given release:</span></span>

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

<span data-ttu-id="bc2a5-130">Fler exempel på Bläddra och använder tillgängliga avbildningar finns [analysera och välja avbildningar för virtuella Azure-datorn med hello Azure CLI](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-130">For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with hello Azure CLI](cli-ps-findimage.md).</span></span>

<span data-ttu-id="bc2a5-131">Hej [az vm skapa](/cli/azure/vm#create) kommandot har alias som du kan använda tooquickly access hello vanliga distributioner och deras senaste versioner.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-131">hello [az vm create](/cli/azure/vm#create) command has aliases you can use tooquickly access hello more common distros and their latest releases.</span></span> <span data-ttu-id="bc2a5-132">Alias är ofta snabbare än att ange hello utgivare, erbjudande, SKU och version varje gång du skapar en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-132">Using aliases is often quicker than specifying hello publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="bc2a5-133">Alias</span><span class="sxs-lookup"><span data-stu-id="bc2a5-133">Alias</span></span> | <span data-ttu-id="bc2a5-134">Utgivare</span><span class="sxs-lookup"><span data-stu-id="bc2a5-134">Publisher</span></span> | <span data-ttu-id="bc2a5-135">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="bc2a5-135">Offer</span></span> | <span data-ttu-id="bc2a5-136">SKU</span><span class="sxs-lookup"><span data-stu-id="bc2a5-136">SKU</span></span> | <span data-ttu-id="bc2a5-137">Version</span><span class="sxs-lookup"><span data-stu-id="bc2a5-137">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="bc2a5-138">CentOS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-138">CentOS</span></span> |<span data-ttu-id="bc2a5-139">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="bc2a5-139">OpenLogic</span></span> |<span data-ttu-id="bc2a5-140">Centos</span><span class="sxs-lookup"><span data-stu-id="bc2a5-140">Centos</span></span> |<span data-ttu-id="bc2a5-141">7.2</span><span class="sxs-lookup"><span data-stu-id="bc2a5-141">7.2</span></span> |<span data-ttu-id="bc2a5-142">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-142">latest</span></span> |
| <span data-ttu-id="bc2a5-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-143">CoreOS</span></span> |<span data-ttu-id="bc2a5-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-144">CoreOS</span></span> |<span data-ttu-id="bc2a5-145">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-145">CoreOS</span></span> |<span data-ttu-id="bc2a5-146">Stable</span><span class="sxs-lookup"><span data-stu-id="bc2a5-146">Stable</span></span> |<span data-ttu-id="bc2a5-147">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-147">latest</span></span> |
| <span data-ttu-id="bc2a5-148">Debian</span><span class="sxs-lookup"><span data-stu-id="bc2a5-148">Debian</span></span> |<span data-ttu-id="bc2a5-149">credativ</span><span class="sxs-lookup"><span data-stu-id="bc2a5-149">credativ</span></span> |<span data-ttu-id="bc2a5-150">Debian</span><span class="sxs-lookup"><span data-stu-id="bc2a5-150">Debian</span></span> |<span data-ttu-id="bc2a5-151">8</span><span class="sxs-lookup"><span data-stu-id="bc2a5-151">8</span></span> |<span data-ttu-id="bc2a5-152">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-152">latest</span></span> |
| <span data-ttu-id="bc2a5-153">openSUSE</span><span class="sxs-lookup"><span data-stu-id="bc2a5-153">openSUSE</span></span> |<span data-ttu-id="bc2a5-154">SUSE</span><span class="sxs-lookup"><span data-stu-id="bc2a5-154">SUSE</span></span> |<span data-ttu-id="bc2a5-155">openSUSE</span><span class="sxs-lookup"><span data-stu-id="bc2a5-155">openSUSE</span></span> |<span data-ttu-id="bc2a5-156">13.2</span><span class="sxs-lookup"><span data-stu-id="bc2a5-156">13.2</span></span> |<span data-ttu-id="bc2a5-157">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-157">latest</span></span> |
| <span data-ttu-id="bc2a5-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="bc2a5-158">RHEL</span></span> |<span data-ttu-id="bc2a5-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="bc2a5-159">Redhat</span></span> |<span data-ttu-id="bc2a5-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="bc2a5-160">RHEL</span></span> |<span data-ttu-id="bc2a5-161">7.2</span><span class="sxs-lookup"><span data-stu-id="bc2a5-161">7.2</span></span> |<span data-ttu-id="bc2a5-162">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-162">latest</span></span> |
| <span data-ttu-id="bc2a5-163">SLES</span><span class="sxs-lookup"><span data-stu-id="bc2a5-163">SLES</span></span> |<span data-ttu-id="bc2a5-164">SLES</span><span class="sxs-lookup"><span data-stu-id="bc2a5-164">SLES</span></span> |<span data-ttu-id="bc2a5-165">SLES</span><span class="sxs-lookup"><span data-stu-id="bc2a5-165">SLES</span></span> |<span data-ttu-id="bc2a5-166">12-SP1</span><span class="sxs-lookup"><span data-stu-id="bc2a5-166">12-SP1</span></span> |<span data-ttu-id="bc2a5-167">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-167">latest</span></span> |
| <span data-ttu-id="bc2a5-168">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-168">UbuntuLTS</span></span> |<span data-ttu-id="bc2a5-169">Canonical</span><span class="sxs-lookup"><span data-stu-id="bc2a5-169">Canonical</span></span> |<span data-ttu-id="bc2a5-170">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="bc2a5-170">UbuntuServer</span></span> |<span data-ttu-id="bc2a5-171">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="bc2a5-171">14.04.4-LTS</span></span> |<span data-ttu-id="bc2a5-172">senaste</span><span class="sxs-lookup"><span data-stu-id="bc2a5-172">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="bc2a5-173">Använda en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="bc2a5-173">Use your own image</span></span>
<span data-ttu-id="bc2a5-174">Om du behöver specifika anpassningar kan du använda en avbildning baserad på en befintlig virtuell Azure-dator genom att avbilda den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-174">If you require specific customizations, you can use an image based on an existing Azure VM by capturing that VM.</span></span> <span data-ttu-id="bc2a5-175">Du kan också ladda upp en avbildning som skapats lokalt.</span><span class="sxs-lookup"><span data-stu-id="bc2a5-175">You can also upload an image created on-premises.</span></span> <span data-ttu-id="bc2a5-176">Mer information om distributioner som stöds och hur toouse egna avbildningar finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-176">For more information on supported distros and how toouse your own images, see hello following articles:</span></span>

* [<span data-ttu-id="bc2a5-177">Azure-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="bc2a5-177">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="bc2a5-178">Information om icke-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="bc2a5-178">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* <span data-ttu-id="bc2a5-179">[Hur toocreate en bild från en befintlig Azure-VM](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-179">[How toocreate an image from an existing Azure VM](tutorial-custom-images.md).</span></span>
  
  * <span data-ttu-id="bc2a5-180">Snabbkurs exempel kommandon toocreate en bild från en befintlig virtuell Azure-dator:</span><span class="sxs-lookup"><span data-stu-id="bc2a5-180">Quick-start example commands toocreate an image from an existing Azure VM:</span></span>
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a><span data-ttu-id="bc2a5-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc2a5-181">Next steps</span></span>
* <span data-ttu-id="bc2a5-182">Skapa en Linux VM med hello [CLI](quick-create-cli.md), från hello [portal](quick-create-portal.md), eller med hjälp av en [Azure Resource Manager-mall](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-182">Create a Linux VM with hello [CLI](quick-create-cli.md), from hello [portal](quick-create-portal.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="bc2a5-183">När du har skapat en Linux VM ska du [lära dig mer om Azure-diskar och lagring](tutorial-manage-disks.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-183">After creating a Linux VM, [learn about Azure disks and storage](tutorial-manage-disks.md).</span></span>
* <span data-ttu-id="bc2a5-184">Snabba steg för[återställa ett lösenord eller SSH-nycklar och hantera användare](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="bc2a5-184">Quick steps too[reset a password or SSH keys and manage users](using-vmaccess-extension.md).</span></span>
