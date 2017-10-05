---
title: "Hur du konfigurerar data matar ut för Stream Analytics-jobb | Microsoft Docs"
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
ms.openlocfilehash: 1ffa517469da1a8d79917b9747abc97ca3bef463
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a><span data-ttu-id="3799c-104">Hur du konfigurerar data matar ut för Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3799c-104">How to configure data outputs for Stream Analytics jobs</span></span>

<span data-ttu-id="3799c-105">Azure Stream Analytics-jobb kan vara ansluten till en eller flera utdata som definierar en anslutning till en befintlig data sink.</span><span class="sxs-lookup"><span data-stu-id="3799c-105">Azure Stream Analytics jobs can be connected to one or more data outputs, which define a connection to an existing data sink.</span></span> <span data-ttu-id="3799c-106">Stream Analytics-jobbet, processer och omvandlar inkommande data som skrivs en dataström med data utdata händelser till ditt jobb utdata.</span><span class="sxs-lookup"><span data-stu-id="3799c-106">As your Stream Analytics job processes and transforms incoming data, a stream of data output events is written to your job's output.</span></span>

<span data-ttu-id="3799c-107">Stream Analytics-data utdata kan användas som källa för realtids-instrumentpaneler eller aviseringar, utlöser arbetsflöden för flytt av data eller bara arkiverar data för batch-bearbetning vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="3799c-107">Stream Analytics data outputs can be used to source real-time dashboards or alerts, trigger data movement workflows, or simply archive data for batch processing later on.</span></span> <span data-ttu-id="3799c-108">Stream Analytics har förstklassigt integrering med flera Azure-tjänster som dokumenteras här.</span><span class="sxs-lookup"><span data-stu-id="3799c-108">Stream Analytics has first class integration with several Azure services, which are documented in detail here.</span></span>

<span data-ttu-id="3799c-109">Lägga till utdata till Stream Analytics-jobb:</span><span class="sxs-lookup"><span data-stu-id="3799c-109">To add an output to your Stream Analytics job:</span></span>

1. <span data-ttu-id="3799c-110">I den [Azure-portalen](https://portal.azure.com), öppna ditt jobb och klickar på **utdata** och klicka sedan på **Lägg till** i bladet utdata som visas.</span><span class="sxs-lookup"><span data-stu-id="3799c-110">In the [Azure portal](https://portal.azure.com), open your job and click **Outputs** and then click **Add** in the Outputs blade that appears.</span></span>
   
    ![Lägga till utdata](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. <span data-ttu-id="3799c-112">Ange ett eget namn för den här utdata i den **kolumnalias** rutan.</span><span class="sxs-lookup"><span data-stu-id="3799c-112">Provide a friendly name for this output in the **Output Alias** box.</span></span> <span data-ttu-id="3799c-113">Det här namnet kan användas i ditt jobb fråga vid ett senare tillfälle för att referera till utdata.</span><span class="sxs-lookup"><span data-stu-id="3799c-113">This name can be used in your job's query later on to refer to the output.</span></span>  
   
    <span data-ttu-id="3799c-114">Fyll i resten av egenskaperna behövs för att ansluta till dina utdata.</span><span class="sxs-lookup"><span data-stu-id="3799c-114">Fill in the rest of the required connection properties to connect to your output.</span></span>  <span data-ttu-id="3799c-115">De här fälten varierar beroende på utdatatypen och definieras här.</span><span class="sxs-lookup"><span data-stu-id="3799c-115">These fields vary by output type and are defined in detail here.</span></span>  
   
    ![Välj typ av data movement](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. <span data-ttu-id="3799c-117">Beroende på vilken Utdatatyp, kan du behöva ange hur data serialiseras eller formaterad.</span><span class="sxs-lookup"><span data-stu-id="3799c-117">Depending on the output type, you may need to specify how the data is serialized or formatted.</span></span> <span data-ttu-id="3799c-118">Här beskrivs specifika serialisering inställningarna för varje Utdatatyp av.</span><span class="sxs-lookup"><span data-stu-id="3799c-118">The specific serialization settings for each output type are documented here.</span></span>
   
    <span data-ttu-id="3799c-119">Fyll i resten av egenskaperna behövs för att ansluta till datakällan.</span><span class="sxs-lookup"><span data-stu-id="3799c-119">Fill in the rest of the required connection properties to connect to your data source.</span></span> <span data-ttu-id="3799c-120">De här fälten varierar beroende på typ av indata- och och definieras i detalj i den [skapa jobbet artikel](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="3799c-120">These fields vary by type of input and source type and are defined in detail in the [Create Job article](stream-analytics-create-a-job.md).</span></span>  

> [!Note]
>
> <span data-ttu-id="3799c-121">En output-elementet som läggs till projektet, måste finnas innan jobbet har startats och händelser börjar flöda.</span><span class="sxs-lookup"><span data-stu-id="3799c-121">Any output element added to the job, must exist before the job is started and events start flowing.</span></span> <span data-ttu-id="3799c-122">Till exempel om du använder Blob storage som utdata jobbet kommer inte att skapa ett lagringskonto automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3799c-122">For example, if you use Blob storage as an output, the job will not create a storage account automatically.</span></span> <span data-ttu-id="3799c-123">Den måste skapas av användaren innan ASA jobbet har startats.</span><span class="sxs-lookup"><span data-stu-id="3799c-123">It needs to be created by the user before the ASA job is started.</span></span>
> 
 

## <a name="get-help"></a><span data-ttu-id="3799c-124">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="3799c-124">Get help</span></span>
<span data-ttu-id="3799c-125">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="3799c-125">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3799c-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3799c-126">Next steps</span></span>
* [<span data-ttu-id="3799c-127">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3799c-127">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="3799c-128">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3799c-128">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="3799c-129">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3799c-129">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="3799c-130">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="3799c-130">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="3799c-131">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="3799c-131">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

