---
title: "aaaReal tidsdata visualisering av sensordata från Azure IoT-hubb – Power BI | Microsoft Docs"
description: "Använd Power BI toovisualize temperatur- och fuktighetskonsekvens data som samlas in från hello sensor och skickas tooyour Azure IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: realtid datavisualisering, realtidsdata visualiseringen sensor datavisualisering
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: d79ce757a9f2ab7a4744e8a0c523106e0f72cecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a><span data-ttu-id="581ee-104">Visualisera sensordata i realtid från Azure IoT-hubb med Power BI</span><span class="sxs-lookup"><span data-stu-id="581ee-104">Visualize real-time sensor data from Azure IoT Hub using Power BI</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="581ee-106">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="581ee-106">What you learn</span></span>

<span data-ttu-id="581ee-107">Du lär dig hur toovisualize sensordata i realtid som tar emot din Azure IoT-hubb av Power BI.</span><span class="sxs-lookup"><span data-stu-id="581ee-107">You learn how toovisualize real-time sensor data that your Azure IoT hub receives by Power BI.</span></span> <span data-ttu-id="581ee-108">Om du vill visualisera tootry hello data i din IoT-hubb med Web Apps Se [Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="581ee-108">If you want tootry visualize hello data in your IoT hub with Web Apps, please see [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="581ee-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="581ee-109">What you do</span></span>

- <span data-ttu-id="581ee-110">Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="581ee-110">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="581ee-111">Skapa, konfigurera och köra ett Stream Analytics-jobb för att överföra data från din IoT-hubb tooyour Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="581ee-111">Create, configure and run a Stream Analytics job for data transfer from your IoT hub tooyour Power BI account.</span></span>
- <span data-ttu-id="581ee-112">Skapa och publicera en Power BI rapporten toovisualize hello data.</span><span class="sxs-lookup"><span data-stu-id="581ee-112">Create and publish a Power BI report toovisualize hello data.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="581ee-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="581ee-113">What you need</span></span>

- <span data-ttu-id="581ee-114">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="581ee-114">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="581ee-115">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="581ee-115">An active Azure subscription.</span></span>
  - <span data-ttu-id="581ee-116">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="581ee-116">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="581ee-117">Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="581ee-117">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="581ee-118">Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="581ee-118">A Power BI account.</span></span> <span data-ttu-id="581ee-119">([Testa Powerbi gratis](https://powerbi.microsoft.com/))</span><span class="sxs-lookup"><span data-stu-id="581ee-119">([Try Power BI for free](https://powerbi.microsoft.com/))</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="581ee-120">Skapa, konfigurera och köra ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="581ee-120">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="581ee-121">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="581ee-121">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="581ee-122">I hello Azure-portalen, klicka på Ny > Internet of Things > Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="581ee-122">In hello Azure portal, click New > Internet of Things > Stream Analytics job.</span></span>
1. <span data-ttu-id="581ee-123">Ange följande information för jobbet hello hello.</span><span class="sxs-lookup"><span data-stu-id="581ee-123">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="581ee-124">**Jobbnamnet**: hello namnet på hello jobb.</span><span class="sxs-lookup"><span data-stu-id="581ee-124">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="581ee-125">hello namn måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="581ee-125">hello name must be globally unique.</span></span>

   <span data-ttu-id="581ee-126">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="581ee-126">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="581ee-127">**Plats**: Använd hello samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="581ee-127">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="581ee-128">**PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="581ee-128">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Skapa ett Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="581ee-130">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="581ee-130">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="581ee-131">Lägg till ett inkommande toohello Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="581ee-131">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="581ee-132">Öppna hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="581ee-132">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="581ee-133">Under **jobbet topologi**, klickar du på **indata**.</span><span class="sxs-lookup"><span data-stu-id="581ee-133">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="581ee-134">I hello **indata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:</span><span class="sxs-lookup"><span data-stu-id="581ee-134">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="581ee-135">**Ett inmatat alias**: unika hello-alias för hello indata.</span><span class="sxs-lookup"><span data-stu-id="581ee-135">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="581ee-136">**Källan**: Välj **IoT-hubb**.</span><span class="sxs-lookup"><span data-stu-id="581ee-136">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="581ee-137">**Konsumentgrupp**: Välj hello konsumentgrupp som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="581ee-137">**Consumer group**: Select hello consumer group you just created.</span></span>
1. <span data-ttu-id="581ee-138">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="581ee-138">Click **Create**.</span></span>

   ![Lägg till ett inkommande tooa Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="581ee-140">Lägga till utdata toohello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="581ee-140">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="581ee-141">Under **jobbet topologi**, klickar du på **utdata**.</span><span class="sxs-lookup"><span data-stu-id="581ee-141">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="581ee-142">I hello **utdata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:</span><span class="sxs-lookup"><span data-stu-id="581ee-142">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="581ee-143">**Kolumnalias**: unikt hello-alias för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="581ee-143">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="581ee-144">**Sink**: Välj **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="581ee-144">**Sink**: Select **Power BI**.</span></span>
1. <span data-ttu-id="581ee-145">Klicka på **auktorisera**, och sedan logga in på ditt Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="581ee-145">Click **Authorize**, and then sign into your Power BI account.</span></span>
1. <span data-ttu-id="581ee-146">När behöriga, ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="581ee-146">Once authorized, enter hello following information:</span></span>

   <span data-ttu-id="581ee-147">**Gruppera arbetsytan**: Välj din grupp målarbetsytan.</span><span class="sxs-lookup"><span data-stu-id="581ee-147">**Group Workspace**: Select your target group workspace.</span></span>

   <span data-ttu-id="581ee-148">**DataSet-namnet**: Ange ett namn för dataset.</span><span class="sxs-lookup"><span data-stu-id="581ee-148">**Dataset Name**: Enter a dataset name.</span></span>

   <span data-ttu-id="581ee-149">**Tabellnamn**: Ange ett tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="581ee-149">**Table Name**: Enter a table name.</span></span>
1. <span data-ttu-id="581ee-150">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="581ee-150">Click **Create**.</span></span>

   ![Lägga till utdata tooa Stream Analytics-jobbet i Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="581ee-152">Konfigurera hello frågan för hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="581ee-152">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="581ee-153">Under **jobbet topologi**, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="581ee-153">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="581ee-154">Ersätt `[YourInputAlias]` med hello Ange alias för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="581ee-154">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>
1. <span data-ttu-id="581ee-155">Ersätt `[YourOutputAlias]` med hello kolumnalias för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="581ee-155">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>
1. <span data-ttu-id="581ee-156">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="581ee-156">Click **Save**.</span></span>

   ![Lägg till en fråga tooa Stream Analytics-jobbet i Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="581ee-158">Kör hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="581ee-158">Run hello Stream Analytics job</span></span>

<span data-ttu-id="581ee-159">Klicka på hello Stream Analytics-jobbet **starta** > **nu** > **starta**.</span><span class="sxs-lookup"><span data-stu-id="581ee-159">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="581ee-160">När hello jobbet har startar hello jobbets status har ändrats från **stoppad** för**kör**.</span><span class="sxs-lookup"><span data-stu-id="581ee-160">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Kör ett Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a><span data-ttu-id="581ee-162">Skapa och publicera en Power BI rapporten toovisualize hello data</span><span class="sxs-lookup"><span data-stu-id="581ee-162">Create and publish a Power BI report toovisualize hello data</span></span>

1. <span data-ttu-id="581ee-163">Kontrollera hello exempelprogrammet körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="581ee-163">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="581ee-164">Om inte, kan du läsa toohello självstudier under [konfigurera enheten](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span><span class="sxs-lookup"><span data-stu-id="581ee-164">If not, you can refer toohello tutorials under [Setup your device](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span></span>
1. <span data-ttu-id="581ee-165">Logga in tooyour [Power BI](https://powerbi.microsoft.com/en-us/) konto.</span><span class="sxs-lookup"><span data-stu-id="581ee-165">Sign in tooyour [Power BI](https://powerbi.microsoft.com/en-us/) account.</span></span>
1. <span data-ttu-id="581ee-166">Gå toohello grupp arbetsyta som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="581ee-166">Go toohello group workspace that you set when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="581ee-167">Klicka på **strömning datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="581ee-167">Click **Streaming datasets**.</span></span>

   <span data-ttu-id="581ee-168">Du bör se hello visas dataset som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="581ee-168">You should see hello listed dataset that you specified when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="581ee-169">Under **åtgärder**, klicka på hello första ikonen toocreate en rapport.</span><span class="sxs-lookup"><span data-stu-id="581ee-169">Under **ACTIONS**, click hello first icon toocreate a report.</span></span>

   ![Skapa en Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. <span data-ttu-id="581ee-171">Skapa ett diagram tooshow realtid temperatur över tid.</span><span class="sxs-lookup"><span data-stu-id="581ee-171">Create a line chart tooshow real-time temperature over time.</span></span>
   1. <span data-ttu-id="581ee-172">Lägg till ett linjediagram hello rapport skapas på sidan.</span><span class="sxs-lookup"><span data-stu-id="581ee-172">On hello report creation page, add a line chart.</span></span>
   1. <span data-ttu-id="581ee-173">På hello **fält** rutan Expandera hello-tabell som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="581ee-173">On hello **Fields** pane, expand hello table that you specified when you created hello output for hello Stream Analytics job.</span></span>
   1. <span data-ttu-id="581ee-174">Dra **EventEnqueuedUtcTime** för**axel** på hello **visualiseringar** fönstret.</span><span class="sxs-lookup"><span data-stu-id="581ee-174">Drag **EventEnqueuedUtcTime** too**Axis** on hello **Visualizations** pane.</span></span>
   1. <span data-ttu-id="581ee-175">Dra **temperatur** för**värden**.</span><span class="sxs-lookup"><span data-stu-id="581ee-175">Drag **temperature** too**Values**.</span></span>

      <span data-ttu-id="581ee-176">Nu skapas ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="581ee-176">Now a line chart is created.</span></span> <span data-ttu-id="581ee-177">hello x-axeln visar datum och tid i hello UTC-tidszonen.</span><span class="sxs-lookup"><span data-stu-id="581ee-177">hello x-axis displays date and time in hello UTC time zone.</span></span> <span data-ttu-id="581ee-178">hello y-axeln visar temperatur från hello sensor.</span><span class="sxs-lookup"><span data-stu-id="581ee-178">hello y-axis displays temperature from hello sensor.</span></span>

      ![Lägg till ett linjediagram för temperatur tooa Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="581ee-180">Skapa en annan rad diagram tooshow realtid fuktighet över tid.</span><span class="sxs-lookup"><span data-stu-id="581ee-180">Create another line chart tooshow real-time humidity over time.</span></span> <span data-ttu-id="581ee-181">toodo, följ samma steg ovan hello och placera **EventEnqueuedUtcTime** på hello x-axeln och **fuktighet** på hello y-axeln.</span><span class="sxs-lookup"><span data-stu-id="581ee-181">toodo this, follow hello same steps above and place **EventEnqueuedUtcTime** on hello x-axis and **humidity** on hello y-axis.</span></span>

   ![Lägg till ett linjediagram för fuktighet tooa Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="581ee-183">Klicka på **spara** toosave hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="581ee-183">Click **Save** toosave hello report.</span></span>
1. <span data-ttu-id="581ee-184">Klicka på **filen** > **publicera tooweb**.</span><span class="sxs-lookup"><span data-stu-id="581ee-184">Click **File** > **Publish tooweb**.</span></span>
1. <span data-ttu-id="581ee-185">Klicka på **skapa inbäddningskod**, och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="581ee-185">Click **Create embed code**, and then click **Publish**.</span></span>

<span data-ttu-id="581ee-186">Du har angett hello rapportlänken som du kan dela med någon för åtkomst till rapporten och en kodfragment toointegrate hello rapport i din blogg eller webbplats.</span><span class="sxs-lookup"><span data-stu-id="581ee-186">You're provided hello report link that you can share with anyone for report access and a code snippet toointegrate hello report into your blog or website.</span></span>

![Publicera en Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

<span data-ttu-id="581ee-188">Microsoft erbjuder även hello [Power BI-appar](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) för att visa och interagera med dina Power BI-instrumentpaneler och rapporter på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="581ee-188">Microsoft also offers hello [Power BI mobile apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) for viewing and interacting with your Power BI dashboards and reports on your mobile device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="581ee-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="581ee-189">Next steps</span></span>

<span data-ttu-id="581ee-190">Du har använt sensordata i Power BI toovisualize realtid från din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="581ee-190">You’ve successfully used Power BI toovisualize real-time sensor data from your Azure IoT hub.</span></span>
<span data-ttu-id="581ee-191">Det finns ett annat sätt toovisualize data från Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="581ee-191">There is an alternate way toovisualize data from Azure IoT Hub.</span></span> <span data-ttu-id="581ee-192">Se [Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="581ee-192">See [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
