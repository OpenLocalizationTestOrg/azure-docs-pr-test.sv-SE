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
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Distribuera en Azure-dator med hjälp av C# och Resource Manager-mall
Den här artikeln beskrivs hur du toodeploy en Azure Resource Manager-mallen med C#. hello-mall som du skapar distribuerar en enskild virtuell dator som kör Windows Server i ett nytt virtuellt nätverk med ett enda undernät.

En detaljerad beskrivning av hello virtuell datorresurs finns [virtuella datorer i en Azure Resource Manager-mall](template-description.md). Mer information om alla hello resurser i en mall finns [genomgång av Azure Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Det tar cirka 10 minuter toodo dessa steg.

## <a name="create-a-visual-studio-project"></a>Skapa ett Visual Studio-projekt

I det här steget ska kontrollera du att Visual Studio är installerat och du skapar en konsol program som används för toodeploy hello mall.

1. Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Välj **.NET skrivbord development** på hello arbetsbelastningar sidan och klicka sedan på **installera**. I hello sammanfattning, kan du se att **utvecklingsverktyg för .NET Framework 4 4.6** väljs automatiskt för dig. Om du redan har installerat Visual Studio kan du lägga till hello .NET arbetsbelastning med hello Visual Studio starta.
2. I Visual Studio klickar du på **filen** > **ny** > **projekt**.
3. I **mallar** > **Visual C#**väljer **Konsolapp (.NET Framework)**, ange *myDotnetProject* för hello namn Hej projektet, Välj hello platsen för hello-projektet och klicka sedan på **OK**.

## <a name="install-hello-packages"></a>Installera hello-paket

NuGet-paket är hello enklaste sättet tooinstall hello bibliotek som du behöver toofinish dessa steg. tooget hello bibliotek som du behöver i Visual Studio gör dessa steg:

1. Klicka på **verktyg** > **Nuget Package Manager**, och klicka sedan på **Pakethanterarkonsolen**.
2. Ange följande kommandon i hello-konsolen:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a>Skapa hello-filer

I det här steget skapar du en mallfil som distribuerar hello resurser och en fil med parametrar som tillhandahåller parametern värden toohello mall. Du kan också skapa en auktoriseringsfil som har använt tooperform Azure Resource Manager-åtgärder.

### <a name="create-hello-template-file"></a>Skapa hello mallfil

1. I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*. Namnet hello filen *CreateVMTemplate.json*, och klicka sedan på **Lägg till**.
2. Lägg till den här JSON-kod toohello-fil som du skapade:

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

3. Spara hello CreateVMTemplate.json filen.

### <a name="create-hello-parameters-file"></a>Skapa hello parameterfilen

toospecify värden för hello Resursparametrar som definieras i mallen för hello, skapa en fil med parametrar som innehåller hello värden.

1. I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*. Namnet hello filen *Parameters.json*, och klicka sedan på **Lägg till**.
2. Lägg till den här JSON-kod toohello-fil som du skapade:

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

4. Spara hello Parameters.json filen.

### <a name="create-hello-authorization-file"></a>Skapa hello auktoriseringsfilen

Innan du kan distribuera en mall måste du se till att du har åtkomst tooan [Active Directory-tjänstens huvudnamn](../../resource-group-authenticate-service-principal.md). Från hello tjänstens huvudnamn skaffa du en token för att autentisera begäranden tooAzure Resource Manager. Du bör också anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i hello auktoriseringsfilen.

1. I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*. Namnet hello filen *azureauth.properties*, och klicka sedan på **Lägg till**.
2. Lägg till dessa auktorisering egenskaper:

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

    Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med hello Active Directory-program identifierare,  **&lt;autentiseringsnyckel&gt;**  med hello programmet nyckel och  **&lt;klient-id&gt;**  med hello-klient identifierare.

3. Spara hello azureauth.properties filen.
4. Ange en miljövariabel i Windows som heter AZURE_AUTH_LOCATION med hello fullständig sökväg tooauthorization filen som du skapade, till exempel hello följande PowerShell-kommandot kan användas:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a>Skapa hello management-klienten

1. Öppna hello Program.cs-filen för hello-projekt som du skapade och Lägg sedan till dem med instruktioner toohello befintliga instruktioner överst i filen hello:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. toocreate hello management-klienten, lägger du till den här koden toohello Main-metoden:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

toospecify värden för programmet hello, lägger du till kod toohello Main-metoden:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

hello mall och parametrar distribueras från ett lagringskonto i Azure. I det här steget Skapa hello och ladda upp hello-filer. 

toocreate hello konto, lägga till den här koden toohello Main-metoden:

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

## <a name="deploy-hello-template"></a>Distribuera hello mall

Distribuera hello mallen och parametrar från hello storage-konto som har skapats. 

toodeploy hello mall och lägga till den här koden toohello Main-metoden:

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

## <a name="delete-hello-resources"></a>Ta bort hello-resurser

Eftersom debiteras du för resurser som används i Azure, men det är alltid bra toodelete resurser som inte längre behövs. Du behöver inte toodelete varje resurs separat från en resursgrupp. Ta bort hello resursgruppen och alla dess resurser tas bort automatiskt. 

toodelete hello resurs och lägga till den här koden toohello Main-metoden:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a>Kör programmet hello

Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish. 

1. toorun hello-konsolprogram klickar du på **starta**.

2. Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify hello skapandet av hello resurser i hello Azure-portalen. Klicka på hello toosee information om Distributionsstatus om hello-distribution.

## <a name="next-steps"></a>Nästa steg
* Om det fanns problem med hello distribution, nästa steg är toolook på [felsöka vanliga Azure-distribution med Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
* Lär dig hur toodeploy en virtuell dator och dess stödfiler resurser genom att granska [distribuera ett Azure virtuella datorer med hjälp av C#](csharp.md).
