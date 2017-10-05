---
title: "Skapa data pipelines med hjälp av Azure .NET SDK | Microsoft Docs"
description: "Lär dig mer om att skapa, övervaka och hantera Azure datafabriker med hjälp av Data Factory SDK."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9d9dac75321c5d4e079f49320d9b7c6f56e48754
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="be7ea-103">Skapa, övervaka och hantera Azure datafabriker med Azure Data Factory .NET SDK</span><span class="sxs-lookup"><span data-stu-id="be7ea-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="be7ea-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="be7ea-104">Overview</span></span>
<span data-ttu-id="be7ea-105">Du kan skapa, övervaka och hantera Azure datafabriker genom programmering med Data Factory .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="be7ea-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="be7ea-106">Den här artikeln innehåller en genomgång som du kan följa om du vill skapa ett exempelkonsol .NET-program som skapar och övervakar en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="be7ea-106">This article contains a walkthrough that you can follow to create a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="be7ea-107">Den här artikeln beskriver inte hela .NET-API:et för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="be7ea-107">This article does not cover all the Data Factory .NET API.</span></span> <span data-ttu-id="be7ea-108">Se [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) omfattande dokumentation för .NET-API för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="be7ea-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="be7ea-109">Krav</span><span class="sxs-lookup"><span data-stu-id="be7ea-109">Prerequisites</span></span>
* <span data-ttu-id="be7ea-110">Visual Studio 2012, 2013 eller 2015</span><span class="sxs-lookup"><span data-stu-id="be7ea-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="be7ea-111">Hämta och installera [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="be7ea-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="be7ea-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be7ea-112">Azure PowerShell.</span></span> <span data-ttu-id="be7ea-113">Följ instruktionerna i artikeln [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) för att installera Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="be7ea-113">Follow instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) article to install Azure PowerShell on your computer.</span></span> <span data-ttu-id="be7ea-114">Du kan använda Azure PowerShell för att skapa ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="be7ea-114">You use Azure PowerShell to create an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="be7ea-115">Skapa ett program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be7ea-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="be7ea-116">Skapa ett Azure Active Directory-program, skapa ett tjänstobjektnamn för programmet och tilldela det rollen som **Data Factory-deltagare**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-116">Create an Azure Active Directory application, create a service principal for the application, and assign it to the **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="be7ea-117">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="be7ea-118">Kör följande kommando och ange användarnamnet och lösenordet som du använder för att logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-118">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="be7ea-119">Kör följande kommando för att visa alla prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="be7ea-119">Run the following command to view all the subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="be7ea-120">Kör följande kommando för att välja den prenumeration som du vill arbeta med.</span><span class="sxs-lookup"><span data-stu-id="be7ea-120">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="be7ea-121">Ersätt **&lt;NameOfAzureSubscription**&gt; med namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="be7ea-121">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="be7ea-122">Anteckna **ID** och **TenantId** i kommandots utdata.</span><span class="sxs-lookup"><span data-stu-id="be7ea-122">Note down **SubscriptionId** and **TenantId** from the output of this command.</span></span>

5. <span data-ttu-id="be7ea-123">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be7ea-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command in the PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="be7ea-124">Om resursgruppen redan finns anger du om du vill uppdatera den (Y) eller lämna den som den är (N).</span><span class="sxs-lookup"><span data-stu-id="be7ea-124">If the resource group already exists, you specify whether to update it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="be7ea-125">Om du använder en annan resursgrupp måste du använda namnet på din resursgrupp i stället för ADFTutorialResourceGroup i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-125">If you use a different resource group, you need to use the name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="be7ea-126">Skapa ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="be7ea-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="be7ea-127">Om följande fel returneras anger du en annan URL och kör kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-127">If you get the following error, specify a different URL and run the command again.</span></span>
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="be7ea-128">Skapa AD-tjänstobjektet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-128">Create the AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="be7ea-129">Lägg till tjänstobjektet till rollen som **Data Factory-deltagare**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-129">Add service principal to the **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="be7ea-130">Hämta program-ID:t.</span><span class="sxs-lookup"><span data-stu-id="be7ea-130">Get the application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="be7ea-131">Anteckna program-ID:t (applicationID) från utdata.</span><span class="sxs-lookup"><span data-stu-id="be7ea-131">Note down the application ID (applicationID) from the output.</span></span>

