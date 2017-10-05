---
title: Skapa en Azure IoT-hubb med en mall (.NET) | Microsoft Docs
description: "Hur du använder en Azure Resource Manager-mall för att skapa en IoT-hubb med ett C#-program."
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
ms.openlocfilehash: 0f197a28e0c51b06d0b47a03c29fe1fde0c6b78d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="e23e4-103">Skapa en IoT-hubb med hjälp av Azure Resource Manager-mall (.NET)</span><span class="sxs-lookup"><span data-stu-id="e23e4-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="e23e4-104">Du kan använda Azure Resource Manager för att skapa och hantera Azure IoT-hubbar programmässigt.</span><span class="sxs-lookup"><span data-stu-id="e23e4-104">You can use Azure Resource Manager to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="e23e4-105">Den här kursen visar hur du skapar en IoT-hubb från ett C#-program med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="e23e4-105">This tutorial shows you how to use an Azure Resource Manager template to create an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="e23e4-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e23e4-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="e23e4-107">Den här artikeln täcker Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e23e4-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="e23e4-108">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e23e4-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="e23e4-109">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e23e4-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="e23e4-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e23e4-110">An active Azure account.</span></span> <br/><span data-ttu-id="e23e4-111">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e23e4-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="e23e4-112">En [Azure Storage-konto] [ lnk-storage-account] där du kan lagra dina mallfiler för Azure Resource Manager-.</span><span class="sxs-lookup"><span data-stu-id="e23e4-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="e23e4-113">[Azure PowerShell 1.0] [ lnk-powershell-install] eller senare.</span><span class="sxs-lookup"><span data-stu-id="e23e4-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="e23e4-114">Förbered din Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e23e4-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="e23e4-115">I Visual Studio, skapa ett Visual C# klassiska skrivbordet projekt med den **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e23e4-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="e23e4-116">Namnge projektet **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-116">Name the project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="e23e4-117">Högerklicka på projektet i Solution Explorer och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="e23e4-118">Kontrollera NuGet Package Manager **inkludera förhandsversion**, och på den **Bläddra** sidan Sök efter **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-118">In NuGet Package Manager, check **Include prerelease**, and on the **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="e23e4-119">Välj paketet, klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** att acceptera licenser.</span><span class="sxs-lookup"><span data-stu-id="e23e4-119">Select the package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the licenses.</span></span>

4. <span data-ttu-id="e23e4-120">I NuGet-Pakethanteraren, söka efter **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="e23e4-121">Klicka på **installera**i **granska ändringar** klickar du på **OK**, klicka på **jag accepterar** att acceptera licensvillkoren.</span><span class="sxs-lookup"><span data-stu-id="e23e4-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the license.</span></span>

5. <span data-ttu-id="e23e4-122">I Program.cs ersätta den befintliga **med** instruktioner med följande kod:</span><span class="sxs-lookup"><span data-stu-id="e23e4-122">In Program.cs, replace the existing **using** statements with the following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="e23e4-123">Lägg till följande statiska variabler ersätta platshållarvärdena i Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e23e4-123">In Program.cs, add the following static variables replacing the placeholder values.</span></span> <span data-ttu-id="e23e4-124">Du antecknade **ApplicationId**, **SubscriptionId**, **TenantId**, och **lösenord** tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e23e4-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="e23e4-125">**Namnet på ditt Azure Storage** är namnet på Azure Storage-konto där du lagrar mallfilerna för Azure Resource Manager-.</span><span class="sxs-lookup"><span data-stu-id="e23e4-125">**Your Azure Storage account name** is the name of the Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="e23e4-126">**Resursgruppens namn** är namnet på resursgruppen som du använder när du skapar en IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="e23e4-126">**Resource group name** is the name of the resource group you use when you create the IoT hub.</span></span> <span data-ttu-id="e23e4-127">Namnet kan vara en befintlig eller ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e23e4-127">The name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="e23e4-128">**Distributionsnamnet** är ett namn för distributionen, till exempel **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-128">**Deployment name** is a name for the deployment, such as **Deployment_01**.</span></span>

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

## <a name="submit-a-template-to-create-an-iot-hub"></a><span data-ttu-id="e23e4-129">Skicka en mall för att skapa en IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="e23e4-129">Submit a template to create an IoT hub</span></span>

<span data-ttu-id="e23e4-130">Använda en JSON-fil i mallen och parametern för att skapa en IoT-hubb i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e23e4-130">Use a JSON template and parameter file to create an IoT hub in your resource group.</span></span> <span data-ttu-id="e23e4-131">Du kan också använda en Azure Resource Manager-mall för att göra ändringar i en befintlig IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e23e4-131">You can also use an Azure Resource Manager template to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="e23e4-132">Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="e23e4-133">Lägg till en JSON-fil som kallas **template.json** i projektet.</span><span class="sxs-lookup"><span data-stu-id="e23e4-133">Add a JSON file called **template.json** to your project.</span></span>

