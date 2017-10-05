---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 3: skicka meddelanden | Microsoft Docs'
description: "Distribuera och köra ett exempelprogram till Intel modern som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data till molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 1b3b1074-f4d4-42ac-b32c-55f18b304b44
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d4b520b9a1852a285b1e10b5b35447a54313af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="07c06-104">Kör ett exempelprogram för att skicka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="07c06-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="07c06-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="07c06-105">What you will do</span></span>
<span data-ttu-id="07c06-106">Den här artikeln visar hur du distribuera och köra ett exempelprogram på Intel modern som skickar meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="07c06-106">This article will show you how to deploy and run a sample application on Intel Edison that sends messages to your IoT hub.</span></span> <span data-ttu-id="07c06-107">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="07c06-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="07c06-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="07c06-108">What you will learn</span></span>
<span data-ttu-id="07c06-109">Du lära dig hur du använder verktyget gulp att distribuera och köra exempelprogrammet C på modern.</span><span class="sxs-lookup"><span data-stu-id="07c06-109">You will learn how to use the gulp tool to deploy and run the sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="07c06-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="07c06-110">What you need</span></span>
* <span data-ttu-id="07c06-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="07c06-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="07c06-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="07c06-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="07c06-113">Anslutningssträngen enheten används för att ansluta modern till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="07c06-113">The device connection string is used to connect Edison to your IoT hub.</span></span> <span data-ttu-id="07c06-114">IoT-hubb anslutningssträngen används för att ansluta din IoT-hubb för enhetens identitet som representerar modern i IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="07c06-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents Edison in the IoT hub.</span></span>

* <span data-ttu-id="07c06-115">Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="07c06-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="07c06-116">Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="07c06-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="07c06-117">Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="07c06-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="07c06-118">`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat modern.</span><span class="sxs-lookup"><span data-stu-id="07c06-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="07c06-119">Hämta anslutningssträngen för enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="07c06-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="07c06-120">Använd `myinteledison` som värde för `{device id}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="07c06-120">Use `myinteledison` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="07c06-121">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="07c06-121">Configure the device connection</span></span>
1. <span data-ttu-id="07c06-122">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="07c06-122">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

2. <span data-ttu-id="07c06-123">Öppna konfigurationsfilen enheten `config-edison.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="07c06-123">Open the device configuration file `config-edison.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. <span data-ttu-id="07c06-125">Se följande ersättningar i den `config-edison.json` filen:</span><span class="sxs-lookup"><span data-stu-id="07c06-125">Make the following replacements in the `config-edison.json` file:</span></span>

   * <span data-ttu-id="07c06-126">Ersätt **[enhet värdnamn eller IP-adress]** med enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="07c06-126">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="07c06-127">Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="07c06-127">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="07c06-128">Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="07c06-128">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="07c06-129">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="07c06-129">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="07c06-130">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="07c06-130">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="07c06-131">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="07c06-131">Deploy and run the sample application</span></span>
<span data-ttu-id="07c06-132">Distribuera och köra exempelprogrammet på modern genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="07c06-132">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="07c06-133">Kontrollera att det fungerar exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="07c06-133">Verify that the sample application works</span></span>
<span data-ttu-id="07c06-134">Du bör se Indikator som är ansluten till modern blinkande varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="07c06-134">You should see the LED that is connected to Edison blinking every two seconds.</span></span> <span data-ttu-id="07c06-135">Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="07c06-135">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="07c06-136">Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="07c06-136">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="07c06-137">Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="07c06-137">The sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="07c06-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="07c06-139">Summary</span></span>
<span data-ttu-id="07c06-140">Du har distribuerat och kör ny blinka exempelprogrammet på modern att skicka meddelanden från enhet till moln till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="07c06-140">You've deployed and run the new blink sample application on Edison to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="07c06-141">Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="07c06-141">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07c06-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07c06-142">Next steps</span></span>
<span data-ttu-id="07c06-143">[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="07c06-143">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md