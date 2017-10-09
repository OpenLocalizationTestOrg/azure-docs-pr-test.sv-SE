---
title: aaaCreate en IoT-hubb med Azure CLI (az.py) | Microsoft Docs
description: Hur en Azure IoT-hubb med toocreate hello plattformsoberoende Azure CLI 2.0 (az.py).
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 9c9639235c2ac343e6ceb9578291dafaea26ea24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a>Skapa en IoT-hubb med hello Azure CLI 2.0

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introduktion

Du kan använda Azure CLI 2.0 (az.py) toocreate och hantera Azure IoT-hubbar programmässigt. Den här artikeln visar hur hello toouse Azure CLI 2.0 (az.py) toocreate en IoT-hubb.

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI för hello klassisk och resurs management distributionsmodeller.
* Azure CLI 2.0 (az.py) - hello nästa generations CLI för hello management resursdistributionsmodell som beskrivs i den här artikeln.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Logga in och ange ditt Azure-konto

Logga in tooyour Azure-konto och välja din prenumeration.

1. Kör hello Kommandotolken hello [inloggningen kommandot][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Följ hello instruktioner tooauthenticate med hello kod och logga in tooyour Azure-konto via en webbläsare.

2. Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-konton som är associerade med dina autentiseringsuppgifter. Använder följande hello [kommandot toolist hello Azure konton] [ lnk-az-account-command] tillgängliga för du toouse:
    
    ```azurecli
    az account list 
    ```

    Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello. Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Skapa en IoT-hubb

Använd hello Azure CLI toocreate en resursgrupp och Lägg sedan till en IoT-hubb.

1. När du skapar en IoT-hubb skapar du den i en resursgrupp. Använd en befintlig resursgrupp eller kör följande hello [kommandot toocreate en resursgrupp][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > hello föregående exempel skapar hello resursgrupp i hello västra USA plats. Du kan visa en lista över tillgängliga platser genom att köra kommandot hello `az account list-locations -o table`.
    >
    >

2. Kör följande hello [kommandot toocreate en IoT-hubb] [ lnk-az-iot-command] med hjälp av ett globalt unikt namn för din IoT-hubb i din resursgrupp:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> hello föregående kommando skapar en IoT-hubb i hello S1 prisnivå du debiteras. Mer information finns i [Azure IoT Hub-priser][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Ta bort en IoT-hubb

Du kan använda hello Azure CLI för[ta bort en enskild resurs][lnk-az-resource-command], till exempel en IoT-hubb eller ta bort en resursgrupp och alla dess resurser, inklusive alla IoT-hubbar.

toodelete en IoT-hubb kör hello följande kommando:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

toodelete en resursgrupp och alla dess resurser, kör hello följande kommando:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Med hjälp av hello Azure portal toomanage IoT-hubb][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
