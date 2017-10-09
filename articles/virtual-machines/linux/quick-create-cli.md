---
title: aaaAzure Snabbstart - skapa VM-CLI | Microsoft Docs
description: "Lär dig snabbt toocreate virtuella datorer med hello Azure CLI."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0d9c686e2f4c7339b29b8c43cd1dd9ee56d7dc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="a6fe8-103">Skapa en virtuell Linux-dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a6fe8-103">Create a Linux virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="a6fe8-104">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="a6fe8-105">Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell dator som kör Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-105">This guide details using hello Azure CLI toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="a6fe8-106">När hello server har distribuerats, en SSH-anslutning skapas och en NGINX-webbserver är installerad.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="a6fe8-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a6fe8-108">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a6fe8-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a6fe8-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a6fe8-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="a6fe8-111">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a6fe8-111">Create a resource group</span></span>

<span data-ttu-id="a6fe8-112">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-112">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="a6fe8-113">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="a6fe8-114">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="a6fe8-115">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a6fe8-115">Create virtual machine</span></span>

<span data-ttu-id="a6fe8-116">Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-116">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="a6fe8-117">hello följande exempel skapas en virtuell dator med namnet *myVM* och skapar SSH-nycklar, om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-117">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="a6fe8-118">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="a6fe8-119">När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-119">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="a6fe8-120">Anteckna hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-120">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="a6fe8-121">Den här adressen är används tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-121">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="a6fe8-122">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="a6fe8-122">Open port 80 for web traffic</span></span> 

<span data-ttu-id="a6fe8-123">Som standard tillåts enbart SSH-anslutningar till virtuella Linux-datorer distribuerade i Azure.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-123">By default only SSH connections are allowed into Linux virtual machines deployed in Azure.</span></span> <span data-ttu-id="a6fe8-124">Om den här virtuella datorn ska toobe en webbserver, måste tooopen port 80 från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-124">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="a6fe8-125">Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-125">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="a6fe8-126">SSH till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="a6fe8-126">SSH into your VM</span></span>

<span data-ttu-id="a6fe8-127">Använd hello följande kommando toocreate en SSH-session med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-127">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="a6fe8-128">Se till att tooreplace  *<publicIpAddress>*  med hello rätt offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-128">Make sure tooreplace *<publicIpAddress>* with hello correct public IP address of your virtual machine.</span></span>  <span data-ttu-id="a6fe8-129">I vårt exempel ovan var vår IP-adress *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-129">In our example above our IP address was *40.68.254.142*.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a><span data-ttu-id="a6fe8-130">Installera NGINX</span><span class="sxs-lookup"><span data-stu-id="a6fe8-130">Install NGINX</span></span>

<span data-ttu-id="a6fe8-131">Använd hello följande bash skriptet tooupdate paketet källor och installera hello senaste NGINX-paketet.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-131">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-nginx-welcome-page"></a><span data-ttu-id="a6fe8-132">Visa hello NGINX-välkomstsidan</span><span class="sxs-lookup"><span data-stu-id="a6fe8-132">View hello NGINX welcome page</span></span>

<span data-ttu-id="a6fe8-133">Du kan använda en webbläsare för dina val tooview hello NGINX välkomstsidan med NGINX installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-133">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="a6fe8-134">Vara säker på att toouse hello *publicIpAddress* du dokumenterade ovan toovisit hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-134">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![NGINX-standardwebbplats](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a><span data-ttu-id="a6fe8-136">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="a6fe8-136">Clean up resources</span></span>

<span data-ttu-id="a6fe8-137">När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-137">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="a6fe8-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6fe8-138">Next steps</span></span>

<span data-ttu-id="a6fe8-139">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-139">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="a6fe8-140">toolearn mer om Azure-datorer, fortsätta toohello självstudier för Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a6fe8-140">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="a6fe8-141">Självstudier om virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="a6fe8-141">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
