---
title: "Använda Azure Webhooks för att övervaka Media Services jobbet meddelanden med .NET | Microsoft Docs"
description: "Lär dig hur du använder Azure Webhooks för att övervaka Media Services jobbet meddelanden. Kodexemplet är skrivna i C# och använder Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a61fe157-81b1-45c1-89f2-224b7ef55869
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 9815e01dffb0342979f17974527b559de8146fed
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-webhooks-to-monitor-media-services-job-notifications-with-net"></a>Använd Azure Webhooks att övervaka Media Services jobbet meddelanden med .NET
När du kör jobb behöver du ofta ett sätt att spåra jobbförloppet. Du kan övervaka Media Services jobbet meddelanden med hjälp av Azure Webhooks eller [Azure Queue storage](media-services-dotnet-check-job-progress-with-queues.md). Den här artikeln visar hur du arbetar med webhooks.

Den här artikeln visar hur du

*  Definiera en Azure-funktion som är anpassad för att svara på webhooks. 
    
    I det här fallet utlöses webhooken av Media Services när din kodningsjobbet ändrar status. Funktionen lyssnar efter webhook-anrop från Media Services-meddelanden och publicerar utdatatillgången när jobbet har slutförts. 
    
    >[!NOTE]
    >Innan du fortsätter bör du kontrollera att du förstår hur [Azure Functions HTTP och webhook bindningar](../azure-functions/functions-bindings-http-webhook.md) fungerar.
    >
    
* Lägga till en webhook kodning uppgiften och ange Webhooksadressen och hemlig nyckel som denna webhook svarar. Du hittar ett exempel som lägger tillför en webhook kodning uppgiften i slutet av artikeln.  

Du kan hitta definitioner av olika Media Services .NET Azure-funktioner (inklusive det som visas i den här artikeln) [här](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).

## <a name="prerequisites"></a>Krav

Följande krävs för att kunna genomföra vägledningen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ett Media Services-konto. Information om hur du skapar ett Media Services-konto finns i [Så här skapar du ett Media Services-konto](media-services-portal-create-account.md).
* Förstå [hur du använder Azure Functions](../azure-functions/functions-overview.md). Granska även [Azure Functions HTTP och webhook bindningar](../azure-functions/functions-bindings-http-webhook.md).

## <a name="create-a-function-app"></a>Skapa en funktionsapp