<span data-ttu-id="be7ea-132">Du bör nu ha tillgång till följande fyra värden efter de här stegen:</span><span class="sxs-lookup"><span data-stu-id="be7ea-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="be7ea-133">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="be7ea-133">Tenant ID</span></span>
* <span data-ttu-id="be7ea-134">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="be7ea-134">Subscription ID</span></span>
* <span data-ttu-id="be7ea-135">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="be7ea-135">Application ID</span></span>
* <span data-ttu-id="be7ea-136">Lösenord (anges i det första kommandot)</span><span class="sxs-lookup"><span data-stu-id="be7ea-136">Password (specified in the first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="be7ea-137">Genomgång</span><span class="sxs-lookup"><span data-stu-id="be7ea-137">Walkthrough</span></span>
<span data-ttu-id="be7ea-138">I den här genomgången skapa en datafabrik med en rörledning som innehåller en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-138">In the walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="be7ea-139">Kopieringsaktiviteten kopierar data från en mapp i din Azure blob storage till en annan mapp i samma blob storage.</span><span class="sxs-lookup"><span data-stu-id="be7ea-139">The copy activity copies data from a folder in your Azure blob storage to another folder in the same blob storage.</span></span> 

<span data-ttu-id="be7ea-140">Kopieringsaktiviteten utför dataflyttningen i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="be7ea-140">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="be7ea-141">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="be7ea-141">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="be7ea-142">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="be7ea-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

1. <span data-ttu-id="be7ea-143">Skapa ett C# .NET-konsolprogram med hjälp av Visual Studio 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="be7ea-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="be7ea-144">Starta **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="be7ea-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="be7ea-145">Klicka på **Arkiv**, peka på **Nytt** och klicka på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-145">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="be7ea-146">Expandera **Mallar** och välj **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="be7ea-147">I den här genomgången ska du använda C#, men du kan använda valfritt .NET-språk.</span><span class="sxs-lookup"><span data-stu-id="be7ea-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="be7ea-148">Välj **Konsolprogram** i listan över projekttyper till höger.</span><span class="sxs-lookup"><span data-stu-id="be7ea-148">Select **Console Application** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="be7ea-149">Ange **DataFactoryAPITestApp** som namn.</span><span class="sxs-lookup"><span data-stu-id="be7ea-149">Enter **DataFactoryAPITestApp** for the Name.</span></span>
   6. <span data-ttu-id="be7ea-150">Välj **C:\ADFGetStarted** som plats.</span><span class="sxs-lookup"><span data-stu-id="be7ea-150">Select **C:\ADFGetStarted** for the Location.</span></span>
   7. <span data-ttu-id="be7ea-151">Klicka på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-151">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="be7ea-152">Klicka på **Verktyg**, peka på **NuGet Package Manager** och klicka på **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-152">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="be7ea-153">I **Package Manager Console** gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="be7ea-153">In the **Package Manager Console**, do the following steps:</span></span>
   1. <span data-ttu-id="be7ea-154">Kör följande kommando för att installera Data Factory-paketet: `Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="be7ea-154">Run the following command to install Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="be7ea-155">Kör följande kommando för att installera Azure Active Directory-paketet (du använder Active Directory-API i koden): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="be7ea-155">Run the following command to install Azure Active Directory package (you use Active Directory API in the code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="be7ea-156">Ersätt innehållet i **App.config** filen i projektet med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="be7ea-156">Replace the contents of **App.config** file in the project with the following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="be7ea-157">I filen App.Config uppdaterar du värden för  **&lt;program-ID&gt;**,  **&lt;lösenord&gt;**,  **&lt;prenumerations-ID&gt;**, och  **&lt;klient-ID&gt;**  med egna värden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-157">In the App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="be7ea-158">Lägg till följande **med** instruktioner till den **Program.cs** filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-158">Add the following **using** statements to the **Program.cs** file in the project.</span></span>

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. <span data-ttu-id="be7ea-159">Lägg till följande kod som skapar en instans av **DataPipelineManagementClient**-klassen till **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-159">Add the following code that creates an instance of **DataPipelineManagementClient** class to the **Main** method.</span></span> <span data-ttu-id="be7ea-160">Du kan använda det här objektet för att skapa en datafabrik, en länkad tjänst, in- och utdatauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="be7ea-160">You use this object to create a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="be7ea-161">Du kan också använda det här objektet om du vill övervaka sektorer för en datauppsättning vid körningstillfället.</span><span class="sxs-lookup"><span data-stu-id="be7ea-161">You also use this object to monitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify the name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: the name of the data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="be7ea-162">Ersätt värdet för **resourceGroupName** med namnet på Azure-resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-162">Replace the value of **resourceGroupName** with the name of your Azure resource group.</span></span> <span data-ttu-id="be7ea-163">Du kan skapa en resurs med det [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-163">You can create a resource group using the [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="be7ea-164">Uppdatera namnet på datafabriken (dataFactoryName) så att det är unikt.</span><span class="sxs-lookup"><span data-stu-id="be7ea-164">Update name of the data factory (dataFactoryName) to be unique.</span></span> <span data-ttu-id="be7ea-165">Namnet på datafabriken måste vara unikt globalt.</span><span class="sxs-lookup"><span data-stu-id="be7ea-165">Name of the data factory must be globally unique.</span></span> <span data-ttu-id="be7ea-166">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="be7ea-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="be7ea-167">Lägg till följande kod som skapar en **datafabrik** till **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-167">Add the following code that creates a **data factory** to the **Main** method.</span></span>

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```
8. <span data-ttu-id="be7ea-168">Lägg till följande kod som skapar en **länkad Azure Storage-tjänst** till **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-168">Add the following code that creates an **Azure Storage linked service** to the **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="be7ea-169">Ersätt **storageaccountname** och **accountkey** med namnet och nyckeln för ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="be7ea-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```
9. <span data-ttu-id="be7ea-170">Lägg till följande kod som skapar **in- och utdatauppsättningar** till **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-170">Add the following code that creates **input and output datasets** to the **Main** method.</span></span>

    <span data-ttu-id="be7ea-171">Den **FolderPath** för inkommande blob har värdet **adftutorial /** där **adftutorial** är namnet på behållaren i blob storage.</span><span class="sxs-lookup"><span data-stu-id="be7ea-171">The **FolderPath** for the input blob is set to **adftutorial/** where **adftutorial** is the name of the container in your blob storage.</span></span> <span data-ttu-id="be7ea-172">Om den här behållaren inte finns i din Azure blob storage, skapa en behållare med namnet: **adftutorial** och ladda upp en textfil till behållaren.</span><span class="sxs-lookup"><span data-stu-id="be7ea-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file to the container.</span></span>

    <span data-ttu-id="be7ea-173">Mappsökväg för utdata-blob har angetts till: **adftutorial/apifactoryoutput / {segmentera}** där **sektorn** beräknas dynamiskt baserat på värdet för **SliceStart** (starta datum och tid för varje segment.)</span><span class="sxs-lookup"><span data-stu-id="be7ea-173">The FolderPath for the output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on the value of **SliceStart** (start date-time of each slice.)</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
    
                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
                },
    
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
            }
        }
    });
    ```
10. <span data-ttu-id="be7ea-174">Lägg till följande kod som **skapar och aktiverar en pipeline** till **Main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="be7ea-174">Add the following code that **creates and activates a pipeline** to the **Main** method.</span></span> <span data-ttu-id="be7ea-175">Den här pipelinen har en **CopyActivity** som använder **BlobSource** som källa och **BlobSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="be7ea-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="be7ea-176">Kopieringsaktiviteten utför dataflyttningen i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="be7ea-176">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="be7ea-177">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="be7ea-177">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="be7ea-178">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="be7ea-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new PipelineCreateOrUpdateParameters()
    {
        Pipeline = new Pipeline()
        {
            Name = PipelineName,
            Properties = new PipelineProperties()
            {
                Description = "Demo Pipeline for data transfer between blobs",
    
                // Initial value for pipeline's active period. With this, you won't need to set slice status
                Start = PipelineActivePeriodStartTime,
                End = PipelineActivePeriodEndTime,
    
                Activities = new List<Activity>()
                {
                    new Activity()
                    {
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
                                Name = Dataset_Source
                            }
                        },
                        Outputs = new List<ActivityOutput>()
                        {
                            new ActivityOutput()
                            {
                                Name = Dataset_Destination
                            }
                        },
                        TypeProperties = new CopyActivity()
                        {
                            Source = new BlobSource(),
                            Sink = new BlobSink()
                            {
                                WriteBatchSize = 10000,
                                WriteBatchTimeout = TimeSpan.FromMinutes(10)
                            }
                        }
                    }
    
                },
            }
        }
    });
    ```
12. <span data-ttu-id="be7ea-179">Lägg till följande kod till **Main**-metoden för att hämta statusen för en datasektor i utdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-179">Add the following code to the **Main** method to get the status of a data slice of the output dataset.</span></span> <span data-ttu-id="be7ea-180">Det finns endast en sektor som förväntas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");
        // wait before the next status check
        Thread.Sleep(1000 * 12);
    
        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });
    
        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```
