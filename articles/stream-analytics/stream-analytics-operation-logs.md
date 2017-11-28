---
title: "aaaDebug med hjälp av åtgärden och service i Stream Analytics | Microsoft Docs"
description: "Hur toouse Stream Analytics åtgärdsloggar"
keywords: "tjänstloggar"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a><span data-ttu-id="9928b-104">Felsöka Stream Analytics-jobb med hjälp av loggar och drift</span><span class="sxs-lookup"><span data-stu-id="9928b-104">Debug Stream Analytics jobs using service and operation logs</span></span>
<span data-ttu-id="9928b-105">Alla Azure-tjänster ange operativa loggning meddelanden toousers toorecord uppgifter relaterade toomanagement åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9928b-105">All Azure services supply operational logging messages toousers toorecord details related toomanagement operations.</span></span> <span data-ttu-id="9928b-106">Den här informationen kan användas för felsökning, till exempel visa jobbstatus och jobbförloppet fel meddelanden tootrack hello förloppet för ett jobb med tiden från start tooprocessing toooutput i Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="9928b-106">In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages tootrack hello progress of a job over time, from start tooprocessing toooutput.</span></span>

## <a name="find-operation-logs-in-hello-azure-management-portal"></a><span data-ttu-id="9928b-107">Hitta åtgärden loggas i hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="9928b-107">Find operation logs in hello Azure Management portal</span></span>
<span data-ttu-id="9928b-108">Åtgärdsloggar kan nås på två sätt:</span><span class="sxs-lookup"><span data-stu-id="9928b-108">Operation Logs can be accessed in two ways:</span></span>  

* <span data-ttu-id="9928b-109">Instrumentpanelen i hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="9928b-109">Dashboard of hello Stream Analytics job</span></span>  
* <span data-ttu-id="9928b-110">Tjänster i hello Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="9928b-110">Management Services in hello Azure Classic portal</span></span>  

