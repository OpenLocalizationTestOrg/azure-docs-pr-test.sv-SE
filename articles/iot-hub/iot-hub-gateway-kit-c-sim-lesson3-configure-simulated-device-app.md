---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: kör exempelappen | Microsoft Docs"
description: "Kör en simulerad enhet exempel app toosend temperatur data tooyour IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Konfigurera och köra en simulerad enhet exempelapp

## <a name="what-you-will-do"></a>Vad du ska göra

- Klona hello exempel databas.
- Använd hello Azure CLI tooget din IoT-hubb och information om logisk enhet för simulerad enhet exempelprogrammet. Konfigurera och köra hello simulerade enheten exempelprogrammet.

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här artikeln får du lära dig:

- Hur tooconfigure och kör hello simulerade enheten exempelprogrammet.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört

- [Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klona hello exempel databasen toohello värddatorn

tooclone hello exempel lagringsplatsen, Följ dessa steg på värddatorn för hello:

1. Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.
2. Kör följande kommandon hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a>Konfigurera hello simulerade enheten och din NUC

1. Öppna hello konfigurationsfilen `config.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   code config.json
   ```

2. Ersätt `"has_sensortag": true` med`"has_sensortag": false`

   ![Konfigurationen som du inte har en TI SensorTag-enhet](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Leta upp följande kodrad hello och ersätta `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet hello Intel NUC.
   ![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a>Hämta hello anslutningssträngen för din IoT-hubb logisk enhet

tooget hello Azure IoT hub-anslutningssträngen för din logiska enheter som kör följande kommando på värddatorn för hello hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`är hello IoT-hubbnamn som du använde. Använd iot-gateway som hello värde för `{resource group name}` och använda mydevice som hello värdet för `{device id}` om du inte ändrar hello värdet i lektionen 2.

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a>Konfigurera hello simulerade enheten molnet överför exempelprogrammet

tooconfigure och kör hello simulerade enhet moln överför exempelprogrammet, Följ dessa steg på värddatorn för hello:

1. Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Att Hej följande ersättningar i hello kod:
   - Ersätt `[IoT hub name]` med hello IoT-hubbnamnet.
   - Ersätt `[IoT device connection string]` med hello anslutningssträngen för din IoT-hubb logisk enhet.

3. Kör hello program.

   Distribuera och köra programmet hello genom att köra följande kommando hello:

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a>Verifiera hello exempel programmet fungerar

Du bör nu se utdata som liknar hello följande:

![simulerade enheten exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

hello skickar temperatur data tooyour IoT-hubben, som gäller i 40 sekunder.

## <a name="summary"></a>Sammanfattning

Du har konfigurerat och kör hello simulerade enheten molnet Överför med exempelprogrammet som skickar data tooyour IoT-hubb med simulerade enhet.

## <a name="next-steps"></a>Nästa steg
[Läs meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)