---
title: "Dedikerade kapacitet för Machine Learning Batch Execution Service jobb | Microsoft Docs"
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
ms.openlocfilehash: 3879eb3d0c6fa9d74fff01b07f5c07d3991dfbbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="de80f-103">Azure Batch-tjänsten för Machine Learning-jobb</span><span class="sxs-lookup"><span data-stu-id="de80f-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="de80f-104">Machine Learning Batch-Pool bearbetning innehåller kundhanterad skala för Azure Machine Learning Batch Execution Service.</span><span class="sxs-lookup"><span data-stu-id="de80f-104">Machine Learning Batch Pool processing provides customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="de80f-105">Klassiska gruppbearbetning för maskininlärning äger rum i en miljö med flera innehavare, vilket begränsar antalet samtidiga jobb som du kan skicka och jobb ställs i kö på grundval av first i first out.</span><span class="sxs-lookup"><span data-stu-id="de80f-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits the number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="de80f-106">Den här osäkerhet innebär att du inte kan förutsäga när jobbet ska köras.</span><span class="sxs-lookup"><span data-stu-id="de80f-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="de80f-107">Poolen batchbearbetning kan du skapa pooler som du kan skicka batchjobb.</span><span class="sxs-lookup"><span data-stu-id="de80f-107">Batch Pool processing allows you to create pools on which you can submit batch jobs.</span></span> <span data-ttu-id="de80f-108">Du styr storleken på poolen och vilka poolen jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="de80f-108">You control the size of the pool, and to which pool the job is submitted.</span></span> <span data-ttu-id="de80f-109">BES-jobb körs i sin egen bearbetning utrymme som ger förutsägbar bearbetning och möjligheten att skapa resurspooler som motsvarar belastningen som du skickar.</span><span class="sxs-lookup"><span data-stu-id="de80f-109">Your BES job runs in its own processing space providing predictable processing performance and the ability to create resource pools that correspond to the processing load that you submit.</span></span>

## <a name="how-to-use-batch-pool-processing"></a><span data-ttu-id="de80f-110">Hur du använder behandling av Batch-Pool</span><span class="sxs-lookup"><span data-stu-id="de80f-110">How to use Batch Pool processing</span></span>

<span data-ttu-id="de80f-111">Konfigurationen av gruppbearbetning poolen är inte tillgänglig via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de80f-111">Configuration of Batch Pool Processing is not currently available through the Azure portal.</span></span> <span data-ttu-id="de80f-112">Om du vill använda Batch-Pool bearbetning, måste du:</span><span class="sxs-lookup"><span data-stu-id="de80f-112">To use Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="de80f-113">Anropa CSS om du vill skapa en Pool med Batch-kontot och få en Pool tjänst-URL och auktoriseringsnyckel</span><span class="sxs-lookup"><span data-stu-id="de80f-113">Call CSS to create a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="de80f-114">Skapa en ny Resource Manager-baserat webbtjänsten och faktureringsavtal</span><span class="sxs-lookup"><span data-stu-id="de80f-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="de80f-115">Ring Microsofts kundservice och Support (CSS) för att skapa ditt konto, och ange ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="de80f-115">To create your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="de80f-116">CSS fungerar med dig för att avgöra lämpliga kapacitet för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="de80f-116">CSS will work with you to determine the appropriate capacity for your scenario.</span></span> <span data-ttu-id="de80f-117">CSS konfigurerar ditt konto med det maximala antalet pooler som du kan skapa och högsta antalet virtuella datorer (VM) som du kan placera i varje pool.</span><span class="sxs-lookup"><span data-stu-id="de80f-117">CSS then configures your account with the maximum number of pools you can create and the maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="de80f-118">När du har konfigurerat ditt konto finns din Pool URL: en och auktoriseringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="de80f-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="de80f-119">När ditt konto har skapats kan använda du nyckeln poolen URL: en och auktorisering för att utföra poolen hanteringsåtgärder på Batch-Pool.</span><span class="sxs-lookup"><span data-stu-id="de80f-119">After your account is created, you use the Pool Service URL and authorization key to perform pool management operations on your Batch Pool.</span></span>

