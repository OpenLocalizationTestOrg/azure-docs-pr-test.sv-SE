---
title: "aaaCreate en Azure IoT-hubb med hjälp av en PowerShell-cmdlet | Microsoft Docs"
description: 'Hur toouse toocreate en PowerShell-cmdlet: en IoT-hubb.'
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 005cd8d48eb39d2e8b1bfb9ef84330d4aae4658f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a>Skapa en IoT-hubb med hello ny AzureRmIotHub cmdlet

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introduktion

Du kan använda Azure PowerShell-cmdlets toocreate och hantera Azure IoT-hubbar. De här självstudierna visar hur toocreate en IoT-hubb med PowerShell.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. <br/>Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure PowerShell-cmdlets][lnk-powershell-install].

## <a name="connect-tooyour-azure-subscription"></a>Ansluta tooyour Azure-prenumeration
Ange hello efter kommandot toosign i tooyour Azure-prenumeration i PowerShell-Kommandotolken:

```powershell
Login-AzureRmAccount
```

Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter. Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:

```powershell
Get-AzureRMSubscription
```

Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello. Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a>Skapa resursgrupp

Du behöver en resurs grupp toodeploy en IoT-hubb. Du kan använda en befintlig resursgrupp eller skapa en ny.

Du kan använda följande kommando toodiscover hello platser där du kan distribuera en IoT-hubb hello:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

toocreate en resursgrupp för din IoT-hubb hello stöds platser för IoT-hubb, Använd hello följande kommando. Det här exemplet skapar en resursgrupp med namnet **MyIoTRG1** i hello **östra USA** region:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

toocreate en IoT-hubb i hello resursgrupp som du skapade i hello föregående steg, Använd hello följande kommando. Det här exemplet skapas en **S1** hubb kallas **MyTestIoTHub** i hello **östra USA** region:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

Hej IoT-hubb hello namn måste vara unikt.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Du kan ange alla hello IoT-hubbar i din prenumeration med hjälp av hello följande kommando:

```powershell
Get-AzureRmIotHub
```

hello föregående exempel läggs en S1 Standard IoT-hubb du debiteras. Du kan ta bort hello IoT-hubb med hello följande kommando:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternativt kan du ta bort en resursgrupp och alla hello resurser som den innehåller med hello följande kommando:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Nästa steg

Nu du har distribuerat en IoT-hubb med hjälp av en PowerShell-cmdlet kan behöva du ytterligare tooexplore:

* Identifiera andra [PowerShell-cmdlets för att arbeta med din IoT-hubb][lnk-iothub-cmdlets].
* Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].

toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:

* [Introduktion tooC SDK][lnk-c-sdk]
* [Azure IoT-SDK][lnk-sdks]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med IoT kant][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
