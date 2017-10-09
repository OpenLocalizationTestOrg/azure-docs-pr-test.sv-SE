---
title: aaaAzure integration Stream Analytics och Machine Learning | Microsoft Docs
description: "Hur toouse en användardefinierad funktion och Machine Learning i ett Stream Analytics-jobb"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a><span data-ttu-id="338e0-103">Utföra sentiment analys med hjälp av Azure Stream Analytics och Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="338e0-103">Performing sentiment analysis by using Azure Stream Analytics and Azure Machine Learning</span></span>
<span data-ttu-id="338e0-104">Den här artikeln beskriver hur tooquickly inställt på ett enkelt Azure Stream Analytics-jobb som integrerar Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="338e0-104">This article describes how tooquickly set up a simple Azure Stream Analytics job that integrates Azure Machine Learning.</span></span> <span data-ttu-id="338e0-105">Du använder en Machine Learning sentiment analytics modell från hello Cortana Intelligence Gallery tooanalyze strömmande textdata och fastställa hello sentiment poäng i realtid.</span><span class="sxs-lookup"><span data-stu-id="338e0-105">You use a Machine Learning sentiment analytics model from hello Cortana Intelligence Gallery tooanalyze streaming text data and determine hello sentiment score in real time.</span></span> <span data-ttu-id="338e0-106">Med hello Cortana Intelligence Suite kan du utföra den här uppgiften utan att oroa hello krångligheter för att skapa en sentiment analytics modell.</span><span class="sxs-lookup"><span data-stu-id="338e0-106">Using hello Cortana Intelligence Suite lets you accomplish this task without worrying about hello intricacies of building a sentiment analytics model.</span></span>

<span data-ttu-id="338e0-107">Du kan använda det du lära dig från den här artikeln tooscenarios som dessa:</span><span class="sxs-lookup"><span data-stu-id="338e0-107">You can apply what you learn from this article tooscenarios such as these:</span></span>

* <span data-ttu-id="338e0-108">Analysera realtid sentiment direktuppspelning Twitter-data.</span><span class="sxs-lookup"><span data-stu-id="338e0-108">Analyzing real-time sentiment on streaming Twitter data.</span></span>
* <span data-ttu-id="338e0-109">Analysera poster kunden chattar med supportpersonalen.</span><span class="sxs-lookup"><span data-stu-id="338e0-109">Analyzing records of customer chats with support staff.</span></span>
* <span data-ttu-id="338e0-110">Utvärderar kommentarer om forum, bloggar och videor.</span><span class="sxs-lookup"><span data-stu-id="338e0-110">Evaluating comments on forums, blogs, and videos.</span></span> 
* <span data-ttu-id="338e0-111">Många andra realtid, förutsägbara bedömningsprofil scenarier.</span><span class="sxs-lookup"><span data-stu-id="338e0-111">Many other real-time, predictive scoring scenarios.</span></span>

<span data-ttu-id="338e0-112">Du skulle få hello data direkt från en Twitter-dataström i ett verkligt scenario.</span><span class="sxs-lookup"><span data-stu-id="338e0-112">In a real-world scenario, you would get hello data directly from a Twitter data stream.</span></span> <span data-ttu-id="338e0-113">toosimplify hello självstudiekursen kommer vi har skrivit det så att hello Streaming Analytics-jobbet hämtar tweets från en CSV-fil i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="338e0-113">toosimplify hello tutorial, we've written it so that hello Streaming Analytics job gets tweets from a CSV file in Azure Blob storage.</span></span> <span data-ttu-id="338e0-114">Du kan skapa egna CSV-fil eller du kan använda en exempel-CSV-fil som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="338e0-114">You can create your own CSV file, or you can use a sample CSV file, as shown in hello following image:</span></span>

