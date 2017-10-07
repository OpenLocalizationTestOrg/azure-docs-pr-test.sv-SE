---
title: "aaaManagement .NET SDK v1.x för Azure Stream Analytics | Microsoft Docs"
description: "Kom igång med Stream Analytics Management .NET SDK. Lär dig hur tooset upp och kör analytics-jobb. Skapa ett projekt, indata, utdata och transformationer."
keywords: .NET SDK, analytics API
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: jeffstok
ms.openlocfilehash: d205c388880e3d9c2ca5df218f4b68abac8c9780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="management-net-sdk-v1x-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a><span data-ttu-id="fa987-106">Hantering av .NET SDK v1.x: konfigurera och kör analytics-jobb med hjälp av hello Azure Stream Analytics API för .NET</span><span class="sxs-lookup"><span data-stu-id="fa987-106">Management .NET SDK v1.x: Set up and run analytics jobs using hello Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="fa987-107">Lär dig hur tooset in och kör analytics-jobb med hello Stream Analytics-API för att använda .NET hello Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="fa987-107">Learn how tooset up and run analytics jobs using hello Stream Analytics API for .NET using hello Management .NET SDK.</span></span> <span data-ttu-id="fa987-108">Ställ in ett projekt, skapa inkommande och utgående källor, omvandlingar och starta och stoppa jobb.</span><span class="sxs-lookup"><span data-stu-id="fa987-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="fa987-109">För analytics-jobb kan du strömma data från Blob-lagring eller från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="fa987-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="fa987-110">Se hello [management i referensdokumentationen för hello Stream Analytics-API för .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa987-110">See hello [management reference documentation for hello Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="fa987-111">Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet, skalbarhet, komplexa händelsebearbetning över strömmande data i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="fa987-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="fa987-112">Stream Analytics gör att kunder tooset in strömning jobb tooanalyze dataströmmar och låter dem toodrive nära analys i realtid.</span><span class="sxs-lookup"><span data-stu-id="fa987-112">Stream Analytics enables customers tooset up streaming jobs tooanalyze data streams, and allows them toodrive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="fa987-113">Exempelkoden i den här artikeln använder fortfarande äldre (1.x) versionen av Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="fa987-113">Sample code in this article still uses legacy (1.x) version of Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="fa987-114">Exempelkod med hello uppdaterade SDK-version, se [Använd hello Management .NET SDK för Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).</span><span class="sxs-lookup"><span data-stu-id="fa987-114">For sample code using hello up-to-date SDK version, please see [Use hello Management .NET SDK for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa987-115">Krav</span><span class="sxs-lookup"><span data-stu-id="fa987-115">Prerequisites</span></span>
<span data-ttu-id="fa987-116">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="fa987-116">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="fa987-117">Installera Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="fa987-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="fa987-118">Hämta och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="fa987-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="fa987-119">Skapa en Azure-resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fa987-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="fa987-120">hello följande är ett exempel Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="fa987-120">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="fa987-121">Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="fa987-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="fa987-122">Ställ in ingen källa och mål toouse utdata.</span><span class="sxs-lookup"><span data-stu-id="fa987-122">Set up an input source and output target toouse.</span></span> <span data-ttu-id="fa987-123">Mer information finns i [lägga till indata](stream-analytics-add-inputs.md) tooset upp en Exempelindata och [lägga till utdata](stream-analytics-add-outputs.md) tooset upp ett exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="fa987-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) tooset up a sample input and [Add Outputs](stream-analytics-add-outputs.md) tooset up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="fa987-124">Ställ in ett projekt</span><span class="sxs-lookup"><span data-stu-id="fa987-124">Set up a project</span></span>
<span data-ttu-id="fa987-125">toocreate analytics-jobbet använda hello Stream Analytics-API för .NET, först ställa in projektet.</span><span class="sxs-lookup"><span data-stu-id="fa987-125">toocreate an analytics job use hello Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="fa987-126">Skapa ett konsolprogram i Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="fa987-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="fa987-127">Kör hello följande kommandon i hello Package Manager-konsolen, tooinstall hello NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="fa987-127">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="fa987-128">hello först är en hello Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="fa987-128">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="fa987-129">hello är andra hello Azure Active Directory-klient som ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="fa987-129">hello second one is hello Azure Active Directory client that will be used for authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
3. <span data-ttu-id="fa987-130">Lägg till följande hello **appSettings** avsnittet toohello App.config-fil:</span><span class="sxs-lookup"><span data-stu-id="fa987-130">Add hello following **appSettings** section toohello App.config file:</span></span>
   
        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>

    <span data-ttu-id="fa987-131">Ersätt värdena för **SubscriptionId** och **ActiveDirectoryTenantId** med din Azure-prenumeration och klient-ID: N.</span><span class="sxs-lookup"><span data-stu-id="fa987-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="fa987-132">Du kan hämta dessa värden genom att köra hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="fa987-132">You can get these values by running hello following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="fa987-133">Lägg till följande referens i filen .csproj hello:</span><span class="sxs-lookup"><span data-stu-id="fa987-133">Add hello following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

