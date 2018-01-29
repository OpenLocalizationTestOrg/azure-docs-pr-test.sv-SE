---
title: "Programmässigt övervaka jobb i Stream Analytics | Microsoft Docs"
description: "Lär dig att övervaka programmässigt Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell."
keywords: "Övervakare för .net, övervakaren, övervaka appen"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 7e9d2f6f03fd539c59b105108fb46697bcd60f1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programmässigt skapa en Övervakare för Stream Analytics-jobb

Den här artikeln visar hur du aktiverar övervakning av ett Stream Analytics-jobb. Stream Analytics-jobb som skapats via REST API: er, Azure SDK eller PowerShell har inte övervaka aktiverad som standard. Du kan manuellt aktivera den i Azure portal genom att gå till sidan för jobbets övervakaren och klicka på knappen Aktivera eller du kan automatisera processen genom att följa stegen i den här artikeln. Övervakningsdata kommer att visas i området mätvärden i Azure portal för Stream Analytics-jobbet.

## <a name="prerequisites"></a>Krav

Innan du börjar den här processen måste du ha följande:

* Visual Studio-2017 eller 2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/) hämtas och installeras
* Ett befintligt Stream Analytics-jobb som måste ha övervakning aktiverad

## <a name="create-a-project"></a>Skapa ett projekt

1. Skapa ett konsolprogram i Visual Studio C# .NET.
2. Kör följande kommandon för att installera NuGet-paket i Package Manager-konsolen. Den första är Azure Stream Analytics Management .NET SDK. Den andra är Azure övervakaren SDK som används för att aktivera övervakning. Den sista som är Azure Active Directory-klient som ska användas för autentisering.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Lägg till avsnittet appSettings i filen App.config.
   
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
   Ersätt värdena för *SubscriptionId* och *ActiveDirectoryTenantId* med din Azure-prenumeration och klient-ID: N. Du kan hämta dessa värden genom att köra följande PowerShell-cmdlet:
   
   ```
   Get-AzureAccount
   ```
4. Lägg till följande med hjälp av rapporter till källfilen (Program.cs) i projektet.
   
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
   
             throw new InvalidOperationException("Failed to acquire token");
     }

## <a name="create-management-clients"></a>Skapa av hanteringsklienter

Följande kod ställer in den nödvändiga variabler och av hanteringsklienter.

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

Följande kod aktiverar övervakning för en **befintliga** Stream Analytics-jobbet. Den första delen av koden utför en GET-begäran mot Stream Analytics-tjänsten att hämta information om Stream Analytics-jobbet. Den använder den *Id* egenskapen (hämtades från GET-begäran) som en parameter för Put-metoden i den andra hälften av den kod som skickar ett PUT begäran till Insights-tjänsten för att aktivera övervakning av Stream Analytics-jobbet.

>[!WARNING]
>Om du tidigare har aktiverat övervakning för ett annat Stream Analytics-jobb, antingen via Azure-portalen eller programmässigt via den nedan kod, **rekommenderar vi att du anger samma lagringskontots namn som du använde när du tidigare aktivera övervakning.**
> 
> Lagringskontot är länkad till regionen som du skapade Stream Analytics-jobbet i, inte till jobbet sig själv.
> 
> Alla Stream Analytics-jobb (och alla andra Azure-resurser) i samma regionen dela det här lagringskontot för att lagra övervakningsdata. Om du anger ett annat lagringskonto kan orsaka oönskade sidoeffekter övervakningen av andra Stream Analytics-jobb eller andra Azure-resurser.
> 
> Lagringskontonamnet som används för att ersätta `<YOUR STORAGE ACCOUNT NAME>` i följande kod ska vara ett lagringskonto som är i samma prenumeration som du aktiverar övervakning för Stream Analytics-jobbet.
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

* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

