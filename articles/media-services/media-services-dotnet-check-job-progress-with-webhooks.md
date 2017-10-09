---
title: aaaUse Azure WebHooks toomonitor Media Services jobbet meddelanden med .NET | Microsoft Docs
description: "Lär dig hur toouse Azure WebHooks toomonitor Media Services jobbet meddelanden. hello kodexempel är skrivna i C# och använder hello Media Services SDK för .NET."
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
ms.date: 03/06/2017
ms.author: juliako
ms.openlocfilehash: b7df597da20e551cb2a02cd21c96c7bddf9e1a66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-webhooks-toomonitor-media-services-job-notifications-with-net"></a>Använda Azure WebHooks toomonitor Media Services jobbet meddelanden med .NET
När du kör jobb behöver du ofta ett sätt tootrack jobb pågår. Du kan övervaka Media Services jobbet meddelanden med hjälp av Azure Webhooks eller [Azure Queue storage](media-services-dotnet-check-job-progress-with-queues.md). Det här avsnittet visar hur toowork med Webhooks.

## <a name="prerequisites"></a>Krav

hello följande är obligatoriska toocomplete hello kursen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ett Media Services-konto. toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).
* Förstå [hur toouse Azure functions](../azure-functions/functions-overview.md). Granska även [Azure functions HTTP och webhook bindningar](../azure-functions/functions-bindings-http-webhook.md).

Det här avsnittet beskrivs hur du

*  Definiera en Azure-funktion som är anpassade toorespond toowebhooks. 
    
    I det här fallet utlöses hello webhook av Media Services när din kodningsjobbet ändrar status. hello funktionen lyssnar efter hello webhook-anrop från Media Services-meddelanden och publicerar hello utdatatillgången när hello jobbet har slutförts. 
    
    >[!NOTE]
    >Innan du fortsätter bör du kontrollera att du förstår hur [Azure Functions HTTP och webhook bindningar](../azure-functions/functions-bindings-http-webhook.md) fungerar.
    >
    
* Lägga till en webhook tooyour kodning aktivitet och ange hello Webhooksadressen och hemlig nyckel som denna webhook svarar. Hello-kod som skapar hello kodning uppgift är en konsolapp i hello exemplet.

## <a name="setting-up-webhook-notification-azure-functions"></a>Ställa in ”webhook notification” Azure functions

hello kod i det här avsnittet visar en implementering av en Azure-funktion som är en webhook. I det här exemplet hello funktionen lyssnar efter hello webhook-anrop från Media Services-meddelanden och publicerar hello utdatatillgången när hello jobbet har slutförts.

Hej webhook förväntar sig en signering nyckel (autentiseringsuppgifter) toomatch hello något du skickar när du konfigurerar hello aviseringsslutpunkten. hello signeringsnyckel är hello 64 byte Base64-kodade värde som har använt tooprotect och skydda WebHooks-återanrop från Azure Media Services. 

I följande kod hello, hello **VerifyWebHookRequestSignature** metod hello verifiering på hello-meddelande. hello syftet med den här verifieringen är tooensure som hello meddelandet skickades av Azure Media Services och inte har ändrats. hello signaturen är valfritt för Azure functions eftersom den har hello **kod** värdet som en frågeparameter över Transport Layer Security (TLS). 

Du kan hitta hello definition av olika Media Services .NET Azure-funktioner (inklusive hello som visas i det här avsnittet) [här](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).

hello följande kodexempel visar hello definitioner av Azure funktionsparametrar och tre filer som är associerade med hello Azure-funktion: function.json, project.json och run.csx.

### <a name="application-settings"></a>Programinställningar 

hello visar följande tabell hello-parametrar som används av hello Azure funktion som definierats i det här avsnittet. 

|Namn|Definition|Exempel| 
|---|---|---|
|AMSAccount|AMS namnet på ditt konto. |juliakomediaservices|
|AMSKey |Din nyckel för AMS-kontot. | JUWJdDaOHQQqsZeiXZuE76eDt2SO + YMJk25Lghgy2nY =|
|MediaServicesStorageAccountName |Ett namn på hello storage-konto som är associerad med AMS-kontot.| storagepkeewmg5c3peq|
|MediaServicesStorageAccountKey |En nyckel för hello storage-konto som är associerad med AMS-kontot.|
|SigningKey |Signeringsnyckeln.| j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt|
|WebHookEndpoint | En webhook slutpunktsadress. | https://juliakofuncapp.azurewebsites.NET/API/Notification_Webhook_Function?Code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.|

### <a name="functionjson"></a>Function.JSON

Hej function.json filen definierar hello funktionsbindningar och andra konfigurationsinställningar. hello runtime använder den här filen toodetermine hello händelser toomonitor och hur toopass data till och returnera data från att fungera körning. 

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [
        "post",
        "get",
        "put",
        "update",
        "patch"
          ]
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
    
