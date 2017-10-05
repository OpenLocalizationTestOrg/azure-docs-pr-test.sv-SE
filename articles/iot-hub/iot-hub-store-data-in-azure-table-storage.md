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
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-table-storage"></a><span data-ttu-id="57482-105">Spara IoT hub-meddelanden som innehåller sensordata till Azure table storage</span><span class="sxs-lookup"><span data-stu-id="57482-105">Save IoT hub messages that contain sensor data to your Azure table storage</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="57482-107">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="57482-107">What you learn</span></span>

<span data-ttu-id="57482-108">Du lär dig hur du skapar ett Azure storage-konto och ett Azure funktionsapp att lagra IoT-hubb meddelanden i table storage.</span><span class="sxs-lookup"><span data-stu-id="57482-108">You learn how to create an Azure storage account and an Azure function app to store IoT hub messages in your table storage.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="57482-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="57482-109">What you do</span></span>

- <span data-ttu-id="57482-110">Skapa ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="57482-110">Create an Azure storage account.</span></span>
- <span data-ttu-id="57482-111">Förbered din IoT-hubb anslutning att läsa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="57482-111">Prepare your IoT hub connection to read messages.</span></span>
- <span data-ttu-id="57482-112">Skapa och distribuera en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="57482-112">Create and deploy an Azure function app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="57482-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="57482-113">What you need</span></span>

- <span data-ttu-id="57482-114">[Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md) som täcker följande krav:</span><span class="sxs-lookup"><span data-stu-id="57482-114">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md) to cover the following requirements:</span></span>
  - <span data-ttu-id="57482-115">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="57482-115">An active Azure subscription</span></span>
  - <span data-ttu-id="57482-116">En IoT-hubb i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="57482-116">An IoT hub under your subscription</span></span> 
  - <span data-ttu-id="57482-117">Ett program som körs som skickar meddelanden till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="57482-117">A running application that sends messages to your IoT hub</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="57482-118">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="57482-118">Create an Azure storage account</span></span>

