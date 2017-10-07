---
title: "aaaCreate en Linux VM som använder en Azure-mall med Azure CLI 1.0 | Microsoft Docs"
description: "Skapa en Linux-VM på Azure med hjälp av en Azure Resource Manager-mall och hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a><span data-ttu-id="5ab3f-103">Hur toocreate en Linux VM som använder hello Azure CLI 1.0 en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="5ab3f-103">How toocreate a Linux VM using hello Azure CLI 1.0 an Azure Resource Manager template</span></span>
<span data-ttu-id="5ab3f-104">Den här artikeln visar hur tooquickly distribuera en Linux-dator med hjälp av hello Azure CLI 1.0 och en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-104">This article shows you how tooquickly deploy a Linux Virtual Machine using hello Azure CLI 1.0 and an Azure Resource Manager template.</span></span> <span data-ttu-id="5ab3f-105">hello artikel kräver:</span><span class="sxs-lookup"><span data-stu-id="5ab3f-105">hello article requires:</span></span>

* <span data-ttu-id="5ab3f-106">ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).</span><span class="sxs-lookup"><span data-stu-id="5ab3f-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="5ab3f-107">Hej [Azure CLI 1.0](../../cli-install-nodejs.md) loggat in med `azure login`.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-107">hello [Azure CLI 1.0](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="5ab3f-108">hello Azure CLI *måste vara i* läget Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

<span data-ttu-id="5ab3f-109">Du kan också snabbt distribuera en Linux VM-mall med hjälp av hello [Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5ab3f-109">You can also quickly deploy a Linux VM template by using hello [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5ab3f-110">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="5ab3f-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="5ab3f-111">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="5ab3f-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="5ab3f-112">[Azure CLI 1.0](#quick-command-summary) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="5ab3f-112">[Azure CLI 1.0](#quick-command-summary) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="5ab3f-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="5ab3f-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="5ab3f-114">Snabb kommandosammanfattning</span><span class="sxs-lookup"><span data-stu-id="5ab3f-114">Quick Command Summary</span></span>
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="5ab3f-115">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="5ab3f-115">Detailed Walkthrough</span></span>
<span data-ttu-id="5ab3f-116">Mallar kan du toocreate virtuella datorer i Azure med inställningar som du vill toocustomize under hello starten, inställningar för användarnamn och värdnamn.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-116">Templates allow you toocreate VMs on Azure with settings that you want toocustomize during hello launch, settings like usernames and hostnames.</span></span> <span data-ttu-id="5ab3f-117">För den här artikeln startar vi en Azure-mall med hjälp av en virtuell Ubuntu-dator tillsammans med en nätverkssäkerhetsgrupp med port 22 öppen för SSH.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-117">For this article, we are launching an Azure template utilizing an Ubuntu VM along with a network security group (NSG) with port 22 open for SSH.</span></span>

<span data-ttu-id="5ab3f-118">Azure Resource Manager-mallar är JSON-filer som kan användas för enkla engångsuppgifter som generering av en virtuell Ubuntu-dator, så som det görs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-118">Azure Resource Manager templates are JSON files that can be used for simple one-off tasks like launching an Ubuntu VM as done in this article.</span></span>  <span data-ttu-id="5ab3f-119">Azure-mallar kan också vara används tooconstruct komplexa Azure-konfigurationer i hela miljöer som en grupp för testning, utveckling eller produktion distribution.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-119">Azure Templates can also be used tooconstruct complex Azure configurations of entire environments like a testing, dev, or production deployment stack.</span></span>

## <a name="create-hello-linux-vm"></a><span data-ttu-id="5ab3f-120">Skapa hello Linux VM</span><span class="sxs-lookup"><span data-stu-id="5ab3f-120">Create hello Linux VM</span></span>
<span data-ttu-id="5ab3f-121">Hej följande exempel visas hur toocall `azure group create` toocreate en resurs gruppen och distribuera en SSH-skyddad Linux VM på hello samma gång med hjälp av [Azure Resource Manager-mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="5ab3f-121">hello following code example shows how toocall `azure group create` toocreate a resource group and deploy an SSH-secured Linux VM at hello same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span></span> <span data-ttu-id="5ab3f-122">Tänk på att i ditt exempel måste toouse namn som är unikt tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-122">Remember that in your example you need toouse names that are unique tooyour environment.</span></span> <span data-ttu-id="5ab3f-123">Det här exemplet används *myResourceGroup* som hello resursgruppens namn, och *myVM* som hello VM-namn.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-123">This example uses *myResourceGroup* as hello resource group name, and *myVM* as hello VM name.</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

<span data-ttu-id="5ab3f-124">hello utdata bör se ut som följande utdatablock hello:</span><span class="sxs-lookup"><span data-stu-id="5ab3f-124">hello output should look like hello following output block:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

<span data-ttu-id="5ab3f-125">Som exempel distribuera en virtuell dator med hjälp av hello `--template-uri` parameter.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-125">That example deployed a VM using hello `--template-uri` parameter.</span></span>  <span data-ttu-id="5ab3f-126">Du kan också hämta eller skapa en mall lokalt och skicka hello mallen med hjälp av hello `--template-file` parameter med en sökväg toohello mallfilen som argument.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-126">You can also download or create a template locally and pass hello template using hello `--template-file` parameter with a path toohello template file as an argument.</span></span> <span data-ttu-id="5ab3f-127">hello Azure CLI uppmanar dig att hello-parametrar som krävs av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-127">hello Azure CLI prompts you for hello parameters required by hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ab3f-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ab3f-128">Next steps</span></span>
<span data-ttu-id="5ab3f-129">Sök hello [mallgalleriet](https://azure.microsoft.com/documentation/templates/) toodiscover vilka appen ramverk toodeploy nästa.</span><span class="sxs-lookup"><span data-stu-id="5ab3f-129">Search hello [templates gallery](https://azure.microsoft.com/documentation/templates/) toodiscover what app frameworks toodeploy next.</span></span>

