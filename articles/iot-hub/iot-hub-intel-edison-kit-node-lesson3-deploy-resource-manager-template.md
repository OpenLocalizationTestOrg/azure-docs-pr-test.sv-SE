---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 3: skapa funktionsapp | Microsoft Docs'
description: "hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
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
ms.openlocfilehash: 8ea0a4cdf978158d70e47eaed57e3de378b638d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="90489-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="90489-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="90489-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="90489-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="90489-106">Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="90489-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="90489-107">Vad ska du göra</span><span class="sxs-lookup"><span data-stu-id="90489-107">What will you do</span></span>
<span data-ttu-id="90489-108">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90489-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="90489-109">hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="90489-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="90489-110">hello storage-konto används för att läsa hello beständiga kopior av meddelanden från Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="90489-110">hello storage account is used for reading hello persisted copies of messages from Azure table.</span></span> <span data-ttu-id="90489-111">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="90489-111">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="90489-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="90489-112">What will you learn</span></span>
<span data-ttu-id="90489-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="90489-113">In this article, you will learn:</span></span>
* <span data-ttu-id="90489-114">Hur toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="90489-114">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="90489-115">Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="90489-115">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="90489-116">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="90489-116">What do you need</span></span>
<span data-ttu-id="90489-117">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="90489-117">You must have successfully completed:</span></span>
- <span data-ttu-id="90489-118">[Kom igång med Intel-modern][get-started-with-your-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="90489-118">[Get started with your Intel Edison][get-started-with-your-intel-edison]</span></span>
- <span data-ttu-id="90489-119">[Skapa din Azure IoT-hubb][create-your-azure-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="90489-119">[Create your Azure IoT hub][create-your-azure-iot-hub]</span></span>

## <a name="open-hello-sample-app"></a><span data-ttu-id="90489-120">Öppna hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="90489-120">Open hello sample app</span></span>
<span data-ttu-id="90489-121">Öppna hello exempelprojektet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="90489-121">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* <span data-ttu-id="90489-123">hello-filen i hello `app` undermapp är hello viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="90489-123">hello file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="90489-124">Den här källfilen innehåller hello kod toosend ett meddelande 20 gånger tooyour IoT-hubb och blinka hello Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="90489-124">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="90489-125">Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90489-125">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="90489-126">Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="90489-126">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="90489-127">Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="90489-127">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="90489-128">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="90489-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="90489-129">Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="90489-129">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar][arm-template-parameters]

* <span data-ttu-id="90489-131">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerad Intel modern][created-your-iot-hub-and-registered-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="90489-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Intel Edison][created-your-iot-hub-and-registered-intel-edison].</span></span>
* <span data-ttu-id="90489-132">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="90489-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="90489-133">hello prefix garanterar hello resursnamnet är globalt unik tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="90489-133">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="90489-134">Använd inte en dash eller en siffra inledande i hello prefix.</span><span class="sxs-lookup"><span data-stu-id="90489-134">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="90489-135">När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="90489-135">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="90489-136">Det tar cirka fem minuter toocreate dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="90489-136">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="90489-137">Medan hello resursen skapas, kan du gå vidare toohello nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="90489-137">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="90489-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="90489-138">Summary</span></span>
<span data-ttu-id="90489-139">Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="90489-139">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="90489-140">Du kan nu distribuera och köra exemplet toosend enhet till moln hälsningsmeddelande på modern.</span><span class="sxs-lookup"><span data-stu-id="90489-140">You can now deploy and run hello sample toosend device-to-cloud messages on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90489-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90489-141">Next steps</span></span>
<span data-ttu-id="90489-142">[Kör ett exempel programmet toosend meddelanden från enhet till moln på Intel modern][send-device-to-cloud-messages].</span><span class="sxs-lookup"><span data-stu-id="90489-142">[Run a sample application toosend device-to-cloud messages on Intel Edison][send-device-to-cloud-messages].</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-node-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure.png
[arm-template-parameters]: media/iot-hub-intel-edison-lessons/lesson3/arm_para.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md