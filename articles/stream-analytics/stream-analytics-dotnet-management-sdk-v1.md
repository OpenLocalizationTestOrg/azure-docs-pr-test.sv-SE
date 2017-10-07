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
# <a name="management-net-sdk-v1x-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a>Hantering av .NET SDK v1.x: konfigurera och kör analytics-jobb med hjälp av hello Azure Stream Analytics API för .NET
Lär dig hur tooset in och kör analytics-jobb med hello Stream Analytics-API för att använda .NET hello Management .NET SDK. Ställ in ett projekt, skapa inkommande och utgående källor, omvandlingar och starta och stoppa jobb. För analytics-jobb kan du strömma data från Blob-lagring eller från en händelsehubb.

Se hello [management i referensdokumentationen för hello Stream Analytics-API för .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet, skalbarhet, komplexa händelsebearbetning över strömmande data i hello molnet. Stream Analytics gör att kunder tooset in strömning jobb tooanalyze dataströmmar och låter dem toodrive nära analys i realtid.  

> [!NOTE]
> Exempelkoden i den här artikeln använder fortfarande äldre (1.x) versionen av Azure Stream Analytics Management .NET SDK. Exempelkod med hello uppdaterade SDK-version, se [Använd hello Management .NET SDK för Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* Installera Visual Studio 2017 eller 2015.
* Hämta och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/).
* Skapa en Azure-resursgrupp i din prenumeration. hello följande är ett exempel Azure PowerShell-skript. Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Ställ in ingen källa och mål toouse utdata. Mer information finns i [lägga till indata](stream-analytics-add-inputs.md) tooset upp en Exempelindata och [lägga till utdata](stream-analytics-add-outputs.md) tooset upp ett exempel på utdata.

## <a name="set-up-a-project"></a>Ställ in ett projekt
toocreate analytics-jobbet använda hello Stream Analytics-API för .NET, först ställa in projektet.

1. Skapa ett konsolprogram i Visual Studio C# .NET.
2. Kör hello följande kommandon i hello Package Manager-konsolen, tooinstall hello NuGet-paket. hello först är en hello Azure Stream Analytics Management .NET SDK. hello är andra hello Azure Active Directory-klient som ska användas för autentisering.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
3. Lägg till följande hello **appSettings** avsnittet toohello App.config-fil:
   
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

    Ersätt värdena för **SubscriptionId** och **ActiveDirectoryTenantId** med din Azure-prenumeration och klient-ID: N. Du kan hämta dessa värden genom att köra hello följande Azure PowerShell-cmdlet:

        Get-AzureAccount

4. Lägg till följande referens i filen .csproj hello:

        <Reference Include="System.Configuration" />

1. Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projektet:
   
        using System;
        using System.Configuration;
        using System.Threading.Tasks;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. Lägg till en helper autentiseringsmetod:

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

## <a name="create-a-stream-analytics-management-client"></a>Skapa ett Stream Analytics management-klienten
En **StreamAnalyticsManagementClient** objektet kan du toomanage hello jobbet och hello jobbet komponenter, till exempel indata, utdata och omvandling.

Lägg till följande kod toohello början av hello hello **Main** metoden:

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

Hej **resourceGroupName** variabelns värde ska vara detsamma som namnet på hello hello resurs gruppen du har skapats eller plockats i hello förkraven hello.

tooautomate hello autentiseringsuppgifter presentation aspekt av skapandet finns för[autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).

hello återstående avsnitten i den här artikeln förutsätter att den här koden är hello början av hello **Main** metod.

## <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb
hello skapar följande kod en Stream Analytics-jobbet under hello resursgrupp som du har definierat. Du lägger till ett jobb för indata, utdata och omvandling toohello senare.

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


## <a name="create-a-stream-analytics-input-source"></a>Skapa en inkommande Stream Analytics-källa
hello skapar följande kod en Stream Analytics indatakälla hello blob indatakälla typ och CSV-serialisering. toocreate en hub inkommande händelsekälla, Använd **EventHubStreamInputDataSource** i stället för **BlobStreamInputDataSource**. På liknande sätt kan du anpassa hello serialiseringstyp av indatakälla hello.

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

Indatakällor, är från Blob-lagring eller en händelsehubb bundet tooa specifikt jobb. toouse Hej samma källa för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.

## <a name="test-a-stream-analytics-input-source"></a>Testa en Stream Analytics indatakälla
Hej **TestConnection** metoden tester om hello Stream Analytics-jobbet är kan tooconnect toohello Ange källa samt andra aspekter specifika toohello ange typen av datakälla. Till exempel i hello blob indatakälla du skapade tidigare, kontrollerar hello metoden att hello lagringskontonamn och en nyckel kan vara används tooconnect toohello lagringskonto samt kontrollera den angivna hello-behållaren finns.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Skapa ett mål för Stream Analytics-utdata
Att skapa ett mål för utdata är mycket lik toocreating en Stream Analytics-Indatakällan. Precis som indatakällor är utdata mål bundet tooa specifikt jobb. toouse Hej samma utdata mål för olika jobb, måste du anropa metoden hello igen och ange ett annat jobbnamn.

hello följande kod skapar ett utgående mål (Azure SQL database). Du kan anpassa hello utdata mål datatyp och/eller serialiseringstyp.

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

## <a name="test-a-stream-analytics-output-target"></a>Testa ett mål för Stream Analytics-utdata
Ett Stream Analytics utdata mål har också hello **TestConnection** metod för att testa anslutningar.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Skapa Stream Analytics
hello följande kod skapar en Stream Analytics-omvandling med hello frågan ”Välj * från indata” och anger tooallocate en enhet för strömning hello Stream Analytics-jobbet. Läs mer om hur du justerar strömningsenheter [skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md).

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

En omvandling är också bundet toohello specifika Stream Analytics-jobbet den skapades under som indata och utdata.

## <a name="start-a-stream-analytics-job"></a>Starta ett Stream Analytics-jobb
När du har skapat ett Stream Analytics-jobb och dess input(s), utdata och omvandling av du kan starta hello jobbet genom att anropa hello **starta** metod.

hello följande exempelkod startar en Stream Analytics-jobbet med en anpassad utdata start tid set tooDecember 12, 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);

## <a name="stop-a-stream-analytics-job"></a>Stoppa ett Stream Analytics-jobb
Du kan stoppa ett Stream Analytics-jobb som körs med anropa hello **stoppa** metod.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Ta bort ett Stream Analytics-jobb
Hej **ta bort** metoden tar bort hello projekt samt hello underliggande underordnade resurser, inklusive input(s), utdata och omvandling av hello jobb.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);

## <a name="get-support"></a>Få support
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
Du har lärt dig hello grunderna i en .NET SDK-toocreate och kör analytics-jobb. toolearn finns fler hello följande:

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
