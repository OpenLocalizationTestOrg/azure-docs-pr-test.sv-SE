---
title: aaaHow toouse Azure queue storage med hello WebJobs SDK
description: "Lär dig hur toouse Azure kö lagring med hello WebJobs-SDK. Skapa och ta bort köer. Infoga, granska, hämta och ta bort Kömeddelanden och mycket mer."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 49f844436b0453489800b2762a5c7dc30b9db805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-queue-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="e1112-104">Hur toouse Azure kö lagring med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1112-104">How toouse Azure queue storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="e1112-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="e1112-105">Overview</span></span>
<span data-ttu-id="e1112-106">Den här guiden innehåller C#-kodexempel som visar hur toouse hello Azure WebJobs SDK version 1.x med hello Azure queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e1112-106">This guide provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure queue storage service.</span></span>

<span data-ttu-id="e1112-107">hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller för[flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="e1112-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="e1112-108">De flesta av hello kodstycken Visa endast funktioner, inte hello kod som skapar hello `JobHost` objekt som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e1112-108">Most of hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="e1112-109">hello guide innehåller hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="e1112-109">hello guide includes hello following topics:</span></span>

* [<span data-ttu-id="e1112-110">Hur tootrigger en funktion när ett kömeddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="e1112-110">How tootrigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="e1112-111">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-111">String queue messages</span></span>
  * <span data-ttu-id="e1112-112">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-112">POCO queue messages</span></span>
  * <span data-ttu-id="e1112-113">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="e1112-113">Async functions</span></span>
  * <span data-ttu-id="e1112-114">Typer hello QueueTrigger attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="e1112-114">Types hello QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="e1112-115">Avsökningen algoritm</span><span class="sxs-lookup"><span data-stu-id="e1112-115">Polling algorithm</span></span>
  * <span data-ttu-id="e1112-116">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="e1112-116">Multiple instances</span></span>
  * <span data-ttu-id="e1112-117">Parallell körning</span><span class="sxs-lookup"><span data-stu-id="e1112-117">Parallel execution</span></span>
  * <span data-ttu-id="e1112-118">Hämta kön eller kön meddelandet metadata</span><span class="sxs-lookup"><span data-stu-id="e1112-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="e1112-119">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="e1112-119">Graceful shutdown</span></span>
* [<span data-ttu-id="e1112-120">Hur toocreate en kö meddelande vid bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e1112-120">How toocreate a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="e1112-121">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-121">String queue messages</span></span>
  * <span data-ttu-id="e1112-122">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-122">POCO queue messages</span></span>
  * <span data-ttu-id="e1112-123">Skapa flera meddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="e1112-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="e1112-124">Typer hello kön attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="e1112-124">Types hello Queue attribute works with</span></span>
  * <span data-ttu-id="e1112-125">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="e1112-125">Use WebJobs SDK attributes in hello body of a function</span></span>
* [<span data-ttu-id="e1112-126">Hur tooread och skriva BLOB-objekt vid bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e1112-126">How tooread and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="e1112-127">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-127">String queue messages</span></span>
  * <span data-ttu-id="e1112-128">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-128">POCO queue messages</span></span>
  * <span data-ttu-id="e1112-129">Typer hello Blob attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="e1112-129">Types hello Blob attribute works with</span></span>
* [<span data-ttu-id="e1112-130">Hur toohandle förgiftade meddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-130">How toohandle poison messages</span></span>](#poison)
  * <span data-ttu-id="e1112-131">Hantering av automatisk skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="e1112-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="e1112-132">Hantering av manuell skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="e1112-132">Manual poison message handling</span></span>
* [<span data-ttu-id="e1112-133">Hur tooset konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="e1112-133">How tooset configuration options</span></span>](#config)
  * <span data-ttu-id="e1112-134">Ange SDK anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="e1112-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="e1112-135">Konfigurera inställningar för QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="e1112-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="e1112-136">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="e1112-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="e1112-137">Hur tootrigger en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="e1112-137">How tootrigger a function manually</span></span>](#manual)
* [<span data-ttu-id="e1112-138">Hur toowrite loggar</span><span class="sxs-lookup"><span data-stu-id="e1112-138">How toowrite logs</span></span>](#logs)
* [<span data-ttu-id="e1112-139">Hur toohandle fel och konfigurera tidsgränser</span><span class="sxs-lookup"><span data-stu-id="e1112-139">How toohandle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="e1112-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1112-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="e1112-141"><a id="trigger"></a>Hur tootrigger en funktion när ett kömeddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="e1112-141"><a id="trigger"></a> How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="e1112-142">toowrite anropar en funktion som hello WebJobs SDK när ett kömeddelande tas emot använder hello `QueueTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-142">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `QueueTrigger` attribute.</span></span> <span data-ttu-id="e1112-143">hello Attributkonstruktorn använder en strängparameter som anger hello kön toopoll hello namn.</span><span class="sxs-lookup"><span data-stu-id="e1112-143">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="e1112-144">Du kan också [ange hello könamnet dynamiskt](#config).</span><span class="sxs-lookup"><span data-stu-id="e1112-144">You can also [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="e1112-145">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-145">String queue messages</span></span>
<span data-ttu-id="e1112-146">I följande exempel hello, hello kön innehåller ett strängmeddelande, så `QueueTrigger` är tillämpade tooa strängparameter med namnet `logMessage` som innehåller hello innehållet hello kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="e1112-146">In hello following example, hello queue contains a string message, so `QueueTrigger` is applied tooa string parameter named `logMessage` which contains hello content of hello queue message.</span></span> <span data-ttu-id="e1112-147">Hej funktionen [skriver en logg meddelandet toohello instrumentpanelen](#logs).</span><span class="sxs-lookup"><span data-stu-id="e1112-147">hello function [writes a log message toohello Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="e1112-148">Förutom `string`, hello kan vara en bytematris en `CloudQueueMessage` objekt eller en POCO som du definierar.</span><span class="sxs-lookup"><span data-stu-id="e1112-148">Besides `string`, hello parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="e1112-149">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="e1112-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="e1112-150">I följande exempel hello, hello kön meddelandet innehåller JSON för en `BlobInformation` objekt som innehåller en `BlobName` egenskap.</span><span class="sxs-lookup"><span data-stu-id="e1112-150">In hello following example, hello queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="e1112-151">hello SDK deserializes automatiskt hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="e1112-151">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="e1112-152">hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e1112-152">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="e1112-153">Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="e1112-153">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="e1112-154">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="e1112-154">Async functions</span></span>
<span data-ttu-id="e1112-155">hello följande async-funktionen [skriver en logg toohello instrumentpanelen](#logs).</span><span class="sxs-lookup"><span data-stu-id="e1112-155">hello following async function [writes a log toohello Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="e1112-156">Asynkrona funktioner kan ta en [annullering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)som visas i följande exempel som kopierar en blobb hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="e1112-157">(En förklaring av hello `queueTrigger` platshållare finns hello [Blobbar](#blobs) avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="e1112-157">(For an explanation of hello `queueTrigger` placeholder, see hello [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="e1112-158"><a id="qtattributetypes"></a>Typer hello QueueTrigger attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="e1112-158"><a id="qtattributetypes"></a> Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="e1112-159">Du kan använda `QueueTrigger` med hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="e1112-159">You can use `QueueTrigger` with hello following types:</span></span>

* `string`
* <span data-ttu-id="e1112-160">En POCO-typen som serialiseras som JSON</span><span class="sxs-lookup"><span data-stu-id="e1112-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="e1112-161"><a id="polling"></a>Avsökningen algoritm</span><span class="sxs-lookup"><span data-stu-id="e1112-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="e1112-162">hello SDK implementerar en exponentiell tillbaka av algoritmen tooreduce hello effekten av inaktiv-kön avsökning på transaktion lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="e1112-162">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="e1112-163">När ett meddelande hittas hello SDK väntar två sekunder och söker efter en annan message; När det finns inget meddelande väntar på fyra sekunder innan du försöker igen.</span><span class="sxs-lookup"><span data-stu-id="e1112-163">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="e1112-164">När efterföljande misslyckade försök tooget ett kömeddelande hello väntetiden fortsätter tooincrease tills den når maximal väntetid för hello, vilka standardvärden tooone minut.</span><span class="sxs-lookup"><span data-stu-id="e1112-164">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="e1112-165">[hello väntetiden konfigureras](#config).</span><span class="sxs-lookup"><span data-stu-id="e1112-165">[hello maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="e1112-166"><a id="instances"></a>Flera instanser</span><span class="sxs-lookup"><span data-stu-id="e1112-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="e1112-167">Om ditt webbprogram som körs på flera instanser är ett kontinuerligt Webbjobb som körs på varje dator och varje dator ska vänta för utlösare och försök toorun funktioner.</span><span class="sxs-lookup"><span data-stu-id="e1112-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="e1112-168">Hej WebJobs SDK kö utlösaren förhindrar automatiskt en funktion från att bearbeta ett kömeddelande flera gånger. funktioner har inte skrivits toobe idempotent toobe.</span><span class="sxs-lookup"><span data-stu-id="e1112-168">hello WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have toobe written toobe idempotent.</span></span> <span data-ttu-id="e1112-169">Men om du vill att tooensure att endast en instans av en funktion körs även när det finns flera instanser av hello värden webbprogram, kan du använda hello `Singleton` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-169">However, if you want tooensure that only one instance of a function runs even when there are multiple instances of hello host web app, you can use hello `Singleton` attribute.</span></span>

### <span data-ttu-id="e1112-170"><a id="parallel"></a>Parallell körning</span><span class="sxs-lookup"><span data-stu-id="e1112-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="e1112-171">Om du har flera funktioner som lyssnar på olika köer anropar hello SDK dem parallellt när meddelanden tas emot samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e1112-171">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="e1112-172">hello sak samma gäller när flera meddelanden tas emot för en enskild kö.</span><span class="sxs-lookup"><span data-stu-id="e1112-172">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="e1112-173">Som standard hello SDK hämtar en batch med 16 Kömeddelanden i taget och utför hello-funktion som behandlar dem parallellt.</span><span class="sxs-lookup"><span data-stu-id="e1112-173">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="e1112-174">[hello storlek är konfigurerbar](#config).</span><span class="sxs-lookup"><span data-stu-id="e1112-174">[hello batch size is configurable](#config).</span></span> <span data-ttu-id="e1112-175">När hello nummer bearbetas hämtar ned toohalf av hello batchstorlek, hämtar en annan batch hello SDK och påbörjar bearbetningen av dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e1112-175">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="e1112-176">Hello maximalt antal samtidiga meddelanden som bearbetas per funktion är därför en och en halv gånger hello batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="e1112-176">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="e1112-177">Den här begränsningen gäller separat tooeach funktion som har en `QueueTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-177">This limit applies separately tooeach function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="e1112-178">Om du inte vill parallell körning för meddelanden som tas emot i en kö kan ange du hello batch storlek too1.</span><span class="sxs-lookup"><span data-stu-id="e1112-178">If you don't want parallel execution for messages received on one queue, you can set hello batch size too1.</span></span> <span data-ttu-id="e1112-179">Se även **mer kontroll över kön bearbetning** i [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="e1112-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="e1112-180"><a id="queuemetadata"></a>Hämta kön eller kön meddelandet metadata</span><span class="sxs-lookup"><span data-stu-id="e1112-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="e1112-181">Du kan få följande meddelandeegenskaper genom att lägga till parametrar toohello Metodsignaturen hello:</span><span class="sxs-lookup"><span data-stu-id="e1112-181">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="e1112-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="e1112-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="e1112-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="e1112-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="e1112-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="e1112-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="e1112-185">`string`queueTrigger (innehåller meddelandetext)</span><span class="sxs-lookup"><span data-stu-id="e1112-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="e1112-186">`string`ID</span><span class="sxs-lookup"><span data-stu-id="e1112-186">`string` id</span></span>
* <span data-ttu-id="e1112-187">`string`popReceipt</span><span class="sxs-lookup"><span data-stu-id="e1112-187">`string` popReceipt</span></span>
* <span data-ttu-id="e1112-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="e1112-188">`int` dequeueCount</span></span>

<span data-ttu-id="e1112-189">Om du vill toowork direkt med hello Azure storage-API, du kan också lägga till en `CloudStorageAccount` parameter.</span><span class="sxs-lookup"><span data-stu-id="e1112-189">If you want toowork directly with hello Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="e1112-190">hello skriver följande exempel alla denna metadata tooan INFO programloggen.</span><span class="sxs-lookup"><span data-stu-id="e1112-190">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="e1112-191">I exemplet hello innehåll både logMessage och queueTrigger hello av hälsningsmeddelande för kön.</span><span class="sxs-lookup"><span data-stu-id="e1112-191">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

<span data-ttu-id="e1112-192">Här är en exempellogg som skrivits av hello exempelkod:</span><span class="sxs-lookup"><span data-stu-id="e1112-192">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="e1112-193"><a id="graceful"></a>Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="e1112-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="e1112-194">En funktion som körs i ett kontinuerligt Webbjobb kan acceptera en `CancellationToken` parameter som aktiverar hello operativsystemet toonotify hello fungerar när hello Webbjobb kommer toobe avslutades.</span><span class="sxs-lookup"><span data-stu-id="e1112-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="e1112-195">Du kan använda det här meddelandet toomake att hello funktionen inte avslutas oväntat på ett sätt som lämnar data i ett inkonsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e1112-195">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="e1112-196">följande exempel visar hur hello toocheck för nära förestående Webbjobb avslutning i en funktion.</span><span class="sxs-lookup"><span data-stu-id="e1112-196">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

<span data-ttu-id="e1112-197">**Obs:** hello instrumentpanelen kanske inte korrekt visar hello status och utdata för funktioner som har stängts av.</span><span class="sxs-lookup"><span data-stu-id="e1112-197">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="e1112-198">Mer information finns i [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="e1112-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="e1112-199"><a id="createqueue"></a>Hur toocreate en kö meddelande vid bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e1112-199"><a id="createqueue"></a> How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="e1112-200">toowrite en funktion som skapar ett nytt meddelande i kön, Använd hello `Queue` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-200">toowrite a function that creates a new queue message, use hello `Queue` attribute.</span></span> <span data-ttu-id="e1112-201">Som `QueueTrigger`kan du skicka in hello könamnet som en sträng eller [ange hello könamnet dynamiskt](#config).</span><span class="sxs-lookup"><span data-stu-id="e1112-201">Like `QueueTrigger`, you pass in hello queue name as a string or you can [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="e1112-202">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-202">String queue messages</span></span>
<span data-ttu-id="e1112-203">hello följande kodexempel icke asynkron skapar ett nytt meddelande i kön i hello kö med namnet ”outputqueue” med hello samma innehåll som kön hello-meddelande togs emot i hello kö med namnet ”inputqueue”.</span><span class="sxs-lookup"><span data-stu-id="e1112-203">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="e1112-204">(För asynkrona funktioner använder `IAsyncCollector<T>` enligt senare i det här avsnittet.)</span><span class="sxs-lookup"><span data-stu-id="e1112-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="e1112-205">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="e1112-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="e1112-206">toocreate ett kömeddelande som innehåller en POCO i stället för en sträng, pass hello POCO skriver som en output-parameter toohello `Queue` Attributkonstruktorn.</span><span class="sxs-lookup"><span data-stu-id="e1112-206">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="e1112-207">hello SDK Serialiserar automatiskt hello objektet tooJSON.</span><span class="sxs-lookup"><span data-stu-id="e1112-207">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="e1112-208">Ett kömeddelande skapas alltid, även om hello-objektet är null.</span><span class="sxs-lookup"><span data-stu-id="e1112-208">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="e1112-209">Skapa flera meddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="e1112-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="e1112-210">toocreate flera meddelanden gör hello parametertypen för hello utgående kö `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-210">toocreate multiple messages, make hello parameter type for hello output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="e1112-211">Varje meddelande i kön skapas omedelbart när hello `Add` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="e1112-211">Each queue message is created immediately when hello `Add` method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="e1112-212">Typer hello kön attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="e1112-212">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="e1112-213">Du kan använda hello `Queue` attributet på hello följande parametertyper:</span><span class="sxs-lookup"><span data-stu-id="e1112-213">You can use hello `Queue` attribute on hello following parameter types:</span></span>

* <span data-ttu-id="e1112-214">`out string`(skapar kömeddelande om parametervärdet är icke-null när hello funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="e1112-214">`out string` (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="e1112-215">`out byte[]`(fungerar som `string`)</span><span class="sxs-lookup"><span data-stu-id="e1112-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="e1112-216">`out CloudQueueMessage`(fungerar som `string`)</span><span class="sxs-lookup"><span data-stu-id="e1112-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="e1112-217">`out POCO`(en serialiserbar typ. skapar ett meddelande med ett null-objekt om hello parameter är null när hello funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="e1112-217">`out POCO` (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="e1112-218">`CloudQueue`(för att skapa meddelanden manuellt direkt med hello Azure Storage-API)</span><span class="sxs-lookup"><span data-stu-id="e1112-218">`CloudQueue` (for creating messages manually using hello Azure Storage API directly)</span></span>

### <span data-ttu-id="e1112-219"><a id="ibinder"></a>Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="e1112-219"><a id="ibinder"></a>Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="e1112-220">Om du behöver toodo vissa fungerar i din funktion innan du använder ett WebJobs-SDK-attribut som `Queue`, `Blob`, eller `Table`, du kan använda hello `IBinder` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e1112-220">If you need toodo some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use hello `IBinder` interface.</span></span>

<span data-ttu-id="e1112-221">hello följande exempel tar ett inkommande kö-meddelande och skapar ett nytt meddelande med hello samma innehåll i en utgående kö.</span><span class="sxs-lookup"><span data-stu-id="e1112-221">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="e1112-222">hello utgående kö namn anges av koden i hello brödtexten i hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="e1112-222">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="e1112-223">Hej `IBinder` gränssnitt kan även användas med hello `Table` och `Blob` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-223">hello `IBinder` interface can also be used with hello `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="e1112-224"><a id="blobs"></a>Hur tooread och skriva BLOB-objekt och tabeller under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e1112-224"><a id="blobs"></a> How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="e1112-225">Hej `Blob` och `Table` attribut aktivera tooread och skriva BLOB och tabeller.</span><span class="sxs-lookup"><span data-stu-id="e1112-225">hello `Blob` and `Table` attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="e1112-226">hello prover i det här avsnittet gäller tooblobs.</span><span class="sxs-lookup"><span data-stu-id="e1112-226">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="e1112-227">Kodexempel som visar hur tootrigger bearbetar när blobbar har skapats eller uppdaterats, se [hur toouse Azure blob storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), och kodexempel som läsa och skriva tabeller, se [hur toouse Azure-tabellen lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="e1112-227">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="e1112-228">Sträng Kömeddelanden utlösa blob-åtgärder</span><span class="sxs-lookup"><span data-stu-id="e1112-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="e1112-229">För ett meddelande i kön som innehåller en sträng, `queueTrigger` är en platshållare som du kan använda i hello `Blob` attributets `blobPath` parametrar som innehåller hello innehållet i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="e1112-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in hello `Blob` attribute's `blobPath` parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="e1112-230">hello följande exempel används `Stream` tooread och skriva BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="e1112-230">hello following example uses `Stream` objects tooread and write blobs.</span></span> <span data-ttu-id="e1112-231">hälsningsmeddelande för kön är en blob som finns i hello textblobs behållare hello namn.</span><span class="sxs-lookup"><span data-stu-id="e1112-231">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="e1112-232">En kopia av hello blob med ”-nya” tillagda toohello namnet skapas i hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="e1112-232">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="e1112-233">Hej `Blob` attributet konstruktorn tar en `blobPath` parameter som anger hello-behållaren och blobbnamnet.</span><span class="sxs-lookup"><span data-stu-id="e1112-233">hello `Blob` attribute constructor takes a `blobPath` parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="e1112-234">Mer information om den här platshållaren finns [hur toouse Azure blob storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="e1112-234">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="e1112-235">När attributet hello decorates en `Stream` objekt, en annan konstruktorparametern anger hello `FileAccess` läge som Läs-, Skriv- eller läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="e1112-235">When hello attribute decorates a `Stream` object, another constructor parameter specifies hello `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="e1112-236">hello följande exempel används en `CloudBlockBlob` objekt toodelete en blob.</span><span class="sxs-lookup"><span data-stu-id="e1112-236">hello following example uses a `CloudBlockBlob` object toodelete a blob.</span></span> <span data-ttu-id="e1112-237">Hej kömeddelande är hello namn hello-blob.</span><span class="sxs-lookup"><span data-stu-id="e1112-237">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="e1112-238"><a id="pocoblobs"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="e1112-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="e1112-239">Du kan använda platshållare att egenskaperna för hello objekt i hello-namnet för en POCO som lagras som JSON i kön hälsningsmeddelande `Queue` attributets `blobPath` parameter.</span><span class="sxs-lookup"><span data-stu-id="e1112-239">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="e1112-240">Du kan också använda [kö metadata egenskapsnamn](#queuemetadata) som platshållare.</span><span class="sxs-lookup"><span data-stu-id="e1112-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="e1112-241">hello kopieras följande exempel en blob tooa nya blob med ett annat tillägg.</span><span class="sxs-lookup"><span data-stu-id="e1112-241">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="e1112-242">hälsningsmeddelande för kön är en `BlobInformation` -objekt som innehåller `BlobName` och `BlobNameWithoutExtension` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="e1112-242">hello queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="e1112-243">hello egenskapsnamn används som platshållare i hello blob sökväg för hello `Blob` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-243">hello property names are used as placeholders in hello blob path for hello `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="e1112-244">hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e1112-244">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="e1112-245">Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="e1112-245">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="e1112-246">Om du behöver toodo vissa fungerar i din funktion innan du binder en tooan blob-objektet, du kan använda hello-attributet i hello brödtext hello funktionen [enligt tidigare för hello kön attributet](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="e1112-246">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, [as shown earlier for hello Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="e1112-247"><a id="blobattributetypes"></a>Du kan använda hello Blob-attribut med</span><span class="sxs-lookup"><span data-stu-id="e1112-247"><a id="blobattributetypes"></a> Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="e1112-248">Hej `Blob` attributet kan användas med hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="e1112-248">hello `Blob` attribute can be used with hello following types:</span></span>

* <span data-ttu-id="e1112-249">`Stream`(läsa eller skriva som anges av parametern hello FileAccess konstruktor)</span><span class="sxs-lookup"><span data-stu-id="e1112-249">`Stream` (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="e1112-250">`string`(Läs)</span><span class="sxs-lookup"><span data-stu-id="e1112-250">`string` (read)</span></span>
* <span data-ttu-id="e1112-251">`out string`(skriva; skapar en blob endast om hello strängparameter är icke-null om hello funktionen returnerar)</span><span class="sxs-lookup"><span data-stu-id="e1112-251">`out string` (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="e1112-252">POCO (läsa)</span><span class="sxs-lookup"><span data-stu-id="e1112-252">POCO (read)</span></span>
* <span data-ttu-id="e1112-253">ut POCO (skriva; alltid skapar en blob, skapar som null-objekt om POCO-parametern är null när hello funktionen returnerar)</span><span class="sxs-lookup"><span data-stu-id="e1112-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="e1112-254">`CloudBlobStream`(skriva)</span><span class="sxs-lookup"><span data-stu-id="e1112-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="e1112-255">`ICloudBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="e1112-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="e1112-256">`CloudBlockBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="e1112-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="e1112-257">`CloudPageBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="e1112-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="e1112-258"><a id="poison"></a>Hur toohandle förgiftade meddelanden</span><span class="sxs-lookup"><span data-stu-id="e1112-258"><a id="poison"></a> How toohandle poison messages</span></span>
<span data-ttu-id="e1112-259">Meddelanden vars innehåll orsakar en funktionen toofail kallas *förgiftade meddelanden*.</span><span class="sxs-lookup"><span data-stu-id="e1112-259">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="e1112-260">När hello funktionen misslyckas hello kön meddelandet tas inte bort och slutligen hämtas igen, vilket ledde hello cykel toobe upprepas.</span><span class="sxs-lookup"><span data-stu-id="e1112-260">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="e1112-261">hello SDK kan avbryta hello cykel efter ett begränsat antal upprepningar eller så kan du göra det manuellt.</span><span class="sxs-lookup"><span data-stu-id="e1112-261">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="e1112-262">Hantering av automatisk skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="e1112-262">Automatic poison message handling</span></span>
<span data-ttu-id="e1112-263">hello SDK ska anropa en funktion in too5 gånger tooprocess ett kömeddelande.</span><span class="sxs-lookup"><span data-stu-id="e1112-263">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="e1112-264">Om hello femte försök misslyckas är hello-meddelande har flyttats tooa skadligt kö.</span><span class="sxs-lookup"><span data-stu-id="e1112-264">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="e1112-265">[hello maximalt antal försök konfigureras](#config).</span><span class="sxs-lookup"><span data-stu-id="e1112-265">[hello maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="e1112-266">hello skadligt kön heter *{originalqueuename}*-skadligt.</span><span class="sxs-lookup"><span data-stu-id="e1112-266">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="e1112-267">Du kan skriva en funktion tooprocess meddelanden från hello skadligt kö med loggning av dem eller skicka ett meddelande om att manuella åtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="e1112-267">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="e1112-268">I följande exempel hello hello `CopyBlob` funktionen kommer att misslyckas när ett kömeddelande innehåller hello namnet på en blob som inte finns.</span><span class="sxs-lookup"><span data-stu-id="e1112-268">In hello following example hello `CopyBlob` function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="e1112-269">När det händer kan flyttas hello-meddelande från hello copyblobqueue kön toohello copyblobqueue poison kö.</span><span class="sxs-lookup"><span data-stu-id="e1112-269">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="e1112-270">Hej `ProcessPoisonMessage` och sedan loggar hello skadligt meddelande.</span><span class="sxs-lookup"><span data-stu-id="e1112-270">hello `ProcessPoisonMessage` then logs hello poison message.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

<span data-ttu-id="e1112-271">hello följande bild visar konsolens utdata från de här funktionerna när ett skadligt meddelande bearbetas.</span><span class="sxs-lookup"><span data-stu-id="e1112-271">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Konsolens utdata för hantering av skadligt meddelande](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="e1112-273">Hantering av manuell skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="e1112-273">Manual poison message handling</span></span>
<span data-ttu-id="e1112-274">Du kan hämta hello många gånger meddelandet har plockats för bearbetning genom att lägga till en `int` parameter med namnet `dequeueCount` tooyour funktion.</span><span class="sxs-lookup"><span data-stu-id="e1112-274">You can get hello number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` tooyour function.</span></span> <span data-ttu-id="e1112-275">Därefter kan du kontrollera hello status Created antalet i Funktionskoden och utföra egna skadligt meddelandehantering när hello antalet överskrider ett tröskelvärde som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-275">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <span data-ttu-id="e1112-276"><a id="config"></a>Hur tooset konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="e1112-276"><a id="config"></a> How tooset configuration options</span></span>
<span data-ttu-id="e1112-277">Du kan använda hello `JobHostConfiguration` typen tooset hello följande konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="e1112-277">You can use hello `JobHostConfiguration` type tooset hello following configuration options:</span></span>

* <span data-ttu-id="e1112-278">Ange hello SDK-anslutningssträngar i koden.</span><span class="sxs-lookup"><span data-stu-id="e1112-278">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="e1112-279">Konfigurera `QueueTrigger` inställningar, till exempel maximalt antal har status Created.</span><span class="sxs-lookup"><span data-stu-id="e1112-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="e1112-280">Hämta könamn från konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e1112-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="e1112-281"><a id="setconnstr"></a>Ange SDK anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="e1112-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="e1112-282">Hello SDK-anslutningssträngar kan i koden du toouse egna namn på anslutning sträng i configuration-filer eller miljövariabler som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-282">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="e1112-283"><a id="configqueue"></a>Konfigurera inställningar för QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="e1112-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="e1112-284">Du kan konfigurera följande inställningar som gäller toohello kön meddelandebehandling hello:</span><span class="sxs-lookup"><span data-stu-id="e1112-284">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="e1112-285">Maximalt antal meddelanden i kö som tas upp samtidigt toobe utförs parallellt hello (standard är 16).</span><span class="sxs-lookup"><span data-stu-id="e1112-285">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="e1112-286">Hej maximalt antal försök innan ett kömeddelande skickas tooa skadligt kön (standardvärdet är 5).</span><span class="sxs-lookup"><span data-stu-id="e1112-286">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="e1112-287">hello väntetiden innan avsökning igen när en kö är tomt (standard är 1 minut).</span><span class="sxs-lookup"><span data-stu-id="e1112-287">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="e1112-288">följande exempel visar hur hello tooconfigure dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="e1112-288">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="e1112-289"><a id="setnamesincode"></a>Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="e1112-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="e1112-290">Vill ibland du toospecify en kö, ett blob-namn eller en behållare eller en tabell i stället för att hårdkoda ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="e1112-290">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="e1112-291">Du kan exempelvis toospecify hello könamnet för `QueueTrigger` i en konfiguration av fil- eller miljö variabel.</span><span class="sxs-lookup"><span data-stu-id="e1112-291">For example, you might want toospecify hello queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="e1112-292">Du kan göra det genom att passera i en `NameResolver` objekt toohello `JobHostConfiguration` typen.</span><span class="sxs-lookup"><span data-stu-id="e1112-292">You can do that by passing in a `NameResolver` object toohello `JobHostConfiguration` type.</span></span> <span data-ttu-id="e1112-293">Du inkludera särskilda platshållare omges av procenttecken (%) i WebJobs SDK attributet konstruktorn parametrar och dina `NameResolver` koden anger hello faktiska värden toobe används i stället för dessa platshållare.</span><span class="sxs-lookup"><span data-stu-id="e1112-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="e1112-294">Till exempel anta att du vill toouse en kö med namnet logqueuetest i hello testmiljö och en namngiven logqueueprod i produktion.</span><span class="sxs-lookup"><span data-stu-id="e1112-294">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="e1112-295">I stället för en hårdkodad könamnet önskade toospecify hello namnet på en post i hello `appSettings` samling som skulle ha hello faktiska könamnet.</span><span class="sxs-lookup"><span data-stu-id="e1112-295">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello `appSettings` collection that would have hello actual queue name.</span></span> <span data-ttu-id="e1112-296">Om hello `appSettings` nyckeln är logqueue, din funktion kan se ut så hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="e1112-296">If hello `appSettings` key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="e1112-297">Din `NameResolver` klassen kan sedan hämta hello könamnet från `appSettings` som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e1112-297">Your `NameResolver` class could then get hello queue name from `appSettings` as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="e1112-298">Du skickar hello `NameResolver` klassen i toohello `JobHost` objekt som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-298">You pass hello `NameResolver` class in toohello `JobHost` object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="e1112-299">**Obs:** kön tabell och blobbnamnen är löst varje gång som en funktion kallas, men blob-behållaren namnmatchning bara när hello programmet startas.</span><span class="sxs-lookup"><span data-stu-id="e1112-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="e1112-300">Du kan inte ändra namnet för blob-behållare medan hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="e1112-300">You can't change blob container name while hello job is running.</span></span>

## <span data-ttu-id="e1112-301"><a id="manual"></a>Hur tootrigger en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="e1112-301"><a id="manual"></a>How tootrigger a function manually</span></span>
<span data-ttu-id="e1112-302">tootrigger en funktion manuellt, använda hello `Call` eller `CallAsync` metod på hello `JobHost` objektet och hello `NoAutomaticTrigger` attributet för hello funktion, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-302">tootrigger a function manually, use hello `Call` or `CallAsync` method on hello `JobHost` object and hello `NoAutomaticTrigger` attribute on hello function, as shown in hello following example.</span></span>

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <span data-ttu-id="e1112-303"><a id="logs"></a>Hur toowrite loggar</span><span class="sxs-lookup"><span data-stu-id="e1112-303"><a id="logs"></a>How toowrite logs</span></span>
<span data-ttu-id="e1112-304">hello instrumentpanelen visar loggar på två platser: hello sidan för hello Webbjobb och hello-sidan för ett särskilt Webbjobb-anrop.</span><span class="sxs-lookup"><span data-stu-id="e1112-304">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Loggar Webbjobb på sidan](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="e1112-307">Utdata från konsolen metoderna i en funktion eller hello `Main()` metoden visas i hello instrumentpanelssidan för hello Webbjobb, inte i hello-sida för en viss metodanropet.</span><span class="sxs-lookup"><span data-stu-id="e1112-307">Output from Console methods that you call in a function or in hello `Main()` method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="e1112-308">Utdata från hello TextWriter-objekt som du får från en parameter i Metodsignaturen visas i hello instrumentpanelssidan för ett metodanrop.</span><span class="sxs-lookup"><span data-stu-id="e1112-308">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="e1112-309">Konsolens utdata kan inte länkade tooa viss metodanropet eftersom hello konsolen är enkeltrådad, medan många jobbfunktioner hello kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e1112-309">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="e1112-310">Det är därför hello SDK innehåller varje funktionsanrop med sin egen unika loggen skrivarobjekt.</span><span class="sxs-lookup"><span data-stu-id="e1112-310">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="e1112-311">toowrite [spårning programloggarna](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), använda `Console.Out` (skapar loggar som är märkta som INFO) och `Console.Error` (skapar loggar som är märkta som fel).</span><span class="sxs-lookup"><span data-stu-id="e1112-311">toowrite [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="e1112-312">Ett alternativ är toouse [spårningen eller TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), vilket möjliggör utförlig, varning, och kritiska nivåer i tillägget tooInfo och fel.</span><span class="sxs-lookup"><span data-stu-id="e1112-312">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="e1112-313">Spårningsloggar för programmet visas i hello web app-loggfiler, Azure-tabeller eller Azure BLOB-objekt beroende på hur du konfigurerar din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="e1112-313">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="e1112-314">Som gäller för alla konsolens utdata visas också hello senaste 100 programloggarna i hello instrumentpanelssidan för hello Webbjobb, inte hello sidan för ett funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="e1112-314">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="e1112-315">Konsolens utdata visas i hello instrumentpanelen endast om hello programmet körs i en Azure-Webbjobb inte om hello program körs lokalt eller i en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="e1112-315">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="e1112-316">Inaktivera loggning för instrumentpanelen för scenarier med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="e1112-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="e1112-317">Som standard hello SDK skriver loggar toostorage och den här aktiviteten kan försämra prestanda vid bearbetning av många meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e1112-317">By default, hello SDK writes logs toostorage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="e1112-318">toodisable loggning, ange hello instrumentpanelen anslutning sträng toonull som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e1112-318">toodisable logging, set hello dashboard connection string toonull as shown in hello following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="e1112-319">hello följande exempel visar flera olika sätt toowrite loggar:</span><span class="sxs-lookup"><span data-stu-id="e1112-319">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="e1112-320">I hello WebJobs SDK-instrumentpanelen, hello utdata från hello `TextWriter` objekt visar upp går toohello sida för en viss fungera anrop och klicka på **växla utdata**:</span><span class="sxs-lookup"><span data-stu-id="e1112-320">In hello WebJobs SDK Dashboard, hello output from hello `TextWriter` object shows up when you go toohello page for a particular function invocation and click **Toggle Output**:</span></span>

![Klicka på länken för anrop av funktionen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="e1112-323">I hello WebJobs SDK-instrumentpanelen, hello senaste 100 rader i konsolen utdata visar dig när du gå toohello sidan för hello Webbjobb (inte för hello funktionsanrop) och klicka på **växla utdata**.</span><span class="sxs-lookup"><span data-stu-id="e1112-323">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and click **Toggle Output**.</span></span>

![Klicka på Visa/Dölj utdata](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="e1112-325">I ett kontinuerligt Webbjobb programloggarna visas i/data/jobb/kontinuerlig/*{webjobname}*/job_log.txt i filsystemet för hello web app.</span><span class="sxs-lookup"><span data-stu-id="e1112-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="e1112-326">I ett Azure blob hello program loggar ut så här: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fel, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="e1112-326">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="e1112-327">Och i en Azure-tabellen hello `Console.Out` och `Console.Error` loggar ut så här:</span><span class="sxs-lookup"><span data-stu-id="e1112-327">And in an Azure table hello `Console.Out` and `Console.Error` logs look like this:</span></span>

![Loggen för information i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Felloggen i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="e1112-330">Om du vill tooplug i din egen loggaren finns [det här exemplet](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="e1112-330">If you want tooplug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="e1112-331"><a id="errors"></a>Hur toohandle fel och konfigurera tidsgränser</span><span class="sxs-lookup"><span data-stu-id="e1112-331"><a id="errors"></a>How toohandle errors and configure timeouts</span></span>
<span data-ttu-id="e1112-332">Hej WebJobs SDK innehåller också en [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribut som du kan använda toocause som en funktion toobe avbrytas om inte slutföras inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="e1112-332">hello WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use toocause a function toobe canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="e1112-333">Och om du vill tooraise en avisering när för många fel som inträffar inom en angiven tidsperiod, kan du använda hello `ErrorTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="e1112-333">And if you want tooraise an alert when too many errors happen within a specified period of time, you can use hello `ErrorTrigger` attribute.</span></span> <span data-ttu-id="e1112-334">Här är en [ErrorTrigger exempel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="e1112-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    too= "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors toohello Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="e1112-335">Du kan också dynamiskt inaktiverar och aktiverar funktioner toocontrol om de kan utlösas, med hjälp av en konfigurationsväxel som kan vara en appinställning eller Miljövariabelns namn.</span><span class="sxs-lookup"><span data-stu-id="e1112-335">You can also dynamically disable and enable functions toocontrol whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="e1112-336">Exempelkod finns hello `Disable` attribut i [hello WebJobs SDK exempel databasen](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="e1112-336">For sample code, see hello `Disable` attribute in [hello WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="e1112-337"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1112-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="e1112-338">Den här guiden har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure köer.</span><span class="sxs-lookup"><span data-stu-id="e1112-338">This guide has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="e1112-339">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="e1112-339">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
