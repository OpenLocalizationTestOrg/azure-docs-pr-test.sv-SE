---
title: "aaaAzure CLI skriptexempel - köra ett jobb med Batch | Microsoft Docs"
description: "Azure CLI-skriptexempel - köra ett jobb med Batch"
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
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a><span data-ttu-id="d9591-103">Jobb som körs på Azure Batch med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d9591-103">Running jobs on Azure Batch with Azure CLI</span></span>

<span data-ttu-id="d9591-104">Det här skriptet skapar ett batchjobb och lägger till en serie uppgifter toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="d9591-104">This script creates a Batch job and adds a series of tasks toohello job.</span></span> <span data-ttu-id="d9591-105">Den visar även hur toomonitor ett jobb och dess uppgifter.</span><span class="sxs-lookup"><span data-stu-id="d9591-105">It also demonstrates how toomonitor a job and its tasks.</span></span> <span data-ttu-id="d9591-106">Slutligen visar hur tooquery hello Batch-tjänsten för information om hello jobbets uppgifter effektivt.</span><span class="sxs-lookup"><span data-stu-id="d9591-106">Finally, it shows how tooquery hello Batch service efficiently for information about hello job's tasks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9591-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d9591-107">Prerequisites</span></span>

- <span data-ttu-id="d9591-108">Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="d9591-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="d9591-109">Skapa ett Batch-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="d9591-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="d9591-110">Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="d9591-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="d9591-111">Konfigurera ett program toorun från en start-aktivitet om du inte gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="d9591-111">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="d9591-112">Se [att lägga till program tooAzure Batch med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) för ett exempelskript som skapar ett program och överför en programmet paketet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d9591-112">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>
- <span data-ttu-id="d9591-113">Konfigurera en pool som hello jobbet ska köras.</span><span class="sxs-lookup"><span data-stu-id="d9591-113">Configure a pool on which hello job will run.</span></span> <span data-ttu-id="d9591-114">Se [hantera Azure Batch-pooler med Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) för ett exempelskript som skapar en pool med en tjänstkonfiguration moln eller en konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d9591-114">See [Managing Azure Batch pools with Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) for a sample script that creates a pool with either a Cloud Service Configuration or a Virtual Machine Configuration.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d9591-115">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d9591-115">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a><span data-ttu-id="d9591-116">Rensa jobb</span><span class="sxs-lookup"><span data-stu-id="d9591-116">Clean up job</span></span>

<span data-ttu-id="d9591-117">När du har kört hello ovan exempelskript kör följande kommando tooremove hello jobbet och alla aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="d9591-117">After you run hello above sample script, run hello following command tooremove the job and all of its tasks.</span></span> <span data-ttu-id="d9591-118">Observera att hello poolen måste toobe bort separat.</span><span class="sxs-lookup"><span data-stu-id="d9591-118">Note that hello pool will need toobe deleted separately.</span></span> <span data-ttu-id="d9591-119">Se [hantera Azure Batch-pooler med Azure CLI](./batch-cli-sample-manage-pool.md) för mer information om hur du skapar och tar bort pooler.</span><span class="sxs-lookup"><span data-stu-id="d9591-119">See [Managing Azure Batch pools with Azure CLI](./batch-cli-sample-manage-pool.md) for more information on creating and deleting pools.</span></span>

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a><span data-ttu-id="d9591-120">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d9591-120">Script explanation</span></span>

<span data-ttu-id="d9591-121">Det här skriptet använder följande kommandon toocreate hello batchjobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="d9591-121">This script uses hello following commands toocreate a Batch job and tasks.</span></span> <span data-ttu-id="d9591-122">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d9591-122">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="d9591-123">Kommando</span><span class="sxs-lookup"><span data-stu-id="d9591-123">Command</span></span> | <span data-ttu-id="d9591-124">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d9591-124">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d9591-125">logga in AZ batch-konto</span><span class="sxs-lookup"><span data-stu-id="d9591-125">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="d9591-126">Autentisera mot en Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="d9591-126">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="d9591-127">Skapa AZ batchjobb</span><span class="sxs-lookup"><span data-stu-id="d9591-127">az batch job create</span></span>](https://docs.microsoft.com/cli/azure/batch/job#create) | <span data-ttu-id="d9591-128">Skapar ett batchjobb.</span><span class="sxs-lookup"><span data-stu-id="d9591-128">Creates a Batch job.</span></span>  |
| [<span data-ttu-id="d9591-129">AZ batch-jobbet uppsättningen</span><span class="sxs-lookup"><span data-stu-id="d9591-129">az batch job set</span></span>](https://docs.microsoft.com/cli/azure/batch/job#set) | <span data-ttu-id="d9591-130">Uppdaterar egenskaperna för ett batchjobb.</span><span class="sxs-lookup"><span data-stu-id="d9591-130">Updates properties of a Batch job.</span></span>  |
| [<span data-ttu-id="d9591-131">AZ batch-jobbet visar</span><span class="sxs-lookup"><span data-stu-id="d9591-131">az batch job show</span></span>](https://docs.microsoft.com/cli/azure/batch/job#show) | <span data-ttu-id="d9591-132">Hämtar information om batchjobb angivna.</span><span class="sxs-lookup"><span data-stu-id="d9591-132">Retrieves details of a specified Batch job.</span></span>  |
| [<span data-ttu-id="d9591-133">Skapa AZ batchaktiviteten</span><span class="sxs-lookup"><span data-stu-id="d9591-133">az batch task create</span></span>](https://docs.microsoft.com/cli/azure/batch/task#create) | <span data-ttu-id="d9591-134">Lägger till en aktivitet toohello angetts Batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="d9591-134">Adds a task toohello specified Batch job.</span></span>  |
| [<span data-ttu-id="d9591-135">AZ batch uppgiften visa</span><span class="sxs-lookup"><span data-stu-id="d9591-135">az batch task show</span></span>](https://docs.microsoft.com/cli/azure/batch/task#show) | <span data-ttu-id="d9591-136">Hämtar hello information om en aktivitet från hello angetts Batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="d9591-136">Retrieves hello details of a task from hello specified Batch job.</span></span>  |
| [<span data-ttu-id="d9591-137">uppgiftslista för AZ batch</span><span class="sxs-lookup"><span data-stu-id="d9591-137">az batch task list</span></span>](https://docs.microsoft.com/cli/azure/batch/task#list) | <span data-ttu-id="d9591-138">Visar hello uppgifter som är kopplad till hello angivna jobbet.</span><span class="sxs-lookup"><span data-stu-id="d9591-138">Lists hello tasks associated with hello specified job.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="d9591-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9591-139">Next steps</span></span>

<span data-ttu-id="d9591-140">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d9591-140">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d9591-141">Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d9591-141">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
