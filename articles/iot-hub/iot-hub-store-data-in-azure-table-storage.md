---
title: Spara dina IoT hub-meddelanden i Azure datalagring | Microsoft Docs
description: "Använda en funktionsapp i Azure-för att spara din IoT-hubb-meddelanden till Azure table storage. IoT-hubb meddelanden innehåller information, till exempel sensordata som skickas från IoT-enhet."
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
ms.openlocfilehash: 06503f9564e00ef62587d02f2da4778974e246c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-table-storage"></a>Spara IoT hub-meddelanden som innehåller sensordata till Azure table storage

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur du skapar ett Azure storage-konto och ett Azure funktionsapp att lagra IoT-hubb meddelanden i table storage.

## <a name="what-you-do"></a>Vad du gör

- Skapa ett Azure-lagringskonto.
- Förbered din IoT-hubb anslutning att läsa meddelanden.
- Skapa och distribuera en funktionsapp i Azure.

## <a name="what-you-need"></a>Vad du behöver

- [Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md) som täcker följande krav:
  - En aktiv Azure-prenumeration
  - En IoT-hubb i din prenumeration 
  - Ett program som körs som skickar meddelanden till din IoT-hubb

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **lagring** > **lagringskonto**  >   **Skapa**.

2. Ange informationen som krävs för lagringskontot:

   ![Skapa ett lagringskonto i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Namnet**: namnet på lagringskontot. Namnet måste vara globalt unikt.

   * **Resursgruppen**: använda samma resursgrupp som använder din IoT-hubb.

   * **Fäst på instrumentpanelen**: Välj det här alternativet för enkel åtkomst till IoT-hubben från instrumentpanelen.

3. Klicka på **Skapa**.

## <a name="prepare-your-iot-hub-connection-to-read-messages"></a>Förbered din IoT-hubb anslutning att läsa meddelanden

IoT-hubb visar en inbyggd händelse hub-kompatibel slutpunkt om du vill aktivera program för att läsa meddelanden för IoT-hubb. Under tiden kan använda program konsumentgrupper för att läsa data från IoT-hubb. Innan du skapar en funktionsapp till Azure-att läsa data från IoT-hubb kan du göra följande:

- Hämta anslutningssträngen för din IoT-hubb slutpunkt.
- Skapa en konsumentgrupp för din IoT-hubb.

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a>Hämta anslutningssträngen för din IoT-hubb-slutpunkt

1. Öppna din IoT-hubb.

2. På den **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.

3. I den högra rutan under **inbyggda slutpunkter**, klickar du på **händelser**.

4. I den **egenskaper** rutan Observera följande värden:
   - Event hub-kompatibel slutpunkt
   - Hubb-kompatibel händelsenamnet

   ![Hämta anslutningssträngen för din IoT-hubb slutpunkt i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. I den **IoT-hubb** rutan under **inställningar**, klickar du på **principer för delad åtkomst**.

6. Klicka på **iothubowner**.

7. Observera den **primärnyckel** värde.

8. Skapa anslutningssträngen för din IoT-hubb slutpunkt på följande sätt:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Ersätt `<Event Hub-compatible endpoint>` och `<Primary key>` med värden som du antecknade tidigare.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Skapa en konsumentgrupp för din IoT-hubb

1. Öppna din IoT-hubb.

2. I den **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.

3. I den högra rutan under **inbyggda slutpunkter**, klickar du på **händelser**.

4. I den **egenskaper** rutan under **konsumentgrupper**, ange ett namn och sedan anteckna den.

5. Klicka på **Spara**.

## <a name="create-and-deploy-an-azure-function-app"></a>Skapa och distribuera en funktionsapp i Azure

1. I den [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Compute** > **Funktionsapp**  >   **Skapa**.

2. Ange informationen som krävs för funktionen appen.

   ![Skapa en funktionsapp i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **Appnamn**: namnet på funktionen appen. Namnet måste vara globalt unikt.

   * **Resursgruppen**: använda samma resursgrupp som använder din IoT-hubb.

   * **Lagringskontot**: lagringskontot som du skapade.

   * **Fäst på instrumentpanelen**: Markera det här alternativet för enkel åtkomst till appen med funktionen från instrumentpanelen.

3. Klicka på **Skapa**.

4. När funktionen appen har skapats, öppna den.

5. Skapa en ny funktion i appen funktionen genom att göra följande:

   a. Klicka på **nya funktionen**.

   b. Välj **JavaScript** för **språk**, och **databearbetning** för **scenariot**.

   c. Klicka på **skapa den här funktionen**, och klicka sedan på **nya funktionen**.

   d. Välj **JavaScript** för språket och **databearbetning** för scenariot.

   e. Klicka på den **EventHubTrigger JavaScript** mall.

   f. Ange informationen som krävs för mallen.

      * **Namnge din funktion**: namnet på funktionen.

      * **Namnet på Händelsehubben**: hub-kompatibel händelsenamnet som du antecknade tidigare.

      * **Händelse-hubbanslutning**: Lägg till anslutningssträng IoT hub-slutpunkt som du har skapat, klicka på **ny**.

   g. Klicka på **Skapa**.

6. Konfigurera en utmatning för funktionen genom att göra följande:

   a. Klicka på **integrera** > **nya utgående** > **Azure Table Storage** > **Välj**.

      ![Lägg till tabellagring i appen funktionen i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Ange nödvändig information.

      * **Parametern tabellnamn**: Använd `outputTable`, som ska användas i funktionen Azure-kod.
      
      * **Tabellnamnet**: Använd `deviceData`.

      * **Storage-konto anslutning**: Klicka på **ny**, och välj eller ange ditt lagringskonto. Om lagringskontot inte visas, se [konto lagringskraven](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Klicka på **Spara**.

7. Under **utlösare**, klickar du på **Azure Event Hub (eventHubMessages)**.

8. Under **Händelsehubb konsumentgrupp**, ange namnet på konsumentgrupp som du skapade och klicka sedan på **spara**.

9. Klicka på funktionen som du har skapat till vänster och klicka sedan på **visa filer** till höger.

10. Ersätt Koden i `index.js` med följande:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in the IoT hub.
   // The message payload is persisted in an Azure storage table
 
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

Nu har du skapat funktionen appen. Meddelanden som din IoT-hubb som tar emot i table storage lagrar.

> [!NOTE]
> Du kan använda den **kör** för att testa funktionen appen. När du klickar på **kör**, skickas meddelandet till din IoT-hubb. Ankomsten av meddelandet ska utlösa funktionen appen för att starta och spara meddelandet till table storage. Den **loggar** fönstret registrerar information om processen.

## <a name="verify-your-message-in-your-table-storage"></a>Kontrollera meddelandet i table storage

1. Kör exempelprogrammet på enheten för att skicka meddelanden till din IoT-hubb.

2. [Hämta och installera Azure Lagringsutforskaren](http://storageexplorer.com/).

3. Öppna Lagringsutforskaren, klicka på **Lägg till ett Azure-konto** > **logga in**, och sedan logga in på ditt Azure-konto.

4. Klicka på Azure-prenumerationen > **Lagringskonton** > ditt lagringskonto > **tabeller** > **deviceData**.

   Du bör se meddelanden som skickas från enheten till din IoT-hubb som är inloggad på `deviceData` tabell.

## <a name="next-steps"></a>Nästa steg

Du har skapat ditt Azure storage-konto och Azure funktionsapp som lagrar meddelanden som din IoT-hubb som tar emot i table storage.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
