---
title: "Programmässigt övervaka jobb i Stream Analytics | Microsoft Docs"
description: "Lär dig att övervaka programmässigt Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell."
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
ms.openlocfilehash: 0d39e77316a03a705586af3ba970a7be1208ec85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a><span data-ttu-id="e3597-104">Programmässigt skapa en Övervakare för Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="e3597-104">Programmatically create a Stream Analytics job monitor</span></span>

<span data-ttu-id="e3597-105">Den här artikeln visar hur du aktiverar övervakning av ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="e3597-105">This article demonstrates how to enable monitoring for a Stream Analytics job.</span></span> <span data-ttu-id="e3597-106">Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell har inte övervaka aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="e3597-106">Stream Analytics jobs that are created via REST APIs, Azure SDK, or PowerShell do not have monitoring enabled by default.</span></span> <span data-ttu-id="e3597-107">Du kan manuellt aktivera den i Azure portal genom att gå till sidan för jobbets övervakaren och klicka på knappen Aktivera eller du kan automatisera processen genom att följa stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e3597-107">You can manually enable it in the Azure portal by going to the job’s Monitor page and clicking the Enable button or you can automate this process by following the steps in this article.</span></span> <span data-ttu-id="e3597-108">Övervakningsdata kommer att visas i området mätvärden i Azure portal för Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3597-108">The monitoring data will show up in the Metrics area of the Azure portal for your Stream Analytics job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3597-109">Krav</span><span class="sxs-lookup"><span data-stu-id="e3597-109">Prerequisites</span></span>

<span data-ttu-id="e3597-110">Innan du börjar den här processen måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="e3597-110">Before you begin this process, you must have the following:</span></span>

* <span data-ttu-id="e3597-111">Visual Studio-2017 eller 2015</span><span class="sxs-lookup"><span data-stu-id="e3597-111">Visual Studio 2017 or 2015</span></span>
* <span data-ttu-id="e3597-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) hämtas och installeras</span><span class="sxs-lookup"><span data-stu-id="e3597-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) downloaded and installed</span></span>
* <span data-ttu-id="e3597-113">Ett befintligt Stream Analytics-jobb som måste ha övervakning aktiverad</span><span class="sxs-lookup"><span data-stu-id="e3597-113">An existing Stream Analytics job that needs to have monitoring enabled</span></span>

## <a name="create-a-project"></a><span data-ttu-id="e3597-114">Skapa ett projekt</span><span class="sxs-lookup"><span data-stu-id="e3597-114">Create a project</span></span>

1. <span data-ttu-id="e3597-115">Skapa ett konsolprogram i Visual Studio C# .NET.</span><span class="sxs-lookup"><span data-stu-id="e3597-115">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="e3597-116">Kör följande kommandon för att installera NuGet-paket i Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e3597-116">In the Package Manager Console, run the following commands to install the NuGet packages.</span></span> <span data-ttu-id="e3597-117">Den första är Azure Stream Analytics Management .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="e3597-117">The first one is the Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="e3597-118">Den andra är Azure övervakaren SDK som används för att aktivera övervakning.</span><span class="sxs-lookup"><span data-stu-id="e3597-118">The second one is the Azure Monitor SDK that will be used to enable monitoring.</span></span> <span data-ttu-id="e3597-119">Den sista som är Azure Active Directory-klient som ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="e3597-119">The last one is the Azure Active Directory client that will be used for authentication.</span></span>
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. <span data-ttu-id="e3597-120">Lägg till avsnittet appSettings i filen App.config.</span><span class="sxs-lookup"><span data-stu-id="e3597-120">Add the following appSettings section to the App.config file.</span></span>
   
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
   <span data-ttu-id="e3597-121">Ersätt värdena för *SubscriptionId* och *ActiveDirectoryTenantId* med din Azure-prenumeration och klient-ID: N.</span><span class="sxs-lookup"><span data-stu-id="e3597-121">Replace values for *SubscriptionId* and *ActiveDirectoryTenantId* with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="e3597-122">Du kan hämta dessa värden genom att köra följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="e3597-122">You can get these values by running the following PowerShell cmdlet:</span></span>
   
   ```
   Get-AzureAccount
   ```
