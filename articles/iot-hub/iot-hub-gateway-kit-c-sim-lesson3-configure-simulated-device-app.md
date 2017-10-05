---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: kör exempelappen | Microsoft Docs"
description: "Kör en simulerad enhet exempelapp för att skicka temperatur data till din IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data till molnet
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
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Konfigurera och köra en simulerad enhet exempelapp

## <a name="what-you-will-do"></a>Vad du ska göra

- Klona lagringsplatsen exempel.
- Använda Azure CLI för att få din IoT-hubb och en logisk enhetsinformation för simulerade enheten exempelprogrammet. Konfigurera och köra exempelprogrammet simulerade enheten.

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här artikeln får du lära dig:

- Hur du konfigurerar och kör exempelprogrammet simulerade enheten.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört

- [Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>Klona lagringsplatsen för exempel på värddatorn

Följ dessa steg om du vill klona databasen prov på värddatorn:

1. Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.
2. Kör följande kommandon:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a>Konfigurera den simulerade enheten och din NUC

1. Öppna konfigurationsfilen `config.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   code config.json
   ```

2. Ersätt `"has_sensortag": true` med`"has_sensortag": false`

   ![Konfigurationen som du inte har en TI SensorTag-enhet](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Leta upp följande rad med kod och ersätter `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet Intel NUC.
   ![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a>Hämta anslutningssträngen för din IoT-hubb logisk enhet

Kör följande kommando för att hämta anslutningssträngen för Azure IoT-hubb för logiska enheten på värddatorn:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`är IoT hub-namn som du använde. Använd iot-gateway som värde för `{resource group name}` och använda mydevice som värde för `{device id}` om du inte ändra värdet i lektionen 2.

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a>Konfigurera simulerade enheten molnet överför exempelprogrammet

Följ dessa steg för att konfigurera och kör exempelprogrammet simulerade enheten molnet överföringen på värddatorn:

1. Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Gör följande ersättningar i koden:
   - Ersätt `[IoT hub name]` med IoT-hubbnamnet.
   - Ersätt `[IoT device connection string]` med anslutningssträngen för din IoT-hubb logisk enhet.

3. Kör appen.

   Distribuera och köra programmet genom att köra följande kommando:

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a>Verifiera exempel programmet fungerar

Du bör nu se utdata som liknar följande:

![simulerade enheten exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

Programmet skickar temperatur data till din IoT-hubb varar 40 sekunder.

## <a name="summary"></a>Sammanfattning

Du har har konfigurerats och kör simulerade enheten molnet överför exempelprogrammet som skickar data till din IoT-hubb med simulerade enhet.

## <a name="next-steps"></a>Nästa steg
[Läs meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)