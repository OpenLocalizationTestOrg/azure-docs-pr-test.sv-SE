---
title: "aaaProgrammatically övervaka jobb i Stream Analytics | Microsoft Docs"
description: "Lär dig hur tooprogrammatically övervaka Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell."
keywords: "Övervakare för .net, övervakaren, övervaka appen"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 44a9c29c2161ee81ea76ece4646a8691bf5d5b48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a><span data-ttu-id="89a1b-104">Programmässigt skapa en Övervakare för Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="89a1b-104">Programmatically create a Stream Analytics job monitor</span></span>

<span data-ttu-id="89a1b-105">Den här artikeln visar hur tooenable övervakning av ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="89a1b-105">This article demonstrates how tooenable monitoring for a Stream Analytics job.</span></span> <span data-ttu-id="89a1b-106">Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell har inte övervaka aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="89a1b-106">Stream Analytics jobs that are created via REST APIs, Azure SDK, or PowerShell do not have monitoring enabled by default.</span></span> <span data-ttu-id="89a1b-107">Du aktivera manuellt den i hello Azure-portalen genom att gå toohello jobbet övervakaren sidan och klicka på hello aktivera knappen eller du kan automatisera processen genom att följa hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="89a1b-107">You can manually enable it in hello Azure portal by going toohello job’s Monitor page and clicking hello Enable button or you can automate this process by following hello steps in this article.</span></span> <span data-ttu-id="89a1b-108">hello övervakningsdata visas under hello mått i hello Azure portal för Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="89a1b-108">hello monitoring data will show up in hello Metrics area of hello Azure portal for your Stream Analytics job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89a1b-109">Krav</span><span class="sxs-lookup"><span data-stu-id="89a1b-109">Prerequisites</span></span>

<span data-ttu-id="89a1b-110">Innan du börjar den här processen måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="89a1b-110">Before you begin this process, you must have hello following:</span></span>

* <span data-ttu-id="89a1b-111">Visual Studio-2017 eller 2015</span><span class="sxs-lookup"><span data-stu-id="89a1b-111">Visual Studio 2017 or 2015</span></span>
* <span data-ttu-id="89a1b-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) hämtas och installeras</span><span class="sxs-lookup"><span data-stu-id="89a1b-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) downloaded and installed</span></span>
* <span data-ttu-id="89a1b-113">Ett Stream Analytics-jobb som behöver toohave övervakning aktiverad</span><span class="sxs-lookup"><span data-stu-id="89a1b-113">An existing Stream Analytics job that needs toohave monitoring enabled</span></span>

## <a name="create-a-project"></a><span data-ttu-id="89a1b-114">Skapa ett projekt</span><span class="sxs-lookup"><span data-stu-id="89a1b-114">Create a project</span></span>

1. <span data-ttu-id="89a1b-115">Skapa ett konsolprogram i Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="89a1b-115">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="89a1b-116">Kör hello följande kommandon i hello Package Manager-konsolen, tooinstall hello NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="89a1b-116">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="89a1b-117">hello först är en hello Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="89a1b-117">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="89a1b-118">hello andra är hello Azure övervakaren SDK som ska användas tooenable övervakning.</span><span class="sxs-lookup"><span data-stu-id="89a1b-118">hello second one is hello Azure Monitor SDK that will be used tooenable monitoring.</span></span> <span data-ttu-id="89a1b-119">hello senast är en hello Azure Active Directory-klient som ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="89a1b-119">hello last one is hello Azure Active Directory client that will be used for authentication.</span></span>
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. <span data-ttu-id="89a1b-120">Lägg till hello följande appSettings avsnittet toohello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="89a1b-120">Add hello following appSettings section toohello App.config file.</span></span>
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   <span data-ttu-id="89a1b-121">Ersätt värdena för *SubscriptionId* och *ActiveDirectoryTenantId* med din Azure-prenumeration och klient-ID: N.</span><span class="sxs-lookup"><span data-stu-id="89a1b-121">Replace values for *SubscriptionId* and *ActiveDirectoryTenantId* with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="89a1b-122">Du kan hämta dessa värden genom att köra följande PowerShell-cmdleten hello:</span><span class="sxs-lookup"><span data-stu-id="89a1b-122">You can get these values by running hello following PowerShell cmdlet:</span></span>
   
   ```
   Get-AzureAccount
   ```
4. <span data-ttu-id="89a1b-123">Lägg till följande hello med instruktioner toohello källfilen (Program.cs) i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="89a1b-123">Add hello following using statements toohello source file (Program.cs) in hello project.</span></span>
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. <span data-ttu-id="89a1b-124">Lägg till en autentiseringsmetod för hjälp.</span><span class="sxs-lookup"><span data-stu-id="89a1b-124">Add an authentication helper method.</span></span>
   
     <span data-ttu-id="89a1b-125">sträng med offentlig statisk GetAuthorizationHeader()</span><span class="sxs-lookup"><span data-stu-id="89a1b-125">public static string GetAuthorizationHeader()</span></span>
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed tooacquire token");
     <span data-ttu-id="89a1b-126">}</span><span class="sxs-lookup"><span data-stu-id="89a1b-126">}</span></span>