1. Gå till [Azure Portal](http://portal.azure.com) och logga in med ditt Azure-konto.
2. Skapa en funktionsapp enligt [här](../azure-functions/functions-create-function-app-portal.md).

## <a name="configure-function-app-settings"></a>Konfigurera funktionen app-inställningar

Det är praktiskt att lägga till miljövariabler som används i hela din funktioner när du utvecklar Media Services-funktioner. Klicka på länken konfigurera App-inställningar om du vill konfigurera inställningar för appen. 

Den [programinställningar](media-services-dotnet-how-to-use-azure-functions.md#configure-function-app-settings) avsnittet definierar parametrar som används i webhooken som definierats i den här artikeln. Också lägga till följande parametrar i app-inställningar. 

|Namn|Definition|Exempel| 
|---|---|---|
|SigningKey |Signeringsnyckeln.| j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt|
|WebHookEndpoint | En webhook slutpunktsadress. När webhook-funktionen har skapats kan du kopiera URL: en från den **få funktionen URL** länk. | https://juliakofuncapp.azurewebsites.NET/API/Notification_Webhook_Function?Code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.|

## <a name="create-a-function"></a>Skapa en funktion

När funktionen appen har distribuerats, du kan hitta den bland **Apptjänster** Azure Functions.

1. Välj funktionen appen och klicka på **nya funktionen**.
2. Välj **C#** koden och **API & Webhooks** scenario. 
3. Välj **allmän Webhook - C#**.
4. Namnge din webhook och tryck på **skapa**.

### <a name="files"></a>Filer

Din Azure-funktion är associerad med kod och andra filer som beskrivs i det här avsnittet. Som standard, en funktion som är associerad med **function.json** och **run.csx** (C#)-filer. Du måste lägga till en **project.json** fil. Resten av det här avsnittet innehåller definitioner för de här filerna.

![Filer](./media/media-services-azure-functions/media-services-azure-functions003.png)

#### <a name="functionjson"></a>Function.JSON

Filen function.json definierar bindningarna som funktionen och andra konfigurationsinställningar. Körningsmiljön använder den här filen för att fastställa händelser att övervaka och hur du överför data till och returnera data från funktionen körning. 

```
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

#### <a name="projectjson"></a>Project.JSON

Filen project.json innehåller beroenden. 

```
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}
```
    
#### <a name="runcsx"></a>Run.csx

Koden i det här avsnittet visar en implementering av en Azure-funktion som är en webhook. I det här exemplet funktionen lyssnar efter webhook-anrop från Media Services-meddelanden och publicerar utdatatillgången när jobbet har slutförts.

Webhooken förväntar signeringsnyckeln (autentiseringsuppgifter) så att den matchar den som du skickar när du konfigurerar aviseringsslutpunkten. Signeringsnyckeln är Base64-kodade 64-byte-värde som används för att skydda och skydda WebHooks-återanrop från Azure Media Services. 

I webhook definition koden som kommer efter den **VerifyWebHookRequestSignature** metod har verifiering av meddelandet. Syftet med den här verifieringen är att säkerställa att meddelandet skickades av Azure Media Services och inte har ändrats. Signaturen är valfri för Azure Functions eftersom den har den **kod** värdet som en frågeparameter över Transport Layer Security (TLS). 

>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

```
///////////////////////////////////////////////////
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Globalization;
using Newtonsoft.Json;
using Microsoft.Azure;
using System.Net;
using System.Security.Cryptography;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

internal const string SignatureHeaderKey = "sha256";
internal const string SignatureHeaderValueTemplate = SignatureHeaderKey + "={0}";
static string _webHookEndpoint = Environment.GetEnvironmentVariable("WebHookEndpoint");
static string _signingKey = Environment.GetEnvironmentVariable("SigningKey");

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");

static readonly string _AMSClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _AMSClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static CloudMediaContext _context = null;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    Task<byte[]> taskForRequestBody = req.Content.ReadAsByteArrayAsync();
    byte[] requestBody = await taskForRequestBody;

    string jsonContent = await req.Content.ReadAsStringAsync();
    log.Info($"Request Body = {jsonContent}");

    IEnumerable<string> values = null;
    if (req.Headers.TryGetValues("ms-signature", out values))
    {
        byte[] signingKey = Convert.FromBase64String(_signingKey);
        string signatureFromHeader = values.FirstOrDefault();

        if (VerifyWebHookRequestSignature(requestBody, signatureFromHeader, signingKey))
        {
            string requestMessageContents = Encoding.UTF8.GetString(requestBody);

            NotificationMessage msg = JsonConvert.DeserializeObject<NotificationMessage>(requestMessageContents);

            if (VerifyHeaders(req, msg, log))
            { 
                string newJobStateStr = (string)msg.Properties.Where(j => j.Key == "NewState").FirstOrDefault().Value;
                if (newJobStateStr == "Finished")
                {
                    AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                                new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                                AzureEnvironments.AzureCloudEnvironment);

                    AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    if(_context!=null)   
                    {                        
                        string urlForClientStreaming = PublishAndBuildStreamingURLs(msg.Properties["JobId"]);
                        log.Info($"URL to the manifest for client streaming using HLS protocol: {urlForClientStreaming}");
                    }
                }

                return req.CreateResponse(HttpStatusCode.OK, string.Empty);
            }
            else
            {
                log.Info($"VerifyHeaders failed.");
                return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyHeaders failed.");
            }
        }
        else
        {
            log.Info($"VerifyWebHookRequestSignature failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyWebHookRequestSignature failed.");
        }
    }

    return req.CreateResponse(HttpStatusCode.BadRequest, "Generic Error.");
}

