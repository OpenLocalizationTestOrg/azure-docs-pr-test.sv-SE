---
title: Skapa en IoT-hubb med Azure CLI (azure.js) | Microsoft Docs
description: "Så här skapar du en Azure IoT-hubb med plattformsoberoende Azure CLI (azure.js)."
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: boltean
ms.openlocfilehash: 5e37c6c5e8625ce446ab203f19f9a8b2f1cd5a46
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a><span data-ttu-id="cedcb-103">Skapa en IoT-hubb med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cedcb-103">Create an IoT hub using the Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="cedcb-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="cedcb-104">Introduction</span></span>

<span data-ttu-id="cedcb-105">Du kan använda Azure CLI (azure.js) för att skapa och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="cedcb-105">You can use Azure CLI (azure.js) to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="cedcb-106">Den här artikeln visar hur du använder Azure CLI (azure.js) för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cedcb-106">This article shows you how to use the Azure CLI (azure.js) to create an IoT hub.</span></span>

<span data-ttu-id="cedcb-107">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="cedcb-107">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="cedcb-108">Azure CLI (azure.js) – CLI för klassisk och resurs management distributionsmodellerna som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="cedcb-108">Azure CLI (azure.js) – the CLI for the classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="cedcb-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) – nästa generations CLI för resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="cedcb-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - the next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="cedcb-110">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cedcb-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="cedcb-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="cedcb-111">An active Azure account.</span></span> <span data-ttu-id="cedcb-112">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="cedcb-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="cedcb-113">[Azure CLI 0.10.4] [ lnk-CLI-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="cedcb-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="cedcb-114">Om du redan har installerat Azure CLI, kan du verifiera den aktuella versionen i Kommandotolken med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cedcb-114">If you already have the Azure CLI installed, you can validate the current version at the command prompt with the following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="cedcb-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cedcb-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cedcb-116">Azure CLI måste vara i läget Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="cedcb-116">The Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="cedcb-117">Konfigurera Azure-kontot och Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="cedcb-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="cedcb-118">I Kommandotolken inloggningen genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cedcb-118">At the command prompt, login by typing the following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="cedcb-119">Använda föreslagna webbläsare och kod för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="cedcb-119">Use the suggested web browser and code to authenticate.</span></span>
1. <span data-ttu-id="cedcb-120">Om du har flera Azure-prenumerationer, ger ansluta till Azure åtkomst till alla de Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cedcb-120">If you have multiple Azure subscriptions, connecting to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="cedcb-121">Du kan visa Azure-prenumerationer och identifiera vilket som är standard, med hjälp av kommandot:</span><span class="sxs-lookup"><span data-stu-id="cedcb-121">You can view the Azure subscriptions, and identify which one is the default, using the command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="cedcb-122">Att ställa in den prenumerationskontext som du vill köra resten av kommandon användas:</span><span class="sxs-lookup"><span data-stu-id="cedcb-122">To set the subscription context under which you want to run the rest of the commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="cedcb-123">Om du inte har en resursgrupp kan du skapa en med namnet **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="cedcb-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="cedcb-124">Artikeln [använda Azure CLI för att hantera Azure-resurser och resursgrupper] [ lnk-CLI-arm] innehåller mer information om hur du använder Azure CLI för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="cedcb-124">The article [Use the Azure CLI to manage Azure resources and resource groups][lnk-CLI-arm] provides more information about how to use the Azure CLI to manage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="cedcb-125">Skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="cedcb-125">Create an IoT Hub</span></span>

<span data-ttu-id="cedcb-126">Obligatoriska parametrar:</span><span class="sxs-lookup"><span data-stu-id="cedcb-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="cedcb-127">**resursgruppens namn**.</span><span class="sxs-lookup"><span data-stu-id="cedcb-127">**resource-group**.</span></span> <span data-ttu-id="cedcb-128">Resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="cedcb-128">The resource group name.</span></span> <span data-ttu-id="cedcb-129">Formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, 1-64-längd.</span><span class="sxs-lookup"><span data-stu-id="cedcb-129">The format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="cedcb-130">**name**.</span><span class="sxs-lookup"><span data-stu-id="cedcb-130">**name**.</span></span> <span data-ttu-id="cedcb-131">Namnet på IoT-hubben ska skapas.</span><span class="sxs-lookup"><span data-stu-id="cedcb-131">The name of the IoT hub to be created.</span></span> <span data-ttu-id="cedcb-132">Formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, längd på 3-50.</span><span class="sxs-lookup"><span data-stu-id="cedcb-132">The format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="cedcb-133">**plats**.</span><span class="sxs-lookup"><span data-stu-id="cedcb-133">**location**.</span></span> <span data-ttu-id="cedcb-134">Platsen (azure region/datacenter) att etablera IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="cedcb-134">The location (azure region/datacenter) to provision the IoT hub.</span></span>
* <span data-ttu-id="cedcb-135">**SKU-name**.</span><span class="sxs-lookup"><span data-stu-id="cedcb-135">**sku-name**.</span></span> <span data-ttu-id="cedcb-136">Namnet på sku, en av: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="cedcb-136">The name of the sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="cedcb-137">Den senaste fullständiga listan finns prissättningssidan för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cedcb-137">For the latest full list, refer to the pricing page for IoT Hub.</span></span>
* <span data-ttu-id="cedcb-138">**enheter**.</span><span class="sxs-lookup"><span data-stu-id="cedcb-138">**units**.</span></span> <span data-ttu-id="cedcb-139">Antal allokerade enheter.</span><span class="sxs-lookup"><span data-stu-id="cedcb-139">The number of provisioned units.</span></span> <span data-ttu-id="cedcb-140">-Intervall: F1 [1-1]: S1, S2 [1 200]: S3 [1-10].</span><span class="sxs-lookup"><span data-stu-id="cedcb-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="cedcb-141">IoT-hubbenheter baseras på din totala meddelandemängd och antalet enheter som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="cedcb-141">IoT Hub units are based on your total message count and the number of devices you want to connect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="cedcb-142">Du kan använda kommandot help i Kommandotolken om du vill se alla parametrar som är tillgängligt för att skapa:</span><span class="sxs-lookup"><span data-stu-id="cedcb-142">To see all the parameters available for creation, you can use the help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="cedcb-143">Enkelt exempel: Om du vill skapa en IoT-hubb med namnet **exampleIoTHubName** i resursgruppen **exampleResourceGroup**, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cedcb-143">Quick example: To create an IoT Hub called **exampleIoTHubName** in the resource group **exampleResourceGroup**, run the following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="cedcb-144">Den här Azure CLI-kommando skapar en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="cedcb-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="cedcb-145">Du kan ta bort IoT-hubben **exampleIoTHubName** med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cedcb-145">You can delete the IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="cedcb-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cedcb-146">Next steps</span></span>

<span data-ttu-id="cedcb-147">Mer information om hur du utvecklar för IoT-hubb finns i följande artikel:</span><span class="sxs-lookup"><span data-stu-id="cedcb-147">To learn more about developing for IoT Hub, see the following article:</span></span>

* <span data-ttu-id="cedcb-148">[IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="cedcb-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="cedcb-149">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="cedcb-149">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="cedcb-150">[Använda Azure portal för att hantera IoT-hubb][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="cedcb-150">[Using the Azure portal to manage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
