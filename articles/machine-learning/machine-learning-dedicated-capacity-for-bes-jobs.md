---
title: "aaaDedicated kapacitet för Machine Learning Batch Execution Service jobb | Microsoft Docs"
description: "Översikt över Azure Batch-tjänster för Machine Learning-jobb."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: bba7970bb31c50e5b0b9d5f4ff4f0d2dacf942e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="232ab-103">Azure Batch-tjänsten för Machine Learning-jobb</span><span class="sxs-lookup"><span data-stu-id="232ab-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="232ab-104">Machine Learning Batch-Pool bearbetning innehåller kundhanterad skala för hello Azure Machine Learning Batch Execution Service.</span><span class="sxs-lookup"><span data-stu-id="232ab-104">Machine Learning Batch Pool processing provides customer-managed scale for hello Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="232ab-105">Klassiska batchbearbetning för machine learning sker i en miljö med flera innehavare, vilka gränser hello antal samtidiga jobb som du kan skicka och jobb i kö på grundval av first i first out.</span><span class="sxs-lookup"><span data-stu-id="232ab-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits hello number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="232ab-106">Den här osäkerhet innebär att du inte kan förutsäga när jobbet ska köras.</span><span class="sxs-lookup"><span data-stu-id="232ab-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="232ab-107">Poolen batchbearbetning kan toocreate pooler som du kan skicka batchjobb.</span><span class="sxs-lookup"><span data-stu-id="232ab-107">Batch Pool processing allows you toocreate pools on which you can submit batch jobs.</span></span> <span data-ttu-id="232ab-108">Du styr hello storleken på hello poolen och toowhich pool hello jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="232ab-108">You control hello size of hello pool, and toowhich pool hello job is submitted.</span></span> <span data-ttu-id="232ab-109">BES-jobb körs i sin egen bearbetning utrymme att tillhandahålla förutsägbar bearbetning och hello möjlighet toocreate med resurspooler som motsvarar toohello belastningen som du skickar.</span><span class="sxs-lookup"><span data-stu-id="232ab-109">Your BES job runs in its own processing space providing predictable processing performance and hello ability toocreate resource pools that correspond toohello processing load that you submit.</span></span>

## <a name="how-toouse-batch-pool-processing"></a><span data-ttu-id="232ab-110">Hur toouse Batch-Pool bearbetning</span><span class="sxs-lookup"><span data-stu-id="232ab-110">How toouse Batch Pool processing</span></span>

<span data-ttu-id="232ab-111">Konfigurationen av gruppbearbetning poolen är inte tillgänglig via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ab-111">Configuration of Batch Pool Processing is not currently available through hello Azure portal.</span></span> <span data-ttu-id="232ab-112">toouse Batch-Pool bearbetning, måste du:</span><span class="sxs-lookup"><span data-stu-id="232ab-112">toouse Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="232ab-113">Anropa CSS toocreate en Pool Batch-kontot och få en Pool tjänst-URL och auktoriseringsnyckel</span><span class="sxs-lookup"><span data-stu-id="232ab-113">Call CSS toocreate a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="232ab-114">Skapa en ny Resource Manager-baserat webbtjänsten och faktureringsavtal</span><span class="sxs-lookup"><span data-stu-id="232ab-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="232ab-115">toocreate ditt konto Ring Microsofts kundservice och Support (CSS) och ange ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="232ab-115">toocreate your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="232ab-116">CSS fungerar med du toodetermine hello lämplig kapacitet för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="232ab-116">CSS will work with you toodetermine hello appropriate capacity for your scenario.</span></span> <span data-ttu-id="232ab-117">CSS konfigurerar ditt konto med hello maximalt antal pooler kan du skapa och hello maximalt antal virtuella datorer (VM) som du kan placera i varje pool.</span><span class="sxs-lookup"><span data-stu-id="232ab-117">CSS then configures your account with hello maximum number of pools you can create and hello maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="232ab-118">När du har konfigurerat ditt konto finns din Pool URL: en och auktoriseringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="232ab-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="232ab-119">När ditt konto har skapats kan använda du hello poolen URL: en och auktorisering viktiga tooperform pool hanteringsåtgärder på Batch-Pool.</span><span class="sxs-lookup"><span data-stu-id="232ab-119">After your account is created, you use hello Pool Service URL and authorization key tooperform pool management operations on your Batch Pool.</span></span>