private static string PublishAndBuildStreamingURLs(String jobID)
{
    IJob job = _context.Jobs.Where(j => j.Id == jobID).FirstOrDefault();
    IAsset asset = job.OutputMediaAssets.FirstOrDefault();

    // Create a 30-day readonly access policy. 
    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
    TimeSpan.FromDays(30),
    AccessPermissions.Read);

    // Create a locator to the streaming content on an origin. 
    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
    policy,
    DateTime.UtcNow.AddMinutes(-5));

    // Get a reference to the streaming manifest file from the  
    // collection of files in the asset. 
    var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

    // Create a full URL to the manifest file. Use this for playback
    // in streaming media clients. 
    string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest" +  "(format=m3u8-aapl)";
    return urlForClientStreaming;

}

private static bool VerifyWebHookRequestSignature(byte[] data, string actualValue, byte[] verificationKey)
{
    using (var hasher = new HMACSHA256(verificationKey))
    {
        byte[] sha256 = hasher.ComputeHash(data);
        string expectedValue = string.Format(CultureInfo.InvariantCulture, SignatureHeaderValueTemplate, ToHex(sha256));

        return (0 == String.Compare(actualValue, expectedValue, System.StringComparison.Ordinal));
    }
}

private static bool VerifyHeaders(HttpRequestMessage req, NotificationMessage msg, TraceWriter log)
{
    bool headersVerified = false;

    try
    {
        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-mediaservices-accountid", out values))
        {
            string accountIdHeader = values.FirstOrDefault();
            string accountIdFromMessage = msg.Properties["AccountId"];

            if (0 == string.Compare(accountIdHeader, accountIdFromMessage, StringComparison.OrdinalIgnoreCase))
            {
                headersVerified = true;
            }
            else
            {
                log.Info($"accountIdHeader={accountIdHeader} does not match accountIdFromMessage={accountIdFromMessage}");
            }
        }
        else
        {
            log.Info($"Header ms-mediaservices-accountid not found.");
        }
    }
    catch (Exception e)
    {
        log.Info($"VerifyHeaders hit exception {e}");
        headersVerified = false;
    }

    return headersVerified;
}

