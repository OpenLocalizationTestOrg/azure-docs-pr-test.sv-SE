---
title: "aaaCreate en Linux VM i Azure från en mall | Microsoft Docs"
description: "Hur hello toouse Azure CLI 2.0 toocreate en Linux VM från en Resource Manager-mall"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a><span data-ttu-id="e3045-103">Hur toocreate en Linux-dator med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="e3045-103">How toocreate a Linux virtual machine with Azure Resource Manager templates</span></span>
<span data-ttu-id="e3045-104">Den här artikeln visar hur tooquickly distribuera en Linux-dator (VM) med Azure Resource Manager-mallar och hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="e3045-104">This article shows you how tooquickly deploy a Linux virtual machine (VM) with Azure Resource Manager templates and hello Azure CLI 2.0.</span></span> <span data-ttu-id="e3045-105">Du kan också utföra dessa steg med hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e3045-105">You can also perform these steps with hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span></span>


## <a name="templates-overview"></a><span data-ttu-id="e3045-106">Översikt över mallar</span><span class="sxs-lookup"><span data-stu-id="e3045-106">Templates overview</span></span>
<span data-ttu-id="e3045-107">Azure Resource Manager-mallarna är JSON-filer som definierar hello-infrastrukturen och konfigurationen av din Azure-lösning.</span><span class="sxs-lookup"><span data-stu-id="e3045-107">Azure Resource Manager templates are JSON files that define hello infrastructure and configuration of your Azure solution.</span></span> <span data-ttu-id="e3045-108">Genom att använda en mall kan du distribuera lösningen flera gånger under dess livscykel och vara säker på att dina resurser distribueras konsekvent.</span><span class="sxs-lookup"><span data-stu-id="e3045-108">By using a template, you can repeatedly deploy your solution throughout its lifecycle and have confidence your resources are deployed in a consistent state.</span></span> <span data-ttu-id="e3045-109">toolearn mer om hello format hello mall och hur du skapar det, se [skapa din första Azure Resource Manager-mallen](../../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="e3045-109">toolearn more about hello format of hello template and how you construct it, see [Create your first Azure Resource Manager template](../../azure-resource-manager/resource-manager-create-first-template.md).</span></span> <span data-ttu-id="e3045-110">tooview hello JSON-syntax för typer av resurser, se [definiera resurser i Azure Resource Manager-mallar](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="e3045-110">tooview hello JSON syntax for resources types, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span>


## <a name="create-resource-group"></a><span data-ttu-id="e3045-111">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e3045-111">Create resource group</span></span>
<span data-ttu-id="e3045-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e3045-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="e3045-113">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e3045-113">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="e3045-114">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupVM* i hello *eastus* region:</span><span class="sxs-lookup"><span data-stu-id="e3045-114">hello following example creates a resource group named *myResourceGroupVM* in hello *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="e3045-115">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e3045-115">Create virtual machine</span></span>
<span data-ttu-id="e3045-116">hello följande exempel skapas en virtuell dator från [Azure Resource Manager-mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="e3045-116">hello following example creates a VM from [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="e3045-117">Ange hello värde egna SSH offentlig nyckel, till exempel hello innehållet i *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="e3045-117">Provide hello value of your own SSH public key, such as hello contents of *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="e3045-118">Om du behöver toocreate ett SSH-nyckelpar finns [hur toocreate och använder en SSH nyckelpar för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="e3045-118">If you need toocreate an SSH key pair, see [How toocreate and use an SSH key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

<span data-ttu-id="e3045-119">I det här exemplet anges en mall som lagras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="e3045-119">In this example, you specified a template stored in GitHub.</span></span> <span data-ttu-id="e3045-120">Du kan också hämta eller skapa en mall och ange hello lokal sökväg med hello samma `--template-file` parameter.</span><span class="sxs-lookup"><span data-stu-id="e3045-120">You can also download or create a template and specify hello local path with hello same `--template-file` parameter.</span></span>

<span data-ttu-id="e3045-121">tooSSH tooyour VM, hämta hello offentliga IP-adress med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show):</span><span class="sxs-lookup"><span data-stu-id="e3045-121">tooSSH tooyour VM, obtain hello public IP address with [az network public-ip show](/cli/azure/network/public-ip#show):</span></span>

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="e3045-122">Du kan sedan SSH tooyour VM som vanligt:</span><span class="sxs-lookup"><span data-stu-id="e3045-122">You can then SSH tooyour VM as normal:</span></span>

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a><span data-ttu-id="e3045-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3045-123">Next steps</span></span>
<span data-ttu-id="e3045-124">I det här exemplet skapas en grundläggande Linux VM.</span><span class="sxs-lookup"><span data-stu-id="e3045-124">In this example, you created a basic Linux VM.</span></span> <span data-ttu-id="e3045-125">För mer Resource Manager-mallar som inkluderar ramverk för programmet eller skapa mer komplexa miljöer, bläddra hello [Azure quickstart mallgalleriet](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="e3045-125">For more Resource Manager templates that include application frameworks or create more complex environments, browse hello [Azure quickstart templates gallery](https://azure.microsoft.com/documentation/templates/).</span></span>
