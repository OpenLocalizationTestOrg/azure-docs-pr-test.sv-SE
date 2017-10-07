---
title: "aaaCreate data pipelines med hjälp av Azure .NET SDK | Microsoft Docs"
description: "Lär dig hur tooprogrammatically skapa, övervaka och hantera Azure datafabriker med hjälp av Data Factory SDK."
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
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="78337-103">Skapa, övervaka och hantera Azure datafabriker med Azure Data Factory .NET SDK</span><span class="sxs-lookup"><span data-stu-id="78337-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="78337-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="78337-104">Overview</span></span>
<span data-ttu-id="78337-105">Du kan skapa, övervaka och hantera Azure datafabriker genom programmering med Data Factory .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="78337-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="78337-106">Den här artikeln innehåller en genomgång som du kan följa toocreate ett exempelkonsol .NET-program som skapar och övervakar en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="78337-106">This article contains a walkthrough that you can follow toocreate a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="78337-107">Den här artikeln täcker inte alla hello Data Factory .NET API: et.</span><span class="sxs-lookup"><span data-stu-id="78337-107">This article does not cover all hello Data Factory .NET API.</span></span> <span data-ttu-id="78337-108">Se [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) omfattande dokumentation för .NET-API för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78337-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="78337-109">Krav</span><span class="sxs-lookup"><span data-stu-id="78337-109">Prerequisites</span></span>
* <span data-ttu-id="78337-110">Visual Studio 2012, 2013 eller 2015</span><span class="sxs-lookup"><span data-stu-id="78337-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="78337-111">Hämta och installera [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="78337-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="78337-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="78337-112">Azure PowerShell.</span></span> <span data-ttu-id="78337-113">Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="78337-113">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="78337-114">Du kan använda Azure PowerShell toocreate ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="78337-114">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="78337-115">Skapa ett program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="78337-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="78337-116">Skapa ett Azure Active Directory-program, skapa ett huvudnamn för tjänsten för hello program och tilldela den toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="78337-116">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="78337-117">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="78337-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="78337-118">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="78337-118">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="78337-119">Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="78337-119">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="78337-120">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="78337-120">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="78337-121">Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="78337-121">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="78337-122">Notera **SubscriptionId** och **TenantId** från hello utdata från kommandot.</span><span class="sxs-lookup"><span data-stu-id="78337-122">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="78337-123">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="78337-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="78337-124">Om hello resursgruppen finns redan, anger du om tooupdate den (Y) eller låta (n).</span><span class="sxs-lookup"><span data-stu-id="78337-124">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="78337-125">Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="78337-125">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="78337-126">Skapa ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="78337-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="78337-127">Om du får följande fel hello kan ange en annan URL och kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="78337-127">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="78337-128">Skapa hello AD tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="78337-128">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="78337-129">Lägga till tjänstens huvudnamn toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="78337-129">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="78337-130">Hämta hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="78337-130">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="78337-131">Notera hello program-ID (applicationID) från hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="78337-131">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="78337-132">Du bör nu ha tillgång till följande fyra värden efter de här stegen:</span><span class="sxs-lookup"><span data-stu-id="78337-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="78337-133">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="78337-133">Tenant ID</span></span>
* <span data-ttu-id="78337-134">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="78337-134">Subscription ID</span></span>
* <span data-ttu-id="78337-135">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="78337-135">Application ID</span></span>
* <span data-ttu-id="78337-136">Lösenord (anges i hello första kommandot)</span><span class="sxs-lookup"><span data-stu-id="78337-136">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="78337-137">Genomgång</span><span class="sxs-lookup"><span data-stu-id="78337-137">Walkthrough</span></span>
<span data-ttu-id="78337-138">I hello genomgång skapar du en datafabrik med en rörledning som innehåller en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="78337-138">In hello walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="78337-139">Hej kopieringsaktiviteten kopierar data från en mapp i Azure blob storage tooanother-mappen i hello samma blob storage.</span><span class="sxs-lookup"><span data-stu-id="78337-139">hello copy activity copies data from a folder in your Azure blob storage tooanother folder in hello same blob storage.</span></span> 

<span data-ttu-id="78337-140">Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78337-140">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="78337-141">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="78337-141">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="78337-142">Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="78337-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