private static readonly char[] HexLookup = new char[] { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F' };

/// <summary>
/// Converts a <see cref="T:byte[]"/> to a hex-encoded string.
/// </summary>
private static string ToHex(byte[] data)
{
    if (data == null)
    {
        return string.Empty;
    }

    char[] content = new char[data.Length * 2];
    int output = 0;
    byte d;

    for (int input = 0; input < data.Length; input++)
    {
        d = data[input];
        content[output++] = HexLookup[d / 0x10];
        content[output++] = HexLookup[d % 0x10];
    }

    return new string(content);
}

internal enum NotificationEventType
{
    None = 0,
    JobStateChange = 1,
    NotificationEndPointRegistration = 2,
    NotificationEndPointUnregistration = 3,
    TaskStateChange = 4,
    TaskProgress = 5
}

internal sealed class NotificationMessage
{
    public string MessageVersion { get; set; }
    public string ETag { get; set; }
    public NotificationEventType EventType { get; set; }
    public DateTime TimeStamp { get; set; }
    public IDictionary<string, string> Properties { get; set; }
}
```

Spara och kör din funktion.

### <a name="function-output"></a>Funktionen utdata

När webhooken utlöses exemplet ovan skapar följande utdata, värdena kan variera.

    C# HTTP trigger function processed a request. RequestUri=https://juliako001-functions.azurewebsites.net/api/Notification_Webhook_Function?code=9376d69kygoy49oft81nel8frty5cme8hb9xsjslxjhalwhfrqd79awz8ic4ieku74dvkdfgvi
    Request Body = {
      "MessageVersion": "1.1",
      "ETag": "b8977308f48858a8f224708bc963e1a09ff917ce730316b4e7ae9137f78f3b20",
      "EventType": 4,
      "TimeStamp": "2017-02-16T03:59:53.3041122Z",
      "Properties": {
        "JobId": "nb:jid:UUID:badd996c-8d7c-4ae0-9bc1-bd7f1902dbdd",
        "TaskId": "nb:tid:UUID:80e26fb9-ee04-4739-abd8-2555dc24639f",
        "NewState": "Finished",
        "OldState": "Processing",
        "AccountName": "mediapkeewmg5c3peq",
        "AccountId": "301912b0-659e-47e0-9bc4-6973f2be3424",
        "NotificationEndPointId": "nb:nepid:UUID:cb5d707b-4db8-45fe-a558-19f8d3306093"
      }
    }
    
    URL to the manifest for client streaming using HLS protocol: http://mediapkeewmg5c3peq.streaming.mediaservices.windows.net/0ac98077-2b58-4db7-a8da-789a13ac6167/BigBuckBunny.ism/manifest(format=m3u8-aapl)

## <a name="add-a-webhook-to-your-encoding-task"></a>Lägg till en webhook kodning uppgiften

I det här avsnittet visas den kod som lägger tillför ett webhook-meddelande till en aktivitet. Du kan också lägga till en nivå indikering jobb, som är mer användbar för ett projekt med länkad uppgifter.  

1. Skapa ett nytt C#-konsolprogram i Visual Studio. Ange namn, plats och lösningen och klicka sedan på OK.
2. Använd [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) att installera Azure Media Services.
3. Uppdatera App.config-fil med lämpliga värden: 
    
    * Azure Media Services anslutningsinformation 
    * Webhooksadressen som förväntar sig att hämta meddelanden, 
    * signeringsnyckeln som matchar nyckeln som din webhook förväntas. Signeringsnyckeln är Base64-kodade 64-byte-värde som används för att skydda och skydda webhooks-återanrop från Azure Media Services. 

            <appSettings>
              <add key="AMSAADTenantDomain" value="domain" />
              <add key="AMSRESTAPIEndpoint" value="endpoint" />

              <add key="AMSClientId" value="clinet id" />
              <add key="AMSClientSecret" value="client secret" />

              <add key="WebhookURL" value="https://yourapp.azurewebsites.net/api/functionname?code=ApiKey" />
              <add key="WebhookSigningKey" value="j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt" />
            </appSettings>

4. Uppdatera filen Program.cs med följande kod:

        using System;
        using System.Configuration;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace NotificationWebHook
        {
            class Program
            {
            // Read values from the App.config file.
            private static readonly string _AMSAADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _AMSRESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];

            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];

            private static readonly string _webHookEndpoint =
                ConfigurationManager.AppSettings["WebhookURL"];
            private static readonly string _signingKey =
                 ConfigurationManager.AppSettings["WebhookSigningKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AMSAADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

                AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_AMSRESTAPIEndpoint), tokenProvider);

                byte[] keyBytes = Convert.FromBase64String(_signingKey);

                IAsset newAsset = _context.Assets.FirstOrDefault();

                // Check for existing Notification Endpoint with the name "FunctionWebHook"

                var existingEndpoint = _context.NotificationEndPoints.Where(e => e.Name == "FunctionWebHook").FirstOrDefault();
                INotificationEndPoint endpoint = null;

                if (existingEndpoint != null)
                {
                Console.WriteLine("webhook endpoint already exists");
                endpoint = (INotificationEndPoint)existingEndpoint;
                }
                else
                {
                endpoint = _context.NotificationEndPoints.Create("FunctionWebHook",
                    NotificationEndPointType.WebHook, _webHookEndpoint, keyBytes);
                Console.WriteLine("Notification Endpoint Created with Key : {0}", keyBytes.ToString());
                }

                // Declare a new encoding job with the Standard encoder
                IJob job = _context.Jobs.Create("MES Job");

                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(newAsset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew(newAsset.Name, AssetCreationOptions.None);

                // Add the WebHook notification to this Task and request all notification state changes.
                // Note that you can also add a job level notification
                // which would be more useful for a job with chained tasks.  
                if (endpoint != null)
                {
                task.TaskNotificationSubscriptions.AddNew(NotificationJobState.All, endpoint, true);
                Console.WriteLine("Created Notification Subscription for endpoint: {0}", _webHookEndpoint);
                }
                else
                {
                Console.WriteLine("No Notification Endpoint is being used");
                }

                job.Submit();

                Console.WriteLine("Expect WebHook to be triggered for the Job ID: {0}", job.Id);
                Console.WriteLine("Expect WebHook to be triggered for the Task ID: {0}", task.Id);

                Console.WriteLine("Job Submitted");

            }
            private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                return processor;
            }
            }
        }

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
