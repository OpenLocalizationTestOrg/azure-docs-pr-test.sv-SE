---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 3: Table storage | Microsoft Docs'
description: "Övervaka hello meddelanden från enhet till moln medan de skrivs tooyour Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Hämta data från molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 8c5558bb-3c31-4445-90e6-b1a978738545
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 307ce2bc595339790db7379cc011fe262c2b8734
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="9478b-104">Läs meddelandena kvar i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9478b-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="9478b-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="9478b-105">What you will do</span></span>
<span data-ttu-id="9478b-106">Övervakaren hello enhet till moln meddelanden som skickas från hallon Pi 3 tooyour IoT-hubb som hälsningsmeddelande skrivs tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="9478b-106">Monitor hello device-to-cloud messages that are sent from Raspberry Pi 3 tooyour IoT hub as hello messages are written tooyour Azure Table storage.</span></span> <span data-ttu-id="9478b-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="9478b-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="9478b-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="9478b-108">What you will learn</span></span>
<span data-ttu-id="9478b-109">I den här artikeln får du lära dig hur toouse gulp Läs meddelandeaktiviteten tooread hälsningsmeddelande kvar i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="9478b-109">In this article, you will learn how toouse hello gulp read-message task tooread messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9478b-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="9478b-110">What you need</span></span>
<span data-ttu-id="9478b-111">Innan du påbörjar den här processen måste har slutförts [köra hello Azure blinka exempelprogrammet på hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md).</span><span class="sxs-lookup"><span data-stu-id="9478b-111">Before starting this process, you must have successfully completed [Run hello Azure blink sample application on Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md).</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="9478b-112">Läsa nya meddelanden från ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9478b-112">Read new messages from your storage account</span></span>
<span data-ttu-id="9478b-113">I föregående artikel hello körde du ett exempelprogram med Pi.</span><span class="sxs-lookup"><span data-stu-id="9478b-113">In hello previous article, you ran a sample application on Pi.</span></span> <span data-ttu-id="9478b-114">hello exempelprogrammet skickade meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9478b-114">hello sample application sent messages tooyour Azure IoT hub.</span></span> <span data-ttu-id="9478b-115">hello-meddelanden som skickas tooyour IoT-hubb som lagras i Azure-tabellagring via hello Azure funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9478b-115">hello messages sent tooyour IoT hub are stored into your Azure Table storage via hello Azure function app.</span></span> <span data-ttu-id="9478b-116">Du behöver Azure storage anslutning sträng tooread hälsningsmeddelande från Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="9478b-116">You need hello Azure storage connection string tooread messages from your Azure Table storage.</span></span>

<span data-ttu-id="9478b-117">tooread meddelanden i din Azure Table storage, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="9478b-117">tooread messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="9478b-118">Hämta hello anslutningssträng genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="9478b-118">Get hello connection string by running hello following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="9478b-119">hello första kommandot hämtar hello `storage name` som används i hello andra kommandot tooget hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9478b-119">hello first command retrieves hello `storage name` that is used in hello second command tooget hello connection string.</span></span> <span data-ttu-id="9478b-120">Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="9478b-120">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
2. <span data-ttu-id="9478b-121">Öppna hello konfigurationsfilen `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9478b-121">Open hello configuration file `config-raspberrypi.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. <span data-ttu-id="9478b-122">Ersätt `[Azure storage connection string]` med hello anslutningssträng som du fick i steg 1.</span><span class="sxs-lookup"><span data-stu-id="9478b-122">Replace `[Azure storage connection string]` with hello connection string you got in step 1.</span></span>
4. <span data-ttu-id="9478b-123">Spara hello `config-raspberrypi.json` fil.</span><span class="sxs-lookup"><span data-stu-id="9478b-123">Save hello `config-raspberrypi.json` file.</span></span>
5. <span data-ttu-id="9478b-124">Skicka meddelanden igen och läsa dem i Azure Table storage genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9478b-124">Send messages again and read them from your Azure Table storage by running hello following command:</span></span>
   
   ```bash
   gulp run --read-storage
   ```
   
   <span data-ttu-id="9478b-125">hello logik för att läsa från Azure Table storage är i hello `azure-table.js` fil.</span><span class="sxs-lookup"><span data-stu-id="9478b-125">hello logic for reading from Azure Table storage is in hello `azure-table.js` file.</span></span>
   
   ![gulp köras, Läs-lagring](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message_c.png)

## <a name="summary"></a><span data-ttu-id="9478b-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9478b-127">Summary</span></span>
<span data-ttu-id="9478b-128">Du har har anslutna Pi tooyour IoT-hubb i hello molnet och används hello blinka exempel toosend enhet till moln programmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="9478b-128">You've successfully connected Pi tooyour IoT hub in hello cloud and used hello blink sample application toosend device-to-cloud messages.</span></span> <span data-ttu-id="9478b-129">Du kan också användas hello Azure funktionen app toostore inkommande IoT-hubb meddelanden tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="9478b-129">You also used hello Azure function app toostore incoming IoT hub messages tooyour Azure Table storage.</span></span> <span data-ttu-id="9478b-130">Du kan nu skicka moln till enhet meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="9478b-130">You can now send cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9478b-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9478b-131">Next steps</span></span>
[<span data-ttu-id="9478b-132">Kör ett exempel programmet tooreceive meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="9478b-132">Run a sample application tooreceive cloud-to-device messages</span></span>](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md)

