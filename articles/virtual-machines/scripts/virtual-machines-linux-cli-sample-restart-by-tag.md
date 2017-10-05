---
title: Azure CLI skriptexempel - omstart VMs | Microsoft Docs
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
ms.openlocfilehash: 4d0fe95287c91a4b656904f9007ceaaf866e155f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="restart-vms"></a><span data-ttu-id="3c500-103">Starta om virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3c500-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="3c500-104">Det här exemplet visas ett par olika sätt att hämta vissa virtuella datorer och starta om dem.</span><span class="sxs-lookup"><span data-stu-id="3c500-104">This sample shows a couple of ways to get some VMs and restart them.</span></span>

<span data-ttu-id="3c500-105">Först startar om alla virtuella datorer i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3c500-105">The first restarts all the VMs in the resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="3c500-106">Andra hämtar taggade virtuella datorer med hjälp av `az resouce list` och filtrerar till resurser som virtuella datorer och startar om dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c500-106">The second gets the tagged VMs using `az resouce list` and filters to the resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="3c500-107">Det här exemplet fungerar i ett Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="3c500-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="3c500-108">Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows Azure CLI](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="3c500-108">For options on running Azure CLI scripts on Windows client, see [Running the Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="3c500-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="3c500-109">Sample script</span></span>

<span data-ttu-id="3c500-110">Exemplet har tre skript.</span><span class="sxs-lookup"><span data-stu-id="3c500-110">The sample has three scripts.</span></span>
<span data-ttu-id="3c500-111">Den första som etablerar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3c500-111">The first one provisions the virtual machines.</span></span>
<span data-ttu-id="3c500-112">Den använder alternativet Nej vänta så att kommandot returnerar utan att vänta på varje virtuell dator som ska etableras.</span><span class="sxs-lookup"><span data-stu-id="3c500-112">It uses the no-wait option so the command returns without waiting on each VM to be provisioned.</span></span>
<span data-ttu-id="3c500-113">Andra väntar på att de virtuella datorerna till helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="3c500-113">The second waits for the VMs to be fully provisioned.</span></span>
<span data-ttu-id="3c500-114">Skriptet tredje startar om alla de virtuella datorerna som var etablerade och sedan bara taggade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c500-114">The third script restarts all the VMs that were provisioned, and then just the tagged VMs.</span></span>

### <a name="provision-the-vms"></a><span data-ttu-id="3c500-115">Etablera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3c500-115">Provision the VMs</span></span>

<span data-ttu-id="3c500-116">Det här skriptet skapar en resursgrupp och skapar sedan tre virtuella datorer startas om.</span><span class="sxs-lookup"><span data-stu-id="3c500-116">This script creates a resource group and then it creates three VMs to restart.</span></span>
<span data-ttu-id="3c500-117">Två av dem märks.</span><span class="sxs-lookup"><span data-stu-id="3c500-117">Two of them are tagged.</span></span>

<span data-ttu-id="3c500-118">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "etablera virtuella datorer")]</span><span class="sxs-lookup"><span data-stu-id="3c500-118">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]</span></span>

### <a name="wait"></a><span data-ttu-id="3c500-119">Vänta</span><span class="sxs-lookup"><span data-stu-id="3c500-119">Wait</span></span>

<span data-ttu-id="3c500-120">Det här skriptet kontrollerar etablering statusen var 20 sekunder tills alla tre virtuella datorer är etablerad, eller en av dem inte kan etablera.</span><span class="sxs-lookup"><span data-stu-id="3c500-120">This script checks on the provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails to provision.</span></span>

<span data-ttu-id="3c500-121">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "vänta tills de virtuella datorerna som ska etableras")]</span><span class="sxs-lookup"><span data-stu-id="3c500-121">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]</span></span>

### <a name="restart-the-vms"></a><span data-ttu-id="3c500-122">Starta om de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="3c500-122">Restart the VMs</span></span>

<span data-ttu-id="3c500-123">Det här skriptet startar om alla virtuella datorer i resursgruppen och startas sedan om bara taggade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c500-123">This script restarts all the VMs in the resource group, and then it restarts just the tagged VMs.</span></span>

<span data-ttu-id="3c500-124">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "starta om virtuella datorer efter tagg")]</span><span class="sxs-lookup"><span data-stu-id="3c500-124">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="3c500-125">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="3c500-125">Clean up deployment</span></span> 

<span data-ttu-id="3c500-126">Efter skriptexempel har körts användas följande kommando för att ta bort resursgrupper, virtuella datorer och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3c500-126">After the script sample has been run, the following command can be used to remove the resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="3c500-127">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="3c500-127">Script explanation</span></span>

<span data-ttu-id="3c500-128">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3c500-128">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="3c500-129">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3c500-129">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3c500-130">Kommando</span><span class="sxs-lookup"><span data-stu-id="3c500-130">Command</span></span> | <span data-ttu-id="3c500-131">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3c500-131">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3c500-132">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="3c500-132">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3c500-133">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="3c500-133">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3c500-134">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="3c500-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="3c500-135">Skapar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3c500-135">Creates the virtual machines.</span></span>  |
| [<span data-ttu-id="3c500-136">AZ vm lista</span><span class="sxs-lookup"><span data-stu-id="3c500-136">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="3c500-137">Används med `--query` att säkerställa att de virtuella datorerna etableras innan du startar om dem, och för att hämta ID för virtuella datorer starta om dem.</span><span class="sxs-lookup"><span data-stu-id="3c500-137">Used with `--query` to ensure the VMs are provisioned before restarting them, and then to get the IDs of the VMs to restart them.</span></span> |
| [<span data-ttu-id="3c500-138">lista över resurser AZ</span><span class="sxs-lookup"><span data-stu-id="3c500-138">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="3c500-139">Används med `--query` att hämta ID: N för de virtuella datorerna med taggen.</span><span class="sxs-lookup"><span data-stu-id="3c500-139">Used with `--query` to get the IDs of the VMs using the tag.</span></span> |
| [<span data-ttu-id="3c500-140">AZ vm-omstart</span><span class="sxs-lookup"><span data-stu-id="3c500-140">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="3c500-141">Startar om de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3c500-141">Restarts the VMs.</span></span> |
| [<span data-ttu-id="3c500-142">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="3c500-142">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="3c500-143">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="3c500-143">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c500-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c500-144">Next steps</span></span>

<span data-ttu-id="3c500-145">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c500-145">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3c500-146">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c500-146">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
