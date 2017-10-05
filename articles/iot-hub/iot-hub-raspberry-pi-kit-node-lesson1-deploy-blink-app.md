---
featureFlags: usabilla
title: 'Ansluta Raspberry Pi (nod) till Azure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona Node.js exempelprogrammet från GitHub och gulp om du vill distribuera programmet till hallon Pi 3-kort. Det här exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Raspberry pi ledde blinka, blinka ledde med raspberry pi
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: a5a03a57-fe86-416f-90ff-6eca17775842
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8b73000c166950172c07b8e188025dc9da5bc011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Skapa och distribuera blinkningsprogrammet
## <a name="what-you-will-do"></a>Vad du ska göra
Klona Node.js exempelprogrammet från GitHub och verktyget gulp för att distribuera exempelprogrammet till din hallon Pi 3. Exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur du använder den `device-discover-cli` verktyg för att hämta nätverksinformation om Pi.
* Hur du distribuerar och kör exempelprogrammet på Pi.
* Hur du distribuerar och felsöka program som körs på Pi.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört följande åtgärder:

* [Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [Skaffa dig verktyg](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a>Hämta IP-adress och värddatornamn namnet PI
Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu och kör sedan följande kommando:

```bash
devdisco list --eth
```

Du bör se utdata som liknar följande:

![Identifiering av nätverksenheter](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Anteckna den `IP address` och `hostname` pi. Du behöver den här informationen senare i den här artikeln.

> [!NOTE]
> Kontrollera att Pi är ansluten till samma nätverk som datorn. Om datorn är ansluten till ett trådlöst nätverk när Pi är ansluten till ett kabelanslutet nätverk, kan du inte se IP-adressen i devdisco utdata.

## <a name="clone-the-sample-application"></a>Klona exempelprogrammet
Följ dessa steg om du vill öppna exempelkoden:

1. Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

Den `app.js` filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.

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
   
   Konfigurationsfilen `config-raspberrypi.json` innehåller autentiseringsuppgifter för användare som du använder för att logga in till Pi. För att undvika läckage av användarautentiseringsuppgifter konfigurationsfilen genereras i undermappen `.iot-hub-getting-started` för arbetsmapp på datorn.

2. Öppna konfigurationsfilen enheten i Visual Studio Code genom att köra följande kommando:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. Ersätt platshållaren `[device hostname or IP address]` med IP-adressen eller värdnamnet som du har fått tidigare i ”hämta i IP-adress och värddatornamn namnet pi”.
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Du kan använda SSH-nyckel i stället för användarnamn och lösenord när du ansluter till hallon Pi. För att kunna göra detta måste du generera en nyckel med hjälp av **ssh-keygen** och **ssh-kopia-id pi @\<enhetsadress\>**.
>
> Dessa kommandon är tillgängliga i Windows **Git Bash**.
>
> Du behöver köra på MacOS **brew installera ssh-kopia-id**.
>
> Efter att överföra nyckeln till hallon Pi, Ersätt **device_password** med **device_key_path** egenskap i **config raspberrypi.json**.
>
> Uppdaterade rader ska se ut som nedan:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Grattis! Du har skapat det första exempelprogrammet för Pi.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
### <a name="install-nodejs-and-npm-on-pi"></a>Installera Node.js och NPM på Pi
Installera Node.js och NPM på Pi genom att köra följande kommando:

```bash
gulp install-tools
```

Den här uppgiften kan ta 10 minuter att slutföra den första gången du kör den.

### <a name="deploy-and-run-the-sample-app"></a>Distribuera och köra sample-appen
Distribuera och köra exempelprogrammet genom att köra följande kommando:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Kontrollera appen fungerar
Du bör nu se Indikatorn på Pi blinkande varannan sekund.  Om du inte ser Indikator blinkar, finns det [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) efter lösningar på vanliga problem.
![Blinka Indikator](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Sammanfattning
Du har installerat de verktyg som krävs för att arbeta med Pi och distribuerat ett exempelprogram till Pi blinkar på Indikator. Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter Pi till Azure IoT Hub skicka och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta Azure-verktyg](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

