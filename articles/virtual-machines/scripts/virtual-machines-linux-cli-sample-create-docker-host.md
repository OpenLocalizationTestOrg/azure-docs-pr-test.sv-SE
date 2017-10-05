---
title: "Azure CLI-skript Sample - skapa en Docker-värd | Microsoft Docs"
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
ms.openlocfilehash: e8704824dec66d724f2d30dab4d6bdf019c6b206
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-docker"></a><span data-ttu-id="5a20e-103">Skapa en virtuell dator med Docker</span><span class="sxs-lookup"><span data-stu-id="5a20e-103">Create a VM with Docker</span></span>

<span data-ttu-id="5a20e-104">Det här skriptet skapar en virtuell dator med Docker aktiverad och startar en dockerbehållare som kör NGINX.</span><span class="sxs-lookup"><span data-stu-id="5a20e-104">This script creates a virtual machine with Docker enabled and starts a Docker container running NGINX.</span></span> <span data-ttu-id="5a20e-105">När skriptet har körts kan du komma åt webbservern NGINX via det fullständiga Domännamnet för den virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="5a20e-105">After running the script, you can access the NGINX web server through the FQDN of the Azure virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5a20e-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="5a20e-106">Sample script</span></span>

<span data-ttu-id="5a20e-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker-värden")]</span><span class="sxs-lookup"><span data-stu-id="5a20e-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5a20e-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="5a20e-108">Clean up deployment</span></span> 

<span data-ttu-id="5a20e-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5a20e-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5a20e-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5a20e-110">Script explanation</span></span>

<span data-ttu-id="5a20e-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="5a20e-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="5a20e-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5a20e-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5a20e-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="5a20e-113">Command</span></span> | <span data-ttu-id="5a20e-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5a20e-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5a20e-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="5a20e-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5a20e-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="5a20e-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5a20e-117">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="5a20e-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5a20e-118">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="5a20e-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="5a20e-119">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5a20e-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="5a20e-120">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="5a20e-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="5a20e-121">Skapar en grupp nätverkssäkerhetsregeln för att tillåta inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="5a20e-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="5a20e-122">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="5a20e-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="5a20e-123">Azure vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="5a20e-123">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5a20e-124">Lägger till och kör ett tillägg för virtuell dator till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5a20e-124">Adds and runs a virtual machine extension to a VM.</span></span> <span data-ttu-id="5a20e-125">I det här exemplet används Docker VM-tillägget för att konfigurera en Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="5a20e-125">In this sample, the Docker VM extension is used to configure a Docker host.</span></span>|
| [<span data-ttu-id="5a20e-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="5a20e-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5a20e-127">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="5a20e-127">Deletes a resource group, including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5a20e-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a20e-128">Next steps</span></span>

<span data-ttu-id="5a20e-129">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5a20e-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5a20e-130">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a20e-130">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
