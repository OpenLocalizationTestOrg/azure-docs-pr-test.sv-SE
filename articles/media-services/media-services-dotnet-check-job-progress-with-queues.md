---
title: aaaUse Azure Queue storage toomonitor Media Services-jobbet meddelanden med .NET | Microsoft Docs
description: "Lär dig hur toouse Azure Queue storage toomonitor Media Services jobbet meddelanden. hello kodexempel är skrivna i C# och använder hello Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: f535d0b5-f86c-465f-81c6-177f4f490987
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/14/2017
ms.author: juliako
ms.openlocfilehash: e4068621ada00d763133dc0d01cfc666b53f8b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-queue-storage-toomonitor-media-services-job-notifications-with-net"></a><span data-ttu-id="57960-104">Använda Azure Queue storage toomonitor Media Services jobbet meddelanden med .NET</span><span class="sxs-lookup"><span data-stu-id="57960-104">Use Azure Queue storage toomonitor Media Services job notifications with .NET</span></span>
<span data-ttu-id="57960-105">När du kör kodning jobb kräver ofta en sätt tootrack jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="57960-105">When you run encoding jobs, you often require a way tootrack job progress.</span></span> <span data-ttu-id="57960-106">Du kan konfigurera aviseringar för Media Services toodeliver för[Azure Queue storage](../storage/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="57960-106">You can configure Media Services toodeliver notifications too[Azure Queue storage](../storage/storage-dotnet-how-to-use-queues.md).</span></span> <span data-ttu-id="57960-107">Du kan övervaka jobbförloppet genom att hämta meddelanden från hello Queue storage.</span><span class="sxs-lookup"><span data-stu-id="57960-107">You can monitor job progress by getting notifications from hello Queue storage.</span></span> 

<span data-ttu-id="57960-108">Meddelanden som har levererats tooQueue storage kan nås från var som helst i hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="57960-108">Messages delivered tooQueue storage can be accessed from anywhere in hello world.</span></span> <span data-ttu-id="57960-109">hello kön meddelanden lagringsarkitektur är tillförlitliga och skalbara.</span><span class="sxs-lookup"><span data-stu-id="57960-109">hello Queue storage messaging architecture is reliable and highly scalable.</span></span> <span data-ttu-id="57960-110">Avsökning Queue storage för meddelanden rekommenderas framför andra metoder.</span><span class="sxs-lookup"><span data-stu-id="57960-110">Polling Queue storage for messages is recommended over using other methods.</span></span>

<span data-ttu-id="57960-111">Ett vanligt scenario för lyssnande tooMedia Services meddelanden är om du utvecklar ett system för innehållshantering som måste tooperform när ett kodningsjobb vissa ytterligare aktiviteten har slutförts (till exempel tootrigger hello nästa steg i ett arbetsflöde eller toopublish innehåll).</span><span class="sxs-lookup"><span data-stu-id="57960-111">One common scenario for listening tooMedia Services notifications is if you are developing a content management system that needs tooperform some additional task after an encoding job completes (for example, tootrigger hello next step in a workflow, or toopublish content).</span></span>

<span data-ttu-id="57960-112">Det här avsnittet visar hur tooget meddelande meddelanden från Queue storage.</span><span class="sxs-lookup"><span data-stu-id="57960-112">This topic shows how tooget notification messages from Queue storage.</span></span>  

## <a name="considerations"></a><span data-ttu-id="57960-113">Överväganden</span><span class="sxs-lookup"><span data-stu-id="57960-113">Considerations</span></span>
<span data-ttu-id="57960-114">Tänk hello följande när du utvecklar Media Services-program som använder Queue storage:</span><span class="sxs-lookup"><span data-stu-id="57960-114">Consider hello following when developing Media Services applications that use Queue storage:</span></span>

* <span data-ttu-id="57960-115">Queue storage tillhandahåller inte av först in first out (FIFO) sorterade leverans.</span><span class="sxs-lookup"><span data-stu-id="57960-115">Queue storage does not provide a guarantee of first-in-first-out (FIFO) ordered delivery.</span></span> <span data-ttu-id="57960-116">Mer information finns i [Azure köer och Azure Service Bus-köer jämfört med och Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).</span><span class="sxs-lookup"><span data-stu-id="57960-116">For more information, see [Azure Queues and Azure Service Bus Queues Compared and Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).</span></span>
* <span data-ttu-id="57960-117">Queue storage är inte en push-tjänst.</span><span class="sxs-lookup"><span data-stu-id="57960-117">Queue storage is not a push service.</span></span> <span data-ttu-id="57960-118">Du har toopoll hello kön.</span><span class="sxs-lookup"><span data-stu-id="57960-118">You have toopoll hello queue.</span></span>
* <span data-ttu-id="57960-119">Du kan ha valfritt antal köer.</span><span class="sxs-lookup"><span data-stu-id="57960-119">You can have any number of queues.</span></span> <span data-ttu-id="57960-120">Mer information finns i [kön Service REST API](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="57960-120">For more information, see [Queue Service REST API](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API).</span></span>
* <span data-ttu-id="57960-121">Queue storage har några begränsningar och krav toobe medveten om.</span><span class="sxs-lookup"><span data-stu-id="57960-121">Queue storage has some limitations and specifics toobe aware of.</span></span> <span data-ttu-id="57960-122">Dessa beskrivs i [Azure köer och Azure Service Bus-köer jämfört med och Contrasted](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).</span><span class="sxs-lookup"><span data-stu-id="57960-122">These are described in [Azure Queues and Azure Service Bus Queues Compared and Contrasted](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).</span></span>

## <a name="net-code-example"></a><span data-ttu-id="57960-123">.NET-kodexempel</span><span class="sxs-lookup"><span data-stu-id="57960-123">.NET code example</span></span>

<span data-ttu-id="57960-124">hello kodexempel i det här avsnittet hello följande:</span><span class="sxs-lookup"><span data-stu-id="57960-124">hello code example in this section does hello following:</span></span>

1. <span data-ttu-id="57960-125">Definierar hello **EncodingJobMessage** klass som mappar toohello meddelande meddelandeformat.</span><span class="sxs-lookup"><span data-stu-id="57960-125">Defines hello **EncodingJobMessage** class that maps toohello notification message format.</span></span> <span data-ttu-id="57960-126">hello kod deserializes meddelanden från kön hello i objekt av hello **EncodingJobMessage** typen.</span><span class="sxs-lookup"><span data-stu-id="57960-126">hello code deserializes messages received from hello queue into objects of hello **EncodingJobMessage** type.</span></span>
2. <span data-ttu-id="57960-127">Belastningar hello Media Services och information om Lagringskonto från hello app.config-fil.</span><span class="sxs-lookup"><span data-stu-id="57960-127">Loads hello Media Services and Storage account information from hello app.config file.</span></span> <span data-ttu-id="57960-128">hello kodexempel används denna information toocreate hello **CloudMediaContext** och **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="57960-128">hello code example uses this information toocreate hello **CloudMediaContext** and **CloudQueue** objects.</span></span>
3. <span data-ttu-id="57960-129">Skapar hello kö som tar emot meddelanden om hello kodning jobb.</span><span class="sxs-lookup"><span data-stu-id="57960-129">Creates hello queue that receives notification messages about hello encoding job.</span></span>
4. <span data-ttu-id="57960-130">Skapar hello-meddelande slutpunkt som är mappad toohello kön.</span><span class="sxs-lookup"><span data-stu-id="57960-130">Creates hello notification end point that is mapped toohello queue.</span></span>
5. <span data-ttu-id="57960-131">Bifogar hello notification slutpunkt toohello jobb och skickar hello kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="57960-131">Attaches hello notification end point toohello job and submits hello encoding job.</span></span> <span data-ttu-id="57960-132">Du kan ha flera notification slutpunkter kopplade tooa jobb.</span><span class="sxs-lookup"><span data-stu-id="57960-132">You can have multiple notification end points attached tooa job.</span></span>
6. <span data-ttu-id="57960-133">Överför **NotificationJobState.FinalStatesOnly** toohello **AddNew** metod.</span><span class="sxs-lookup"><span data-stu-id="57960-133">Passes **NotificationJobState.FinalStatesOnly** toohello **AddNew** method.</span></span> <span data-ttu-id="57960-134">(I det här exemplet är det bara är intresserad sluttillstånd hello jobbet bearbetning.)</span><span class="sxs-lookup"><span data-stu-id="57960-134">(In this example, we are only interested in final states of hello job processing.)</span></span>

        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
7. <span data-ttu-id="57960-135">Om du skickar **NotificationJobState.All**, du får alla hello efter aviseringar om tillståndsändringar: köade schemalagda, bearbetning och klar.</span><span class="sxs-lookup"><span data-stu-id="57960-135">If you pass **NotificationJobState.All**, you get all of hello following state change notifications: queued, scheduled, processing, and finished.</span></span> <span data-ttu-id="57960-136">Dock som tidigare nämnts garanterar Queue storage inte begärd leverans.</span><span class="sxs-lookup"><span data-stu-id="57960-136">However, as noted earlier, Queue storage does not guarantee ordered delivery.</span></span> <span data-ttu-id="57960-137">tooorder meddelanden använder hello **tidsstämpel** egenskapen (definieras på hello **EncodingJobMessage** typen i hello exemplet nedan).</span><span class="sxs-lookup"><span data-stu-id="57960-137">tooorder messages, use hello **Timestamp** property (defined on hello **EncodingJobMessage** type in hello example below).</span></span> <span data-ttu-id="57960-138">Dubbla meddelanden är möjliga.</span><span class="sxs-lookup"><span data-stu-id="57960-138">Duplicate messages are possible.</span></span> <span data-ttu-id="57960-139">toocheck dubbletter, Använd hello **ETag-egenskapen** (definieras på hello **EncodingJobMessage** typ).</span><span class="sxs-lookup"><span data-stu-id="57960-139">toocheck for duplicates, use hello **ETag property** (defined on hello **EncodingJobMessage** type).</span></span> <span data-ttu-id="57960-140">Det är också möjligt att vissa aviseringar om tillståndsändringar hämta hoppas över.</span><span class="sxs-lookup"><span data-stu-id="57960-140">It is also possible that some state change notifications get skipped.</span></span>
8. <span data-ttu-id="57960-141">Väntar på hello jobbet tooget toohello slutfört tillstånd genom att kontrollera hello kön var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="57960-141">Waits for hello job tooget toohello finished state by checking hello queue every 10 seconds.</span></span> <span data-ttu-id="57960-142">Tar bort meddelanden när de har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="57960-142">Deletes messages after they have been processed.</span></span>
9. <span data-ttu-id="57960-143">Tar bort hello kön och slutpunkten för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="57960-143">Deletes hello queue and hello notification end point.</span></span>

> [!NOTE]
> <span data-ttu-id="57960-144">Hej rekommenderade sättet toomonitor en jobbets status är lyssnande toonotification meddelanden som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="57960-144">hello recommended way toomonitor a job’s state is by listening toonotification messages, as shown in hello following example.</span></span>
>
> <span data-ttu-id="57960-145">Du kan också du kan kontrollera på tillstånd för ett jobb med hjälp av hello **IJob.State** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="57960-145">Alternatively, you could check on a job’s state by using hello **IJob.State** property.</span></span>  <span data-ttu-id="57960-146">Ett meddelande om en jobbet har slutförts kan tas emot innan hello tillstånd på **IJob** har angetts för**avslutad**.</span><span class="sxs-lookup"><span data-stu-id="57960-146">A notification message about a job’s completion may arrive before hello state on **IJob** is set too**Finished**.</span></span> <span data-ttu-id="57960-147">Hej **IJob.State** egenskapen visar hello korrekt tillstånd med en viss fördröjning.</span><span class="sxs-lookup"><span data-stu-id="57960-147">hello **IJob.State**  property reflects hello accurate state with a slight delay.</span></span>
>
>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="57960-148">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="57960-148">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="57960-149">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="57960-149">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="57960-150">Skapa en ny mapp (mappen kan finnas var som helst på den lokala enheten) och kopiera en .mp4-fil som du vill att tooencode och dataströmmen eller progressivt hämta.</span><span class="sxs-lookup"><span data-stu-id="57960-150">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want tooencode and stream or progressively download.</span></span> <span data-ttu-id="57960-151">I det här exemplet används hello ”C:\Media” sökväg.</span><span class="sxs-lookup"><span data-stu-id="57960-151">In this example, hello "C:\Media" path is used.</span></span>

### <a name="code"></a><span data-ttu-id="57960-152">Kod</span><span class="sxs-lookup"><span data-stu-id="57960-152">Code</span></span>

```
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Collections.Generic;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;
using System.Runtime.Serialization.Json;

namespace JobNotification
{
    public class EncodingJobMessage
    {
        // MessageVersion is used for version control.
        public String MessageVersion { get; set; }

        // Type of hello event. Valid values are
        // JobStateChange and NotificationEndpointRegistration.
        public String EventType { get; set; }

        // ETag is used toohelp hello customer detect if
        // hello message is a duplicate of another message previously sent.
        public String ETag { get; set; }

        // Time of occurrence of hello event.
        public String TimeStamp { get; set; }

        // Collection of values specific toohello event.

        // For hello JobStateChange event hello values are:
        //     JobId - Id of hello Job that triggered hello notification.
        //     NewState- hello new state of hello Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
        //     OldState- hello old state of hello Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished

        // For hello NotificationEndpointRegistration event hello values are:
        //     NotificationEndpointId- Id of hello NotificationEndpoint
        //          that triggered hello notification.
        //     State- hello state of hello Endpoint.
        //          Valid values are: Registered and Unregistered.

        public IDictionary<string, object> Properties { get; set; }
    }

    class Program
    {

        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
        private static readonly string _StorageConnectionString = 
            ConfigurationManager.AppSettings["StorageConnectionString"];

        private static CloudMediaContext _context = null;
        private static CloudQueue _queue = null;
        private static INotificationEndPoint _notificationEndPoint = null;

        private static readonly string _singleInputMp4Path =
            Path.GetFullPath(@"C:\Media\BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            string endPointAddress = Guid.NewGuid().ToString();

            // Create hello context.
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Create hello queue that will be receiving hello notification messages.
            _queue = CreateQueue(_StorageConnectionString, endPointAddress);

            // Create hello notification point that is mapped toohello queue.
            _notificationEndPoint =
                    _context.NotificationEndPoints.Create(
                    Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);


            if (_notificationEndPoint != null)
            {
                IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                WaitForJobToReachedFinishedState(job.Id);
            }

            // Clean up.
            _queue.Delete();
            _notificationEndPoint.Delete();
        }


        static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

            // Create hello queue client
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            // Retrieve a reference tooa queue
            CloudQueue queue = queueClient.GetQueueReference(endPointAddress);

            // Create hello queue if it doesn't already exist
            queue.CreateIfNotExists();

            return queue;
        }


        public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("My MP4 tooSmooth Streaming encoding job");

            //Create an encrypted asset and upload hello mp4.
            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted,
                inputMediaFilePath);

            // Get a media processor reference, and pass tooit hello name of the
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello conversion details, using a configuration file.
            ITask task = job.Tasks.AddNew("My encoding Task",
                processor,
                "Adaptive Streaming",
                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Add a notification point toohello job. You can add multiple notification points.  
            job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly,
                _notificationEndPoint);

            job.Submit();

            return job;
        }

        public static void WaitForJobToReachedFinishedState(string jobId)
        {
            int expectedState = (int)JobState.Finished;
            int timeOutInSeconds = 600;

            bool jobReachedExpectedState = false;
            DateTime startTime = DateTime.Now;
            int jobState = -1;

            while (!jobReachedExpectedState)
            {
                // Specify how often you want tooget messages from hello queue.
                Thread.Sleep(TimeSpan.FromSeconds(10));

                foreach (var message in _queue.GetMessages(10))
                {
                    using (Stream stream = new MemoryStream(message.AsBytes))
                    {
                        DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                        settings.UseSimpleDictionaryFormat = true;
                        DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                        EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);

                        Console.WriteLine();

                        // Display hello message information.
                        Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                        Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                        Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                        Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                        foreach (var property in encodingJobMsg.Properties)
                        {
                            Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                        }

                        // We are only interested in messages
                        // where EventType is "JobStateChange".
                        if (encodingJobMsg.EventType == "JobStateChange")
                        {
                            string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                            if (JobId == jobId)
                            {
                                string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                string newJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "NewState").FirstOrDefault().Value;

                                JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);

                                if (newJobState == (JobState)expectedState)
                                {
                                    Console.WriteLine("job with Id: {0} reached expected state: {1}",
                                        jobId, newJobState);
                                    jobReachedExpectedState = true;
                                    break;
                                }
                            }
                        }
                    }
                    // Delete hello message after we've read it.
                    _queue.DeleteMessage(message);
                }

                // Wait until timeout
                TimeSpan timeDiff = DateTime.Now - startTime;
                bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                if (timedOut)
                {
                    Console.WriteLine(@"Timeout for checking job notification messages,
                                        latest found state ='{0}', wait time = {1} secs",
                        jobState,
                        timeDiff.TotalSeconds);

                    throw new TimeoutException();
                }
            }
        }

        static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(),
                assetCreationOptions);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);
            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading of {0}", assetFile.Name);

            return asset;
        }

        static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```
<span data-ttu-id="57960-153">Hej föregående exempel produceras hello följande utdata.</span><span class="sxs-lookup"><span data-stu-id="57960-153">hello preceding example produced hello following output.</span></span> <span data-ttu-id="57960-154">Värdena kan variera.</span><span class="sxs-lookup"><span data-stu-id="57960-154">Your values will vary.</span></span>

    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4

    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35

    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 tooSmooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected
    State: Finished


## <a name="next-step"></a><span data-ttu-id="57960-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57960-155">Next step</span></span>
<span data-ttu-id="57960-156">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="57960-156">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="57960-157">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="57960-157">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
