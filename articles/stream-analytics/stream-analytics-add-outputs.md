---
title: "aaaHow tooconfigure data matar ut för Stream Analytics-jobb | Microsoft Docs"
description: "Konfigurera utdata för Stream Analytics-jobb | Learning sökvägssegment."
keywords: data som utdata, dataflyttning
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a><span data-ttu-id="55382-104">Hur tooconfigure data matar ut för Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="55382-104">How tooconfigure data outputs for Stream Analytics jobs</span></span>

<span data-ttu-id="55382-105">Azure Stream Analytics-jobb kan vara anslutna tooone eller mer utdata som definierar en anslutning tooan befintliga data mottagare.</span><span class="sxs-lookup"><span data-stu-id="55382-105">Azure Stream Analytics jobs can be connected tooone or more data outputs, which define a connection tooan existing data sink.</span></span> <span data-ttu-id="55382-106">Stream Analytics-jobbet, processer och omvandlar inkommande data som skrivs en dataström med data utdata händelser tooyour jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="55382-106">As your Stream Analytics job processes and transforms incoming data, a stream of data output events is written tooyour job's output.</span></span>

<span data-ttu-id="55382-107">Utdata för Stream Analytics kan vara används toosource realtids-instrumentpaneler och aviseringar, utlösare data movement arbetsflöden eller helt enkelt Arkivera data för batch-bearbetning vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="55382-107">Stream Analytics data outputs can be used toosource real-time dashboards or alerts, trigger data movement workflows, or simply archive data for batch processing later on.</span></span> <span data-ttu-id="55382-108">Stream Analytics har förstklassigt integrering med flera Azure-tjänster som dokumenteras här.</span><span class="sxs-lookup"><span data-stu-id="55382-108">Stream Analytics has first class integration with several Azure services, which are documented in detail here.</span></span>

<span data-ttu-id="55382-109">tooadd utdata tooyour Stream Analytics-jobbet:</span><span class="sxs-lookup"><span data-stu-id="55382-109">tooadd an output tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="55382-110">I hello [Azure-portalen](https://portal.azure.com), öppna ditt jobb och klickar på **utdata** och klicka sedan på **Lägg till** i hello utdata bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="55382-110">In hello [Azure portal](https://portal.azure.com), open your job and click **Outputs** and then click **Add** in hello Outputs blade that appears.</span></span>
   
    ![Lägga till utdata](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. <span data-ttu-id="55382-112">Ange ett eget namn för den här utdata i hello **kolumnalias** rutan.</span><span class="sxs-lookup"><span data-stu-id="55382-112">Provide a friendly name for this output in hello **Output Alias** box.</span></span> <span data-ttu-id="55382-113">Det här namnet kan användas i ditt jobb fråga senare på toorefer toohello utdata.</span><span class="sxs-lookup"><span data-stu-id="55382-113">This name can be used in your job's query later on toorefer toohello output.</span></span>  
   
    <span data-ttu-id="55382-114">Fyll i hello resten av hello krävs anslutning egenskaper tooconnect tooyour utdata.</span><span class="sxs-lookup"><span data-stu-id="55382-114">Fill in hello rest of hello required connection properties tooconnect tooyour output.</span></span>  <span data-ttu-id="55382-115">De här fälten varierar beroende på utdatatypen och definieras här.</span><span class="sxs-lookup"><span data-stu-id="55382-115">These fields vary by output type and are defined in detail here.</span></span>  
   
    ![Välj typ av data movement](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. <span data-ttu-id="55382-117">Hello utdatatypen måste du använda toospecify hur hello data serialiseras eller formaterad.</span><span class="sxs-lookup"><span data-stu-id="55382-117">Depending on hello output type, you may need toospecify how hello data is serialized or formatted.</span></span> <span data-ttu-id="55382-118">Här beskrivs hello specifika serialisering inställningar för varje Utdatatyp av.</span><span class="sxs-lookup"><span data-stu-id="55382-118">hello specific serialization settings for each output type are documented here.</span></span>
   
    <span data-ttu-id="55382-119">Fyll i hello resten av hello behövs egenskaper tooconnect tooyour datakälla.</span><span class="sxs-lookup"><span data-stu-id="55382-119">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="55382-120">De här fälten varierar beroende på typ av indata- och och definieras i detalj i hello [skapa jobbet artikel](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="55382-120">These fields vary by type of input and source type and are defined in detail in hello [Create Job article](stream-analytics-create-a-job.md).</span></span>  

> [!Note]
>
> <span data-ttu-id="55382-121">Output-elementet tillagda toohello jobb, måste finnas innan hello jobbet har startats och händelser börjar flöda.</span><span class="sxs-lookup"><span data-stu-id="55382-121">Any output element added toohello job, must exist before hello job is started and events start flowing.</span></span> <span data-ttu-id="55382-122">Till exempel om du använder Blob storage som utdata hello jobbet kommer inte att skapa ett lagringskonto automatiskt.</span><span class="sxs-lookup"><span data-stu-id="55382-122">For example, if you use Blob storage as an output, hello job will not create a storage account automatically.</span></span> <span data-ttu-id="55382-123">Den måste toobe som skapats av hello användare innan hello ASA jobb har startat.</span><span class="sxs-lookup"><span data-stu-id="55382-123">It needs toobe created by hello user before hello ASA job is started.</span></span>
> 
 

## <a name="get-help"></a><span data-ttu-id="55382-124">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="55382-124">Get help</span></span>
<span data-ttu-id="55382-125">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="55382-125">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="55382-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55382-126">Next steps</span></span>
* [<span data-ttu-id="55382-127">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="55382-127">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="55382-128">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="55382-128">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="55382-129">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="55382-129">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="55382-130">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="55382-130">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="55382-131">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="55382-131">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

