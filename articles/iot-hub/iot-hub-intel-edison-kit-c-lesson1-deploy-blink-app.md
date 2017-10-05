---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 1: distribuera programmet | Microsoft Docs'
description: "Klona exempelprogrammet C från GitHub och kör gulp om du vill distribuera programmet till Intel modern-kort. Det här exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: arduino ledde projekt, arduino ledde blinka, arduino ledde blinka koden, arduino blinka program, arduino blinka exempel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c45ff5f41bdbc78da8532ffdcaaeec15c695f531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Skapa och distribuera blinkningsprogrammet
## <a name="what-you-will-do"></a>Vad du ska göra
Klona exempelprogrammet C från GitHub och verktyget gulp för att distribuera exempelprogrammet till Intel modern. Exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
* Hur du distribuerar och kör exempelprogrammet på modern.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört följande åtgärder:

* [Konfigurera din enhet][configure-your-device]
* [Skaffa dig verktyg][get-the-tools]

## <a name="open-the-sample-application"></a>Öppna exempelprogrammet
Följ dessa steg om du vill öppna exempelprogrammet:

1. Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

Filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.

### <a name="install-application-dependencies"></a>Installera beroenden
Installera bibliotek och andra moduler som du behöver för exempelprogrammet genom att köra följande kommando:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning
Följ dessa steg för att konfigurera enhetsanslutningen:

1. Generera konfigurationsfilen enheten genom att köra följande kommando:

   ```bash
   gulp init
   ```

   Konfigurationsfilen `config-edison.json` innehåller autentiseringsuppgifter för användare som du använder för att logga in på modern. För att undvika läckage av användarautentiseringsuppgifter konfigurationsfilen genereras i undermappen `.iot-hub-getting-started` för arbetsmapp på datorn.

2. Öppna konfigurationsfilen enheten i Visual Studio Code genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Ersätt platshållaren `[device hostname or IP address]` och `[device password]` med IP-adress och lösenord som du har markerat i föregående lektionen.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Grattis! Du har skapat det första exempelprogrammet för modern.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
### <a name="install-the-azure-iot-hub-sdk-on-edison"></a>Installera Azure IoT-hubb SDK på modern
Installera Azure IoT-hubb SDK på modern genom att köra följande kommando:

```bash
gulp install-tools
```

Den här uppgiften kan ta lång tid att slutföra beroende på nätverksanslutningen. Den måste köras en gång för en modern.

### <a name="deploy-and-run-the-sample-app"></a>Distribuera och köra sample-appen
Distribuera och köra exempelprogrammet genom att köra följande kommando:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Kontrollera appen fungerar
Exempelprogrammet avbryter automatiskt när Indikatorn blinkar för 20 gånger. Om du inte ser Indikator blinkar, finns det [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.

![Blinka Indikator][led-blinking]

## <a name="summary"></a>Sammanfattning
Du har installerat de verktyg som krävs för att arbeta med modern och distribuerat ett exempelprogram för modern blinkar på Indikator. Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter modern till Azure IoT Hub skicka och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta Azure-verktyg][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
