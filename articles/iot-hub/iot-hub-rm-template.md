---
title: aaaCreate en Azure IoT-hubb med en mall (.NET) | Microsoft Docs
description: "Hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb med ett C#-program."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 6140deff3553701f994502fd4a60178f874e27cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Skapa en IoT-hubb med hjälp av Azure Resource Manager-mall (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Du kan använda Azure Resource Manager toocreate och hantera Azure IoT-hubbar programmässigt. De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb från ett C#-program.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. <br/>Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* En [Azure Storage-konto] [ lnk-storage-account] där du kan lagra dina mallfiler för Azure Resource Manager-.
* [Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Förbered din Visual Studio-projekt

1. Skapa ett Visual C# klassiska skrivbordet projekt med hello i Visual Studio **Konsolapp (.NET Framework)** projektmall. Namnet hello projektet **CreateIoTHub**.

2. Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket**.

3. Kontrollera NuGet Package Manager **inkludera förhandsversion**, och på hello **Bläddra** sidan Sök efter **Microsoft.Azure.Management.ResourceManager**. Välj hello paketet, klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licenser.

4. I NuGet-Pakethanteraren, söka efter **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licens.

5. I Program.cs ersätta hello befintliga **med** instruktioner med hello följande kod:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Lägg till följande statiska variabler som ersätter platshållarvärdena hello hello i Program.cs. Du antecknade **ApplicationId**, **SubscriptionId**, **TenantId**, och **lösenord** tidigare i den här kursen. **Namnet på ditt Azure Storage** är hello namnet på hello Azure Storage-konto där du lagrar mallfilerna för Azure Resource Manager-. **Resursgruppens namn** är hello namnet på hello resursgrupp som du använder när du skapar hello IoT-hubb. hello namnet kan vara en befintlig eller ny resursgrupp. **Distributionsnamnet** är ett namn för hello distributionen som **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Skicka en mall toocreate en IoT-hubb

Använda en JSON-mall och parametern filen toocreate en IoT-hubb i resursgruppen. Du kan också använda en Azure Resource Manager mallen toomake ändringar tooan befintliga IoT-hubb.

1. Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**. Lägg till en JSON-fil som kallas **template.json** tooyour projekt.

2. tooadd en standard IoT-hubb toohello **östra USA** region, Ersätt hello innehållet i **template.json** med hello efter resursdefinitionen. Hello aktuell lista över regioner som har stöd för IoT-hubb finns [Azure Status][lnk-status]:

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

3. Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**. Lägg till en JSON-fil som kallas **parameters.json** tooyour projekt.

4. Ersätt hello innehållet i **parameters.json** med hello följande parameterinformation som anger ett namn för hello ny IoT-hubb som **{din initialer} mynewiothub**. Hej IoT-hubbnamnet måste vara globalt unika så att den ska innehålla ditt namn eller initialer:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. I **Server Explorer**, ansluta tooyour Azure-prenumeration och i Azure Storage konto för att skapa en behållare som kallas **mallar**. I hello **egenskaper** panelen, ange hello **offentlig läsbehörighet** behörigheter för hello **mallar** behållare för**Blob**.

6. I **Server Explorer**, högerklicka på hello **mallar** behållaren och klicka sedan på **visa Blob-behållaren**. Klicka på hello **överför Blob** , Välj hello två filer, **parameters.json** och **templates.json**, och klicka sedan på **öppna** tooupload hello JSON filer toohello **mallar** behållare. hello URL: er för hello blob som innehåller hello JSON-data är:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Lägg till följande metod tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Lägg till följande kod toohello hello **CreateIoTHub** metoden toosubmit hello mallen och parametern filer toohello Azure Resource Manager:

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. Lägg till följande kod toohello hello **CreateIoTHub** metod som visar hello status och hello nycklar för hello ny IoT-hubb:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a>Fullständig och kör hello program

Du kan nu slutföra hello program genom att anropa hello **CreateIoTHub** metoden innan du skapar och kör den.

1. Lägg till hello efter koden toohello hello **Main** metoden:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Klicka på **skapa** och sedan **skapa lösning**. Korrigera eventuella fel.

3. Klicka på **felsöka** och sedan **Start Debugging** toorun hello program. Det kan ta flera minuter för hello distribution toorun.

4. tooverify lägga till din programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser. Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras. Du kan ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.

## <a name="next-steps"></a>Nästa steg
Nu du har distribuerat en IoT-hubb med en Azure Resource Manager-mall med ett C#-program kan behöva du ytterligare tooexplore:

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
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
