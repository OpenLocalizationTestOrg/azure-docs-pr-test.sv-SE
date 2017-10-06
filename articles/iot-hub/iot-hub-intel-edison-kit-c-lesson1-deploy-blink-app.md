---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 1: distribuera programmet | Microsoft Docs'
description: "Klona hello-exempelprogram C från GitHub och kör gulp toodeploy det här programmet tooyour Intel modern kort. Det här exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund."
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
ms.openlocfilehash: fa84fae812dd742a2ad4997a5e213c8e40e6fcf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Skapa och distribuera hello blinka program
## <a name="what-you-will-do"></a>Vad du ska göra
Klona hello-exempelprogram C från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooIntel modern. hello exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
* Hur toodeploy och kör hello exempelprogram på modern.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört hello följande åtgärder:

* [Konfigurera din enhet][configure-your-device]
* [Hämta hello-verktyg][get-the-tools]

## <a name="open-hello-sample-application"></a>Öppna hello exempelprogrammet
tooopen hello exempelprogram, gör du följande:

1. Klona hello exempel databasen från GitHub genom att köra följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

hello-filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.

### <a name="install-application-dependencies"></a>Installera beroenden
Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning
tooconfigure Hej enhetsanslutning, gör du följande:

1. Generera hello konfigurationsfilen för enheten genom att köra följande kommando hello:

   ```bash
   gulp init
   ```

   hello konfigurationsfilen `config-edison.json` innehåller hello-autentiseringsuppgifter som du använder toolog i tooEdison. tooavoid hello läckage av användarautentiseringsuppgifter, hello konfigurationsfilen genereras i hello undermapp `.iot-hub-getting-started` av hello arbetsmapp på datorn.

2. Öppna konfigurationsfilen för hello enheten i Visual Studio Code genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Ersätt platshållaren hello `[device hostname or IP address]` och `[device password]` med hello IP-adress och lösenord som du har markerat i föregående lektionen.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Grattis! Du har skapat hello första exempelprogram för modern.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
### <a name="install-hello-azure-iot-hub-sdk-on-edison"></a>Installera hello Azure IoT-hubb SDK på modern
Installera hello Azure IoT-hubb SDK på modern genom att köra följande kommando hello:

```bash
gulp install-tools
```

Den här uppgiften kan ta en lång tid toocomplete, beroende på nätverksanslutningen. Det krävs toobe köras en gång för en modern.

### <a name="deploy-and-run-hello-sample-app"></a>Distribuera och köra hello sample-appen
Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Verifiera hello appen fungerar
hello exempelprogrammet avbryter automatiskt när hello Indikator blinkar för 20 gånger. Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.

![Blinka Indikator][led-blinking]

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs verktyg toowork med modern och distribuerat en exempel programmet tooEdison tooblink hello Indikator. Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter modern tooAzure IoT-hubb toosend och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta hello Azure-verktyg][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
