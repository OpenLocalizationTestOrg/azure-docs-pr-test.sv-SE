---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona hello-exempelprogram C från GitHub och gulp toodeploy det här programmet tooyour hallon Pi 3-kort. Det här exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry pi ledde blinka, blinka ledde med raspberry pi
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: e90c3360c4de1873313db19561c781eb21dbf1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Skapa och distribuera hello blinka program
## <a name="what-you-will-do"></a>Vad du ska göra
Klona hello C exempelprogrammet från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooRaspberry Pi 3. hello exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur toouse hello `device-discover-cli` verktyget tooretrieve nätverk information om Pi.
* Hur toodeploy och kör hello exempelprogram med Pi.
* Hur toodeploy och felsöka program som körs på Pi.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört hello följande åtgärder:

* [Konfigurera din enhet](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Hämta hello-verktyg](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a>Hämta hello IP-adress och värddatornamn namn PI
Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu och kör sedan följande kommando hello:

```bash
devdisco list --eth
```

Du bör se utdata som är liknande toohello följande:

![Identifiering av nätverksenheter](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Anteckna hello `IP address` och `hostname` pi. Du behöver den här informationen senare i den här artikeln.

> [!NOTE]
> Se till att Pi är anslutna toohello samma nätverk som datorn. Till exempel om datorn är ansluten tooa trådlösa nätverk medan Pi är anslutna tooa kabelanslutet nätverk kan du kanske inte se hello IP-adress i hello devdisco utdata.

## <a name="open-hello-sample-application"></a>Öppna hello exempelprogrammet
tooopen hello exempelprogram, gör du följande:

1. Klona hello exempel databasen från GitHub genom att köra följande kommando hello:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

Hej `main.c` filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.

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
   
   hello konfigurationsfilen `config-raspberrypi.json` innehåller hello-autentiseringsuppgifter som du använder toolog i tooPi. tooavoid hello läckage av användarautentiseringsuppgifter, hello konfigurationsfilen genereras i hello undermapp `.iot-hub-getting-started` av hello arbetsmapp på datorn.

2. Öppna konfigurationsfilen för hello enheten i Visual Studio Code genom att köra följande kommando hello:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Ersätt platshållaren hello `[device hostname or IP address]` med hello IP-adress eller värdnamn för hello som du har fått tidigare i ”hämta hello IP-adress och värddatornamn name pi”.
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Du kan använda SSH-nyckel i stället för användarnamn och lösenord när du ansluter tooRaspberry Pi. I ordning toodo detta behöver toogenerate hello nyckel med **ssh-keygen** och **ssh-kopia-id pi @\<enhetsadress\>**.
>
> Dessa kommandon är tillgängliga i Windows **Git Bash**.
>
> I MacOS måste toorun **brew installera ssh-kopia-id**.
>
> Efter överföring har hello viktiga toohello hallon Pi, Ersätt **device_password** med **device_key_path** egenskap i **config raspberrypi.json**.
>
> Uppdaterade rader ska se ut som nedan:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Grattis! Du har skapat hello första exempelprogram för Pi.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
### <a name="install-hello-azure-iot-hub-sdk-on-pi"></a>Installera hello Azure IoT-hubb SDK på Pi
Installera hello Azure IoT-hubb SDK på Pi genom att köra följande kommando hello:

```bash
gulp install-tools
```

Den här uppgiften kan ta några minuter toocomplete hello första gången du kör den.

### <a name="deploy-and-run-hello-sample-app"></a>Distribuera och köra hello sample-appen
Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Verifiera hello appen fungerar
hello exempelprogrammet avbryter automatiskt när hello Indikator blinkar för 20 gånger. Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) för lösningar toocommon problem.
![Blinka Indikator](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs verktyg toowork med Pi och distribuerat en exempel programmet tooPi tooblink hello Indikator. Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter Pi tooAzure IoT-hubb toosend och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta Azure-verktyg](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

