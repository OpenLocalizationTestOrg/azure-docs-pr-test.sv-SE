---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: läsa meddelanden | Microsoft Docs"
description: "Köra en exempelkod på värden för datorn tooread hälsningsmeddelande från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data i hello molnet, insamling av molnet, iot-Molntjänsten, iot-data"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Läsa meddelanden från din IoT-hubb

## <a name="what-you-will-do"></a>Vad du ska göra

- Köra exempelkod på din värd datorn tooread meddelanden från din IoT-hubb.

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

Hur gulp toouse hello verktyget tooread meddelanden från din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- hello simulerade enheten provet i [konfigurera och köra en simulerad enhet moln överför exempelprogrammet](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar

anslutningssträngen för hello enheten används av din simulerade enhet tooconnect tooyour IoT-hubb. hello anslutningssträngen för IoT-hubben är används tooconnect toohello identitetsregistret i din IoT-hubb toomanage hello enheter som beviljats tooconnect tooyour IoT-hubb.

- Lista alla IoT hubs i resursgruppen genom att köra följande kommando hello:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar den.
- Hämta anslutningssträngen för hello IoT hub genom att köra följande kommando hello:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`är hello-namn som du angav i lektionen 2.

## <a name="configure-hello-device-connection-for-hello-sample-code"></a>Konfigurera hello enhetsanslutning hello-kodexempel

Uppdatera konfigurationer för IoT-hubb och enheten anslutning i `config-azure.json` genom att utföra hello följande steg:

1. Öppna `config-azure.json` i Visual Studio-koden genom att köra följande kommando i konsolfönstret hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Se följande ersättningar i hello hello `config-azure.json` fil:

   ![Skärmbild av azure config](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Ersätt `[IoT hub connection string]` med hello anslutningssträngen för IoT-hubb.

## <a name="read-messages-from-your-iot-hub"></a>Läsa meddelanden från din IoT-hubb

Kör exempelprogrammet i hello simulerade enheten och läsa IoT-hubb meddelanden hello följande kommando:

```bash
gulp run --iot-hub
```

hello körs hello-program som skickar meddelanden tooyour IoT-hubb varannan sekund. Det skapas också en underordnad process tooreceive hello-meddelande.

hello-meddelanden som skickas och tas emot är alla visas direkt på hello samma konsolfönstret i hello värddatorn. hello programmet avslutas i 40 sekunder.

![Simulerade exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a>Sammanfattning

Du har har kört hello exempel programmet toosend data tooyour IoT-hubb med simulerade enhet. Du har också läsa hälsningsmeddelande som har skickats tooyour IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en Azure-funktionsapp och ett Azure Storage-konto](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


