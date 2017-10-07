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
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="d8f5c-103">Skapa en IoT-hubb med hjälp av Azure Resource Manager-mall (.NET)</span><span class="sxs-lookup"><span data-stu-id="d8f5c-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="d8f5c-104">Du kan använda Azure Resource Manager toocreate och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="d8f5c-105">De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för en IoT-hubb från ett C#-program.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="d8f5c-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d8f5c-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="d8f5c-107">Den här artikeln täcker hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="d8f5c-108">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d8f5c-109">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d8f5c-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-110">An active Azure account.</span></span> <br/><span data-ttu-id="d8f5c-111">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d8f5c-112">En [Azure Storage-konto] [ lnk-storage-account] där du kan lagra dina mallfiler för Azure Resource Manager-.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="d8f5c-113">[Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="d8f5c-114">Förbered din Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="d8f5c-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="d8f5c-115">Skapa ett Visual C# klassiska skrivbordet projekt med hello i Visual Studio **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d8f5c-116">Namnet hello projektet **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-116">Name hello project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="d8f5c-117">Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="d8f5c-118">Kontrollera NuGet Package Manager **inkludera förhandsversion**, och på hello **Bläddra** sidan Sök efter **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-118">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="d8f5c-119">Välj hello paketet, klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licenser.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-119">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="d8f5c-120">I NuGet-Pakethanteraren, söka efter **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="d8f5c-121">Klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** tooaccept hello licens.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="d8f5c-122">I Program.cs ersätta hello befintliga **med** instruktioner med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-122">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="d8f5c-123">Lägg till följande statiska variabler som ersätter platshållarvärdena hello hello i Program.cs.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-123">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="d8f5c-124">Du antecknade **ApplicationId**, **SubscriptionId**, **TenantId**, och **lösenord** tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="d8f5c-125">**Namnet på ditt Azure Storage** är hello namnet på hello Azure Storage-konto där du lagrar mallfilerna för Azure Resource Manager-.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-125">**Your Azure Storage account name** is hello name of hello Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="d8f5c-126">**Resursgruppens namn** är hello namnet på hello resursgrupp som du använder när du skapar hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-126">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="d8f5c-127">hello namnet kan vara en befintlig eller ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-127">hello name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="d8f5c-128">**Distributionsnamnet** är ett namn för hello distributionen som **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

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

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="d8f5c-129">Skicka en mall toocreate en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d8f5c-129">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="d8f5c-130">Använda en JSON-mall och parametern filen toocreate en IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-130">Use a JSON template and parameter file toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="d8f5c-131">Du kan också använda en Azure Resource Manager mallen toomake ändringar tooan befintliga IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-131">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="d8f5c-132">Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="d8f5c-133">Lägg till en JSON-fil som kallas **template.json** tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-133">Add a JSON file called **template.json** tooyour project.</span></span>

2. <span data-ttu-id="d8f5c-134">tooadd en standard IoT-hubb toohello **östra USA** region, Ersätt hello innehållet i **template.json** med hello efter resursdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-134">tooadd a standard IoT hub toohello **East US** region, replace hello contents of **template.json** with hello following resource definition.</span></span> <span data-ttu-id="d8f5c-135">Hello aktuell lista över regioner som har stöd för IoT-hubb finns [Azure Status][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-135">For hello current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

3. <span data-ttu-id="d8f5c-136">Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="d8f5c-137">Lägg till en JSON-fil som kallas **parameters.json** tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-137">Add a JSON file called **parameters.json** tooyour project.</span></span>

4. <span data-ttu-id="d8f5c-138">Ersätt hello innehållet i **parameters.json** med hello följande parameterinformation som anger ett namn för hello ny IoT-hubb som **{din initialer} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-138">Replace hello contents of **parameters.json** with hello following parameter information that sets a name for hello new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="d8f5c-139">Hej IoT-hubbnamnet måste vara globalt unika så att den ska innehålla ditt namn eller initialer:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-139">hello IoT hub name must be globally unique so it should include your name or initials:</span></span>

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

