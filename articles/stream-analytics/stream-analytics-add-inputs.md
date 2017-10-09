---
title: aaaAdd data indata tooyour Stream Analytics-jobb | Microsoft Docs
description: "Lär dig hur toohook upp en datakälla tooyour Stream Analytics jobbet som strömmande data i indata från Händelsehubbar eller referens data från blogg lagring."
keywords: "indata, strömmande data"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a><span data-ttu-id="8efde-104">Lägg till en strömmande data indata eller referens data tooa Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="8efde-104">Add a streaming data input or reference data tooa Stream Analytics job</span></span>
<span data-ttu-id="8efde-105">Lär dig hur toohook upp en datakälla tooyour Stream Analytics-jobbet som strömmande data i indata från Händelsehubbar eller referens data från Blob storage.</span><span class="sxs-lookup"><span data-stu-id="8efde-105">Learn how toohook up a data source tooyour Stream Analytics job as streaming data input from Event Hubs or reference data from Blob storage.</span></span>

<span data-ttu-id="8efde-106">Azure Stream Analytics-jobb kan vara anslutna tooone indata eller mer, som definierar en anslutning tooan befintlig datakälla.</span><span class="sxs-lookup"><span data-stu-id="8efde-106">Azure Stream Analytics jobs can be connected tooone data input or more, each of which define a connection tooan existing data source.</span></span> <span data-ttu-id="8efde-107">När data skickas toothat datakälla, är det används av hello Stream Analytics-jobbet och behandlas i realtid som strömmande data.</span><span class="sxs-lookup"><span data-stu-id="8efde-107">As data is sent toothat data source, it is consumed by hello Stream Analytics job and processed in real time as streaming data.</span></span> <span data-ttu-id="8efde-108">Stream Analytics har förstklassigt integrering med [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) och [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) både inom och utanför hello jobbet prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8efde-108">Stream Analytics has first class integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) both within and outside of hello job's subscription.</span></span>