1. <span data-ttu-id="fa987-134">Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projektet:</span><span class="sxs-lookup"><span data-stu-id="fa987-134">Add hello following **using** statements toohello source file (Program.cs) in hello project:</span></span>
   
        using System;
        using System.Configuration;
        using System.Threading.Tasks;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. <span data-ttu-id="fa987-135">Lägg till en helper autentiseringsmetod:</span><span class="sxs-lookup"><span data-stu-id="fa987-135">Add an authentication helper method:</span></span>

   ```   
   private static async Task<string> GetAuthorizationHeader()
   {
       var context = new AuthenticationContext(
           ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
           ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

        AuthenticationResult result = await context.AcquireTokenASync(
           resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
           clientId: ConfigurationManager.AppSettings["AsaClientId"],
           redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
           promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

       throw new InvalidOperationException("Failed tooacquire token");
   }
   ```  

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="fa987-136">Skapa ett Stream Analytics management-klienten</span><span class="sxs-lookup"><span data-stu-id="fa987-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="fa987-137">En **StreamAnalyticsManagementClient** objektet kan du toomanage hello jobbet och hello jobbet komponenter, till exempel indata, utdata och omvandling.</span><span class="sxs-lookup"><span data-stu-id="fa987-137">A **StreamAnalyticsManagementClient** object allows you toomanage hello job and hello job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="fa987-138">Lägg till följande kod toohello början av hello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="fa987-138">Add hello following code toohello beginning of hello **Main** method:</span></span>

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
        ConfigurationManager.AppSettings["SubscriptionId"],
        GetAuthorizationHeader().Result);

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

<span data-ttu-id="fa987-139">Hej **resourceGroupName** variabelns värde ska vara detsamma som namnet på hello hello resurs gruppen du har skapats eller plockats i hello förkraven hello.</span><span class="sxs-lookup"><span data-stu-id="fa987-139">hello **resourceGroupName** variable's value should be hello same as hello name of hello resource group you created or picked in hello prerequisite steps.</span></span>

