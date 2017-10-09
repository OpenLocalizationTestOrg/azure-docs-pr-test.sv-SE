---
title: "aaaManagement .NET SDK för Azure Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 507c11938bc5bf2249a2e41f6bcc076db8ead3f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a><span data-ttu-id="aea34-106">Hantering av .NET SDK: Konfigurera och köra analytics-jobb med hello Azure Stream Analytics API för .NET</span><span class="sxs-lookup"><span data-stu-id="aea34-106">Management .NET SDK: Set up and run analytics jobs using hello Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="aea34-107">Lär dig hur tooset in och kör analytics-jobb med hello Stream Analytics-API för att använda .NET hello Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="aea34-107">Learn how tooset up and run analytics jobs using hello Stream Analytics API for .NET using hello Management .NET SDK.</span></span> <span data-ttu-id="aea34-108">Ställ in ett projekt, skapa inkommande och utgående källor, omvandlingar och starta och stoppa jobb.</span><span class="sxs-lookup"><span data-stu-id="aea34-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="aea34-109">För analytics-jobb kan du strömma data från Blob-lagring eller från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="aea34-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="aea34-110">Se hello [management i referensdokumentationen för hello Stream Analytics-API för .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="aea34-110">See hello [management reference documentation for hello Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="aea34-111">Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet, skalbarhet, komplexa händelsebearbetning över strömmande data i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="aea34-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="aea34-112">Stream Analytics gör att kunder tooset in strömning jobb tooanalyze dataströmmar och låter dem toodrive nära analys i realtid.</span><span class="sxs-lookup"><span data-stu-id="aea34-112">Stream Analytics enables customers tooset up streaming jobs tooanalyze data streams, and allows them toodrive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="aea34-113">Vi har uppdaterat hello exempelkoden i den här artikeln med Azure Stream Analytics Management .NET SDK v2.x version.</span><span class="sxs-lookup"><span data-stu-id="aea34-113">We have updated hello sample code in this article with Azure Stream Analytics Management .NET SDK v2.x version.</span></span> <span data-ttu-id="aea34-114">För exempelkod med hello använder lagecy (1.x) SDK-version, se [använder hello Management .NET SDK v1.x för Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span><span class="sxs-lookup"><span data-stu-id="aea34-114">For sample code using hello uses lagecy (1.x) SDK version, please see [Use hello Management .NET SDK v1.x for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aea34-115">Krav</span><span class="sxs-lookup"><span data-stu-id="aea34-115">Prerequisites</span></span>
<span data-ttu-id="aea34-116">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="aea34-116">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="aea34-117">Installera Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="aea34-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="aea34-118">Hämta och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="aea34-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="aea34-119">Skapa en Azure-resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="aea34-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="aea34-120">hello följande är ett exempel Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="aea34-120">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="aea34-121">Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="aea34-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="aea34-122">Ställ in ingen källa och mål toouse utdata.</span><span class="sxs-lookup"><span data-stu-id="aea34-122">Set up an input source and output target toouse.</span></span> <span data-ttu-id="aea34-123">Mer information finns i [lägga till indata](stream-analytics-add-inputs.md) tooset upp en Exempelindata och [lägga till utdata](stream-analytics-add-outputs.md) tooset upp ett exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="aea34-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) tooset up a sample input and [Add Outputs](stream-analytics-add-outputs.md) tooset up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="aea34-124">Ställ in ett projekt</span><span class="sxs-lookup"><span data-stu-id="aea34-124">Set up a project</span></span>
<span data-ttu-id="aea34-125">toocreate analytics-jobbet använda hello Stream Analytics-API för .NET, först ställa in projektet.</span><span class="sxs-lookup"><span data-stu-id="aea34-125">toocreate an analytics job use hello Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="aea34-126">Skapa ett konsolprogram i Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="aea34-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="aea34-127">Kör hello följande kommandon i hello Package Manager-konsolen, tooinstall hello NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="aea34-127">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="aea34-128">hello först är en hello Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="aea34-128">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="aea34-129">hello är andra för Azure klientautentisering.</span><span class="sxs-lookup"><span data-stu-id="aea34-129">hello second one is for Azure client authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. <span data-ttu-id="aea34-130">Lägg till följande hello **appSettings** avsnittet toohello App.config-fil:</span><span class="sxs-lookup"><span data-stu-id="aea34-130">Add hello following **appSettings** section toohello App.config file:</span></span>
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    <span data-ttu-id="aea34-131">Ersätt värdena för **SubscriptionId** och **ActiveDirectoryTenantId** med din Azure-prenumeration och klient-ID: N.</span><span class="sxs-lookup"><span data-stu-id="aea34-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="aea34-132">Du kan hämta dessa värden genom att köra hello följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="aea34-132">You can get these values by running hello following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="aea34-133">Lägg till följande referens i filen .csproj hello:</span><span class="sxs-lookup"><span data-stu-id="aea34-133">Add hello following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

5. <span data-ttu-id="aea34-134">Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projektet:</span><span class="sxs-lookup"><span data-stu-id="aea34-134">Add hello following **using** statements toohello source file (Program.cs) in hello project:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. <span data-ttu-id="aea34-135">Lägg till en helper autentiseringsmetod:</span><span class="sxs-lookup"><span data-stu-id="aea34-135">Add an authentication helper method:</span></span>

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="aea34-136">Skapa ett Stream Analytics management-klienten</span><span class="sxs-lookup"><span data-stu-id="aea34-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="aea34-137">En **StreamAnalyticsManagementClient** objektet kan du toomanage hello jobbet och hello jobbet komponenter, till exempel indata, utdata och omvandling.</span><span class="sxs-lookup"><span data-stu-id="aea34-137">A **StreamAnalyticsManagementClient** object allows you toomanage hello job and hello job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="aea34-138">Lägg till följande kod toohello början av hello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="aea34-138">Add hello following code toohello beginning of hello **Main** method:</span></span>

   ```
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

<span data-ttu-id="aea34-139">Hej **resourceGroupName** variabelns värde ska vara detsamma som namnet på hello hello resurs gruppen du har skapats eller plockats i hello förkraven hello.</span><span class="sxs-lookup"><span data-stu-id="aea34-139">hello **resourceGroupName** variable's value should be hello same as hello name of hello resource group you created or picked in hello prerequisite steps.</span></span>

<span data-ttu-id="aea34-140">tooautomate hello autentiseringsuppgifter presentation aspekt av skapandet finns för[autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="aea34-140">tooautomate hello credential presentation aspect of job creation, refer too[Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="aea34-141">hello återstående avsnitten i den här artikeln förutsätter att den här koden är hello början av hello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="aea34-141">hello remaining sections of this article assume that this code is at hello beginning of hello **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="aea34-142">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="aea34-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="aea34-143">hello skapar följande kod en Stream Analytics-jobbet under hello resursgrupp som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="aea34-143">hello following code creates a Stream Analytics job under hello resource group that you have defined.</span></span> <span data-ttu-id="aea34-144">Du lägger till ett jobb för indata, utdata och omvandling toohello senare.</span><span class="sxs-lookup"><span data-stu-id="aea34-144">You will add an input, output, and transformation toohello job later.</span></span>

   ```
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="aea34-145">Skapa en inkommande Stream Analytics-källa</span><span class="sxs-lookup"><span data-stu-id="aea34-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="aea34-146">hello skapar följande kod en Stream Analytics indatakälla hello blob indatakälla typ och CSV-serialisering.</span><span class="sxs-lookup"><span data-stu-id="aea34-146">hello following code creates a Stream Analytics input source with hello blob input source type and CSV serialization.</span></span> <span data-ttu-id="aea34-147">toocreate en hub inkommande händelsekälla, Använd **EventHubStreamInputDataSource** i stället för **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="aea34-147">toocreate an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="aea34-148">På liknande sätt kan du anpassa hello serialiseringstyp av indatakälla hello.</span><span class="sxs-lookup"><span data-stu-id="aea34-148">Similarly, you can customize hello serialization type of hello input source.</span></span>

   ```
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

<span data-ttu-id="aea34-149">Indatakällor, är från Blob-lagring eller en händelsehubb bundet tooa specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="aea34-149">Input sources, whether from Blob storage or an event hub, are tied tooa specific job.</span></span> <span data-ttu-id="aea34-150">toouse Hej samma källa för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="aea34-150">toouse hello same input source for different jobs, you must call hello method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="aea34-151">Testa en Stream Analytics indatakälla</span><span class="sxs-lookup"><span data-stu-id="aea34-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="aea34-152">Hej **TestConnection** metoden tester om hello Stream Analytics-jobbet är kan tooconnect toohello Ange källa samt andra aspekter specifika toohello ange typen av datakälla.</span><span class="sxs-lookup"><span data-stu-id="aea34-152">hello **TestConnection** method tests whether hello Stream Analytics job is able tooconnect toohello input source as well as other aspects specific toohello input source type.</span></span> <span data-ttu-id="aea34-153">Till exempel i hello blob indatakälla du skapade tidigare, kontrollerar hello metoden att hello lagringskontonamn och en nyckel kan vara används tooconnect toohello lagringskonto samt kontrollera den angivna hello-behållaren finns.</span><span class="sxs-lookup"><span data-stu-id="aea34-153">For example, in hello blob input source you created in an earlier step, hello method will check that hello Storage account name and key pair can be used tooconnect toohello Storage account as well as check that hello specified container exists.</span></span>

   ```
   // Test hello connection toohello input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="aea34-154">Skapa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="aea34-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="aea34-155">Att skapa ett mål för utdata är mycket lik toocreating en Stream Analytics-Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="aea34-155">Creating an output target is very similar toocreating a Stream Analytics input source.</span></span> <span data-ttu-id="aea34-156">Precis som indatakällor är utdata mål bundet tooa specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="aea34-156">Like input sources, output targets are tied tooa specific job.</span></span> <span data-ttu-id="aea34-157">toouse Hej samma utdata mål för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="aea34-157">toouse hello same output target for different jobs, you must call hello method again and specify a different job name.</span></span>

<span data-ttu-id="aea34-158">hello följande kod skapar ett utgående mål (Azure SQL database).</span><span class="sxs-lookup"><span data-stu-id="aea34-158">hello following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="aea34-159">Du kan anpassa hello utdata mål datatyp och/eller serialiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="aea34-159">You can customize hello output target's data type and/or serialization type.</span></span>

   ```
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="aea34-160">Testa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="aea34-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="aea34-161">Ett Stream Analytics utdata mål har också hello **TestConnection** metod för att testa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="aea34-161">A Stream Analytics output target also has hello **TestConnection** method for testing connections.</span></span>

   ```
   // Test hello connection toohello output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="aea34-162">Skapa Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="aea34-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="aea34-163">hello följande kod skapar en Stream Analytics-omvandling med hello frågan ”Välj * från indata” och anger tooallocate en enhet för strömning hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aea34-163">hello following code creates a Stream Analytics transformation with hello query "select * from Input" and specifies tooallocate one streaming unit for hello Stream Analytics job.</span></span> <span data-ttu-id="aea34-164">Läs mer om hur du justerar strömningsenheter [skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="aea34-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with hello value you put for hello 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

<span data-ttu-id="aea34-165">En omvandling är också bundet toohello specifika Stream Analytics-jobbet den skapades under som indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="aea34-165">Like input and output, a transformation is also tied toohello specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="aea34-166">Starta ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="aea34-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="aea34-167">När du har skapat ett Stream Analytics-jobb och dess input(s), utdata och omvandling av du kan starta hello jobbet genom att anropa hello **starta** metod.</span><span class="sxs-lookup"><span data-stu-id="aea34-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start hello job by calling hello **Start** method.</span></span>

<span data-ttu-id="aea34-168">hello följande exempelkod startar en Stream Analytics-jobbet med en anpassad utdata start tid set tooDecember 12, 2012, 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="aea34-168">hello following sample code starts a Stream Analytics job with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC:</span></span>

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="aea34-169">Stoppa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="aea34-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="aea34-170">Du kan stoppa ett Stream Analytics-jobb som körs med anropa hello **stoppa** metod.</span><span class="sxs-lookup"><span data-stu-id="aea34-170">You can stop a running Stream Analytics job by calling hello **Stop** method.</span></span>

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="aea34-171">Ta bort ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="aea34-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="aea34-172">Hej **ta bort** metoden tar bort hello projekt samt hello underliggande underordnade resurser, inklusive input(s), utdata och omvandling av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="aea34-172">hello **Delete** method will delete hello job as well as hello underlying sub-resources, including input(s), output(s), and transformation of hello job.</span></span>

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a><span data-ttu-id="aea34-173">Få support</span><span class="sxs-lookup"><span data-stu-id="aea34-173">Get support</span></span>
<span data-ttu-id="aea34-174">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="aea34-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="aea34-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aea34-175">Next steps</span></span>
<span data-ttu-id="aea34-176">Du har lärt dig hello grunderna i en .NET SDK-toocreate och kör analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="aea34-176">You've learned hello basics of using a .NET SDK toocreate and run analytics jobs.</span></span> <span data-ttu-id="aea34-177">toolearn finns fler hello följande:</span><span class="sxs-lookup"><span data-stu-id="aea34-177">toolearn more, see hello following:</span></span>

* [<span data-ttu-id="aea34-178">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="aea34-178">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="aea34-179">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="aea34-179">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="aea34-180">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="aea34-180">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* <span data-ttu-id="aea34-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="aea34-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* [<span data-ttu-id="aea34-182">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="aea34-182">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="aea34-183">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="aea34-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
