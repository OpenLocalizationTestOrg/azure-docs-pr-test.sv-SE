---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 3: skapa funktionsapp | Microsoft Docs'
description: "Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 37ee5962-95ce-40e8-8162-17e735eaec21
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6ada1cbbb560f1373346eca561dceb28d7ca7242
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="accd5-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="accd5-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="accd5-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i molnet.</span><span class="sxs-lookup"><span data-stu-id="accd5-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="accd5-106">Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="accd5-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="accd5-107">Vad ska du göra</span><span class="sxs-lookup"><span data-stu-id="accd5-107">What will you do</span></span>
<span data-ttu-id="accd5-108">Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="accd5-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="accd5-109">Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="accd5-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span> <span data-ttu-id="accd5-110">Storage-konto används för att läsa de beständiga kopiorna av meddelanden från Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="accd5-110">The storage account is used for reading the persisted copies of messages from Azure table.</span></span> <span data-ttu-id="accd5-111">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="accd5-111">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="accd5-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="accd5-112">What will you learn</span></span>
<span data-ttu-id="accd5-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="accd5-113">In this article, you will learn:</span></span>
* <span data-ttu-id="accd5-114">Hur du använder [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) att distribuera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="accd5-114">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="accd5-115">Hur du använder en funktionsapp i Azure-att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="accd5-115">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="accd5-116">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="accd5-116">What do you need</span></span>
<span data-ttu-id="accd5-117">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="accd5-117">You must have successfully completed:</span></span>
- <span data-ttu-id="accd5-118">[Kom igång med Intel-modern][get-started-with-your-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="accd5-118">[Get started with your Intel Edison][get-started-with-your-intel-edison]</span></span>
- <span data-ttu-id="accd5-119">[Skapa din Azure IoT-hubb][create-your-azure-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="accd5-119">[Create your Azure IoT hub][create-your-azure-iot-hub]</span></span>

## <a name="open-the-sample-app"></a><span data-ttu-id="accd5-120">Öppna sample-appen</span><span class="sxs-lookup"><span data-stu-id="accd5-120">Open the sample app</span></span>
<span data-ttu-id="accd5-121">Öppna exempelprojektet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="accd5-121">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* <span data-ttu-id="accd5-123">Filen i den `app` undermapp är viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="accd5-123">The file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="accd5-124">Den här källfilen innehåller koden för att skicka ett meddelande till 20 gånger till din IoT-hubb och blinka Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="accd5-124">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="accd5-125">Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="accd5-125">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="accd5-126">Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="accd5-126">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="accd5-127">Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="accd5-127">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="accd5-128">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="accd5-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="accd5-129">Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="accd5-129">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar][arm-template-parameters]

* <span data-ttu-id="accd5-131">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerad Intel modern][created-your-iot-hub-and-registered-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="accd5-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span></span>
* <span data-ttu-id="accd5-132">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="accd5-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="accd5-133">Prefixet gör att resursnamnet globalt unikt för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="accd5-133">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="accd5-134">Använd inte en dash eller en siffra inledande i prefixet.</span><span class="sxs-lookup"><span data-stu-id="accd5-134">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="accd5-135">När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="accd5-135">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="accd5-136">Det tar cirka fem minuter för att skapa dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="accd5-136">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="accd5-137">När resursen skapas pågår, kan du gå vidare till nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="accd5-137">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="accd5-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="accd5-138">Summary</span></span>
<span data-ttu-id="accd5-139">Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="accd5-139">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="accd5-140">Du kan nu distribuera och köra exemplet för att skicka meddelanden från enhet till moln för modern.</span><span class="sxs-lookup"><span data-stu-id="accd5-140">You can now deploy and run the sample to send device-to-cloud messages on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="accd5-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="accd5-141">Next steps</span></span>
<span data-ttu-id="accd5-142">[Kör ett exempelprogram för att skicka meddelanden från enhet till moln på Intel modern][send-device-to-cloud-messages].</span><span class="sxs-lookup"><span data-stu-id="accd5-142">[Run a sample application to send device-to-cloud messages on Intel Edison][send-device-to-cloud-messages].</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-node-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure.png
[arm-template-parameters]: media/iot-hub-intel-edison-lessons/lesson3/arm_para.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md