### <a name="projectjson"></a>Project.JSON

Hej project.json filen innehåller beroenden. 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a>Run.csx

hello innehåller följande C#-kod en definition av en Azure-funktion som är en webhook. hello funktionen lyssnar efter hello webhook-anrop från Media Services-meddelanden och publicerar hello utdatatillgången när hello jobbet har slutförts. 


>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

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

    internal const string SignatureHeaderKey = "sha256";
    internal const string SignatureHeaderValueTemplate = SignatureHeaderKey + "={0}";
    static string _webHookEndpoint = Environment.GetEnvironmentVariable("WebHookEndpoint");
    static string _signingKey = Environment.GetEnvironmentVariable("SigningKey");
    static string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    static string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

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
                _context = new CloudMediaContext(new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey));

                if(_context!=null)   
                {                        
                string urlForClientStreaming = PublishAndBuildStreamingURLs(msg.Properties["JobId"]);
                log.Info($"URL toohello manifest for client streaming using HLS protocol: {urlForClientStreaming}");
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

        // Create a locator toohello streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy,
        DateTime.UtcNow.AddMinutes(-5));


        // Get a reference toohello streaming manifest file from hello  
        // collection of files in hello asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                    EndsWith(".ism")).
                    FirstOrDefault();

        // Create a full URL toohello manifest file. Use this for playback
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
    /// Converts a <see cref="T:byte[]"/> tooa hex-encoded string.
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

### <a name="function-output"></a>Funktionen utdata

hello-exemplet ovan producerats hello följande utdata, värdena kan variera.

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
    
    URL toohello manifest for client streaming using HLS protocol: http://mediapkeewmg5c3peq.streaming.mediaservices.windows.net/0ac98077-2b58-4db7-a8da-789a13ac6167/BigBuckBunny.ism/manifest(format=m3u8-aapl)

## <a name="adding-webhook-tooyour-encoding-task"></a>Lägga till Webhook tooyour kodning aktivitet

I det här avsnittet visas hello-kod som lägger till en webhook meddelande tooa aktivitet. Du kan också lägga till en nivå indikering jobb, som är mer användbar för ett projekt med länkad uppgifter.  

1. Skapa ett nytt C#-konsolprogram i Visual Studio. Ange hello namn, plats och lösningen och klicka sedan på OK.
2. Använd [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) tooinstall Azure Media Services.
3. Uppdatera App.config-fil med lämpliga värden: 
    
    * Azure Media Services namn och nyckel som kommer att skicka meddelanden 
    * Webhooksadressen som förväntar sig tooget hello meddelanden 
    * hello signeringsnyckel som matchar hello-nyckel som din webhook förväntas. hello signeringsnyckel är hello 64 byte Base64-kodade värde som har använt tooprotect och skydda WebHooks-återanrop från Azure Media Services. 

            <appSettings>
              <add key="MediaServicesAccountName" value="AMSAcctName" />
              <add key="MediaServicesAccountKey" value="AMSAcctKey" />
              <add key="WebhookURL" value="https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>" />
              <add key="WebhookSigningKey" value="j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt" />
            </appSettings>
            
4. Uppdatera filen Program.cs med följande kod hello:

        using System;
        using System.Configuration;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace NotificationWebHook
        {
            class Program
            {
            // Read values from hello App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            private static readonly string _webHookEndpoint =
                ConfigurationManager.AppSettings["WebhookURL"];
            private static readonly string _signingKey =
                 ConfigurationManager.AppSettings["WebhookSigningKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {

                // Used hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(new MediaServicesCredentials(
                        _mediaServicesAccountName,
                        _mediaServicesAccountKey));

                byte[] keyBytes = Convert.FromBase64String(_signingKey);

                IAsset newAsset = _context.Assets.FirstOrDefault();

                // Check for existing Notification Endpoint with hello name "FunctionWebHook"

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

                // Declare a new encoding job with hello Standard encoder
                IJob job = _context.Jobs.Create("MES Job");

                // Get a media processor reference, and pass tooit hello name of hello 
                // processor toouse for hello specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify hello input asset toobe encoded.
                task.InputAssets.Add(newAsset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is not encrypted. 
                task.OutputAssets.AddNew(newAsset.Name, AssetCreationOptions.None);

                // Add hello WebHook notification toothis Task and request all notification state changes.
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

                Console.WriteLine("Expect WebHook toobe triggered for hello Job ID: {0}", job.Id);
                Console.WriteLine("Expect WebHook toobe triggered for hello Task ID: {0}", task.Id);

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

## <a name="next-step"></a>Nästa steg
Granska utbildningsvägar för Media Services

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
