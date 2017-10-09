---
title: "aaaPower BI-instrumentpanelen på Azure Stream Analytics | Microsoft Docs"
description: "Använd en realtid strömmande Power BI dashboard toogather affärsinformation och analysera stora volymer data från ett Stream Analytics-jobb."
keywords: instrumentpanelen, realtid instrumentpanelen
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a><span data-ttu-id="bff5e-104">Strömma analyser och Power BI: en analys i realtid instrumentpanel för strömmande data</span><span class="sxs-lookup"><span data-stu-id="bff5e-104">Stream Analytics and Power BI: A real-time analytics dashboard for streaming data</span></span>
<span data-ttu-id="bff5e-105">Azure Stream Analytics gör tootake fördel av hello inledande verktyg för business intelligence [Microsoft Power BI](https://powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="bff5e-105">Azure Stream Analytics enables you tootake advantage of one of hello leading business intelligence tools, [Microsoft Power BI](https://powerbi.com/).</span></span> <span data-ttu-id="bff5e-106">I den här artikeln får du lära dig hur skapa business intelligence-verktyg med hjälp av Power BI som utdata för Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="bff5e-106">In this article, you learn how create business intelligence tools by using Power BI as an output for your Azure Stream Analytics jobs.</span></span> <span data-ttu-id="bff5e-107">Du också lära dig hur toocreate och använda en realtid instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="bff5e-107">You also learn how toocreate and use a real-time dashboard.</span></span>

<span data-ttu-id="bff5e-108">Den här artikeln fortsätter från hello Stream Analytics [att upptäcka bedrägerier realtid](stream-analytics-real-time-fraud-detection.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="bff5e-108">This article continues from hello Stream Analytics [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="bff5e-109">Den bygger på hello arbetsflöde som skapats i denna självstudiekurs och lägger till en Power BI utdata så att du kan visualisera bedrägliga telefonsamtal som identifieras av en Streaming Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="bff5e-109">It builds on hello workflow created in that tutorial and adds a Power BI output so that you can visualize fraudulent phone calls that are detected by a Streaming Analytics job.</span></span> 

<span data-ttu-id="bff5e-110">Du kan titta på [en video](https://www.youtube.com/watch?v=SGUpT-a99MA) som visar det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="bff5e-110">You can watch [a video](https://www.youtube.com/watch?v=SGUpT-a99MA)  that illustrates this scenario.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="bff5e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="bff5e-111">Prerequisites</span></span>

<span data-ttu-id="bff5e-112">Innan du börjar bör du kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="bff5e-112">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="bff5e-113">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bff5e-113">An Azure account.</span></span>
* <span data-ttu-id="bff5e-114">Ett konto för Power BI.</span><span class="sxs-lookup"><span data-stu-id="bff5e-114">An account for Power BI.</span></span> <span data-ttu-id="bff5e-115">Du kan använda ett arbetskonto eller ett skolkonto.</span><span class="sxs-lookup"><span data-stu-id="bff5e-115">You can use a work account or a school account.</span></span>
* <span data-ttu-id="bff5e-116">En fullständig version av hello [att upptäcka bedrägerier realtid](stream-analytics-real-time-fraud-detection.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="bff5e-116">A completed version of hello [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="bff5e-117">hello självstudier innehåller en app som genererar fiktiva telefonsamtal metadata.</span><span class="sxs-lookup"><span data-stu-id="bff5e-117">hello tutorial includes an app that generates fictitious telephone-call metadata.</span></span> <span data-ttu-id="bff5e-118">I hello självstudien du skapar en händelsehubb och skickar hello strömning telefonsamtal data toohello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="bff5e-118">In hello tutorial, you create an event hub and send hello streaming phone call data toohello event hub.</span></span> <span data-ttu-id="bff5e-119">Du kan skriva en fråga som identifierar bedrägliga anrop (anrop från samma tal på hello hello, samma tid på olika platser).</span><span class="sxs-lookup"><span data-stu-id="bff5e-119">You write a query that detects fraudulent calls (calls from hello same number at hello same time in different locations).</span></span> 


## <a name="add-power-bi-output"></a><span data-ttu-id="bff5e-120">Lägga till Power BI-utdata</span><span class="sxs-lookup"><span data-stu-id="bff5e-120">Add Power BI output</span></span>
<span data-ttu-id="bff5e-121">I hello realtid bedrägeri identifiering självstudien skickas hello utdata tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bff5e-121">In hello real-time fraud detection tutorial, hello output is sent tooAzure Blob storage.</span></span> <span data-ttu-id="bff5e-122">I det här avsnittet kan du lägga till utdata som skickar information tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="bff5e-122">In this section, you add an output that sends information tooPower BI.</span></span>

1. <span data-ttu-id="bff5e-123">Öppna hello Streaming Analytics-jobbet som du skapade tidigare i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bff5e-123">In hello Azure portal, open hello Streaming Analytics job that you created earlier.</span></span> <span data-ttu-id="bff5e-124">Om du har använt hello föreslagna namn hello jobbet heter `sa_frauddetection_job_demo`.</span><span class="sxs-lookup"><span data-stu-id="bff5e-124">If you used hello suggested name, hello job is named `sa_frauddetection_job_demo`.</span></span>

2. <span data-ttu-id="bff5e-125">Välj hello **utdata** hello mitten av hello jobbet instrumentpanelen och välj sedan **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-125">Select hello **Outputs** box in hello middle of hello job dashboard and then select **+ Add**.</span></span>

3. <span data-ttu-id="bff5e-126">För **Utdataalias**, ange `CallStream-PowerBI`.</span><span class="sxs-lookup"><span data-stu-id="bff5e-126">For **Output Alias**, enter `CallStream-PowerBI`.</span></span> <span data-ttu-id="bff5e-127">Du kan använda ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="bff5e-127">You can use a different name.</span></span> <span data-ttu-id="bff5e-128">Om du vill anteckna, eftersom du måste ha namnet hello senare.</span><span class="sxs-lookup"><span data-stu-id="bff5e-128">If you do, make a note of it, because you need hello name later.</span></span> 

4. <span data-ttu-id="bff5e-129">Under **Sink**väljer **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-129">Under **Sink**, select **Power BI**.</span></span>

   ![Skapa ett utgående för Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. <span data-ttu-id="bff5e-131">Klicka på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-131">Click **Authorize**.</span></span>

    <span data-ttu-id="bff5e-132">En öppnas där du kan ange dina Azure-autentiseringsuppgifter för ett konto för arbetet eller skolan.</span><span class="sxs-lookup"><span data-stu-id="bff5e-132">A window opens where you can provide your Azure credentials for a work or school account.</span></span> 

    ![Ange autentiseringsuppgifter för åtkomst tooPower BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. <span data-ttu-id="bff5e-134">Ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bff5e-134">Enter your credentials.</span></span> <span data-ttu-id="bff5e-135">Tänk sedan när du anger dina autentiseringsuppgifter måste du också håller behörighet toohello Streaming Analytics-jobbet tooaccess Power BI-område.</span><span class="sxs-lookup"><span data-stu-id="bff5e-135">Be aware then when you enter your credentials, you're also giving permission toohello Streaming Analytics job tooaccess your Power BI area.</span></span>

7. <span data-ttu-id="bff5e-136">När du kommer tillbaka toohello **nya utdata** bladet ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="bff5e-136">When you're returned toohello **New output** blade, enter hello following information:</span></span>

    * <span data-ttu-id="bff5e-137">**Gruppera arbetsytan**: Välj en arbetsyta i Power BI-klienten där du vill att toocreate hello dataset.</span><span class="sxs-lookup"><span data-stu-id="bff5e-137">**Group Workspace**: Select a workspace in your Power BI tenant where you want toocreate hello dataset.</span></span>
    * <span data-ttu-id="bff5e-138">**DataSet-namnet**: Ange `sa-dataset`.</span><span class="sxs-lookup"><span data-stu-id="bff5e-138">**Dataset Name**:  Enter `sa-dataset`.</span></span> <span data-ttu-id="bff5e-139">Du kan använda ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="bff5e-139">You can use a different name.</span></span> <span data-ttu-id="bff5e-140">Om du vill anteckna den för senare.</span><span class="sxs-lookup"><span data-stu-id="bff5e-140">If you do, make a note of it for later.</span></span>
    * <span data-ttu-id="bff5e-141">**Tabellnamn**: Ange `fraudulent-calls`.</span><span class="sxs-lookup"><span data-stu-id="bff5e-141">**Table Name**: Enter `fraudulent-calls`.</span></span> <span data-ttu-id="bff5e-142">Power BI-utdata från Stream Analytics-jobb kan för närvarande har endast en tabell i en dataset.</span><span class="sxs-lookup"><span data-stu-id="bff5e-142">Currently, Power BI output from Stream Analytics jobs can have only one table in a dataset.</span></span>

    ![PBI-arbetsytan](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > <span data-ttu-id="bff5e-144">Om Power BI har en datauppsättning och tabell som har hello samma namn som hello som du anger i hello Stream Analytics-jobbet, skrivs hello befintliga över.</span><span class="sxs-lookup"><span data-stu-id="bff5e-144">If Power BI has a dataset and table that have hello same names as hello ones that you specify in hello Stream Analytics job, hello existing ones are overwritten.</span></span>
    > <span data-ttu-id="bff5e-145">Vi rekommenderar att du inte uttryckligen skapar den här datauppsättningen och tabellen i Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="bff5e-145">We recommend that you do not explicitly create this dataset and table in your Power BI account.</span></span> <span data-ttu-id="bff5e-146">De skapas automatiskt när du startar Stream Analytics-jobbet och hello jobbet startar pumpande utdata till Power BI.</span><span class="sxs-lookup"><span data-stu-id="bff5e-146">They are automatically created when you start your Stream Analytics job and hello job starts pumping output into Power BI.</span></span> <span data-ttu-id="bff5e-147">Om jobbet frågan inte returnerar något resultat, skapas inte hello dataset och tabell.</span><span class="sxs-lookup"><span data-stu-id="bff5e-147">If your job query doesn't return any results, hello dataset and table are not  created.</span></span>
    >

8. <span data-ttu-id="bff5e-148">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-148">Click **Create**.</span></span>

<span data-ttu-id="bff5e-149">hello datauppsättningen har skapats med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="bff5e-149">hello dataset is created with hello following settings:</span></span>

* <span data-ttu-id="bff5e-150">**defaultRetentionPolicy: BasicFIFO**: Data är FIFO, med högst 200 000 rader.</span><span class="sxs-lookup"><span data-stu-id="bff5e-150">**defaultRetentionPolicy: BasicFIFO**: Data is FIFO, with a maximum of 200,000 rows.</span></span>
* <span data-ttu-id="bff5e-151">**defaultMode: pushStreaming**: hello dataset stöder både strömmande paneler och traditionella rapport-baserade visuell information (kallas även</span><span class="sxs-lookup"><span data-stu-id="bff5e-151">**defaultMode: pushStreaming**: hello dataset supports both streaming tiles and traditional report-based visuals (a.k.a.</span></span> <span data-ttu-id="bff5e-152">push).</span><span class="sxs-lookup"><span data-stu-id="bff5e-152">push).</span></span>

<span data-ttu-id="bff5e-153">För närvarande kan skapa du inte datauppsättningar med andra flaggor.</span><span class="sxs-lookup"><span data-stu-id="bff5e-153">Currently, you can't create datasets with other flags.</span></span>

<span data-ttu-id="bff5e-154">Mer information om Power BI-datauppsättningar finns hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) referens.</span><span class="sxs-lookup"><span data-stu-id="bff5e-154">For more information about Power BI datasets, see hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) reference.</span></span>


## <a name="write-hello-query"></a><span data-ttu-id="bff5e-155">Skriva hello fråga</span><span class="sxs-lookup"><span data-stu-id="bff5e-155">Write hello query</span></span>

1. <span data-ttu-id="bff5e-156">Stäng hello **utdata** bladet och returnera toohello jobb-bladet.</span><span class="sxs-lookup"><span data-stu-id="bff5e-156">Close hello **Outputs** blade and return toohello job blade.</span></span>

2. <span data-ttu-id="bff5e-157">Klicka på hello **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="bff5e-157">Click hello **Query** box.</span></span> 

3. <span data-ttu-id="bff5e-158">Ange hello följande fråga.</span><span class="sxs-lookup"><span data-stu-id="bff5e-158">Enter hello following query.</span></span> <span data-ttu-id="bff5e-159">Den här frågan är liknande toohello självkoppling frågan du skapade i hello upptäckt av bedrägerier kursen.</span><span class="sxs-lookup"><span data-stu-id="bff5e-159">This query is similar toohello self-join query you created in hello fraud-detection tutorial.</span></span> <span data-ttu-id="bff5e-160">hello skillnaden är att den här frågan skickar resultaten toohello nya utdata som du skapade (`CallStream-PowerBI`).</span><span class="sxs-lookup"><span data-stu-id="bff5e-160">hello difference is that this query sends results toohello new output you created (`CallStream-PowerBI`).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="bff5e-161">Om du inte name hello indata `CallStream` i hello upptäckt av bedrägerier självstudien ersätta ditt namn för `CallStream` i hello **från** och **ansluta** satser i hello fråga.</span><span class="sxs-lookup"><span data-stu-id="bff5e-161">If you did not name hello input `CallStream` in hello fraud-detection tutorial, substitute your name for `CallStream` in hello **FROM** and **JOIN** clauses in hello query.</span></span>

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. <span data-ttu-id="bff5e-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-162">Click **Save**.</span></span>


## <a name="test-hello-query"></a><span data-ttu-id="bff5e-163">Testa hello fråga</span><span class="sxs-lookup"><span data-stu-id="bff5e-163">Test hello query</span></span>
<span data-ttu-id="bff5e-164">Det här avsnittet är valfritt men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="bff5e-164">This section is optional, but recommended.</span></span> 

1. <span data-ttu-id="bff5e-165">Om hello TelcoStreaming appen inte körs startar du den genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="bff5e-165">If hello TelcoStreaming app is not currently running, start it by following these steps:</span></span>

    * <span data-ttu-id="bff5e-166">Öppna ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="bff5e-166">Open a command window.</span></span>
    * <span data-ttu-id="bff5e-167">Gå toohello mapp där hello telcogenerator.exe och ändrade telcodatagen.exe.config filer.</span><span class="sxs-lookup"><span data-stu-id="bff5e-167">Go toohello folder where hello telcogenerator.exe and modified telcodatagen.exe.config files are.</span></span>
    * <span data-ttu-id="bff5e-168">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bff5e-168">Run hello following command:</span></span>

            telcodatagen.exe 1000 .2 2

2. <span data-ttu-id="bff5e-169">I hello **frågan** bladet, klickar du på hello punkter nästa toohello `CallStream` indata och välj sedan **exempeldata från indata**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-169">In hello **Query** blade, click hello dots next toohello `CallStream` input and then select **Sample data from input**.</span></span>

3. <span data-ttu-id="bff5e-170">Du vill att tre minuter kan du se data och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-170">Specify that you want three minutes' worth of data and click **OK**.</span></span> <span data-ttu-id="bff5e-171">Vänta tills du meddelas att hello data har samlats in.</span><span class="sxs-lookup"><span data-stu-id="bff5e-171">Wait until you're notified that hello data has been sampled.</span></span>

4. <span data-ttu-id="bff5e-172">Klicka på **Test** och se till att du får resultat.</span><span class="sxs-lookup"><span data-stu-id="bff5e-172">Click **Test** and make sure you're getting results.</span></span>


## <a name="run-hello-job"></a><span data-ttu-id="bff5e-173">Kör jobb för hello</span><span class="sxs-lookup"><span data-stu-id="bff5e-173">Run hello job</span></span>

1. <span data-ttu-id="bff5e-174">Se till att hello TelcoStreaming appen körs.</span><span class="sxs-lookup"><span data-stu-id="bff5e-174">Make sure that hello TelcoStreaming app is running.</span></span>

2. <span data-ttu-id="bff5e-175">Stäng hello **frågan** bladet.</span><span class="sxs-lookup"><span data-stu-id="bff5e-175">Close hello **Query** blade.</span></span>

3. <span data-ttu-id="bff5e-176">I hello jobb-bladet, klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-176">In hello job blade, click **Start**.</span></span>

    ![Starta hello Stream Analytics-jobbet](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

<span data-ttu-id="bff5e-178">Streaming Analytics-jobbet startar söker efter bedrägliga anrop i hello inkommande dataström.</span><span class="sxs-lookup"><span data-stu-id="bff5e-178">Your Streaming Analytics job starts looking for fraudulent calls in hello incoming stream.</span></span> <span data-ttu-id="bff5e-179">hello jobbet också hello dataset och tabell skapas i Power BI och börjar skicka data om hello bedrägliga anrop toothem.</span><span class="sxs-lookup"><span data-stu-id="bff5e-179">hello job also creates hello dataset and table in Power BI and starts sending data about hello fraudulent calls toothem.</span></span>


## <a name="create-hello-dashboard-in-power-bi"></a><span data-ttu-id="bff5e-180">Skapa hello instrumentpanel i Power BI</span><span class="sxs-lookup"><span data-stu-id="bff5e-180">Create hello dashboard in Power BI</span></span>

1. <span data-ttu-id="bff5e-181">Gå för[Powerbi.com](https://powerbi.com) och logga in med ditt arbets- eller skolkonto.</span><span class="sxs-lookup"><span data-stu-id="bff5e-181">Go too[Powerbi.com](https://powerbi.com) and sign in with your work or school account.</span></span> <span data-ttu-id="bff5e-182">Om hello Stream Analytics-jobbet frågan överför resultaten, ser du att dina dataset redan har skapats:</span><span class="sxs-lookup"><span data-stu-id="bff5e-182">If hello Stream Analytics job query outputs results, you see that your dataset is already created:</span></span>

    ![Strömmande datauppsättning i Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. <span data-ttu-id="bff5e-184">I arbetsytan, klickar du på  **+ &nbsp;skapa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-184">In your workspace, click **+&nbsp;Create**.</span></span>

    ![hello skapa knappen i Power BI-arbetsyta](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. <span data-ttu-id="bff5e-186">Skapa en ny instrumentpanel och ger den namnet `Fraudulent Calls`.</span><span class="sxs-lookup"><span data-stu-id="bff5e-186">Create a new dashboard and name it `Fraudulent Calls`.</span></span>

    ![Skapa en instrumentpanel och ge det ett namn i Power BI-arbetsyta](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. <span data-ttu-id="bff5e-188">Hello överkant hello-fönstret klickar du på **Lägg till panelen**väljer **anpassade STRÖMMANDE DATA**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-188">At hello top of hello window, click **Add tile**, select **CUSTOM STREAMING DATA**, and then click **Next**.</span></span>

    ![Anpassad strömning dataset](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. <span data-ttu-id="bff5e-190">Under **din DATSETS**, Välj din datauppsättning och sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-190">Under **YOUR DATSETS**, select your dataset and then click **Next**.</span></span>

    ![Din strömmande datauppsättning](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. <span data-ttu-id="bff5e-192">Under **visualiseringen typen**väljer **kort**, och klicka sedan på hello **fält** väljer **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-192">Under **Visualization Type**, select **Card**, and then in hello **Fields** list, select **fraudulentcalls**.</span></span>

    ![Visualiseringen information för den nya panelen](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. <span data-ttu-id="bff5e-194">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-194">Click **Next**.</span></span>

8. <span data-ttu-id="bff5e-195">Fyll i panelen detaljer som en rubrik och underrubrik.</span><span class="sxs-lookup"><span data-stu-id="bff5e-195">Fill in tile details like a title and subtitle.</span></span>

    ![Rubrik och underrubrik för den nya panelen](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. <span data-ttu-id="bff5e-197">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-197">Click **Apply**.</span></span>

    <span data-ttu-id="bff5e-198">Nu har du en bedrägeri räknare!</span><span class="sxs-lookup"><span data-stu-id="bff5e-198">Now you have a fraud counter!</span></span>

    ![Bedrägeri räknare](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. <span data-ttu-id="bff5e-200">Följ hello steg igen tooadd en panel (startar med steg 4).</span><span class="sxs-lookup"><span data-stu-id="bff5e-200">Follow hello steps again tooadd a tile (starting with step 4).</span></span> <span data-ttu-id="bff5e-201">Den här tiden kan hello följande:</span><span class="sxs-lookup"><span data-stu-id="bff5e-201">This time, do hello following:</span></span>

    * <span data-ttu-id="bff5e-202">När du får för**visualiseringen typen**väljer **linjediagram**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-202">When you get too**Visualization Type**, select **Line chart**.</span></span> 
    * <span data-ttu-id="bff5e-203">Lägg till en axel och välj **windowend**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-203">Add an axis and select **windowend**.</span></span> 
    * <span data-ttu-id="bff5e-204">Lägg till ett värde och välj **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-204">Add a value and select **fraudulentcalls**.</span></span>
    * <span data-ttu-id="bff5e-205">För **tid fönstret toodisplay**väljer hello sista 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="bff5e-205">For **Time window toodisplay**, select hello last 10 minutes.</span></span>

    ![Skapa panelen för linjediagram](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. <span data-ttu-id="bff5e-207">Klicka på **nästa**, lägga till en rubrik och underrubrik och på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-207">Click **Next**, add a title and subtitle, and click **Apply**.</span></span>

    <span data-ttu-id="bff5e-208">hello Power BI-instrumentpanel nu får du två vyer av data om falska samtal som identifierats i hello strömmande data.</span><span class="sxs-lookup"><span data-stu-id="bff5e-208">hello Power BI dashboard now gives you two views of data about fraudulent calls as detected in hello streaming data.</span></span>

    ![Färdig Power BI-instrumentpanelen visar två paneler för bedrägliga anrop](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a><span data-ttu-id="bff5e-210">Ta reda på med om Power BI</span><span class="sxs-lookup"><span data-stu-id="bff5e-210">Learn more about Power BI</span></span>

<span data-ttu-id="bff5e-211">Den här kursen visar hur toocreate endast några typer av visualiseringar för en dataset.</span><span class="sxs-lookup"><span data-stu-id="bff5e-211">This tutorial demonstrates how toocreate only a few kinds of visualizations for a dataset.</span></span> <span data-ttu-id="bff5e-212">Med hjälp av Powerbi kan du skapa andra kunden business intelligence-verktyg för din organisation.</span><span class="sxs-lookup"><span data-stu-id="bff5e-212">Power BI can help you create other customer business intelligence tools for your organization.</span></span> <span data-ttu-id="bff5e-213">Fler idéer finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="bff5e-213">For more ideas, see hello following resources:</span></span>

* <span data-ttu-id="bff5e-214">Ett annat exempel på en Power BI-instrumentpanel titta i hello [komma igång med Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span><span class="sxs-lookup"><span data-stu-id="bff5e-214">For another example of a Power BI dashboard, watch hello [Getting Started with Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span></span>
* <span data-ttu-id="bff5e-215">Mer information om hur du konfigurerar Streaming Analytics-jobbet utdata tooPower BI och använder Power BI-grupper granska hello [Power BI](stream-analytics-define-outputs.md#power-bi) avsnitt i hello [Stream Analytics matar ut](stream-analytics-define-outputs.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="bff5e-215">For more information about configuring Streaming Analytics job output tooPower BI and using Power BI groups, review hello [Power BI](stream-analytics-define-outputs.md#power-bi) section of hello [Stream Analytics outputs](stream-analytics-define-outputs.md) article.</span></span> 
* <span data-ttu-id="bff5e-216">Information om hur du använder Power BI vanligtvis finns [instrumentpaneler i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span><span class="sxs-lookup"><span data-stu-id="bff5e-216">For information about using Power BI generally, see [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span></span>


## <a name="learn-about-limitations-and-best-practices"></a><span data-ttu-id="bff5e-217">Lär dig mer om metodtips och begränsningar</span><span class="sxs-lookup"><span data-stu-id="bff5e-217">Learn about limitations and best practices</span></span>
<span data-ttu-id="bff5e-218">Power BI kan för närvarande anropas ungefär en gång per sekund.</span><span class="sxs-lookup"><span data-stu-id="bff5e-218">Currently, Power BI can be called roughly once per second.</span></span> <span data-ttu-id="bff5e-219">Strömmande visuell information stöder paket på 15 KB.</span><span class="sxs-lookup"><span data-stu-id="bff5e-219">Streaming visuals support packets of 15 KB.</span></span> <span data-ttu-id="bff5e-220">Strömmande visuell information misslyckas utöver det kan (men push fortsätter toowork).</span><span class="sxs-lookup"><span data-stu-id="bff5e-220">Beyond that, streaming visuals fail (but push continues toowork).</span></span> <span data-ttu-id="bff5e-221">På grund av dessa begränsningar lämpar Power BI sig bäst naturligt toocases där Azure Stream Analytics har viktiga data belastningen minska.</span><span class="sxs-lookup"><span data-stu-id="bff5e-221">Because of these limitations, Power BI lends itself most naturally toocases where Azure Stream Analytics does a significant data load reduction.</span></span> <span data-ttu-id="bff5e-222">Vi rekommenderar att du använder en rullande fönster eller Hopping fönstret tooensure som data-push är som mest en push per sekund och som frågan hamnar inom hello genomströmning krav.</span><span class="sxs-lookup"><span data-stu-id="bff5e-222">We recommend using a Tumbling window or Hopping window tooensure that data push is at most one push per second, and that your query lands within hello throughput requirements.</span></span>

<span data-ttu-id="bff5e-223">Du kan använda följande formel toocompute hello värdet toogive hello fönstret i sekunder:</span><span class="sxs-lookup"><span data-stu-id="bff5e-223">You can use hello following equation toocompute hello value toogive your window in seconds:</span></span>

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

<span data-ttu-id="bff5e-225">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bff5e-225">For example:</span></span>

* <span data-ttu-id="bff5e-226">Du har 1 000 enheter som skickar data med en sekunds intervall.</span><span class="sxs-lookup"><span data-stu-id="bff5e-226">You have 1,000 devices sending data at one-second intervals.</span></span>
* <span data-ttu-id="bff5e-227">Du använder hello Power BI Pro SKU som har stöd för 1 000 000 rader per timme.</span><span class="sxs-lookup"><span data-stu-id="bff5e-227">You are using hello Power BI Pro SKU that supports 1,000,000 rows per hour.</span></span>
* <span data-ttu-id="bff5e-228">Vill du toopublish hello mängden genomsnittlig data per enhet tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="bff5e-228">You want toopublish hello amount of average data per device tooPower BI.</span></span>

<span data-ttu-id="bff5e-229">Därför blir hello formel:</span><span class="sxs-lookup"><span data-stu-id="bff5e-229">As a result, hello equation becomes:</span></span>

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

<span data-ttu-id="bff5e-231">Med den här konfigurationen kan du ändra hello ursprungliga frågan toohello följande:</span><span class="sxs-lookup"><span data-stu-id="bff5e-231">Given this configuration, you can change hello original query toohello following:</span></span>

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a><span data-ttu-id="bff5e-232">Förnya auktorisering</span><span class="sxs-lookup"><span data-stu-id="bff5e-232">Renew authorization</span></span>
<span data-ttu-id="bff5e-233">Om hello lösenord har ändrats sedan jobbet skapades eller senast autentiserad, måste tooreauthenticate Power BI-konto.</span><span class="sxs-lookup"><span data-stu-id="bff5e-233">If hello password has changed since your job was created or last authenticated, you need tooreauthenticate your Power BI account.</span></span> <span data-ttu-id="bff5e-234">Om Azure Multi-Factor Authentication har konfigurerats på din Azure Active Directory (Azure AD)-klient, måste du också toorenew Power BI-auktorisering varannan vecka.</span><span class="sxs-lookup"><span data-stu-id="bff5e-234">If Azure Multi-Factor Authentication is configured on your Azure Active Directory (Azure AD) tenant, you also need toorenew Power BI authorization every two weeks.</span></span> <span data-ttu-id="bff5e-235">Om du inte förnyar kan du se problem, till exempel brist på jobbutdata eller en `Authenticate user error` i hello åtgärdsloggar.</span><span class="sxs-lookup"><span data-stu-id="bff5e-235">If you don't renew, you could see symptoms such as a lack of job output or an `Authenticate user error` in hello operation logs.</span></span>

<span data-ttu-id="bff5e-236">Om ett jobb startar efter hello token har upphört att gälla, uppstår ett fel och hello jobb misslyckas.</span><span class="sxs-lookup"><span data-stu-id="bff5e-236">Similarly, if a job starts after hello token has expired, an error occurs and hello job fails.</span></span> <span data-ttu-id="bff5e-237">tooresolve det här problemet stoppa hello jobb som körs och gå tooyour Power BI-utdata.</span><span class="sxs-lookup"><span data-stu-id="bff5e-237">tooresolve this issue, stop hello job that's running and go tooyour Power BI output.</span></span> <span data-ttu-id="bff5e-238">tooavoid dataförlust, Välj hello **förnya auktorisering** länka och starta sedan om jobbet från hello **stoppats senast**.</span><span class="sxs-lookup"><span data-stu-id="bff5e-238">tooavoid data loss, select hello **Renew authorization** link, and then restart your job from hello **Last Stopped Time**.</span></span>

<span data-ttu-id="bff5e-239">När hello auktorisering har uppdaterats med Power BI, visas en grön avisering i hello auktorisering området tooreflect att hello problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="bff5e-239">After hello authorization has been refreshed with Power BI, a green alert appears in hello authorization area tooreflect that hello issue has been resolved.</span></span>

## <a name="get-help"></a><span data-ttu-id="bff5e-240">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="bff5e-240">Get help</span></span>
<span data-ttu-id="bff5e-241">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="bff5e-241">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bff5e-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bff5e-242">Next steps</span></span>
* [<span data-ttu-id="bff5e-243">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bff5e-243">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="bff5e-244">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bff5e-244">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="bff5e-245">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="bff5e-245">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="bff5e-246">Azure Stream Analytics query language-referens</span><span class="sxs-lookup"><span data-stu-id="bff5e-246">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="bff5e-247">Azure Stream Analytics Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="bff5e-247">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