![Arkitektur för batch-pool.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="de80f-121">Du kan skapa pooler genom att anropa åtgärden Skapa poolen på poolen tjänstens Webbadress CSS som du har fått.</span><span class="sxs-lookup"><span data-stu-id="de80f-121">You create pools by calling the Create Pool operation on the pool service URL that CSS provided to you.</span></span> <span data-ttu-id="de80f-122">När du skapar en pool, kan du ange hur många virtuella datorer och URL-Adressen för swagger.json av en ny Resource Manager baserat Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="de80f-122">When you create a pool, specify the number of VMs and the URL of the swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="de80f-123">Den här webbtjänsten har angetts för att upprätta fakturering association.</span><span class="sxs-lookup"><span data-stu-id="de80f-123">This web service is provided to establish the billing association.</span></span> <span data-ttu-id="de80f-124">Swagger.json använder tjänsten Batch-Pool för att associera poolen med ett faktureringsavtal.</span><span class="sxs-lookup"><span data-stu-id="de80f-124">The Batch Pool service uses the swagger.json to associate the pool with a billing plan.</span></span> <span data-ttu-id="de80f-125">Du kan köra alla BES webbtjänst, både nya Resource Manager-baserat och classic du väljer på poolen.</span><span class="sxs-lookup"><span data-stu-id="de80f-125">You can run any BES web service, both New Resource Manager based and classic, you choose on the pool.</span></span>

<span data-ttu-id="de80f-126">Du kan använda alla nya Resource Manager-baserat webbtjänst, men tänk på att faktureringen för jobben debiteras mot den faktureringsplan som är associerade med den tjänsten.</span><span class="sxs-lookup"><span data-stu-id="de80f-126">You can use any New Resource Manager based web service, but be aware that the billing for the jobs are charged against the billing plan associated with that service.</span></span> <span data-ttu-id="de80f-127">Du kanske vill skapa en webbtjänst och nya faktureringsavtal specifikt för Batch-Pool jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="de80f-127">You may want to create a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="de80f-128">Mer information om hur du skapar webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="de80f-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="de80f-129">När du har skapat en pool kan skicka du BES jobbet med Batch begäranden URL för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="de80f-129">Once you have created a pool, you submit the BES job using the Batch Requests URL for the web service.</span></span> <span data-ttu-id="de80f-130">Du kan välja att skicka den till en pool eller klassiska batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="de80f-130">You can choose to submit it to a pool or to classic batch processing.</span></span> <span data-ttu-id="de80f-131">Om du vill skicka ett jobb för att bearbeta Batch-Pool du lägga till följande parameter till jobbet skicka frågans brödtext:</span><span class="sxs-lookup"><span data-stu-id="de80f-131">To submit a job to Batch Pool processing, you add the following parameter to the job submission request body:</span></span>

<span data-ttu-id="de80f-132">”AzureBatchPoolId” ”:&lt;poolen ID&gt;”</span><span class="sxs-lookup"><span data-stu-id="de80f-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="de80f-133">Om du inte lägga till parametern körs jobbet i klassiskt batch processmiljö.</span><span class="sxs-lookup"><span data-stu-id="de80f-133">If you do not add the parameter, the job is run in the classic batch process environment.</span></span> <span data-ttu-id="de80f-134">Om poolen har tillgängliga resurser, startar jobbet körs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="de80f-134">If the pool has available resources, the job starts running immediately.</span></span> <span data-ttu-id="de80f-135">Om poolen saknar lediga resurser i ditt jobb kö tills en resurs som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="de80f-135">If the pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="de80f-136">Om du upptäcker att du regelbundet når kapaciteten för din pooler, och du behöver bättre kapacitet kan du anropa CSS och arbeta med en representant för att öka din kvoter.</span><span class="sxs-lookup"><span data-stu-id="de80f-136">If you find that you regularly reach the capacity of your pools, and you need increased capacity, you can call CSS and work with a representative to increase your quotas.</span></span>

<span data-ttu-id="de80f-137">Exempelbegäran:</span><span class="sxs-lookup"><span data-stu-id="de80f-137">Example Request:</span></span>

<span data-ttu-id="de80f-138">https://ussouthcentral.Services.azureml.NET/subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/jobs?API-version=2.0</span><span class="sxs-lookup"><span data-stu-id="de80f-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

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

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="de80f-139">Information om bearbetningen av Batch-Pool</span><span class="sxs-lookup"><span data-stu-id="de80f-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="de80f-140">Gruppbearbetning poolen är en alltid i fakturerbar tjänst och som kräver att du associerar den med en Resource Manager-baserat faktureringsavtal.</span><span class="sxs-lookup"><span data-stu-id="de80f-140">Batch Pool Processing is an always-on billable service and that requires you to associate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="de80f-141">Endast debiteras du för antal beräkningstimmar poolen körs på. oavsett vilket antal jobb som körs under tiden poolen.</span><span class="sxs-lookup"><span data-stu-id="de80f-141">You are only billed for the number of compute hours the pool is running; regardless of the number of jobs run during that time pool.</span></span> <span data-ttu-id="de80f-142">Om du skapar en pool, debiteras du för beräkningstimmar för varje virtuell dator i poolen tills poolen tas bort även om inga batchjobb körs i poolen.</span><span class="sxs-lookup"><span data-stu-id="de80f-142">If you create a pool, you are billed for the compute hours of each virtual machine in the pool until the pool is deleted, even if no batch jobs are running in the pool.</span></span> <span data-ttu-id="de80f-143">Fakturering för virtuella datorer startar när etableringen är klar och stoppas när de har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="de80f-143">Billing for the virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="de80f-144">Du kan använda någon av planer hittades på den [Machine Learning-priser sidan](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="de80f-144">You can use any of the plans found on the [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="de80f-145">Fakturering exempel:</span><span class="sxs-lookup"><span data-stu-id="de80f-145">Billing example:</span></span>

<span data-ttu-id="de80f-146">Om du skapar en Batch-Pool med 2 virtuella datorer och ta bort den efter 24 timmar debiteras faktureringsavtalet 48 beräkningstimmar; oavsett hur många jobb kördes under denna period.</span><span class="sxs-lookup"><span data-stu-id="de80f-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="de80f-147">Om du skapar en Batch-Pool med 4 virtuella datorer och ta bort den efter 12 timmar, är också faktureringsavtalet Debiterat 48 beräkningstimmar.</span><span class="sxs-lookup"><span data-stu-id="de80f-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="de80f-148">Vi rekommenderar att du avsöka jobbstatus för att avgöra när jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="de80f-148">We recommend that you poll the job status to determine when jobs complete.</span></span> <span data-ttu-id="de80f-149">När alla jobb har körts anropa ändra storlek på poolen igen för att ange antalet virtuella datorer i poolen med noll.</span><span class="sxs-lookup"><span data-stu-id="de80f-149">When all your jobs have finished running, call the Resize Pool operation to set the number of virtual machines in the pool to zero.</span></span> <span data-ttu-id="de80f-150">Om du har ont om resurser och du behöver skapa en ny pool, till exempel för att debiterar mot en annan faktureringsplan, du kan ta bort poolen i stället när dina jobb har körts.</span><span class="sxs-lookup"><span data-stu-id="de80f-150">If you are short on pool resources and you need to create a new pool, for example to bill against a different billing plan, you can delete the pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="de80f-151">**Använd bearbetning när Batch-Pool**</span><span class="sxs-lookup"><span data-stu-id="de80f-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="de80f-152">**Klassiska batchbearbetning när**</span><span class="sxs-lookup"><span data-stu-id="de80f-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="de80f-153">Du måste köra ett stort antal jobb</span><span class="sxs-lookup"><span data-stu-id="de80f-153">You need to run a large number of jobs</span></span><br><span data-ttu-id="de80f-154">Eller</span><span class="sxs-lookup"><span data-stu-id="de80f-154">Or</span></span><br/><span data-ttu-id="de80f-155">Du behöver veta att dina jobb ska köras omedelbart</span><span class="sxs-lookup"><span data-stu-id="de80f-155">You need to know that your jobs will run immediately</span></span><br/><span data-ttu-id="de80f-156">Eller</span><span class="sxs-lookup"><span data-stu-id="de80f-156">Or</span></span><br/><span data-ttu-id="de80f-157">Du måste garanterad genomflöde.</span><span class="sxs-lookup"><span data-stu-id="de80f-157">You need guaranteed throughput.</span></span> <span data-ttu-id="de80f-158">Du behöver exempelvis kör ett antal jobb i en angiven tidsperiod och vill skala upp dina beräkningsresurser som uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="de80f-158">For example, you need to run a number of jobs in a given time frame, and want to scale out your compute resources to meet your needs.</span></span>    | <span data-ttu-id="de80f-159">Du kör några jobb</span><span class="sxs-lookup"><span data-stu-id="de80f-159">You are running just a few jobs</span></span><br/><span data-ttu-id="de80f-160">Och</span><span class="sxs-lookup"><span data-stu-id="de80f-160">And</span></span><br/> <span data-ttu-id="de80f-161">Du behöver inte jobb ska köras omedelbart</span><span class="sxs-lookup"><span data-stu-id="de80f-161">You don’t need the jobs to run immediately</span></span> |
