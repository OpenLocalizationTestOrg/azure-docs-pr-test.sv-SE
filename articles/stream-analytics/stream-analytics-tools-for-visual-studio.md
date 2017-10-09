---
title: "aaaUse Azure Stream Analytics Tools för Visual Studio | Microsoft Docs"
description: "Komma igång-kursen för hello Azure Stream Analytics Tools för Visual Studio"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a><span data-ttu-id="8e467-104">Använda Azure Stream Analytics-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e467-104">Use Azure Stream Analytics Tools for Visual Studio</span></span>
## <a name="introduction"></a><span data-ttu-id="8e467-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="8e467-105">Introduction</span></span>
<span data-ttu-id="8e467-106">I den här kursen lär du dig hur toouse Azure Stream Analytics-verktyg för Visual Studio toocreate, skapa, testa lokalt, hantera och felsöka Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="8e467-106">In this tutorial, you learn how toouse Azure Stream Analytics Tools for Visual Studio toocreate, author, test locally, manage, and debug your Stream Analytics jobs.</span></span> 

<span data-ttu-id="8e467-107">När du har slutfört den här självstudiekursen kommer du att kunna:</span><span class="sxs-lookup"><span data-stu-id="8e467-107">After completing this tutorial, you will be able to:</span></span>
* <span data-ttu-id="8e467-108">Bekanta dig med Stream Analytics-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e467-108">Familiarize yourself with Stream Analytics Tools for Visual Studio.</span></span>
* <span data-ttu-id="8e467-109">Konfigurera och distribuera ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="8e467-109">Configure and deploy a Stream Analytics job.</span></span>
* <span data-ttu-id="8e467-110">TESTJOBBET lokalt med lokala exempeldata.</span><span class="sxs-lookup"><span data-stu-id="8e467-110">Test your job locally with local sample data.</span></span>
* <span data-ttu-id="8e467-111">Använd övervakning tootroubleshoot problem.</span><span class="sxs-lookup"><span data-stu-id="8e467-111">Use monitoring tootroubleshoot issues.</span></span>
* <span data-ttu-id="8e467-112">Exportera befintliga jobb tooprojects.</span><span class="sxs-lookup"><span data-stu-id="8e467-112">Export existing jobs tooprojects.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e467-113">Krav</span><span class="sxs-lookup"><span data-stu-id="8e467-113">Prerequisites</span></span>
<span data-ttu-id="8e467-114">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="8e467-114">toocomplete this tutorial, you need hello following prerequisites:</span></span>
* <span data-ttu-id="8e467-115">Slutför hello steg som föregår ”skapa ett Stream Analytics-jobb” i hello [skapar en IoT-lösning med Stream Analytics-självstudier](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="8e467-115">Finish hello steps that precede "Create a Stream Analytics job" in hello [Build an IoT solution by using Stream Analytics tutorial](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span></span> 
* <span data-ttu-id="8e467-116">Använd Visual Studio 2015, Visual Studio 2013 update 4 eller Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8e467-116">Use Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012.</span></span> <span data-ttu-id="8e467-117">Enterprise (Ultimate/Premium), Professional och Community-versioner stöds.</span><span class="sxs-lookup"><span data-stu-id="8e467-117">Enterprise (Ultimate/Premium), Professional, and Community editions are supported.</span></span> <span data-ttu-id="8e467-118">Express edition stöds inte.</span><span class="sxs-lookup"><span data-stu-id="8e467-118">Express edition is not supported.</span></span> <span data-ttu-id="8e467-119">Visual Studio-2017 stöds inte.</span><span class="sxs-lookup"><span data-stu-id="8e467-119">Visual Studio 2017 is not supported.</span></span> 
* <span data-ttu-id="8e467-120">Använd hello Azure SDK för .NET version 2.7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8e467-120">Use hello Azure SDK for .NET version 2.7.1 or later.</span></span> <span data-ttu-id="8e467-121">Installera den med hjälp av hello [installationsprogram för webbplattform](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e467-121">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="8e467-122">Installera hello [Stream Analytics Tools för Visual Studio](http://aka.ms/asatoolsvs).</span><span class="sxs-lookup"><span data-stu-id="8e467-122">Install hello [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span></span>

## <a name="create-a-stream-analytics-project"></a><span data-ttu-id="8e467-123">Skapa ett Stream Analytics-projekt</span><span class="sxs-lookup"><span data-stu-id="8e467-123">Create a Stream Analytics project</span></span>
1. <span data-ttu-id="8e467-124">I Visual Studio klickar du på hello **filen** -menyn och välj **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e467-124">In Visual Studio, click hello **File** menu and select **New Project**.</span></span> 

2. <span data-ttu-id="8e467-125">Hello mallar hello vänster Markera i **Stream Analytics** och klicka sedan på **Azure Stream Analytics-programmet**.</span><span class="sxs-lookup"><span data-stu-id="8e467-125">In hello templates list on hello left, select **Stream Analytics** and then click **Azure Stream Analytics Application**.</span></span>

3. <span data-ttu-id="8e467-126">Ange hello projekt **namn**, **plats**, och **lösningsnamn** som du gör för andra projekt.</span><span class="sxs-lookup"><span data-stu-id="8e467-126">Enter hello project **Name**, **Location**, and **Solution name** as you do for other projects.</span></span>

    ![Nytt projekt-fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    <span data-ttu-id="8e467-128">En **avgift** projekt har genererats i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8e467-128">A **Toll** project is generated in **Solution Explorer**.</span></span>

    ![Avgift projektet genereras i Solution Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a><span data-ttu-id="8e467-130">Välj rätt hello-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8e467-130">Choose hello correct subscription</span></span>
1. <span data-ttu-id="8e467-131">I Visual Studio klickar du på hello **visa** -menyn och öppna **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8e467-131">In Visual Studio, click hello **View** menu and open **Server Explorer**.</span></span>

2. <span data-ttu-id="8e467-132">Logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8e467-132">Sign in with your Azure Account.</span></span> 

## <a name="define-hello-input-sources"></a><span data-ttu-id="8e467-133">Definiera hello indatakällor</span><span class="sxs-lookup"><span data-stu-id="8e467-133">Define hello input sources</span></span>
1.  <span data-ttu-id="8e467-134">I **Solution Explorer**, expandera hello **indata** nod och Byt namn på **Input.json** för**EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-134">In **Solution Explorer**, expand hello **Inputs** node and rename **Input.json** too**EntryStream.json**.</span></span> <span data-ttu-id="8e467-135">Dubbelklicka på **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-135">Double-click **EntryStream.json**.</span></span>
2.  <span data-ttu-id="8e467-136">Hej **indata Alias** är nu **EntryStream**.</span><span class="sxs-lookup"><span data-stu-id="8e467-136">hello **Input Alias** is now **EntryStream**.</span></span> <span data-ttu-id="8e467-137">Ange Hello-alias används i hello Frågeskript.</span><span class="sxs-lookup"><span data-stu-id="8e467-137">hello input alias is used in hello query script.</span></span> 
3.  <span data-ttu-id="8e467-138">I **källtypen**väljer **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="8e467-138">In **Source Type**, select **Data Stream**.</span></span>
4.  <span data-ttu-id="8e467-139">I **källa**väljer **Händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="8e467-139">In **Source**, select **Event Hub**.</span></span>
5.  <span data-ttu-id="8e467-140">I **Service Bus Namespace**väljer hello **TollData** alternativet.</span><span class="sxs-lookup"><span data-stu-id="8e467-140">In **Service Bus Namespace**, select hello **TollData** option.</span></span>
6.  <span data-ttu-id="8e467-141">I **Händelsehubbens namn**väljer **post**.</span><span class="sxs-lookup"><span data-stu-id="8e467-141">In **Event Hub Name**, select **entry**.</span></span>
7.  <span data-ttu-id="8e467-142">I **Event Hub principnamn**väljer **RootManageSharedAccessKey** (hello standardvärdet).</span><span class="sxs-lookup"><span data-stu-id="8e467-142">In **Event Hub Policy Name**, select **RootManageSharedAccessKey** (hello default value).</span></span>
8.  <span data-ttu-id="8e467-143">I **händelse serialiseringsformat**väljer **Json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-143">In **Event Serialization Format**, select **Json**.</span></span> 
9.  <span data-ttu-id="8e467-144">I **kodning**väljer **UTF8**.</span><span class="sxs-lookup"><span data-stu-id="8e467-144">In **Encoding**, select **UTF8**.</span></span> <span data-ttu-id="8e467-145">Inställningarna bör se ut som följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="8e467-145">Your settings should look like hello following screenshot:</span></span>

    ![Inkommande fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. <span data-ttu-id="8e467-147">toofinish hello guiden klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8e467-147">toofinish hello wizard, click **Save**.</span></span> <span data-ttu-id="8e467-148">Nu kan du lägga till en annan indatakälla toocreate hello avsluta dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="8e467-148">Now you can add another input source toocreate hello exit stream.</span></span> <span data-ttu-id="8e467-149">Högerklicka på hello **indata** och väljer **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8e467-149">Right-click hello **Inputs** node, and select **New Item**.</span></span>

    ![Nytt objekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. <span data-ttu-id="8e467-151">Välj hello fönstret i **Azure Stream Analytics indata**, och ändra hello **namn** för**ExitStream.json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-151">In hello window, select **Azure Stream Analytics Input**, and change hello **Name** too**ExitStream.json**.</span></span> <span data-ttu-id="8e467-152">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8e467-152">Click **Add**.</span></span>

    ![Lägg till nytt objekt-fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. <span data-ttu-id="8e467-154">Dubbelklicka på **ExitStream.json** i hello projektet och följ hello samma steg som du gjorde för hello post dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="8e467-154">Double-click **ExitStream.json** in hello project, and follow hello same steps as you did for hello entry stream.</span></span> <span data-ttu-id="8e467-155">Vara säker på att tooenter **avsluta** för hello **Händelsehubbens namn** som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="8e467-155">Be sure tooenter **exit** for hello **Event Hub Name** as shown in hello following screenshot:</span></span>

    ![ExitStream fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    <span data-ttu-id="8e467-157">Du har nu definierat två inkommande dataströmmar:</span><span class="sxs-lookup"><span data-stu-id="8e467-157">Now you have defined two input streams:</span></span>

    ![Ingång och utgång inkommande strömmar](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    <span data-ttu-id="8e467-159">Lägg till referensindata för hello blob-fil som innehåller bil registreringsdata.</span><span class="sxs-lookup"><span data-stu-id="8e467-159">Next, add reference data input for hello blob file that contains car registration data.</span></span>

13. <span data-ttu-id="8e467-160">Högerklicka på hello **indata** i hello projektet och följ hello samma steg som du gjorde för hello dataströmmen indata.</span><span class="sxs-lookup"><span data-stu-id="8e467-160">Right-click hello **Inputs** node in hello project, and then follow hello same steps as you did for hello stream inputs.</span></span> <span data-ttu-id="8e467-161">I **indata Alias**, ange **registrering**, och i **källtypen**väljer **referensdata**.</span><span class="sxs-lookup"><span data-stu-id="8e467-161">In **Input Alias**, enter **Registration**, and in **Source Type**, select **Reference data**.</span></span>

    ![Fönster för registrering](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. <span data-ttu-id="8e467-163">I **Lagringskonto**väljer hello **tolldata** alternativet.</span><span class="sxs-lookup"><span data-stu-id="8e467-163">In **Storage Account**, select hello **tolldata** option.</span></span> <span data-ttu-id="8e467-164">I **behållare**väljer **tolldata**, och i **sökvägar**, ange **registration.json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-164">In **Container**, select **tolldata**, and in **Path Pattern**, enter **registration.json**.</span></span> <span data-ttu-id="8e467-165">Det här namnet är skiftlägeskänsligt och måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="8e467-165">This file name is case sensitive and should be lowercase.</span></span>
15. <span data-ttu-id="8e467-166">toofinish hello guiden klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8e467-166">toofinish hello wizard, click **Save**.</span></span>

<span data-ttu-id="8e467-167">Nu har alla hello indata definierats.</span><span class="sxs-lookup"><span data-stu-id="8e467-167">Now all hello inputs are defined.</span></span>

## <a name="define-hello-output"></a><span data-ttu-id="8e467-168">Definiera hello-utdata</span><span class="sxs-lookup"><span data-stu-id="8e467-168">Define hello output</span></span>
1.  <span data-ttu-id="8e467-169">I **Solution Explorer**, expandera hello **indata** nod och dubbelklickar på **Output.json**.</span><span class="sxs-lookup"><span data-stu-id="8e467-169">In **Solution Explorer**, expand hello **Inputs** node and double-click **Output.json**.</span></span>

2.  <span data-ttu-id="8e467-170">I **Utdataalias**, ange **utdata**.</span><span class="sxs-lookup"><span data-stu-id="8e467-170">In **Output Alias**, enter **output**.</span></span> 
3.  <span data-ttu-id="8e467-171">I **Sink**väljer **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="8e467-171">In **Sink**, select **SQL Database**.</span></span>
4.  <span data-ttu-id="8e467-172">I **databasen**väljer **TollDataDB**.</span><span class="sxs-lookup"><span data-stu-id="8e467-172">In **Database**, select **TollDataDB**.</span></span>
5.  <span data-ttu-id="8e467-173">I **användarnamn**, ange **tolladmin**.</span><span class="sxs-lookup"><span data-stu-id="8e467-173">In **User Name**, enter **tolladmin**.</span></span> 
6.  <span data-ttu-id="8e467-174">I **lösenord**, ange **123toll!**.</span><span class="sxs-lookup"><span data-stu-id="8e467-174">In **Password**, enter **123toll!**.</span></span>
7.  <span data-ttu-id="8e467-175">I **tabell**, ange **TollDataRefJoin**.</span><span class="sxs-lookup"><span data-stu-id="8e467-175">In **Table**, enter **TollDataRefJoin**.</span></span>
8.  <span data-ttu-id="8e467-176">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8e467-176">Click **Save**.</span></span>

    ![Utdatafönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a><span data-ttu-id="8e467-178">Skapa en Stream Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="8e467-178">Create a Stream Analytics query</span></span>
<span data-ttu-id="8e467-179">Den här självstudiekursen försöker tooanswer flera verksamhetsrelaterade frågor som är relaterade tootoll data.</span><span class="sxs-lookup"><span data-stu-id="8e467-179">This tutorial attempts tooanswer several business questions that are related tootoll data.</span></span> <span data-ttu-id="8e467-180">Det skapar också Stream Analytics-frågor som kan användas i Stream Analytics tooprovide relevanta svar.</span><span class="sxs-lookup"><span data-stu-id="8e467-180">It also constructs Stream Analytics queries that can be used in Stream Analytics tooprovide relevant answers.</span></span>
<span data-ttu-id="8e467-181">Innan du börjar din första Stream Analytics-jobbet ska vi titta närmare ett enkelt scenario och hello frågesyntaxen.</span><span class="sxs-lookup"><span data-stu-id="8e467-181">Before you start your first Stream Analytics job, let’s explore a simple scenario and hello query syntax.</span></span>

### <a name="introduction-toohello-stream-analytics-query-language"></a><span data-ttu-id="8e467-182">Introduktion toohello Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="8e467-182">Introduction toohello Stream Analytics query language</span></span>
<span data-ttu-id="8e467-183">Anta att du behöver toocount hello antalet fordon som anger en avgift monter.</span><span class="sxs-lookup"><span data-stu-id="8e467-183">Let’s say that you need toocount hello number of vehicles that enter a toll booth.</span></span> <span data-ttu-id="8e467-184">Eftersom det här exemplet är en ständig ström med händelser kan ha toodefine en tid.</span><span class="sxs-lookup"><span data-stu-id="8e467-184">Because this example is a continuous stream of events, you have toodefine a period of time.</span></span> <span data-ttu-id="8e467-185">Ändra hello fråga toobe ”hur många fordon anger en avgift monter alla tre minuter”?</span><span class="sxs-lookup"><span data-stu-id="8e467-185">Modify hello question toobe “How many vehicles enter a toll booth every three minutes?”</span></span> <span data-ttu-id="8e467-186">Det här sättet toocount data är ofta kallas tooas hello rullande count.</span><span class="sxs-lookup"><span data-stu-id="8e467-186">This way toocount data is commonly referred tooas hello tumbling count.</span></span>

<span data-ttu-id="8e467-187">Titta på hello Stream Analytics-fråga som svar på den här frågan:</span><span class="sxs-lookup"><span data-stu-id="8e467-187">Look at hello Stream Analytics query that answers this question:</span></span>

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

<span data-ttu-id="8e467-188">Stream Analytics använder ett frågespråk som som SQL och lägger till några tillägg toospecify tidsrelaterade aspekter av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="8e467-188">Stream Analytics uses a query language that's like SQL and adds a few extensions toospecify time-related aspects of hello query.</span></span>

<span data-ttu-id="8e467-189">Mer information finns i [tidshantering](https://msdn.microsoft.com/library/azure/mt582045.aspx) och [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstruktioner som används i hello frågan från MSDN.</span><span class="sxs-lookup"><span data-stu-id="8e467-189">For more information, see [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructs used in hello query from MSDN.</span></span>

<span data-ttu-id="8e467-190">Nu när du har skrivit din första Stream Analytics-fråga, är det tid tootest den.</span><span class="sxs-lookup"><span data-stu-id="8e467-190">Now that you have written your first Stream Analytics query, it's time tootest it.</span></span> <span data-ttu-id="8e467-191">Använd hello exempeldatafiler finns i mappen TollApp i hello följande sökväg:</span><span class="sxs-lookup"><span data-stu-id="8e467-191">Use hello sample data files located in your TollApp folder in hello following path:</span></span>

<span data-ttu-id="8e467-192">.. \TollApp\TollApp\Data</span><span class="sxs-lookup"><span data-stu-id="8e467-192">..\TollApp\TollApp\Data</span></span>

<span data-ttu-id="8e467-193">Den här mappen innehåller hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="8e467-193">This folder contains hello following files:</span></span>
*   <span data-ttu-id="8e467-194">Entry.JSON</span><span class="sxs-lookup"><span data-stu-id="8e467-194">Entry.json</span></span>
*   <span data-ttu-id="8e467-195">Exit.JSON</span><span class="sxs-lookup"><span data-stu-id="8e467-195">Exit.json</span></span>
*   <span data-ttu-id="8e467-196">Registration.JSON</span><span class="sxs-lookup"><span data-stu-id="8e467-196">Registration.json</span></span>

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a><span data-ttu-id="8e467-197">Antal hello fordon att ange en avgift monter</span><span class="sxs-lookup"><span data-stu-id="8e467-197">Count hello number of vehicles entering a toll booth</span></span>
<span data-ttu-id="8e467-198">Dubbelklicka i hello-projektet **Script.asaql** tooopen hello skriptet i hello **frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="8e467-198">In hello project, double-click **Script.asaql** tooopen hello script in hello **Query Editor**.</span></span> <span data-ttu-id="8e467-199">Kopiera och klistra in hello skriptet i föregående avsnitt i hello hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="8e467-199">Copy and paste hello script in hello previous section into hello editor.</span></span> <span data-ttu-id="8e467-200">Hej frågeredigeraren stöder IntelliSense, syntax färgläggning och hello fel markör.</span><span class="sxs-lookup"><span data-stu-id="8e467-200">hello Query Editor supports IntelliSense, syntax coloring, and hello error marker.</span></span>

![Frågeredigeraren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a><span data-ttu-id="8e467-202">Testa lokalt Stream Analytics-frågor</span><span class="sxs-lookup"><span data-stu-id="8e467-202">Test Stream Analytics queries locally</span></span>

1. <span data-ttu-id="8e467-203">toocompile hello fråga toosee om det finns ett syntaxfel, högerklicka på hello projektet och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8e467-203">toocompile hello query toosee if there is a syntax error, right-click hello project and select **Build**.</span></span> 

2. <span data-ttu-id="8e467-204">toovalidate den här frågan mot exempeldata, kan du använda lokala exempeldata.</span><span class="sxs-lookup"><span data-stu-id="8e467-204">toovalidate this query against sample data, you can use local sample data.</span></span> <span data-ttu-id="8e467-205">Högerklicka på hello indata och välj **lägga till lokala indata**.</span><span class="sxs-lookup"><span data-stu-id="8e467-205">Right-click hello input, and select **Add local input**.</span></span>

    ![Lägga till lokala indata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. <span data-ttu-id="8e467-207">I hello popup-fönster, Välj hello exempeldata från din lokala sökväg.</span><span class="sxs-lookup"><span data-stu-id="8e467-207">In hello pop-up window, select hello sample data from your local path.</span></span> <span data-ttu-id="8e467-208">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8e467-208">Click **Save**.</span></span>

    ![Lägg till lokala inkommande fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    <span data-ttu-id="8e467-210">En fil med namnet **local_EntryStream.json** läggs till automatiskt tooyour indata mapp.</span><span class="sxs-lookup"><span data-stu-id="8e467-210">A file named **local_EntryStream.json** is automatically added tooyour inputs folder.</span></span>

    ![Tillagda tooinputs mapp](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. <span data-ttu-id="8e467-212">I hello **frågeredigeraren**, klickar du på **kör lokalt**.</span><span class="sxs-lookup"><span data-stu-id="8e467-212">In hello **Query Editor**, click **Run Locally**.</span></span> <span data-ttu-id="8e467-213">Eller du kan trycka på F5 för hello.</span><span class="sxs-lookup"><span data-stu-id="8e467-213">Or you can press hello F5 key.</span></span>

    ![Kör lokalt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Lokal körning utdata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    <span data-ttu-id="8e467-216">Tryck på några viktiga tooview hello utdata i hello **ASA lokala kör resultatet** fönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e467-216">Press any key tooview hello output in hello **ASA Local Run Result** window in Visual Studio.</span></span> 

    ![ASA lokala kör resultatfönstret](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. <span data-ttu-id="8e467-218">Klicka på **öppna resultatet mappen** toocheck hello utdatafiler både i csv- och JSON-format.</span><span class="sxs-lookup"><span data-stu-id="8e467-218">Click **Open Result Folder** toocheck hello output files both in CSV and JSON format.</span></span>

    ![Öppna mappen resultatet utdata](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a><span data-ttu-id="8e467-220">Exemplet hello-indata</span><span class="sxs-lookup"><span data-stu-id="8e467-220">Sample hello input data</span></span>
<span data-ttu-id="8e467-221">Du kan också inkommande exempeldata från indatakällor tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="8e467-221">You can also sample input data from input sources tooa local file.</span></span> 
1. <span data-ttu-id="8e467-222">Högerklicka på hello inkommande konfigurationsfilen och välj **exempeldata**.</span><span class="sxs-lookup"><span data-stu-id="8e467-222">Right-click hello input config file, and select **Sample Data**.</span></span> 

   ![Exempeldata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    <span data-ttu-id="8e467-224">Du kan prova endast händelsehubb eller IoT-hubb för tillfället.</span><span class="sxs-lookup"><span data-stu-id="8e467-224">You can sample only event hub or IoT hub for now.</span></span> <span data-ttu-id="8e467-225">Andra indatakällor stöds inte.</span><span class="sxs-lookup"><span data-stu-id="8e467-225">Other input sources are not supported.</span></span>

2. <span data-ttu-id="8e467-226">Ange hello lokal sökväg används toosave hello exempeldata i hello popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="8e467-226">In hello pop-up window, enter hello local path used toosave hello sample data.</span></span> <span data-ttu-id="8e467-227">Klicka på **exempel**.</span><span class="sxs-lookup"><span data-stu-id="8e467-227">Click **Sample**.</span></span>

    ![Exempel på fönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    <span data-ttu-id="8e467-229">Du kan se hello förlopp i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="8e467-229">You can see hello progress in hello **Output** window.</span></span> 

    ![Utdatafönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a><span data-ttu-id="8e467-231">Skicka ett Stream Analytics query tooAzure</span><span class="sxs-lookup"><span data-stu-id="8e467-231">Submit a Stream Analytics query tooAzure</span></span>
1. <span data-ttu-id="8e467-232">I hello **frågeredigeraren**, klickar du på **skicka tooAzure** i hello Skriptredigeraren.</span><span class="sxs-lookup"><span data-stu-id="8e467-232">In hello **Query Editor**, click **Submit tooAzure** in hello script editor.</span></span>

    ![Skicka tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. <span data-ttu-id="8e467-234">Välj **skapa en ny Azure Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="8e467-234">Select **Create a New Azure Stream Analytics Job**.</span></span> <span data-ttu-id="8e467-235">Ange hello **jobbnamn**, och välj rätt hello **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="8e467-235">Enter hello **Job Name**, and select hello correct **Subscription**.</span></span> <span data-ttu-id="8e467-236">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="8e467-236">Click **Submit**.</span></span>

    ![Skicka jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a><span data-ttu-id="8e467-238">Starta ett jobb</span><span class="sxs-lookup"><span data-stu-id="8e467-238">Start a job</span></span>
<span data-ttu-id="8e467-239">Nu när jobbet har skapats, öppnas automatiskt hello jobbet vyn.</span><span class="sxs-lookup"><span data-stu-id="8e467-239">Now that your job is created, hello job view is automatically opened.</span></span> 
1. <span data-ttu-id="8e467-240">toostart hello jobbet, klickar du på hello **grön pil** knappen.</span><span class="sxs-lookup"><span data-stu-id="8e467-240">toostart hello job, click hello **green arrow** button.</span></span>

    ![Starta ett jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. <span data-ttu-id="8e467-242">Välj hello standardinställningen och klicka på **starta**.</span><span class="sxs-lookup"><span data-stu-id="8e467-242">Select hello default setting, and click **Start**.</span></span>
 
    ![Starta jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    <span data-ttu-id="8e467-244">hello jobbet **Status** ändras för**kör**, och **indata händelser** och **Utdatahändelserna** visas.</span><span class="sxs-lookup"><span data-stu-id="8e467-244">hello job **Status** changes too**Running**, and **Input Events** and **Output Events** appear.</span></span>

    ![Kör status i jobbsammanfattning](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a><span data-ttu-id="8e467-246">Resultat av hello i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e467-246">Check hello results in Visual Studio</span></span>
1. <span data-ttu-id="8e467-247">Öppna i Visual Studio **Server Explorer** och högerklicka på hello **TollDataRefJoin** tabell.</span><span class="sxs-lookup"><span data-stu-id="8e467-247">In Visual Studio, open **Server Explorer** and right-click hello **TollDataRefJoin** table.</span></span>
2. <span data-ttu-id="8e467-248">Välj **visa tabelldata** toosee hello utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="8e467-248">Select **Show Table Data** toosee hello output of your job.</span></span>
   
    ![Val av Visa tabelldata i Server Explorer](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a><span data-ttu-id="8e467-250">Visa hello jobbet mått</span><span class="sxs-lookup"><span data-stu-id="8e467-250">View hello job metrics</span></span>
<span data-ttu-id="8e467-251">Vissa grundläggande jobbet statistik finns i **jobbet mått**.</span><span class="sxs-lookup"><span data-stu-id="8e467-251">Some basic job statistics can be found in **Job Metrics**.</span></span> 

![Jobbet mått fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a><span data-ttu-id="8e467-253">Lista hello jobbet i Server Explorer</span><span class="sxs-lookup"><span data-stu-id="8e467-253">List hello job in Server Explorer</span></span>
<span data-ttu-id="8e467-254">I **Server Explorer**, klickar du på **Stream Analytics-jobb** och klicka sedan på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="8e467-254">In **Server Explorer**, click **Stream Analytics Jobs** and then click **Refresh**.</span></span> <span data-ttu-id="8e467-255">hello jobb visas under **Stream Analytics-jobb**.</span><span class="sxs-lookup"><span data-stu-id="8e467-255">hello job appears under **Stream Analytics jobs**.</span></span>

![Stream Analytics-jobb som finns i Server Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a><span data-ttu-id="8e467-257">Öppna hello jobbet vy</span><span class="sxs-lookup"><span data-stu-id="8e467-257">Open hello job view</span></span>
<span data-ttu-id="8e467-258">tooopen hello jobbet vyn, expandera jobbnoden och dubbelklicka på hello **jobbet** nod.</span><span class="sxs-lookup"><span data-stu-id="8e467-258">tooopen hello job view, expand your job node and double-click hello **Job View** node.</span></span>

![Jobbet nod](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a><span data-ttu-id="8e467-260">Exportera ett befintligt jobb tooa projekt</span><span class="sxs-lookup"><span data-stu-id="8e467-260">Export an existing job tooa project</span></span>
<span data-ttu-id="8e467-261">Det finns två sätt som du kan exportera ett befintligt jobb tooa projekt.</span><span class="sxs-lookup"><span data-stu-id="8e467-261">There are two ways you can export an existing job tooa project.</span></span>

<span data-ttu-id="8e467-262">I **Server Explorer**, högerklicka på hello jobbet nod i hello **Stream Analytics-jobb** och välj **exportera tooNew Stream Analytics projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e467-262">In **Server Explorer**, right-click hello job node in hello **Stream Analytics Jobs** node and select **Export tooNew Stream Analytics Project**.</span></span>

![Exportera tooNew Stream Analytics-projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

<span data-ttu-id="8e467-264">hello-projektet har genererats i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8e467-264">hello project is generated in **Solution Explorer**.</span></span>

![Projektet har skapats i Solution Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
<span data-ttu-id="8e467-266">Du också kan använda hello jobbet vyn och klicka på **generera projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e467-266">You also can use hello job view, and click **Generate Project**.</span></span>

![Skapa projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a><span data-ttu-id="8e467-268">Kända problem och begränsningar</span><span class="sxs-lookup"><span data-stu-id="8e467-268">Known issues and limitations</span></span>
 
- <span data-ttu-id="8e467-269">Det finns inget stöd för Power BI-utdata och Azure datum Datasjölager utdata.</span><span class="sxs-lookup"><span data-stu-id="8e467-269">There is no support for Power BI output and Azure Date Lake Store output.</span></span>
- <span data-ttu-id="8e467-270">Det finns inget stöd för Redigeraren för att lägga till eller ändra JavaScript användardefinierade funktioner.</span><span class="sxs-lookup"><span data-stu-id="8e467-270">There is no editor support for adding or changing JavaScript user-defined functions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e467-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e467-271">Next steps</span></span>
* [<span data-ttu-id="8e467-272">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e467-272">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8e467-273">Kom igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8e467-273">Get started by using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8e467-274">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8e467-274">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8e467-275">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="8e467-275">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8e467-276">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="8e467-276">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
