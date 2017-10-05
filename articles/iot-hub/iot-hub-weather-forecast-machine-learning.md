---
title: "Väder prognos med data från IoT-hubb Azure Machine Learning | Microsoft Docs"
description: "Använd Azure Machine Learning att förutsäga risken för regn baserat på din IoT-hubb som samlar in från en sensor temperatur- och fuktighetskonsekvens data."
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
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="6047f-104">Väder prognos använder sensordata från IoT-hubb i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6047f-104">Weather forecast using the sensor data from your IoT hub in Azure Machine Learning</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="6047f-106">Machine learning är en teknik för datavetenskap som hjälper datorer lära sig från befintliga data att förutsäga framtida beteenden, resultat och trender.</span><span class="sxs-lookup"><span data-stu-id="6047f-106">Machine learning is a technique of data science that helps computers learn from existing data to forecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="6047f-107">Azure Machine Learning är en molnbaserad tjänst för förutsägelseanalys som gör det möjligt att snabbt skapa och distribuera förutsägelsemodeller som analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="6047f-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible to quickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="6047f-108">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="6047f-108">What you learn</span></span>

<span data-ttu-id="6047f-109">Du lär dig hur du använder Azure Machine Learning att väder prognos (risken för regn) med hjälp av temperatur- och fuktighetskonsekvens data från Azure IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="6047f-109">You learn how to use Azure Machine Learning to do weather forecast (chance of rain) using the temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="6047f-110">Risken för regn är resultatet av en modell för förberedda väder förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="6047f-110">The chance of rain is the output of a prepared weather prediction model.</span></span> <span data-ttu-id="6047f-111">Modellen bygger på historiska data för att bedöma risken för regn baserat på temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="6047f-111">The model is built upon historic data to forecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="6047f-112">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="6047f-112">What you do</span></span>

- <span data-ttu-id="6047f-113">Distribuera väder förutsägelse modellen som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6047f-113">Deploy the weather prediction model as a web service.</span></span>
- <span data-ttu-id="6047f-114">Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="6047f-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="6047f-115">Skapa ett Stream Analytics-jobb och konfigurera jobbet är:</span><span class="sxs-lookup"><span data-stu-id="6047f-115">Create a Stream Analytics job and configure the job to:</span></span>
  - <span data-ttu-id="6047f-116">Läsa temperatur- och fuktighetskonsekvens data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6047f-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="6047f-117">Anropa webbtjänsten för att få regn risken.</span><span class="sxs-lookup"><span data-stu-id="6047f-117">Call the web service to get the rain chance.</span></span>
  - <span data-ttu-id="6047f-118">Spara resultatet till en Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6047f-118">Save the result to an Azure blob storage.</span></span>
- <span data-ttu-id="6047f-119">Använda Microsoft Azure Lagringsutforskaren för att visa väder prognos.</span><span class="sxs-lookup"><span data-stu-id="6047f-119">Use Microsoft Azure Storage Explorer to view the weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6047f-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="6047f-120">What you need</span></span>

- <span data-ttu-id="6047f-121">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar följande krav:</span><span class="sxs-lookup"><span data-stu-id="6047f-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="6047f-122">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6047f-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="6047f-123">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6047f-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="6047f-124">Ett klientprogram som skickar meddelanden till din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6047f-124">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="6047f-125">Ett konto i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="6047f-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="6047f-126">([Försök Machine Learning Studio gratis](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="6047f-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="6047f-127">Distribuera väder förutsägelse modellen som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6047f-127">Deploy the weather prediction model as a web service</span></span>

