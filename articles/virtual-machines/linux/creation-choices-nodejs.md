---
title: "Olika sätt att skapa en Linux-VM med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig mer om vilka alternativ som du kan välja mellan när du skapar en virtuell Linux-dator i Azure, inklusive länkar till verktyg och självstudier för varje metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="2314c-103">Olika sätt att skapa en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="2314c-103">Different ways to create a Linux virtual machine in Azure</span></span>
<span data-ttu-id="2314c-104">I Azure har du tillräcklig flexibilitet för att kunna skapa en virtuell Linux-dator (VM) med hjälp av de verktyg och arbetsflöden som du känner dig mest bekväm med.</span><span class="sxs-lookup"><span data-stu-id="2314c-104">You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you.</span></span> <span data-ttu-id="2314c-105">I den här artikeln sammanfattas skillnader och exempel när du ska skapa dina virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="2314c-105">This article summarizes these differences and examples for creating your Linux VMs.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="2314c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2314c-106">Azure CLI</span></span>
<span data-ttu-id="2314c-107">Du kan skapa virtuella datorer i Azure med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="2314c-107">You can create VMs in Azure using one of the following CLI versions:</span></span>

- <span data-ttu-id="2314c-108">Azure CLI 1.0 – vårt CLI för den klassiska distributionsmodellen och Resource Manager-distributionsmodellen (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="2314c-108">Azure CLI 1.0 – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="2314c-109">[Azure CLI 2.0](../windows/creation-choices.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="2314c-109">[Azure CLI 2.0](../windows/creation-choices.md) - our next generation CLI for the resource management deployment model</span></span>

<span data-ttu-id="2314c-110">Azure CLI 1.0 är tillgängligt på flera plattformar via ett npm-paket, distro-paket eller Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="2314c-110">The Azure CLI 1.0 is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="2314c-111">Du kan läsa mer om [hur du installerar och konfigurerar Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2314c-111">You can read more about [how to install and configure the Azure CLI](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="2314c-112">Följande självstudier innehåller exempel på hur du använder Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="2314c-112">The following tutorials provide examples on using the Azure CLI 1.0.</span></span> <span data-ttu-id="2314c-113">Läs alla artiklarna för mer information om de CLI-snabbstartkommandon som visas:</span><span class="sxs-lookup"><span data-stu-id="2314c-113">Read each article for more details on the CLI quick-start commands shown:</span></span>

* [<span data-ttu-id="2314c-114">Skapa en virtuell Linux-dator från Azure CLI för utveckling och testning</span><span class="sxs-lookup"><span data-stu-id="2314c-114">Create a Linux VM from the Azure CLI for dev and test</span></span>](quick-create-cli-nodejs.md)
  
  * <span data-ttu-id="2314c-115">I följande exempel skapas en CoreOS VM som använder en offentlig nyckel som heter *azure_id_rsa.pub*:</span><span class="sxs-lookup"><span data-stu-id="2314c-115">The following example creates a CoreOS VM using a public key named *azure_id_rsa.pub*:</span></span>
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [<span data-ttu-id="2314c-116">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="2314c-116">Create a secured Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="2314c-117">Följande exempel skapar en VM med en mall som lagrats i GitHub:</span><span class="sxs-lookup"><span data-stu-id="2314c-117">The following example creates a VM using a template stored on GitHub:</span></span>
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [<span data-ttu-id="2314c-118">Skapa en fullständig Linux-miljö med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2314c-118">Create a complete Linux environment using the Azure CLI</span></span>](create-cli-complete-nodejs.md)
  
  * <span data-ttu-id="2314c-119">Innefattar att skapa en belastningsutjämnare och flera virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2314c-119">Includes creating a load balancer and multiple VMs in an availability set.</span></span>
* [<span data-ttu-id="2314c-120">Lägg till en disk till en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="2314c-120">Add a disk to a Linux VM</span></span>](add-disk.md)
  
  * <span data-ttu-id="2314c-121">I följande exempel läggs en *5* Gb disk till en befintlig virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="2314c-121">The following example adds a *5* Gb disk to an existing VM named *myVM*:</span></span>
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a><span data-ttu-id="2314c-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2314c-122">Azure portal</span></span>
<span data-ttu-id="2314c-123">I [Azure Portal](https://portal.azure.com) kan du snabbt skapa en virtuell dator eftersom det inte finns något att installera i ditt system.</span><span class="sxs-lookup"><span data-stu-id="2314c-123">The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system.</span></span> <span data-ttu-id="2314c-124">Använd Azure Portal när du skapar den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="2314c-124">Use the Azure portal to create the VM:</span></span>

* [<span data-ttu-id="2314c-125">Skapa en virtuell Linux-dator med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2314c-125">Create a Linux VM using the Azure portal</span></span>](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a><span data-ttu-id="2314c-126">Alternativ för operativsystem och avbildning</span><span class="sxs-lookup"><span data-stu-id="2314c-126">Operating system and image choices</span></span>
<span data-ttu-id="2314c-127">När du skapar en virtuell dator, väljer du en avbildning baserat på vilket operativsystem du vill köra.</span><span class="sxs-lookup"><span data-stu-id="2314c-127">When creating a VM, you choose an image based on the operating system you want to run.</span></span> <span data-ttu-id="2314c-128">Azure och dess samarbetspartner erbjuder många avbildningar, varav några innehåller förinstallerade program och verktyg.</span><span class="sxs-lookup"><span data-stu-id="2314c-128">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="2314c-129">Eller ladda upp en av dina egna avbildningar (se [följande avsnitt](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="2314c-129">Or, upload one of your own images (see [the following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="2314c-130">Azure-avbildningar</span><span class="sxs-lookup"><span data-stu-id="2314c-130">Azure images</span></span>
<span data-ttu-id="2314c-131">Använd `azure vm image` CLI-kommandon för att se vad som finns tillgängligt via utgivare, distributionsutgåva och version.</span><span class="sxs-lookup"><span data-stu-id="2314c-131">Use the `azure vm image` CLI commands to see what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="2314c-132">Lista tillgängliga utgivare enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2314c-132">List available publishers as follows:</span></span>

```azurecli
azure vm image list-publishers --location eastus
```

<span data-ttu-id="2314c-133">Lista tillgängliga produkter (erbjudanden) för en viss utgivare enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2314c-133">List available products (offers) for a given publisher as follows:</span></span>

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

<span data-ttu-id="2314c-134">Lista tillgängliga SKU:er (distributionsutgåvor) för ett givet erbjudande enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2314c-134">List available SKUs (distro releases) of a given offer as follows:</span></span>

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

<span data-ttu-id="2314c-135">Lista alla tillgängliga avbildningar för en viss version enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2314c-135">List all available images for a given release follows:</span></span>

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

<span data-ttu-id="2314c-136">Kommandona `azure vm quick-create` och `azure vm create` har alias som du kan använda för att snabbt komma åt vanliga distributioner och deras senaste versioner.</span><span class="sxs-lookup"><span data-stu-id="2314c-136">The `azure vm quick-create` and `azure vm create` commands have aliases you can use to quickly access the more common distros and their latest releases.</span></span> <span data-ttu-id="2314c-137">Det går snabbare att använda alias än ange utgivare, erbjudande, SKU och version varje gång du skapar en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="2314c-137">Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="2314c-138">Alias</span><span class="sxs-lookup"><span data-stu-id="2314c-138">Alias</span></span> | <span data-ttu-id="2314c-139">Utgivare</span><span class="sxs-lookup"><span data-stu-id="2314c-139">Publisher</span></span> | <span data-ttu-id="2314c-140">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="2314c-140">Offer</span></span> | <span data-ttu-id="2314c-141">SKU</span><span class="sxs-lookup"><span data-stu-id="2314c-141">SKU</span></span> | <span data-ttu-id="2314c-142">Version</span><span class="sxs-lookup"><span data-stu-id="2314c-142">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="2314c-143">CentOS</span><span class="sxs-lookup"><span data-stu-id="2314c-143">CentOS</span></span> |<span data-ttu-id="2314c-144">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="2314c-144">OpenLogic</span></span> |<span data-ttu-id="2314c-145">Centos</span><span class="sxs-lookup"><span data-stu-id="2314c-145">Centos</span></span> |<span data-ttu-id="2314c-146">7.2</span><span class="sxs-lookup"><span data-stu-id="2314c-146">7.2</span></span> |<span data-ttu-id="2314c-147">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-147">latest</span></span> |
| <span data-ttu-id="2314c-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2314c-148">CoreOS</span></span> |<span data-ttu-id="2314c-149">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2314c-149">CoreOS</span></span> |<span data-ttu-id="2314c-150">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2314c-150">CoreOS</span></span> |<span data-ttu-id="2314c-151">Stable</span><span class="sxs-lookup"><span data-stu-id="2314c-151">Stable</span></span> |<span data-ttu-id="2314c-152">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-152">latest</span></span> |
| <span data-ttu-id="2314c-153">Debian</span><span class="sxs-lookup"><span data-stu-id="2314c-153">Debian</span></span> |<span data-ttu-id="2314c-154">credativ</span><span class="sxs-lookup"><span data-stu-id="2314c-154">credativ</span></span> |<span data-ttu-id="2314c-155">Debian</span><span class="sxs-lookup"><span data-stu-id="2314c-155">Debian</span></span> |<span data-ttu-id="2314c-156">8</span><span class="sxs-lookup"><span data-stu-id="2314c-156">8</span></span> |<span data-ttu-id="2314c-157">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-157">latest</span></span> |
| <span data-ttu-id="2314c-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2314c-158">openSUSE</span></span> |<span data-ttu-id="2314c-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="2314c-159">SUSE</span></span> |<span data-ttu-id="2314c-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2314c-160">openSUSE</span></span> |<span data-ttu-id="2314c-161">13.2</span><span class="sxs-lookup"><span data-stu-id="2314c-161">13.2</span></span> |<span data-ttu-id="2314c-162">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-162">latest</span></span> |
| <span data-ttu-id="2314c-163">RHEL</span><span class="sxs-lookup"><span data-stu-id="2314c-163">RHEL</span></span> |<span data-ttu-id="2314c-164">Redhat</span><span class="sxs-lookup"><span data-stu-id="2314c-164">Redhat</span></span> |<span data-ttu-id="2314c-165">RHEL</span><span class="sxs-lookup"><span data-stu-id="2314c-165">RHEL</span></span> |<span data-ttu-id="2314c-166">7.2</span><span class="sxs-lookup"><span data-stu-id="2314c-166">7.2</span></span> |<span data-ttu-id="2314c-167">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-167">latest</span></span> |
| <span data-ttu-id="2314c-168">SLES</span><span class="sxs-lookup"><span data-stu-id="2314c-168">SLES</span></span> |<span data-ttu-id="2314c-169">SLES</span><span class="sxs-lookup"><span data-stu-id="2314c-169">SLES</span></span> |<span data-ttu-id="2314c-170">SLES</span><span class="sxs-lookup"><span data-stu-id="2314c-170">SLES</span></span> |<span data-ttu-id="2314c-171">12-SP1</span><span class="sxs-lookup"><span data-stu-id="2314c-171">12-SP1</span></span> |<span data-ttu-id="2314c-172">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-172">latest</span></span> |
| <span data-ttu-id="2314c-173">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="2314c-173">UbuntuLTS</span></span> |<span data-ttu-id="2314c-174">Canonical</span><span class="sxs-lookup"><span data-stu-id="2314c-174">Canonical</span></span> |<span data-ttu-id="2314c-175">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="2314c-175">UbuntuServer</span></span> |<span data-ttu-id="2314c-176">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="2314c-176">14.04.4-LTS</span></span> |<span data-ttu-id="2314c-177">senaste</span><span class="sxs-lookup"><span data-stu-id="2314c-177">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="2314c-178">Använda en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="2314c-178">Use your own image</span></span>
<span data-ttu-id="2314c-179">Om du behöver specifika anpassningar kan du använda en avbildning baserad på en befintlig virtuell Azure-dator genom att *avbilda* den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2314c-179">If you require specific customizations, you can use an image based on an existing Azure VM by *capturing* that VM.</span></span> <span data-ttu-id="2314c-180">Du kan också ladda upp en avbildning som skapats lokalt.</span><span class="sxs-lookup"><span data-stu-id="2314c-180">You can also upload an image created on-premises.</span></span> <span data-ttu-id="2314c-181">Mer information om distributioner som stöds och hur du använder egna avbildningar finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2314c-181">For more information on supported distros and how to use your own images, see the following articles:</span></span>

* [<span data-ttu-id="2314c-182">Azure-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="2314c-182">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="2314c-183">Information om icke-godkända distributioner</span><span class="sxs-lookup"><span data-stu-id="2314c-183">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* [<span data-ttu-id="2314c-184">Överföra och skapa en Linux VM från anpassad diskavbildning</span><span class="sxs-lookup"><span data-stu-id="2314c-184">Upload and create a Linux VM from custom disk image</span></span>](upload-vhd.md)
* <span data-ttu-id="2314c-185">[Avbilda en virtuell Linux-dator som en Resource Manager-mall](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="2314c-185">[How to capture a Linux virtual machine as a Resource Manager template](capture-image.md).</span></span>
  
  * <span data-ttu-id="2314c-186">Snabbstart med exempelkommandon för att avbilda en befintlig virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="2314c-186">Quick-start example commands to capture an existing VM:</span></span>
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a><span data-ttu-id="2314c-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2314c-187">Next steps</span></span>
* <span data-ttu-id="2314c-188">Skapa en virtuell Linux-dator från [portalen](quick-create-portal.md), med [CLI](quick-create-cli.md) eller genom att använda en [Azure Resource Manager-mall](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2314c-188">Create a Linux VM from the [portal](quick-create-portal.md), with the [CLI](quick-create-cli.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="2314c-189">När du har skapat en virtuell Linux-dator kan du [lägga till en datadisk](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="2314c-189">After creating a Linux VM, [add a data disk](add-disk.md).</span></span>
* <span data-ttu-id="2314c-190">Snabba steg för att [återställa ett lösenord eller SSH-nycklar och hantera användare](using-vmaccess-extension.md)</span><span class="sxs-lookup"><span data-stu-id="2314c-190">Quick steps to [reset a password or SSH keys and manage users](using-vmaccess-extension.md)</span></span>

