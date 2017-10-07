---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: läsa meddelanden | Microsoft Docs"
description: "Köra en exempelkod på värden för datorn tooread hälsningsmeddelande från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data i hello molnet, insamling av molnet, iot-Molntjänsten, iot-data"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="ce7b2-104">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ce7b2-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="ce7b2-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="ce7b2-105">What you will do</span></span>

- <span data-ttu-id="ce7b2-106">Köra exempelkod på din värd datorn tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="ce7b2-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="ce7b2-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ce7b2-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="ce7b2-108">What you will learn</span></span>

<span data-ttu-id="ce7b2-109">Hur gulp toouse hello verktyget tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ce7b2-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="ce7b2-110">What you need</span></span>

- <span data-ttu-id="ce7b2-111">hello simulerade enheten provet i [konfigurera och köra en simulerad enhet moln överför exempelprogrammet](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="ce7b2-111">hello simulated device sample in [Configure and run a simulated device cloud upload sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="ce7b2-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="ce7b2-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="ce7b2-113">anslutningssträngen för hello enheten används av din simulerade enhet tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-113">hello device connection string is used by your simulated device tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="ce7b2-114">hello anslutningssträngen för IoT-hubben är används tooconnect toohello identitetsregistret i din IoT-hubb toomanage hello enheter som beviljats tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="ce7b2-115">Lista alla IoT hubs i resursgruppen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="ce7b2-116">Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar den.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change it.</span></span>
- <span data-ttu-id="ce7b2-117">Hämta anslutningssträngen för hello IoT hub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="ce7b2-118">`{my hub name}`är hello-namn som du angav i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="ce7b2-119">Konfigurera hello enhetsanslutning hello-kodexempel</span><span class="sxs-lookup"><span data-stu-id="ce7b2-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="ce7b2-120">Uppdatera konfigurationer för IoT-hubb och enheten anslutning i `config-azure.json` genom att utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-120">Update IoT hub and device connection configurations in `config-azure.json` by performing hello following steps:</span></span>

1. <span data-ttu-id="ce7b2-121">Öppna `config-azure.json` i Visual Studio-koden genom att köra följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-121">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="ce7b2-122">Se följande ersättningar i hello hello `config-azure.json` fil:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-122">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![Skärmbild av azure config](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="ce7b2-124">Ersätt `[IoT hub connection string]` med hello anslutningssträngen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-124">Replace `[IoT hub connection string]` with hello IoT hub connection string.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="ce7b2-125">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ce7b2-125">Read messages from your IoT hub</span></span>

<span data-ttu-id="ce7b2-126">Kör exempelprogrammet i hello simulerade enheten och läsa IoT-hubb meddelanden hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ce7b2-126">Run hello simulated device sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="ce7b2-127">hello körs hello-program som skickar meddelanden tooyour IoT-hubb varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-127">hello command runs hello application that sends messages tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="ce7b2-128">Det skapas också en underordnad process tooreceive hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-128">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="ce7b2-129">hello-meddelanden som skickas och tas emot är alla visas direkt på hello samma konsolfönstret i hello värddatorn.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-129">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="ce7b2-130">hello programmet avslutas i 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-130">hello application will exit in 40 seconds.</span></span>

![Simulerade exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a><span data-ttu-id="ce7b2-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ce7b2-132">Summary</span></span>

<span data-ttu-id="ce7b2-133">Du har har kört hello exempel programmet toosend data tooyour IoT-hubb med simulerade enhet.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-133">You've successfully run hello sample application toosend data tooyour IoT hub with simulated device.</span></span> <span data-ttu-id="ce7b2-134">Du har också läsa hälsningsmeddelande som har skickats tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ce7b2-134">You've also read hello messages that have been sent tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce7b2-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce7b2-135">Next steps</span></span>
[<span data-ttu-id="ce7b2-136">Skapa en Azure-funktionsapp och ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="ce7b2-136">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


