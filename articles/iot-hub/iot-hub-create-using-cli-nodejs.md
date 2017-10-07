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
# <a name="create-an-iot-hub-using-hello-azure-cli"></a>Skapa en IoT-hubb med hello Azure CLI

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introduktion

Du kan använda Azure CLI (azure.js) toocreate och hantera Azure IoT-hubbar programmässigt. Den här artikeln visar hur hello toouse Azure CLI (azure.js) toocreate en IoT-hubb.

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* Azure CLI (azure.js) – hello CLI för hello klassiska och resursen management-distributionsmodeller som beskrivs i den här artikeln.
* [Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello nästa generations CLI för hello resursdistributionsmodell för hantering.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure CLI 0.10.4] [ lnk-CLI-install] eller senare. Om du redan har hello Azure CLI installerat verifiera du hello aktuell version hello Kommandotolken med hello följande kommando:

```azurecli
azure --version
```

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). hello Azure CLI måste vara i läget Azure Resource Manager:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Konfigurera Azure-kontot och Azure-prenumerationen

1. Kommandotolken hello hello inloggningen genom att skriva följande kommando:

   ```azurecli
    azure login
   ```

   Använd hello föreslagna webbläsare och koden tooauthenticate.
1. Om du har flera Azure-prenumerationer, ansluta tooAzure ger dig åtkomst tooall hello Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter. Du kan visa hello Azure-prenumerationer och identifiera vilka en är hello standard hello-kommandot:

   ```azurecli
    azure account list
   ```

   tooset hello prenumerationskontext där du vill toorun hello resten av hello kommandon användas:

   ```azurecli
    azure account set <subscription name>
   ```

1. Om du inte har en resursgrupp kan du skapa en med namnet **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> hello artikel [använda hello Azure CLI toomanage Azure resurser och resursgrupper] [ lnk-CLI-arm] innehåller mer information om hur toouse hello Azure CLI toomanage Azure resurser.

## <a name="create-an-iot-hub"></a>Skapa en IoT-hubb

Obligatoriska parametrar:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **resursgruppens namn**. hello resursgruppens namn. hello-formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, 1-64-längd.
* **name**. hello namnet på hello IoT-hubb toobe skapas. hello-formatet är skiftlägeskänsligt alfanumerisk, understreck och bindestreck, längd på 3-50.
* **plats**. hello plats (azure region/datacenter) tooprovision hello IoT-hubb.
* **SKU-name**. hello namnet på hello sku och en av: [F1, S1, S2, S3]. Hello senaste fullständig lista finns toohello sida med priser för IoT-hubb.
* **enheter**. hello antal allokerade enheter. -Intervall: F1 [1-1]: S1, S2 [1 200]: S3 [1-10]. IoT-hubbenheter baseras på din totala antal och hello meddelandenummer av enheter som du vill tooconnect.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

toosee alla Hej parametrar som finns tillgängliga för att skapa, du kan använda hello hjälpkommando i Kommandotolken:

```azurecli
azure iothub create -h
```

Enkelt exempel: toocreate kallas för en IoT-hubb **exampleIoTHubName** i hello resursgruppen **exampleResourceGroup**kör hello följande kommando:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Den här Azure CLI-kommando skapar en S1 Standard IoT-hubb du debiteras. Du kan ta bort hello IoT-hubb **exampleIoTHubName** med hjälp av följande kommando:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Nästa steg

toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artikel:

* [IoT-SDK][lnk-sdks]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Med hjälp av hello Azure portal toomanage IoT-hubb][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
