---
title: aaaSave IoT-hubb meddelanden tooAzure datalagring | Microsoft Docs
description: "Använda en Azure-funktion app toosave din IoT-hubb meddelanden tooyour Azure-tabellagring. hälsningsmeddelande IoT-hubb innehålla information, till exempel sensordata som skickas från IoT-enhet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-datalagring, datalagring för iot-temperatursensor"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: be72d9ba9a781822926364351b50f58f5d96e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a>Spara IoT hub-meddelanden som innehåller sensor data tooyour Azure-tabellagring

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur toocreate ett Azure storage-konto och ett Azure fungera toostore IoT-hubb meddelanden i table storage.

## <a name="what-you-do"></a>Vad du gör

- Skapa ett Azure-lagringskonto.
- Förbered din IoT-hubb tooread felmeddelanden för anslutningen.
- Skapa och distribuera en funktionsapp i Azure.

## <a name="what-you-need"></a>Vad du behöver

- [Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello följande krav:
  - En aktiv Azure-prenumeration
  - En IoT-hubb i din prenumeration 
  - Ett program som körs som skickar meddelanden tooyour IoT-hubb

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **lagring** > **lagringskonto**  >   **Skapa**.

2. Ange hello nödvändig information för hello storage-konto:

   ![Skapa ett lagringskonto i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Namnet**: hello namnet på hello storage-konto. hello namn måste vara globalt unika.

   * **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   * **PIN-kod toodashboard**: Välj det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.

3. Klicka på **Skapa**.

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a>Förbered din IoT-hubb tooread anslutningsmeddelanden

IoT-hubb visar en inbyggd hub-kompatibel endpoint tooenable program tooread IoT hub händelsemeddelanden. Under tiden kan använda program konsumenten grupper tooread data från IoT-hubb. Innan du skapar ett Azure-funktionen tooread AppData från din IoT-hubb hello följande:

- Hämta hello anslutningssträngen för din IoT-hubb slutpunkt.
- Skapa en konsumentgrupp för din IoT-hubb.

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a>Hämta hello anslutningssträngen för din IoT-hubb-slutpunkt

1. Öppna din IoT-hubb.

2. På hello **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.

3. I hello Högerklicka rutan under **inbyggda slutpunkter**, klickar du på **händelser**.

4. I hello **egenskaper** rutan, Observera hello följande värden:
   - Event hub-kompatibel slutpunkt
   - Hubb-kompatibel händelsenamnet

   ![Hämta hello anslutningssträngen för din IoT-hubb slutpunkt i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. I hello **IoT-hubb** rutan under **inställningar**, klickar du på **principer för delad åtkomst**.

6. Klicka på **iothubowner**.

7. Obs hello **primärnyckel** värde.

8. Skapa hello anslutningssträngen för din IoT-hubb slutpunkt på följande sätt:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Ersätt `<Event Hub-compatible endpoint>` och `<Primary key>` med hello-värden som du antecknade tidigare.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Skapa en konsumentgrupp för din IoT-hubb

1. Öppna din IoT-hubb.

2. I hello **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.

3. I hello Högerklicka rutan under **inbyggda slutpunkter**, klickar du på **händelser**.

4. I hello **egenskaper** rutan under **konsumentgrupper**, ange ett namn och sedan anteckna den.

5. Klicka på **Spara**.

## <a name="create-and-deploy-an-azure-function-app"></a>Skapa och distribuera en funktionsapp i Azure

1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Compute** > **Funktionsapp**  >   **Skapa**.

2. Ange hello nödvändig information för hello funktionsapp.

   ![Skapa en funktionsapp i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **Appnamn**: hello namnet på hello funktionsapp. hello namn måste vara globalt unika.

   * **Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.

   * **Lagringskontot**: hello storage-konto som du skapade.

   * **PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst toohello funktionsapp hello instrumentpanel.

3. Klicka på **Skapa**.

4. När hello funktionsapp har skapats, öppna den.

5. Skapa en ny funktion i hello funktionsapp hello följande:

   a. Klicka på **nya funktionen**.

   b. Välj **JavaScript** för **språk**, och **databearbetning** för **scenariot**.

   c. Klicka på **skapa den här funktionen**, och klicka sedan på **nya funktionen**.

   d. Välj **JavaScript** för hello språk och **databearbetning** hello scenariot.

   e. Klicka på hello **EventHubTrigger JavaScript** mall.

   f. Ange hello nödvändig information för hello mallen.

      * **Namnge din funktion**: hello namnet på hello-funktion.

      * **Namnet på Händelsehubben**: hello händelse hub-kompatibelt namn som du antecknade tidigare.

      * **Händelse-hubbanslutning**: tooadd hello anslutningssträngen för hello IoT-hubb slutpunkt som du skapade, klicka på **ny**.

   g. Klicka på **Skapa**.

6. Konfigurera utdata för hello funktionen hello följande:

   a. Klicka på **integrera** > **nya utgående** > **Azure Table Storage** > **Välj**.

      ![Lägg till tabell lagring tooyour funktionsapp i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Ange hello nödvändig information.

      * **Parametern tabellnamn**: Använd `outputTable`, som ska användas i hello Azure funktionens kod.
      
      * **Tabellnamnet**: Använd `deviceData`.

      * **Storage-konto anslutning**: Klicka på **ny**, och välj eller ange ditt lagringskonto. Om inte hello storage-konto visas finns [konto lagringskraven](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Klicka på **Spara**.

7. Under **utlösare**, klickar du på **Azure Event Hub (eventHubMessages)**.

8. Under **Händelsehubb konsumentgrupp**, ange hello namnet på hello konsumentgrupp som du skapade och klicka sedan på **spara**.

9. Hello-funktionen som du har skapat på hello vänster och klicka sedan på **visa filer** på hello rätt.

10. Ersätt hello koden i `index.js` med hello följande:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in hello IoT hub.
   // hello message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. Klicka på **Spara**.

Nu har du skapat hello funktionsapp. Meddelanden som din IoT-hubb som tar emot i table storage lagrar.

> [!NOTE]
> Du kan använda hello **kör** knappen tootest hello funktionsapp. När du klickar på **kör**, hello testmeddelande skickas tooyour IoT-hubb. hello hello-meddelande har ankommit ska utlösa hello funktionen app toostart och spara hello meddelandet tooyour tabellagring. Hej **loggar** fönstret registrerar hello information om hello-processen.

## <a name="verify-your-message-in-your-table-storage"></a>Kontrollera meddelandet i table storage

1. Kör exempelprogrammet hello på din enhet toosend meddelanden tooyour IoT-hubb.

2. [Hämta och installera Azure Lagringsutforskaren](http://storageexplorer.com/).

3. Öppna Lagringsutforskaren, klicka på **Lägg till ett Azure-konto** > **logga in**, och logga sedan in tooyour Azure-konto.

4. Klicka på Azure-prenumerationen > **Lagringskonton** > ditt lagringskonto > **tabeller** > **deviceData**.

   Du bör se meddelanden som skickas från din enhet tooyour IoT-hubb inloggad hello `deviceData` tabell.

## <a name="next-steps"></a>Nästa steg

Du har skapat ditt Azure storage-konto och Azure funktionsapp som lagrar meddelanden som din IoT-hubb som tar emot i table storage.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
