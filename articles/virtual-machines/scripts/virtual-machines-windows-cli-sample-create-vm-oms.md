---
title: "aaaAzure CLI skriptexempel - skapa en virtuell Windows Server 2016-dator med övervakning av OMS | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Windows Server 2016 VM med OMS-övervakning"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 4f070430ccc5d5402ed4a80ead3b78eff25dcd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="2cf09-103">Övervaka en virtuell dator med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="2cf09-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="2cf09-104">Det här skriptet skapar en virtuell dator i Azure, hello Operations Management Suite (OMS) agent installeras och registrerar hello system med en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="2cf09-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="2cf09-105">När hello skriptet har körts visas hello virtuell dator i hello OMS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="2cf09-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2cf09-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2cf09-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="2cf09-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="2cf09-107">Clean up deployment</span></span> 

<span data-ttu-id="2cf09-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="2cf09-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="2cf09-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2cf09-109">Script explanation</span></span>

<span data-ttu-id="2cf09-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2cf09-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="2cf09-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2cf09-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2cf09-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="2cf09-112">Command</span></span> | <span data-ttu-id="2cf09-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2cf09-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2cf09-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="2cf09-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2cf09-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2cf09-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2cf09-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="2cf09-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="2cf09-117">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="2cf09-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="2cf09-118">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2cf09-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="2cf09-119">Azure vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="2cf09-119">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2cf09-120">Kör en VM-tillägget mot en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2cf09-120">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="2cf09-121">I det här fallet är används tooinstall hello OMS-agent hello Operations Management Suite-agenten tillägget och registrera hello VM i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="2cf09-121">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="2cf09-122">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="2cf09-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2cf09-123">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="2cf09-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2cf09-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cf09-124">Next steps</span></span>

<span data-ttu-id="2cf09-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2cf09-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2cf09-126">Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2cf09-126">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