<span data-ttu-id="8efde-109">Den här artikeln är ett steg i hello [Stream Analytics-Utbildningsväg](/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="8efde-109">This article is a step in hello [Stream Analytics learning path](/documentation/learning-paths/stream-analytics/).</span></span>

## <a name="data-input-streaming-data-and-reference-data"></a><span data-ttu-id="8efde-110">Indata: data och referensdata</span><span class="sxs-lookup"><span data-stu-id="8efde-110">Data input: Streaming data and reference data</span></span>
<span data-ttu-id="8efde-111">Det finns två olika typer av indata i Stream Analytics: dataströmmar och referensdata.</span><span class="sxs-lookup"><span data-stu-id="8efde-111">There are two distinct types of inputs in Stream Analytics: data streams and reference data.</span></span>

* <span data-ttu-id="8efde-112">**Dataströmmar**: Stream Analytics-jobb måste innehålla minst en data dataströmmen inkommande toobe konsumeras och transformeras av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="8efde-112">**Data Streams**: Stream Analytics jobs must include at least one data stream input toobe consumed and transformed by hello job.</span></span> <span data-ttu-id="8efde-113">Azure Blob storage och Azure Event Hubs kan användas som indata datakällor för dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="8efde-113">Azure Blob storage and Azure Event Hubs are supported as data stream input sources.</span></span> <span data-ttu-id="8efde-114">Händelsehubbar i Azure är används toocollect händelseströmmar från anslutna enheter, tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="8efde-114">Azure Event Hubs are used toocollect event streams from connected devices, services and applications.</span></span> <span data-ttu-id="8efde-115">Azure Blob storage kan användas som en Indatakällan för att föra in stora mängder data som en dataström.</span><span class="sxs-lookup"><span data-stu-id="8efde-115">Azure Blob storage can be used as an input source for ingesting bulk data as a stream.</span></span>  
* <span data-ttu-id="8efde-116">**Referensdata**: Stream Analytics stöder en andra typen av extra inkommande kallas referensdata.</span><span class="sxs-lookup"><span data-stu-id="8efde-116">**Reference data**: Stream Analytics supports a second type of auxiliary input called reference data.</span></span>  <span data-ttu-id="8efde-117">Dessa data är statiska eller sakta ändra som skillnad från toodata i rörelse.</span><span class="sxs-lookup"><span data-stu-id="8efde-117">As opposed toodata in motion, this data is static or slowing changing.</span></span>  <span data-ttu-id="8efde-118">Den används vanligtvis för att utföra look-ups och samband med dataströmmar toocreate en större mängd data.</span><span class="sxs-lookup"><span data-stu-id="8efde-118">It is typically used for performing look-ups and correlations with data streams toocreate a richer data set.</span></span>  <span data-ttu-id="8efde-119">Azure Blob storage är för närvarande stöds endast hello Indatakällan för referensdata.</span><span class="sxs-lookup"><span data-stu-id="8efde-119">Azure Blob storage is currently hello only supported input source for reference data.</span></span>  

<span data-ttu-id="8efde-120">tooadd ett inkommande tooyour Stream Analytics-jobb:</span><span class="sxs-lookup"><span data-stu-id="8efde-120">tooadd an input tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="8efde-121">I hello Azure-portalen klickar du på **indata** och klicka sedan på **lägga till indata** i Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="8efde-121">In hello Azure portal click **Inputs** and then click **Add an Input** in your Stream Analytics job.</span></span>
   
    ![Klassiska Azure-portalen – lägga till indata.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    <span data-ttu-id="8efde-123">I hello Azure-portalen klickar du på hello **indata** panelen i Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="8efde-123">In hello Azure portal click hello **Inputs** tile in your Stream Analytics job.</span></span>  
   
    ![Azure portal – lägga till indata.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. <span data-ttu-id="8efde-125">Ange hello hello indata: antingen **dataströmmen** eller **referensdata**.</span><span class="sxs-lookup"><span data-stu-id="8efde-125">Specify hello type of hello input: either **Data stream** or **Reference data**.</span></span>
   
    ![Lägga till hello korrekta data indata, strömmas eller refererar till](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Lägga till hello korrekta data indata, strömmas eller refererar till](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. <span data-ttu-id="8efde-128">Om du skapar en dataström inmatning, ange hello källtyp för hello indata.</span><span class="sxs-lookup"><span data-stu-id="8efde-128">If creating a Data Stream input, specify hello source type for hello input.</span></span>  <span data-ttu-id="8efde-129">Det här steget kan hoppas över när referensdata skapas som endast Blob-lagring stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="8efde-129">This step can be skipped during Reference Data creation as only Blob storage is supported at this time.</span></span>
   
    ![Lägg till inkommande dataström data](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Lägg till inkommande dataström data](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. <span data-ttu-id="8efde-132">Ange ett eget namn för den här inkommande i hello inmatat Alias rutan.</span><span class="sxs-lookup"><span data-stu-id="8efde-132">Provide a friendly name for this input in hello Input Alias box.</span></span>  <span data-ttu-id="8efde-133">Det här namnet används i jobbfråga senare på toorefer toohello indata.</span><span class="sxs-lookup"><span data-stu-id="8efde-133">This name will be used in your job's query later on toorefer toohello input.</span></span>
   
    <span data-ttu-id="8efde-134">Fyll i hello resten av hello behövs egenskaper tooconnect tooyour datakälla.</span><span class="sxs-lookup"><span data-stu-id="8efde-134">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="8efde-135">De här fälten varierar beroende på typ av indata- och och definieras i detalj [här](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="8efde-135">These fields vary by type of input and source type and are defined in detail [here](stream-analytics-create-a-job.md).</span></span>  
   
    ![Lägga till event hub data indata](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. <span data-ttu-id="8efde-137">Ange inställningar för hello-serialisering för hello indata:</span><span class="sxs-lookup"><span data-stu-id="8efde-137">Specify hello serialization settings for hello input data:</span></span>
   
   * <span data-ttu-id="8efde-138">toomake att dina frågor fungerar hello som du tänkt ange hello **händelse serialiseringsformat** för inkommande data.</span><span class="sxs-lookup"><span data-stu-id="8efde-138">toomake sure your queries work hello way you expect, specify hello **Event Serialization Format** of incoming data.</span></span>  <span data-ttu-id="8efde-139">Stöds serialisering format är JSON-, CSV- och Avro.</span><span class="sxs-lookup"><span data-stu-id="8efde-139">Supported serialization formats are JSON, CSV, and Avro.</span></span>
   * <span data-ttu-id="8efde-140">Kontrollera hello **kodning** för hello data.</span><span class="sxs-lookup"><span data-stu-id="8efde-140">Verify hello **Encoding** for hello data.</span></span>  <span data-ttu-id="8efde-141">UTF-8 är hello stöds endast kodningsformat just nu.</span><span class="sxs-lookup"><span data-stu-id="8efde-141">UTF-8 is hello only supported encoding format at this time.</span></span>
     
     ![Inställningarna för serialisering för hello indata](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Inställningarna för serialisering för hello indata](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. <span data-ttu-id="8efde-144">När du har slutfört hello inkommande skapa verifierar Stream Analytics att den kan ansluta toohello Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="8efde-144">After completing hello input creation, Stream Analytics will verify that it can connect toohello input source.</span></span>  <span data-ttu-id="8efde-145">Du kan visa hello status för hello Testa anslutning igen i hello Notification hub.</span><span class="sxs-lookup"><span data-stu-id="8efde-145">You can view hello status of hello Test Connection operation in hello Notification hub.</span></span>
   
    ![Testa anslutningen för hello strömning indata](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Testa anslutningen för hello strömning indata](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a><span data-ttu-id="8efde-148">Få hjälp med streaming indata</span><span class="sxs-lookup"><span data-stu-id="8efde-148">Get help with streaming data inputs</span></span>
<span data-ttu-id="8efde-149">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="8efde-149">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8efde-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8efde-150">Next steps</span></span>
* [<span data-ttu-id="8efde-151">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8efde-151">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8efde-152">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8efde-152">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8efde-153">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8efde-153">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8efde-154">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="8efde-154">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8efde-155">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="8efde-155">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

