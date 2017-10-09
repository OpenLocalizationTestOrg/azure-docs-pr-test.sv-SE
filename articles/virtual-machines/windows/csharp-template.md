---
title: "aaaDeploy en virtuell dator med hjälp av C# och en Resource Manager-mall | Microsoft Docs"
description: "Lär dig toohow toouse C# och toodeploy en Resource Manager-mall för en Azure VM."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: davidmu
ms.openlocfilehash: 91d470228cfeed72336b488ffef4dfbb25bc3a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a><span data-ttu-id="a0921-103">Distribuera en Azure-dator med hjälp av C# och Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="a0921-103">Deploy an Azure Virtual Machine using C# and a Resource Manager template</span></span>
<span data-ttu-id="a0921-104">Den här artikeln beskrivs hur du toodeploy en Azure Resource Manager-mallen med C#.</span><span class="sxs-lookup"><span data-stu-id="a0921-104">This article shows you how toodeploy an Azure Resource Manager template using C#.</span></span> <span data-ttu-id="a0921-105">hello-mall som du skapar distribuerar en enskild virtuell dator som kör Windows Server i ett nytt virtuellt nätverk med ett enda undernät.</span><span class="sxs-lookup"><span data-stu-id="a0921-105">hello template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="a0921-106">En detaljerad beskrivning av hello virtuell datorresurs finns [virtuella datorer i en Azure Resource Manager-mall](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="a0921-106">For a detailed description of hello virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="a0921-107">Mer information om alla hello resurser i en mall finns [genomgång av Azure Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="a0921-107">For more information about all hello resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="a0921-108">Det tar cirka 10 minuter toodo dessa steg.</span><span class="sxs-lookup"><span data-stu-id="a0921-108">It takes about 10 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="a0921-109">Skapa ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="a0921-109">Create a Visual Studio project</span></span>

<span data-ttu-id="a0921-110">I det här steget ska kontrollera du att Visual Studio är installerat och du skapar en konsol program som används för toodeploy hello mall.</span><span class="sxs-lookup"><span data-stu-id="a0921-110">In this step, you make sure that Visual Studio is installed and you create a console application used toodeploy hello template.</span></span>

1. <span data-ttu-id="a0921-111">Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a0921-111">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="a0921-112">Välj **.NET skrivbord development** på hello arbetsbelastningar sidan och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="a0921-112">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="a0921-113">I hello sammanfattning, kan du se att **utvecklingsverktyg för .NET Framework 4 4.6** väljs automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="a0921-113">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="a0921-114">Om du redan har installerat Visual Studio kan du lägga till hello .NET arbetsbelastning med hello Visual Studio starta.</span><span class="sxs-lookup"><span data-stu-id="a0921-114">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="a0921-115">I Visual Studio klickar du på **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a0921-115">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="a0921-116">I **mallar** > **Visual C#**väljer **Konsolapp (.NET Framework)**, ange *myDotnetProject* för hello namn Hej projektet, Välj hello platsen för hello-projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0921-116">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-packages"></a><span data-ttu-id="a0921-117">Installera hello-paket</span><span class="sxs-lookup"><span data-stu-id="a0921-117">Install hello packages</span></span>

<span data-ttu-id="a0921-118">NuGet-paket är hello enklaste sättet tooinstall hello bibliotek som du behöver toofinish dessa steg.</span><span class="sxs-lookup"><span data-stu-id="a0921-118">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="a0921-119">tooget hello bibliotek som du behöver i Visual Studio gör dessa steg:</span><span class="sxs-lookup"><span data-stu-id="a0921-119">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="a0921-120">Klicka på **verktyg** > **Nuget Package Manager**, och klicka sedan på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="a0921-120">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="a0921-121">Ange följande kommandon i hello-konsolen:</span><span class="sxs-lookup"><span data-stu-id="a0921-121">Type these commands in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a><span data-ttu-id="a0921-122">Skapa hello-filer</span><span class="sxs-lookup"><span data-stu-id="a0921-122">Create hello files</span></span>

