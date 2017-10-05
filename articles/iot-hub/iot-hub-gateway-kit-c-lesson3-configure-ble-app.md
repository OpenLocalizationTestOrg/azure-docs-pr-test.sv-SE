---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 3: kör exempelappen | Microsoft Docs"
description: "Kör ett TIVERA exempelprogram som tar emot data från TIVERA SensorTag och från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Aktivera appen, sensor övervaka app, insamling av sensor, data från sensorer, sensordata till molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Konfigurera och köra ett TIVERA exempelprogram

## <a name="what-you-will-do"></a>Vad du ska göra

- Klona lagringsplatsen exempel. 
- Konfigurera anslutningen mellan SensorTag och Intel NUC. 
- Använda Azure CLI för att få din IoT-hubb och SensorTag information för ett exempelprogram TIVERA (Bluetooth låg energi). Konfigurera och köra exempelprogrammet tabell. 

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här artikeln får du lära dig:

- Hur du konfigurerar och kör exempelprogrammet tabell.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört

- [Skapa en IoT-hubb och registrera SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>Klona lagringsplatsen för exempel på värddatorn

Följ dessa steg om du vill klona databasen prov på värddatorn:

1. Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.
2. Kör följande kommandon:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a>Konfigurera anslutningen mellan SensorTag och Intel NUC

Följ dessa steg om du vill konfigurera anslutningen på värddatorn:

1. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Leta upp följande rad med kod och ersätter `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet på Intel NUC.
   ![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Installera helper verktyg på Intel NUC genom att köra följande kommando:

   ```bash
   gulp install-tools
   ```

5. Aktivera SensorTag genom att trycka på strömknappen som på bilden nedan och grön Indikator blinka.

   ![Aktivera Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Sök igenom SensorTag-enheter genom att köra följande kommandon:

   ```bash
   gulp discover-sensortag
   ```

7. Testa anslutningen mellan SensorTag och Intel NUC genom att köra följande kommando:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Ersätt `{mac address}` med MAC-adress som du hämtade i föregående steg.

## <a name="get-the-connection-string-of-sensortag"></a>Hämta anslutningssträngen för SensorTag

Kör följande kommando för att hämta anslutningssträngen för Azure IoT-hubb för SensorTag på värddatorn:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`är IoT hub-namn som du använde. Använd iot-gateway som värde för `{resource group name}` och använda mydevice som värde för `{device id}` om du inte ändra värdet i lektionen 2.

## <a name="configure-the-ble-sample-application"></a>Konfigurera TIVERA exempelprogrammet

Följ dessa steg för att konfigurera och köra exempelprogrammet TIVERA på värddatorn:

1. Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Gör följande ersättningar i koden:
   - Ersätt `[IoT hub name]` med IoT-hubbnamn som du använde.
   - Ersätt `[IoT device connection string]` med anslutningssträngen för SensorTag som du fick.
   - Ersätt `[device_mac_address]` med MAC-adressen för SensorTag som du fick.

3. Kör exempelprogrammet tabell.

   Följ dessa steg om du vill köra exempelprogrammet TIVERA på värddatorn:

   1. Aktivera SensorTag.

   2. Distribuera och köra exempelprogrammet TIVERA på Intel NUC genom att köra följande kommando:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a>Kontrollera att exempelprogrammet TIVERA fungerar

Du bör nu se utdata som liknar följande:

![TIVERA exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

Exempelprogrammet fortsätter att samla in data för temperatur och skickas till din IoT-hubb. Exempelprogrammet avbryter automatiskt när du har skickat 40 sekunder.

## <a name="summary"></a>Sammanfattning

Du har har konfigurera anslutningar mellan SensorTag och Intel NUC och kör ett tabell-exempelprogram som samlar in och skickar data från SensorTag till din IoT-hubb. Du är redo att lära dig hur du kontrollerar att din IoT-hubb har tagit emot data.

## <a name="next-steps"></a>Nästa steg
[Läs meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)