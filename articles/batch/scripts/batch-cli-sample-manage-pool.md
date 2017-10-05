---
title: Azure CLI skriptexempel - hantera pooler i Batch | Microsoft Docs
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
ms.openlocfilehash: 2556b02459886390b803407c5cb828687229a44e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="b9b7b-103">Hantera Azure Batch-pooler med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b9b7b-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="b9b7b-104">Dessa skript visar några av verktyg som finns i Azure CLI för att skapa och hantera pooler för beräkningsnoder i Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-104">These script demonstrates some of the tools available in the Azure CLI to create and manage pools of compute nodes in the Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b7b-105">Kommandona i det här exemplet skapar virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-105">The commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="b9b7b-106">Virtuella datorer som körs kommer påförs kostnader till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-106">Running VMs will accrue charges to your account.</span></span> <span data-ttu-id="b9b7b-107">Ta bort de virtuella datorerna för att minimera dessa debiteringar, när du är klar kör exemplet.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-107">To minimize these charges, delete the VMs once you're done running the sample.</span></span> <span data-ttu-id="b9b7b-108">Se [Rensa pooler](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="b9b7b-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="b9b7b-109">Batch-pooler kan konfigureras på två sätt, antingen med Cloud Services-konfiguration (Windows) eller en virtuell dator-konfiguration (Windows och Linux).</span><span class="sxs-lookup"><span data-stu-id="b9b7b-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="b9b7b-110">Exempel på skript nedan visar hur du skapa pooler med båda konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-110">The sample scripts below show how to create pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9b7b-111">Krav</span><span class="sxs-lookup"><span data-stu-id="b9b7b-111">Prerequisites</span></span>

- <span data-ttu-id="b9b7b-112">Installera Azure CLI med hjälp av instruktionerna i den [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-112">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="b9b7b-113">Skapa ett Batch-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="b9b7b-114">Se [skapa ett batchkonto med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-114">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="b9b7b-115">Konfigurera ett program att köras från en start-aktivitet om du inte gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-115">Configure an application to run from a start task if you haven't yet done so.</span></span> <span data-ttu-id="b9b7b-116">Se [att lägga till program till Azure Batch med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) för ett exempelskript som skapar ett program och överför ett programpaket till Azure.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-116">See [Adding applications to Azure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package to Azure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="b9b7b-117">Pool med cloud service configuration exempelskript</span><span class="sxs-lookup"><span data-stu-id="b9b7b-117">Pool with cloud service configuration sample script</span></span>

<span data-ttu-id="b9b7b-118">[!code-azurecli[huvudsakliga](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "hantera pooler för Cloud Services")]</span><span class="sxs-lookup"><span data-stu-id="b9b7b-118">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]</span></span>

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="b9b7b-119">Pool med exempelskript konfigurationen för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b9b7b-119">Pool with virtual machine configuration sample script</span></span>

<span data-ttu-id="b9b7b-120">[!code-azurecli[huvudsakliga](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "hantera pooler för virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="b9b7b-120">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]</span></span>

## <a name="clean-up-pools"></a><span data-ttu-id="b9b7b-121">Rensa pooler</span><span class="sxs-lookup"><span data-stu-id="b9b7b-121">Clean up pools</span></span>

<span data-ttu-id="b9b7b-122">När du har kört ovan exempelskript kör du följande kommando för att ta bort pooler.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-122">After you run the above sample script, run the following command to delete the pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="b9b7b-123">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b9b7b-123">Script explanation</span></span>

<span data-ttu-id="b9b7b-124">Det här skriptet använder följande kommandon för att skapa och ändra Batch pooler.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-124">This script uses the following commands to create and manipulate Batch pools.</span></span>
<span data-ttu-id="b9b7b-125">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-125">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="b9b7b-126">Kommando</span><span class="sxs-lookup"><span data-stu-id="b9b7b-126">Command</span></span> | <span data-ttu-id="b9b7b-127">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b9b7b-127">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b9b7b-128">logga in AZ batch-konto</span><span class="sxs-lookup"><span data-stu-id="b9b7b-128">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="b9b7b-129">Autentisera mot en Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-129">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="b9b7b-130">Sammanfattning av AZ batch-program</span><span class="sxs-lookup"><span data-stu-id="b9b7b-130">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="b9b7b-131">Lista över tillgängliga program i Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-131">List the available applications in the Batch account.</span></span>  |
| [<span data-ttu-id="b9b7b-132">Skapa AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="b9b7b-132">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="b9b7b-133">Skapa en pool för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-133">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="b9b7b-134">AZ batch-pool uppsättningen</span><span class="sxs-lookup"><span data-stu-id="b9b7b-134">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="b9b7b-135">Uppdatera egenskaperna för en pool.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-135">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="b9b7b-136">listan för AZ batch pool nod-agent-SKU: er</span><span class="sxs-lookup"><span data-stu-id="b9b7b-136">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="b9b7b-137">Lista över tillgängliga noden agent SKU: er och bild information.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-137">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="b9b7b-138">Ändra storlek AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="b9b7b-138">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="b9b7b-139">Ändra storlek på hur många virtuella datorer som körs i den angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-139">Resize the number of running VMs in the specified pool.</span></span>  |
| [<span data-ttu-id="b9b7b-140">Visa för AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="b9b7b-140">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="b9b7b-141">Visa egenskaperna för en pool.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-141">Display the properties of a pool.</span></span>  |
| [<span data-ttu-id="b9b7b-142">ta bort AZ batch-pool</span><span class="sxs-lookup"><span data-stu-id="b9b7b-142">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="b9b7b-143">Ta bort den angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-143">Delete the specified pool.</span></span>  |
| [<span data-ttu-id="b9b7b-144">Aktivera AZ batch-pool Autoskala</span><span class="sxs-lookup"><span data-stu-id="b9b7b-144">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="b9b7b-145">Aktivera automatisk skalning på poolen och tillämpa en formel.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-145">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="b9b7b-146">inaktivera AZ batch-pool Autoskala</span><span class="sxs-lookup"><span data-stu-id="b9b7b-146">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="b9b7b-147">Inaktivera automatisk skalning på poolen.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-147">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="b9b7b-148">AZ batch nodlistan</span><span class="sxs-lookup"><span data-stu-id="b9b7b-148">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="b9b7b-149">Lista över alla compute-nod i den angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-149">List all the compute node in the specified pool.</span></span>  |
| [<span data-ttu-id="b9b7b-150">omstart av AZ batch nod</span><span class="sxs-lookup"><span data-stu-id="b9b7b-150">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="b9b7b-151">Starta om den angivna compute-nod.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-151">Reboot the specified compute node.</span></span>  |
| [<span data-ttu-id="b9b7b-152">ta bort AZ batch-nod</span><span class="sxs-lookup"><span data-stu-id="b9b7b-152">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="b9b7b-153">Ta bort de angivna noderna från den angivna poolen.</span><span class="sxs-lookup"><span data-stu-id="b9b7b-153">Delete the listed nodes from the specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="b9b7b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9b7b-154">Next steps</span></span>

<span data-ttu-id="b9b7b-155">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b9b7b-155">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b9b7b-156">Ytterligare Batch CLI skriptexempel finns i den [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b9b7b-156">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