<span data-ttu-id="a0921-123">I det här steget skapar du en mallfil som distribuerar hello resurser och en fil med parametrar som tillhandahåller parametern värden toohello mall.</span><span class="sxs-lookup"><span data-stu-id="a0921-123">In this step, you create a template file that deploys hello resources and a parameters file that supplies parameter values toohello template.</span></span> <span data-ttu-id="a0921-124">Du kan också skapa en auktoriseringsfil som har använt tooperform Azure Resource Manager-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a0921-124">You also create an authorization file that is used tooperform Azure Resource Manager operations.</span></span>

### <a name="create-hello-template-file"></a><span data-ttu-id="a0921-125">Skapa hello mallfil</span><span class="sxs-lookup"><span data-stu-id="a0921-125">Create hello template file</span></span>

1. <span data-ttu-id="a0921-126">I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*.</span><span class="sxs-lookup"><span data-stu-id="a0921-126">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="a0921-127">Namnet hello filen *CreateVMTemplate.json*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a0921-127">Name hello file *CreateVMTemplate.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="a0921-128">Lägg till den här JSON-kod toohello-fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="a0921-128">Add this JSON code toohello file that you created:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. <span data-ttu-id="a0921-129">Spara hello CreateVMTemplate.json filen.</span><span class="sxs-lookup"><span data-stu-id="a0921-129">Save hello CreateVMTemplate.json file.</span></span>

### <a name="create-hello-parameters-file"></a><span data-ttu-id="a0921-130">Skapa hello parameterfilen</span><span class="sxs-lookup"><span data-stu-id="a0921-130">Create hello parameters file</span></span>

<span data-ttu-id="a0921-131">toospecify värden för hello Resursparametrar som definieras i mallen för hello, skapa en fil med parametrar som innehåller hello värden.</span><span class="sxs-lookup"><span data-stu-id="a0921-131">toospecify values for hello resource parameters that are defined in hello template, you create a parameters file that contains hello values.</span></span>

1. <span data-ttu-id="a0921-132">I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*.</span><span class="sxs-lookup"><span data-stu-id="a0921-132">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="a0921-133">Namnet hello filen *Parameters.json*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a0921-133">Name hello file *Parameters.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="a0921-134">Lägg till den här JSON-kod toohello-fil som du skapade:</span><span class="sxs-lookup"><span data-stu-id="a0921-134">Add this JSON code toohello file that you created:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. <span data-ttu-id="a0921-135">Spara hello Parameters.json filen.</span><span class="sxs-lookup"><span data-stu-id="a0921-135">Save hello Parameters.json file.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="a0921-136">Skapa hello auktoriseringsfilen</span><span class="sxs-lookup"><span data-stu-id="a0921-136">Create hello authorization file</span></span>

