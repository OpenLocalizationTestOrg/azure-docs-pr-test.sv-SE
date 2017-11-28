---
title: "aaaWeather prognos med data från IoT-hubb Azure Machine Learning | Microsoft Docs"
description: "Använd Azure Machine Learning toopredict hello risken för regn utifrån hello temperatur- och fuktighetskonsekvens data din IoT-hubb som samlar in från en sensor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "väder prognos machine learning"
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 04abe97558ccfc152bae2e0d435033433c0023dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="10152-104">Väder prognos med hello sensordata från IoT-hubb i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="10152-104">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="10152-106">Machine learning är en teknik för datavetenskap som hjälper datorer Läs från befintliga data tooforecast framtida beteenden, resultat och trender.</span><span class="sxs-lookup"><span data-stu-id="10152-106">Machine learning is a technique of data science that helps computers learn from existing data tooforecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="10152-107">Azure Machine Learning är en molntjänst för förutsägelseanalys som gör det möjligt tooquickly skapa och distribuera förutsägelsemodeller som Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="10152-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible tooquickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="10152-108">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="10152-108">What you learn</span></span>

<span data-ttu-id="10152-109">Du lär dig hur toouse Azure Machine Learning toodo väder prognos (risken för regn) med hjälp av hello temperatur- och fuktighetskonsekvens data från din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-109">You learn how toouse Azure Machine Learning toodo weather forecast (chance of rain) using hello temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="10152-110">hello risken för regn är hello utdata från en förberedd väder förutsägelse modell.</span><span class="sxs-lookup"><span data-stu-id="10152-110">hello chance of rain is hello output of a prepared weather prediction model.</span></span> <span data-ttu-id="10152-111">hello modellen bygger på historiska data tooforecast risken för regn baserat på temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="10152-111">hello model is built upon historic data tooforecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="10152-112">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="10152-112">What you do</span></span>

- <span data-ttu-id="10152-113">Distribuera hello väder förutsägelse modellen som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="10152-113">Deploy hello weather prediction model as a web service.</span></span>
- <span data-ttu-id="10152-114">Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="10152-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="10152-115">Skapa ett Stream Analytics-jobb och konfigurera hello jobbet:</span><span class="sxs-lookup"><span data-stu-id="10152-115">Create a Stream Analytics job and configure hello job to:</span></span>
  - <span data-ttu-id="10152-116">Läsa temperatur- och fuktighetskonsekvens data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="10152-117">Anropa hello web service tooget hello regn risken.</span><span class="sxs-lookup"><span data-stu-id="10152-117">Call hello web service tooget hello rain chance.</span></span>
  - <span data-ttu-id="10152-118">Spara hello resultatet tooan Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="10152-118">Save hello result tooan Azure blob storage.</span></span>
- <span data-ttu-id="10152-119">Använd Microsoft Azure Lagringsutforskaren tooview hello väderprognos.</span><span class="sxs-lookup"><span data-stu-id="10152-119">Use Microsoft Azure Storage Explorer tooview hello weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="10152-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="10152-120">What you need</span></span>

- <span data-ttu-id="10152-121">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="10152-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="10152-122">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="10152-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="10152-123">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="10152-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="10152-124">Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-124">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="10152-125">Ett konto i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="10152-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="10152-126">([Försök Machine Learning Studio gratis](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="10152-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="10152-127">Distribuera hello väder förutsägelse modellen som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="10152-127">Deploy hello weather prediction model as a web service</span></span>

