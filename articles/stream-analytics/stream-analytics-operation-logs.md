---
title: "Felsöka med hjälp av åtgärden och tjänsten loggar i Stream Analytics | Microsoft Docs"
description: "Användningsinstruktioner Stream Analytics åtgärdsloggar"
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
ms.openlocfilehash: c95d240ebef6a84228eb98db70002792fcfbdea6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a><span data-ttu-id="726cb-104">Felsöka Stream Analytics-jobb med hjälp av loggar och drift</span><span class="sxs-lookup"><span data-stu-id="726cb-104">Debug Stream Analytics jobs using service and operation logs</span></span>
<span data-ttu-id="726cb-105">Alla Azure-tjänster ange operativa meddelandeloggning för användarna att registrera information som rör hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="726cb-105">All Azure services supply operational logging messages to users to record details related to management operations.</span></span> <span data-ttu-id="726cb-106">Den här informationen kan användas för felsökning, till exempel visa jobbstatus jobb pågår i Azure Stream Analytics och felmeddelanden för att spåra förloppet för ett jobb med tiden från börjar bearbetningen till utdata.</span><span class="sxs-lookup"><span data-stu-id="726cb-106">In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages to track the progress of a job over time, from start to processing to output.</span></span>

## <a name="find-operation-logs-in-the-azure-management-portal"></a><span data-ttu-id="726cb-107">Hitta åtgärden loggas i Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="726cb-107">Find operation logs in the Azure Management portal</span></span>
<span data-ttu-id="726cb-108">Åtgärdsloggar kan nås på två sätt:</span><span class="sxs-lookup"><span data-stu-id="726cb-108">Operation Logs can be accessed in two ways:</span></span>  

* <span data-ttu-id="726cb-109">Instrumentpanelen i Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="726cb-109">Dashboard of the Stream Analytics job</span></span>  
* <span data-ttu-id="726cb-110">Tjänster i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="726cb-110">Management Services in the Azure Classic portal</span></span>  