![Arkitektur för batch-pool.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="232ab-121">Du kan skapa pooler genom att anropa hello skapa poolen igen på hello poolen tjänst-URL som angetts CSS-tooyou.</span><span class="sxs-lookup"><span data-stu-id="232ab-121">You create pools by calling hello Create Pool operation on hello pool service URL that CSS provided tooyou.</span></span> <span data-ttu-id="232ab-122">När du skapar en pool kan du ange hello antalet virtuella datorer och hello URL för hello swagger.json av en ny Resource Manager baserat Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="232ab-122">When you create a pool, specify hello number of VMs and hello URL of hello swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="232ab-123">Den här webbtjänsten tillhandahålls tooestablish hello fakturering association.</span><span class="sxs-lookup"><span data-stu-id="232ab-123">This web service is provided tooestablish hello billing association.</span></span> <span data-ttu-id="232ab-124">hello Batch-Pool-tjänsten använder hello swagger.json tooassociate hello pool med ett faktureringsavtal.</span><span class="sxs-lookup"><span data-stu-id="232ab-124">hello Batch Pool service uses hello swagger.json tooassociate hello pool with a billing plan.</span></span> <span data-ttu-id="232ab-125">Du kan köra alla BES webbtjänst, både nya Resource Manager-baserat och classic du väljer på hello poolen.</span><span class="sxs-lookup"><span data-stu-id="232ab-125">You can run any BES web service, both New Resource Manager based and classic, you choose on hello pool.</span></span>

<span data-ttu-id="232ab-126">Du kan använda alla nya Resource Manager-baserat webbtjänst, men tänk på att debiteras hello fakturering för hello jobb mot hello faktureringsplan som är associerade med den tjänsten.</span><span class="sxs-lookup"><span data-stu-id="232ab-126">You can use any New Resource Manager based web service, but be aware that hello billing for hello jobs are charged against hello billing plan associated with that service.</span></span> <span data-ttu-id="232ab-127">Du kanske vill toocreate som en webbtjänst och nya fakturering planera för Batch-Pool jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="232ab-127">You may want toocreate a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="232ab-128">Mer information om hur du skapar webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="232ab-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="232ab-129">När du har skapat en pool kan du skicka hello BES hello jobbet med Batch-begäranden med URL: en för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="232ab-129">Once you have created a pool, you submit hello BES job using hello Batch Requests URL for hello web service.</span></span> <span data-ttu-id="232ab-130">Du kan välja toosubmit den tooa pool eller tooclassic batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="232ab-130">You can choose toosubmit it tooa pool or tooclassic batch processing.</span></span> <span data-ttu-id="232ab-131">toosubmit en Pool bearbetningen av jobbet tooBatch lägga hello följande begärandetexten för parametern toohello jobbet skickas:</span><span class="sxs-lookup"><span data-stu-id="232ab-131">toosubmit a job tooBatch Pool processing, you add hello following parameter toohello job submission request body:</span></span>

<span data-ttu-id="232ab-132">”AzureBatchPoolId” ”:&lt;poolen ID&gt;”</span><span class="sxs-lookup"><span data-stu-id="232ab-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="232ab-133">Om du inte lägga till parametern hello körs hello jobbet i hello klassiska batch processmiljö.</span><span class="sxs-lookup"><span data-stu-id="232ab-133">If you do not add hello parameter, hello job is run in hello classic batch process environment.</span></span> <span data-ttu-id="232ab-134">Om hello poolen har tillgängliga resurser, startar hello jobbet körs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="232ab-134">If hello pool has available resources, hello job starts running immediately.</span></span> <span data-ttu-id="232ab-135">Om hello poolen saknar lediga resurser i ditt jobb kö tills en resurs som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="232ab-135">If hello pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="232ab-136">Om du upptäcker att du regelbundet når hello kapaciteten för din pooler, och du behöver bättre kapacitet kan du arbeta med ett representativt tooincrease kvoterna anropa CSS.</span><span class="sxs-lookup"><span data-stu-id="232ab-136">If you find that you regularly reach hello capacity of your pools, and you need increased capacity, you can call CSS and work with a representative tooincrease your quotas.</span></span>

<span data-ttu-id="232ab-137">Exempelbegäran:</span><span class="sxs-lookup"><span data-stu-id="232ab-137">Example Request:</span></span>

<span data-ttu-id="232ab-138">https://ussouthcentral.Services.azureml.NET/subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/jobs?API-version=2.0</span><span class="sxs-lookup"><span data-stu-id="232ab-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="232ab-139">Information om bearbetningen av Batch-Pool</span><span class="sxs-lookup"><span data-stu-id="232ab-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="232ab-140">Gruppbearbetning poolen är en alltid i fakturerbar tjänst och som kräver tooassociate den med Resource Manager baserad faktureringsavtal.</span><span class="sxs-lookup"><span data-stu-id="232ab-140">Batch Pool Processing is an always-on billable service and that requires you tooassociate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="232ab-141">Endast debiteras du för hello antal beräkningstimmar hello poolen körs på. oavsett hello antal jobb som körs under tiden poolen.</span><span class="sxs-lookup"><span data-stu-id="232ab-141">You are only billed for hello number of compute hours hello pool is running; regardless of hello number of jobs run during that time pool.</span></span> <span data-ttu-id="232ab-142">Om du skapar en pool, debiteras du för hello beräkningstimmar för varje virtuell dator i hello poolen tills hello poolen tas bort även om inga batchjobb körs i hello pool.</span><span class="sxs-lookup"><span data-stu-id="232ab-142">If you create a pool, you are billed for hello compute hours of each virtual machine in hello pool until hello pool is deleted, even if no batch jobs are running in hello pool.</span></span> <span data-ttu-id="232ab-143">Fakturering för hello virtuella datorer startar när etableringen är klar och stoppas när de har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="232ab-143">Billing for hello virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="232ab-144">Du kan använda någon av hello-planer finns på hello [Machine Learning-priser sidan](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="232ab-144">You can use any of hello plans found on hello [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="232ab-145">Fakturering exempel:</span><span class="sxs-lookup"><span data-stu-id="232ab-145">Billing example:</span></span>

<span data-ttu-id="232ab-146">Om du skapar en Batch-Pool med 2 virtuella datorer och ta bort den efter 24 timmar debiteras faktureringsavtalet 48 beräkningstimmar; oavsett hur många jobb kördes under denna period.</span><span class="sxs-lookup"><span data-stu-id="232ab-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="232ab-147">Om du skapar en Batch-Pool med 4 virtuella datorer och ta bort den efter 12 timmar, är också faktureringsavtalet Debiterat 48 beräkningstimmar.</span><span class="sxs-lookup"><span data-stu-id="232ab-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="232ab-148">Vi rekommenderar att avsöka hello jobbet status toodetermine när jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="232ab-148">We recommend that you poll hello job status toodetermine when jobs complete.</span></span> <span data-ttu-id="232ab-149">Anropa hello ändra storlek på poolen igen tooset hello antalet virtuella datorer i hello poolen toozero när dina jobb har körts.</span><span class="sxs-lookup"><span data-stu-id="232ab-149">When all your jobs have finished running, call hello Resize Pool operation tooset hello number of virtual machines in hello pool toozero.</span></span> <span data-ttu-id="232ab-150">Om du har ont om resurser och du behöver toocreate en ny pool, till exempel toobill mot en annan faktureringsplan, du kan ta bort hello poolen i stället när dina jobb har körts.</span><span class="sxs-lookup"><span data-stu-id="232ab-150">If you are short on pool resources and you need toocreate a new pool, for example toobill against a different billing plan, you can delete hello pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="232ab-151">**Använd bearbetning när Batch-Pool**</span><span class="sxs-lookup"><span data-stu-id="232ab-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="232ab-152">**Klassiska batchbearbetning när**</span><span class="sxs-lookup"><span data-stu-id="232ab-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="232ab-153">Du behöver toorun ett stort antal jobb</span><span class="sxs-lookup"><span data-stu-id="232ab-153">You need toorun a large number of jobs</span></span><br><span data-ttu-id="232ab-154">Eller</span><span class="sxs-lookup"><span data-stu-id="232ab-154">Or</span></span><br/><span data-ttu-id="232ab-155">Du behöver tooknow som dina jobb ska köras omedelbart</span><span class="sxs-lookup"><span data-stu-id="232ab-155">You need tooknow that your jobs will run immediately</span></span><br/><span data-ttu-id="232ab-156">Eller</span><span class="sxs-lookup"><span data-stu-id="232ab-156">Or</span></span><br/><span data-ttu-id="232ab-157">Du måste garanterad genomflöde.</span><span class="sxs-lookup"><span data-stu-id="232ab-157">You need guaranteed throughput.</span></span> <span data-ttu-id="232ab-158">Till exempel du behöver toorun ett antal jobb i en angiven tidsperiod och vill tooscale ut din beräkning resurser toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="232ab-158">For example, you need toorun a number of jobs in a given time frame, and want tooscale out your compute resources toomeet your needs.</span></span>    | <span data-ttu-id="232ab-159">Du kör några jobb</span><span class="sxs-lookup"><span data-stu-id="232ab-159">You are running just a few jobs</span></span><br/><span data-ttu-id="232ab-160">Och</span><span class="sxs-lookup"><span data-stu-id="232ab-160">And</span></span><br/> <span data-ttu-id="232ab-161">Du behöver inte hello jobb toorun omedelbart</span><span class="sxs-lookup"><span data-stu-id="232ab-161">You don’t need hello jobs toorun immediately</span></span> |
