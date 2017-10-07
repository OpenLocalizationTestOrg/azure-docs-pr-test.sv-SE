---
title: aaaAzure CLI skriptexempel - hantera pooler i Batch | Microsoft Docs
description: Azure CLI skriptexempel - hantera pooler i Batch
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="0b016-103">Hantera Azure Batch-pooler med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0b016-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="0b016-104">Dessa skript visar några av hello verktygen i hello Azure CLI toocreate och hantera pooler för beräkningsnoder i hello Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0b016-104">These script demonstrates some of hello tools available in hello Azure CLI toocreate and manage pools of compute nodes in hello Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="0b016-105">hello kommandona i det här exemplet skapas virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="0b016-105">hello commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="0b016-106">Virtuella datorer som körs kommer påförs kostnader tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="0b016-106">Running VMs will accrue charges tooyour account.</span></span> <span data-ttu-id="0b016-107">toominimize dessa debiteringar ta bort hello virtuella datorer när du är klar körs hello-exemplet.</span><span class="sxs-lookup"><span data-stu-id="0b016-107">toominimize these charges, delete hello VMs once you're done running hello sample.</span></span> <span data-ttu-id="0b016-108">Se [Rensa pooler](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="0b016-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="0b016-109">Batch-pooler kan konfigureras på två sätt, antingen med Cloud Services-konfiguration (Windows) eller en virtuell dator-konfiguration (Windows och Linux).</span><span class="sxs-lookup"><span data-stu-id="0b016-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="0b016-110">hello exempelskript nedan visar hur toocreate pooler med båda konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="0b016-110">hello sample scripts below show how toocreate pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b016-111">Krav</span><span class="sxs-lookup"><span data-stu-id="0b016-111">Prerequisites</span></span>

- <span data-ttu-id="0b016-112">Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="0b016-112">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="0b016-113">Skapa ett Batch-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="0b016-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="0b016-114">Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="0b016-114">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="0b016-115">Konfigurera ett program toorun från en start-aktivitet om du inte gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="0b016-115">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="0b016-116">Se [att lägga till program tooAzure Batch med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) för ett exempelskript som skapar ett program och överför en programmet paketet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0b016-116">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="0b016-117">Pool med cloud service configuration exempelskript</span><span class="sxs-lookup"><span data-stu-id="0b016-117">Pool with cloud service configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="0b016-118">Pool med exempelskript konfigurationen för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0b016-118">Pool with virtual machine configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a><span data-ttu-id="0b016-119">Rensa pooler</span><span class="sxs-lookup"><span data-stu-id="0b016-119">Clean up pools</span></span>

<span data-ttu-id="0b016-120">När du har kört hello ovan exempelskript kör du följande kommando toodelete hello pooler hello.</span><span class="sxs-lookup"><span data-stu-id="0b016-120">After you run hello above sample script, run hello following command toodelete hello pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="0b016-121">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="0b016-121">Script explanation</span></span>

<span data-ttu-id="0b016-122">Det här skriptet använder följande kommandon toocreate hello och manipulera Batch pooler.</span><span class="sxs-lookup"><span data-stu-id="0b016-122">This script uses hello following commands toocreate and manipulate Batch pools.</span></span>
<span data-ttu-id="0b016-123">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="0b016-123">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="0b016-124">Kommando</span><span class="sxs-lookup"><span data-stu-id="0b016-124">Command</span></span> | <span data-ttu-id="0b016-125">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0b016-125">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0b016-126">logga in AZ batch-konto</span><span class="sxs-lookup"><span data-stu-id="0b016-126">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="0b016-127">Autentisera mot en Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="0b016-127">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="0b016-128">Sammanfattning av AZ batch-program</span><span class="sxs-lookup"><span data-stu-id="0b016-128">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="0b016-129">Lista över hello tillgängliga program i hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="0b016-129">List hello available applications in hello Batch account.</span></span>  |
| [<span data-ttu-id="0b016-130">Skapa AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="0b016-130">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="0b016-131">Skapa en pool för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0b016-131">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="0b016-132">AZ batch-pool uppsättningen</span><span class="sxs-lookup"><span data-stu-id="0b016-132">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="0b016-133">Uppdatera egenskaperna för en pool.</span><span class="sxs-lookup"><span data-stu-id="0b016-133">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="0b016-134">listan för AZ batch pool nod-agent-SKU: er</span><span class="sxs-lookup"><span data-stu-id="0b016-134">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="0b016-135">Lista över tillgängliga noden agent SKU: er och bild information.</span><span class="sxs-lookup"><span data-stu-id="0b016-135">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="0b016-136">Ändra storlek AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="0b016-136">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="0b016-137">Ändra storlek på hello antalet virtuella datorer som körs i hello angetts poolen.</span><span class="sxs-lookup"><span data-stu-id="0b016-137">Resize hello number of running VMs in hello specified pool.</span></span>  |
| [<span data-ttu-id="0b016-138">Visa för AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="0b016-138">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="0b016-139">Visar hello egenskaperna för en pool.</span><span class="sxs-lookup"><span data-stu-id="0b016-139">Display hello properties of a pool.</span></span>  |
| [<span data-ttu-id="0b016-140">ta bort AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="0b016-140">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="0b016-141">Ta bort hello angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="0b016-141">Delete hello specified pool.</span></span>  |
| [<span data-ttu-id="0b016-142">Aktivera AZ batch-pool Autoskala</span><span class="sxs-lookup"><span data-stu-id="0b016-142">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="0b016-143">Aktivera automatisk skalning på poolen och tillämpa en formel.</span><span class="sxs-lookup"><span data-stu-id="0b016-143">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="0b016-144">inaktivera AZ batch-pool Autoskala</span><span class="sxs-lookup"><span data-stu-id="0b016-144">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="0b016-145">Inaktivera automatisk skalning på poolen.</span><span class="sxs-lookup"><span data-stu-id="0b016-145">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="0b016-146">AZ batch nodlistan</span><span class="sxs-lookup"><span data-stu-id="0b016-146">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="0b016-147">Lista över alla hello datornod i hello angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="0b016-147">List all hello compute node in hello specified pool.</span></span>  |
| [<span data-ttu-id="0b016-148">omstart av AZ batch nod</span><span class="sxs-lookup"><span data-stu-id="0b016-148">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="0b016-149">Starta om hello angivna compute-nod.</span><span class="sxs-lookup"><span data-stu-id="0b016-149">Reboot hello specified compute node.</span></span>  |
| [<span data-ttu-id="0b016-150">ta bort AZ batch-nod</span><span class="sxs-lookup"><span data-stu-id="0b016-150">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="0b016-151">Ta bort hello visas noder från hello angetts poolen.</span><span class="sxs-lookup"><span data-stu-id="0b016-151">Delete hello listed nodes from hello specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="0b016-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b016-152">Next steps</span></span>

<span data-ttu-id="0b016-153">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0b016-153">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0b016-154">Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0b016-154">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