<span data-ttu-id="fa987-140">tooautomate hello autentiseringsuppgifter presentation aspekt av skapandet finns för[autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="fa987-140">tooautomate hello credential presentation aspect of job creation, refer too[Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="fa987-141">hello återstående avsnitten i den här artikeln förutsätter att den här koden är hello början av hello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="fa987-141">hello remaining sections of this article assume that this code is at hello beginning of hello **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="fa987-142">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="fa987-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="fa987-143">hello skapar följande kod en Stream Analytics-jobbet under hello resursgrupp som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="fa987-143">hello following code creates a Stream Analytics job under hello resource group that you have defined.</span></span> <span data-ttu-id="fa987-144">Du lägger till ett jobb för indata, utdata och omvandling toohello senare.</span><span class="sxs-lookup"><span data-stu-id="fa987-144">You will add an input, output, and transformation toohello job later.</span></span>

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="fa987-145">Skapa en inkommande Stream Analytics-källa</span><span class="sxs-lookup"><span data-stu-id="fa987-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="fa987-146">hello skapar följande kod en Stream Analytics indatakälla hello blob indatakälla typ och CSV-serialisering.</span><span class="sxs-lookup"><span data-stu-id="fa987-146">hello following code creates a Stream Analytics input source with hello blob input source type and CSV serialization.</span></span> <span data-ttu-id="fa987-147">toocreate en hub inkommande händelsekälla, Använd **EventHubStreamInputDataSource** i stället för **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="fa987-147">toocreate an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="fa987-148">På liknande sätt kan du anpassa hello serialiseringstyp av indatakälla hello.</span><span class="sxs-lookup"><span data-stu-id="fa987-148">Similarly, you can customize hello serialization type of hello input source.</span></span>

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

<span data-ttu-id="fa987-149">Indatakällor, är från Blob-lagring eller en händelsehubb bundet tooa specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="fa987-149">Input sources, whether from Blob storage or an event hub, are tied tooa specific job.</span></span> <span data-ttu-id="fa987-150">toouse Hej samma källa för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="fa987-150">toouse hello same input source for different jobs, you must call hello method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="fa987-151">Testa en Stream Analytics indatakälla</span><span class="sxs-lookup"><span data-stu-id="fa987-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="fa987-152">Hej **TestConnection** metoden tester om hello Stream Analytics-jobbet är kan tooconnect toohello Ange källa samt andra aspekter specifika toohello ange typen av datakälla.</span><span class="sxs-lookup"><span data-stu-id="fa987-152">hello **TestConnection** method tests whether hello Stream Analytics job is able tooconnect toohello input source as well as other aspects specific toohello input source type.</span></span> <span data-ttu-id="fa987-153">Till exempel i hello blob indatakälla du skapade tidigare, kontrollerar hello metoden att hello lagringskontonamn och en nyckel kan vara används tooconnect toohello lagringskonto samt kontrollera den angivna hello-behållaren finns.</span><span class="sxs-lookup"><span data-stu-id="fa987-153">For example, in hello blob input source you created in an earlier step, hello method will check that hello Storage account name and key pair can be used tooconnect toohello Storage account as well as check that hello specified container exists.</span></span>

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="fa987-154">Skapa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="fa987-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="fa987-155">Att skapa ett mål för utdata är mycket lik toocreating en Stream Analytics-Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="fa987-155">Creating an output target is very similar toocreating a Stream Analytics input source.</span></span> <span data-ttu-id="fa987-156">Precis som indatakällor är utdata mål bundet tooa specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="fa987-156">Like input sources, output targets are tied tooa specific job.</span></span> <span data-ttu-id="fa987-157">toouse Hej samma utdata mål för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="fa987-157">toouse hello same output target for different jobs, you must call hello method again and specify a different job name.</span></span>

<span data-ttu-id="fa987-158">hello följande kod skapar ett utgående mål (Azure SQL database).</span><span class="sxs-lookup"><span data-stu-id="fa987-158">hello following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="fa987-159">Du kan anpassa hello utdata mål datatyp och/eller serialiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="fa987-159">You can customize hello output target's data type and/or serialization type.</span></span>

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="fa987-160">Testa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="fa987-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="fa987-161">Ett Stream Analytics utdata mål har också hello **TestConnection** metod för att testa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="fa987-161">A Stream Analytics output target also has hello **TestConnection** method for testing connections.</span></span>

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="fa987-162">Skapa Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fa987-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="fa987-163">hello följande kod skapar en Stream Analytics-omvandling med hello frågan ”Välj * från indata” och anger tooallocate en enhet för strömning hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="fa987-163">hello following code creates a Stream Analytics transformation with hello query "select * from Input" and specifies tooallocate one streaming unit for hello Stream Analytics job.</span></span> <span data-ttu-id="fa987-164">Läs mer om hur du justerar strömningsenheter [skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="fa987-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

<span data-ttu-id="fa987-165">En omvandling är också bundet toohello specifika Stream Analytics-jobbet den skapades under som indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="fa987-165">Like input and output, a transformation is also tied toohello specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="fa987-166">Starta ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="fa987-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="fa987-167">När du har skapat ett Stream Analytics-jobb och dess input(s), utdata och omvandling av du kan starta hello jobbet genom att anropa hello **starta** metod.</span><span class="sxs-lookup"><span data-stu-id="fa987-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start hello job by calling hello **Start** method.</span></span>

<span data-ttu-id="fa987-168">hello följande exempelkod startar en Stream Analytics-jobbet med en anpassad utdata start tid set tooDecember 12, 2012, 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="fa987-168">hello following sample code starts a Stream Analytics job with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC:</span></span>

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="fa987-169">Stoppa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="fa987-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="fa987-170">Du kan stoppa ett Stream Analytics-jobb som körs med anropa hello **stoppa** metod.</span><span class="sxs-lookup"><span data-stu-id="fa987-170">You can stop a running Stream Analytics job by calling hello **Stop** method.</span></span>

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="fa987-171">Ta bort ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="fa987-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="fa987-172">Hej **ta bort** metoden tar bort hello projekt samt hello underliggande underordnade resurser, inklusive input(s), utdata och omvandling av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="fa987-172">hello **Delete** method will delete hello job as well as hello underlying sub-resources, including input(s), output(s), and transformation of hello job.</span></span>

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);

## <a name="get-support"></a><span data-ttu-id="fa987-173">Få support</span><span class="sxs-lookup"><span data-stu-id="fa987-173">Get support</span></span>
<span data-ttu-id="fa987-174">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="fa987-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa987-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa987-175">Next steps</span></span>
<span data-ttu-id="fa987-176">Du har lärt dig hello grunderna i en .NET SDK-toocreate och kör analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="fa987-176">You've learned hello basics of using a .NET SDK toocreate and run analytics jobs.</span></span> <span data-ttu-id="fa987-177">toolearn finns fler hello följande:</span><span class="sxs-lookup"><span data-stu-id="fa987-177">toolearn more, see hello following:</span></span>

* [<span data-ttu-id="fa987-178">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fa987-178">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="fa987-179">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fa987-179">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="fa987-180">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="fa987-180">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* <span data-ttu-id="fa987-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa987-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* [<span data-ttu-id="fa987-182">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="fa987-182">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="fa987-183">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="fa987-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