## <a name="dashboard-of-hello-stream-analytics-job"></a><span data-ttu-id="9928b-111">Instrumentpanelen i hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="9928b-111">Dashboard of hello Stream Analytics job</span></span>
<span data-ttu-id="9928b-112">En länk toohello motsvarande loggar för ett Stream Analytics-jobb visas på fliken instrumentpanelen i hello jobb. Om du klickar på länken anges hello filter på ett sätt att den visar senaste loggar för det specifika jobbet.</span><span class="sxs-lookup"><span data-stu-id="9928b-112">A link toohello corresponding logs of a Stream Analytics job is displayed on hello job’s Dashboard tab. If you click on that link, it will set hello filters in a way that it shows latest logs for that specific job.</span></span>

  ![Välj Management Services-loggar](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a><span data-ttu-id="9928b-114">Hanteringstjänster</span><span class="sxs-lookup"><span data-stu-id="9928b-114">Management Services</span></span>
<span data-ttu-id="9928b-115">toomanually navigera toohello Åtgärdsloggar för Stream Analytics och andra tjänster i hello Azure klassiska portal:</span><span class="sxs-lookup"><span data-stu-id="9928b-115">toomanually navigate toohello Operation Logs for Stream Analytics and other services in hello Azure Classic portal:</span></span>

1. <span data-ttu-id="9928b-116">Klicka på **hanteringstjänster** i hello [Azure klassiska portal](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9928b-116">Click on **Management Services** in hello [Azure Classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9928b-117">Välj **Stream Analytics** för **typen** och hello namnet på hello jobb för **tjänstnamnet**.</span><span class="sxs-lookup"><span data-stu-id="9928b-117">Select **Stream Analytics** for **Type** and hello name of hello job for **Service Name**.</span></span>  
   
   ![Välj Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a><span data-ttu-id="9928b-119">Hitta granskningsloggar i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9928b-119">Find audit logs in hello Azure portal</span></span>
<span data-ttu-id="9928b-120">toofind operativa loggar för Stream Analytics-jobbet i hello Azure-portalen klickar du på **Bläddra** och välj sedan **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="9928b-120">toofind operational logs for your Stream Analytics job in hello Azure portal, Click **Browse** and then select **Audit logs**.</span></span>

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-122">Då öppnas ett blad som visar händelser från hello senaste 7 dagarna för alla resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9928b-122">This will open a blade showing events from hello last 7 days for all resources in your subscription.</span></span>  <span data-ttu-id="9928b-123">Du kan filtrera toosee händelser av en ange typ eller tidsintervall genom att klicka på hello **Filter** kommando.</span><span class="sxs-lookup"><span data-stu-id="9928b-123">You can filter toosee events of a specify type or time frame by clicking hello **Filter** command.</span></span>

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a><span data-ttu-id="9928b-125">Hämta logginformation om</span><span class="sxs-lookup"><span data-stu-id="9928b-125">Get log details</span></span>
<span data-ttu-id="9928b-126">Du kan filtrera efter Status tooview hello loggar och tidsintervall för jobbet.</span><span class="sxs-lookup"><span data-stu-id="9928b-126">You can filter by Time Range and Status tooview hello logs for your job.</span></span>

<span data-ttu-id="9928b-127">Klicka på hello i hello Azure Management portal **information** knappen längst ned hello hello fönstret tooview mer information om en markerad händelse.</span><span class="sxs-lookup"><span data-stu-id="9928b-127">In hello Azure Management portal, click on hello **Details** button at hello bottom of hello window tooview more details about a selected event.</span></span> 

  ![Välj information](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-129">I hello hello Azure-portalen klickar du på en logg post toosee detaljerade händelser i den.</span><span class="sxs-lookup"><span data-stu-id="9928b-129">In hello Azure portal, click on a log entry toosee hello detailed events inside it.</span></span>

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-131">Därifrån kan du öppna hello **detaljer** bladet genom att klicka på hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="9928b-131">From there, you can open hello **Detail** blade by clicking on hello event.</span></span>

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a><span data-ttu-id="9928b-133">Felsöka ett jobb som misslyckades</span><span class="sxs-lookup"><span data-stu-id="9928b-133">Debug a failed job</span></span>
<span data-ttu-id="9928b-134">Klicka på sökikonen hello i hello Azure-hanteringsportalen och Skriv ”misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="9928b-134">In hello Azure Management portal, click on hello Search icon and type ‘failed’.</span></span> <span data-ttu-id="9928b-135">Detta ger ett resultat av alla loggar med fel.</span><span class="sxs-lookup"><span data-stu-id="9928b-135">This gives a result of all logs with failures.</span></span> 

  ![Felsökning av ett jobb som misslyckades](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-137">I hello Azure-portalen, kan du filtrera efter nivå av meddelandet tooview **kritisk** händelser.</span><span class="sxs-lookup"><span data-stu-id="9928b-137">In hello Azure portal, you can filter by level of message tooview **Critical** events.</span></span>

  ![Felsökning i Azure portal](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-139">Du kan välja en hello fel och klicka på hello **information** för mer information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="9928b-139">You can select any one of hello failures, and click on hello **Details** for more information on hello error.</span></span>  <span data-ttu-id="9928b-140">Somliga felmeddelanden innehåller också information om hur toomitigate hello problemet.</span><span class="sxs-lookup"><span data-stu-id="9928b-140">Some error messages also provide information about how toomitigate hello issue.</span></span> 

  ![Åtgärdsinformation](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

<span data-ttu-id="9928b-142">Om du behöver toocontact [stöd](https://azure.microsoft.com/support/options/) eller ange information toohello team via hello [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), särskilt Observera hello driftsinformation hello **Korrelations-ID**.</span><span class="sxs-lookup"><span data-stu-id="9928b-142">In case you need toocontact [Support](https://azure.microsoft.com/support/options/) or provide information toohello team via hello [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), please note hello Operation Details, specifically hello **Correlation ID**.</span></span> 

## <a name="get-help"></a><span data-ttu-id="9928b-143">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="9928b-143">Get help</span></span>
<span data-ttu-id="9928b-144">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="9928b-144">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9928b-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9928b-145">Next steps</span></span>
* [<span data-ttu-id="9928b-146">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9928b-146">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="9928b-147">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9928b-147">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="9928b-148">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="9928b-148">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="9928b-149">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="9928b-149">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="9928b-150">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="9928b-150">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

