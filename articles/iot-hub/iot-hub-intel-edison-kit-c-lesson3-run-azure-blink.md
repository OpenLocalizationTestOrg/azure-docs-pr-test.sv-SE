---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 3: skicka meddelanden | Microsoft Docs'
description: "Distribuera och köra en exempel programmet tooIntel modern som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 12672b64-795a-4dfc-86fd-df53ed3eeef7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 83615335027cc792b89d3894578fc3f7a55e5c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="e4a91-104">Kör ett exempel programmet toosend meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="e4a91-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="e4a91-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="e4a91-105">What you will do</span></span>
<span data-ttu-id="e4a91-106">Den här artikeln visar hur toodeploy och kör ett exempelprogram på Intel modern som skickar meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a91-106">This article will show you how toodeploy and run a sample application on Intel Edison that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="e4a91-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e4a91-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e4a91-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="e4a91-108">What you will learn</span></span>
<span data-ttu-id="e4a91-109">Du lär dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet C på modern.</span><span class="sxs-lookup"><span data-stu-id="e4a91-109">You will learn how toouse hello gulp tool toodeploy and run hello sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e4a91-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="e4a91-110">What you need</span></span>
* <span data-ttu-id="e4a91-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="e4a91-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="e4a91-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="e4a91-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="e4a91-113">anslutningssträngen för hello enhet är används tooconnect modern tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a91-113">hello device connection string is used tooconnect Edison tooyour IoT hub.</span></span> <span data-ttu-id="e4a91-114">Hej anslutningssträngen för IoT-hubben är används tooconnect din IoT-hubb toohello enhetsidentitet som representerar modern i hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a91-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents Edison in hello IoT hub.</span></span>

* <span data-ttu-id="e4a91-115">Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="e4a91-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="e4a91-116">Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="e4a91-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="e4a91-117">Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="e4a91-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="e4a91-118">`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat modern.</span><span class="sxs-lookup"><span data-stu-id="e4a91-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="e4a91-119">Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e4a91-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="e4a91-120">Använd `myinteledison` som hello värde för `{device id}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="e4a91-120">Use `myinteledison` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="e4a91-121">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="e4a91-121">Configure hello device connection</span></span>
1. <span data-ttu-id="e4a91-122">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="e4a91-122">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > <span data-ttu-id="e4a91-123">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="e4a91-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="e4a91-124">Öppna hello enheten konfigurationsfilen `config-edison.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e4a91-124">Open hello device configuration file `config-edison.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. <span data-ttu-id="e4a91-126">Se följande ersättningar i hello hello `config-edison.json` fil:</span><span class="sxs-lookup"><span data-stu-id="e4a91-126">Make hello following replacements in hello `config-edison.json` file:</span></span>

   * <span data-ttu-id="e4a91-127">Ersätt **[enhet värdnamn eller IP-adress]** med hello enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="e4a91-127">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="e4a91-128">Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="e4a91-128">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="e4a91-129">Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="e4a91-129">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e4a91-130">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e4a91-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="e4a91-131">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="e4a91-131">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="e4a91-132">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e4a91-132">Deploy and run hello sample application</span></span>
<span data-ttu-id="e4a91-133">Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e4a91-133">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="e4a91-134">Kontrollera att det fungerar hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e4a91-134">Verify that hello sample application works</span></span>
<span data-ttu-id="e4a91-135">Du bör se hello Indikator som är anslutna tooEdison blinka varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="e4a91-135">You should see hello LED that is connected tooEdison blinking every two seconds.</span></span> <span data-ttu-id="e4a91-136">Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a91-136">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="e4a91-137">Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="e4a91-137">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="e4a91-138">hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="e4a91-138">hello sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="e4a91-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e4a91-140">Summary</span></span>
<span data-ttu-id="e4a91-141">Du har distribuerat och köra hello nya blinka exempelprogrammet på modern toosend meddelanden från enhet till moln tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a91-141">You've deployed and run hello new blink sample application on Edison toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="e4a91-142">Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e4a91-142">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4a91-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4a91-143">Next steps</span></span>
<span data-ttu-id="e4a91-144">[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="e4a91-144">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md