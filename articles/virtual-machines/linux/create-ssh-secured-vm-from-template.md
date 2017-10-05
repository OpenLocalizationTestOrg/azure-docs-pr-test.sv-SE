---
title: "Skapa en Linux-VM i Azure från en mall | Microsoft Docs"
description: "Hur du använder Azure CLI 2.0 för att skapa en Linux VM från en Resource Manager-mall"
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
ms.openlocfilehash: 908a8a0c82b2d21fb25c9b33dbd714570d1ac272
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a><span data-ttu-id="37927-103">Så här skapar du en virtuell Linux-dator med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="37927-103">How to create a Linux virtual machine with Azure Resource Manager templates</span></span>
<span data-ttu-id="37927-104">Den här artikeln visar hur du snabbt distribuerar en Linux-dator (VM) med Azure Resource Manager-mallar och Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="37927-104">This article shows you how to quickly deploy a Linux virtual machine (VM) with Azure Resource Manager templates and the Azure CLI 2.0.</span></span> <span data-ttu-id="37927-105">Du kan också utföra dessa steg med [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="37927-105">You can also perform these steps with the [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span></span>


## <a name="templates-overview"></a><span data-ttu-id="37927-106">Översikt över mallar</span><span class="sxs-lookup"><span data-stu-id="37927-106">Templates overview</span></span>
<span data-ttu-id="37927-107">Azure Resource Manager-mallarna är JSON-filer som definierar infrastrukturen och konfigurationen av din Azure-lösning.</span><span class="sxs-lookup"><span data-stu-id="37927-107">Azure Resource Manager templates are JSON files that define the infrastructure and configuration of your Azure solution.</span></span> <span data-ttu-id="37927-108">Genom att använda en mall kan du distribuera lösningen flera gånger under dess livscykel och vara säker på att dina resurser distribueras konsekvent.</span><span class="sxs-lookup"><span data-stu-id="37927-108">By using a template, you can repeatedly deploy your solution throughout its lifecycle and have confidence your resources are deployed in a consistent state.</span></span> <span data-ttu-id="37927-109">Läs mer om formatet för mallen och hur du skapar i [skapa din första Azure Resource Manager-mallen](../../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="37927-109">To learn more about the format of the template and how you construct it, see [Create your first Azure Resource Manager template](../../azure-resource-manager/resource-manager-create-first-template.md).</span></span> <span data-ttu-id="37927-110">JSON-syntaxen för resurstyper finns i [Define resources in Azure Resource Manager templates](/azure/templates/) (Definiera resurser i Azure Resource Manager-mallar).</span><span class="sxs-lookup"><span data-stu-id="37927-110">To view the JSON syntax for resources types, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span>


## <a name="create-resource-group"></a><span data-ttu-id="37927-111">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="37927-111">Create resource group</span></span>
<span data-ttu-id="37927-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="37927-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="37927-113">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="37927-113">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="37927-114">I följande exempel skapas en resursgrupp med namnet *myResourceGroupVM* i den *eastus* region:</span><span class="sxs-lookup"><span data-stu-id="37927-114">The following example creates a resource group named *myResourceGroupVM* in the *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="37927-115">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="37927-115">Create virtual machine</span></span>
<span data-ttu-id="37927-116">I följande exempel skapas en virtuell dator från [Azure Resource Manager-mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) med [az distribution skapa](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="37927-116">The following example creates a VM from [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="37927-117">Ange värdet för egna SSH offentlig nyckel, till exempel innehållet i *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="37927-117">Provide the value of your own SSH public key, such as the contents of *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="37927-118">Om du behöver skapa en SSH-nyckel finns [hur du skapar och använder en SSH-nyckel för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="37927-118">If you need to create an SSH key pair, see [How to create and use an SSH key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

<span data-ttu-id="37927-119">I det här exemplet anges en mall som lagras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="37927-119">In this example, you specified a template stored in GitHub.</span></span> <span data-ttu-id="37927-120">Du kan också hämta eller skapa en mall och ange en lokal sökväg med samma `--template-file` parameter.</span><span class="sxs-lookup"><span data-stu-id="37927-120">You can also download or create a template and specify the local path with the same `--template-file` parameter.</span></span>

<span data-ttu-id="37927-121">Hämta den offentliga IP-adressen till SSH till den virtuella datorn, [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show):</span><span class="sxs-lookup"><span data-stu-id="37927-121">To SSH to your VM, obtain the public IP address with [az network public-ip show](/cli/azure/network/public-ip#show):</span></span>

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="37927-122">Du kan sedan SSH till den virtuella datorn som vanligt:</span><span class="sxs-lookup"><span data-stu-id="37927-122">You can then SSH to your VM as normal:</span></span>

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a><span data-ttu-id="37927-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37927-123">Next steps</span></span>
<span data-ttu-id="37927-124">I det här exemplet skapas en grundläggande Linux VM.</span><span class="sxs-lookup"><span data-stu-id="37927-124">In this example, you created a basic Linux VM.</span></span> <span data-ttu-id="37927-125">Fler Resource Manager-mallar som inkluderar ramverk för programmet eller skapa mer komplexa miljöer, bläddra i [Azure quickstart mallgalleriet](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="37927-125">For more Resource Manager templates that include application frameworks or create more complex environments, browse the [Azure quickstart templates gallery](https://azure.microsoft.com/documentation/templates/).</span></span>