<span data-ttu-id="a0921-137">Innan du kan distribuera en mall måste du se till att du har åtkomst tooan [Active Directory-tjänstens huvudnamn](../../resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="a0921-137">Before you can deploy a template, make sure that you have access tooan [Active Directory service principal](../../resource-group-authenticate-service-principal.md).</span></span> <span data-ttu-id="a0921-138">Från hello tjänstens huvudnamn skaffa du en token för att autentisera begäranden tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0921-138">From hello service principal, you acquire a token for authenticating requests tooAzure Resource Manager.</span></span> <span data-ttu-id="a0921-139">Du bör också anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i hello auktoriseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="a0921-139">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in hello authorization file.</span></span>

1. <span data-ttu-id="a0921-140">I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*.</span><span class="sxs-lookup"><span data-stu-id="a0921-140">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="a0921-141">Namnet hello filen *azureauth.properties*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a0921-141">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="a0921-142">Lägg till dessa auktorisering egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a0921-142">Add these authorization properties:</span></span>

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    <span data-ttu-id="a0921-143">Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med hello Active Directory-program identifierare,  **&lt;autentiseringsnyckel&gt;**  med hello programmet nyckel och  **&lt;klient-id&gt;**  med hello-klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="a0921-143">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="a0921-144">Spara hello azureauth.properties filen.</span><span class="sxs-lookup"><span data-stu-id="a0921-144">Save hello azureauth.properties file.</span></span>
4. <span data-ttu-id="a0921-145">Ange en miljövariabel i Windows som heter AZURE_AUTH_LOCATION med hello fullständig sökväg tooauthorization filen som du skapade, till exempel hello följande PowerShell-kommandot kan användas:</span><span class="sxs-lookup"><span data-stu-id="a0921-145">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created, for example hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a><span data-ttu-id="a0921-146">Skapa hello management-klienten</span><span class="sxs-lookup"><span data-stu-id="a0921-146">Create hello management client</span></span>

1. <span data-ttu-id="a0921-147">Öppna hello Program.cs-filen för hello-projekt som du skapade och Lägg sedan till dem med instruktioner toohello befintliga instruktioner överst i filen hello:</span><span class="sxs-lookup"><span data-stu-id="a0921-147">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. <span data-ttu-id="a0921-148">toocreate hello management-klienten, lägger du till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="a0921-148">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="a0921-149">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a0921-149">Create a resource group</span></span>

<span data-ttu-id="a0921-150">toospecify värden för programmet hello, lägger du till kod toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="a0921-150">toospecify values for hello application, add code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a><span data-ttu-id="a0921-151">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a0921-151">Create a storage account</span></span>

<span data-ttu-id="a0921-152">hello mall och parametrar distribueras från ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="a0921-152">hello template and parameters are deployed from a storage account in Azure.</span></span> <span data-ttu-id="a0921-153">I det här steget Skapa hello och ladda upp hello-filer.</span><span class="sxs-lookup"><span data-stu-id="a0921-153">In this step, you create hello account and upload hello files.</span></span> 

<span data-ttu-id="a0921-154">toocreate hello konto, lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="a0921-154">toocreate hello account, add this code toohello Main method:</span></span>

```
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFile("..\\..\\CreateVMTemplate.json");

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFile("..\\..\\Parameters.json");
```

## <a name="deploy-hello-template"></a><span data-ttu-id="a0921-155">Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="a0921-155">Deploy hello template</span></span>

<span data-ttu-id="a0921-156">Distribuera hello mallen och parametrar från hello storage-konto som har skapats.</span><span class="sxs-lookup"><span data-stu-id="a0921-156">Deploy hello template and parameters from hello storage account that was created.</span></span> 

<span data-ttu-id="a0921-157">toodeploy hello mall och lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="a0921-157">toodeploy hello template, add this code toohello Main method:</span></span>

```
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter toodelete hello resource group...");
Console.ReadLine();
```

## <a name="delete-hello-resources"></a><span data-ttu-id="a0921-158">Ta bort hello-resurser</span><span class="sxs-lookup"><span data-stu-id="a0921-158">Delete hello resources</span></span>

<span data-ttu-id="a0921-159">Eftersom debiteras du för resurser som används i Azure, men det är alltid bra toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="a0921-159">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="a0921-160">Du behöver inte toodelete varje resurs separat från en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0921-160">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="a0921-161">Ta bort hello resursgruppen och alla dess resurser tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a0921-161">Delete hello resource group and all its resources are automatically deleted.</span></span> 

<span data-ttu-id="a0921-162">toodelete hello resurs och lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="a0921-162">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="a0921-163">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="a0921-163">Run hello application</span></span>

<span data-ttu-id="a0921-164">Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish.</span><span class="sxs-lookup"><span data-stu-id="a0921-164">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="a0921-165">toorun hello-konsolprogram klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="a0921-165">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="a0921-166">Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify hello skapandet av hello resurser i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0921-166">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="a0921-167">Klicka på hello toosee information om Distributionsstatus om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="a0921-167">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0921-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0921-168">Next steps</span></span>
* <span data-ttu-id="a0921-169">Om det fanns problem med hello distribution, nästa steg är toolook på [felsöka vanliga Azure-distribution med Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="a0921-169">If there were issues with hello deployment, a next step would be toolook at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="a0921-170">Lär dig hur toodeploy en virtuell dator och dess stödfiler resurser genom att granska [distribuera ett Azure virtuella datorer med hjälp av C#](csharp.md).</span><span class="sxs-lookup"><span data-stu-id="a0921-170">Learn how toodeploy a virtual machine and its supporting resources by reviewing [Deploy an Azure Virtual Machine Using C#](csharp.md).</span></span>
