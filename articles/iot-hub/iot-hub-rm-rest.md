---
title: en Azure IoT-hubb med aaaCreate hello resursprovidern REST API | Microsoft Docs
description: Hur hello toouse resource provider REST API toocreate en IoT-hubb.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 98d240ccce47dec13a255bce28943b40f5354ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a>Skapa en IoT-hubb med hello resursprovidern REST API (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Du kan använda hello [IoT-hubb resursprovidern REST API] [ lnk-rest-api] toocreate och hantera Azure IoT-hubbar programmässigt. Den här kursen visar hur hello toouse IoT-hubb resource provider REST API toocreate en IoT-hubb från ett C#-program.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. <br/>Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Förbered din Visual Studio-projekt

1. Skapa ett Visual C# klassiska skrivbordet projekt med hello i Visual Studio **Konsolapp (.NET Framework)** projektmall. Namnet hello projektet **CreateIoTHubREST**.

2. Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket**.

3. Kontrollera NuGet Package Manager **inkludera förhandsversion**, och på hello **Bläddra** sidan Sök efter **Microsoft.Azure.Management.ResourceManager**. Välj hello paketet, klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licenser.

4. I NuGet-Pakethanteraren, söka efter **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licens.

5. I Program.cs ersätta hello befintliga **med** instruktioner med hello följande kod:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. Lägg till följande statiska variabler som ersätter platshållarvärdena hello hello i Program.cs. Du antecknade **ApplicationId**, **SubscriptionId**, **TenantId**, och **lösenord** tidigare i den här kursen. **Resursgruppens namn** är hello namnet på hello resursgrupp som du använder när du skapar hello IoT-hubb. Du kan använda en befintlig eller en ny resursgrupp. **IoT-hubbnamnet** är hello namnet på hello IoT-hubb som du skapar, till exempel **MyIoTHub**. hello namnet på din IoT-hubb måste vara globalt unika. **Distributionsnamnet** är ett namn för hello distributionen som **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a>Använd hello resource provider REST API toocreate en IoT-hubb

Använd hello [IoT-hubb resursprovidern REST API] [ lnk-rest-api] toocreate en IoT-hubb i resursgruppen. Du kan också använda hello resource provider REST API toomake ändringar tooan befintliga IoT-hubb.

1. Lägg till följande metod tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Lägg till följande kod toohello hello **CreateIoTHub** metod. Den här koden skapar en **HttpClient** objekt med hello autentiseringstoken i hello rubriker:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Lägg till följande kod toohello hello **CreateIoTHub** metod. Den här koden beskriver hello IoT-hubb toocreate och genererar en JSON-representation. Hello aktuella listan över platser som har stöd för IoT-hubb finns [Azure Status][lnk-status]:

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Lägg till följande kod toohello hello **CreateIoTHub** metod. Den här koden skickar hello REST begäran tooAzure. hello kod kontrollerar hello svar och hämtar hello-URL som du kan använda toomonitor hello tillstånd hello distribution uppgiften:

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Lägg till hello efter koden toohello hello **CreateIoTHub** metod. Den här koden använder hello **asyncStatusUri** adressen hämtas i hello föregående steg toowait för hello distribution toocomplete:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Lägg till hello efter koden toohello hello **CreateIoTHub** metod. Den här koden hämtar hello nycklar av hello IoT hub du skapat och skriver ut dem toohello konsolen:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a>Fullständig och kör hello program

Du kan nu slutföra hello program genom att anropa hello **CreateIoTHub** metoden innan du skapar och kör den.

1. Lägg till hello efter koden toohello hello **Main** metoden:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Klicka på **skapa** och sedan **skapa lösning**. Korrigera eventuella fel.

3. Klicka på **felsöka** och sedan **Start Debugging** toorun hello program. Det kan ta flera minuter för hello distribution toorun.

4. tooverify som lagts till programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser. Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras. När du är klar kan du ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.

## <a name="next-steps"></a>Nästa steg
Nu du har distribuerat en IoT-hubb med hello resursprovidern REST-API kan behöva du tooexplore ytterligare:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