![exempel tweets i en CSV-fil](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

<span data-ttu-id="338e0-116">hello Streaming Analytics-jobbet som du skapar gäller hello sentiment analytics modellen som en användardefinierad funktion (UDF) på hello text exempeldata från hello blobstore.</span><span class="sxs-lookup"><span data-stu-id="338e0-116">hello Streaming Analytics job that you create applies hello sentiment analytics model as a user-defined function (UDF) on hello sample text data from hello blob store.</span></span> <span data-ttu-id="338e0-117">hello (hello resultatet av hello sentiment analys) skrivs toohello samma blobstore i en annan CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="338e0-117">hello output (hello result of hello sentiment analysis) is written toohello same blob store in a different CSV file.</span></span> 

<span data-ttu-id="338e0-118">hello visar följande bild den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="338e0-118">hello following figure demonstrates this configuration.</span></span> <span data-ttu-id="338e0-119">Som anges för en mer realistisk scenario ersätta du blob-lagring med streaming Twitter-data från Azure Event Hubs indata.</span><span class="sxs-lookup"><span data-stu-id="338e0-119">As noted, for a more realistic scenario, you can replace blob storage with streaming Twitter data from an Azure Event Hubs input.</span></span> <span data-ttu-id="338e0-120">Dessutom kan du bygga en [Microsoft Power BI](https://powerbi.microsoft.com/) realtid visualisering av hello sammanställd sentiment.</span><span class="sxs-lookup"><span data-stu-id="338e0-120">Additionally, you could build a [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualization of hello aggregate sentiment.</span></span>    

![Stream Analytics Machine Learning-integration: översikt](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a><span data-ttu-id="338e0-122">Krav</span><span class="sxs-lookup"><span data-stu-id="338e0-122">Prerequisites</span></span>
<span data-ttu-id="338e0-123">Innan du börjar bör du kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="338e0-123">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="338e0-124">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="338e0-124">An active Azure subscription.</span></span>
* <span data-ttu-id="338e0-125">En CSV-fil med vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="338e0-125">A CSV file with some data in it.</span></span> <span data-ttu-id="338e0-126">Du kan hämta hello-filen som visades tidigare från [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), eller skapa en egen fil.</span><span class="sxs-lookup"><span data-stu-id="338e0-126">You can download hello file shown earlier from [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), or you can create your own file.</span></span> <span data-ttu-id="338e0-127">I den här artikeln förutsätter att du använder hello-filen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="338e0-127">For this article, we assume that you're using hello file from GitHub.</span></span>

<span data-ttu-id="338e0-128">På en hög nivå toocomplete hello aktiviteter visas i den här artikeln får du hello följande:</span><span class="sxs-lookup"><span data-stu-id="338e0-128">At a high level, toocomplete hello tasks demonstrated in this article, you do hello following:</span></span>

1. <span data-ttu-id="338e0-129">Skapa ett Azure storage-konto och en behållare för blob storage och överför en CSV-formaterad indatafilen toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="338e0-129">Create an Azure storage account and a blob storage container, and upload a CSV-formatted input file toohello container.</span></span>
3. <span data-ttu-id="338e0-130">Lägga till en sentiment analytics modell från hello Cortana Intelligence Gallery tooyour Azure Machine Learning-arbetsytan och distribuera den här modellen som en webbtjänst i hello Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="338e0-130">Add a sentiment analytics model from hello Cortana Intelligence Gallery tooyour Azure Machine Learning workspace and deploy this model as a web service in hello Machine Learning workspace.</span></span>
5. <span data-ttu-id="338e0-131">Skapa ett Stream Analytics-jobb som anropar den här webbtjänsten som en funktion i ordning toodetermine sentiment för hello textinmatning.</span><span class="sxs-lookup"><span data-stu-id="338e0-131">Create a Stream Analytics job that calls this web service as a function in order toodetermine sentiment for hello text input.</span></span>
6. <span data-ttu-id="338e0-132">Starta hello Stream Analytics-jobbet och kontrollera hello utdata.</span><span class="sxs-lookup"><span data-stu-id="338e0-132">Start hello Stream Analytics job and check hello output.</span></span>

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a><span data-ttu-id="338e0-133">Skapa en lagringsbehållare och överför inkommande hello CSV-fil</span><span class="sxs-lookup"><span data-stu-id="338e0-133">Create a storage container and upload hello CSV input file</span></span>
<span data-ttu-id="338e0-134">Du kan använda en CSV-fil, till exempel hello en tillgänglig från GitHub för det här steget.</span><span class="sxs-lookup"><span data-stu-id="338e0-134">For this step, you can use any CSV file, such as hello one available from GitHub.</span></span>

1. <span data-ttu-id="338e0-135">I hello Azure-portalen klickar du på **ny** &gt; **lagring** &gt; **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="338e0-135">In hello Azure portal, click **New** &gt; **Storage** &gt; **Storage account**.</span></span>

   ![Skapa nytt lagringskonto](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. <span data-ttu-id="338e0-137">Ange ett namn (`samldemo` i hello exemplet).</span><span class="sxs-lookup"><span data-stu-id="338e0-137">Provide a name (`samldemo` in hello example).</span></span> <span data-ttu-id="338e0-138">hello namn kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="338e0-138">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 

3. <span data-ttu-id="338e0-139">Ange en befintlig resursgrupp och ange en plats.</span><span class="sxs-lookup"><span data-stu-id="338e0-139">Specify an existing resource group and specify a location.</span></span> <span data-ttu-id="338e0-140">Plats, rekommenderar vi att alla hello resurser som skapats i denna självstudiekurs Använd hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="338e0-140">For location, we recommend that all hello resources created in this tutorial use hello same location.</span></span>

    ![Ange information om lagringskonto](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. <span data-ttu-id="338e0-142">Välj hello storage-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="338e0-142">In hello Azure portal, select hello storage account.</span></span> <span data-ttu-id="338e0-143">I hello lagring-kontoblad klickar du på **behållare** och klicka sedan på  **+ &nbsp;behållare** toocreate blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="338e0-143">In hello storage account blade, click **Containers** and then click **+&nbsp;Container** toocreate blob storage.</span></span>

    ![Skapa blob-behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. <span data-ttu-id="338e0-145">Ange ett namn för hello behållare (`azuresamldemoblob` i hello exempel) och kontrollera att **komma åt typen** har angetts för**Blob**.</span><span class="sxs-lookup"><span data-stu-id="338e0-145">Provide a name for hello container (`azuresamldemoblob` in hello example) and verify that **Access type** is set too**Blob**.</span></span> <span data-ttu-id="338e0-146">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="338e0-146">When you're done, click **OK**.</span></span>

    ![Ange information om blob-behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. <span data-ttu-id="338e0-148">I hello **behållare** bladet, Välj hello ny behållare, vilket öppnar bladet hello för behållaren.</span><span class="sxs-lookup"><span data-stu-id="338e0-148">In hello **Containers** blade, select hello new container, which opens hello blade for that container.</span></span>

7. <span data-ttu-id="338e0-149">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="338e0-149">Click **Upload**.</span></span>

    ![Överför knapp för en behållare](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. <span data-ttu-id="338e0-151">I hello **överför blob** bladet ange hello CSV-fil som du vill toouse för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="338e0-151">In hello **Upload blob** blade, specify hello CSV file that you want toouse for this tutorial.</span></span> <span data-ttu-id="338e0-152">För **Blob typen**väljer **blockblob** och ange hello blockera storlek too4 MB, vilket är tillräcklig för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="338e0-152">For **Blob type**, select **Block blob** and set hello block size too4 MB, which is sufficient for this tutorial.</span></span>

    ![Överför blob-fil](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. <span data-ttu-id="338e0-154">Klicka på hello **överför** knappen längst ned hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="338e0-154">Click hello **Upload** button at hello bottom of hello blade.</span></span>

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a><span data-ttu-id="338e0-155">Lägga till hello sentiment analytics modell från hello Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="338e0-155">Add hello sentiment analytics model from hello Cortana Intelligence Gallery</span></span>

<span data-ttu-id="338e0-156">Nu när hello exempeldata i en blob, kan du aktivera hello sentiment analysmodell i Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="338e0-156">Now that hello sample data is in a blob, you can enable hello sentiment analysis model in Cortana Intelligence Gallery.</span></span>

1. <span data-ttu-id="338e0-157">Gå toohello [förutsägande sentiment analytics modellen](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) sida i hello Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="338e0-157">Go toohello [predictive sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) page in hello Cortana Intelligence Gallery.</span></span>  

2. <span data-ttu-id="338e0-158">Klicka på **öppna i Studio**.</span><span class="sxs-lookup"><span data-stu-id="338e0-158">Click **Open in Studio**.</span></span>  
   
   ![Strömma Analytics Machine Learning, öppna Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. <span data-ttu-id="338e0-160">Logga in toogo toohello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="338e0-160">Sign in toogo toohello workspace.</span></span> <span data-ttu-id="338e0-161">Välj en plats.</span><span class="sxs-lookup"><span data-stu-id="338e0-161">Select a location.</span></span>

4. <span data-ttu-id="338e0-162">Klicka på **kör** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="338e0-162">Click **Run** at hello bottom of hello page.</span></span> <span data-ttu-id="338e0-163">hello processen körs som tar ungefär en minut.</span><span class="sxs-lookup"><span data-stu-id="338e0-163">hello process runs, which takes about a minute.</span></span>

   ![Kör experiment i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. <span data-ttu-id="338e0-165">När hello processen har körts, Välj **distribuera webbtjänsten** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="338e0-165">After hello process has run successfully, select **Deploy Web Service** at hello bottom of hello page.</span></span>

   ![distribuera experiment i Machine Learning Studio som en webbtjänst](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. <span data-ttu-id="338e0-167">toovalidate som hello sentiment analytics modellen är klar toouse klickar du på hello **Test** knappen.</span><span class="sxs-lookup"><span data-stu-id="338e0-167">toovalidate that hello sentiment analytics model is ready toouse, click hello **Test** button.</span></span> <span data-ttu-id="338e0-168">Ange text som indata som ”jag älskar Microsoft”.</span><span class="sxs-lookup"><span data-stu-id="338e0-168">Provide text input such as "I love Microsoft".</span></span> 

   ![Testa experiment i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    <span data-ttu-id="338e0-170">Om hello test fungerar, visas en liknande toohello resultat som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="338e0-170">If hello test works, you see a result similar toohello following example:</span></span>

   ![testresultaten i Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. <span data-ttu-id="338e0-172">I hello **appar** kolumn, och klicka på hello **Excel 2010 eller tidigare arbetsboken** länk toodownload en Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="338e0-172">In hello **Apps** column, click hello **Excel 2010 or earlier workbook** link toodownload an Excel workbook.</span></span> <span data-ttu-id="338e0-173">hello arbetsboken innehåller hello en API-nyckeln och hello-URL som du behöver senare tooset in hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="338e0-173">hello workbook contains hello an API key and hello URL that you need later tooset up hello Stream Analytics job.</span></span>

    ![Stream Analytics Machine Learning, snabböversikten](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a><span data-ttu-id="338e0-175">Skapa ett Stream Analytics-jobb som använder hello Machine Learning-modellen</span><span class="sxs-lookup"><span data-stu-id="338e0-175">Create a Stream Analytics job that uses hello Machine Learning model</span></span>

<span data-ttu-id="338e0-176">Du kan nu skapa ett Stream Analytics-jobb som läser hello exempel tweets från hello CSV-fil i blob storage.</span><span class="sxs-lookup"><span data-stu-id="338e0-176">You can now create a Stream Analytics job that reads hello sample tweets from hello CSV file in blob storage.</span></span> 

### <a name="create-hello-job"></a><span data-ttu-id="338e0-177">Skapa hello jobb</span><span class="sxs-lookup"><span data-stu-id="338e0-177">Create hello job</span></span>

1. <span data-ttu-id="338e0-178">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="338e0-178">Go toohello [Azure portal](https://portal.azure.com).</span></span>  

2. <span data-ttu-id="338e0-179">Klicka på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="338e0-179">Click **New** > **Internet of Things** > **Stream Analytics job**.</span></span> 

   ![Azure portal sökvägen för att hämta tooa nya Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. <span data-ttu-id="338e0-181">Hello jobb `azure-sa-ml-demo`, ange en prenumeration, ange en befintlig resursgrupp eller skapa en ny och välj hello plats för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="338e0-181">Name hello job `azure-sa-ml-demo`, specify a subscription, specify an existing resource group or create a new one, and select hello location for hello job.</span></span>

   ![Ange inställningar för nya Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a><span data-ttu-id="338e0-183">Konfigurera hello jobbet indata</span><span class="sxs-lookup"><span data-stu-id="338e0-183">Configure hello job input</span></span>
<span data-ttu-id="338e0-184">hello jobbet hämtar indata från hello CSV-fil som du överfört tidigare tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="338e0-184">hello job gets its input from hello CSV file that you uploaded earlier tooblob storage.</span></span>

1. <span data-ttu-id="338e0-185">När hello jobbet har skapats under **jobbet topologi** i hello jobb-bladet, klickar du på hello **indata** rutan.</span><span class="sxs-lookup"><span data-stu-id="338e0-185">After hello job has been created, under **Job Topology** in hello job blade, click hello **Inputs** box.</span></span>  
   
   !['Indata-rutan i Stream Analytics-jobbet bladet](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. <span data-ttu-id="338e0-187">I hello **indata** bladet, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="338e0-187">In hello **Inputs** blade, click **+ Add**.</span></span>

   ![”Knappen Lägg till' för att lägga till ett inkommande toohello Stream Analytics-jobb](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. <span data-ttu-id="338e0-189">Fyll i hello **nya indata** bladet med dessa värden:</span><span class="sxs-lookup"><span data-stu-id="338e0-189">Fill out hello **New input** blade with these values:</span></span>

    * <span data-ttu-id="338e0-190">**Ett inmatat alias**: Använd hello namn `datainput`.</span><span class="sxs-lookup"><span data-stu-id="338e0-190">**Input alias**: Use hello name `datainput`.</span></span>
    * <span data-ttu-id="338e0-191">**Typ av datakälla**: Välj **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="338e0-191">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="338e0-192">**Källan**: Välj **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="338e0-192">**Source**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="338e0-193">**Importera alternativet**: Välj **använda blob storage från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="338e0-193">**Import option**: Select **Use blob storage from current subscription**.</span></span> 
    * <span data-ttu-id="338e0-194">**Lagringskontot**.</span><span class="sxs-lookup"><span data-stu-id="338e0-194">**Storage account**.</span></span> <span data-ttu-id="338e0-195">Välj hello storage-konto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="338e0-195">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="338e0-196">**Behållaren**.</span><span class="sxs-lookup"><span data-stu-id="338e0-196">**Container**.</span></span> <span data-ttu-id="338e0-197">Välj hello-behållare som du skapade tidigare (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="338e0-197">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="338e0-198">**Händelsen serialiseringsformat**.</span><span class="sxs-lookup"><span data-stu-id="338e0-198">**Event serialization format**.</span></span> <span data-ttu-id="338e0-199">Välj **CSV**.</span><span class="sxs-lookup"><span data-stu-id="338e0-199">Select **CSV**.</span></span>

    ![Inställningar för nya jobb indata](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. <span data-ttu-id="338e0-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="338e0-201">Click **Create**.</span></span>

### <a name="configure-hello-job-output"></a><span data-ttu-id="338e0-202">Konfigurera hello jobbutdata</span><span class="sxs-lookup"><span data-stu-id="338e0-202">Configure hello job output</span></span>
<span data-ttu-id="338e0-203">hello jobbet skickar resultaten toohello samma blob storage där den hämtar indata.</span><span class="sxs-lookup"><span data-stu-id="338e0-203">hello job sends results toohello same blob storage where it gets input.</span></span> 

1. <span data-ttu-id="338e0-204">Under **jobbet topologi** i hello jobb-bladet, klickar du på hello **utdata** rutan.</span><span class="sxs-lookup"><span data-stu-id="338e0-204">Under **Job Topology** in hello job blade, click hello **Outputs** box.</span></span>  
  
   ![Skapa nya utdata för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. <span data-ttu-id="338e0-206">I hello **utdata** bladet, klickar du på **+ Lägg till**, och sedan lägga till utdata med hello-alias `datamloutput`.</span><span class="sxs-lookup"><span data-stu-id="338e0-206">In hello **Outputs** blade, click **+ Add**, and then add an output with hello alias `datamloutput`.</span></span> 

3. <span data-ttu-id="338e0-207">För **Sink**väljer **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="338e0-207">For **Sink**, select **Blob storage**.</span></span> <span data-ttu-id="338e0-208">Fyller i hello resten av hello utdata inställningar med hjälp av hello samma värden som du använde för hello blob-lagring för indata:</span><span class="sxs-lookup"><span data-stu-id="338e0-208">Then fill in hello rest of hello output settings using hello same values that you used for hello blob storage for input:</span></span>

    * <span data-ttu-id="338e0-209">**Lagringskontot**.</span><span class="sxs-lookup"><span data-stu-id="338e0-209">**Storage account**.</span></span> <span data-ttu-id="338e0-210">Välj hello storage-konto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="338e0-210">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="338e0-211">**Behållaren**.</span><span class="sxs-lookup"><span data-stu-id="338e0-211">**Container**.</span></span> <span data-ttu-id="338e0-212">Välj hello-behållare som du skapade tidigare (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="338e0-212">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="338e0-213">**Händelsen serialiseringsformat**.</span><span class="sxs-lookup"><span data-stu-id="338e0-213">**Event serialization format**.</span></span> <span data-ttu-id="338e0-214">Välj **CSV**.</span><span class="sxs-lookup"><span data-stu-id="338e0-214">Select **CSV**.</span></span>

   ![Inställningar för nya jobbutdata](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. <span data-ttu-id="338e0-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="338e0-216">Click **Create**.</span></span>   


### <a name="add-hello-machine-learning-function"></a><span data-ttu-id="338e0-217">Lägg till hello Machine Learning-funktion</span><span class="sxs-lookup"><span data-stu-id="338e0-217">Add hello Machine Learning function</span></span> 
<span data-ttu-id="338e0-218">Tidigare du har publicerat en webbtjänsten för Machine Learning modellen tooa.</span><span class="sxs-lookup"><span data-stu-id="338e0-218">Earlier you published a Machine Learning model tooa web service.</span></span> <span data-ttu-id="338e0-219">I vårt scenario, när hello dataströmmen analys jobbet körs, skickas varje prov tweet från hello inkommande toohello webbtjänst för sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="338e0-219">In our scenario, when hello Stream Analysis job runs, it sends each sample tweet from hello input toohello web service for sentiment analysis.</span></span> <span data-ttu-id="338e0-220">hello Machine Learning-webbtjänst returnerar en sentiment (`positive`, `neutral`, eller `negative`) och sannolikhet hello tweet vara positivt.</span><span class="sxs-lookup"><span data-stu-id="338e0-220">hello Machine Learning web service returns a sentiment (`positive`, `neutral`, or `negative`) and a probability of hello tweet being positive.</span></span> 

<span data-ttu-id="338e0-221">I det här avsnittet av kursen hello definierar du en funktion i hello dataströmmen analys jobbet.</span><span class="sxs-lookup"><span data-stu-id="338e0-221">In this section of hello tutorial, you define a function in hello Stream Analysis job.</span></span> <span data-ttu-id="338e0-222">hello-funktionen kan anropade toosend en tweet toohello webbtjänst och komma hello svar.</span><span class="sxs-lookup"><span data-stu-id="338e0-222">hello function can be invoked toosend a tweet toohello web service and get hello response back.</span></span> 

1. <span data-ttu-id="338e0-223">Kontrollera att du har hello web service URL: en och API-nyckeln som du hämtade tidigare i hello Excel-arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="338e0-223">Make sure you have hello web service URL and API key that you downloaded earlier in hello Excel workbook.</span></span>

2. <span data-ttu-id="338e0-224">Returnera toohello jobbet översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="338e0-224">Return toohello job overview blade.</span></span>

3. <span data-ttu-id="338e0-225">Under **inställningar**väljer **funktioner** och klicka sedan på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="338e0-225">Under **Settings**, select **Functions** and then click **+ Add**.</span></span>

   ![Lägga till en funktion toohello Stream Analytics-jobb](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. <span data-ttu-id="338e0-227">Ange `sentiment` som hello fungera alias och fylla i hello resten av hello blad med hjälp av dessa värden:</span><span class="sxs-lookup"><span data-stu-id="338e0-227">Enter `sentiment` as hello function alias and fill out hello rest of hello blade using these values:</span></span>

    * <span data-ttu-id="338e0-228">**Funktionen typen**: Välj **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="338e0-228">**Function type**: Select **Azure ML**.</span></span>
    * <span data-ttu-id="338e0-229">**Importera alternativet**: Välj **importera från en annan prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="338e0-229">**Import option**: Select **Import from a different subscription**.</span></span> <span data-ttu-id="338e0-230">Detta ger dig en möjlighet tooenter hello URL och en nyckel.</span><span class="sxs-lookup"><span data-stu-id="338e0-230">This gives you a chance tooenter hello URL and key.</span></span>
    * <span data-ttu-id="338e0-231">**URL: en**: klistra in hello webbtjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="338e0-231">**URL**: Paste in hello web service URL.</span></span>
    * <span data-ttu-id="338e0-232">**Nyckeln**: klistra in hello API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="338e0-232">**Key**: Paste in hello API key.</span></span>
  
    ![Inställningar för att lägga till en Machine Learning-funktionen toohello Stream Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. <span data-ttu-id="338e0-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="338e0-234">Click **Create**.</span></span>

### <a name="create-a-query-tootransform-hello-data"></a><span data-ttu-id="338e0-235">Skapa en fråga tootransform hello data</span><span class="sxs-lookup"><span data-stu-id="338e0-235">Create a query tootransform hello data</span></span>

<span data-ttu-id="338e0-236">Stream Analytics använder en deklarativ, SQL-baserade frågan tooexamine hello indata och bearbetar den.</span><span class="sxs-lookup"><span data-stu-id="338e0-236">Stream Analytics uses a declarative, SQL-based query tooexamine hello input and process it.</span></span> <span data-ttu-id="338e0-237">I det här avsnittet skapar du en fråga som läser varje tweet från indata och anropar sedan hello Machine Learning-funktionen tooperform sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="338e0-237">In this section, you create a query that reads each tweet from input and then invokes hello Machine Learning function tooperform sentiment analysis.</span></span> <span data-ttu-id="338e0-238">hello frågan skickar sedan hello resultatet toohello utdata du definierat (blobblagring).</span><span class="sxs-lookup"><span data-stu-id="338e0-238">hello query then sends hello result toohello output that you defined (blob storage).</span></span>

1. <span data-ttu-id="338e0-239">Returnera toohello jobbet översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="338e0-239">Return toohello job overview blade.</span></span>

2.  <span data-ttu-id="338e0-240">Under **jobbet topologi**, klicka på hello **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="338e0-240">Under **Job Topology**, click hello **Query** box.</span></span>

    ![Skapa en fråga för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. <span data-ttu-id="338e0-242">Ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="338e0-242">Enter hello following query:</span></span>

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    <span data-ttu-id="338e0-243">hello frågan anropar hello-funktionen som du skapade tidigare (`sentiment`) i ordning tooperform sentiment analys på varje tweet i hello indata.</span><span class="sxs-lookup"><span data-stu-id="338e0-243">hello query invokes hello function you created earlier (`sentiment`) in order tooperform sentiment analysis on each tweet in hello input.</span></span> 

4. <span data-ttu-id="338e0-244">Klicka på **spara** toosave hello frågan.</span><span class="sxs-lookup"><span data-stu-id="338e0-244">Click **Save** toosave hello query.</span></span>


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a><span data-ttu-id="338e0-245">Starta hello Stream Analytics-jobbet och kontrollera hello-utdata</span><span class="sxs-lookup"><span data-stu-id="338e0-245">Start hello Stream Analytics job and check hello output</span></span>

<span data-ttu-id="338e0-246">Du kan nu starta hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="338e0-246">You can now start hello Stream Analytics job.</span></span>

### <a name="start-hello-job"></a><span data-ttu-id="338e0-247">Starta hello jobbet</span><span class="sxs-lookup"><span data-stu-id="338e0-247">Start hello job</span></span>
1. <span data-ttu-id="338e0-248">Returnera toohello jobbet översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="338e0-248">Return toohello job overview blade.</span></span>

2. <span data-ttu-id="338e0-249">Klicka på **starta** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="338e0-249">Click **Start** at hello top of hello blade.</span></span>

    ![Skapa en fråga för Streaming Analytics-jobbet](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. <span data-ttu-id="338e0-251">I hello **startjobb**väljer **anpassade**, och välj sedan en dag tidigare toowhen du överförde hello CSV tooblob fillagring.</span><span class="sxs-lookup"><span data-stu-id="338e0-251">In hello **Start job**, select **Custom**, and then select one day prior toowhen you uploaded hello CSV file tooblob storage.</span></span> <span data-ttu-id="338e0-252">När du är klar klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="338e0-252">When you're done, click **Start**.</span></span>  


### <a name="check-hello-output"></a><span data-ttu-id="338e0-253">Kontrollera hello-utdata</span><span class="sxs-lookup"><span data-stu-id="338e0-253">Check hello output</span></span>
1. <span data-ttu-id="338e0-254">Låt hello-jobbet körs i några minuter tills du ser aktivitet i hello **övervakning** rutan.</span><span class="sxs-lookup"><span data-stu-id="338e0-254">Let hello job run for a few minutes until you see activity in hello **Monitoring** box.</span></span> 

2. <span data-ttu-id="338e0-255">Om du har ett verktyg som du vanligtvis använder tooexamine hello innehållet i blob-lagring kan använda den verktyget tooexamine hello `azuresamldemoblob` behållare.</span><span class="sxs-lookup"><span data-stu-id="338e0-255">If you have a tool that you normally use tooexamine hello contents of blob storage, use that tool tooexamine hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="338e0-256">Du kan också hello följande i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="338e0-256">Alternatively, do hello following steps in hello Azure portal:</span></span>

    1. <span data-ttu-id="338e0-257">Hitta hello i hello portal `samldemo` lagring kontot och hitta hello i hello konto `azuresamldemoblob` behållare.</span><span class="sxs-lookup"><span data-stu-id="338e0-257">In hello portal, find hello `samldemo` storage account, and within hello account, find hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="338e0-258">Du ser två filer i hello behållare: hello-fil som innehåller hello exempel tweets och en CSV-fil som genererats av hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="338e0-258">You see two files in hello container: hello file that contains hello sample tweets and a CSV file generated by hello Stream Analytics job.</span></span>
    2. <span data-ttu-id="338e0-259">Högerklicka på filen hello genereras och välj sedan **hämta**.</span><span class="sxs-lookup"><span data-stu-id="338e0-259">Right-click hello generated file and then select **Download**.</span></span> 

   ![Ladda ned CSV jobbutdata från Blob storage](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. <span data-ttu-id="338e0-261">Öppna hello genereras CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="338e0-261">Open hello generated CSV file.</span></span> <span data-ttu-id="338e0-262">Du ser något som liknar följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="338e0-262">You see something like hello following example:</span></span>  
   
   ![Visa Stream Analytics, Machine Learning, CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a><span data-ttu-id="338e0-264">Visa mått</span><span class="sxs-lookup"><span data-stu-id="338e0-264">View metrics</span></span>
<span data-ttu-id="338e0-265">Du kan också visa Azure Machine Learning-funktionen-relaterade mått.</span><span class="sxs-lookup"><span data-stu-id="338e0-265">You also can view Azure Machine Learning function-related metrics.</span></span> <span data-ttu-id="338e0-266">hello följande funktion-relaterade mått visas i hello **övervakning** rutan hello jobb-bladet:</span><span class="sxs-lookup"><span data-stu-id="338e0-266">hello following function-related metrics are displayed in hello **Monitoring** box in hello job blade:</span></span>

* <span data-ttu-id="338e0-267">**Funktionen begäranden** anger hello antal förfrågningar som skickas tooa Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="338e0-267">**Function Requests** indicates hello number of requests sent tooa Machine Learning web service.</span></span>  
* <span data-ttu-id="338e0-268">**Funktionen händelser** anger hello antalet händelser i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="338e0-268">**Function Events** indicates hello number of events in hello request.</span></span> <span data-ttu-id="338e0-269">Som standard varje begäran tooa Machine Learning-webbtjänst som innehåller too1 000 händelser.</span><span class="sxs-lookup"><span data-stu-id="338e0-269">By default, each request tooa Machine Learning web service contains up too1,000 events.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="338e0-270">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="338e0-270">Next steps</span></span>

* [<span data-ttu-id="338e0-271">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="338e0-271">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="338e0-272">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="338e0-272">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="338e0-273">Integrera REST-API och Maskininlärning</span><span class="sxs-lookup"><span data-stu-id="338e0-273">Integrate REST API and Machine Learning</span></span>](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [<span data-ttu-id="338e0-274">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="338e0-274">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)



