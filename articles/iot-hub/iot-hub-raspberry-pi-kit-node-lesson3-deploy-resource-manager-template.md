---
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 3: malldistribution | Microsoft Docs'
description: "hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b6c0a9530cb80e3f78c0e96037f6f3942b602aea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="6033b-104">Skapa en Azure funktionsapp och Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="6033b-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="6033b-105">[Azure Functions](../azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6033b-105">[Azure Functions](../azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="6033b-106">Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="6033b-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6033b-107">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="6033b-107">What you will do</span></span>
<span data-ttu-id="6033b-108">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6033b-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="6033b-109">hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="6033b-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="6033b-110">Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6033b-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6033b-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="6033b-111">What you will learn</span></span>
<span data-ttu-id="6033b-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="6033b-112">In this article, you will learn:</span></span>

* <span data-ttu-id="6033b-113">Hur toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="6033b-113">How toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="6033b-114">Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="6033b-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6033b-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="6033b-115">What you need</span></span>
<span data-ttu-id="6033b-116">Du måste ha slutfört:</span><span class="sxs-lookup"><span data-stu-id="6033b-116">You must have successfully completed:</span></span>
* [<span data-ttu-id="6033b-117">Kom igång med hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="6033b-117">Get started with Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)
* [<span data-ttu-id="6033b-118">Skapa din Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6033b-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-hello-sample-app"></a><span data-ttu-id="6033b-119">Öppna hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="6033b-119">Open hello sample app</span></span>
<span data-ttu-id="6033b-120">Öppna hello exempelprojektet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="6033b-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* <span data-ttu-id="6033b-122">Hej `app.js` filen i hello `app` undermapp är hello viktiga källfilen.</span><span class="sxs-lookup"><span data-stu-id="6033b-122">hello `app.js` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="6033b-123">Den här källfilen innehåller hello kod toosend ett meddelande 20 gånger tooyour IoT-hubb och blinka hello Indikator för varje meddelande som skickas.</span><span class="sxs-lookup"><span data-stu-id="6033b-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="6033b-124">Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6033b-124">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="6033b-125">Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="6033b-125">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="6033b-126">Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="6033b-126">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="6033b-127">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="6033b-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="6033b-128">Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="6033b-128">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Azure Resource Manager-mallparametrar](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* <span data-ttu-id="6033b-130">Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerad hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="6033b-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="6033b-131">Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="6033b-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="6033b-132">hello prefix garanterar hello resursnamnet är globalt unik tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="6033b-132">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="6033b-133">Använd inte en dash eller en siffra inledande i hello prefix.</span><span class="sxs-lookup"><span data-stu-id="6033b-133">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="6033b-134">När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6033b-134">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="6033b-135">Det tar cirka fem minuter toocreate dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="6033b-135">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="6033b-136">Medan hello resursen skapas, kan du gå vidare toohello nästa artikel.</span><span class="sxs-lookup"><span data-stu-id="6033b-136">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="6033b-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6033b-137">Summary</span></span>
<span data-ttu-id="6033b-138">Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6033b-138">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="6033b-139">Du kan nu distribuera och köra exemplet toosend enhet till moln hälsningsmeddelande på Pi.</span><span class="sxs-lookup"><span data-stu-id="6033b-139">You can now deploy and run hello sample toosend device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6033b-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6033b-140">Next steps</span></span>
[<span data-ttu-id="6033b-141">Kör ett exempel programmet toosend meddelanden från enhet till moln på hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="6033b-141">Run a sample application toosend device-to-cloud messages on Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

