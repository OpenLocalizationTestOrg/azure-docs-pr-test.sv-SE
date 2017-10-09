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
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Visualisera sensordata i realtid från Azure IoT-hubb med Power BI

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur toovisualize sensordata i realtid som tar emot din Azure IoT-hubb av Power BI. Om du vill visualisera tootry hello data i din IoT-hubb med Web Apps Se [Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Vad du gör

- Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.
- Skapa, konfigurera och köra ett Stream Analytics-jobb för att överföra data från din IoT-hubb tooyour Power BI-konto.
- Skapa och publicera en Power BI rapporten toovisualize hello data.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:
  - En aktiv Azure-prenumeration.
  - En Azure IoT-hubb i din prenumeration.
  - Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.
- Power BI-konto. ([Testa Powerbi gratis](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Skapa, konfigurera och köra ett Stream Analytics-jobb

### <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb

1. I hello Azure-portalen, klicka på Ny > Internet of Things > Stream Analytics-jobbet.
1. Ange följande information för jobbet hello hello.

   **Jobbnamnet**: hello namnet på hello jobb. hello namn måste vara globalt unika.

   **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   **Plats**: Använd hello samma plats som resursgruppen.

   **PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.

   ![Skapa ett Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Klicka på **Skapa**.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Lägg till ett inkommande toohello Stream Analytics-jobb

1. Öppna hello Stream Analytics-jobbet.
1. Under **jobbet topologi**, klickar du på **indata**.
1. I hello **indata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:

   **Ett inmatat alias**: unika hello-alias för hello indata.

   **Källan**: Välj **IoT-hubb**.

   **Konsumentgrupp**: Välj hello konsumentgrupp som du nyss skapade.
1. Klicka på **Skapa**.

   ![Lägg till ett inkommande tooa Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a>Lägga till utdata toohello Stream Analytics-jobbet

1. Under **jobbet topologi**, klickar du på **utdata**.
1. I hello **utdata** rutan klickar du på **Lägg till**, och ange sedan hello följande information:

   **Kolumnalias**: unikt hello-alias för hello utdata.

   **Sink**: Välj **Power BI**.
1. Klicka på **auktorisera**, och sedan logga in på ditt Power BI-konto.
1. När behöriga, ange hello följande information:

   **Gruppera arbetsytan**: Välj din grupp målarbetsytan.

   **DataSet-namnet**: Ange ett namn för dataset.

   **Tabellnamn**: Ange ett tabellnamn.
1. Klicka på **Skapa**.

   ![Lägga till utdata tooa Stream Analytics-jobbet i Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Konfigurera hello frågan för hello Stream Analytics-jobbet

1. Under **jobbet topologi**, klickar du på **frågan**.
1. Ersätt `[YourInputAlias]` med hello Ange alias för hello jobb.
1. Ersätt `[YourOutputAlias]` med hello kolumnalias för hello jobb.
1. Klicka på **Spara**.

   ![Lägg till en fråga tooa Stream Analytics-jobbet i Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a>Kör hello Stream Analytics-jobbet

Klicka på hello Stream Analytics-jobbet **starta** > **nu** > **starta**. När hello jobbet har startar hello jobbets status har ändrats från **stoppad** för**kör**.

![Kör ett Stream Analytics-jobb i Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a>Skapa och publicera en Power BI rapporten toovisualize hello data

1. Kontrollera hello exempelprogrammet körs på enheten. Om inte, kan du läsa toohello självstudier under [konfigurera enheten](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Logga in tooyour [Power BI](https://powerbi.microsoft.com/en-us/) konto.
1. Gå toohello grupp arbetsyta som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.
1. Klicka på **strömning datauppsättningar**.

   Du bör se hello visas dataset som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.
1. Under **åtgärder**, klicka på hello första ikonen toocreate en rapport.

   ![Skapa en Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Skapa ett diagram tooshow realtid temperatur över tid.
   1. Lägg till ett linjediagram hello rapport skapas på sidan.
   1. På hello **fält** rutan Expandera hello-tabell som du angav när du skapade hello utdata för hello Stream Analytics-jobbet.
   1. Dra **EventEnqueuedUtcTime** för**axel** på hello **visualiseringar** fönstret.
   1. Dra **temperatur** för**värden**.

      Nu skapas ett linjediagram. hello x-axeln visar datum och tid i hello UTC-tidszonen. hello y-axeln visar temperatur från hello sensor.

      ![Lägg till ett linjediagram för temperatur tooa Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Skapa en annan rad diagram tooshow realtid fuktighet över tid. toodo, följ samma steg ovan hello och placera **EventEnqueuedUtcTime** på hello x-axeln och **fuktighet** på hello y-axeln.

   ![Lägg till ett linjediagram för fuktighet tooa Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Klicka på **spara** toosave hello rapporten.
1. Klicka på **filen** > **publicera tooweb**.
1. Klicka på **skapa inbäddningskod**, och klicka sedan på **publicera**.

Du har angett hello rapportlänken som du kan dela med någon för åtkomst till rapporten och en kodfragment toointegrate hello rapport i din blogg eller webbplats.

![Publicera en Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft erbjuder även hello [Power BI-appar](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) för att visa och interagera med dina Power BI-instrumentpaneler och rapporter på din mobila enhet.

## <a name="next-steps"></a>Nästa steg

Du har använt sensordata i Power BI toovisualize realtid från din Azure IoT-hubb.
Det finns ett annat sätt toovisualize data från Azure IoT Hub. Se [Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
