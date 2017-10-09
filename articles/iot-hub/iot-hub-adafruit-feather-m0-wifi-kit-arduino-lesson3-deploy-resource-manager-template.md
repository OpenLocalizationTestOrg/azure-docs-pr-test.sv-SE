---
title: 'Connect Arduino (C) tooAzure IoT - lektionen 3: malldistribution | Microsoft Docs'
description: "hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
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
ms.openlocfilehash: 6a84a6d3c5263a85c8997cf69fe446d73ab7a5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="5c2de-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="5c2de-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="5c2de-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="5c2de-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="5c2de-106">Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="5c2de-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="5c2de-107">Vad ska du göra</span><span class="sxs-lookup"><span data-stu-id="5c2de-107">What will you do</span></span>
<span data-ttu-id="5c2de-108">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5c2de-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="5c2de-109">hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="5c2de-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="5c2de-110">Om du har några problem med söka efter lösningar på hello [felsökning för Adafruit ludd M0 WiFi Arduino-kort](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5c2de-110">If you have any problems, look for solutions on hello [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="5c2de-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="5c2de-111">What will you learn</span></span>
<span data-ttu-id="5c2de-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="5c2de-112">In this article, you will learn:</span></span>
* <span data-ttu-id="5c2de-113">Hur toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="5c2de-113">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="5c2de-114">Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="5c2de-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="5c2de-115">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="5c2de-115">What do you need</span></span>
<span data-ttu-id="5c2de-116">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="5c2de-116">You must have successfully completed:</span></span>
- <span data-ttu-id="5c2de-117">[Kom igång med Arduino-kort][get-started]</span><span class="sxs-lookup"><span data-stu-id="5c2de-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="5c2de-118">[Skapa din Azure IoT-hubb][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="5c2de-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-hello-sample-app"></a><span data-ttu-id="5c2de-119">Öppna hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="5c2de-119">Open hello sample app</span></span>
<span data-ttu-id="5c2de-120">Öppna hello exempelprojektet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="5c2de-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* <span data-ttu-id="5c2de-122">Hej `app.ino` filen i hello `app` undermapp är hello viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="5c2de-122">hello `app.ino` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="5c2de-123">Den här källfilen innehåller hello kod toosend ett meddelande 20 gånger tooyour IoT-hubb och blinka hello Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="5c2de-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="5c2de-124">Hej `config.json` innehåller konfigurationsinställningar som krävs.</span><span class="sxs-lookup"><span data-stu-id="5c2de-124">hello `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="5c2de-125">Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5c2de-125">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="5c2de-126">Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="5c2de-126">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="5c2de-127">Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="5c2de-127">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="5c2de-128">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="5c2de-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="5c2de-129">Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="5c2de-129">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar][arm-template-params]

* <span data-ttu-id="5c2de-131">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerat din Arduino board] [ created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="5c2de-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="5c2de-132">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="5c2de-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="5c2de-133">hello prefix garanterar hello resursnamnet är globalt unik tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="5c2de-133">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="5c2de-134">Använd inte en dash eller en siffra inledande i hello prefix.</span><span class="sxs-lookup"><span data-stu-id="5c2de-134">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="5c2de-135">När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="5c2de-135">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="5c2de-136">Det tar cirka fem minuter toocreate dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="5c2de-136">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="5c2de-137">Medan hello resursen skapas, kan du gå vidare toohello nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="5c2de-137">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="5c2de-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5c2de-138">Summary</span></span>
<span data-ttu-id="5c2de-139">Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5c2de-139">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="5c2de-140">Du kan nu distribuera och köra exemplet toosend enhet till moln hälsningsmeddelande på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="5c2de-140">You can now deploy and run hello sample toosend device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c2de-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c2de-141">Next steps</span></span>
<span data-ttu-id="5c2de-142">[Kör ett exempel programmet toosend meddelanden från enhet till moln på Arduino-kort][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="5c2de-142">[Run a sample application toosend device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md