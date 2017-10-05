---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 4: spara meddelanden | Microsoft Docs'
description: "Spara meddelanden från Intel NUC till din IoT-hubb, skriva dem till Azure Table storage och läses sedan från molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: ffed0c2e-b092-40e1-9113-8196ec057d67
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7fc47b07acede28ffe790debca7e38521726011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="32db0-104">Skapa en Azure-funktionsapp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="32db0-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="32db0-105">Azure Functions är en lösning för att enkelt köra _funktioner_ (små delar av kod) i molnet.</span><span class="sxs-lookup"><span data-stu-id="32db0-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in the cloud.</span></span> <span data-ttu-id="32db0-106">Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="32db0-106">An Azure function app hosts the execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="32db0-107">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="32db0-107">What you will do</span></span>

- <span data-ttu-id="32db0-108">Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32db0-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="32db0-109">Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="32db0-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="32db0-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="32db0-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="32db0-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="32db0-111">What you will learn</span></span>

<span data-ttu-id="32db0-112">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="32db0-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="32db0-113">Hur du använder Azure Resource Manager för att distribuera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="32db0-113">How to use Azure Resource Manager to deploy Azure resources.</span></span>
- <span data-ttu-id="32db0-114">Hur du använder en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="32db0-114">How to use an Azure function app to process IoT Hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="32db0-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="32db0-115">What you need</span></span>

<span data-ttu-id="32db0-116">Du måste ha slutfört tidigare erfarenheter:</span><span class="sxs-lookup"><span data-stu-id="32db0-116">You must have successfully completed the previous lessons:</span></span>

- [<span data-ttu-id="32db0-117">Lektionen 1: Konfigurera Intel-NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="32db0-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)
- [<span data-ttu-id="32db0-118">Lektionen 2: Förbereda din värddatorn och Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32db0-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="32db0-119">Lektionen 3: Ta emot meddelanden från den simulerade enheten och läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="32db0-119">Lesson 3: Receive messages from the simulated device and read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="32db0-120">Öppna en exempelapp</span><span class="sxs-lookup"><span data-stu-id="32db0-120">Open a sample app</span></span>

<span data-ttu-id="32db0-121">Gå till din `iot-hub-c-intel-nuc-gateway-getting-started` lagringsplatsen, initiera konfigurationsfiler och öppna exempelprojektet i Visual Studio Code genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32db0-121">Go to your `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize the configuration files, and then open the sample project in Visual Studio Code by running the following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Lagringsplatsen struktur](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="32db0-123">Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32db0-123">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="32db0-124">Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="32db0-124">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
- <span data-ttu-id="32db0-125">Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.</span><span class="sxs-lookup"><span data-stu-id="32db0-125">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="32db0-126">Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure</span><span class="sxs-lookup"><span data-stu-id="32db0-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="32db0-127">Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="32db0-127">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![arm-mall json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="32db0-129">Ersätt `[your IoT Hub name]` med `{my hub name}` som du angav i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="32db0-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="32db0-130">När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="32db0-130">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="32db0-131">Använd `iot-gateway` som värde för `{resource group name}` om du inte ändra värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="32db0-131">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="32db0-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="32db0-132">Summary</span></span>

<span data-ttu-id="32db0-133">Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="32db0-133">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="32db0-134">Du kan läsa meddelanden som skickas av din gateway till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="32db0-134">You can now read messages that are sent by your gateway to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32db0-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32db0-135">Next steps</span></span>
<span data-ttu-id="32db0-136">[Läs meddelandena kvar i Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="32db0-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>
