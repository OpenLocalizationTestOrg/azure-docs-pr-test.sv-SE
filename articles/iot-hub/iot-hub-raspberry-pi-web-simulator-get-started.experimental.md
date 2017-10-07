---
title: "aaaSimulated hallon Pi toocloud (Node.js) – ansluta hallon Pi web simulator tooAzure IoT-hubb | Microsoft Docs"
description: "Ansluta hallon Pi web simulator tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry pi simulatorn, azure iot raspberry pi, raspberry pi iot-hubb, raspberry pi skicka data toocloud, raspberry pi toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/7/2017
ms.author: xshi
ms.openlocfilehash: 0ebe1004e96ff4e64fdd1b05b7e35192ec42f7d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a>Ansluta hallon Pi online simulator tooAzure IoT-hubb (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

I den här självstudiekursen börjar du med learning hello grunderna i att arbeta med hallon Pi online simulatorn. Du lär dig hur tooseamlessly ansluta hello Pi simulator toohello moln med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md). 

Om du har fysiska enheter gå [ansluta hallon Pi tooAzure IoT-hubb](iot-hub-raspberry-pi-kit-node-get-started.md) tooget igång. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Vad du gör

* Lär dig grunderna hello av hallon Pi online simulatorn.
* Skapa en IoT-hubb.
* Registrera en enhet för Pi i din IoT-hubb.
* Kör ett exempelprogram på Pi toosend simulerade sensor data tooyour IoT-hubb.

Ansluta simulerade hallon Pi tooan IoT-hubb som du skapar. Sedan kör du ett exempelprogram med hello simulator toogenerate sensordata. Slutligen kan skicka du hello sensor data tooyour IoT-hubb.

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* Hur toowork med hallon Pi online simulatorn.
* Hur toosend sensor data tooyour IoT-hubb.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Översikt över hallon Pi web simulator

Klicka på hello knappen toolaunch hallon Pi online simulatorn.

> [!div class="button"]
[Starta hallon Pi simulator](https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted)

Det finns tre områden i hello web-simulatorn.
* Sammansättningen område - hello standard kretsen är att en Pi ansluter med en BME280 sensor och en Indikator. hello området är låst i förhandsversionen så för närvarande kan du inte göra anpassning.
* Kodning område - Kodredigerare online för toocode med hallon Pi. hello standard exempelprogrammet hjälper toocollect sensordata från BME280 sensor och skickar tooyour Azure IoT Hub. hello programmet är helt kompatibel med verkliga Pi-enheter. 
* Integrerad konsolfönstret - visar hello utdata från din kod. Det finns tre knappar överst hello i det här fönstret.
   * **Kör** -körs programmet hello i hello kodning område.
   * **Återställ** -återställning hello kodning området toohello standard exempelprogrammet.
   * **Vikning/Expandera** – på höger sida det är en knapp för du toofold/Expandera hello konsolfönstret hello.

> [!NOTE] 
hello hallon Pi web simulator är nu tillgängliga i förhandsversionen. Vi vill gärna toohear rösten i hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). hello källkoden är offentlig på [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Översikt över Pi online simulator](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Kör ett exempelprogram på Pi web simulator

1. Kontrollera att du arbetar med hello standard exempelprogrammet i kodning område. Ersätt hello platshållare i rad 15 med hello anslutningssträngen för Azure IoT hub-enhet.
   ![Ersätt hello anslutningssträngen för enhet](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Klicka på **kör** eller typ `npm start` toorun hello program.


Du bör se följande hello utdata som visar hello sensordata och hello-meddelanden som skickas tooyour IoT-hubb ![utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Nästa steg

Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
