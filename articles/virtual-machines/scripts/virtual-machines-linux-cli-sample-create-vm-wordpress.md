---
title: aaaAzure CLI skriptexempel - skapa en Linux VM med WordPress | Microsoft Docs
description: Azure CLI Script exempel - skapa en virtuell Linux-dator med WordPress
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
ms.openlocfilehash: 2c5c03d08b6d5d27eb8c505b1dbd817eda5f2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="ff551-103">Skapa en virtuell dator med WordPress</span><span class="sxs-lookup"><span data-stu-id="ff551-103">Create a VM with WordPress</span></span>

<span data-ttu-id="ff551-104">Det här skriptet skapar en virtuell dator och använder sedan hello Azure-dator anpassat skript tillägget tooinstall WordPress.</span><span class="sxs-lookup"><span data-stu-id="ff551-104">This script creates a virtual machine, and then uses hello Azure Virtual Machine custom script extension tooinstall WordPress.</span></span> <span data-ttu-id="ff551-105">Efter körs hello skript du kan komma åt hello WordPress configuration webbplatsen på `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="ff551-105">After running hello script, you can access hello WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ff551-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ff551-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ff551-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ff551-107">Clean up deployment</span></span> 

<span data-ttu-id="ff551-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="ff551-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ff551-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ff551-109">Script explanation</span></span>

<span data-ttu-id="ff551-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="ff551-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="ff551-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ff551-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ff551-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="ff551-112">Command</span></span> | <span data-ttu-id="ff551-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ff551-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ff551-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="ff551-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ff551-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ff551-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ff551-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="ff551-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="ff551-117">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="ff551-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="ff551-118">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ff551-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="ff551-119">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="ff551-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="ff551-120">Skapar en network security group regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="ff551-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="ff551-121">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="ff551-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="ff551-122">AZ vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="ff551-122">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="ff551-123">Lägg till hello tillägget för anpassat skript toohello virtuella datorn, som anropar en skriptet tooinstall WordPress.</span><span class="sxs-lookup"><span data-stu-id="ff551-123">Add hello Custom Script Extension toohello virtual machine, which invokes a script tooinstall WordPress.</span></span> |
| [<span data-ttu-id="ff551-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="ff551-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="ff551-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="ff551-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ff551-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff551-126">Next steps</span></span>

<span data-ttu-id="ff551-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ff551-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ff551-128">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff551-128">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
