---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 3: malldistribution | Microsoft Docs'
description: "Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b8bab31489f2e912b51212cb58cb598a9db9b59d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="72ed1-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="72ed1-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="72ed1-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i molnet.</span><span class="sxs-lookup"><span data-stu-id="72ed1-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="72ed1-106">Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="72ed1-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="72ed1-107">Vad ska du göra</span><span class="sxs-lookup"><span data-stu-id="72ed1-107">What will you do</span></span>
<span data-ttu-id="72ed1-108">Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="72ed1-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="72ed1-109">Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="72ed1-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span> <span data-ttu-id="72ed1-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="72ed1-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="72ed1-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="72ed1-111">What will you learn</span></span>
<span data-ttu-id="72ed1-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="72ed1-112">In this article, you will learn:</span></span>
* <span data-ttu-id="72ed1-113">Hur du använder [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) att distribuera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="72ed1-113">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="72ed1-114">Hur du använder en funktionsapp i Azure-att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="72ed1-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="72ed1-115">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="72ed1-115">What do you need</span></span>
* <span data-ttu-id="72ed1-116">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="72ed1-116">You must have successfully completed:</span></span>
- [<span data-ttu-id="72ed1-117">Kom igång med din hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="72ed1-117">Get started with your Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)
- [<span data-ttu-id="72ed1-118">Skapa din Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="72ed1-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)

## <a name="open-the-sample-app"></a><span data-ttu-id="72ed1-119">Öppna sample-appen</span><span class="sxs-lookup"><span data-stu-id="72ed1-119">Open the sample app</span></span>
<span data-ttu-id="72ed1-120">Öppna exempelprojektet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="72ed1-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure_c.png)

* <span data-ttu-id="72ed1-122">Den `main.c` filen i den `app` undermapp är viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="72ed1-122">The `main.c` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="72ed1-123">Den här källfilen innehåller koden för att skicka ett meddelande till 20 gånger till din IoT-hubb och blinka Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="72ed1-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="72ed1-124">Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="72ed1-124">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="72ed1-125">Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="72ed1-125">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="72ed1-126">Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="72ed1-126">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="72ed1-127">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="72ed1-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="72ed1-128">Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="72ed1-128">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para_c.png)

* <span data-ttu-id="72ed1-130">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerad hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="72ed1-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="72ed1-131">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="72ed1-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="72ed1-132">Prefixet gör att resursnamnet globalt unikt för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="72ed1-132">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="72ed1-133">Använd inte en dash eller en siffra inledande i prefixet.</span><span class="sxs-lookup"><span data-stu-id="72ed1-133">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="72ed1-134">När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="72ed1-134">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="72ed1-135">Det tar cirka fem minuter för att skapa dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="72ed1-135">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="72ed1-136">När resursen skapas pågår, kan du gå vidare till nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="72ed1-136">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="72ed1-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="72ed1-137">Summary</span></span>
<span data-ttu-id="72ed1-138">Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="72ed1-138">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="72ed1-139">Du kan nu distribuera och köra exemplet för att skicka meddelanden från enhet till moln på Pi.</span><span class="sxs-lookup"><span data-stu-id="72ed1-139">You can now deploy and run the sample to send device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72ed1-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72ed1-140">Next steps</span></span>
[<span data-ttu-id="72ed1-141">Kör ett exempelprogram för att skicka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="72ed1-141">Run a sample application to send device-to-cloud messages</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md)