5. <span data-ttu-id="d8f5c-140">I **Server Explorer**, ansluta tooyour Azure-prenumeration och i Azure Storage konto för att skapa en behållare som kallas **mallar**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-140">In **Server Explorer**, connect tooyour Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="d8f5c-141">I hello **egenskaper** panelen, ange hello **offentlig läsbehörighet** behörigheter för hello **mallar** behållare för**Blob**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-141">In hello **Properties** panel, set hello **Public Read Access** permissions for hello **templates** container too**Blob**.</span></span>

6. <span data-ttu-id="d8f5c-142">I **Server Explorer**, högerklicka på hello **mallar** behållaren och klicka sedan på **visa Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-142">In **Server Explorer**, right-click on hello **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="d8f5c-143">Klicka på hello **överför Blob** , Välj hello två filer, **parameters.json** och **templates.json**, och klicka sedan på **öppna** tooupload hello JSON filer toohello **mallar** behållare.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-143">Click hello **Upload Blob** button, select hello two files, **parameters.json** and **templates.json**, and then click **Open** tooupload hello JSON files toohello **templates** container.</span></span> <span data-ttu-id="d8f5c-144">hello URL: er för hello blob som innehåller hello JSON-data är:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-144">hello URLs of hello blobs containing hello JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="d8f5c-145">Lägg till följande metod tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-145">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="d8f5c-146">Lägg till följande kod toohello hello **CreateIoTHub** metoden toosubmit hello mallen och parametern filer toohello Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-146">Add hello following code toohello **CreateIoTHub** method toosubmit hello template and parameter files toohello Azure Resource Manager:</span></span>

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

9. <span data-ttu-id="d8f5c-147">Lägg till följande kod toohello hello **CreateIoTHub** metod som visar hello status och hello nycklar för hello ny IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-147">Add hello following code toohello **CreateIoTHub** method that displays hello status and hello keys for hello new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="d8f5c-148">Fullständig och kör hello program</span><span class="sxs-lookup"><span data-stu-id="d8f5c-148">Complete and run hello application</span></span>

<span data-ttu-id="d8f5c-149">Du kan nu slutföra hello program genom att anropa hello **CreateIoTHub** metoden innan du skapar och kör den.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-149">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="d8f5c-150">Lägg till hello efter koden toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-150">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="d8f5c-151">Klicka på **skapa** och sedan **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="d8f5c-152">Korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-152">Correct any errors.</span></span>

3. <span data-ttu-id="d8f5c-153">Klicka på **felsöka** och sedan **Start Debugging** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-153">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="d8f5c-154">Det kan ta flera minuter för hello distribution toorun.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-154">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="d8f5c-155">tooverify lägga till din programmet hello ny IoT-hubb, besök hello [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-155">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="d8f5c-156">Du kan också använda hello **Get-AzureRmResource** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-156">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="d8f5c-157">Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="d8f5c-158">Du kan ta bort hello IoT-hubb via hello [Azure-portalen] [ lnk-azure-portal] eller genom att använda hello **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-158">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8f5c-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8f5c-159">Next steps</span></span>
<span data-ttu-id="d8f5c-160">Nu du har distribuerat en IoT-hubb med en Azure Resource Manager-mall med ett C#-program kan behöva du ytterligare tooexplore:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want tooexplore further:</span></span>

* <span data-ttu-id="d8f5c-161">Läs mer om hello funktionerna i hello [IoT-hubb resursprovidern REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="d8f5c-161">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="d8f5c-162">Läs [översikt över Azure Resource Manager] [ lnk-azure-rm-overview] toolearn mer om hello funktioner i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d8f5c-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="d8f5c-163">toolearn mer information om hur du utvecklar för IoT-hubb finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-163">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="d8f5c-164">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="d8f5c-164">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="d8f5c-165">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d8f5c-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d8f5c-166">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="d8f5c-166">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d8f5c-167">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d8f5c-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
