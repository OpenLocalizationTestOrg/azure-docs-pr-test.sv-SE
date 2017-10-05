---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) till Azure IoT - lektionen 3: Table storage | Microsoft Docs'
description: "Övervaka meddelandena från enhet till moln medan de skrivs till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 9965bd54-61da-479b-aaad-5604850a2be5
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 60084906c05ff9e5396f8e2378d73f7ac939d8df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Läs meddelandena kvar i Azure Storage
## <a name="what-you-will-do"></a>Vad du ska göra
Övervaka enhet till moln-meddelanden som skickas från hallon Pi 3 till din IoT-hubb som meddelandena som skrivs till Azure Table storage. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig hur du använder aktiviteten gulp läsa meddelandet för att läsa meddelanden kvar i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver
Innan du påbörjar den här processen måste har slutförts [kör exempelprogrammet Azure blinka på hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="read-new-messages-from-your-storage-account"></a>Läsa nya meddelanden från ditt lagringskonto
I föregående artikel körde du ett exempelprogram med Pi. Exempel programmet skickas meddelanden till din Azure IoT-hubb. Meddelanden som skickas till din IoT-hubb som lagras i Azure-tabellagring via appen Azure-funktion. Du måste anslutningssträngen för Azure storage att läsa meddelanden från Azure Table storage.

Följ dessa steg om du vill läsa meddelanden i Azure Table storage:

1. Hämta anslutningssträngen genom att köra följande kommandon:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Det första kommandot hämtar den `storage name` som används i det andra kommandot för att hämta anslutningssträngen. Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.
2. Öppna konfigurationsfilen `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Ersätt `[Azure storage connection string]` med den anslutningssträng som du fick i steg 1.
4. Spara den `config-raspberrypi.json` filen.
5. Skicka meddelanden igen och läsa dem i Azure Table storage genom att köra följande kommando:
   
   ```bash
   gulp run --read-storage
   ```
   
   Logik för att läsa från Azure Table storage är i den `azure-table.js` filen.
   
    ![gulp köras, Läs-lagring](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a>Sammanfattning
Du har korrekt anslutna Pi till din IoT-hubb i molnet och används exempelprogrammet blinka för att skicka meddelanden från enhet till moln. Du kan också används appen Azure funktionen för att lagra inkommande meddelanden i IoT-hubb till Azure Table storage. Du kan nu skicka moln till enhet meddelanden från din IoT-hubb till Pi.

## <a name="next-steps"></a>Nästa steg
[Kör exempelprogrammet ta emot meddelanden moln till enhet](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

