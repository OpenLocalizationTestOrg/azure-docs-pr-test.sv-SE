---
title: "aaaAzure CLI skriptexempel - skapa en Docker-värd | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Docker-värd"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a><span data-ttu-id="a5423-103">Skapa en virtuell dator med Docker</span><span class="sxs-lookup"><span data-stu-id="a5423-103">Create a VM with Docker</span></span>

<span data-ttu-id="a5423-104">Det här skriptet skapar en virtuell dator med Docker aktiverad och startar en dockerbehållare som kör NGINX.</span><span class="sxs-lookup"><span data-stu-id="a5423-104">This script creates a virtual machine with Docker enabled and starts a Docker container running NGINX.</span></span> <span data-ttu-id="a5423-105">När du har kört hello skript du komma åt hello NGINX-webbservern via hello FQDN för hello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="a5423-105">After running hello script, you can access hello NGINX web server through hello FQDN of hello Azure virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a5423-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a5423-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a5423-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a5423-107">Clean up deployment</span></span> 

<span data-ttu-id="a5423-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="a5423-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a5423-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a5423-109">Script explanation</span></span>

<span data-ttu-id="a5423-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="a5423-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="a5423-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a5423-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a5423-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="a5423-112">Command</span></span> | <span data-ttu-id="a5423-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a5423-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a5423-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="a5423-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a5423-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a5423-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a5423-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="a5423-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="a5423-117">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="a5423-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="a5423-118">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a5423-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="a5423-119">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="a5423-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="a5423-120">Skapar en network security group regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="a5423-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="a5423-121">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="a5423-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="a5423-122">Azure vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="a5423-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="a5423-123">Lägger till och kör en virtuell dator tillägget tooa VM.</span><span class="sxs-lookup"><span data-stu-id="a5423-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="a5423-124">I det här exemplet är hello Docker VM-tillägget används tooconfigure en Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="a5423-124">In this sample, hello Docker VM extension is used tooconfigure a Docker host.</span></span>|
| [<span data-ttu-id="a5423-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="a5423-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="a5423-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="a5423-126">Deletes a resource group, including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a5423-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5423-127">Next steps</span></span>

<span data-ttu-id="a5423-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a5423-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a5423-129">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a5423-129">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
