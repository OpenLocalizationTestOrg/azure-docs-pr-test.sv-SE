---
title: aaaAzure CLI skriptexempel - skapa en Linux VM med NGINX | Microsoft Docs
description: Azure CLI Script exempel - skapa en virtuell Linux-dator med NGINX
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9166ccfd4f2e6eea731a8dc6956575d9f8f85488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-nginx"></a><span data-ttu-id="e5110-103">Skapa en virtuell dator med NGINX</span><span class="sxs-lookup"><span data-stu-id="e5110-103">Create a VM with NGINX</span></span>

<span data-ttu-id="e5110-104">Det här skriptet skapar en virtuell Azure-dator och använder hello Azure virtuella tillägget för anpassat skript tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="e5110-104">This script creates an Azure Virtual Machine and uses hello Azure Virtual Machine Custom Script Extension tooinstall NGINX.</span></span> <span data-ttu-id="e5110-105">Du kan komma åt en demo-webbplats på hello offentliga IP-adress hello virtuella datorn när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="e5110-105">After running hello script, you can access a demo website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="e5110-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e5110-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a><span data-ttu-id="e5110-107">Anpassat skripttillägg</span><span class="sxs-lookup"><span data-stu-id="e5110-107">Custom Script Extension</span></span>

<span data-ttu-id="e5110-108">hello-tillägget för anpassat skript kopierar det här skriptet på hello virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="e5110-108">hello custom script extension copies this script onto hello virtual machine.</span></span> <span data-ttu-id="e5110-109">hello skript körs tooinstall och konfigurera en NGINX-webbserver.</span><span class="sxs-lookup"><span data-stu-id="e5110-109">hello script is then run tooinstall and configure an NGINX web server.</span></span> 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a><span data-ttu-id="e5110-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="e5110-110">Clean up deployment</span></span> 

<span data-ttu-id="e5110-111">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="e5110-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="e5110-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e5110-112">Script explanation</span></span>

<span data-ttu-id="e5110-113">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e5110-113">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="e5110-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e5110-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e5110-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="e5110-115">Command</span></span> | <span data-ttu-id="e5110-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e5110-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e5110-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="e5110-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e5110-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e5110-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e5110-119">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="e5110-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="e5110-120">Skapar hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5110-120">Creates hello virtual machine.</span></span> <span data-ttu-id="e5110-121">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e5110-121">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="e5110-122">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="e5110-122">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="e5110-123">Skapar en network security group regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="e5110-123">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="e5110-124">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="e5110-124">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="e5110-125">Azure vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="e5110-125">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="e5110-126">Lägger till och kör en virtuell dator tillägget tooa VM.</span><span class="sxs-lookup"><span data-stu-id="e5110-126">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="e5110-127">I det här exemplet är hello tillägget för anpassat skript används tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="e5110-127">In this sample, hello custom script extension is used tooinstall NGINX.</span></span>|
| [<span data-ttu-id="e5110-128">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="e5110-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="e5110-129">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="e5110-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e5110-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5110-130">Next steps</span></span>

<span data-ttu-id="e5110-131">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e5110-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e5110-132">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5110-132">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
