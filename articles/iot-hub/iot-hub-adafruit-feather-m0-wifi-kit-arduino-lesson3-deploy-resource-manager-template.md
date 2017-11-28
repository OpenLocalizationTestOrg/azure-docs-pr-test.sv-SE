---
title: 'Connect Arduino (C) till Azure IoT - lektionen 3: malldistribution | Microsoft Docs'
description: "Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: be6105927645ae2ec56f6885c61dbcb6faf5b11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="e49f4-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="e49f4-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="e49f4-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i molnet.</span><span class="sxs-lookup"><span data-stu-id="e49f4-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="e49f4-106">Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="e49f4-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="e49f4-107">Vad ska du göra</span><span class="sxs-lookup"><span data-stu-id="e49f4-107">What will you do</span></span>
<span data-ttu-id="e49f4-108">Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e49f4-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="e49f4-109">Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e49f4-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="e49f4-110">Om du har några problem kan hitta lösningar på den [felsökning för Adafruit ludd M0 WiFi Arduino-kort](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e49f4-110">If you have any problems, look for solutions on the [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="e49f4-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="e49f4-111">What will you learn</span></span>
<span data-ttu-id="e49f4-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="e49f4-112">In this article, you will learn:</span></span>
* <span data-ttu-id="e49f4-113">Hur du använder [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) att distribuera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e49f4-113">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="e49f4-114">Hur du använder en funktionsapp i Azure-att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e49f4-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="e49f4-115">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="e49f4-115">What do you need</span></span>
<span data-ttu-id="e49f4-116">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="e49f4-116">You must have successfully completed:</span></span>
- <span data-ttu-id="e49f4-117">[Kom igång med Arduino-kort][get-started]</span><span class="sxs-lookup"><span data-stu-id="e49f4-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="e49f4-118">[Skapa din Azure IoT-hubb][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="e49f4-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-the-sample-app"></a><span data-ttu-id="e49f4-119">Öppna sample-appen</span><span class="sxs-lookup"><span data-stu-id="e49f4-119">Open the sample app</span></span>
<span data-ttu-id="e49f4-120">Öppna exempelprojektet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e49f4-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* <span data-ttu-id="e49f4-122">Den `app.ino` filen i den `app` undermapp är viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="e49f4-122">The `app.ino` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="e49f4-123">Den här källfilen innehåller koden för att skicka ett meddelande till 20 gånger till din IoT-hubb och blinka Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="e49f4-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="e49f4-124">Den `config.json` innehåller konfigurationsinställningar som krävs.</span><span class="sxs-lookup"><span data-stu-id="e49f4-124">The `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="e49f4-125">Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e49f4-125">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="e49f4-126">Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="e49f4-126">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="e49f4-127">Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="e49f4-127">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="e49f4-128">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="e49f4-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="e49f4-129">Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="e49f4-129">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar][arm-template-params]

* <span data-ttu-id="e49f4-131">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerat din Arduino board] [ created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="e49f4-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="e49f4-132">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="e49f4-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="e49f4-133">Prefixet gör att resursnamnet globalt unikt för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="e49f4-133">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="e49f4-134">Använd inte en dash eller en siffra inledande i prefixet.</span><span class="sxs-lookup"><span data-stu-id="e49f4-134">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="e49f4-135">När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e49f4-135">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="e49f4-136">Det tar cirka fem minuter för att skapa dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="e49f4-136">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="e49f4-137">När resursen skapas pågår, kan du gå vidare till nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="e49f4-137">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="e49f4-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e49f4-138">Summary</span></span>
<span data-ttu-id="e49f4-139">Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e49f4-139">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="e49f4-140">Du kan nu distribuera och köra exemplet för att skicka meddelanden från enhet till moln på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="e49f4-140">You can now deploy and run the sample to send device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e49f4-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e49f4-141">Next steps</span></span>
<span data-ttu-id="e49f4-142">[Kör ett exempelprogram för att skicka meddelanden från enhet till moln på Arduino-kort][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="e49f4-142">[Run a sample application to send device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md