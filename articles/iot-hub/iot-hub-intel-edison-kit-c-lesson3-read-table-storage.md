---
title: "Connect Intel EDISON (C) tooAzure IoT - lektionen 3: övervaka meddelanden | Microsoft Docs"
description: "Övervaka hello meddelanden från enhet till moln medan de skrivs tooyour Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data i hello molnet, insamling av molnet, iot-Molntjänsten, iot-data"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: cad545c3-dd88-486c-a663-d587a924ccd4
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2679b22f2987f77ecd1eea03044ed8ea03bf73f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Läs meddelandena kvar i Azure Storage
## <a name="what-you-will-do"></a>Vad du ska göra
Övervakaren hello enhet till moln meddelanden som skickas från Intel modern tooyour IoT-hubb som hälsningsmeddelande skrivs tooyour Azure Table storage. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig hur toouse gulp Läs meddelandeaktiviteten tooread hälsningsmeddelande kvar i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver
Innan du påbörjar den här processen måste har slutförts [köra hello Azure blinka exempelprogrammet på Intel modern][run-the-azure-blink-sample-application-on-intel-edison].

## <a name="read-new-messages-from-your-storage-account"></a>Läsa nya meddelanden från ditt lagringskonto
I föregående artikel hello körde du ett exempelprogram på modern. hello exempelprogrammet skickade meddelanden tooyour Azure IoT-hubb. hello-meddelanden som skickas tooyour IoT-hubb som lagras i Azure-tabellagring via hello Azure funktionsapp. Du behöver Azure storage anslutning sträng tooread hälsningsmeddelande från Azure Table storage.

tooread meddelanden i din Azure Table storage, Följ dessa steg:

1. Hämta hello anslutningssträng genom att köra följande kommandon hello:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   hello första kommandot hämtar hello `storage name` som används i hello andra kommandot tooget hello anslutningssträngen. Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.
2. Öppna hello konfigurationsfilen `config-edison.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. Ersätt `[Azure storage connection string]` med hello anslutningssträng som du fick i steg 1.
4. Spara hello `config-edison.json` fil.
5. Skicka meddelanden igen och läsa dem i Azure Table storage genom att köra följande kommando hello:

   ```bash
   gulp run --read-storage
   ```

   hello logik för att läsa från Azure Table storage är i hello `azure-table.js` fil.

   ![gulp köras, Läs-lagring][gulp run]

## <a name="summary"></a>Sammanfattning
Du har har anslutna modern tooyour IoT-hubb i hello molnet och används hello blinka exempel toosend enhet till moln programmeddelanden. Du kan också användas hello Azure funktionen app toostore inkommande IoT-hubb meddelanden tooyour Azure Table storage. Du kan nu skicka moln till enhet meddelanden från din IoT-hubb tooEdison.

## <a name="next-steps"></a>Nästa steg
[Kör ett exempel programmet tooreceive meddelanden moln till enhet][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md