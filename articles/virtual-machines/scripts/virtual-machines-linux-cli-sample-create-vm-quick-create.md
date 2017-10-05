---
title: Azure CLI skriptexempel - Snabbregistrering en virtuell Linux-dator | Microsoft Docs
description: Azure CLI skriptexempel - Snabbregistrering en virtuell Linux-dator
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
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3df3affcedf9edbe6e548d881a877f14204ec280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="f2d38-103">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f2d38-103">Create a virtual machine</span></span>

<span data-ttu-id="f2d38-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem och relaterade nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="f2d38-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="f2d38-105">När skriptet har körts kan du komma åt den virtuella datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="f2d38-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f2d38-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f2d38-106">Sample script</span></span>

<span data-ttu-id="f2d38-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="f2d38-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f2d38-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="f2d38-108">Clean up deployment</span></span> 

<span data-ttu-id="f2d38-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="f2d38-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f2d38-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f2d38-110">Script explanation</span></span>

<span data-ttu-id="f2d38-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="f2d38-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="f2d38-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f2d38-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f2d38-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="f2d38-113">Command</span></span> | <span data-ttu-id="f2d38-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f2d38-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f2d38-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="f2d38-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f2d38-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="f2d38-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f2d38-117">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="f2d38-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="f2d38-118">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="f2d38-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="f2d38-119">Det här kommandot anger också avbildningen av virtuella datorn ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f2d38-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="f2d38-120">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="f2d38-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="f2d38-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="f2d38-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f2d38-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2d38-122">Next steps</span></span>

<span data-ttu-id="f2d38-123">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f2d38-123">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f2d38-124">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f2d38-124">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
