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
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a><span data-ttu-id="35edd-105">Spara IoT hub-meddelanden som innehåller sensor data tooyour Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="35edd-105">Save IoT hub messages that contain sensor data tooyour Azure table storage</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="35edd-107">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="35edd-107">What you learn</span></span>

<span data-ttu-id="35edd-108">Du lär dig hur toocreate ett Azure storage-konto och ett Azure fungera toostore IoT-hubb meddelanden i table storage.</span><span class="sxs-lookup"><span data-stu-id="35edd-108">You learn how toocreate an Azure storage account and an Azure function app toostore IoT hub messages in your table storage.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="35edd-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="35edd-109">What you do</span></span>

- <span data-ttu-id="35edd-110">Skapa ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="35edd-110">Create an Azure storage account.</span></span>
- <span data-ttu-id="35edd-111">Förbered din IoT-hubb tooread felmeddelanden för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="35edd-111">Prepare your IoT hub connection tooread messages.</span></span>
- <span data-ttu-id="35edd-112">Skapa och distribuera en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="35edd-112">Create and deploy an Azure function app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="35edd-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="35edd-113">What you need</span></span>

- <span data-ttu-id="35edd-114">[Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="35edd-114">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello following requirements:</span></span>
  - <span data-ttu-id="35edd-115">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="35edd-115">An active Azure subscription</span></span>
  - <span data-ttu-id="35edd-116">En IoT-hubb i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="35edd-116">An IoT hub under your subscription</span></span> 
  - <span data-ttu-id="35edd-117">Ett program som körs som skickar meddelanden tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="35edd-117">A running application that sends messages tooyour IoT hub</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="35edd-118">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="35edd-118">Create an Azure storage account</span></span>

1. <span data-ttu-id="35edd-119">I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **lagring** > **lagringskonto**  >   **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35edd-119">In hello [Azure portal](https://portal.azure.com/), click **New** > **Storage** > **Storage account** > **Create**.</span></span>

2. <span data-ttu-id="35edd-120">Ange hello nödvändig information för hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="35edd-120">Enter hello necessary information for hello storage account:</span></span>

   ![Skapa ett lagringskonto i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * <span data-ttu-id="35edd-122">**Namnet**: hello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="35edd-122">**Name**: hello name of hello storage account.</span></span> <span data-ttu-id="35edd-123">hello namn måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="35edd-123">hello name must be globally unique.</span></span>

   * <span data-ttu-id="35edd-124">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-124">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="35edd-125">**PIN-kod toodashboard**: Välj det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="35edd-125">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

3. <span data-ttu-id="35edd-126">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35edd-126">Click **Create**.</span></span>

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a><span data-ttu-id="35edd-127">Förbered din IoT-hubb tooread anslutningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="35edd-127">Prepare your IoT hub connection tooread messages</span></span>

<span data-ttu-id="35edd-128">IoT-hubb visar en inbyggd hub-kompatibel endpoint tooenable program tooread IoT hub händelsemeddelanden.</span><span class="sxs-lookup"><span data-stu-id="35edd-128">IoT hub exposes a built-in event hub-compatible endpoint tooenable applications tooread IoT hub messages.</span></span> <span data-ttu-id="35edd-129">Under tiden kan använda program konsumenten grupper tooread data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-129">Meanwhile, applications use consumer groups tooread data from your IoT hub.</span></span> <span data-ttu-id="35edd-130">Innan du skapar ett Azure-funktionen tooread AppData från din IoT-hubb hello följande:</span><span class="sxs-lookup"><span data-stu-id="35edd-130">Before you create an Azure function app tooread data from your IoT hub, do hello following:</span></span>

- <span data-ttu-id="35edd-131">Hämta hello anslutningssträngen för din IoT-hubb slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="35edd-131">Get hello connection string of your IoT hub endpoint.</span></span>
- <span data-ttu-id="35edd-132">Skapa en konsumentgrupp för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-132">Create a consumer group for your IoT hub.</span></span>

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a><span data-ttu-id="35edd-133">Hämta hello anslutningssträngen för din IoT-hubb-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="35edd-133">Get hello connection string of your IoT hub endpoint</span></span>

1. <span data-ttu-id="35edd-134">Öppna din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-134">Open your IoT hub.</span></span>

2. <span data-ttu-id="35edd-135">På hello **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="35edd-135">On hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="35edd-136">I hello Högerklicka rutan under **inbyggda slutpunkter**, klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="35edd-136">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="35edd-137">I hello **egenskaper** rutan, Observera hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="35edd-137">In hello **Properties** pane, note hello following values:</span></span>
   - <span data-ttu-id="35edd-138">Event hub-kompatibel slutpunkt</span><span class="sxs-lookup"><span data-stu-id="35edd-138">Event hub-compatible endpoint</span></span>
   - <span data-ttu-id="35edd-139">Hubb-kompatibel händelsenamnet</span><span class="sxs-lookup"><span data-stu-id="35edd-139">Event hub-compatible name</span></span>

   ![Hämta hello anslutningssträngen för din IoT-hubb slutpunkt i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. <span data-ttu-id="35edd-141">I hello **IoT-hubb** rutan under **inställningar**, klickar du på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="35edd-141">In hello **IoT Hub** pane, under **Settings**, click **Shared access policies**.</span></span>

6. <span data-ttu-id="35edd-142">Klicka på **iothubowner**.</span><span class="sxs-lookup"><span data-stu-id="35edd-142">Click **iothubowner**.</span></span>

7. <span data-ttu-id="35edd-143">Obs hello **primärnyckel** värde.</span><span class="sxs-lookup"><span data-stu-id="35edd-143">Note hello **Primary key** value.</span></span>

8. <span data-ttu-id="35edd-144">Skapa hello anslutningssträngen för din IoT-hubb slutpunkt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="35edd-144">Create hello connection string of your IoT hub endpoint as follows:</span></span>

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > <span data-ttu-id="35edd-145">Ersätt `<Event Hub-compatible endpoint>` och `<Primary key>` med hello-värden som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="35edd-145">Replace `<Event Hub-compatible endpoint>` and `<Primary key>` with hello values that you noted earlier.</span></span>

### <a name="create-a-consumer-group-for-your-iot-hub"></a><span data-ttu-id="35edd-146">Skapa en konsumentgrupp för din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="35edd-146">Create a consumer group for your IoT hub</span></span>

1. <span data-ttu-id="35edd-147">Öppna din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-147">Open your IoT hub.</span></span>

2. <span data-ttu-id="35edd-148">I hello **IoT-hubb** rutan under **Messaging**, klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="35edd-148">In hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="35edd-149">I hello Högerklicka rutan under **inbyggda slutpunkter**, klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="35edd-149">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="35edd-150">I hello **egenskaper** rutan under **konsumentgrupper**, ange ett namn och sedan anteckna den.</span><span class="sxs-lookup"><span data-stu-id="35edd-150">In hello **Properties** pane, under **Consumer groups**, enter a name, and then make a note of it.</span></span>

5. <span data-ttu-id="35edd-151">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="35edd-151">Click **Save**.</span></span>

## <a name="create-and-deploy-an-azure-function-app"></a><span data-ttu-id="35edd-152">Skapa och distribuera en funktionsapp i Azure</span><span class="sxs-lookup"><span data-stu-id="35edd-152">Create and deploy an Azure function app</span></span>

1. <span data-ttu-id="35edd-153">I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Compute** > **Funktionsapp**  >   **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35edd-153">In hello [Azure portal](https://portal.azure.com/), click **New** > **Compute** > **Function App** > **Create**.</span></span>

2. <span data-ttu-id="35edd-154">Ange hello nödvändig information för hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="35edd-154">Enter hello necessary information for hello function app.</span></span>

   ![Skapa en funktionsapp i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * <span data-ttu-id="35edd-156">**Appnamn**: hello namnet på hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="35edd-156">**App name**: hello name of hello function app.</span></span> <span data-ttu-id="35edd-157">hello namn måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="35edd-157">hello name must be globally unique.</span></span>

   * <span data-ttu-id="35edd-158">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-158">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="35edd-159">**Lagringskontot**: hello storage-konto som du skapade.</span><span class="sxs-lookup"><span data-stu-id="35edd-159">**Storage Account**: hello storage account that you created.</span></span>

   * <span data-ttu-id="35edd-160">**PIN-kod toodashboard**: Markera det här alternativet för enkel åtkomst toohello funktionsapp hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="35edd-160">**Pin toodashboard**: Check this option for easy access toohello function app from hello dashboard.</span></span>

3. <span data-ttu-id="35edd-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35edd-161">Click **Create**.</span></span>

4. <span data-ttu-id="35edd-162">När hello funktionsapp har skapats, öppna den.</span><span class="sxs-lookup"><span data-stu-id="35edd-162">After hello function app has been created, open it.</span></span>

5. <span data-ttu-id="35edd-163">Skapa en ny funktion i hello funktionsapp hello följande:</span><span class="sxs-lookup"><span data-stu-id="35edd-163">In hello function app, create a new function by doing hello following:</span></span>

   <span data-ttu-id="35edd-164">a.</span><span class="sxs-lookup"><span data-stu-id="35edd-164">a.</span></span> <span data-ttu-id="35edd-165">Klicka på **nya funktionen**.</span><span class="sxs-lookup"><span data-stu-id="35edd-165">Click **New Function**.</span></span>

   <span data-ttu-id="35edd-166">b.</span><span class="sxs-lookup"><span data-stu-id="35edd-166">b.</span></span> <span data-ttu-id="35edd-167">Välj **JavaScript** för **språk**, och **databearbetning** för **scenariot**.</span><span class="sxs-lookup"><span data-stu-id="35edd-167">Select **JavaScript** for **Language**, and **Data Processing** for **Scenario**.</span></span>

   <span data-ttu-id="35edd-168">c.</span><span class="sxs-lookup"><span data-stu-id="35edd-168">c.</span></span> <span data-ttu-id="35edd-169">Klicka på **skapa den här funktionen**, och klicka sedan på **nya funktionen**.</span><span class="sxs-lookup"><span data-stu-id="35edd-169">Click **Create this function**, and then click **New Function**.</span></span>

   <span data-ttu-id="35edd-170">d.</span><span class="sxs-lookup"><span data-stu-id="35edd-170">d.</span></span> <span data-ttu-id="35edd-171">Välj **JavaScript** för hello språk och **databearbetning** hello scenariot.</span><span class="sxs-lookup"><span data-stu-id="35edd-171">Select **JavaScript** for hello language, and **Data Processing** for hello scenario.</span></span>

   <span data-ttu-id="35edd-172">e.</span><span class="sxs-lookup"><span data-stu-id="35edd-172">e.</span></span> <span data-ttu-id="35edd-173">Klicka på hello **EventHubTrigger JavaScript** mall.</span><span class="sxs-lookup"><span data-stu-id="35edd-173">Click hello **EventHubTrigger-JavaScript** template.</span></span>

   <span data-ttu-id="35edd-174">f.</span><span class="sxs-lookup"><span data-stu-id="35edd-174">f.</span></span> <span data-ttu-id="35edd-175">Ange hello nödvändig information för hello mallen.</span><span class="sxs-lookup"><span data-stu-id="35edd-175">Enter hello necessary information for hello template.</span></span>

      * <span data-ttu-id="35edd-176">**Namnge din funktion**: hello namnet på hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="35edd-176">**Name your function**: hello name of hello function.</span></span>

      * <span data-ttu-id="35edd-177">**Namnet på Händelsehubben**: hello händelse hub-kompatibelt namn som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="35edd-177">**Event Hub name**: hello event hub-compatible name that you noted earlier.</span></span>

      * <span data-ttu-id="35edd-178">**Händelse-hubbanslutning**: tooadd hello anslutningssträngen för hello IoT-hubb slutpunkt som du skapade, klicka på **ny**.</span><span class="sxs-lookup"><span data-stu-id="35edd-178">**Event Hub connection**: tooadd hello connection string of hello IoT hub endpoint that you created, click **New**.</span></span>

   <span data-ttu-id="35edd-179">g.</span><span class="sxs-lookup"><span data-stu-id="35edd-179">g.</span></span> <span data-ttu-id="35edd-180">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35edd-180">Click **Create**.</span></span>

6. <span data-ttu-id="35edd-181">Konfigurera utdata för hello funktionen hello följande:</span><span class="sxs-lookup"><span data-stu-id="35edd-181">Configure an output of hello function by doing hello following:</span></span>

   <span data-ttu-id="35edd-182">a.</span><span class="sxs-lookup"><span data-stu-id="35edd-182">a.</span></span> <span data-ttu-id="35edd-183">Klicka på **integrera** > **nya utgående** > **Azure Table Storage** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="35edd-183">Click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

      ![Lägg till tabell lagring tooyour funktionsapp i hello Azure-portalen](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   <span data-ttu-id="35edd-185">b.</span><span class="sxs-lookup"><span data-stu-id="35edd-185">b.</span></span> <span data-ttu-id="35edd-186">Ange hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="35edd-186">Enter hello necessary information.</span></span>

      * <span data-ttu-id="35edd-187">**Parametern tabellnamn**: Använd `outputTable`, som ska användas i hello Azure funktionens kod.</span><span class="sxs-lookup"><span data-stu-id="35edd-187">**Table parameter name**: Use `outputTable`, which will be used in hello Azure function's code.</span></span>
      
      * <span data-ttu-id="35edd-188">**Tabellnamnet**: Använd `deviceData`.</span><span class="sxs-lookup"><span data-stu-id="35edd-188">**Table name**: Use `deviceData`.</span></span>

      * <span data-ttu-id="35edd-189">**Storage-konto anslutning**: Klicka på **ny**, och välj eller ange ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="35edd-189">**Storage account connection**: Click **New**, and then select or enter your storage account.</span></span> <span data-ttu-id="35edd-190">Om inte hello storage-konto visas finns [konto lagringskraven](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="35edd-190">If hello storage account is not displayed, see [Storage account requirements](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span></span>
      
   <span data-ttu-id="35edd-191">c.</span><span class="sxs-lookup"><span data-stu-id="35edd-191">c.</span></span> <span data-ttu-id="35edd-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="35edd-192">Click **Save**.</span></span>

7. <span data-ttu-id="35edd-193">Under **utlösare**, klickar du på **Azure Event Hub (eventHubMessages)**.</span><span class="sxs-lookup"><span data-stu-id="35edd-193">Under **Triggers**, click **Azure Event Hub (eventHubMessages)**.</span></span>

8. <span data-ttu-id="35edd-194">Under **Händelsehubb konsumentgrupp**, ange hello namnet på hello konsumentgrupp som du skapade och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="35edd-194">Under **Event Hub consumer group**, enter hello name of hello consumer group that you created, and then click **Save**.</span></span>

9. <span data-ttu-id="35edd-195">Hello-funktionen som du har skapat på hello vänster och klicka sedan på **visa filer** på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="35edd-195">Click hello function you've created on hello left and then click **View Files** on hello right.</span></span>

10. <span data-ttu-id="35edd-196">Ersätt hello koden i `index.js` med hello följande:</span><span class="sxs-lookup"><span data-stu-id="35edd-196">Replace hello code in `index.js` with hello following:</span></span>

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

11. <span data-ttu-id="35edd-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="35edd-197">Click **Save**.</span></span>

<span data-ttu-id="35edd-198">Nu har du skapat hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="35edd-198">You have now created hello function app.</span></span> <span data-ttu-id="35edd-199">Meddelanden som din IoT-hubb som tar emot i table storage lagrar.</span><span class="sxs-lookup"><span data-stu-id="35edd-199">It stores messages that your IoT hub receives in your table storage.</span></span>

> [!NOTE]
> <span data-ttu-id="35edd-200">Du kan använda hello **kör** knappen tootest hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="35edd-200">You can use hello **Run** button tootest hello function app.</span></span> <span data-ttu-id="35edd-201">När du klickar på **kör**, hello testmeddelande skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-201">When you click **Run**, hello test message is sent tooyour IoT hub.</span></span> <span data-ttu-id="35edd-202">hello hello-meddelande har ankommit ska utlösa hello funktionen app toostart och spara hello meddelandet tooyour tabellagring.</span><span class="sxs-lookup"><span data-stu-id="35edd-202">hello arrival of hello message should trigger hello function app toostart and then save hello message tooyour table storage.</span></span> <span data-ttu-id="35edd-203">Hej **loggar** fönstret registrerar hello information om hello-processen.</span><span class="sxs-lookup"><span data-stu-id="35edd-203">hello **Logs** pane records hello details of hello process.</span></span>

## <a name="verify-your-message-in-your-table-storage"></a><span data-ttu-id="35edd-204">Kontrollera meddelandet i table storage</span><span class="sxs-lookup"><span data-stu-id="35edd-204">Verify your message in your table storage</span></span>

1. <span data-ttu-id="35edd-205">Kör exempelprogrammet hello på din enhet toosend meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="35edd-205">Run hello sample application on your device toosend messages tooyour IoT hub.</span></span>

2. <span data-ttu-id="35edd-206">[Hämta och installera Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="35edd-206">[Download and install Azure Storage Explorer](http://storageexplorer.com/).</span></span>

3. <span data-ttu-id="35edd-207">Öppna Lagringsutforskaren, klicka på **Lägg till ett Azure-konto** > **logga in**, och logga sedan in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="35edd-207">Open Storage Explorer, click **Add an Azure Account** > **Sign in**, and then sign in tooyour Azure account.</span></span>

4. <span data-ttu-id="35edd-208">Klicka på Azure-prenumerationen > **Lagringskonton** > ditt lagringskonto > **tabeller** > **deviceData**.</span><span class="sxs-lookup"><span data-stu-id="35edd-208">Click your Azure subscription > **Storage Accounts** > your storage account > **Tables** > **deviceData**.</span></span>

   <span data-ttu-id="35edd-209">Du bör se meddelanden som skickas från din enhet tooyour IoT-hubb inloggad hello `deviceData` tabell.</span><span class="sxs-lookup"><span data-stu-id="35edd-209">You should see messages sent from your device tooyour IoT hub logged in hello `deviceData` table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35edd-210">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35edd-210">Next steps</span></span>

<span data-ttu-id="35edd-211">Du har skapat ditt Azure storage-konto och Azure funktionsapp som lagrar meddelanden som din IoT-hubb som tar emot i table storage.</span><span class="sxs-lookup"><span data-stu-id="35edd-211">You’ve successfully created your Azure storage account and Azure function app, which stores messages that your IoT hub receives in your table storage.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
