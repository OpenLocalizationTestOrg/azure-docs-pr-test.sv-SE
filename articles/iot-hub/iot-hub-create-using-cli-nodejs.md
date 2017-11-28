---
title: aaaCreate en IoT-hubb med Azure CLI (azure.js) | Microsoft Docs
description: Hur en Azure IoT-hubb med toocreate hello plattformsoberoende Azure CLI (azure.js).
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
ms.openlocfilehash: c2a7ea98500b0a0e55a39f4cdfea4605c92add94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli"></a><span data-ttu-id="6a8d0-103">Skapa en IoT-hubb med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6a8d0-103">Create an IoT hub using hello Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="6a8d0-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6a8d0-104">Introduction</span></span>

<span data-ttu-id="6a8d0-105">Du kan använda Azure CLI (azure.js) toocreate och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-105">You can use Azure CLI (azure.js) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="6a8d0-106">Den här artikeln visar hur hello toouse Azure CLI (azure.js) toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-106">This article shows you how toouse hello Azure CLI (azure.js) toocreate an IoT hub.</span></span>

<span data-ttu-id="6a8d0-107">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="6a8d0-108">Azure CLI (azure.js) – hello CLI för hello klassiska och resursen management-distributionsmodeller som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-108">Azure CLI (azure.js) – hello CLI for hello classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="6a8d0-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello nästa generations CLI för hello resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - hello next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="6a8d0-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="6a8d0-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-111">An active Azure account.</span></span> <span data-ttu-id="6a8d0-112">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="6a8d0-113">[Azure CLI 0.10.4] [ lnk-CLI-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="6a8d0-114">Om du redan har hello Azure CLI installerat verifiera du hello aktuell version hello Kommandotolken med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-114">If you already have hello Azure CLI installed, you can validate hello current version at hello command prompt with hello following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="6a8d0-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6a8d0-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6a8d0-116">hello Azure CLI måste vara i läget Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-116">hello Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="6a8d0-117">Konfigurera Azure-kontot och Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="6a8d0-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="6a8d0-118">Kommandotolken hello hello inloggningen genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-118">At hello command prompt, login by typing hello following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="6a8d0-119">Använd hello föreslagna webbläsare och koden tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-119">Use hello suggested web browser and code tooauthenticate.</span></span>
1. <span data-ttu-id="6a8d0-120">Om du har flera Azure-prenumerationer, ansluta tooAzure ger dig åtkomst tooall hello Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-120">If you have multiple Azure subscriptions, connecting tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="6a8d0-121">Du kan visa hello Azure-prenumerationer och identifiera vilka en är hello standard hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-121">You can view hello Azure subscriptions, and identify which one is hello default, using hello command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="6a8d0-122">tooset hello prenumerationskontext där du vill toorun hello resten av hello kommandon användas:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-122">tooset hello subscription context under which you want toorun hello rest of hello commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="6a8d0-123">Om du inte har en resursgrupp kan du skapa en med namnet **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="6a8d0-124">hello artikel [använda hello Azure CLI toomanage Azure resurser och resursgrupper] [ lnk-CLI-arm] innehåller mer information om hur toouse hello Azure CLI toomanage Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-124">hello article [Use hello Azure CLI toomanage Azure resources and resource groups][lnk-CLI-arm] provides more information about how toouse hello Azure CLI toomanage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="6a8d0-125">Skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6a8d0-125">Create an IoT Hub</span></span>

<span data-ttu-id="6a8d0-126">Obligatoriska parametrar:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="6a8d0-127">**resursgruppens namn**.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-127">**resource-group**.</span></span> <span data-ttu-id="6a8d0-128">hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-128">hello resource group name.</span></span> <span data-ttu-id="6a8d0-129">hello-formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, 1-64-längd.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-129">hello format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="6a8d0-130">**name**.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-130">**name**.</span></span> <span data-ttu-id="6a8d0-131">hello namnet på hello IoT-hubb toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-131">hello name of hello IoT hub toobe created.</span></span> <span data-ttu-id="6a8d0-132">hello-formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, längd på 3-50.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-132">hello format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="6a8d0-133">**plats**.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-133">**location**.</span></span> <span data-ttu-id="6a8d0-134">hello plats (azure region/datacenter) tooprovision hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-134">hello location (azure region/datacenter) tooprovision hello IoT hub.</span></span>
* <span data-ttu-id="6a8d0-135">**SKU-name**.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-135">**sku-name**.</span></span> <span data-ttu-id="6a8d0-136">hello namnet på hello sku och en av: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="6a8d0-136">hello name of hello sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="6a8d0-137">Hello senaste fullständig lista finns toohello sida med priser för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-137">For hello latest full list, refer toohello pricing page for IoT Hub.</span></span>
* <span data-ttu-id="6a8d0-138">**enheter**.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-138">**units**.</span></span> <span data-ttu-id="6a8d0-139">hello antal allokerade enheter.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-139">hello number of provisioned units.</span></span> <span data-ttu-id="6a8d0-140">-Intervall: F1 [1-1]: S1, S2 [1 200]: S3 [1-10].</span><span class="sxs-lookup"><span data-stu-id="6a8d0-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="6a8d0-141">IoT-hubbenheter baseras på din totala antal och hello meddelandenummer av enheter som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-141">IoT Hub units are based on your total message count and hello number of devices you want tooconnect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="6a8d0-142">toosee alla Hej parametrar som finns tillgängliga för att skapa, du kan använda hello hjälpkommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-142">toosee all hello parameters available for creation, you can use hello help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="6a8d0-143">Enkelt exempel: toocreate kallas för en IoT-hubb **exampleIoTHubName** i hello resursgruppen **exampleResourceGroup**kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-143">Quick example: toocreate an IoT Hub called **exampleIoTHubName** in hello resource group **exampleResourceGroup**, run hello following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="6a8d0-144">Den här Azure CLI-kommando skapar en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="6a8d0-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="6a8d0-145">Du kan ta bort hello IoT-hubb **exampleIoTHubName** med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-145">You can delete hello IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="6a8d0-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a8d0-146">Next steps</span></span>

<span data-ttu-id="6a8d0-147">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artikel:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-147">toolearn more about developing for IoT Hub, see hello following article:</span></span>

* <span data-ttu-id="6a8d0-148">[IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="6a8d0-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="6a8d0-149">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="6a8d0-149">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="6a8d0-150">[Med hjälp av hello Azure portal toomanage IoT-hubb][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="6a8d0-150">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
