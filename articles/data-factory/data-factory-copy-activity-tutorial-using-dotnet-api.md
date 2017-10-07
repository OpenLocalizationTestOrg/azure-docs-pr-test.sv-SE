---
title: "Självstudiekurs: Skapa en pipeline med en kopieringsaktivitet med hjälp av .NET-API:et | Microsoft Docs"
description: "I den här självstudiekursen ska du skapa en Azure Data Factory-pipeline med en kopieringsaktivitet med hjälp av .NET-API:et."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a><span data-ttu-id="062ea-103">Självstudiekurs: Skapa en pipeline med en kopieringsaktivitet med hjälp av .NET-API:et</span><span class="sxs-lookup"><span data-stu-id="062ea-103">Tutorial: Create a pipeline with Copy Activity using .NET API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="062ea-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="062ea-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="062ea-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="062ea-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="062ea-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="062ea-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="062ea-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="062ea-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="062ea-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="062ea-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="062ea-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="062ea-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="062ea-110">REST API</span><span class="sxs-lookup"><span data-stu-id="062ea-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="062ea-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="062ea-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="062ea-112">I den här artikeln får du lära dig hur toouse [.NET API](https://portal.azure.com) toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="062ea-112">In this article, you learn how toouse [.NET API](https://portal.azure.com) toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="062ea-113">Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="062ea-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="062ea-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="062ea-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="062ea-115">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="062ea-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="062ea-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="062ea-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="062ea-117">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="062ea-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="062ea-118">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="062ea-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="062ea-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="062ea-120">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="062ea-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="062ea-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="062ea-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="062ea-122">Fullständig dokumentation för .NET-API för Data Factory finns [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="062ea-122">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>
> 
> <span data-ttu-id="062ea-123">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="062ea-123">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="062ea-124">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-124">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="062ea-125">Krav</span><span class="sxs-lookup"><span data-stu-id="062ea-125">Prerequisites</span></span>
* <span data-ttu-id="062ea-126">Gå igenom [kursen översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget en översikt över hello självstudier och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="062ea-126">Go through [Tutorial Overview and Pre-requisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget an overview of hello tutorial and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="062ea-127">Visual Studio 2012, 2013 eller 2015</span><span class="sxs-lookup"><span data-stu-id="062ea-127">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="062ea-128">Ladda ned och installera [Azure .NET SDK](http://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="062ea-128">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/)</span></span>
* <span data-ttu-id="062ea-129">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="062ea-129">Azure PowerShell.</span></span> <span data-ttu-id="062ea-130">Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](../powershell-install-configure.md) artikel tooinstall Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="062ea-130">Follow instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="062ea-131">Du kan använda Azure PowerShell toocreate ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="062ea-131">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="062ea-132">Skapa ett program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="062ea-132">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="062ea-133">Skapa ett Azure Active Directory-program, skapa ett huvudnamn för tjänsten för hello program och tilldela den toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="062ea-133">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="062ea-134">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="062ea-134">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="062ea-135">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="062ea-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="062ea-136">Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="062ea-136">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="062ea-137">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="062ea-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="062ea-138">Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="062ea-138">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="062ea-139">Notera **SubscriptionId** och **TenantId** från hello utdata från kommandot.</span><span class="sxs-lookup"><span data-stu-id="062ea-139">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="062ea-140">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="062ea-140">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="062ea-141">Om hello resursgruppen finns redan, anger du om tooupdate den (Y) eller låta (n).</span><span class="sxs-lookup"><span data-stu-id="062ea-141">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="062ea-142">Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="062ea-142">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="062ea-143">Skapa ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="062ea-143">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="062ea-144">Om du får följande fel hello kan ange en annan URL och kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="062ea-144">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="062ea-145">Skapa hello AD tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="062ea-145">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="062ea-146">Lägga till tjänstens huvudnamn toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="062ea-146">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="062ea-147">Hämta hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="062ea-147">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="062ea-148">Notera hello program-ID (applicationID) från hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="062ea-148">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="062ea-149">Du bör nu ha tillgång till följande fyra värden efter de här stegen:</span><span class="sxs-lookup"><span data-stu-id="062ea-149">You should have following four values from these steps:</span></span>

* <span data-ttu-id="062ea-150">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="062ea-150">Tenant ID</span></span>
* <span data-ttu-id="062ea-151">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="062ea-151">Subscription ID</span></span>
* <span data-ttu-id="062ea-152">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="062ea-152">Application ID</span></span>
* <span data-ttu-id="062ea-153">Lösenord (anges i hello första kommandot)</span><span class="sxs-lookup"><span data-stu-id="062ea-153">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="062ea-154">Genomgång</span><span class="sxs-lookup"><span data-stu-id="062ea-154">Walkthrough</span></span>
1. <span data-ttu-id="062ea-155">Skapa ett C# .NET-konsolprogram med hjälp av Visual Studio 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="062ea-155">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="062ea-156">Starta **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="062ea-156">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="062ea-157">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="062ea-157">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="062ea-158">Expandera **Mallar** och välj **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="062ea-158">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="062ea-159">I den här genomgången ska du använda C#, men du kan använda valfritt .NET-språk.</span><span class="sxs-lookup"><span data-stu-id="062ea-159">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="062ea-160">Välj **konsolprogram** hello listan över projekttyper på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="062ea-160">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="062ea-161">Ange **DataFactoryAPITestApp** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="062ea-161">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="062ea-162">Välj **C:\ADFGetStarted** för hello plats.</span><span class="sxs-lookup"><span data-stu-id="062ea-162">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="062ea-163">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="062ea-163">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="062ea-164">Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="062ea-164">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="062ea-165">I hello **Pakethanterarkonsolen**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="062ea-165">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="062ea-166">Kör hello efter kommandot tooinstall Data Factory paketet:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="062ea-166">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="062ea-167">Kör hello efter kommandot tooinstall Azure Active Directory-paket (använder Active Directory API hello kod):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="062ea-167">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="062ea-168">Lägg till följande hello **appSetttings** avsnittet toohello **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="062ea-168">Add hello following **appSetttings** section toohello **App.config** file.</span></span> <span data-ttu-id="062ea-169">De här inställningarna används av hello hjälpmetod: **GetAuthorizationHeader**.</span><span class="sxs-lookup"><span data-stu-id="062ea-169">These settings are used by hello helper method: **GetAuthorizationHeader**.</span></span>

    <span data-ttu-id="062ea-170">Ersätt värdena för **&lt;Program-ID&gt;**, **&lt;Lösenord&gt;**, **&lt;Prenumerations-ID&gt;** och **&lt;Klient-ID&gt;** med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="062ea-170">Replace values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>

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

5. <span data-ttu-id="062ea-171">Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="062ea-171">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

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

6. <span data-ttu-id="062ea-172">Lägg till följande kod som skapar en instans av hello **DataPipelineManagementClient** klassen toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-172">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="062ea-173">Du kan använda det här objektet toocreate en datafabrik, en länkad tjänst, inkommande och utgående datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="062ea-173">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="062ea-174">Du kan också använda det här objektet toomonitor segment i en dataset vid körning.</span><span class="sxs-lookup"><span data-stu-id="062ea-174">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="062ea-175">Ersätt hello värdet för **resourceGroupName** med hello namnet på ditt Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="062ea-175">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span>
   >
   > <span data-ttu-id="062ea-176">Uppdatera hello data factory (dataFactoryName) toobe unika namn.</span><span class="sxs-lookup"><span data-stu-id="062ea-176">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="062ea-177">Namnet på hello data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="062ea-177">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="062ea-178">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="062ea-178">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>

7. <span data-ttu-id="062ea-179">Lägg till följande kod som skapar hello en **datafabriken** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-179">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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

    <span data-ttu-id="062ea-180">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="062ea-180">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="062ea-181">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="062ea-181">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="062ea-182">Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata.</span><span class="sxs-lookup"><span data-stu-id="062ea-182">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="062ea-183">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="062ea-183">Let's start with creating hello data factory in this step.</span></span>
8. <span data-ttu-id="062ea-184">Lägg till följande kod som skapar hello en **Azure länkade lagringstjänsten** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-184">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="062ea-185">Ersätt **storageaccountname** och **accountkey** med namnet och nyckeln för ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="062ea-185">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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

    <span data-ttu-id="062ea-186">Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="062ea-186">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="062ea-187">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="062ea-187">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="062ea-188">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="062ea-188">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

    <span data-ttu-id="062ea-189">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="062ea-189">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

    <span data-ttu-id="062ea-190">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="062ea-190">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="062ea-191">Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-191">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
9. <span data-ttu-id="062ea-192">Lägg till följande kod som skapar hello en **Azure SQL länkade tjänsten** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-192">Add hello following code that creates an **Azure SQL linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="062ea-193">Ersätt **servername**, **databasename**, **username** och **password** med namnen för Azure SQL-servern, databasen, användaren och lösenordet.</span><span class="sxs-lookup"><span data-stu-id="062ea-193">Replace **servername**, **databasename**, **username**, and **password** with names of your Azure SQL server, database, user, and password.</span></span>

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    <span data-ttu-id="062ea-194">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="062ea-194">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="062ea-195">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="062ea-195">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="062ea-196">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-196">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
10. <span data-ttu-id="062ea-197">Lägg till följande kod som skapar hello **inkommande och utgående datauppsättningar** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-197">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "InputDataset";
    string Dataset_Destination = "OutputDataset";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
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

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
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
    
    <span data-ttu-id="062ea-198">I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="062ea-198">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="062ea-199">I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.</span><span class="sxs-lookup"><span data-stu-id="062ea-199">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

    <span data-ttu-id="062ea-200">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="062ea-200">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="062ea-201">Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="062ea-201">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="062ea-202">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="062ea-202">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="062ea-203">Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="062ea-203">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

    <span data-ttu-id="062ea-204">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="062ea-204">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="062ea-205">Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="062ea-205">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="062ea-206">I kursen får ange du ett värde för hello filnamnet.</span><span class="sxs-lookup"><span data-stu-id="062ea-206">In this tutorial, you specify a value for hello fileName.</span></span>    

    <span data-ttu-id="062ea-207">I det här steget ska du skapa en utdatauppsättning med namnet **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="062ea-207">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="062ea-208">Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="062ea-208">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span>
11. <span data-ttu-id="062ea-209">Lägg till hello följande kod som **skapar och aktiverar en pipeline** toohello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-209">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="062ea-210">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="062ea-210">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

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
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
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
                    }
                }
            }
        });
    ```

    <span data-ttu-id="062ea-211">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="062ea-211">Note hello following points:</span></span>
   
    - <span data-ttu-id="062ea-212">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="062ea-212">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="062ea-213">Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-213">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="062ea-214">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="062ea-214">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="062ea-215">Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="062ea-215">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="062ea-216">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="062ea-216">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="062ea-217">En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="062ea-217">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="062ea-218">toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="062ea-218">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
   
    <span data-ttu-id="062ea-219">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="062ea-219">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="062ea-220">I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="062ea-220">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="062ea-221">hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="062ea-221">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="062ea-222">Därför produceras 24 segment för utdatauppsättningen av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="062ea-222">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span>
12. <span data-ttu-id="062ea-223">Lägg till följande kod toohello hello **Main** metoden tooget hello status för en datasektorn av hello utdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="062ea-223">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="062ea-224">Endast en sektor förväntas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="062ea-224">There is only slice expected in this sample.</span></span>

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

13. <span data-ttu-id="062ea-225">Lägg till följande kod tooget kör information för en data-segmentet toohello hello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="062ea-225">Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

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
            }
        );

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

14. <span data-ttu-id="062ea-226">Lägg till följande helper-metod som används av hello hello **Main** metoden toohello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="062ea-226">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="062ea-227">När du kopiera och klistra in följande kod hello, se till att hello kopierade koden är på samma nivå som hello Main-metoden hello.</span><span class="sxs-lookup"><span data-stu-id="062ea-227">When you copy and paste hello following code, make sure that hello copied code is at hello same level as hello Main method.</span></span>

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

15. <span data-ttu-id="062ea-228">I hello Solution Explorer, expandera hello projekt (DataFactoryAPITestApp), högerklicka på **referenser**, och klicka på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="062ea-228">In hello Solution Explorer, expand hello project (DataFactoryAPITestApp), right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="062ea-229">Markera kryssrutan för **System.Configuration**-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="062ea-229">Select check box for **System.Configuration** assembly.</span></span> <span data-ttu-id="062ea-230">och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="062ea-230">and click **OK**.</span></span>
16. <span data-ttu-id="062ea-231">Skapa hello konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="062ea-231">Build hello console application.</span></span> <span data-ttu-id="062ea-232">Klicka på **skapa** på hello-menyn och klicka på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="062ea-232">Click **Build** on hello menu and click **Build Solution**.</span></span>
17. <span data-ttu-id="062ea-233">Bekräfta att det finns minst en fil i hello **adftutorial** behållare i Azure blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="062ea-233">Confirm that there is at least one file in hello **adftutorial** container in your Azure blob storage.</span></span> <span data-ttu-id="062ea-234">Om inte, skapa **Emp.txt** fil i anteckningar med hello efter innehåll och överföra den toohello adftutorial behållare.</span><span class="sxs-lookup"><span data-stu-id="062ea-234">If not, create **Emp.txt** file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
18. <span data-ttu-id="062ea-235">Kör hello exempel genom att klicka på **felsöka** -> **Start Debugging** på hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="062ea-235">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="062ea-236">När du ser hello **komma kör information om en datasektorn**, Vänta några minuter och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="062ea-236">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
19. <span data-ttu-id="062ea-237">Använd hello Azure portal tooverify hello data factory **APITutorialFactory** skapas med följande artefakter hello:</span><span class="sxs-lookup"><span data-stu-id="062ea-237">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
   * <span data-ttu-id="062ea-238">Länkad tjänst: **LinkedService_AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="062ea-238">Linked service: **LinkedService_AzureStorage**</span></span>
   * <span data-ttu-id="062ea-239">DataSet: **InputDataset** och **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="062ea-239">Dataset: **InputDataset** and **OutputDataset**.</span></span>
   * <span data-ttu-id="062ea-240">Pipeline: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="062ea-240">Pipeline: **PipelineBlobSample**</span></span>
20. <span data-ttu-id="062ea-241">Kontrollera att hello två medarbetarposter skapas i hello **tomma** tabellen i hello angetts Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="062ea-241">Verify that hello two employee records are created in hello **emp** table in hello specified Azure SQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="062ea-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="062ea-242">Next steps</span></span>
<span data-ttu-id="062ea-243">Fullständig dokumentation för .NET-API för Data Factory finns [Data Factory .NET API-referens](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="062ea-243">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>

<span data-ttu-id="062ea-244">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="062ea-244">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="062ea-245">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="062ea-245">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="062ea-246">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="062ea-246">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>