4. <span data-ttu-id="e3597-123">Lägg till följande med hjälp av rapporter till källfilen (Program.cs) i projektet.</span><span class="sxs-lookup"><span data-stu-id="e3597-123">Add the following using statements to the source file (Program.cs) in the project.</span></span>
   
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
5. <span data-ttu-id="e3597-124">Lägg till en autentiseringsmetod för hjälp.</span><span class="sxs-lookup"><span data-stu-id="e3597-124">Add an authentication helper method.</span></span>
   
     <span data-ttu-id="e3597-125">sträng med offentlig statisk GetAuthorizationHeader()</span><span class="sxs-lookup"><span data-stu-id="e3597-125">public static string GetAuthorizationHeader()</span></span>
   
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
   
             throw new InvalidOperationException("Failed to acquire token");
     <span data-ttu-id="e3597-126">}</span><span class="sxs-lookup"><span data-stu-id="e3597-126">}</span></span>

## <a name="create-management-clients"></a><span data-ttu-id="e3597-127">Skapa av hanteringsklienter</span><span class="sxs-lookup"><span data-stu-id="e3597-127">Create management clients</span></span>

<span data-ttu-id="e3597-128">Följande kod ställer in den nödvändiga variabler och av hanteringsklienter.</span><span class="sxs-lookup"><span data-stu-id="e3597-128">The following code will set up the necessary variables and management clients.</span></span>

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

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a><span data-ttu-id="e3597-129">Aktivera övervakning av ett befintligt Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="e3597-129">Enable monitoring for an existing Stream Analytics job</span></span>

<span data-ttu-id="e3597-130">Följande kod aktiverar övervakning för en **befintliga** Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3597-130">The following code enables monitoring for an **existing** Stream Analytics job.</span></span> <span data-ttu-id="e3597-131">Den första delen av koden utför en GET-begäran mot Stream Analytics-tjänsten att hämta information om Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3597-131">The first part of the code performs a GET request against the Stream Analytics service to retrieve information about the particular Stream Analytics job.</span></span> <span data-ttu-id="e3597-132">Den använder den *Id* egenskapen (hämtades från GET-begäran) som en parameter för Put-metoden i den andra hälften av den kod som skickar ett PUT begäran till Insights-tjänsten för att aktivera övervakning av Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3597-132">It uses the *Id* property (retrieved from the GET request) as a parameter for the Put method in the second half of the code, which sends a PUT request to the Insights service to enable monitoring for the Stream Analytics job.</span></span>

>[!WARNING]
><span data-ttu-id="e3597-133">Om du tidigare har aktiverat övervakning för ett annat Stream Analytics-jobb, antingen via Azure-portalen eller programmässigt via den nedan kod, **rekommenderar vi att du anger samma lagringskontots namn som du använde när du tidigare aktivera övervakning.**</span><span class="sxs-lookup"><span data-stu-id="e3597-133">If you have previously enabled monitoring for a different Stream Analytics job, either through the Azure portal or programmatically via the below code, **we recommend that you provide the same storage account name that you used when you previously enabled monitoring.**</span></span>
> 
> <span data-ttu-id="e3597-134">Lagringskontot är länkad till regionen som du skapade Stream Analytics-jobbet i, inte till jobbet sig själv.</span><span class="sxs-lookup"><span data-stu-id="e3597-134">The storage account is linked to the region that you created your Stream Analytics job in, not specifically to the job itself.</span></span>
> 
> <span data-ttu-id="e3597-135">Alla Stream Analytics-jobb (och alla andra Azure-resurser) i samma regionen dela det här lagringskontot för att lagra övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="e3597-135">All Stream Analytics jobs (and all other Azure resources) in that same region share this storage account to store monitoring data.</span></span> <span data-ttu-id="e3597-136">Om du anger ett annat lagringskonto kan orsaka oönskade sidoeffekter övervakningen av andra Stream Analytics-jobb eller andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e3597-136">If you provide a different storage account, it might cause unintended side effects in the monitoring of your other Stream Analytics jobs or other Azure resources.</span></span>
> 
> <span data-ttu-id="e3597-137">Lagringskontonamnet som används för att ersätta `<YOUR STORAGE ACCOUNT NAME>` i följande kod ska vara ett lagringskonto som är i samma prenumeration som du aktiverar övervakning för Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3597-137">The storage account name that you use to replace `<YOUR STORAGE ACCOUNT NAME>` in the following code should be a storage account that is in the same subscription as the Stream Analytics job that you are enabling monitoring for.</span></span>
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



## <a name="get-support"></a><span data-ttu-id="e3597-138">Få support</span><span class="sxs-lookup"><span data-stu-id="e3597-138">Get support</span></span>

<span data-ttu-id="e3597-139">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="e3597-139">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3597-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3597-140">Next steps</span></span>

* [<span data-ttu-id="e3597-141">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e3597-141">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="e3597-142">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e3597-142">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="e3597-143">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="e3597-143">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="e3597-144">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="e3597-144">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="e3597-145">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="e3597-145">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

