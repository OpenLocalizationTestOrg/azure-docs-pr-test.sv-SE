---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 3: läsa meddelanden | Microsoft Docs"
description: "Köra en exempelkod på värden för datorn tooread hälsningsmeddelande från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data i hello molnet, insamling av molnet, iot-Molntjänsten, iot-data"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d3ffbe2e83f9d61c0088b8876a7f0eea62c1fbe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="3db7a-104">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3db7a-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="3db7a-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="3db7a-105">What you will do</span></span>

- <span data-ttu-id="3db7a-106">Köra exempelkod på din värd datorn tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3db7a-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="3db7a-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="3db7a-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="3db7a-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="3db7a-108">What you will learn</span></span>

<span data-ttu-id="3db7a-109">Hur gulp toouse hello verktyget tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3db7a-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3db7a-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="3db7a-110">What you need</span></span>

- <span data-ttu-id="3db7a-111">hello TIVERA exempelprogram som du har körts i lektionen 3.</span><span class="sxs-lookup"><span data-stu-id="3db7a-111">hello BLE sample application that you ran successfully in Lesson 3.</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="3db7a-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="3db7a-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="3db7a-113">anslutningssträngen för hello enheten används av din enhet (TI SensorTag eller simulerade enheten) tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3db7a-113">hello device connection string is used by your device (TI SensorTag or simulated device) tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="3db7a-114">hello anslutningssträngen för IoT-hubben är används tooconnect toohello identitetsregistret i din IoT-hubb toomanage hello enheter som beviljats tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3db7a-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="3db7a-115">Lista alla IoT hubs i resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3db7a-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="3db7a-116">Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="3db7a-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
- <span data-ttu-id="3db7a-117">Hämta anslutningssträngen för hello IoT hub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3db7a-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="3db7a-118">`{my hub name}`är hello-namn som du angav i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="3db7a-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="3db7a-119">Konfigurera hello enhetsanslutning hello-kodexempel</span><span class="sxs-lookup"><span data-stu-id="3db7a-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="3db7a-120">Konfigurationsfilen för Update hello enheten `config-azure.json` så att du kan läsa meddelanden från din IoT-hubb på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="3db7a-120">Update hello device configuration file `config-azure.json` so that you can read messages from your IoT hub on your host computer.</span></span> <span data-ttu-id="3db7a-121">toodo, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="3db7a-121">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="3db7a-122">Öppna `config-azure.json` i Visual Studio-koden genom att köra följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="3db7a-122">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="3db7a-123">Se följande ersättningar i hello hello `config-azure.json` fil:</span><span class="sxs-lookup"><span data-stu-id="3db7a-123">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![Skärmbild av azure config](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="3db7a-125">Ersätt `[IoT hub connection string]` med hello anslutningssträngen för IoT-hubb som du fick.</span><span class="sxs-lookup"><span data-stu-id="3db7a-125">Replace `[IoT hub connection string]` with hello IoT hub connection string that you obtained.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="3db7a-126">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3db7a-126">Read messages from your IoT hub</span></span>

<span data-ttu-id="3db7a-127">Om du har en TI SensorTag, kontrollera att du redan har påslagen din SensorTag.</span><span class="sxs-lookup"><span data-stu-id="3db7a-127">If you have a TI SensorTag, make sure you have already powered on your SensorTag.</span></span> <span data-ttu-id="3db7a-128">Kör exempelprogrammet i hello gateway och läsa IoT-hubb meddelanden hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3db7a-128">Run hello gateway sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="3db7a-129">hello körs hello TIVERA exempelprogram som läser och paket temperatur data från din SensorTag eller simulerade enhet och skickar hello-meddelande tooyour IoT-hubb varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="3db7a-129">hello command runs hello BLE sample application that reads and packages temperature data from your SensorTag or simulated device and sends hello message tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="3db7a-130">Det skapas också en underordnad process tooreceive hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="3db7a-130">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="3db7a-131">hello-meddelanden som skickas och tas emot är alla visas direkt på hello samma konsolfönstret i hello värddatorn.</span><span class="sxs-lookup"><span data-stu-id="3db7a-131">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="3db7a-132">hello exempel programinstansen avslutas automatiskt i 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="3db7a-132">hello sample application instance will terminate automatically in 40 seconds.</span></span>

![TIVERA exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a><span data-ttu-id="3db7a-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3db7a-134">Summary</span></span>

<span data-ttu-id="3db7a-135">Du har kört en exempel kod tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3db7a-135">You've run a sample code tooread messages from your IoT hub.</span></span> <span data-ttu-id="3db7a-136">Du är klar tooread hälsningsmeddelande som lagras i Azure-tabellagring.</span><span class="sxs-lookup"><span data-stu-id="3db7a-136">You're ready tooread hello messages that are stored in your Azure table storage.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3db7a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3db7a-137">Next steps</span></span>
[<span data-ttu-id="3db7a-138">Skapa en Azure-funktionsapp och ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="3db7a-138">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


