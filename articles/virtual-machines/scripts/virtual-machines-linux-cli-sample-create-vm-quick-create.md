---
title: aaaAzure CLI skriptexempel - snabbt skapa en Linux VM | Microsoft Docs
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
ms.openlocfilehash: edecf274f4e401af3603e5be37a989e2e0eb22c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="8f359-103">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8f359-103">Create a virtual machine</span></span>

<span data-ttu-id="8f359-104">Det här skriptet skapar en virtuell dator i Azure med ett Ubuntu-operativsystem och relaterade nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="8f359-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="8f359-105">Du kan komma åt hello virtuella datorn när du har kört hello skript via SSH.</span><span class="sxs-lookup"><span data-stu-id="8f359-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8f359-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8f359-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8f359-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8f359-107">Clean up deployment</span></span> 

<span data-ttu-id="8f359-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="8f359-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8f359-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8f359-109">Script explanation</span></span>

<span data-ttu-id="8f359-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8f359-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="8f359-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8f359-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8f359-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="8f359-112">Command</span></span> | <span data-ttu-id="8f359-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8f359-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8f359-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="8f359-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8f359-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8f359-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8f359-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="8f359-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8f359-117">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="8f359-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="8f359-118">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8f359-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="8f359-119">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="8f359-119">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8f359-120">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="8f359-120">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8f359-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f359-121">Next steps</span></span>

<span data-ttu-id="8f359-122">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8f359-122">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8f359-123">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8f359-123">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