1. <span data-ttu-id="10152-128">Gå toohello [väder förutsägelse modellen sidan](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="10152-128">Go toohello [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="10152-129">Klicka på **öppna i Studio** i Microsoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="10152-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="10152-130">![Öppna hello väder förutsägelse modellen sida i Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="10152-130">![Open hello weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="10152-131">Klicka på **kör** toovalidate hello steg i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="10152-131">Click **Run** toovalidate hello steps in hello model.</span></span> <span data-ttu-id="10152-132">Det här steget kan ta 2 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="10152-132">This step might take 2 minutes toocomplete.</span></span>
   <span data-ttu-id="10152-133">![Öppna hello väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="10152-133">![Open hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="10152-134">Klicka på **konfigurera WEBBTJÄNSTEN** > **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="10152-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="10152-135">![Distribuera hello väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="10152-135">![Deploy hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="10152-136">Dra i hello hello **Web service indata** modulen någonstans nära hello **Poängmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="10152-136">In hello diagram, drag hello **Web service input** module somewhere near hello **Score Model** module.</span></span>
1. <span data-ttu-id="10152-137">Ansluta hello **Web service indata** modulen toohello **Poängmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="10152-137">Connect hello **Web service input** module toohello **Score Model** module.</span></span>
   <span data-ttu-id="10152-138">![Ansluta två moduler i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="10152-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="10152-139">Klicka på **kör** toovalidate hello steg i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="10152-139">Click **RUN** toovalidate hello steps in hello model.</span></span>
1. <span data-ttu-id="10152-140">Klicka på **DISTRIBUERA WEBBTJÄNSTEN** toodeploy hello modellen som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="10152-140">Click **DEPLOY WEB SERVICE** toodeploy hello model as a web service.</span></span>
1. <span data-ttu-id="10152-141">Hämta hello på hello instrumentpanelen för hello modellen **Excel 2010 eller tidigare arbetsboken** för **frågor och svar**.</span><span class="sxs-lookup"><span data-stu-id="10152-141">On hello dashboard of hello model, download hello **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="10152-142">Se till att du hämtar hello **Excel 2010 eller tidigare arbetsboken** även om du kör en senare version av Excel på datorn.</span><span class="sxs-lookup"><span data-stu-id="10152-142">Ensure that you download hello **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Hämta hello Excel för hello REQUEST RESPONSE slutpunkt](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="10152-144">Öppna hello Excel-arbetsbok, anteckna hello **URL för WEBBTJÄNSTEN** och **ÅTKOMSTNYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="10152-144">Open hello Excel workbook, make a note of hello **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="10152-145">Skapa, konfigurera och köra ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="10152-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="10152-146">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="10152-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="10152-147">I hello [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="10152-147">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="10152-148">Ange följande information för jobbet hello hello.</span><span class="sxs-lookup"><span data-stu-id="10152-148">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="10152-149">**Jobbnamnet**: hello namnet på hello jobb.</span><span class="sxs-lookup"><span data-stu-id="10152-149">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="10152-150">hello namn måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="10152-150">hello name must be globally unique.</span></span>

   <span data-ttu-id="10152-151">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-151">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="10152-152">**Plats**: Använd hello samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="10152-152">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="10152-153">**PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="10152-153">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Skapa ett Stream Analytics-jobb i Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="10152-155">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="10152-155">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="10152-156">Lägg till ett inkommande toohello Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="10152-156">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="10152-157">Öppna hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="10152-157">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="10152-158">Under **jobbet topologi**, klickar du på **indata**.</span><span class="sxs-lookup"><span data-stu-id="10152-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="10152-159">I hello **indata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:</span><span class="sxs-lookup"><span data-stu-id="10152-159">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="10152-160">**Ett inmatat alias**: unika hello-alias för hello indata.</span><span class="sxs-lookup"><span data-stu-id="10152-160">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="10152-161">**Källan**: Välj **IoT-hubb**.</span><span class="sxs-lookup"><span data-stu-id="10152-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="10152-162">**Konsumentgrupp**: Välj hello konsumentgrupp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="10152-162">**Consumer group**: Select hello consumer group you created.</span></span>

   ![Lägg till ett inkommande toohello Stream Analytics-jobb i Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="10152-164">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="10152-164">Click **Create**.</span></span>

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="10152-165">Lägga till utdata toohello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="10152-165">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="10152-166">Under **jobbet topologi**, klickar du på **utdata**.</span><span class="sxs-lookup"><span data-stu-id="10152-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="10152-167">I hello **utdata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:</span><span class="sxs-lookup"><span data-stu-id="10152-167">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="10152-168">**Kolumnalias**: unikt hello-alias för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="10152-168">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="10152-169">**Sink**: Välj **Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="10152-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="10152-170">**Lagringskontot**: hello storage-konto för blobblagring.</span><span class="sxs-lookup"><span data-stu-id="10152-170">**Storage account**: hello storage account for your blob storage.</span></span> <span data-ttu-id="10152-171">Du kan skapa ett lagringskonto eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="10152-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="10152-172">**Behållaren**: hello behållaren där hello blob sparas.</span><span class="sxs-lookup"><span data-stu-id="10152-172">**Container**: hello container where hello blob is saved.</span></span> <span data-ttu-id="10152-173">Du kan skapa en behållare eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="10152-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="10152-174">**Händelsen serialiseringsformat**: Välj **CSV**.</span><span class="sxs-lookup"><span data-stu-id="10152-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Lägga till utdata toohello Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="10152-176">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="10152-176">Click **Create**.</span></span>

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a><span data-ttu-id="10152-177">Lägg till en funktion toohello Stream Analytics-jobbet toocall hello webbtjänst du distribuerat</span><span class="sxs-lookup"><span data-stu-id="10152-177">Add a function toohello Stream Analytics job toocall hello web service you deployed</span></span>

1. <span data-ttu-id="10152-178">Under **jobbet topologi**, klickar du på **funktioner** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="10152-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="10152-179">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="10152-179">Enter hello following information:</span></span>

   <span data-ttu-id="10152-180">**Funktionen Alias**: Ange `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="10152-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="10152-181">**Funktionen typen**: Välj **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="10152-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="10152-182">**Importera alternativet**: Välj **importera från en annan prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="10152-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="10152-183">**URL: en**: Ange hello WEBBTJÄNST-URL som du antecknade ned från hello Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="10152-183">**URL**: Enter hello WEB SERVICE URL that you noted down from hello Excel workbook.</span></span>

   <span data-ttu-id="10152-184">**Nyckeln**: Ange hello ÅTKOMSTNYCKEL som du antecknade ned från hello Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="10152-184">**Key**: Enter hello ACCESS KEY that you noted down from hello Excel workbook.</span></span>

   ![Lägg till en funktion toohello Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="10152-186">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="10152-186">Click **Create**.</span></span>

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="10152-187">Konfigurera hello frågan för hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="10152-187">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="10152-188">Under **jobbet topologi**, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="10152-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="10152-189">Ersätt hello befintlig kod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="10152-189">Replace hello existing code with hello following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="10152-190">Ersätt `[YourInputAlias]` med hello Ange alias för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="10152-190">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>

   <span data-ttu-id="10152-191">Ersätt `[YourOutputAlias]` med hello kolumnalias för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="10152-191">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>

1. <span data-ttu-id="10152-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="10152-192">Click **Save**.</span></span>

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="10152-193">Kör hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="10152-193">Run hello Stream Analytics job</span></span>

<span data-ttu-id="10152-194">Klicka på hello Stream Analytics-jobbet **starta** > **nu** > **starta**.</span><span class="sxs-lookup"><span data-stu-id="10152-194">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="10152-195">När hello jobbet har startar hello jobbets status har ändrats från **stoppad** för**kör**.</span><span class="sxs-lookup"><span data-stu-id="10152-195">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Kör hello Stream Analytics-jobbet](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a><span data-ttu-id="10152-197">Använd Microsoft Azure Lagringsutforskaren tooview hello väderprognos</span><span class="sxs-lookup"><span data-stu-id="10152-197">Use Microsoft Azure Storage Explorer tooview hello weather forecast</span></span>

<span data-ttu-id="10152-198">Kör hello klienten programmet toostart samla in och skicka temperatur- och fuktighetskonsekvens data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-198">Run hello client application toostart collecting and sending temperature and humidity data tooyour IoT hub.</span></span> <span data-ttu-id="10152-199">För varje meddelande som tar emot din IoT-hubb anropar hello Stream Analytics-jobbet hello väder prognos web service tooproduce hello risken för regn.</span><span class="sxs-lookup"><span data-stu-id="10152-199">For each message that your IoT hub receives, hello Stream Analytics job calls hello weather forecast web service tooproduce hello chance of rain.</span></span> <span data-ttu-id="10152-200">hello-resultatet sparas sedan tooyour Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="10152-200">hello result is then saved tooyour Azure blob storage.</span></span> <span data-ttu-id="10152-201">Azure Lagringsutforskaren är ett verktyg som du kan använda tooview hello resultat.</span><span class="sxs-lookup"><span data-stu-id="10152-201">Azure Storage Explorer is a tool that you can use tooview hello result.</span></span>

1. <span data-ttu-id="10152-202">[Hämta och installera Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="10152-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="10152-203">Öppna Utforskaren i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="10152-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="10152-204">Logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="10152-204">Sign in tooyour Azure account.</span></span>
1. <span data-ttu-id="10152-205">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="10152-205">Select your subscription.</span></span>
1. <span data-ttu-id="10152-206">Klicka på din prenumeration > **Lagringskonton** > ditt lagringskonto > **Blob-behållare** > din behållare.</span><span class="sxs-lookup"><span data-stu-id="10152-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="10152-207">Öppna ett CSV-filen toosee hello resultat.</span><span class="sxs-lookup"><span data-stu-id="10152-207">Open a .csv file toosee hello result.</span></span> <span data-ttu-id="10152-208">hello sista kolumnen poster hello risken för regn.</span><span class="sxs-lookup"><span data-stu-id="10152-208">hello last column records hello chance of rain.</span></span>

   ![Få väder prognos resultat med Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="10152-210">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="10152-210">Summary</span></span>

<span data-ttu-id="10152-211">Du har använt Azure Machine Learning tooproduce hello risken för regn baserat på hello temperatur- och fuktighetskonsekvens data som tar emot din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10152-211">You’ve successfully used Azure Machine Learning tooproduce hello chance of rain based on hello temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]