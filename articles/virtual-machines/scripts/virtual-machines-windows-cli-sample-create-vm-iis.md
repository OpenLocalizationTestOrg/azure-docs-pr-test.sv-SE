---
title: aaaAzure CLI skriptexempel - installera IIS | Microsoft Docs
description: Azure CLI skriptexempel - installera IIS
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 2fabc9522f424cab4c672084ba8bedfd623c87cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="58ed1-103">Snabbt skapa en virtuell dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="58ed1-103">Quick Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="58ed1-104">Det här skriptet skapar en Azure-dator som kör Windows Server 2016 och använder hello Azure virtuella tillägget för anpassat skript tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="58ed1-104">This script creates an Azure Virtual Machine running Windows Server 2016, and uses hello Azure Virtual Machine Custom Script Extension tooinstall IIS.</span></span> <span data-ttu-id="58ed1-105">Du kan komma åt hello IIS-standardwebbplatsen på hello offentliga IP-adress hello virtuella datorn när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="58ed1-105">After running hello script, you can access hello default IIS website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="58ed1-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="58ed1-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="58ed1-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="58ed1-107">Clean up deployment</span></span> 

<span data-ttu-id="58ed1-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="58ed1-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="58ed1-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="58ed1-109">Script explanation</span></span>

<span data-ttu-id="58ed1-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="58ed1-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="58ed1-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="58ed1-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="58ed1-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="58ed1-112">Command</span></span> | <span data-ttu-id="58ed1-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="58ed1-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="58ed1-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="58ed1-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="58ed1-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="58ed1-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="58ed1-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="58ed1-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="58ed1-117">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="58ed1-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="58ed1-118">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="58ed1-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="58ed1-119">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="58ed1-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="58ed1-120">Skapar en network security group regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="58ed1-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="58ed1-121">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="58ed1-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="58ed1-122">Azure vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="58ed1-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="58ed1-123">Lägger till och kör en virtuell dator tillägget tooa VM.</span><span class="sxs-lookup"><span data-stu-id="58ed1-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="58ed1-124">I det här exemplet är hello tillägget för anpassat skript används tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="58ed1-124">In this sample, hello custom script extension is used tooinstall IIS.</span></span>|
| [<span data-ttu-id="58ed1-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="58ed1-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="58ed1-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="58ed1-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="58ed1-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58ed1-127">Next steps</span></span>

<span data-ttu-id="58ed1-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="58ed1-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="58ed1-129">Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58ed1-129">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