2. <span data-ttu-id="e23e4-134">Att lägga till en IoT-hubb som standard till den **östra USA** region, ersätter du innehållet i **template.json** med följande resursdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="e23e4-134">To add a standard IoT hub to the **East US** region, replace the contents of **template.json** with the following resource definition.</span></span> <span data-ttu-id="e23e4-135">Den aktuella listan över regioner som har stöd för IoT-hubb finns [Azure Status][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="e23e4-135">For the current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

3. <span data-ttu-id="e23e4-136">Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="e23e4-137">Lägg till en JSON-fil som kallas **parameters.json** i projektet.</span><span class="sxs-lookup"><span data-stu-id="e23e4-137">Add a JSON file called **parameters.json** to your project.</span></span>

4. <span data-ttu-id="e23e4-138">Ersätt innehållet i **parameters.json** med följande parameterinformation som anger ett namn för den nya IoT-hubben som **{din initialer} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-138">Replace the contents of **parameters.json** with the following parameter information that sets a name for the new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="e23e4-139">IoT-hubbnamnet måste vara globalt unika så att den ska innehålla ditt namn eller initialer:</span><span class="sxs-lookup"><span data-stu-id="e23e4-139">The IoT hub name must be globally unique so it should include your name or initials:</span></span>

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

5. <span data-ttu-id="e23e4-140">I **Server Explorer**, ansluta till din Azure-prenumeration och skapa en behållare som kallas i Azure Storage-konto **mallar**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-140">In **Server Explorer**, connect to your Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="e23e4-141">I den **egenskaper** panelen genom att ange den **offentlig läsbehörighet** behörigheter för den **mallar** behållare för att **Blob**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-141">In the **Properties** panel, set the **Public Read Access** permissions for the **templates** container to **Blob**.</span></span>

6. <span data-ttu-id="e23e4-142">I **Server Explorer**, högerklicka på den **mallar** behållaren och klicka sedan på **visa Blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-142">In **Server Explorer**, right-click on the **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="e23e4-143">Klicka på den **överför Blob** , Välj två filer, **parameters.json** och **templates.json**, och klicka sedan på **öppna** överför JSON-filer till den **mallar** behållare.</span><span class="sxs-lookup"><span data-stu-id="e23e4-143">Click the **Upload Blob** button, select the two files, **parameters.json** and **templates.json**, and then click **Open** to upload the JSON files to the **templates** container.</span></span> <span data-ttu-id="e23e4-144">URL: er för de blobbar som innehåller de JSON-data är:</span><span class="sxs-lookup"><span data-stu-id="e23e4-144">The URLs of the blobs containing the JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="e23e4-145">Lägg till följande metod i Program.cs:</span><span class="sxs-lookup"><span data-stu-id="e23e4-145">Add the following method to Program.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="e23e4-146">Lägg till följande kod i den **CreateIoTHub** metod för att skicka filer till Azure Resource Manager-mall och parameter:</span><span class="sxs-lookup"><span data-stu-id="e23e4-146">Add the following code to the **CreateIoTHub** method to submit the template and parameter files to the Azure Resource Manager:</span></span>

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

9. <span data-ttu-id="e23e4-147">Lägg till följande kod i den **CreateIoTHub** metod som visar status och nycklarna för ny IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="e23e4-147">Add the following code to the **CreateIoTHub** method that displays the status and the keys for the new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a><span data-ttu-id="e23e4-148">Slutför och köra programmet</span><span class="sxs-lookup"><span data-stu-id="e23e4-148">Complete and run the application</span></span>

<span data-ttu-id="e23e4-149">Du kan nu slutföra programmet genom att anropa den **CreateIoTHub** metoden innan du skapar och kör den.</span><span class="sxs-lookup"><span data-stu-id="e23e4-149">You can now complete the application by calling the **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="e23e4-150">Lägg till följande kod i slutet av den **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e23e4-150">Add the following code to the end of the **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="e23e4-151">Klicka på **skapa** och sedan **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="e23e4-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="e23e4-152">Korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="e23e4-152">Correct any errors.</span></span>

3. <span data-ttu-id="e23e4-153">Klicka på **felsöka** och sedan **Start Debugging** att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="e23e4-153">Click **Debug** and then **Start Debugging** to run the application.</span></span> <span data-ttu-id="e23e4-154">Det kan ta flera minuter för att distributionen ska köras.</span><span class="sxs-lookup"><span data-stu-id="e23e4-154">It may take several minutes for the deployment to run.</span></span>

4. <span data-ttu-id="e23e4-155">För att verifiera ditt program läggs ny IoT-hubben, finns det [Azure-portalen] [ lnk-azure-portal] och visa en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="e23e4-155">To verify your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="e23e4-156">Du kan också använda den **Get-AzureRmResource** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e23e4-156">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="e23e4-157">Det här exempelprogrammet lägger till en S1 Standard IoT-hubb du debiteras.</span><span class="sxs-lookup"><span data-stu-id="e23e4-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="e23e4-158">Du kan ta bort IoT-hubb via den [Azure-portalen] [ lnk-azure-portal] eller genom att använda den **ta bort AzureRmResource** PowerShell-cmdlet när du är klar.</span><span class="sxs-lookup"><span data-stu-id="e23e4-158">You can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e23e4-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e23e4-159">Next steps</span></span>
<span data-ttu-id="e23e4-160">Nu du har distribuerat en IoT-hubb med en Azure Resource Manager-mall med ett C#-program, kanske du vill utforska vidare:</span><span class="sxs-lookup"><span data-stu-id="e23e4-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want to explore further:</span></span>

* <span data-ttu-id="e23e4-161">Läs mer om funktionerna i den [IoT-hubb resursprovidern REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="e23e4-161">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="e23e4-162">Läs [översikt över Azure Resource Manager] [ lnk-azure-rm-overview] att lära dig mer om funktionerna i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e23e4-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="e23e4-163">Mer information om hur du utvecklar för IoT-hubb finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="e23e4-163">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="e23e4-164">[Introduktion till C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="e23e4-164">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="e23e4-165">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="e23e4-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="e23e4-166">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="e23e4-166">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="e23e4-167">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="e23e4-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
