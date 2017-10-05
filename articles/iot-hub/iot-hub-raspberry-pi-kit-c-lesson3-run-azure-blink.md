---
title: "Connect Raspberry PI (C) till Azure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra ett exempelprogram hallon Pi 3 som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
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
ms.openlocfilehash: e583ba455a94f9afcc7b31e49425b518d7968919
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="c9e52-104">Kör ett exempelprogram för att skicka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="c9e52-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="c9e52-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="c9e52-105">What you will do</span></span>
<span data-ttu-id="c9e52-106">Den här artikeln visar hur du distribuera och köra ett exempelprogram på hallon Pi 3 som skickar meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c9e52-106">This article will show you how to deploy and run a sample application on Raspberry Pi 3 that sends messages to your IoT hub.</span></span> <span data-ttu-id="c9e52-107">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c9e52-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c9e52-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="c9e52-108">What you will learn</span></span>
<span data-ttu-id="c9e52-109">Du lära dig hur du använder verktyget gulp att distribuera och köra Node.js exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="c9e52-109">You will learn how to use the gulp tool to deploy and run the sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c9e52-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c9e52-110">What you need</span></span>
* <span data-ttu-id="c9e52-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="c9e52-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="c9e52-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="c9e52-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="c9e52-113">Anslutningssträngen enheten används av din Pi för att ansluta till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c9e52-113">The device connection string is used by your Pi to connect to your IoT hub.</span></span> <span data-ttu-id="c9e52-114">IoT-hubb anslutningssträngen används för att ansluta till identitetsregistret i IoT-hubb för att hantera enheter som tillåts att ansluta till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c9e52-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span> 

* <span data-ttu-id="c9e52-115">Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="c9e52-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="c9e52-116">Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="c9e52-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="c9e52-117">Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="c9e52-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="c9e52-118">`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat Pi.</span><span class="sxs-lookup"><span data-stu-id="c9e52-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="c9e52-119">Hämta anslutningssträngen för enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c9e52-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="c9e52-120">Använd `myraspberrypi` som värde för `{device id}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="c9e52-120">Use `myraspberrypi` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="c9e52-121">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="c9e52-121">Configure the device connection</span></span>
1. <span data-ttu-id="c9e52-122">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c9e52-122">Initialize the configuration file by running the following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> <span data-ttu-id="c9e52-123">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="c9e52-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="c9e52-124">Öppna konfigurationsfilen enheten `config-raspberrypi.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c9e52-124">Open the device configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="c9e52-126">Se följande ersättningar i den `config-raspberrypi.json` filen:</span><span class="sxs-lookup"><span data-stu-id="c9e52-126">Make the following replacements in the `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="c9e52-127">Ersätt **[enhet värdnamn eller IP-adress]** med IP-adressen eller värdnamnet enhetsnamnet som du fick från `device-discovery-cli` eller med värdet överförs när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="c9e52-127">Replace **[device hostname or IP address]** with the device IP address or host name you got from `device-discovery-cli` or with the value inherited when you configured your device.</span></span>
   * <span data-ttu-id="c9e52-128">Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="c9e52-128">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="c9e52-129">Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="c9e52-129">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

> [!NOTE]
> <span data-ttu-id="c9e52-130">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c9e52-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="c9e52-131">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="c9e52-131">Keep it as is.</span></span>

<span data-ttu-id="c9e52-132">Uppdatera den `config-raspberrypi.json` filen så att du kan distribuera exempelprogrammet från datorn.</span><span class="sxs-lookup"><span data-stu-id="c9e52-132">Update the `config-raspberrypi.json` file so that you can deploy the sample application from your computer.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="c9e52-133">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c9e52-133">Deploy and run the sample application</span></span>
<span data-ttu-id="c9e52-134">Distribuera och köra exempelprogrammet på Pi genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c9e52-134">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="c9e52-135">Kontrollera att det fungerar exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c9e52-135">Verify that the sample application works</span></span>
<span data-ttu-id="c9e52-136">Du bör se som är ansluten till Pi blinkande varannan sekund LED.</span><span class="sxs-lookup"><span data-stu-id="c9e52-136">You should see the LED that is connected to Pi blinking every two seconds.</span></span> <span data-ttu-id="c9e52-137">Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c9e52-137">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="c9e52-138">Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="c9e52-138">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="c9e52-139">Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="c9e52-139">The sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a><span data-ttu-id="c9e52-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c9e52-141">Summary</span></span>
<span data-ttu-id="c9e52-142">Du har distribuerat och kör ny blinka exempelprogrammet på Pi för att skicka meddelanden från enhet till moln till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c9e52-142">You've deployed and run the new blink sample application on Pi to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="c9e52-143">Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="c9e52-143">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9e52-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9e52-144">Next steps</span></span>
[<span data-ttu-id="c9e52-145">Läs meddelandena kvar i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c9e52-145">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)