1. <span data-ttu-id="6047f-128">Gå till den [väder förutsägelse modellen sidan](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="6047f-128">Go to the [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="6047f-129">Klicka på **öppna i Studio** i Microsoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="6047f-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="6047f-130">![Öppna sidan väder förutsägelse modellen i Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="6047f-130">![Open the weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="6047f-131">Klicka på **kör** att validera stegen i modellen.</span><span class="sxs-lookup"><span data-stu-id="6047f-131">Click **Run** to validate the steps in the model.</span></span> <span data-ttu-id="6047f-132">Det här steget kan ta 2 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="6047f-132">This step might take 2 minutes to complete.</span></span>
   <span data-ttu-id="6047f-133">![Öppna väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="6047f-133">![Open the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="6047f-134">Klicka på **konfigurera WEBBTJÄNSTEN** > **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="6047f-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="6047f-135">![Distribuera väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="6047f-135">![Deploy the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="6047f-136">Dra i den den **Web service indata** modulen någonstans nära den **Poängmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="6047f-136">In the diagram, drag the **Web service input** module somewhere near the **Score Model** module.</span></span>
1. <span data-ttu-id="6047f-137">Ansluta den **Web service indata** modulen till den **Poängmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="6047f-137">Connect the **Web service input** module to the **Score Model** module.</span></span>
   <span data-ttu-id="6047f-138">![Ansluta två moduler i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="6047f-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="6047f-139">Klicka på **kör** att validera stegen i modellen.</span><span class="sxs-lookup"><span data-stu-id="6047f-139">Click **RUN** to validate the steps in the model.</span></span>
1. <span data-ttu-id="6047f-140">Klicka på **DISTRIBUERA WEBBTJÄNSTEN** att distribuera modellen som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6047f-140">Click **DEPLOY WEB SERVICE** to deploy the model as a web service.</span></span>
1. <span data-ttu-id="6047f-141">På instrumentpanelen i modellen, ladda ned den **Excel 2010 eller tidigare arbetsboken** för **frågor och svar**.</span><span class="sxs-lookup"><span data-stu-id="6047f-141">On the dashboard of the model, download the **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="6047f-142">Se till att du hämtar den **Excel 2010 eller tidigare arbetsboken** även om du kör en senare version av Excel på datorn.</span><span class="sxs-lookup"><span data-stu-id="6047f-142">Ensure that you download the **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Hämta Excel för REQUEST RESPONSE-slutpunkt](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="6047f-144">Öppna Excel-arbetsboken, notera den **URL för WEBBTJÄNSTEN** och **ÅTKOMSTNYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="6047f-144">Open the Excel workbook, make a note of the **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="6047f-145">Skapa, konfigurera och köra ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="6047f-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="6047f-146">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="6047f-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="6047f-147">I den [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="6047f-147">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="6047f-148">Ange följande information för jobbet.</span><span class="sxs-lookup"><span data-stu-id="6047f-148">Enter the following information for the job.</span></span>

   <span data-ttu-id="6047f-149">**Jobbnamnet**: namnet på jobbet.</span><span class="sxs-lookup"><span data-stu-id="6047f-149">**Job name**: The name of the job.</span></span> <span data-ttu-id="6047f-150">Namnet måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="6047f-150">The name must be globally unique.</span></span>

   <span data-ttu-id="6047f-151">**Resursgruppen**: använda samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6047f-151">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="6047f-152">**Plats**: använda samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6047f-152">**Location**: Use the same location as your resource group.</span></span>

   <span data-ttu-id="6047f-153">**Fäst på instrumentpanelen**: Markera det här alternativet för enkel åtkomst till din IoT-hubb från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6047f-153">**Pin to dashboard**: Check this option for easy access to your IoT hub from the dashboard.</span></span>

   ![Skapa ett Stream Analytics-jobb i Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="6047f-155">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6047f-155">Click **Create**.</span></span>

### <a name="add-an-input-to-the-stream-analytics-job"></a><span data-ttu-id="6047f-156">Lägga till indata till Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="6047f-156">Add an input to the Stream Analytics job</span></span>

1. <span data-ttu-id="6047f-157">Öppna Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="6047f-157">Open the Stream Analytics job.</span></span>
1. <span data-ttu-id="6047f-158">Under **jobbet topologi**, klickar du på **indata**.</span><span class="sxs-lookup"><span data-stu-id="6047f-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="6047f-159">I den **indata** rutan klickar du på **Lägg till**, och ange följande information:</span><span class="sxs-lookup"><span data-stu-id="6047f-159">In the **Inputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="6047f-160">**Ett inmatat alias**: unika alias för indata.</span><span class="sxs-lookup"><span data-stu-id="6047f-160">**Input alias**: The unique alias for the input.</span></span>

   <span data-ttu-id="6047f-161">**Källan**: Välj **IoT-hubb**.</span><span class="sxs-lookup"><span data-stu-id="6047f-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="6047f-162">**Konsumentgrupp**: Välj konsumentgrupp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="6047f-162">**Consumer group**: Select the consumer group you created.</span></span>

   ![Lägga till indata till Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="6047f-164">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6047f-164">Click **Create**.</span></span>

### <a name="add-an-output-to-the-stream-analytics-job"></a><span data-ttu-id="6047f-165">Lägga till utdata till Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="6047f-165">Add an output to the Stream Analytics job</span></span>

1. <span data-ttu-id="6047f-166">Under **jobbet topologi**, klickar du på **utdata**.</span><span class="sxs-lookup"><span data-stu-id="6047f-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="6047f-167">I den **utdata** rutan klickar du på **Lägg till**, och ange följande information:</span><span class="sxs-lookup"><span data-stu-id="6047f-167">In the **Outputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="6047f-168">**Kolumnalias**: unika alias för utdata.</span><span class="sxs-lookup"><span data-stu-id="6047f-168">**Output alias**: The unique alias for the output.</span></span>

   <span data-ttu-id="6047f-169">**Sink**: Välj **Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="6047f-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="6047f-170">**Lagringskontot**: storage-konto för blobblagring.</span><span class="sxs-lookup"><span data-stu-id="6047f-170">**Storage account**: The storage account for your blob storage.</span></span> <span data-ttu-id="6047f-171">Du kan skapa ett lagringskonto eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="6047f-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="6047f-172">**Behållaren**: behållaren där blob sparas.</span><span class="sxs-lookup"><span data-stu-id="6047f-172">**Container**: The container where the blob is saved.</span></span> <span data-ttu-id="6047f-173">Du kan skapa en behållare eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="6047f-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="6047f-174">**Händelsen serialiseringsformat**: Välj **CSV**.</span><span class="sxs-lookup"><span data-stu-id="6047f-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Lägga till utdata till Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="6047f-176">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6047f-176">Click **Create**.</span></span>

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a><span data-ttu-id="6047f-177">Lägga till en funktion i Stream Analytics-jobbet för att anropa webbtjänsten som du distribuerat</span><span class="sxs-lookup"><span data-stu-id="6047f-177">Add a function to the Stream Analytics job to call the web service you deployed</span></span>

1. <span data-ttu-id="6047f-178">Under **jobbet topologi**, klickar du på **funktioner** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6047f-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="6047f-179">Ange följande information:</span><span class="sxs-lookup"><span data-stu-id="6047f-179">Enter the following information:</span></span>

   <span data-ttu-id="6047f-180">**Funktionen Alias**: Ange `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="6047f-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="6047f-181">**Funktionen typen**: Välj **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="6047f-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="6047f-182">**Importera alternativet**: Välj **importera från en annan prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="6047f-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="6047f-183">**URL: en**: Ange URL för WEBBTJÄNSTEN som du antecknade ned från Excel-arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="6047f-183">**URL**: Enter the WEB SERVICE URL that you noted down from the Excel workbook.</span></span>

   <span data-ttu-id="6047f-184">**Nyckeln**: Ange ÅTKOMSTNYCKEL som du antecknade ned från Excel-arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="6047f-184">**Key**: Enter the ACCESS KEY that you noted down from the Excel workbook.</span></span>

   ![Lägga till en funktion i Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="6047f-186">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6047f-186">Click **Create**.</span></span>

### <a name="configure-the-query-of-the-stream-analytics-job"></a><span data-ttu-id="6047f-187">Konfigurera frågan i Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="6047f-187">Configure the query of the Stream Analytics job</span></span>

1. <span data-ttu-id="6047f-188">Under **jobbet topologi**, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="6047f-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="6047f-189">Ersätt den befintliga koden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="6047f-189">Replace the existing code with the following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="6047f-190">Ersätt `[YourInputAlias]` med indata alias för jobbet.</span><span class="sxs-lookup"><span data-stu-id="6047f-190">Replace `[YourInputAlias]` with the input alias of the job.</span></span>

   <span data-ttu-id="6047f-191">Ersätt `[YourOutputAlias]` med utdataalias för jobbet.</span><span class="sxs-lookup"><span data-stu-id="6047f-191">Replace `[YourOutputAlias]` with the output alias of the job.</span></span>

1. <span data-ttu-id="6047f-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6047f-192">Click **Save**.</span></span>

### <a name="run-the-stream-analytics-job"></a><span data-ttu-id="6047f-193">Kör Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="6047f-193">Run the Stream Analytics job</span></span>

<span data-ttu-id="6047f-194">Klicka på i Stream Analytics-jobbet **starta** > **nu** > **starta**.</span><span class="sxs-lookup"><span data-stu-id="6047f-194">In the Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="6047f-195">När jobbet har startar jobbets status har ändrats från **stoppad** till **kör**.</span><span class="sxs-lookup"><span data-stu-id="6047f-195">Once the job successfully starts, the job status changes from **Stopped** to **Running**.</span></span>

![Kör Stream Analytics-jobbet](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a><span data-ttu-id="6047f-197">Använda Microsoft Azure Lagringsutforskaren för att visa väder prognos</span><span class="sxs-lookup"><span data-stu-id="6047f-197">Use Microsoft Azure Storage Explorer to view the weather forecast</span></span>

<span data-ttu-id="6047f-198">Köra klientprogram att börja samla in och skicka temperatur- och fuktighetskonsekvens data till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6047f-198">Run the client application to start collecting and sending temperature and humidity data to your IoT hub.</span></span> <span data-ttu-id="6047f-199">För varje meddelande som tar emot din IoT-hubb anropar Stream Analytics-jobbet väder prognos webbtjänsten för att skapa risken för regn.</span><span class="sxs-lookup"><span data-stu-id="6047f-199">For each message that your IoT hub receives, the Stream Analytics job calls the weather forecast web service to produce the chance of rain.</span></span> <span data-ttu-id="6047f-200">Resultatet sparas sedan till Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6047f-200">The result is then saved to your Azure blob storage.</span></span> <span data-ttu-id="6047f-201">Azure Lagringsutforskaren är ett verktyg som du kan använda för att visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="6047f-201">Azure Storage Explorer is a tool that you can use to view the result.</span></span>

1. <span data-ttu-id="6047f-202">[Hämta och installera Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6047f-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="6047f-203">Öppna Utforskaren i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6047f-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="6047f-204">Logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6047f-204">Sign in to your Azure account.</span></span>
1. <span data-ttu-id="6047f-205">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6047f-205">Select your subscription.</span></span>
1. <span data-ttu-id="6047f-206">Klicka på din prenumeration > **Lagringskonton** > ditt lagringskonto > **Blob-behållare** > din behållare.</span><span class="sxs-lookup"><span data-stu-id="6047f-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="6047f-207">Öppna en CSV-fil om du vill se resultatet.</span><span class="sxs-lookup"><span data-stu-id="6047f-207">Open a .csv file to see the result.</span></span> <span data-ttu-id="6047f-208">Den sista kolumnen registrerar risken för regn.</span><span class="sxs-lookup"><span data-stu-id="6047f-208">The last column records the chance of rain.</span></span>

   ![Få väder prognos resultat med Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="6047f-210">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6047f-210">Summary</span></span>

<span data-ttu-id="6047f-211">Du har använt Azure Machine Learning risken för regn baserat på de temperatur- och fuktighetskonsekvens som tar emot din IoT-hubb gav inga.</span><span class="sxs-lookup"><span data-stu-id="6047f-211">You’ve successfully used Azure Machine Learning to produce the chance of rain based on the temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]