---
title: "Connect Raspberry PI (C) till Azure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra ett exempelprogram hallon Pi 3 som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
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
ms.openlocfilehash: e583ba455a94f9afcc7b31e49425b518d7968919
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Kör ett exempelprogram för att skicka meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur du distribuera och köra ett exempelprogram på hallon Pi 3 som skickar meddelanden till din IoT-hubb. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du lära dig hur du använder verktyget gulp att distribuera och köra Node.js exempelprogrammet på Pi.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
Anslutningssträngen enheten används av din Pi för att ansluta till din IoT-hubb. IoT-hubb anslutningssträngen används för att ansluta till identitetsregistret i IoT-hubb för att hantera enheter som tillåts att ansluta till din IoT-hubb. 

* Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.

* Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat Pi.

* Hämta anslutningssträngen för enheten genom att köra följande kommando:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Använd `myraspberrypi` som värde för `{device id}` om du inte ändra värdet.

## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning
1. Initiera konfigurationsfilen genom att köra följande kommandon:
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

2. Öppna konfigurationsfilen enheten `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Se följande ersättningar i den `config-raspberrypi.json` filen:
   
   * Ersätt **[enhet värdnamn eller IP-adress]** med IP-adressen eller värdnamnet enhetsnamnet som du fick från `device-discovery-cli` eller med värdet överförs när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.

> [!NOTE]
> Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

Uppdatera den `config-raspberrypi.json` filen så att du kan distribuera exempelprogrammet från datorn.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Pi genom att köra följande kommando:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Kontrollera att det fungerar exempelprogrammet
Du bör se som är ansluten till Pi blinkande varannan sekund LED. Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb. Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret. Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a>Sammanfattning
Du har distribuerat och kör ny blinka exempelprogrammet på Pi för att skicka meddelanden från enhet till moln till din IoT-hubb. Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)

