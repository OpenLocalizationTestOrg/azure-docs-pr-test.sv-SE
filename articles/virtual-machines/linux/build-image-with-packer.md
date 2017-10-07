---
title: "aaaHow toocreate Linux Azure VM-avbildningar med förpackaren | Microsoft Docs"
description: "Lär dig hur toouse förpackaren toocreate avbildningar av virtuella Linux-datorer i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 5990598859e73efac477884bc8de5fd5138bf6e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a><span data-ttu-id="39cbc-103">Hur toouse förpackaren toocreate Linux virtuella bilder i Azure</span><span class="sxs-lookup"><span data-stu-id="39cbc-103">How toouse Packer toocreate Linux virtual machine images in Azure</span></span>
<span data-ttu-id="39cbc-104">Varje virtuell dator (VM) i Azure skapas från en avbildning som definierar hello distribution för Linux och OS-version.</span><span class="sxs-lookup"><span data-stu-id="39cbc-104">Each virtual machine (VM) in Azure is created from an image that defines hello Linux distribution and OS version.</span></span> <span data-ttu-id="39cbc-105">Avbildningar kan innehålla förinstallerade program och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="39cbc-105">Images can include pre-installed applications and configurations.</span></span> <span data-ttu-id="39cbc-106">hello Azure Marketplace innehåller många första och tredje parts avbildningar för programmet miljöer och de vanligaste distributioner eller du kan skapa dina egna anpassade avbildningar som är anpassade tooyour behov.</span><span class="sxs-lookup"><span data-stu-id="39cbc-106">hello Azure Marketplace provides many first and third-party images for most common distributions and application environments, or you can create your own custom images tailored tooyour needs.</span></span> <span data-ttu-id="39cbc-107">Den här artikeln beskrivs hur toouse hello Öppna källa för [förpackaren](https://www.packer.io/) toodefine och skapa anpassade avbildningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="39cbc-107">This article details how toouse hello open source tool [Packer](https://www.packer.io/) toodefine and build custom images in Azure.</span></span>


## <a name="create-azure-resource-group"></a><span data-ttu-id="39cbc-108">Skapa Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="39cbc-108">Create Azure resource group</span></span>
<span data-ttu-id="39cbc-109">Under hello build processen skapar förpackaren tillfälliga Azure-resurser som den skapar Virtuella hello källdatorn.</span><span class="sxs-lookup"><span data-stu-id="39cbc-109">During hello build process, Packer creates temporary Azure resources as it builds hello source VM.</span></span> <span data-ttu-id="39cbc-110">toocapture som datakällan VM för användning som en avbildning måste du definiera en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="39cbc-110">toocapture that source VM for use as an image, you must define a resource group.</span></span> <span data-ttu-id="39cbc-111">hello lagras utdata från hello förpackaren skapandeprocess i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="39cbc-111">hello output from hello Packer build process is stored in this resource group.</span></span>

<span data-ttu-id="39cbc-112">Skapa en resursgrupp med [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="39cbc-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="39cbc-113">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="39cbc-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a><span data-ttu-id="39cbc-114">Skapa autentiseringsuppgifter för Azure</span><span class="sxs-lookup"><span data-stu-id="39cbc-114">Create Azure credentials</span></span>
<span data-ttu-id="39cbc-115">Packare autentiserar med Azure med hjälp av ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="39cbc-115">Packer authenticates with Azure using a service principal.</span></span> <span data-ttu-id="39cbc-116">En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som förpackaren.</span><span class="sxs-lookup"><span data-stu-id="39cbc-116">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Packer.</span></span> <span data-ttu-id="39cbc-117">Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure.</span><span class="sxs-lookup"><span data-stu-id="39cbc-117">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span>

<span data-ttu-id="39cbc-118">Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som förpackaren måste:</span><span class="sxs-lookup"><span data-stu-id="39cbc-118">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Packer needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="39cbc-119">Ett exempel på hello utdata från hello föregående kommandon är följande:</span><span class="sxs-lookup"><span data-stu-id="39cbc-119">An example of hello output from hello preceding commands is as follows:</span></span>

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

<span data-ttu-id="39cbc-120">tooauthenticate tooAzure du måste också tooobtain din Azure-prenumeration med ID med [az konto visa](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="39cbc-120">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="39cbc-121">Du kan använda hello utdata från dessa två kommandon i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="39cbc-121">You use hello output from these two commands in hello next step.</span></span>


## <a name="define-packer-template"></a><span data-ttu-id="39cbc-122">Definiera förpackaren mall</span><span class="sxs-lookup"><span data-stu-id="39cbc-122">Define Packer template</span></span>
<span data-ttu-id="39cbc-123">toobuild bilder du skapar en mall som en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="39cbc-123">toobuild images, you create a template as a JSON file.</span></span> <span data-ttu-id="39cbc-124">Du definierar builders i hello mall och provisioners som utför hello faktiska Byggprocessen.</span><span class="sxs-lookup"><span data-stu-id="39cbc-124">In hello template, you define builders and provisioners that carry out hello actual build process.</span></span> <span data-ttu-id="39cbc-125">Förpackaren har en [provisioner för Azure](https://www.packer.io/docs/builders/azure.html) som du kan toodefine Azure resurser, till exempel hello huvudnamn Tjänstereferenser skapade i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="39cbc-125">Packer has a [provisioner for Azure](https://www.packer.io/docs/builders/azure.html) that allows you toodefine Azure resources, such as hello service principal credentials created in hello preceding step.</span></span>

<span data-ttu-id="39cbc-126">Skapa en fil med namnet *ubuntu.json* och klistra in hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="39cbc-126">Create a file named *ubuntu.json* and paste hello following content.</span></span> <span data-ttu-id="39cbc-127">Ange egna värden för hello följande:</span><span class="sxs-lookup"><span data-stu-id="39cbc-127">Enter your own values for hello following:</span></span>

| <span data-ttu-id="39cbc-128">Parameter</span><span class="sxs-lookup"><span data-stu-id="39cbc-128">Parameter</span></span>                           | <span data-ttu-id="39cbc-129">Där tooobtain</span><span class="sxs-lookup"><span data-stu-id="39cbc-129">Where tooobtain</span></span> |
|-------------------------------------|----------------------------------------------------|
| <span data-ttu-id="39cbc-130">*client_id*</span><span class="sxs-lookup"><span data-stu-id="39cbc-130">*client_id*</span></span>                         | <span data-ttu-id="39cbc-131">Första raden i utdata från `az ad sp` skapa kommando - *appId*</span><span class="sxs-lookup"><span data-stu-id="39cbc-131">First line of output from `az ad sp` create command - *appId*</span></span> |
| <span data-ttu-id="39cbc-132">*client_secret*</span><span class="sxs-lookup"><span data-stu-id="39cbc-132">*client_secret*</span></span>                     | <span data-ttu-id="39cbc-133">Andra raden på utdata från `az ad sp` skapa kommando - *lösenord*</span><span class="sxs-lookup"><span data-stu-id="39cbc-133">Second line of output from `az ad sp` create command - *password*</span></span> |
| <span data-ttu-id="39cbc-134">*tenant_id*</span><span class="sxs-lookup"><span data-stu-id="39cbc-134">*tenant_id*</span></span>                         | <span data-ttu-id="39cbc-135">Tredje raden på utdata från `az ad sp` skapa kommando - *klient*</span><span class="sxs-lookup"><span data-stu-id="39cbc-135">Third line of output from `az ad sp` create command - *tenant*</span></span> |
| <span data-ttu-id="39cbc-136">*PRENUMERATIONSID*</span><span class="sxs-lookup"><span data-stu-id="39cbc-136">*subscription_id*</span></span>                   | <span data-ttu-id="39cbc-137">Utdata från `az account show` kommando</span><span class="sxs-lookup"><span data-stu-id="39cbc-137">Output from `az account show` command</span></span> |
| <span data-ttu-id="39cbc-138">*managed_image_resource_group_name*</span><span class="sxs-lookup"><span data-stu-id="39cbc-138">*managed_image_resource_group_name*</span></span> | <span data-ttu-id="39cbc-139">Namnet på resursgruppen som du skapade i hello första steg</span><span class="sxs-lookup"><span data-stu-id="39cbc-139">Name of resource group you created in hello first step</span></span> |
| <span data-ttu-id="39cbc-140">*managed_image_name*</span><span class="sxs-lookup"><span data-stu-id="39cbc-140">*managed_image_name*</span></span>                | <span data-ttu-id="39cbc-141">Namn för hello hanterad avbildning som har skapats</span><span class="sxs-lookup"><span data-stu-id="39cbc-141">Name for hello managed disk image that is created</span></span> |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

<span data-ttu-id="39cbc-142">Den här mallen bygger en avbildning Ubuntu 16.04 LTS, installerar NGINX sedan deprovisions hello VM.</span><span class="sxs-lookup"><span data-stu-id="39cbc-142">This template builds an Ubuntu 16.04 LTS image, installs NGINX, then deprovisions hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="39cbc-143">Om du expanderar på den här mallen tooprovision användarens autentiseringsuppgifter, justera hello provisioner kommandot att deprovisions hello Azure-agenten tooread `-deprovision` snarare än `deprovision+user`.</span><span class="sxs-lookup"><span data-stu-id="39cbc-143">If you expand on this template tooprovision user credentials, adjust hello provisioner command that deprovisions hello Azure agent tooread `-deprovision` rather than `deprovision+user`.</span></span>
> <span data-ttu-id="39cbc-144">Hej `+user` flaggan tar bort alla användarkonton från Virtuella hello källdatorn.</span><span class="sxs-lookup"><span data-stu-id="39cbc-144">hello `+user` flag removes all user accounts from hello source VM.</span></span>


## <a name="build-packer-image"></a><span data-ttu-id="39cbc-145">Skapa förpackaren bild</span><span class="sxs-lookup"><span data-stu-id="39cbc-145">Build Packer image</span></span>
<span data-ttu-id="39cbc-146">Om du inte redan har förpackaren installerad på din lokala dator [följa hello förpackaren Installationsinstruktioner](https://www.packer.io/docs/install/index.html).</span><span class="sxs-lookup"><span data-stu-id="39cbc-146">If you don't already have Packer installed on your local machine, [follow hello Packer installation instructions](https://www.packer.io/docs/install/index.html).</span></span>

<span data-ttu-id="39cbc-147">Skapa hello bilden genom att ange din förpackaren mallfilen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="39cbc-147">Build hello image by specifying your Packer template file as follows:</span></span>

```bash
./packer build ubuntu.json
```

<span data-ttu-id="39cbc-148">Ett exempel på hello utdata från hello föregående kommandon är följande:</span><span class="sxs-lookup"><span data-stu-id="39cbc-148">An example of hello output from hello preceding commands is as follows:</span></span>

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting hello VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH toobecome available...
==> azure-arm: Connected tooSSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! hello waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able toologin as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying hello machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting hello temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. hello artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```


## <a name="create-vm-from-azure-image"></a><span data-ttu-id="39cbc-149">Skapa virtuell dator från Azure-avbildning</span><span class="sxs-lookup"><span data-stu-id="39cbc-149">Create VM from Azure Image</span></span>
<span data-ttu-id="39cbc-150">Nu kan du skapa en virtuell dator från avbildningen med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="39cbc-150">You can now create a VM from your Image with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="39cbc-151">Ange hello bilden som du skapade med hello `--image` parameter.</span><span class="sxs-lookup"><span data-stu-id="39cbc-151">Specify hello Image you created with hello `--image` parameter.</span></span> <span data-ttu-id="39cbc-152">hello följande exempel skapas en virtuell dator med namnet *myVM* från *myPackerImage* och genererar SSH-nycklar, om de inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="39cbc-152">hello following example creates a VM named *myVM* from *myPackerImage* and generates SSH keys if they do not already exist:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="39cbc-153">Det tar några minuter toocreate hello VM.</span><span class="sxs-lookup"><span data-stu-id="39cbc-153">It takes a few minutes toocreate hello VM.</span></span> <span data-ttu-id="39cbc-154">När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="39cbc-154">Once hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="39cbc-155">Den här adressen är används tooaccess hello NGINX plats via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="39cbc-155">This address is used tooaccess hello NGINX site via a web browser.</span></span>

<span data-ttu-id="39cbc-156">tooallow web tooreach trafik den virtuella datorn, öppna port 80 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="39cbc-156">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a><span data-ttu-id="39cbc-157">Testa virtuell dator och NGINX</span><span class="sxs-lookup"><span data-stu-id="39cbc-157">Test VM and NGINX</span></span>
<span data-ttu-id="39cbc-158">Nu kan du öppna en webbläsare och ange `http://publicIpAddress` i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="39cbc-158">Now you can open a web browser and enter `http://publicIpAddress` in hello address bar.</span></span> <span data-ttu-id="39cbc-159">Ange dina egna offentliga IP-adress från hello VM skapa processen.</span><span class="sxs-lookup"><span data-stu-id="39cbc-159">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="39cbc-160">hello visas standard NGINX som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="39cbc-160">hello default NGINX page is displayed as in hello following example:</span></span>

![NGINX-standardwebbplats](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a><span data-ttu-id="39cbc-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39cbc-162">Next steps</span></span>
<span data-ttu-id="39cbc-163">I det här exemplet används förpackaren toocreate en VM-avbildning med NGINX som redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="39cbc-163">In this example, you used Packer toocreate a VM image with NGINX already installed.</span></span> <span data-ttu-id="39cbc-164">Du kan använda den här Virtuella datorn tillsammans med befintliga arbetsflöden, till exempel toodeploy app-tooVMs skapas från hello avbildningen med Ansible, Chef eller Puppet.</span><span class="sxs-lookup"><span data-stu-id="39cbc-164">You can use this VM image alongside existing deployment workflows, such as toodeploy your app tooVMs created from hello Image with Ansible, Chef, or Puppet.</span></span>

<span data-ttu-id="39cbc-165">Ytterligare exempel förpackaren mallar för andra Linux-distributioner, se [GitHub-repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span><span class="sxs-lookup"><span data-stu-id="39cbc-165">For additional example Packer templates for other Linux distros, see [this GitHub repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span></span>