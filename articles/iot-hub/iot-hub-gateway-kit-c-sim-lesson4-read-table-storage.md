---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 4: Table storage | Microsoft Docs'
description: "Spara meddelanden från Intel NUC tooyour IoT-hubb, skrivs tooAzure Table storage och läses sedan från hello molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 78e4b6ea-968d-401e-a7dc-8f9acdb3ec1a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 43e299d63bbbe10d4d867af25e700c3a7cc07c53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Läs meddelandena kvar i Azure Table storage

## <a name="what-you-will-do"></a>Vad du ska göra

- Kör exempelprogrammet i hello gateway på din gateway som skickar meddelanden tooyour IoT-hubb.
- Kör exempelkod på värden datorn tooread meddelanden i Azure Table storage.

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

Hur gulp toouse hello verktyget toorun exempel kod tooread hälsningsmeddelande i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver

Du har nu hello följande uppgifter:

- [Skapat hello Azure funktionsapp och hello Azure storage-konto](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).
- [Kör exempelprogrammet i hello gateway](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).
- [Läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Hämta ditt Azure storage-anslutningssträngar

Tidigt i kursen skapat du ett Azure storage-konto. tooget hello anslutningssträngen för hello Azure storage-konto som kör hello följande kommandon:

* Visa en lista med alla lagringskonton.

```bash
az storage account list -g iot-gateway --query [].name
```

* Hämta anslutningssträngen för azure-lagring.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar hello värdet i lektionen 2.

## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning

Uppdatera hello `config-azure.json` filen så att hello exempelkod som körs på värddatorn för hello kan läsa meddelandet i Azure Table storage. tooconfigure Hej enhetsanslutning, gör du följande:

1. Öppna hello enheten konfigurationsfilen `config-azure.json` genom att köra följande kommandon hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguration](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Ersätt `[Azure storage connection string]` med hello Azure storage-anslutningssträngen som du fick.

   `[IoT hub connection string]`redan ska ersättas i avsnittet [läsa meddelanden från Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) i lektion3.

## <a name="read-messages-in-your-azure-table-storage"></a>Läsa meddelanden i Azure-tabellagring

Kör exempelprogrammet i hello gateway och läsa Azure Table storage meddelanden hello följande kommando:

```bash
gulp run --table-storage
```

Din IoT-hubb startar din Azure-funktion toosave-meddelande till din Azure Table storage när nytt meddelande tas emot.
Hej `gulp run` körs gateway exempelprogram som skickar meddelanden tooyour IoT-hubb. Med `table-storage` parameter, den också skapas en underordnad process tooreceive hello sparas meddelandet i Azure Table storage.

hello-meddelanden som skickas och tas emot är alla visas direkt på hello samma konsolfönstret i hello värddatorn. hello exempel programinstansen avslutas automatiskt i 40 sekunder.

   ![gulp Läs](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a>Sammanfattning

Du har kört hello exempel tooread hello meddelanden i din Azure Table storage sparas av ditt program för Azure-funktion.
