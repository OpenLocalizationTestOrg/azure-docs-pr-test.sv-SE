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
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Väder prognos med hello sensordata från IoT-hubb i Azure Machine Learning

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning är en teknik för datavetenskap som hjälper datorer Läs från befintliga data tooforecast framtida beteenden, resultat och trender. Azure Machine Learning är en molntjänst för förutsägelseanalys som gör det möjligt tooquickly skapa och distribuera förutsägelsemodeller som Analyslösningar.

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur toouse Azure Machine Learning toodo väder prognos (risken för regn) med hjälp av hello temperatur- och fuktighetskonsekvens data från din Azure IoT-hubb. hello risken för regn är hello utdata från en förberedd väder förutsägelse modell. hello modellen bygger på historiska data tooforecast risken för regn baserat på temperatur- och fuktighetskonsekvens.

## <a name="what-you-do"></a>Vad du gör

- Distribuera hello väder förutsägelse modellen som en webbtjänst.
- Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.
- Skapa ett Stream Analytics-jobb och konfigurera hello jobbet:
  - Läsa temperatur- och fuktighetskonsekvens data från IoT-hubb.
  - Anropa hello web service tooget hello regn risken.
  - Spara hello resultatet tooan Azure blob storage.
- Använd Microsoft Azure Lagringsutforskaren tooview hello väderprognos.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:
  - En aktiv Azure-prenumeration.
  - En Azure IoT-hubb i din prenumeration.
  - Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.
- Ett konto i Azure Machine Learning Studio. ([Försök Machine Learning Studio gratis](https://studio.azureml.net/)).

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a>Distribuera hello väder förutsägelse modellen som en webbtjänst

1. Gå toohello [väder förutsägelse modellen sidan](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Klicka på **öppna i Studio** i Microsoft Azure Machine Learning Studio.
   ![Öppna hello väder förutsägelse modellen sida i Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Klicka på **kör** toovalidate hello steg i hello modellen. Det här steget kan ta 2 minuter toocomplete.
   ![Öppna hello väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Klicka på **konfigurera WEBBTJÄNSTEN** > **förutsägande webbtjänsten**.
   ![Distribuera hello väder förutsägelse modellen i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Dra i hello hello **Web service indata** modulen någonstans nära hello **Poängmodell** modul.
1. Ansluta hello **Web service indata** modulen toohello **Poängmodell** modul.
   ![Ansluta två moduler i Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Klicka på **kör** toovalidate hello steg i hello modellen.
1. Klicka på **DISTRIBUERA WEBBTJÄNSTEN** toodeploy hello modellen som en webbtjänst.
1. Hämta hello på hello instrumentpanelen för hello modellen **Excel 2010 eller tidigare arbetsboken** för **frågor och svar**.

   > [!Note]
   > Se till att du hämtar hello **Excel 2010 eller tidigare arbetsboken** även om du kör en senare version av Excel på datorn.

   ![Hämta hello Excel för hello REQUEST RESPONSE slutpunkt](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Öppna hello Excel-arbetsbok, anteckna hello **URL för WEBBTJÄNSTEN** och **ÅTKOMSTNYCKELN**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Skapa, konfigurera och köra ett Stream Analytics-jobb

### <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb

1. I hello [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.
1. Ange följande information för jobbet hello hello.

   **Jobbnamnet**: hello namnet på hello jobb. hello namn måste vara globalt unika.

   **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   **Plats**: Använd hello samma plats som resursgruppen.

   **PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.

   ![Skapa ett Stream Analytics-jobb i Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klicka på **Skapa**.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Lägg till ett inkommande toohello Stream Analytics-jobb

1. Öppna hello Stream Analytics-jobbet.
1. Under **jobbet topologi**, klickar du på **indata**.
1. I hello **indata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:

   **Ett inmatat alias**: unika hello-alias för hello indata.

   **Källan**: Välj **IoT-hubb**.

   **Konsumentgrupp**: Välj hello konsumentgrupp som du skapade.

   ![Lägg till ett inkommande toohello Stream Analytics-jobb i Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klicka på **Skapa**.

### <a name="add-an-output-toohello-stream-analytics-job"></a>Lägga till utdata toohello Stream Analytics-jobbet

1. Under **jobbet topologi**, klickar du på **utdata**.
1. I hello **utdata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:

   **Kolumnalias**: unikt hello-alias för hello utdata.

   **Sink**: Välj **Blob Storage**.

   **Lagringskontot**: hello storage-konto för blobblagring. Du kan skapa ett lagringskonto eller använda en befintlig.

   **Behållaren**: hello behållaren där hello blob sparas. Du kan skapa en behållare eller använda en befintlig.

   **Händelsen serialiseringsformat**: Välj **CSV**.

   ![Lägga till utdata toohello Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klicka på **Skapa**.

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a>Lägg till en funktion toohello Stream Analytics-jobbet toocall hello webbtjänst du distribuerat

1. Under **jobbet topologi**, klickar du på **funktioner** > **Lägg till**.
1. Ange hello följande information:

   **Funktionen Alias**: Ange `machinelearning`.

   **Funktionen typen**: Välj **Azure ML**.

   **Importera alternativet**: Välj **importera från en annan prenumeration**.

   **URL: en**: Ange hello WEBBTJÄNST-URL som du antecknade ned från hello Excel-arbetsbok.

   **Nyckeln**: Ange hello ÅTKOMSTNYCKEL som du antecknade ned från hello Excel-arbetsbok.

   ![Lägg till en funktion toohello Stream Analytics-jobbet i Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klicka på **Skapa**.

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Konfigurera hello frågan för hello Stream Analytics-jobbet

1. Under **jobbet topologi**, klickar du på **frågan**.
1. Ersätt hello befintlig kod med hello följande kod:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Ersätt `[YourInputAlias]` med hello Ange alias för hello jobb.

   Ersätt `[YourOutputAlias]` med hello kolumnalias för hello jobb.

1. Klicka på **Spara**.

### <a name="run-hello-stream-analytics-job"></a>Kör hello Stream Analytics-jobbet

Klicka på hello Stream Analytics-jobbet **starta** > **nu** > **starta**. När hello jobbet har startar hello jobbets status har ändrats från **stoppad** för**kör**.

![Kör hello Stream Analytics-jobbet](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a>Använd Microsoft Azure Lagringsutforskaren tooview hello väderprognos

Kör hello klienten programmet toostart samla in och skicka temperatur- och fuktighetskonsekvens data tooyour IoT-hubb. För varje meddelande som tar emot din IoT-hubb anropar hello Stream Analytics-jobbet hello väder prognos web service tooproduce hello risken för regn. hello-resultatet sparas sedan tooyour Azure blob storage. Azure Lagringsutforskaren är ett verktyg som du kan använda tooview hello resultat.

1. [Hämta och installera Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).
1. Öppna Utforskaren i Azure Storage.
1. Logga in tooyour Azure-konto.
1. Välj din prenumeration.
1. Klicka på din prenumeration > **Lagringskonton** > ditt lagringskonto > **Blob-behållare** > din behållare.
1. Öppna ett CSV-filen toosee hello resultat. hello sista kolumnen poster hello risken för regn.

   ![Få väder prognos resultat med Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Sammanfattning

Du har använt Azure Machine Learning tooproduce hello risken för regn baserat på hello temperatur- och fuktighetskonsekvens data som tar emot din IoT-hubb.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]