## <a name="create-management-clients"></a><span data-ttu-id="89a1b-127">Skapa av hanteringsklienter</span><span class="sxs-lookup"><span data-stu-id="89a1b-127">Create management clients</span></span>

<span data-ttu-id="89a1b-128">hello ställer följande kod in hello nödvändiga variabler och av hanteringsklienter.</span><span class="sxs-lookup"><span data-stu-id="89a1b-128">hello following code will set up hello necessary variables and management clients.</span></span>

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a><span data-ttu-id="89a1b-129">Aktivera övervakning av ett befintligt Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="89a1b-129">Enable monitoring for an existing Stream Analytics job</span></span>

<span data-ttu-id="89a1b-130">hello följande kod aktiverar övervakning för en **befintliga** Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="89a1b-130">hello following code enables monitoring for an **existing** Stream Analytics job.</span></span> <span data-ttu-id="89a1b-131">hello första delen av hello koden utför en GET-begäran mot hello Stream Analytics-tjänsten tooretrieve information om hello viss Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="89a1b-131">hello first part of hello code performs a GET request against hello Stream Analytics service tooretrieve information about hello particular Stream Analytics job.</span></span> <span data-ttu-id="89a1b-132">Den använder hello *Id* egenskapen (hämtades från hello GET-begäran) som en parameter för hello Put-metoden i hello andra halvan av hello-kod som skickar en PUT-begäran toohello insikter tjänsten tooenable övervakning för hello Stream Analytics jobbet.</span><span class="sxs-lookup"><span data-stu-id="89a1b-132">It uses hello *Id* property (retrieved from hello GET request) as a parameter for hello Put method in hello second half of hello code, which sends a PUT request toohello Insights service tooenable monitoring for hello Stream Analytics job.</span></span>

>[!WARNING]
><span data-ttu-id="89a1b-133">Om du tidigare har aktiverat övervakning för en annan Stream Analytics-jobbet hello Azure-portalen eller via programmässigt via hello nedan kod, **rekommenderar vi att du anger hello samma lagringskontonamnet som du använde när du tidigare har aktiverat övervakning.**</span><span class="sxs-lookup"><span data-stu-id="89a1b-133">If you have previously enabled monitoring for a different Stream Analytics job, either through hello Azure portal or programmatically via hello below code, **we recommend that you provide hello same storage account name that you used when you previously enabled monitoring.**</span></span>
> 
> <span data-ttu-id="89a1b-134">hello storage-konto är länkade toohello region som du skapade Stream Analytics-jobbet i inte uttryckligen toohello jobbet sig själv.</span><span class="sxs-lookup"><span data-stu-id="89a1b-134">hello storage account is linked toohello region that you created your Stream Analytics job in, not specifically toohello job itself.</span></span>
> 
> <span data-ttu-id="89a1b-135">Alla Stream Analytics-jobb (och alla andra Azure-resurser) i samma regionen dela den här storage-konto toostore övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="89a1b-135">All Stream Analytics jobs (and all other Azure resources) in that same region share this storage account toostore monitoring data.</span></span> <span data-ttu-id="89a1b-136">Om du anger ett annat lagringskonto kan orsaka oönskade sidoeffekter hello övervakningen av andra Stream Analytics-jobb eller andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="89a1b-136">If you provide a different storage account, it might cause unintended side effects in hello monitoring of your other Stream Analytics jobs or other Azure resources.</span></span>
> 
> <span data-ttu-id="89a1b-137">Hej lagringskontonamnet att du använder tooreplace `<YOUR STORAGE ACCOUNT NAME>` hello följande kod ska vara ett lagringskonto som är i hello samma prenumeration som hello Stream Analytics-jobbet som du aktiverar övervakning för.</span><span class="sxs-lookup"><span data-stu-id="89a1b-137">hello storage account name that you use tooreplace `<YOUR STORAGE ACCOUNT NAME>` in hello following code should be a storage account that is in hello same subscription as hello Stream Analytics job that you are enabling monitoring for.</span></span>
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a><span data-ttu-id="89a1b-138">Få support</span><span class="sxs-lookup"><span data-stu-id="89a1b-138">Get support</span></span>

<span data-ttu-id="89a1b-139">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="89a1b-139">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="89a1b-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89a1b-140">Next steps</span></span>

* [<span data-ttu-id="89a1b-141">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="89a1b-141">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="89a1b-142">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="89a1b-142">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="89a1b-143">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="89a1b-143">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="89a1b-144">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="89a1b-144">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="89a1b-145">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="89a1b-145">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

