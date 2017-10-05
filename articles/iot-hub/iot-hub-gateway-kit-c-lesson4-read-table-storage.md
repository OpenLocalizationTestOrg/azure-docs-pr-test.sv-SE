---
title: 'SensorTag enhet & Azure IoT-Gateway - Lektion 4: Table storage | Microsoft Docs'
description: "Spara meddelanden från Intel NUC till din IoT-hubb, skriva dem till Azure Table storage och läses sedan från molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 8ca78045-ad92-4a6a-90f1-05f9668e6f0e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 72659ef3a7fd2f6011590d37176fd05503269aff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Läs meddelandena kvar i Azure Table storage

## <a name="what-you-will-do"></a>Vad du ska göra

- Kör exempelprogrammet gateway på din gateway som skickar meddelanden till din IoT-hubb.
- Kör sedan en exempelkod på värddatorn för att läsa meddelanden i Azure Table storage. 

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

Hur du använder verktyget gulp köra exempelkod för att läsa meddelanden i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver

Du har nu gjort följande uppgifter:

- [Skapade appen Azure-funktion och Azure-lagringskontot](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).
- [Kör exempelprogrammet gateway](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).
- [Läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Hämta ditt Azure storage-anslutningssträngar

Tidigt i kursen skapat du ett Azure storage-konto. Kör följande kommandon för att hämta anslutningssträngen för Azure storage-konto:

* Visa en lista med alla lagringskonton.

```bash
az storage account list -g iot-gateway --query [].name
```

* Hämta anslutningssträngen för azure-lagring.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Använd iot-gateway som värde för `{resource group name}` om du inte ändra värdet i lektionen 2.

## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning

Uppdatera den `config-azure.json` filen så att exempelkoden som körs på värddatorn kan läsa meddelandet i Azure Table storage. Följ dessa steg för att konfigurera enhetsanslutningen:

1. Öppna konfigurationsfilen enheten `config-azure.json` genom att köra följande kommandon:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguration](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Ersätt `[Azure storage connection string]` med Azure storage-anslutningssträngen som du fick.

   `[IoT hub connection string]`redan ska ersättas i avsnittet [läsa meddelanden från Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) i lektion3.

## <a name="read-messages-in-your-azure-table-storage"></a>Läsa meddelanden i Azure-tabellagring

Kör exempelprogrammet gateway och läsa Azure Table storage meddelanden med följande kommando:

```bash
gulp run --table-storage
```

IoT-hubb utlöser tillämpningsprogrammet Azure-funktion för att spara meddelanden i Azure Table storage när nytt meddelande tas emot.
Den `gulp run` körs gateway exempelprogram som skickar meddelanden till din IoT-hubb. Med `table-storage` parameter, den också skapas en underordnad process för att ta emot meddelandet sparade i Azure Table storage.

De meddelanden som skickas och tas emot är alla visas direkt på samma konsolfönstret i värddatorn. Exempel programinstansen avslutas automatiskt i 40 sekunder.

   ![gulp Läs](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


## <a name="summary"></a>Sammanfattning

Du har kört exempelkod för att läsa meddelanden i din Azure Table storage sparas av ditt program för Azure-funktion.