13. <span data-ttu-id="be7ea-181">**(valfritt)**  Lägg till följande kod för att få köra information för en datasektor som den **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="be7ea-181">**(optional)** Add the following code to get run details for a data slice to the **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();
    
    var datasliceRunListResponse = client.DataSliceRuns.List(
        resourceGroupName,
        dataFactoryName,
        Dataset_Destination,
        new DataSliceRunListParameters()
        {
            DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
        });
    
    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }
    
    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="be7ea-182">Lägg till följande helper-metod som används av **Main**-metoden i **Program**-klassen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-182">Add the following helper method used by the **Main** method to the **Program** class.</span></span> <span data-ttu-id="be7ea-183">Den här metoden visas en dialogruta som låter dig ange **användarnamn** och **lösenord** som används för att logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be7ea-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use to log in to Azure portal.</span></span>

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. <span data-ttu-id="be7ea-184">Expandera projektet i Solution Explorer: **DataFactoryAPITestApp**, högerklicka på **referenser**, och klicka på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-184">In the Solution Explorer, expand the project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="be7ea-185">Markera kryssrutan för `System.Configuration` sammansättningen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="be7ea-186">Skapa konsolprogrammet.</span><span class="sxs-lookup"><span data-stu-id="be7ea-186">Build the console application.</span></span> <span data-ttu-id="be7ea-187">Klicka på **Skapa** på menyn och klicka sedan på **Build Solution** (Skapa lösning).</span><span class="sxs-lookup"><span data-stu-id="be7ea-187">Click **Build** on the menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="be7ea-188">Bekräfta att det finns minst en fil i behållaren adftutorial i Azure blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="be7ea-188">Confirm that there is at least one file in the adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="be7ea-189">Om inte, skapa Emp.txt fil i anteckningar med följande innehåll och överför den till behållaren adftutorial.</span><span class="sxs-lookup"><span data-stu-id="be7ea-189">If not, create Emp.txt file in Notepad with the following content and upload it to the adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="be7ea-190">Kör exemplet genom att klicka på **Felsök** -> **Börja felsöka** på menyn.</span><span class="sxs-lookup"><span data-stu-id="be7ea-190">Run the sample by clicking **Debug** -> **Start Debugging** on the menu.</span></span> <span data-ttu-id="be7ea-191">När du ser **Getting run details of a data slice** (Hämta körningsdata för en datorsektor) väntar du några minuter och trycker sedan på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-191">When you see the **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="be7ea-192">Använd Azure-portalen och kontrollera att datafabriken **APITutorialFactory** har skapats med följande artefakter:</span><span class="sxs-lookup"><span data-stu-id="be7ea-192">Use the Azure portal to verify that the data factory **APITutorialFactory** is created with the following artifacts:</span></span>
    * <span data-ttu-id="be7ea-193">Länkade tjänsten: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="be7ea-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="be7ea-194">Datauppsättning: **DatasetBlobSource** och **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="be7ea-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="be7ea-195">Pipeline: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="be7ea-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="be7ea-196">Kontrollera att en utdatafil skapas i den **apifactoryoutput** mapp i den **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="be7ea-196">Verify that an output file is created in the **apifactoryoutput** folder in the **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="be7ea-197">Hämta en lista över misslyckade datasektorer</span><span class="sxs-lookup"><span data-stu-id="be7ea-197">Get a list of failed data slices</span></span> 

```csharp
// Parse the resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a><span data-ttu-id="be7ea-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be7ea-198">Next steps</span></span>
<span data-ttu-id="be7ea-199">Se följande exempel för att skapa en pipeline med .NET SDK som kopierar data från ett Azure blob storage till en Azure SQL database:</span><span class="sxs-lookup"><span data-stu-id="be7ea-199">See the following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage to an Azure SQL database:</span></span> 

- [<span data-ttu-id="be7ea-200">Skapa en pipeline för att kopiera data från Blob Storage till SQL-databas</span><span class="sxs-lookup"><span data-stu-id="be7ea-200">Create a pipeline to copy data from Blob Storage to SQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
