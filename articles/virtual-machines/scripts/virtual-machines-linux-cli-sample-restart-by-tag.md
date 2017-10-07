---
title: aaaAzure CLI skriptexempel - starta om VMs | Microsoft Docs
description: 'Skriptexempel Azure CLI - omstart av virtuella datorer efter tagg och -ID: t'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a><span data-ttu-id="1c75a-103">Starta om virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1c75a-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="1c75a-104">Det här exemplet visar några olika sätt tooget vissa virtuella datorer och starta om dem.</span><span class="sxs-lookup"><span data-stu-id="1c75a-104">This sample shows a couple of ways tooget some VMs and restart them.</span></span>

<span data-ttu-id="1c75a-105">hello startar om alla hello virtuella datorer i resursgruppen hello först.</span><span class="sxs-lookup"><span data-stu-id="1c75a-105">hello first restarts all hello VMs in hello resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="1c75a-106">hello andra hämtar hello märkta virtuella datorer med `az resouce list` filtrerar toohello resurser som virtuella datorer och startar om dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-106">hello second gets hello tagged VMs using `az resouce list` and filters toohello resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="1c75a-107">Det här exemplet fungerar i ett Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1c75a-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="1c75a-108">Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows hello Azure CLI](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="1c75a-108">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="1c75a-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1c75a-109">Sample script</span></span>

<span data-ttu-id="1c75a-110">hello exemplet har tre skript.</span><span class="sxs-lookup"><span data-stu-id="1c75a-110">hello sample has three scripts.</span></span>
<span data-ttu-id="1c75a-111">Hej först en bestämmelser hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-111">hello first one provisions hello virtual machines.</span></span>
<span data-ttu-id="1c75a-112">Hello Nej vänta alternativet används så hello kommando returnerar utan att vänta på varje VM-toobe etableras.</span><span class="sxs-lookup"><span data-stu-id="1c75a-112">It uses hello no-wait option so hello command returns without waiting on each VM toobe provisioned.</span></span>
<span data-ttu-id="1c75a-113">hello väntar andra hello VMs toobe helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="1c75a-113">hello second waits for hello VMs toobe fully provisioned.</span></span>
<span data-ttu-id="1c75a-114">hello tredje skript startar om alla hello VMs som etablerades och sedan bara hello märkta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-114">hello third script restarts all hello VMs that were provisioned, and then just hello tagged VMs.</span></span>

### <a name="provision-hello-vms"></a><span data-ttu-id="1c75a-115">Etablera hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1c75a-115">Provision hello VMs</span></span>

<span data-ttu-id="1c75a-116">Det här skriptet skapar en resursgrupp och skapar sedan tre virtuella datorer toorestart.</span><span class="sxs-lookup"><span data-stu-id="1c75a-116">This script creates a resource group and then it creates three VMs toorestart.</span></span>
<span data-ttu-id="1c75a-117">Två av dem märks.</span><span class="sxs-lookup"><span data-stu-id="1c75a-117">Two of them are tagged.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a><span data-ttu-id="1c75a-118">Vänta</span><span class="sxs-lookup"><span data-stu-id="1c75a-118">Wait</span></span>

<span data-ttu-id="1c75a-119">Det här skriptet kontrollerar hello Etableringsstatus var 20 sekunder tills alla tre virtuella datorer är etablerad, eller en av dem misslyckas tooprovision.</span><span class="sxs-lookup"><span data-stu-id="1c75a-119">This script checks on hello provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails tooprovision.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a><span data-ttu-id="1c75a-120">Starta om hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1c75a-120">Restart hello VMs</span></span>

<span data-ttu-id="1c75a-121">Det här skriptet startar om alla hello virtuella datorer i resursgruppen hello och startas sedan om bara hello märkta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-121">This script restarts all hello VMs in hello resource group, and then it restarts just hello tagged VMs.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1c75a-122">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="1c75a-122">Clean up deployment</span></span> 

<span data-ttu-id="1c75a-123">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupper, virtuella datorer och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="1c75a-123">After hello script sample has been run, hello following command can be used tooremove hello resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="1c75a-124">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1c75a-124">Script explanation</span></span>

<span data-ttu-id="1c75a-125">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="1c75a-125">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="1c75a-126">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1c75a-126">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1c75a-127">Kommando</span><span class="sxs-lookup"><span data-stu-id="1c75a-127">Command</span></span> | <span data-ttu-id="1c75a-128">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1c75a-128">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1c75a-129">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="1c75a-129">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1c75a-130">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="1c75a-130">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1c75a-131">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="1c75a-131">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="1c75a-132">Skapar hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-132">Creates hello virtual machines.</span></span>  |
| [<span data-ttu-id="1c75a-133">AZ vm lista</span><span class="sxs-lookup"><span data-stu-id="1c75a-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="1c75a-134">Används med `--query` tooensure hello VMs etableras innan du startar om dem och sedan tooget hello-ID för hello VMs toorestart dem.</span><span class="sxs-lookup"><span data-stu-id="1c75a-134">Used with `--query` tooensure hello VMs are provisioned before restarting them, and then tooget hello IDs of hello VMs toorestart them.</span></span> |
| [<span data-ttu-id="1c75a-135">lista över resurser AZ</span><span class="sxs-lookup"><span data-stu-id="1c75a-135">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="1c75a-136">Används med `--query` tooget hello-ID för hello virtuella datorer med hello-tagg.</span><span class="sxs-lookup"><span data-stu-id="1c75a-136">Used with `--query` tooget hello IDs of hello VMs using hello tag.</span></span> |
| [<span data-ttu-id="1c75a-137">AZ vm-omstart</span><span class="sxs-lookup"><span data-stu-id="1c75a-137">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="1c75a-138">Startar om hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1c75a-138">Restarts hello VMs.</span></span> |
| [<span data-ttu-id="1c75a-139">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="1c75a-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="1c75a-140">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="1c75a-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1c75a-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c75a-141">Next steps</span></span>

<span data-ttu-id="1c75a-142">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1c75a-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1c75a-143">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1c75a-143">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