## <a name="dashboard-of-the-stream-analytics-job"></a><span data-ttu-id="726cb-111">Instrumentpanelen i Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="726cb-111">Dashboard of the Stream Analytics job</span></span>
<span data-ttu-id="726cb-112">En länk till en Stream Analytics-jobbet motsvarande loggarna visas på fliken instrumentpanelen i jobbets. Om du klickar på länken anges filtren så att den visar senaste loggar för det specifika jobbet.</span><span class="sxs-lookup"><span data-stu-id="726cb-112">A link to the corresponding logs of a Stream Analytics job is displayed on the job’s Dashboard tab. If you click on that link, it will set the filters in a way that it shows latest logs for that specific job.</span></span>

  ![Välj Management Services-loggar](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a><span data-ttu-id="726cb-114">Hanteringstjänster</span><span class="sxs-lookup"><span data-stu-id="726cb-114">Management Services</span></span>
<span data-ttu-id="726cb-115">Navigera till åtgärden-loggar manuellt för Stream Analytics och andra tjänster i den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="726cb-115">To manually navigate to the Operation Logs for Stream Analytics and other services in the Azure Classic portal:</span></span>

1. <span data-ttu-id="726cb-116">Klicka på **hanteringstjänster** i den [Azure klassiska portal](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="726cb-116">Click on **Management Services** in the [Azure Classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="726cb-117">Välj **Stream Analytics** för **typen** och namnet på jobbet för **tjänstnamnet**.</span><span class="sxs-lookup"><span data-stu-id="726cb-117">Select **Stream Analytics** for **Type** and the name of the job for **Service Name**.</span></span>  
   
   ![Välj Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a><span data-ttu-id="726cb-119">Hitta granskningsloggar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="726cb-119">Find audit logs in the Azure portal</span></span>
<span data-ttu-id="726cb-120">Om du vill hitta operativa loggar för Stream Analytics-jobbet i Azure-portalen klickar du på **Bläddra** och välj sedan **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="726cb-120">To find operational logs for your Stream Analytics job in the Azure portal, Click **Browse** and then select **Audit logs**.</span></span>

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-122">Då öppnas ett blad som visar händelser från de senaste 7 dagarna för alla resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="726cb-122">This will open a blade showing events from the last 7 days for all resources in your subscription.</span></span>  <span data-ttu-id="726cb-123">Du kan filtrera om du vill se händelser med en ange typ eller tidsintervall genom att klicka på den **Filter** kommando.</span><span class="sxs-lookup"><span data-stu-id="726cb-123">You can filter to see events of a specify type or time frame by clicking the **Filter** command.</span></span>

  ![Azure portal väljer Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a><span data-ttu-id="726cb-125">Hämta logginformation om</span><span class="sxs-lookup"><span data-stu-id="726cb-125">Get log details</span></span>
<span data-ttu-id="726cb-126">Du kan filtrera efter tidsintervall och Status för att visa loggar för jobbet.</span><span class="sxs-lookup"><span data-stu-id="726cb-126">You can filter by Time Range and Status to view the logs for your job.</span></span>

<span data-ttu-id="726cb-127">I Azure-hanteringsportalen klickar du på den **information** längst ned i fönstret för att visa mer information om en markerad händelse.</span><span class="sxs-lookup"><span data-stu-id="726cb-127">In the Azure Management portal, click on the **Details** button at the bottom of the window to view more details about a selected event.</span></span> 

  ![Välj information](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-129">Klicka på en loggpost för att se de detaljerade händelserna i den i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="726cb-129">In the Azure portal, click on a log entry to see the detailed events inside it.</span></span>

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-131">Därifrån kan du öppna den **detaljer** bladet genom att klicka på händelsen.</span><span class="sxs-lookup"><span data-stu-id="726cb-131">From there, you can open the **Detail** blade by clicking on the event.</span></span>

  ![Azure portal väljer information](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a><span data-ttu-id="726cb-133">Felsöka ett jobb som misslyckades</span><span class="sxs-lookup"><span data-stu-id="726cb-133">Debug a failed job</span></span>
<span data-ttu-id="726cb-134">Klicka på sökikonen och typ ”misslyckad” i Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="726cb-134">In the Azure Management portal, click on the Search icon and type ‘failed’.</span></span> <span data-ttu-id="726cb-135">Detta ger ett resultat av alla loggar med fel.</span><span class="sxs-lookup"><span data-stu-id="726cb-135">This gives a result of all logs with failures.</span></span> 

  ![Felsökning av ett jobb som misslyckades](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-137">I Azure-portalen kan du filtrera efter nivå av meddelandet för att visa **kritisk** händelser.</span><span class="sxs-lookup"><span data-stu-id="726cb-137">In the Azure portal, you can filter by level of message to view **Critical** events.</span></span>

  ![Felsökning i Azure portal](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-139">Du kan välja en av felen och klicka på den **information** mer information om felet.</span><span class="sxs-lookup"><span data-stu-id="726cb-139">You can select any one of the failures, and click on the **Details** for more information on the error.</span></span>  <span data-ttu-id="726cb-140">Vissa felmeddelanden ger också information om hur du kan undvika problemet.</span><span class="sxs-lookup"><span data-stu-id="726cb-140">Some error messages also provide information about how to mitigate the issue.</span></span> 

  ![Åtgärdsinformation](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

<span data-ttu-id="726cb-142">Om du behöver kontakta [stöd](https://azure.microsoft.com/support/options/) eller ange information i teamet via den [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), Observera driftsinformation, särskilt de **Korrelations-ID**.</span><span class="sxs-lookup"><span data-stu-id="726cb-142">In case you need to contact [Support](https://azure.microsoft.com/support/options/) or provide information to the team via the [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), please note the Operation Details, specifically the **Correlation ID**.</span></span> 

## <a name="get-help"></a><span data-ttu-id="726cb-143">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="726cb-143">Get help</span></span>
<span data-ttu-id="726cb-144">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="726cb-144">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="726cb-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="726cb-145">Next steps</span></span>
* [<span data-ttu-id="726cb-146">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="726cb-146">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="726cb-147">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="726cb-147">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="726cb-148">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="726cb-148">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="726cb-149">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="726cb-149">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="726cb-150">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="726cb-150">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