1. <span data-ttu-id="78337-143">Skapa ett C# .NET-konsolprogram med hjälp av Visual Studio 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="78337-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="78337-144">Starta **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="78337-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="78337-145">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="78337-145">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="78337-146">Expandera **Mallar** och välj **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="78337-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="78337-147">I den här genomgången ska du använda C#, men du kan använda valfritt .NET-språk.</span><span class="sxs-lookup"><span data-stu-id="78337-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="78337-148">Välj **konsolprogram** hello listan över projekttyper på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="78337-148">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="78337-149">Ange **DataFactoryAPITestApp** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="78337-149">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="78337-150">Välj **C:\ADFGetStarted** för hello plats.</span><span class="sxs-lookup"><span data-stu-id="78337-150">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="78337-151">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="78337-151">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="78337-152">Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="78337-152">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="78337-153">I hello **Pakethanterarkonsolen**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="78337-153">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="78337-154">Kör hello efter kommandot tooinstall Data Factory paketet:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="78337-154">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="78337-155">Kör hello efter kommandot tooinstall Azure Active Directory-paket (använder Active Directory API hello kod):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="78337-155">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="78337-156">Ersätt hello innehållet i **App.config** fil i hello-projekt med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="78337-156">Replace hello contents of **App.config** file in hello project with hello following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="78337-157">Uppdatera värden för i hello App.Config-filen  **&lt;program-ID&gt;**,  **&lt;lösenord&gt;**,  **&lt; Prenumerations-ID&gt;**, och  **&lt;klient-ID&gt;**  med egna värden.</span><span class="sxs-lookup"><span data-stu-id="78337-157">In hello App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="78337-158">Lägg till följande hello **med** instruktioner toohello **Program.cs** fil i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="78337-158">Add hello following **using** statements toohello **Program.cs** file in hello project.</span></span>

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
6. <span data-ttu-id="78337-159">Lägg till följande kod som skapar en instans av hello **DataPipelineManagementClient** klassen toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-159">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="78337-160">Du kan använda det här objektet toocreate en datafabrik, en länkad tjänst, inkommande och utgående datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="78337-160">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="78337-161">Du kan också använda det här objektet toomonitor segment i en dataset vid körning.</span><span class="sxs-lookup"><span data-stu-id="78337-161">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="78337-162">Ersätt hello värdet för **resourceGroupName** med hello namnet på ditt Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="78337-162">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span> <span data-ttu-id="78337-163">Du kan skapa en resursgrupp med hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="78337-163">You can create a resource group using hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="78337-164">Uppdatera hello data factory (dataFactoryName) toobe unika namn.</span><span class="sxs-lookup"><span data-stu-id="78337-164">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="78337-165">Namnet på hello data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="78337-165">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="78337-166">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="78337-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="78337-167">Lägg till följande kod som skapar hello en **datafabriken** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-167">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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
8. <span data-ttu-id="78337-168">Lägg till följande kod som skapar hello en **Azure länkade lagringstjänsten** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-168">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="78337-169">Ersätt **storageaccountname** och **accountkey** med namnet och nyckeln för ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="78337-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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
9. <span data-ttu-id="78337-170">Lägg till följande kod som skapar hello **inkommande och utgående datauppsättningar** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-170">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    <span data-ttu-id="78337-171">Hej **FolderPath** för inkommande hello-blob har angetts för**adftutorial /** där **adftutorial** är hello namnet på behållaren hello i blob storage.</span><span class="sxs-lookup"><span data-stu-id="78337-171">hello **FolderPath** for hello input blob is set too**adftutorial/** where **adftutorial** is hello name of hello container in your blob storage.</span></span> <span data-ttu-id="78337-172">Om den här behållaren inte finns i din Azure blob storage, skapa en behållare med namnet: **adftutorial** och ladda upp toohello textbehållaren.</span><span class="sxs-lookup"><span data-stu-id="78337-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file toohello container.</span></span>

    <span data-ttu-id="78337-173">hello FolderPath för hello utdata blob har angetts till: **adftutorial/apifactoryoutput / {segmentera}** där **sektorn** är dynamiskt beräknat utifrån hello värdet för **SliceStart**(starta datum och tid för varje segment.)</span><span class="sxs-lookup"><span data-stu-id="78337-173">hello FolderPath for hello output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on hello value of **SliceStart** (start date-time of each slice.)</span></span>

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
10. <span data-ttu-id="78337-174">Lägg till hello följande kod som **skapar och aktiverar en pipeline** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-174">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="78337-175">Den här pipelinen har en **CopyActivity** som använder **BlobSource** som källa och **BlobSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="78337-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="78337-176">Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78337-176">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="78337-177">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="78337-177">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="78337-178">Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="78337-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

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
    
                // Initial value for pipeline's active period. With this, you won't need tooset slice status
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
12. <span data-ttu-id="78337-179">Lägg till följande kod toohello hello **Main** metoden tooget hello status för en datasektorn av hello utdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="78337-179">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="78337-180">Det finns endast en sektor som förväntas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="78337-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");
        // wait before hello next status check
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
13. <span data-ttu-id="78337-181">**(valfritt)**  Lägg till hello följande kod tooget kör information för en data-segmentet toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="78337-181">**(optional)** Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
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
    
    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="78337-182">Lägg till följande helper-metod som används av hello hello **Main** metoden toohello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="78337-182">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span> <span data-ttu-id="78337-183">Den här metoden visas en dialogruta som låter dig ange **användarnamn** och **lösenord** att du använder toolog i tooAzure portal.</span><span class="sxs-lookup"><span data-stu-id="78337-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use toolog in tooAzure portal.</span></span>

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

        throw new InvalidOperationException("Failed tooacquire token");
    }
    ```

15. <span data-ttu-id="78337-184">Hello Solution Explorer, expandera hello projekt: **DataFactoryAPITestApp**, högerklicka på **referenser**, och klicka på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="78337-184">In hello Solution Explorer, expand hello project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="78337-185">Markera kryssrutan för `System.Configuration` sammansättningen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="78337-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="78337-186">Skapa hello konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="78337-186">Build hello console application.</span></span> <span data-ttu-id="78337-187">Klicka på **skapa** på hello-menyn och klicka på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="78337-187">Click **Build** on hello menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="78337-188">Bekräfta att det finns minst en fil i hello adftutorial behållare i Azure blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="78337-188">Confirm that there is at least one file in hello adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="78337-189">Om inte, skapa Emp.txt fil i anteckningar med hello efter innehåll och överför den toohello adftutorial behållare.</span><span class="sxs-lookup"><span data-stu-id="78337-189">If not, create Emp.txt file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="78337-190">Kör hello exempel genom att klicka på **felsöka** -> **Start Debugging** på hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="78337-190">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="78337-191">När du ser hello **komma kör information om en datasektorn**, Vänta några minuter och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="78337-191">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="78337-192">Använd hello Azure portal tooverify hello data factory **APITutorialFactory** skapas med följande artefakter hello:</span><span class="sxs-lookup"><span data-stu-id="78337-192">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
    * <span data-ttu-id="78337-193">Länkade tjänsten: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="78337-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="78337-194">Datauppsättning: **DatasetBlobSource** och **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="78337-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="78337-195">Pipeline: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="78337-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="78337-196">Kontrollera att en utdatafil skapas i hello **apifactoryoutput** mapp i hello **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="78337-196">Verify that an output file is created in hello **apifactoryoutput** folder in hello **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="78337-197">Hämta en lista över misslyckade datasektorer</span><span class="sxs-lookup"><span data-stu-id="78337-197">Get a list of failed data slices</span></span> 

```csharp
// Parse hello resource path
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

## <a name="next-steps"></a><span data-ttu-id="78337-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78337-198">Next steps</span></span>
<span data-ttu-id="78337-199">Se följande exempel för att skapa en pipeline med .NET SDK som kopierar data från en Azure blob storage tooan Azure SQL database hello:</span><span class="sxs-lookup"><span data-stu-id="78337-199">See hello following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage tooan Azure SQL database:</span></span> 

- [<span data-ttu-id="78337-200">Skapa en pipeline toocopy data från Blob Storage tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="78337-200">Create a pipeline toocopy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
