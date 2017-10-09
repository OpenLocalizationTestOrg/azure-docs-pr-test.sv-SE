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
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programmässigt skapa en Övervakare för Stream Analytics-jobb

Den här artikeln visar hur tooenable övervakning av ett Stream Analytics-jobb. Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell har inte övervaka aktiverad som standard. Du aktivera manuellt den i hello Azure-portalen genom att gå toohello jobbet övervakaren sidan och klicka på hello aktivera knappen eller du kan automatisera processen genom att följa hello stegen i den här artikeln. hello övervakningsdata visas under hello mått i hello Azure portal för Stream Analytics-jobbet.

## <a name="prerequisites"></a>Krav

Innan du börjar den här processen måste du ha hello följande:

* Visual Studio-2017 eller 2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/) hämtas och installeras
* Ett Stream Analytics-jobb som behöver toohave övervakning aktiverad

## <a name="create-a-project"></a>Skapa ett projekt

1. Skapa ett konsolprogram i Visual Studio C# .NET.
2. Kör hello följande kommandon i hello Package Manager-konsolen, tooinstall hello NuGet-paket. hello först är en hello Azure Stream Analytics Management .NET SDK. hello andra är hello Azure övervakaren SDK som ska användas tooenable övervakning. hello senast är en hello Azure Active Directory-klient som ska användas för autentisering.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Lägg till hello följande appSettings avsnittet toohello App.config-fil.
   
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
   Ersätt värdena för *SubscriptionId* och *ActiveDirectoryTenantId* med din Azure-prenumeration och klient-ID: N. Du kan hämta dessa värden genom att köra följande PowerShell-cmdleten hello:
   
   ```
   Get-AzureAccount
   ```
4. Lägg till följande hello med instruktioner toohello källfilen (Program.cs) i hello-projekt.
   
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
5. Lägg till en autentiseringsmetod för hjälp.
   
     sträng med offentlig statisk GetAuthorizationHeader()
   
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
     }

## <a name="create-management-clients"></a>Skapa av hanteringsklienter

hello ställer följande kod in hello nödvändiga variabler och av hanteringsklienter.

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

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Aktivera övervakning av ett befintligt Stream Analytics-jobb

hello följande kod aktiverar övervakning för en **befintliga** Stream Analytics-jobbet. hello första delen av hello koden utför en GET-begäran mot hello Stream Analytics-tjänsten tooretrieve information om hello viss Stream Analytics-jobbet. Den använder hello *Id* egenskapen (hämtades från hello GET-begäran) som en parameter för hello Put-metoden i hello andra halvan av hello-kod som skickar en PUT-begäran toohello insikter tjänsten tooenable övervakning för hello Stream Analytics jobbet.

>[!WARNING]
>Om du tidigare har aktiverat övervakning för en annan Stream Analytics-jobbet hello Azure-portalen eller via programmässigt via hello nedan kod, **rekommenderar vi att du anger hello samma lagringskontonamnet som du använde när du tidigare har aktiverat övervakning.**
> 
> hello storage-konto är länkade toohello region som du skapade Stream Analytics-jobbet i inte uttryckligen toohello jobbet sig själv.
> 
> Alla Stream Analytics-jobb (och alla andra Azure-resurser) i samma regionen dela den här storage-konto toostore övervakningsdata. Om du anger ett annat lagringskonto kan orsaka oönskade sidoeffekter hello övervakningen av andra Stream Analytics-jobb eller andra Azure-resurser.
> 
> Hej lagringskontonamnet att du använder tooreplace `<YOUR STORAGE ACCOUNT NAME>` hello följande kod ska vara ett lagringskonto som är i hello samma prenumeration som hello Stream Analytics-jobbet som du aktiverar övervakning för.
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



## <a name="get-support"></a>Få support

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

