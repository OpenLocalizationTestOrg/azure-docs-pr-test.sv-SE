---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 3: skicka meddelanden | Microsoft Docs'
description: "Distribuera och köra en exempel programmet tooIntel modern som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 1b3b1074-f4d4-42ac-b32c-55f18b304b44
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ebd4c7558544d64086fb4cd615cee546aeed2fc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Kör ett exempel programmet toosend meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur toodeploy och kör ett exempelprogram på Intel modern som skickar meddelanden tooyour IoT-hubb. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du lär dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet C på modern.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
anslutningssträngen för hello enhet är används tooconnect modern tooyour IoT-hubb. Hej anslutningssträngen för IoT-hubben är används tooconnect din IoT-hubb toohello enhetsidentitet som representerar modern i hello IoT-hubb.

* Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.

* Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat modern.

* Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Använd `myinteledison` som hello värde för `{device id}` om du inte ändrar hello-värdet.

## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning
1. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   npm install
   gulp init
   ```

2. Öppna hello enheten konfigurationsfilen `config-edison.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. Se följande ersättningar i hello hello `config-edison.json` fil:

   * Ersätt **[enhet värdnamn eller IP-adress]** med hello enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.

   > [!NOTE]
   > Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Kontrollera att det fungerar hello exempelprogrammet
Du bör se hello Indikator som är anslutna tooEdison blinka varannan sekund. Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb. Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret. hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Sammanfattning
Du har distribuerat och köra hello nya blinka exempelprogrammet på modern toosend meddelanden från enhet till moln tooyour IoT-hubb. Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md