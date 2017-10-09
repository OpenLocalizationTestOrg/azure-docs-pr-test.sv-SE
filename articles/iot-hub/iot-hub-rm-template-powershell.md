---
title: aaaCreate en Azure IoT-hubb med en mall (PowerShell) | Microsoft Docs
description: "Hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb med PowerShell."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e98ff5e898200cd727b9326fb3df393e43b021e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Skapa en IoT-hubb med hjälp av Azure Resource Manager-mall (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Du kan använda Azure Resource Manager toocreate och hantera Azure IoT-hubbar programmässigt. De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb med PowerShell.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. <br/>Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.

> [!TIP]
> hello artikel [med hjälp av Azure PowerShell med Azure Resource Manager] [ lnk-powershell-arm] ger mer information om hur toouse PowerShell och Azure Resource Manager-mallar toocreate Azure resurser.

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

Du kan använda följande kommandon toodiscover där du kan distribuera en IoT-hubb och hello API-versioner som stöds för närvarande hello:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Skapa en resurs grupp toocontain din IoT-hubb med hjälp av följande kommando på någon av platserna hello stöds för IoT-hubb hello. Det här exemplet skapar en resursgrupp med namnet **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Skicka en mall toocreate en IoT-hubb

Använd en IoT-hubb för toocreate en JSON-mall i resursgruppen. Du kan också använda en Azure Resource Manager mallen toomake ändringar tooan befintliga IoT-hubb.

1. Använd en text editor toocreate som kallas för en Azure Resource Manager-mall **template.json** med hello följande resource definition toocreate en ny standard IoT-hubb. Det här exemplet lägger till hello IoT-hubb i hello **östra USA** region, skapar två konsumentgrupper (**cg1** och **cg2**) på hello Event Hub-kompatibel slutpunkt och använder hello **2016-02-03** API-versionen. Den här mallen också förväntas du toopass i hello IoT hub-namn som en parameter med namnet **hubName**. Hello aktuella listan över platser som har stöd för IoT-hubb finns [Azure Status][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Spara filen med hello Azure Resource Manager mallen på den lokala datorn. Det här exemplet förutsätter att du sparar den i en mapp med namnet **c:\templates**.

3. Kör följande kommando toodeploy hello din nya IoT-hubb som skickar hello namnet på din IoT-hubb som en parameter. I det här exemplet hello hello IoT-hubb heter `abcmyiothub`. hello namnet på din IoT-hubb måste vara globalt unika:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. hello utdata visar hello nycklar för hello IoT hub du skapat.

5. tooverify lägga till din programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser. Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras. Du kan ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.

## <a name="next-steps"></a>Nästa steg

Nu du har distribuerat en IoT-hubb med en Azure Resource Manager-mall med PowerShell kan behöva du ytterligare tooexplore:

* Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].
* Läs [översikt över Azure Resource Manager] [ lnk-azure-rm-overview] toolearn mer om hello funktioner i Azure Resource Manager.

toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:

* [Introduktion tooC SDK][lnk-c-sdk]
* [Azure IoT-SDK][lnk-sdks]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
