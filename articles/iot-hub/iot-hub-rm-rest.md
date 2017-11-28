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
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a><span data-ttu-id="f6cc9-103">Skapa en IoT-hubb med hello resursprovidern REST API (.NET)</span><span class="sxs-lookup"><span data-stu-id="f6cc9-103">Create an IoT hub using hello resource provider REST API (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="f6cc9-104">Du kan använda hello [IoT-hubb resursprovidern REST API] [ lnk-rest-api] toocreate och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-104">You can use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="f6cc9-105">Den här kursen visar hur hello toouse IoT-hubb resource provider REST API toocreate en IoT-hubb från ett C#-program.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-105">This tutorial shows you how toouse hello IoT Hub resource provider REST API toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="f6cc9-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f6cc9-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="f6cc9-107">Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="f6cc9-108">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="f6cc9-109">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="f6cc9-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-110">An active Azure account.</span></span> <br/><span data-ttu-id="f6cc9-111">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="f6cc9-112">[Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-112">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="f6cc9-113">Förbered din Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="f6cc9-113">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="f6cc9-114">Skapa ett Visual C# klassiska skrivbordet projekt med hello i Visual Studio **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-114">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="f6cc9-115">Namnet hello projektet **CreateIoTHubREST**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-115">Name hello project **CreateIoTHubREST**.</span></span>

2. <span data-ttu-id="f6cc9-116">Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-116">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="f6cc9-117">Kontrollera NuGet Package Manager **inkludera förhandsversion**, och på hello **Bläddra** sidan Sök efter **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-117">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="f6cc9-118">Välj hello paketet, klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licenser.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-118">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="f6cc9-119">I NuGet-Pakethanteraren, söka efter **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-119">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="f6cc9-120">Klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licens.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-120">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="f6cc9-121">I Program.cs ersätta hello befintliga **med** instruktioner med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-121">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

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

6. <span data-ttu-id="f6cc9-122">Lägg till följande statiska variabler som ersätter platshållarvärdena hello hello i Program.cs.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-122">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="f6cc9-123">Du antecknade **ApplicationId**, **SubscriptionId**, **TenantId**, och **lösenord** tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-123">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="f6cc9-124">**Resursgruppens namn** är hello namnet på hello resursgrupp som du använder när du skapar hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-124">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="f6cc9-125">Du kan använda en befintlig eller en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-125">You can use a pre-existing or a new resource group.</span></span> <span data-ttu-id="f6cc9-126">**IoT-hubbnamnet** är hello namnet på hello IoT-hubb som du skapar, till exempel **MyIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-126">**IoT Hub name** is hello name of hello IoT Hub you create, such as **MyIoTHub**.</span></span> <span data-ttu-id="f6cc9-127">hello namnet på din IoT-hubb måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-127">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="f6cc9-128">**Distributionsnamnet** är ett namn för hello distributionen som **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

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

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a><span data-ttu-id="f6cc9-129">Använd hello resource provider REST API toocreate en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="f6cc9-129">Use hello resource provider REST API toocreate an IoT hub</span></span>

<span data-ttu-id="f6cc9-130">Använd hello [IoT-hubb resursprovidern REST API] [ lnk-rest-api] toocreate en IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-130">Use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="f6cc9-131">Du kan också använda hello resource provider REST API toomake ändringar tooan befintliga IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-131">You can also use hello resource provider REST API toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="f6cc9-132">Lägg till följande metod tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-132">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. <span data-ttu-id="f6cc9-133">Lägg till följande kod toohello hello **CreateIoTHub** metod.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-133">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="f6cc9-134">Den här koden skapar en **HttpClient** objekt med hello autentiseringstoken i hello rubriker:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-134">This code creates an **HttpClient** object with hello authentication token in hello headers:</span></span>

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. <span data-ttu-id="f6cc9-135">Lägg till följande kod toohello hello **CreateIoTHub** metod.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-135">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="f6cc9-136">Den här koden beskriver hello IoT-hubb toocreate och genererar en JSON-representation.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-136">This code describes hello IoT hub toocreate and generates a JSON representation.</span></span> <span data-ttu-id="f6cc9-137">Hello aktuella listan över platser som har stöd för IoT-hubb finns [Azure Status][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-137">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

4. <span data-ttu-id="f6cc9-138">Lägg till följande kod toohello hello **CreateIoTHub** metod.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-138">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="f6cc9-139">Den här koden skickar hello REST begäran tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-139">This code submits hello REST request tooAzure.</span></span> <span data-ttu-id="f6cc9-140">hello kod kontrollerar hello svar och hämtar hello-URL som du kan använda toomonitor hello tillstånd hello distribution uppgiften:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-140">hello code then checks hello response and retrieves hello URL you can use toomonitor hello state of hello deployment task:</span></span>

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

5. <span data-ttu-id="f6cc9-141">Lägg till hello efter koden toohello hello **CreateIoTHub** metod.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-141">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="f6cc9-142">Den här koden använder hello **asyncStatusUri** adressen hämtas i hello föregående steg toowait för hello distribution toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-142">This code uses hello **asyncStatusUri** address retrieved in hello previous step toowait for hello deployment toocomplete:</span></span>

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. <span data-ttu-id="f6cc9-143">Lägg till hello efter koden toohello hello **CreateIoTHub** metod.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-143">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="f6cc9-144">Den här koden hämtar hello nycklar av hello IoT hub du skapat och skriver ut dem toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-144">This code retrieves hello keys of hello IoT hub you created and prints them toohello console:</span></span>

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="f6cc9-145">Fullständig och kör hello program</span><span class="sxs-lookup"><span data-stu-id="f6cc9-145">Complete and run hello application</span></span>

<span data-ttu-id="f6cc9-146">Du kan nu slutföra hello program genom att anropa hello **CreateIoTHub** metoden innan du skapar och kör den.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-146">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="f6cc9-147">Lägg till hello efter koden toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-147">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. <span data-ttu-id="f6cc9-148">Klicka på **skapa** och sedan **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-148">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="f6cc9-149">Korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-149">Correct any errors.</span></span>

3. <span data-ttu-id="f6cc9-150">Klicka på **felsöka** och sedan **Start Debugging** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-150">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="f6cc9-151">Det kan ta flera minuter för hello distribution toorun.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-151">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="f6cc9-152">tooverify som lagts till programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-152">tooverify that your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="f6cc9-153">Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-153">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="f6cc9-154">Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-154">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="f6cc9-155">När du är klar kan du ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-155">When you are finished, you can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6cc9-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6cc9-156">Next steps</span></span>
<span data-ttu-id="f6cc9-157">Nu du har distribuerat en IoT-hubb med hello resursprovidern REST-API kan behöva du tooexplore ytterligare:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-157">Now you have deployed an IoT hub using hello resource provider REST API, you may want tooexplore further:</span></span>

* <span data-ttu-id="f6cc9-158">Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="f6cc9-158">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="f6cc9-159">Läs [översikt över Azure Resource Manager] [ lnk-azure-rm-overview] toolearn mer om hello funktioner i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f6cc9-159">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="f6cc9-160">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-160">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="f6cc9-161">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="f6cc9-161">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="f6cc9-162">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="f6cc9-162">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="f6cc9-163">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="f6cc9-163">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="f6cc9-164">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="f6cc9-164">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
