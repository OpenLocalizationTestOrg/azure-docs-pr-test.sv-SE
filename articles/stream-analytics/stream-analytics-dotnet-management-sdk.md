---
title: "Hantering av .NET SDK för Azure Stream Analytics | Microsoft Docs"
description: "Kom igång med Stream Analytics Management .NET SDK. Lär dig hur du ställer in och kör analytics-jobb. Skapa ett projekt, indata, utdata och transformationer."
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
ms.openlocfilehash: f9aa812e6e82cc0f72d0cd1fe63058e53f794775
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a><span data-ttu-id="3b01a-106">Hantering av .NET SDK: Konfigurera och köra analytics-jobb med hjälp av Azure Stream Analytics-API: et för .NET</span><span class="sxs-lookup"><span data-stu-id="3b01a-106">Management .NET SDK: Set up and run analytics jobs using the Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="3b01a-107">Lär dig hur du ställer in och kör analytics-jobb med hjälp av Stream Analytics-API för .NET med hantering av .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="3b01a-107">Learn how to set up and run analytics jobs using the Stream Analytics API for .NET using the Management .NET SDK.</span></span> <span data-ttu-id="3b01a-108">Ställ in ett projekt, skapa inkommande och utgående källor, omvandlingar och starta och stoppa jobb.</span><span class="sxs-lookup"><span data-stu-id="3b01a-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="3b01a-109">För analytics-jobb kan du strömma data från Blob-lagring eller från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="3b01a-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="3b01a-110">Finns det [management referensdokumentationen för Stream Analytics-API för .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b01a-110">See the [management reference documentation for the Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="3b01a-111">Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet, skalbarhet, komplexa händelsebearbetning över strömmande data i molnet.</span><span class="sxs-lookup"><span data-stu-id="3b01a-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in the cloud.</span></span> <span data-ttu-id="3b01a-112">Stream Analytics ger kunder möjlighet att konfigurera direktuppspelningsjobb för att analysera dataströmmar, vilket gör att enheten nära analys i realtid.</span><span class="sxs-lookup"><span data-stu-id="3b01a-112">Stream Analytics enables customers to set up streaming jobs to analyze data streams, and allows them to drive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="3b01a-113">Vi har uppdaterat exempelkoden i den här artikeln med Azure Stream Analytics Management .NET SDK v2.x version.</span><span class="sxs-lookup"><span data-stu-id="3b01a-113">We have updated the sample code in this article with Azure Stream Analytics Management .NET SDK v2.x version.</span></span> <span data-ttu-id="3b01a-114">Exempelkod med SDK-version använder lagecy (1.x), se [använder Management .NET SDK-v1.x för Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span><span class="sxs-lookup"><span data-stu-id="3b01a-114">For sample code using the uses lagecy (1.x) SDK version, please see [Use the Management .NET SDK v1.x for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b01a-115">Krav</span><span class="sxs-lookup"><span data-stu-id="3b01a-115">Prerequisites</span></span>
<span data-ttu-id="3b01a-116">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="3b01a-116">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="3b01a-117">Installera Visual Studio 2017 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="3b01a-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="3b01a-118">Hämta och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3b01a-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="3b01a-119">Skapa en Azure-resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3b01a-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="3b01a-120">Följande är ett exempel Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="3b01a-120">The following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="3b01a-121">Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="3b01a-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="3b01a-122">Konfigurera ett inkommande käll- och utdata för användning.</span><span class="sxs-lookup"><span data-stu-id="3b01a-122">Set up an input source and output target to use.</span></span> <span data-ttu-id="3b01a-123">Mer information finns i [lägga till indata](stream-analytics-add-inputs.md) att ställa in en Exempelindata och [lägga till utdata](stream-analytics-add-outputs.md) att ställa in ett exempel på utdata.</span><span class="sxs-lookup"><span data-stu-id="3b01a-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) to set up a sample input and [Add Outputs](stream-analytics-add-outputs.md) to set up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="3b01a-124">Ställ in ett projekt</span><span class="sxs-lookup"><span data-stu-id="3b01a-124">Set up a project</span></span>
<span data-ttu-id="3b01a-125">Använd Stream Analytics-API för .NET, först konfigurera ditt projekt för att skapa analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="3b01a-125">To create an analytics job use the Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="3b01a-126">Skapa ett konsolprogram i Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="3b01a-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="3b01a-127">Kör följande kommandon för att installera NuGet-paket i Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3b01a-127">In the Package Manager Console, run the following commands to install the NuGet packages.</span></span> <span data-ttu-id="3b01a-128">Den första är Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="3b01a-128">The first one is the Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="3b01a-129">Den andra är för Azure klientautentisering.</span><span class="sxs-lookup"><span data-stu-id="3b01a-129">The second one is for Azure client authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. <span data-ttu-id="3b01a-130">Lägg till följande **appSettings** avsnitt i filen App.config:</span><span class="sxs-lookup"><span data-stu-id="3b01a-130">Add the following **appSettings** section to the App.config file:</span></span>
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    <span data-ttu-id="3b01a-131">Ersätt värdena för **SubscriptionId** och **ActiveDirectoryTenantId** med din Azure-prenumeration och klient-ID: N.</span><span class="sxs-lookup"><span data-stu-id="3b01a-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="3b01a-132">Du kan hämta dessa värden genom att köra följande Azure PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="3b01a-132">You can get these values by running the following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="3b01a-133">Lägg till följande referens i filen .csproj:</span><span class="sxs-lookup"><span data-stu-id="3b01a-133">Add the following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

5. <span data-ttu-id="3b01a-134">Lägg till följande **med** instruktioner till källfilen (Program.cs) i projektet:</span><span class="sxs-lookup"><span data-stu-id="3b01a-134">Add the following **using** statements to the source file (Program.cs) in the project:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. <span data-ttu-id="3b01a-135">Lägg till en helper autentiseringsmetod:</span><span class="sxs-lookup"><span data-stu-id="3b01a-135">Add an authentication helper method:</span></span>

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="3b01a-136">Skapa ett Stream Analytics management-klienten</span><span class="sxs-lookup"><span data-stu-id="3b01a-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="3b01a-137">En **StreamAnalyticsManagementClient** objektet kan du hantera jobbet och jobbet-komponenter, till exempel indata, utdata och omvandling.</span><span class="sxs-lookup"><span data-stu-id="3b01a-137">A **StreamAnalyticsManagementClient** object allows you to manage the job and the job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="3b01a-138">Lägg till följande kod i början av den **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="3b01a-138">Add the following code to the beginning of the **Main** method:</span></span>

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

<span data-ttu-id="3b01a-139">Den **resourceGroupName** variabelns värde ska vara samma som namnet på resursgruppen som du har skapat eller plockats i förkraven.</span><span class="sxs-lookup"><span data-stu-id="3b01a-139">The **resourceGroupName** variable's value should be the same as the name of the resource group you created or picked in the prerequisite steps.</span></span>

<span data-ttu-id="3b01a-140">Om du vill automatisera autentiseringsuppgifter presentation aspekt av skapa jobb, referera till [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="3b01a-140">To automate the credential presentation aspect of job creation, refer to [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="3b01a-141">De återstående avsnitten i den här artikeln förutsätter att den här koden är i början av den **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="3b01a-141">The remaining sections of this article assume that this code is at the beginning of the **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="3b01a-142">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3b01a-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="3b01a-143">Följande kod skapar en Stream Analytics-jobbet under den resursgrupp som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="3b01a-143">The following code creates a Stream Analytics job under the resource group that you have defined.</span></span> <span data-ttu-id="3b01a-144">Du lägger till indata, utdata och omvandling i jobbet senare.</span><span class="sxs-lookup"><span data-stu-id="3b01a-144">You will add an input, output, and transformation to the job later.</span></span>

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

## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="3b01a-145">Skapa en inkommande Stream Analytics-källa</span><span class="sxs-lookup"><span data-stu-id="3b01a-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="3b01a-146">Följande kod skapar en Stream Analytics-Indatakällan med Indatakällan blobbtypen och CSV-serialisering.</span><span class="sxs-lookup"><span data-stu-id="3b01a-146">The following code creates a Stream Analytics input source with the blob input source type and CSV serialization.</span></span> <span data-ttu-id="3b01a-147">Så här skapar du en hub inkommande händelsekälla **EventHubStreamInputDataSource** i stället för **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="3b01a-147">To create an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="3b01a-148">På liknande sätt kan du anpassa serialisering typ av Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="3b01a-148">Similarly, you can customize the serialization type of the input source.</span></span>

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

<span data-ttu-id="3b01a-149">Indatakällor, som från Blob-lagring eller en händelsehubb är knutna till ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="3b01a-149">Input sources, whether from Blob storage or an event hub, are tied to a specific job.</span></span> <span data-ttu-id="3b01a-150">Om du vill använda samma Indatakällan för olika jobb, måste du anropa metoden igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="3b01a-150">To use the same input source for different jobs, you must call the method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="3b01a-151">Testa en Stream Analytics indatakälla</span><span class="sxs-lookup"><span data-stu-id="3b01a-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="3b01a-152">Den **TestConnection** metoden testar om Stream Analytics-jobbet är ansluta till Indatakällan samt andra aspekter som är specifika för inkommande källtypen.</span><span class="sxs-lookup"><span data-stu-id="3b01a-152">The **TestConnection** method tests whether the Stream Analytics job is able to connect to the input source as well as other aspects specific to the input source type.</span></span> <span data-ttu-id="3b01a-153">Till exempel i blob Indatakällan du skapade tidigare, kontrollerar metoden att lagringskontonamn och nyckel kan användas för att ansluta till lagringskontot samt kontrollera att den angivna behållaren finns.</span><span class="sxs-lookup"><span data-stu-id="3b01a-153">For example, in the blob input source you created in an earlier step, the method will check that the Storage account name and key pair can be used to connect to the Storage account as well as check that the specified container exists.</span></span>

   ```
   // Test the connection to the input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="3b01a-154">Skapa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="3b01a-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="3b01a-155">Skapa ett mål för utdata liknar att skapa ett Stream Analytics-Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="3b01a-155">Creating an output target is very similar to creating a Stream Analytics input source.</span></span> <span data-ttu-id="3b01a-156">Som indatakällor, är utdata mål knutna till ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="3b01a-156">Like input sources, output targets are tied to a specific job.</span></span> <span data-ttu-id="3b01a-157">Om du vill använda samma utdata mål för olika jobb, måste du anropa metoden igen och ange ett annat jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="3b01a-157">To use the same output target for different jobs, you must call the method again and specify a different job name.</span></span>

<span data-ttu-id="3b01a-158">Följande kod skapar ett utgående mål (Azure SQL database).</span><span class="sxs-lookup"><span data-stu-id="3b01a-158">The following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="3b01a-159">Du kan anpassa utdata målets datatyp och/eller serialiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="3b01a-159">You can customize the output target's data type and/or serialization type.</span></span>

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

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="3b01a-160">Testa ett mål för Stream Analytics-utdata</span><span class="sxs-lookup"><span data-stu-id="3b01a-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="3b01a-161">Ett Stream Analytics utdata mål har även den **TestConnection** metod för att testa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="3b01a-161">A Stream Analytics output target also has the **TestConnection** method for testing connections.</span></span>

   ```
   // Test the connection to the output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="3b01a-162">Skapa Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3b01a-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="3b01a-163">Följande kod skapar en Stream Analytics-omvandling med frågan ”Välj * från indata” och anger om du vill allokera en strömmande enhet för Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="3b01a-163">The following code creates a Stream Analytics transformation with the query "select * from Input" and specifies to allocate one streaming unit for the Stream Analytics job.</span></span> <span data-ttu-id="3b01a-164">Läs mer om hur du justerar strömningsenheter [skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="3b01a-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with the value you put for the 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

<span data-ttu-id="3b01a-165">En omvandling är även kopplad till specifika Stream Analytics-jobbet den skapades under som indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="3b01a-165">Like input and output, a transformation is also tied to the specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="3b01a-166">Starta ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3b01a-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="3b01a-167">När du har skapat ett Stream Analytics-jobb och dess input(s), utdata och omvandling av du startar jobbet genom att anropa den **starta** metod.</span><span class="sxs-lookup"><span data-stu-id="3b01a-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start the job by calling the **Start** method.</span></span>

<span data-ttu-id="3b01a-168">Följande exempel kod startar ett Stream Analytics-jobb med anpassade utdata starttid inställd på 12 December 2012 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="3b01a-168">The following sample code starts a Stream Analytics job with a custom output start time set to December 12, 2012, 12:12:12 UTC:</span></span>

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="3b01a-169">Stoppa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3b01a-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="3b01a-170">Du kan stoppa ett Stream Analytics-jobb som körs genom att anropa den **stoppa** metod.</span><span class="sxs-lookup"><span data-stu-id="3b01a-170">You can stop a running Stream Analytics job by calling the **Stop** method.</span></span>

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="3b01a-171">Ta bort ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3b01a-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="3b01a-172">Den **ta bort** metoden tar bort jobbet samt de underliggande underordnade resurser, inklusive input(s), utdata och omvandling av jobbet.</span><span class="sxs-lookup"><span data-stu-id="3b01a-172">The **Delete** method will delete the job as well as the underlying sub-resources, including input(s), output(s), and transformation of the job.</span></span>

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a><span data-ttu-id="3b01a-173">Få support</span><span class="sxs-lookup"><span data-stu-id="3b01a-173">Get support</span></span>
<span data-ttu-id="3b01a-174">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="3b01a-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b01a-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b01a-175">Next steps</span></span>
<span data-ttu-id="3b01a-176">Du har lärt dig grunderna i att använda en .NET SDK för att skapa och köra analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="3b01a-176">You've learned the basics of using a .NET SDK to create and run analytics jobs.</span></span> <span data-ttu-id="3b01a-177">Läs mer i:</span><span class="sxs-lookup"><span data-stu-id="3b01a-177">To learn more, see the following:</span></span>

* [<span data-ttu-id="3b01a-178">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3b01a-178">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="3b01a-179">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3b01a-179">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="3b01a-180">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="3b01a-180">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* <span data-ttu-id="3b01a-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b01a-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* [<span data-ttu-id="3b01a-182">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="3b01a-182">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="3b01a-183">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="3b01a-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
