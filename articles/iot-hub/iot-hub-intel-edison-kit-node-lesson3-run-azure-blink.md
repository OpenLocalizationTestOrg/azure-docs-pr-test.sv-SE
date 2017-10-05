---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 3: skicka meddelanden | Microsoft Docs'
description: "Distribuera och köra ett exempelprogram till Intel modern som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data till molnet"
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
ms.openlocfilehash: d4b520b9a1852a285b1e10b5b35447a54313af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Kör ett exempelprogram för att skicka meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur du distribuera och köra ett exempelprogram på Intel modern som skickar meddelanden till din IoT-hubb. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du lära dig hur du använder verktyget gulp att distribuera och köra exempelprogrammet C på modern.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
Anslutningssträngen enheten används för att ansluta modern till din IoT-hubb. IoT-hubb anslutningssträngen används för att ansluta din IoT-hubb för enhetens identitet som representerar modern i IoT-hubben.

* Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.

* Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat modern.

* Hämta anslutningssträngen för enheten genom att köra följande kommando:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Använd `myinteledison` som värde för `{device id}` om du inte ändra värdet.

## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning
1. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   npm install
   gulp init
   ```

2. Öppna konfigurationsfilen enheten `config-edison.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. Se följande ersättningar i den `config-edison.json` filen:

   * Ersätt **[enhet värdnamn eller IP-adress]** med enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.

   > [!NOTE]
   > Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på modern genom att köra följande kommando:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Kontrollera att det fungerar exempelprogrammet
Du bör se Indikator som är ansluten till modern blinkande varannan sekund. Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb. Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret. Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Sammanfattning
Du har distribuerat och kör ny blinka exempelprogrammet på modern att skicka meddelanden från enhet till moln till din IoT-hubb. Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md