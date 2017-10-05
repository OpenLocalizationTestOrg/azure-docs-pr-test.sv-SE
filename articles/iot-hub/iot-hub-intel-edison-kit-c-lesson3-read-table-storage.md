---
title: "Connect Intel EDISON (C) till Azure IoT - lektionen 3: övervaka meddelanden | Microsoft Docs"
description: "Övervaka meddelandena från enhet till moln medan de skrivs till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data i molnet, insamling av molnet, iot-Molntjänsten, iot-data"
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
ms.openlocfilehash: 249b5e0e96051fa2adeedfb9befd98fc939b4d40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="39b06-104">Läs meddelandena kvar i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="39b06-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="39b06-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="39b06-105">What you will do</span></span>
<span data-ttu-id="39b06-106">Övervaka enhet till moln-meddelanden som skickas från Intel modern till din IoT-hubb som meddelandena som skrivs till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="39b06-106">Monitor the device-to-cloud messages that are sent from Intel Edison to your IoT hub as the messages are written to your Azure Table storage.</span></span> <span data-ttu-id="39b06-107">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="39b06-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="39b06-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="39b06-108">What you will learn</span></span>
<span data-ttu-id="39b06-109">I den här artikeln får du lära dig hur du använder aktiviteten gulp läsa meddelandet för att läsa meddelanden kvar i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="39b06-109">In this article, you will learn how to use the gulp read-message task to read messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="39b06-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="39b06-110">What you need</span></span>
<span data-ttu-id="39b06-111">Innan du påbörjar den här processen måste har slutförts [kör exempelprogrammet Azure blinka på Intel modern][run-the-azure-blink-sample-application-on-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="39b06-111">Before starting this process, you must have successfully completed [Run the Azure blink sample application on Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="39b06-112">Läsa nya meddelanden från ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="39b06-112">Read new messages from your storage account</span></span>
<span data-ttu-id="39b06-113">I föregående artikel körde du ett exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="39b06-113">In the previous article, you ran a sample application on Edison.</span></span> <span data-ttu-id="39b06-114">Exempel programmet skickas meddelanden till din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="39b06-114">The sample application sent messages to your Azure IoT hub.</span></span> <span data-ttu-id="39b06-115">Meddelanden som skickas till din IoT-hubb som lagras i Azure-tabellagring via appen Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="39b06-115">The messages sent to your IoT hub are stored into your Azure Table storage via the Azure function app.</span></span> <span data-ttu-id="39b06-116">Du måste anslutningssträngen för Azure storage att läsa meddelanden från Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="39b06-116">You need the Azure storage connection string to read messages from your Azure Table storage.</span></span>

<span data-ttu-id="39b06-117">Följ dessa steg om du vill läsa meddelanden i Azure Table storage:</span><span class="sxs-lookup"><span data-stu-id="39b06-117">To read messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="39b06-118">Hämta anslutningssträngen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="39b06-118">Get the connection string by running the following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="39b06-119">Det första kommandot hämtar den `storage name` som används i det andra kommandot för att hämta anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="39b06-119">The first command retrieves the `storage name` that is used in the second command to get the connection string.</span></span> <span data-ttu-id="39b06-120">Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="39b06-120">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>
2. <span data-ttu-id="39b06-121">Öppna konfigurationsfilen `config-edison.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="39b06-121">Open the configuration file `config-edison.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. <span data-ttu-id="39b06-122">Ersätt `[Azure storage connection string]` med den anslutningssträng som du fick i steg 1.</span><span class="sxs-lookup"><span data-stu-id="39b06-122">Replace `[Azure storage connection string]` with the connection string you got in step 1.</span></span>
4. <span data-ttu-id="39b06-123">Spara den `config-edison.json` filen.</span><span class="sxs-lookup"><span data-stu-id="39b06-123">Save the `config-edison.json` file.</span></span>
5. <span data-ttu-id="39b06-124">Skicka meddelanden igen och läsa dem i Azure Table storage genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="39b06-124">Send messages again and read them from your Azure Table storage by running the following command:</span></span>

   ```bash
   gulp run --read-storage
   ```

   <span data-ttu-id="39b06-125">Logik för att läsa från Azure Table storage är i den `azure-table.js` filen.</span><span class="sxs-lookup"><span data-stu-id="39b06-125">The logic for reading from Azure Table storage is in the `azure-table.js` file.</span></span>

   ![gulp köras, Läs-lagring][gulp run]

## <a name="summary"></a><span data-ttu-id="39b06-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="39b06-127">Summary</span></span>
<span data-ttu-id="39b06-128">Du har korrekt anslutna modern till din IoT-hubb i molnet och används exempelprogrammet blinka för att skicka meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="39b06-128">You've successfully connected Edison to your IoT hub in the cloud and used the blink sample application to send device-to-cloud messages.</span></span> <span data-ttu-id="39b06-129">Du kan också används appen Azure funktionen för att lagra inkommande meddelanden i IoT-hubb till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="39b06-129">You also used the Azure function app to store incoming IoT hub messages to your Azure Table storage.</span></span> <span data-ttu-id="39b06-130">Du kan nu skicka moln till enhet meddelanden från din IoT-hubb för modern.</span><span class="sxs-lookup"><span data-stu-id="39b06-130">You can now send cloud-to-device messages from your IoT hub to Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39b06-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39b06-131">Next steps</span></span>
<span data-ttu-id="39b06-132">[Kör ett exempelprogram som tar emot meddelanden moln till enhet][receive-cloud-to-device-messages]</span><span class="sxs-lookup"><span data-stu-id="39b06-132">[Run a sample application to receive cloud-to-device messages][receive-cloud-to-device-messages]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md