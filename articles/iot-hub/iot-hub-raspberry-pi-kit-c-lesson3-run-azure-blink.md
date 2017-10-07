---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra en exempel programmet tooRaspberry Pi 3 som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "blinka ledde molnet pi, ledde blinka från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c484beb2e2d3a3cf19f071f2ba87b9a4fe41c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="7723d-104">Kör ett exempel programmet toosend meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="7723d-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="7723d-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="7723d-105">What you will do</span></span>
<span data-ttu-id="7723d-106">Den här artikeln visar hur toodeploy och kör ett exempelprogram på hallon Pi 3 som skickar meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7723d-106">This article will show you how toodeploy and run a sample application on Raspberry Pi 3 that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="7723d-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7723d-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7723d-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="7723d-108">What you will learn</span></span>
<span data-ttu-id="7723d-109">Du kommer lära dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet Node.js på Pi.</span><span class="sxs-lookup"><span data-stu-id="7723d-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7723d-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="7723d-110">What you need</span></span>
* <span data-ttu-id="7723d-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="7723d-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="7723d-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="7723d-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="7723d-113">anslutningssträngen för hello enheten används av din Pi tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7723d-113">hello device connection string is used by your Pi tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="7723d-114">hello anslutningssträngen för IoT-hubben är används tooconnect toohello identitetsregistret i din IoT-hubb toomanage hello enheter som beviljats tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7723d-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span> 

* <span data-ttu-id="7723d-115">Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="7723d-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="7723d-116">Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="7723d-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="7723d-117">Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="7723d-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="7723d-118">`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat Pi.</span><span class="sxs-lookup"><span data-stu-id="7723d-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="7723d-119">Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7723d-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="7723d-120">Använd `myraspberrypi` som hello värde för `{device id}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="7723d-120">Use `myraspberrypi` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="7723d-121">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="7723d-121">Configure hello device connection</span></span>
1. <span data-ttu-id="7723d-122">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="7723d-122">Initialize hello configuration file by running hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> <span data-ttu-id="7723d-123">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="7723d-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="7723d-124">Öppna hello enheten konfigurationsfilen `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7723d-124">Open hello device configuration file `config-raspberrypi.json` in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="7723d-126">Se följande ersättningar i hello hello `config-raspberrypi.json` fil:</span><span class="sxs-lookup"><span data-stu-id="7723d-126">Make hello following replacements in hello `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="7723d-127">Ersätt **[enhet värdnamn eller IP-adress]** med hello IP-adressen eller värdnamnet enhetsnamn som du fick från `device-discovery-cli` eller med hello-värde som överförs när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="7723d-127">Replace **[device hostname or IP address]** with hello device IP address or host name you got from `device-discovery-cli` or with hello value inherited when you configured your device.</span></span>
   * <span data-ttu-id="7723d-128">Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="7723d-128">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="7723d-129">Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="7723d-129">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

> [!NOTE]
> <span data-ttu-id="7723d-130">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7723d-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="7723d-131">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="7723d-131">Keep it as is.</span></span>

<span data-ttu-id="7723d-132">Uppdatera hello `config-raspberrypi.json` filen så att du kan distribuera hello exempelprogrammet från datorn.</span><span class="sxs-lookup"><span data-stu-id="7723d-132">Update hello `config-raspberrypi.json` file so that you can deploy hello sample application from your computer.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="7723d-133">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="7723d-133">Deploy and run hello sample application</span></span>
<span data-ttu-id="7723d-134">Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7723d-134">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="7723d-135">Kontrollera att det fungerar hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="7723d-135">Verify that hello sample application works</span></span>
<span data-ttu-id="7723d-136">Du bör se hello Indikator som är anslutna tooPi blinka varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="7723d-136">You should see hello LED that is connected tooPi blinking every two seconds.</span></span> <span data-ttu-id="7723d-137">Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7723d-137">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="7723d-138">Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="7723d-138">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="7723d-139">hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="7723d-139">hello sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a><span data-ttu-id="7723d-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7723d-141">Summary</span></span>
<span data-ttu-id="7723d-142">Du har distribuerat och köra hello nya blinka exempelprogrammet på Pi toosend meddelanden från enhet till moln tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7723d-142">You've deployed and run hello new blink sample application on Pi toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="7723d-143">Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7723d-143">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7723d-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7723d-144">Next steps</span></span>
[<span data-ttu-id="7723d-145">Läs meddelandena kvar i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7723d-145">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)

