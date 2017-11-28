---
title: 'SensorTag enhet & Azure IoT-Gateway - Lektion 4: skapa funktionsapp | Microsoft Docs'
description: "Spara meddelanden från Intel NUC tooyour IoT-hubb, skrivs tooAzure Table storage och läses sedan från hello molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: efee3bdc15ced104651f4a500311a5fe614267c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="28885-104">Skapa en Azure-funktionsapp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="28885-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="28885-105">Azure Functions är en lösning för att enkelt köra _funktioner_ (små delar av kod) i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="28885-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="28885-106">Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="28885-106">An Azure function app hosts hello execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="28885-107">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="28885-107">What you will do</span></span>

- <span data-ttu-id="28885-108">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="28885-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="28885-109">hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="28885-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="28885-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="28885-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="28885-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="28885-111">What you will learn</span></span>

<span data-ttu-id="28885-112">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="28885-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="28885-113">Hur toouse Azure Resource Manager toodeploy Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="28885-113">How toouse Azure Resource Manager toodeploy Azure resources.</span></span>
- <span data-ttu-id="28885-114">Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="28885-114">How toouse an Azure function app tooprocess IoT Hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="28885-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="28885-115">What you need</span></span>

<span data-ttu-id="28885-116">Du måste ha slutfört hello tidigare erfarenheter:</span><span class="sxs-lookup"><span data-stu-id="28885-116">You must have successfully completed hello previous lessons:</span></span>

- [<span data-ttu-id="28885-117">Lektionen 1: Konfigurera Intel-NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="28885-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [<span data-ttu-id="28885-118">Lektionen 2: Förbereda din värddatorn och Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="28885-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="28885-119">Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="28885-119">Lesson 3: Receive messages from SensorTag and read messages from IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="28885-120">Öppna en exempelapp</span><span class="sxs-lookup"><span data-stu-id="28885-120">Open a sample app</span></span>

<span data-ttu-id="28885-121">Gå tooyour `iot-hub-c-intel-nuc-gateway-getting-started` lagringsplatsen mappen, initiera hello-konfigurationsfiler och öppna sedan hello exempelprojektet i Visual Studio Code genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="28885-121">Go tooyour `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize hello configuration files, and then open hello sample project in Visual Studio Code by running hello following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Lagringsplatsen struktur](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="28885-123">Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="28885-123">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="28885-124">Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="28885-124">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
- <span data-ttu-id="28885-125">Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="28885-125">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="28885-126">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="28885-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="28885-127">Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="28885-127">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![arm-mall json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="28885-129">Ersätt `[your IoT Hub name]` med `{my hub name}` som du angav i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="28885-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="28885-130">När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="28885-130">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="28885-131">Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar hello värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="28885-131">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="28885-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="28885-132">Summary</span></span>

<span data-ttu-id="28885-133">Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="28885-133">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="28885-134">Du kan läsa meddelanden som skickas av din gateway tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="28885-134">You can now read messages that are sent by your gateway tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28885-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28885-135">Next steps</span></span>
<span data-ttu-id="28885-136">[Läs meddelandena kvar i Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="28885-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>
