---
title: "aaaReal tidsdata visualisering av sensordata från din Azure IoT-hubb – Web Apps | Microsoft Docs"
description: "Med funktionen hello webbprogram med Microsoft Azure App Service toovisualize temperatur- och fuktighetskonsekvens data som samlas in från hello sensor och skickas tooyour Iot-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: realtid datavisualisering, realtidsdata visualiseringen sensor datavisualisering
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a>Visualisera sensordata i realtid från Azure IoT-hubben med hjälp av funktionen för hello Web Apps i Azure App Service

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Detta får du får lära dig

I kursen får du lära dig hur toovisualize sensordata i realtid som din IoT-hubb som tar emot genom att köra ett webbprogram som är värd för ett webbprogram. Om du vill tootry toovisualize hello data i din IoT-hubb med hjälp av Power BI, se [Använd Power BI toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Vad du gör

- Skapa en webbapp i hello Azure-portalen.
- Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.
- Konfigurera hello web app tooread sensordata från IoT-hubb.
- Överför en web application toobe hello webbprogram som värd.
- Öppna hello web app toosee temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- [Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md), som omfattar hello följande krav:
  - En aktiv Azure-prenumeration
  - En Iot-hubb i din prenumeration
  - Ett klientprogram som skickar meddelanden tooyour Iot-hubb
- [Hämta Git](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Skapa en webbapp

1. I hello [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **webb + mobilt** > **Web App**.
2. Ange ett unikt jobbnamn verifierar hello prenumeration, ange en resursgrupp och en plats, väljer **PIN-kod toodashboard**, och klicka sedan på **skapa**.

   Vi rekommenderar att du väljer hello samma plats som resursgruppen. Detta hjälper till med bearbetningshastigheten och minskar hello kostnaden för dataöverföring.

   ![Skapa en webbapp](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a>Konfigurera hello web app tooread data från IoT-hubb

1. Öppna hello webbapp som du precis har etablerats.
2. Klicka på **programinställningar**, och under **appinställningar**, Lägg till följande nyckel/värde-par hello:

   | Nyckel                                   | Värde                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Hämtas från iothub explorer                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | hello namnet på hello konsumentgrupp som du lägger till tooyour IoT-hubb  |

   ![Lägga till inställningarna tooyour webbprogrammet med nyckel/värde-par](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Klicka på **programinställningar**under **allmänna inställningar**, växla hello **Web sockets** alternativ och klickar sedan på **spara**.

   ![Växla hello sockets webbinställningar](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a>Överför en web application toobe hos hello-webbprogram

På GitHub, har vi gjort tillgängliga ett webbprogram som visar sensordata i realtid från din IoT-hubb. Allt du behöver toodo är konfigurera hello web app toowork med en Git-lagringsplats, hämta hello webbprogrammet från GitHub och sedan ladda upp den tooAzure för hello web app toohost.

1. I hello webbapp klickar du på **distributionsalternativ** > **Välj källa** > **lokal Git-lagringsplats**, och klicka sedan på **OK**.

   ![Konfigurera din web app distribution toouse hello lokal Git-lagringsplats](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Klicka på **Distributionsbehörigheterna**, skapa en användare och lösenord toouse tooconnect toohello Git-lagringsplats i Azure och klicka sedan på **spara**.

3. Klicka på **översikt**, och anteckna värdet för hello av **url för Git-klon**.

   ![Hämta hello Git klon-URL för ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Öppna ett kommando eller ett terminalfönster på den lokala datorn.

5. Hämta hello webbprogrammet från GitHub och överför den tooAzure för hello web app toohost. toodo kör så hello följande kommandon:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<URL för Git-klon\> är hello URL för hello Git-lagringsplatsen finns på hello **översikt** sidan av hello webbprogram.

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Öppna hello web app toosee temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb

På hello **översikt** sidan av ditt webbprogram, klickar du på hello URL tooopen hello webbprogrammet.

![Hämta hello URL för ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

Du bör se hello realtid temperatur- och fuktighetskonsekvens data från IoT-hubb.

![Appen webbsidan visar realtid temperatur- och fuktighetskonsekvens](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Kontrollera hello exempelprogrammet körs på enheten. Om inte, får du ett tomt diagram, kan du läsa toohello självstudier under [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Nästa steg
Du har använt sensordata i realtid din web app toovisualize från IoT-hubb.

Ett annat sätt toovisualize data från Azure IoT Hub, se [Använd Power BI toovisualize realtid sensordata från IoT-hubb](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
