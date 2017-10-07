---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra en exempel programmet tooRaspberry Pi 3 som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "blinka ledde molnet pi, ledde blinka från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c484beb2e2d3a3cf19f071f2ba87b9a4fe41c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Kör ett exempel programmet toosend meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur toodeploy och kör ett exempelprogram på hallon Pi 3 som skickar meddelanden tooyour IoT-hubb. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du kommer lära dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet Node.js på Pi.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
anslutningssträngen för hello enheten används av din Pi tooconnect tooyour IoT-hubb. hello anslutningssträngen för IoT-hubben är används tooconnect toohello identitetsregistret i din IoT-hubb toomanage hello enheter som beviljats tooconnect tooyour IoT-hubb. 

* Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.

* Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat Pi.

* Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Använd `myraspberrypi` som hello värde för `{device id}` om du inte ändrar hello-värdet.

## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning
1. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

2. Öppna hello enheten konfigurationsfilen `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando hello:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Se följande ersättningar i hello hello `config-raspberrypi.json` fil:
   
   * Ersätt **[enhet värdnamn eller IP-adress]** med hello IP-adressen eller värdnamnet enhetsnamn som du fick från `device-discovery-cli` eller med hello-värde som överförs när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.

> [!NOTE]
> Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

Uppdatera hello `config-raspberrypi.json` filen så att du kan distribuera hello exempelprogrammet från datorn.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Kontrollera att det fungerar hello exempelprogrammet
Du bör se hello Indikator som är anslutna tooPi blinka varannan sekund. Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb. Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret. hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a>Sammanfattning
Du har distribuerat och köra hello nya blinka exempelprogrammet på Pi toosend meddelanden från enhet till moln tooyour IoT-hubb. Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)