1. <span data-ttu-id="57482-119">I den [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **lagring** > **lagringskonto**  >   **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="57482-119">In the [Azure portal](https://portal.azure.com/), click **New** > **Storage** > **Storage account** > **Create**.</span></span>

2. <span data-ttu-id="57482-120">Ange informationen som krävs för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="57482-120">Enter the necessary information for the storage account:</span></span>

   ![Skapa ett lagringskonto i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * <span data-ttu-id="57482-122">**Namnet**: namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="57482-122">**Name**: The name of the storage account.</span></span> <span data-ttu-id="57482-123">Namnet måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="57482-123">The name must be globally unique.</span></span>

   * <span data-ttu-id="57482-124">**Resursgruppen**: använda samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-124">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="57482-125">**Fäst på instrumentpanelen**: Välj det här alternativet för enkel åtkomst till IoT-hubben från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="57482-125">**Pin to dashboard**: Select this option for easy access to your IoT hub from the dashboard.</span></span>

3. <span data-ttu-id="57482-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="57482-126">Click **Create**.</span></span>

## <a name="prepare-your-iot-hub-connection-to-read-messages"></a><span data-ttu-id="57482-127">Förbered din IoT-hubb anslutning att läsa meddelanden</span><span class="sxs-lookup"><span data-stu-id="57482-127">Prepare your IoT hub connection to read messages</span></span>

<span data-ttu-id="57482-128">IoT-hubb visar en inbyggd händelse hub-kompatibel slutpunkt om du vill aktivera program för att läsa meddelanden för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-128">IoT hub exposes a built-in event hub-compatible endpoint to enable applications to read IoT hub messages.</span></span> <span data-ttu-id="57482-129">Under tiden kan använda program konsumentgrupper för att läsa data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-129">Meanwhile, applications use consumer groups to read data from your IoT hub.</span></span> <span data-ttu-id="57482-130">Innan du skapar en funktionsapp till Azure-att läsa data från IoT-hubb kan du göra följande:</span><span class="sxs-lookup"><span data-stu-id="57482-130">Before you create an Azure function app to read data from your IoT hub, do the following:</span></span>

- <span data-ttu-id="57482-131">Hämta anslutningssträngen för din IoT-hubb slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="57482-131">Get the connection string of your IoT hub endpoint.</span></span>
- <span data-ttu-id="57482-132">Skapa en konsumentgrupp för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-132">Create a consumer group for your IoT hub.</span></span>

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a><span data-ttu-id="57482-133">Hämta anslutningssträngen för din IoT-hubb-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="57482-133">Get the connection string of your IoT hub endpoint</span></span>

1. <span data-ttu-id="57482-134">Öppna din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-134">Open your IoT hub.</span></span>

2. <span data-ttu-id="57482-135">På den **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="57482-135">On the **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="57482-136">I den högra rutan under **inbyggda slutpunkter**, klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="57482-136">In the right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="57482-137">I den **egenskaper** rutan Observera följande värden:</span><span class="sxs-lookup"><span data-stu-id="57482-137">In the **Properties** pane, note the following values:</span></span>
   - <span data-ttu-id="57482-138">Event hub-kompatibel slutpunkt</span><span class="sxs-lookup"><span data-stu-id="57482-138">Event hub-compatible endpoint</span></span>
   - <span data-ttu-id="57482-139">Hubb-kompatibel händelsenamnet</span><span class="sxs-lookup"><span data-stu-id="57482-139">Event hub-compatible name</span></span>

   ![Hämta anslutningssträngen för din IoT-hubb slutpunkt i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. <span data-ttu-id="57482-141">I den **IoT-hubb** rutan under **inställningar**, klickar du på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="57482-141">In the **IoT Hub** pane, under **Settings**, click **Shared access policies**.</span></span>

6. <span data-ttu-id="57482-142">Klicka på **iothubowner**.</span><span class="sxs-lookup"><span data-stu-id="57482-142">Click **iothubowner**.</span></span>

7. <span data-ttu-id="57482-143">Observera den **primärnyckel** värde.</span><span class="sxs-lookup"><span data-stu-id="57482-143">Note the **Primary key** value.</span></span>

8. <span data-ttu-id="57482-144">Skapa anslutningssträngen för din IoT-hubb slutpunkt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="57482-144">Create the connection string of your IoT hub endpoint as follows:</span></span>

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > <span data-ttu-id="57482-145">Ersätt `<Event Hub-compatible endpoint>` och `<Primary key>` med värden som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="57482-145">Replace `<Event Hub-compatible endpoint>` and `<Primary key>` with the values that you noted earlier.</span></span>

### <a name="create-a-consumer-group-for-your-iot-hub"></a><span data-ttu-id="57482-146">Skapa en konsumentgrupp för din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="57482-146">Create a consumer group for your IoT hub</span></span>

1. <span data-ttu-id="57482-147">Öppna din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-147">Open your IoT hub.</span></span>

2. <span data-ttu-id="57482-148">I den **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="57482-148">In the **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="57482-149">I den högra rutan under **inbyggda slutpunkter**, klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="57482-149">In the right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="57482-150">I den **egenskaper** rutan under **konsumentgrupper**, ange ett namn och sedan anteckna den.</span><span class="sxs-lookup"><span data-stu-id="57482-150">In the **Properties** pane, under **Consumer groups**, enter a name, and then make a note of it.</span></span>

5. <span data-ttu-id="57482-151">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="57482-151">Click **Save**.</span></span>

## <a name="create-and-deploy-an-azure-function-app"></a><span data-ttu-id="57482-152">Skapa och distribuera en funktionsapp i Azure</span><span class="sxs-lookup"><span data-stu-id="57482-152">Create and deploy an Azure function app</span></span>

1. <span data-ttu-id="57482-153">I den [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Compute** > **Funktionsapp**  >   **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="57482-153">In the [Azure portal](https://portal.azure.com/), click **New** > **Compute** > **Function App** > **Create**.</span></span>

2. <span data-ttu-id="57482-154">Ange informationen som krävs för funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="57482-154">Enter the necessary information for the function app.</span></span>

   ![Skapa en funktionsapp i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * <span data-ttu-id="57482-156">**Appnamn**: namnet på funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="57482-156">**App name**: The name of the function app.</span></span> <span data-ttu-id="57482-157">Namnet måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="57482-157">The name must be globally unique.</span></span>

   * <span data-ttu-id="57482-158">**Resursgruppen**: använda samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-158">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="57482-159">**Lagringskontot**: lagringskontot som du skapade.</span><span class="sxs-lookup"><span data-stu-id="57482-159">**Storage Account**: The storage account that you created.</span></span>

   * <span data-ttu-id="57482-160">**Fäst på instrumentpanelen**: Markera det här alternativet för enkel åtkomst till appen med funktionen från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="57482-160">**Pin to dashboard**: Check this option for easy access to the function app from the dashboard.</span></span>

3. <span data-ttu-id="57482-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="57482-161">Click **Create**.</span></span>

4. <span data-ttu-id="57482-162">När funktionen appen har skapats, öppna den.</span><span class="sxs-lookup"><span data-stu-id="57482-162">After the function app has been created, open it.</span></span>

5. <span data-ttu-id="57482-163">Skapa en ny funktion i appen funktionen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="57482-163">In the function app, create a new function by doing the following:</span></span>

   <span data-ttu-id="57482-164">a.</span><span class="sxs-lookup"><span data-stu-id="57482-164">a.</span></span> <span data-ttu-id="57482-165">Klicka på **nya funktionen**.</span><span class="sxs-lookup"><span data-stu-id="57482-165">Click **New Function**.</span></span>

   <span data-ttu-id="57482-166">b.</span><span class="sxs-lookup"><span data-stu-id="57482-166">b.</span></span> <span data-ttu-id="57482-167">Välj **JavaScript** för **språk**, och **databearbetning** för **scenariot**.</span><span class="sxs-lookup"><span data-stu-id="57482-167">Select **JavaScript** for **Language**, and **Data Processing** for **Scenario**.</span></span>

   <span data-ttu-id="57482-168">c.</span><span class="sxs-lookup"><span data-stu-id="57482-168">c.</span></span> <span data-ttu-id="57482-169">Klicka på **skapa den här funktionen**, och klicka sedan på **nya funktionen**.</span><span class="sxs-lookup"><span data-stu-id="57482-169">Click **Create this function**, and then click **New Function**.</span></span>

   <span data-ttu-id="57482-170">d.</span><span class="sxs-lookup"><span data-stu-id="57482-170">d.</span></span> <span data-ttu-id="57482-171">Välj **JavaScript** för språket och **databearbetning** för scenariot.</span><span class="sxs-lookup"><span data-stu-id="57482-171">Select **JavaScript** for the language, and **Data Processing** for the scenario.</span></span>

   <span data-ttu-id="57482-172">e.</span><span class="sxs-lookup"><span data-stu-id="57482-172">e.</span></span> <span data-ttu-id="57482-173">Klicka på den **EventHubTrigger JavaScript** mall.</span><span class="sxs-lookup"><span data-stu-id="57482-173">Click the **EventHubTrigger-JavaScript** template.</span></span>

   <span data-ttu-id="57482-174">f.</span><span class="sxs-lookup"><span data-stu-id="57482-174">f.</span></span> <span data-ttu-id="57482-175">Ange informationen som krävs för mallen.</span><span class="sxs-lookup"><span data-stu-id="57482-175">Enter the necessary information for the template.</span></span>

      * <span data-ttu-id="57482-176">**Namnge din funktion**: namnet på funktionen.</span><span class="sxs-lookup"><span data-stu-id="57482-176">**Name your function**: The name of the function.</span></span>

      * <span data-ttu-id="57482-177">**Namnet på Händelsehubben**: hub-kompatibel händelsenamnet som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="57482-177">**Event Hub name**: The event hub-compatible name that you noted earlier.</span></span>

      * <span data-ttu-id="57482-178">**Händelse-hubbanslutning**: Lägg till anslutningssträng IoT hub-slutpunkt som du har skapat, klicka på **ny**.</span><span class="sxs-lookup"><span data-stu-id="57482-178">**Event Hub connection**: To add the connection string of the IoT hub endpoint that you created, click **New**.</span></span>

   <span data-ttu-id="57482-179">g.</span><span class="sxs-lookup"><span data-stu-id="57482-179">g.</span></span> <span data-ttu-id="57482-180">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="57482-180">Click **Create**.</span></span>

6. <span data-ttu-id="57482-181">Konfigurera en utmatning för funktionen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="57482-181">Configure an output of the function by doing the following:</span></span>

   <span data-ttu-id="57482-182">a.</span><span class="sxs-lookup"><span data-stu-id="57482-182">a.</span></span> <span data-ttu-id="57482-183">Klicka på **integrera** > **nya utgående** > **Azure Table Storage** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="57482-183">Click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

      ![Lägg till tabellagring i appen funktionen i Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   <span data-ttu-id="57482-185">b.</span><span class="sxs-lookup"><span data-stu-id="57482-185">b.</span></span> <span data-ttu-id="57482-186">Ange nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="57482-186">Enter the necessary information.</span></span>

      * <span data-ttu-id="57482-187">**Parametern tabellnamn**: Använd `outputTable`, som ska användas i funktionen Azure-kod.</span><span class="sxs-lookup"><span data-stu-id="57482-187">**Table parameter name**: Use `outputTable`, which will be used in the Azure function's code.</span></span>
      
      * <span data-ttu-id="57482-188">**Tabellnamnet**: Använd `deviceData`.</span><span class="sxs-lookup"><span data-stu-id="57482-188">**Table name**: Use `deviceData`.</span></span>

      * <span data-ttu-id="57482-189">**Storage-konto anslutning**: Klicka på **ny**, och välj eller ange ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="57482-189">**Storage account connection**: Click **New**, and then select or enter your storage account.</span></span> <span data-ttu-id="57482-190">Om lagringskontot inte visas, se [konto lagringskraven](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="57482-190">If the storage account is not displayed, see [Storage account requirements](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span></span>
      
   <span data-ttu-id="57482-191">c.</span><span class="sxs-lookup"><span data-stu-id="57482-191">c.</span></span> <span data-ttu-id="57482-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="57482-192">Click **Save**.</span></span>

7. <span data-ttu-id="57482-193">Under **utlösare**, klickar du på **Azure Event Hub (eventHubMessages)**.</span><span class="sxs-lookup"><span data-stu-id="57482-193">Under **Triggers**, click **Azure Event Hub (eventHubMessages)**.</span></span>

8. <span data-ttu-id="57482-194">Under **Händelsehubb konsumentgrupp**, ange namnet på konsumentgrupp som du skapade och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="57482-194">Under **Event Hub consumer group**, enter the name of the consumer group that you created, and then click **Save**.</span></span>

9. <span data-ttu-id="57482-195">Klicka på funktionen som du har skapat till vänster och klicka sedan på **visa filer** till höger.</span><span class="sxs-lookup"><span data-stu-id="57482-195">Click the function you've created on the left and then click **View Files** on the right.</span></span>

10. <span data-ttu-id="57482-196">Ersätt Koden i `index.js` med följande:</span><span class="sxs-lookup"><span data-stu-id="57482-196">Replace the code in `index.js` with the following:</span></span>

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

11. <span data-ttu-id="57482-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="57482-197">Click **Save**.</span></span>

<span data-ttu-id="57482-198">Nu har du skapat funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="57482-198">You have now created the function app.</span></span> <span data-ttu-id="57482-199">Meddelanden som din IoT-hubb som tar emot i table storage lagrar.</span><span class="sxs-lookup"><span data-stu-id="57482-199">It stores messages that your IoT hub receives in your table storage.</span></span>

> [!NOTE]
> <span data-ttu-id="57482-200">Du kan använda den **kör** för att testa funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="57482-200">You can use the **Run** button to test the function app.</span></span> <span data-ttu-id="57482-201">När du klickar på **kör**, skickas meddelandet till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-201">When you click **Run**, the test message is sent to your IoT hub.</span></span> <span data-ttu-id="57482-202">Ankomsten av meddelandet ska utlösa funktionen appen för att starta och spara meddelandet till table storage.</span><span class="sxs-lookup"><span data-stu-id="57482-202">The arrival of the message should trigger the function app to start and then save the message to your table storage.</span></span> <span data-ttu-id="57482-203">Den **loggar** fönstret registrerar information om processen.</span><span class="sxs-lookup"><span data-stu-id="57482-203">The **Logs** pane records the details of the process.</span></span>

## <a name="verify-your-message-in-your-table-storage"></a><span data-ttu-id="57482-204">Kontrollera meddelandet i table storage</span><span class="sxs-lookup"><span data-stu-id="57482-204">Verify your message in your table storage</span></span>

1. <span data-ttu-id="57482-205">Kör exempelprogrammet på enheten för att skicka meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="57482-205">Run the sample application on your device to send messages to your IoT hub.</span></span>

2. <span data-ttu-id="57482-206">[Hämta och installera Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="57482-206">[Download and install Azure Storage Explorer](http://storageexplorer.com/).</span></span>

3. <span data-ttu-id="57482-207">Öppna Lagringsutforskaren, klicka på **Lägg till ett Azure-konto** > **logga in**, och sedan logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="57482-207">Open Storage Explorer, click **Add an Azure Account** > **Sign in**, and then sign in to your Azure account.</span></span>

4. <span data-ttu-id="57482-208">Klicka på Azure-prenumerationen > **Lagringskonton** > ditt lagringskonto > **tabeller** > **deviceData**.</span><span class="sxs-lookup"><span data-stu-id="57482-208">Click your Azure subscription > **Storage Accounts** > your storage account > **Tables** > **deviceData**.</span></span>

   <span data-ttu-id="57482-209">Du bör se meddelanden som skickas från enheten till din IoT-hubb som är inloggad på `deviceData` tabell.</span><span class="sxs-lookup"><span data-stu-id="57482-209">You should see messages sent from your device to your IoT hub logged in the `deviceData` table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57482-210">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57482-210">Next steps</span></span>

<span data-ttu-id="57482-211">Du har skapat ditt Azure storage-konto och Azure funktionsapp som lagrar meddelanden som din IoT-hubb som tar emot i table storage.</span><span class="sxs-lookup"><span data-stu-id="57482-211">You’ve successfully created your Azure storage account and Azure function app, which stores messages that your IoT hub receives in your table storage.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
