---
title: "Använd Azure-kölagring med WebJobs SDK:n"
description: "Lär dig mer om att använda Azure queue storage med WebJobs SDK. Skapa och ta bort köer. Infoga, granska, hämta och ta bort Kömeddelanden och mycket mer."
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
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a><span data-ttu-id="1f520-104">Använd Azure-kölagring med WebJobs SDK:n</span><span class="sxs-lookup"><span data-stu-id="1f520-104">How to use Azure queue storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="1f520-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="1f520-105">Overview</span></span>
<span data-ttu-id="1f520-106">Den här guiden innehåller C#-kodexempel som visar hur du använder Azure WebJobs SDK version 1.x med tjänsten Azure queue storage.</span><span class="sxs-lookup"><span data-stu-id="1f520-106">This guide provides C# code samples that show how to use the Azure WebJobs SDK version 1.x with the Azure queue storage service.</span></span>

<span data-ttu-id="1f520-107">Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller [flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="1f520-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="1f520-108">De flesta av kodavsnitten Visa endast funktioner, inte koden som skapar den `JobHost` objekt som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1f520-108">Most of the code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="1f520-109">Guiden innehåller följande ämnen:</span><span class="sxs-lookup"><span data-stu-id="1f520-109">The guide includes the following topics:</span></span>

* [<span data-ttu-id="1f520-110">Hur du utlöser en funktion när ett kömeddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="1f520-110">How to trigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="1f520-111">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-111">String queue messages</span></span>
  * <span data-ttu-id="1f520-112">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-112">POCO queue messages</span></span>
  * <span data-ttu-id="1f520-113">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="1f520-113">Async functions</span></span>
  * <span data-ttu-id="1f520-114">Attributet QueueTrigger fungerar med typer</span><span class="sxs-lookup"><span data-stu-id="1f520-114">Types the QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="1f520-115">Avsökningen algoritm</span><span class="sxs-lookup"><span data-stu-id="1f520-115">Polling algorithm</span></span>
  * <span data-ttu-id="1f520-116">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="1f520-116">Multiple instances</span></span>
  * <span data-ttu-id="1f520-117">Parallell körning</span><span class="sxs-lookup"><span data-stu-id="1f520-117">Parallel execution</span></span>
  * <span data-ttu-id="1f520-118">Hämta kön eller kön meddelandet metadata</span><span class="sxs-lookup"><span data-stu-id="1f520-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="1f520-119">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="1f520-119">Graceful shutdown</span></span>
* [<span data-ttu-id="1f520-120">Så här skapar du ett kömeddelande under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="1f520-120">How to create a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="1f520-121">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-121">String queue messages</span></span>
  * <span data-ttu-id="1f520-122">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-122">POCO queue messages</span></span>
  * <span data-ttu-id="1f520-123">Skapa flera meddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="1f520-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="1f520-124">Attributet kön fungerar med typer</span><span class="sxs-lookup"><span data-stu-id="1f520-124">Types the Queue attribute works with</span></span>
  * <span data-ttu-id="1f520-125">Använda WebJobs-SDK-attribut i brödtexten för en funktion</span><span class="sxs-lookup"><span data-stu-id="1f520-125">Use WebJobs SDK attributes in the body of a function</span></span>
* [<span data-ttu-id="1f520-126">Hur kan läsa och skriva BLOB under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="1f520-126">How to read and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="1f520-127">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-127">String queue messages</span></span>
  * <span data-ttu-id="1f520-128">POCO Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-128">POCO queue messages</span></span>
  * <span data-ttu-id="1f520-129">Blob-attributet som fungerar med typer</span><span class="sxs-lookup"><span data-stu-id="1f520-129">Types the Blob attribute works with</span></span>
* [<span data-ttu-id="1f520-130">Hur du hanterar förgiftade meddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-130">How to handle poison messages</span></span>](#poison)
  * <span data-ttu-id="1f520-131">Hantering av automatisk skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="1f520-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="1f520-132">Hantering av manuell skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="1f520-132">Manual poison message handling</span></span>
* [<span data-ttu-id="1f520-133">Hur du ställer in konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="1f520-133">How to set configuration options</span></span>](#config)
  * <span data-ttu-id="1f520-134">Ange SDK anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="1f520-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="1f520-135">Konfigurera inställningar för QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="1f520-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="1f520-136">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="1f520-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="1f520-137">Hur du utlöser en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="1f520-137">How to trigger a function manually</span></span>](#manual)
* [<span data-ttu-id="1f520-138">Hur du skriver loggar</span><span class="sxs-lookup"><span data-stu-id="1f520-138">How to write logs</span></span>](#logs)
* [<span data-ttu-id="1f520-139">Hantera fel och konfigurera tidsgränser</span><span class="sxs-lookup"><span data-stu-id="1f520-139">How to handle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="1f520-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f520-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="1f520-141"><a id="trigger"></a>Hur du utlöser en funktion när ett kömeddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="1f520-141"><a id="trigger"></a> How to trigger a function when a queue message is received</span></span>
<span data-ttu-id="1f520-142">Om du vill skriva en funktion som WebJobs SDK anropar när ett kömeddelande tas emot, använder den `QueueTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-142">To write a function that the WebJobs SDK calls when a queue message is received, use the `QueueTrigger` attribute.</span></span> <span data-ttu-id="1f520-143">Attributkonstruktorn använder en strängparameter som anger namnet på kön avsöker.</span><span class="sxs-lookup"><span data-stu-id="1f520-143">The attribute constructor takes a string parameter that specifies the name of the queue to poll.</span></span> <span data-ttu-id="1f520-144">Du kan också [ange könamnet dynamiskt](#config).</span><span class="sxs-lookup"><span data-stu-id="1f520-144">You can also [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="1f520-145">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-145">String queue messages</span></span>
<span data-ttu-id="1f520-146">I följande exempel kön innehåller ett strängmeddelande, så `QueueTrigger` tillämpas på en som strängparameter med namnet `logMessage` som innehåller innehållet i kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1f520-146">In the following example, the queue contains a string message, so `QueueTrigger` is applied to a string parameter named `logMessage` which contains the content of the queue message.</span></span> <span data-ttu-id="1f520-147">Funktionen [skriver ett loggmeddelande till instrumentpanelen](#logs).</span><span class="sxs-lookup"><span data-stu-id="1f520-147">The function [writes a log message to the Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="1f520-148">Förutom `string`, parametern kan vara en bytematris en `CloudQueueMessage` objekt eller en POCO som du definierar.</span><span class="sxs-lookup"><span data-stu-id="1f520-148">Besides `string`, the parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="1f520-149">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="1f520-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1f520-150">I följande exempel innehåller kömeddelandet JSON för en `BlobInformation` objekt som innehåller en `BlobName` egenskap.</span><span class="sxs-lookup"><span data-stu-id="1f520-150">In the following example, the queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="1f520-151">SDK deserializes automatiskt objektet.</span><span class="sxs-lookup"><span data-stu-id="1f520-151">The SDK automatically deserializes the object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="1f520-152">SDK använder den [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) serialisera och deserialisera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f520-152">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="1f520-153">Om du skapar meddelanden i kö i ett program som inte använder WebJobs SDK kan du skriva kod som i följande exempel för att skapa ett kömeddelande för POCO som SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="1f520-153">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="1f520-154">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="1f520-154">Async functions</span></span>
<span data-ttu-id="1f520-155">Följande async-funktion [skriver en logg till instrumentpanelen](#logs).</span><span class="sxs-lookup"><span data-stu-id="1f520-155">The following async function [writes a log to the Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="1f520-156">Asynkrona funktioner kan ta en [annullering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)som visas i följande exempel som kopierar en blob.</span><span class="sxs-lookup"><span data-stu-id="1f520-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in the following example which copies a blob.</span></span> <span data-ttu-id="1f520-157">(En förklaring av den `queueTrigger` platshållare, finns det [Blobbar](#blobs) avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="1f520-157">(For an explanation of the `queueTrigger` placeholder, see the [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="1f520-158"><a id="qtattributetypes"></a>Attributet QueueTrigger fungerar med typer</span><span class="sxs-lookup"><span data-stu-id="1f520-158"><a id="qtattributetypes"></a> Types the QueueTrigger attribute works with</span></span>
<span data-ttu-id="1f520-159">Du kan använda `QueueTrigger` med följande typer:</span><span class="sxs-lookup"><span data-stu-id="1f520-159">You can use `QueueTrigger` with the following types:</span></span>

* `string`
* <span data-ttu-id="1f520-160">En POCO-typen som serialiseras som JSON</span><span class="sxs-lookup"><span data-stu-id="1f520-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="1f520-161"><a id="polling"></a>Avsökningen algoritm</span><span class="sxs-lookup"><span data-stu-id="1f520-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="1f520-162">SDK implementerar en exponentiell tillbaka av algoritmen för att minska effekten av inaktiv-kön avsökning på transaktion lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="1f520-162">The SDK implements a random exponential back-off algorithm to reduce the effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="1f520-163">När ett meddelande hittas SDK väntar två sekunder och söker efter en annan message; När det finns inget meddelande väntar på fyra sekunder innan du försöker igen.</span><span class="sxs-lookup"><span data-stu-id="1f520-163">When a message is found, the SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="1f520-164">Väntetiden fortsätter att öka tills väntetiden, där standardinställningen är en minut efter efterföljande misslyckade försök att få ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="1f520-164">After subsequent failed attempts to get a queue message, the wait time continues to increase until it reaches the maximum wait time, which defaults to one minute.</span></span> <span data-ttu-id="1f520-165">[Väntetiden kan konfigureras](#config).</span><span class="sxs-lookup"><span data-stu-id="1f520-165">[The maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="1f520-166"><a id="instances"></a>Flera instanser</span><span class="sxs-lookup"><span data-stu-id="1f520-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="1f520-167">Om ditt webbprogram som körs på flera instanser är ett kontinuerligt Webbjobb som körs på varje dator och varje dator ska vänta tills utlösare och försök att köra funktioner.</span><span class="sxs-lookup"><span data-stu-id="1f520-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt to run functions.</span></span> <span data-ttu-id="1f520-168">WebJobs SDK kö utlösaren förhindrar automatiskt en funktion från att bearbeta ett kömeddelande flera gånger. funktioner har inte att skrivas till vara idempotent.</span><span class="sxs-lookup"><span data-stu-id="1f520-168">The WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have to be written to be idempotent.</span></span> <span data-ttu-id="1f520-169">Men om du vill se till att endast en instans av en funktion körs även när det finns flera instanser av värden webbprogram, kan du använda den `Singleton` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-169">However, if you want to ensure that only one instance of a function runs even when there are multiple instances of the host web app, you can use the `Singleton` attribute.</span></span>

### <span data-ttu-id="1f520-170"><a id="parallel"></a>Parallell körning</span><span class="sxs-lookup"><span data-stu-id="1f520-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="1f520-171">Om du har flera funktioner som lyssnar på olika köer anropar SDK dem parallellt när meddelanden tas emot samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1f520-171">If you have multiple functions listening on different queues, the SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="1f520-172">Samma sak gäller när flera meddelanden tas emot för en enskild kö.</span><span class="sxs-lookup"><span data-stu-id="1f520-172">The same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="1f520-173">Som standard SDK hämtar en batch med 16 Kömeddelanden i taget och kör den funktion som behandlar dem parallellt.</span><span class="sxs-lookup"><span data-stu-id="1f520-173">By default, the SDK gets a batch of 16 queue messages at a time and executes the function that processes them in parallel.</span></span> <span data-ttu-id="1f520-174">[Batchstorleken konfigureras](#config).</span><span class="sxs-lookup"><span data-stu-id="1f520-174">[The batch size is configurable](#config).</span></span> <span data-ttu-id="1f520-175">När antalet bearbetas hämtar till hälften av batchstorleken, hämtar en annan batch SDK och påbörjar bearbetningen av dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f520-175">When the number being processed gets down to half of the batch size, the SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="1f520-176">Därför är det maximala antalet samtidiga meddelanden som bearbetas per funktion en och en halv gånger batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="1f520-176">Therefore the maximum number of concurrent messages being processed per function is one and a half times the batch size.</span></span> <span data-ttu-id="1f520-177">Den här begränsningen gäller separat för varje funktion som har en `QueueTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-177">This limit applies separately to each function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="1f520-178">Om du inte vill parallell körning för meddelanden som tas emot i en kö kan ange du batchstorleken till 1.</span><span class="sxs-lookup"><span data-stu-id="1f520-178">If you don't want parallel execution for messages received on one queue, you can set the batch size to 1.</span></span> <span data-ttu-id="1f520-179">Se även **mer kontroll över kön bearbetning** i [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="1f520-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="1f520-180"><a id="queuemetadata"></a>Hämta kön eller kön meddelandet metadata</span><span class="sxs-lookup"><span data-stu-id="1f520-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="1f520-181">Du kan få följande meddelandeegenskaper genom att lägga till parametrar Metodsignaturen:</span><span class="sxs-lookup"><span data-stu-id="1f520-181">You can get the following message properties by adding parameters to the method signature:</span></span>

* <span data-ttu-id="1f520-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="1f520-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="1f520-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="1f520-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="1f520-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="1f520-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="1f520-185">`string`queueTrigger (innehåller meddelandetext)</span><span class="sxs-lookup"><span data-stu-id="1f520-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="1f520-186">`string`ID</span><span class="sxs-lookup"><span data-stu-id="1f520-186">`string` id</span></span>
* <span data-ttu-id="1f520-187">`string`popReceipt</span><span class="sxs-lookup"><span data-stu-id="1f520-187">`string` popReceipt</span></span>
* <span data-ttu-id="1f520-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="1f520-188">`int` dequeueCount</span></span>

<span data-ttu-id="1f520-189">Om du vill arbeta direkt med Azure storage API, du kan också lägga till en `CloudStorageAccount` parameter.</span><span class="sxs-lookup"><span data-stu-id="1f520-189">If you want to work directly with the Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="1f520-190">I följande exempel skriver alla dessa metadata till en INFO programloggen.</span><span class="sxs-lookup"><span data-stu-id="1f520-190">The following example writes all of this metadata to an INFO application log.</span></span> <span data-ttu-id="1f520-191">Både logMessage och queueTrigger innehåller innehållet i kön meddelandet i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1f520-191">In the example, both logMessage and queueTrigger contain the content of the queue message.</span></span>

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

<span data-ttu-id="1f520-192">Här är en exempellogg som skrivits av exempelkoden:</span><span class="sxs-lookup"><span data-stu-id="1f520-192">Here is a sample log written by the sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="1f520-193"><a id="graceful"></a>Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="1f520-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="1f520-194">En funktion som körs i ett kontinuerligt Webbjobb kan acceptera en `CancellationToken` parameter som gör operativsystemet för att meddela funktionen när Webbjobbet håller på att avslutas.</span><span class="sxs-lookup"><span data-stu-id="1f520-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables the operating system to notify the function when the WebJob is about to be terminated.</span></span> <span data-ttu-id="1f520-195">Du kan använda det här meddelandet för att kontrollera att funktionen inte avslutas oväntat på ett sätt som lämnar data i ett inkonsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1f520-195">You can use this notification to make sure the function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="1f520-196">I följande exempel visas hur du kontrollerar för nära förestående Webbjobb avslutning i en funktion.</span><span class="sxs-lookup"><span data-stu-id="1f520-196">The following example shows how to check for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="1f520-197">**Obs:** instrumentpanelen kanske inte korrekt visar status och utdata för funktioner som har stängts av.</span><span class="sxs-lookup"><span data-stu-id="1f520-197">**Note:** The Dashboard might not correctly show the status and output of functions that have been shut down.</span></span>

<span data-ttu-id="1f520-198">Mer information finns i [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="1f520-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="1f520-199"><a id="createqueue"></a>Så här skapar du ett kömeddelande under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="1f520-199"><a id="createqueue"></a> How to create a queue message while processing a queue message</span></span>
<span data-ttu-id="1f520-200">Om du vill skriva en funktion som skapar ett nytt meddelande i kön, använder den `Queue` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-200">To write a function that creates a new queue message, use the `Queue` attribute.</span></span> <span data-ttu-id="1f520-201">Som `QueueTrigger`kan du skicka in könamnet som en sträng eller [ange könamnet dynamiskt](#config).</span><span class="sxs-lookup"><span data-stu-id="1f520-201">Like `QueueTrigger`, you pass in the queue name as a string or you can [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="1f520-202">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-202">String queue messages</span></span>
<span data-ttu-id="1f520-203">Följande kodexempel icke asynkron skapar ett nytt meddelande i kön i kön med namnet ”outputqueue” med samma innehåll som kön meddelandet som togs emot i kön med namnet ”inputqueue”.</span><span class="sxs-lookup"><span data-stu-id="1f520-203">The following non-async code sample creates a new queue message in the queue named "outputqueue" with the same content as the queue message received in the queue named "inputqueue".</span></span> <span data-ttu-id="1f520-204">(För asynkrona funktioner använder `IAsyncCollector<T>` enligt senare i det här avsnittet.)</span><span class="sxs-lookup"><span data-stu-id="1f520-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="1f520-205">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="1f520-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1f520-206">Skicka POCO-typ för att skapa ett meddelande i kön som innehåller en POCO i stället för en sträng som en utdataparameter till den `Queue` Attributkonstruktorn.</span><span class="sxs-lookup"><span data-stu-id="1f520-206">To create a queue message that contains a POCO rather than a string, pass the POCO type as an output parameter to the `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="1f520-207">SDK Serialiserar automatiskt objektet till JSON.</span><span class="sxs-lookup"><span data-stu-id="1f520-207">The SDK automatically serializes the object to JSON.</span></span> <span data-ttu-id="1f520-208">Ett kömeddelande skapas alltid, även om objektet är null.</span><span class="sxs-lookup"><span data-stu-id="1f520-208">A queue message is always created, even if the object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="1f520-209">Skapa flera meddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="1f520-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="1f520-210">Om du vill skapa flera meddelanden gör parametertypen för utgående kö `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-210">To create multiple messages, make the parameter type for the output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="1f520-211">Varje meddelande i kön skapas omedelbart när den `Add` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="1f520-211">Each queue message is created immediately when the `Add` method is called.</span></span>

### <a name="types-that-the-queue-attribute-works-with"></a><span data-ttu-id="1f520-212">Typer som attributet kön fungerar med</span><span class="sxs-lookup"><span data-stu-id="1f520-212">Types that the Queue attribute works with</span></span>
<span data-ttu-id="1f520-213">Du kan använda den `Queue` -attributet på följande parameter:</span><span class="sxs-lookup"><span data-stu-id="1f520-213">You can use the `Queue` attribute on the following parameter types:</span></span>

* <span data-ttu-id="1f520-214">`out string`(skapar kömeddelande om parametervärdet är icke-null när funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="1f520-214">`out string` (creates queue message if parameter value is non-null when the function ends)</span></span>
* <span data-ttu-id="1f520-215">`out byte[]`(fungerar som `string`)</span><span class="sxs-lookup"><span data-stu-id="1f520-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="1f520-216">`out CloudQueueMessage`(fungerar som `string`)</span><span class="sxs-lookup"><span data-stu-id="1f520-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="1f520-217">`out POCO`(en serialiserbar typ. skapar ett meddelande med ett null-objekt om parametern är null när funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="1f520-217">`out POCO` (a serializable type, creates a message with a null object if the paramter is null when the function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="1f520-218">`CloudQueue`(för att skapa meddelanden manuellt med hjälp av Azure Storage-API direkt)</span><span class="sxs-lookup"><span data-stu-id="1f520-218">`CloudQueue` (for creating messages manually using the Azure Storage API directly)</span></span>

### <span data-ttu-id="1f520-219"><a id="ibinder"></a>Använda WebJobs-SDK-attribut i brödtexten för en funktion</span><span class="sxs-lookup"><span data-stu-id="1f520-219"><a id="ibinder"></a>Use WebJobs SDK attributes in the body of a function</span></span>
<span data-ttu-id="1f520-220">Om du behöver göra några resurser i din funktion innan du använder ett WebJobs-SDK-attribut som `Queue`, `Blob`, eller `Table`, du kan använda den `IBinder` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1f520-220">If you need to do some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use the `IBinder` interface.</span></span>

<span data-ttu-id="1f520-221">I följande exempel tar ett inkommande kö-meddelande och skapar ett nytt meddelande med samma innehåll i en utgående kö.</span><span class="sxs-lookup"><span data-stu-id="1f520-221">The following example takes an input queue message and creates a new message with the same content in an output queue.</span></span> <span data-ttu-id="1f520-222">Könamnet utdata anges av koden i själva funktionen.</span><span class="sxs-lookup"><span data-stu-id="1f520-222">The output queue name is set by code in the body of the function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="1f520-223">Den `IBinder` gränssnitt kan även användas med den `Table` och `Blob` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-223">The `IBinder` interface can also be used with the `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="1f520-224"><a id="blobs"></a>Hur kan läsa och skriva BLOB och tabeller under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="1f520-224"><a id="blobs"></a> How to read and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="1f520-225">Den `Blob` och `Table` attribut kan du läsa och skriva BLOB och tabeller.</span><span class="sxs-lookup"><span data-stu-id="1f520-225">The `Blob` and `Table` attributes enable you to read and write blobs and tables.</span></span> <span data-ttu-id="1f520-226">Exemplen i det här avsnittet gäller för blobbar.</span><span class="sxs-lookup"><span data-stu-id="1f520-226">The samples in this section apply to blobs.</span></span> <span data-ttu-id="1f520-227">Kodexempel som visar hur du utlöser processer när blobbar har skapats eller uppdaterats, se [använda Azure blob storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), och kodexempel som läsa och skriva tabeller, se [hur du använder Azure-tabellen Storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="1f520-227">For code samples that show how to trigger processes when blobs are created or updated, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="1f520-228">Sträng Kömeddelanden utlösa blob-åtgärder</span><span class="sxs-lookup"><span data-stu-id="1f520-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="1f520-229">För ett meddelande i kön som innehåller en sträng, `queueTrigger` är en platshållare som du kan använda i den `Blob` attributets `blobPath` parametrar som innehåller innehållet i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1f520-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in the `Blob` attribute's `blobPath` parameter that contains the contents of the message.</span></span>

<span data-ttu-id="1f520-230">I följande exempel används `Stream` objekt att läsa och skriva BLOB.</span><span class="sxs-lookup"><span data-stu-id="1f520-230">The following example uses `Stream` objects to read and write blobs.</span></span> <span data-ttu-id="1f520-231">Kömeddelandet är namnet på en blob som finns i behållaren textblobs.</span><span class="sxs-lookup"><span data-stu-id="1f520-231">The queue message is the name of a blob located in the textblobs container.</span></span> <span data-ttu-id="1f520-232">En kopia av blobben med ”-nya” läggs till namnet skapas i samma behållare.</span><span class="sxs-lookup"><span data-stu-id="1f520-232">A copy of the blob with "-new" appended to the name is created in the same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="1f520-233">Den `Blob` attributet konstruktorn tar en `blobPath` parameter som anger namnet på behållaren och blob.</span><span class="sxs-lookup"><span data-stu-id="1f520-233">The `Blob` attribute constructor takes a `blobPath` parameter that specifies the container and blob name.</span></span> <span data-ttu-id="1f520-234">Mer information om den här platshållaren finns [använda Azure blob storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="1f520-234">For more information about this placeholder, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="1f520-235">När attributet decorates en `Stream` objekt, en annan konstruktorparametern anger det `FileAccess` läge som Läs-, Skriv- eller läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="1f520-235">When the attribute decorates a `Stream` object, another constructor parameter specifies the `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="1f520-236">I följande exempel används en `CloudBlockBlob` objekt att ta bort en blob.</span><span class="sxs-lookup"><span data-stu-id="1f520-236">The following example uses a `CloudBlockBlob` object to delete a blob.</span></span> <span data-ttu-id="1f520-237">Kömeddelandet är namnet på blob.</span><span class="sxs-lookup"><span data-stu-id="1f520-237">The queue message is the name of the blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="1f520-238"><a id="pocoblobs"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="1f520-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1f520-239">För en POCO lagras som JSON i kön meddelandet, kan du använda platshållare som namn egenskaperna för objektet i den `Queue` attributets `blobPath` parameter.</span><span class="sxs-lookup"><span data-stu-id="1f520-239">For a POCO stored as JSON in the queue message, you can use placeholders that name properties of the object in the `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="1f520-240">Du kan också använda [kö metadata egenskapsnamn](#queuemetadata) som platshållare.</span><span class="sxs-lookup"><span data-stu-id="1f520-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="1f520-241">I följande exempel kopierar en blobb till en ny blob med ett annat tillägg.</span><span class="sxs-lookup"><span data-stu-id="1f520-241">The following example copies a blob to a new blob with a different extension.</span></span> <span data-ttu-id="1f520-242">Kön meddelandet är ett `BlobInformation` -objekt som innehåller `BlobName` och `BlobNameWithoutExtension` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1f520-242">The queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="1f520-243">Egenskapsnamnen används som platshållare i blobbsökvägen för den `Blob` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-243">The property names are used as placeholders in the blob path for the `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="1f520-244">SDK använder den [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) serialisera och deserialisera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f520-244">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="1f520-245">Om du skapar meddelanden i kö i ett program som inte använder WebJobs SDK kan du skriva kod som i följande exempel för att skapa ett kömeddelande för POCO som SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="1f520-245">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="1f520-246">Om du behöver göra vissa arbetet i din funktion innan du binder en blobb till ett objekt kan du använda attributet i brödtexten i funktion, [enligt tidigare för attributet kön](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="1f520-246">If you need to do some work in your function before binding a blob to an object, you can use the attribute in the body of the function, [as shown earlier for the Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="1f520-247"><a id="blobattributetypes"></a>Du kan använda Blob-attribut med</span><span class="sxs-lookup"><span data-stu-id="1f520-247"><a id="blobattributetypes"></a> Types you can use the Blob attribute with</span></span>
<span data-ttu-id="1f520-248">Den `Blob` attributet kan användas med följande typer:</span><span class="sxs-lookup"><span data-stu-id="1f520-248">The `Blob` attribute can be used with the following types:</span></span>

* <span data-ttu-id="1f520-249">`Stream`(läsa eller skriva anges med hjälp av parametern FileAccess konstruktor)</span><span class="sxs-lookup"><span data-stu-id="1f520-249">`Stream` (read or write, specified by using the FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="1f520-250">`string`(Läs)</span><span class="sxs-lookup"><span data-stu-id="1f520-250">`string` (read)</span></span>
* <span data-ttu-id="1f520-251">`out string`(skriva; skapar en blob endast om strängparametern är icke-null när returnerar funktionen)</span><span class="sxs-lookup"><span data-stu-id="1f520-251">`out string` (write; creates a blob only if the string parameter is non-null when the function returns)</span></span>
* <span data-ttu-id="1f520-252">POCO (läsa)</span><span class="sxs-lookup"><span data-stu-id="1f520-252">POCO (read)</span></span>
* <span data-ttu-id="1f520-253">ut POCO (skriva; alltid skapar en blob, skapar som null-objekt om POCO-parametern är null när returnerar funktionen)</span><span class="sxs-lookup"><span data-stu-id="1f520-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when the function returns)</span></span>
* <span data-ttu-id="1f520-254">`CloudBlobStream`(skriva)</span><span class="sxs-lookup"><span data-stu-id="1f520-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="1f520-255">`ICloudBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="1f520-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="1f520-256">`CloudBlockBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="1f520-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="1f520-257">`CloudPageBlob`(läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="1f520-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="1f520-258"><a id="poison"></a>Hur du hanterar förgiftade meddelanden</span><span class="sxs-lookup"><span data-stu-id="1f520-258"><a id="poison"></a> How to handle poison messages</span></span>
<span data-ttu-id="1f520-259">Meddelanden vars innehåll gör en funktionen misslyckas kallas *förgiftade meddelanden*.</span><span class="sxs-lookup"><span data-stu-id="1f520-259">Messages whose content causes a function to fail are called *poison messages*.</span></span> <span data-ttu-id="1f520-260">När funktionen misslyckas, tas inte bort kömeddelandet och slutligen hämtas igen och orsakar cykeln ska upprepas.</span><span class="sxs-lookup"><span data-stu-id="1f520-260">When the function fails, the queue message is not deleted and eventually is picked up again, causing the cycle to be repeated.</span></span> <span data-ttu-id="1f520-261">SDK kan avbryta cykeln efter ett begränsat antal upprepningar eller så kan du göra det manuellt.</span><span class="sxs-lookup"><span data-stu-id="1f520-261">The SDK can automatically interrupt the cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="1f520-262">Hantering av automatisk skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="1f520-262">Automatic poison message handling</span></span>
<span data-ttu-id="1f520-263">SDK: N ska anropa en funktion upp till 5 gånger för att bearbeta ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="1f520-263">The SDK will call a function up to 5 times to process a queue message.</span></span> <span data-ttu-id="1f520-264">Om den femte försök misslyckas, flyttas meddelandet till en skadligt kö.</span><span class="sxs-lookup"><span data-stu-id="1f520-264">If the fifth try fails, the message is moved to a poison queue.</span></span> <span data-ttu-id="1f520-265">[Det maximala antalet nya försök kan konfigureras](#config).</span><span class="sxs-lookup"><span data-stu-id="1f520-265">[The maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="1f520-266">Skadligt kön heter *{originalqueuename}*-skadligt.</span><span class="sxs-lookup"><span data-stu-id="1f520-266">The poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="1f520-267">Du kan skriva en funktion för att bearbeta meddelanden från skadligt kön av loggning av dem eller skicka ett meddelande till den manuella åtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="1f520-267">You can write a function to process messages from the poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="1f520-268">I följande exempel på `CopyBlob` funktionen kommer att misslyckas när ett kömeddelande innehåller namnet på en blob som inte finns.</span><span class="sxs-lookup"><span data-stu-id="1f520-268">In the following example the `CopyBlob` function will fail when a queue message contains the name of a blob that doesn't exist.</span></span> <span data-ttu-id="1f520-269">När det händer kan flyttas meddelandet från kön copyblobqueue till copyblobqueue poison kön.</span><span class="sxs-lookup"><span data-stu-id="1f520-269">When that happens, the message is moved from the copyblobqueue queue to the copyblobqueue-poison queue.</span></span> <span data-ttu-id="1f520-270">Den `ProcessPoisonMessage` loggar skadligt meddelande.</span><span class="sxs-lookup"><span data-stu-id="1f520-270">The `ProcessPoisonMessage` then logs the poison message.</span></span>

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
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

<span data-ttu-id="1f520-271">Följande bild visar konsolens utdata från de här funktionerna när ett skadligt meddelande bearbetas.</span><span class="sxs-lookup"><span data-stu-id="1f520-271">The following illustration shows console output from these functions when a poison message is processed.</span></span>

![Konsolens utdata för hantering av skadligt meddelande](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="1f520-273">Hantering av manuell skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="1f520-273">Manual poison message handling</span></span>
<span data-ttu-id="1f520-274">Du kan hämta antalet gånger som ett meddelande har plockats för bearbetning genom att lägga till en `int` parameter med namnet `dequeueCount` till funktionen.</span><span class="sxs-lookup"><span data-stu-id="1f520-274">You can get the number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` to your function.</span></span> <span data-ttu-id="1f520-275">Du kan kontrollera antalet dequeue i Funktionskoden och utföra egna skadligt meddelandehantering när antalet överskrider ett tröskelvärde som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-275">You can then check the dequeue count in function code and perform your own poison message handling when the number exceeds a threshold, as shown in the following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <span data-ttu-id="1f520-276"><a id="config"></a>Hur du ställer in konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="1f520-276"><a id="config"></a> How to set configuration options</span></span>
<span data-ttu-id="1f520-277">Du kan använda den `JobHostConfiguration` typen för att ange följande konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="1f520-277">You can use the `JobHostConfiguration` type to set the following configuration options:</span></span>

* <span data-ttu-id="1f520-278">Ange anslutningssträngar SDK i koden.</span><span class="sxs-lookup"><span data-stu-id="1f520-278">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="1f520-279">Konfigurera `QueueTrigger` inställningar, till exempel maximalt antal har status Created.</span><span class="sxs-lookup"><span data-stu-id="1f520-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="1f520-280">Hämta könamn från konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1f520-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="1f520-281"><a id="setconnstr"></a>Ange SDK anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="1f520-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="1f520-282">Ställa in anslutningssträngar SDK i koden kan du använda din egen anslutning sträng namn i konfigurationsfiler eller miljövariabler som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-282">Setting the SDK connection strings in code enables you to use your own connection string names in configuration files or environment variables, as shown in the following example.</span></span>

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

### <span data-ttu-id="1f520-283"><a id="configqueue"></a>Konfigurera inställningar för QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="1f520-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="1f520-284">Du kan konfigurera följande inställningar som gäller för meddelandebehandling kö:</span><span class="sxs-lookup"><span data-stu-id="1f520-284">You can configure the following settings that apply to the queue message processing:</span></span>

* <span data-ttu-id="1f520-285">Det maximala antalet Kömeddelanden som fångas upp samtidigt kan köras parallellt (standard är 16).</span><span class="sxs-lookup"><span data-stu-id="1f520-285">The maximum number of queue messages that are picked up simultaneously to be executed in parallel (default is 16).</span></span>
* <span data-ttu-id="1f520-286">Det maximala antalet försök innan ett kömeddelande skickas till ett skadligt kö (standardvärdet är 5).</span><span class="sxs-lookup"><span data-stu-id="1f520-286">The maximum number of retries before a queue message is sent to a poison queue (default is 5).</span></span>
* <span data-ttu-id="1f520-287">Maximal väntetid innan avsökning igen när en kö är tomt (standard är 1 minut).</span><span class="sxs-lookup"><span data-stu-id="1f520-287">The maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="1f520-288">I följande exempel visas hur du konfigurerar dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="1f520-288">The following example shows how to configure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="1f520-289"><a id="setnamesincode"></a>Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="1f520-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="1f520-290">Du vill ibland ange en ny kö, ett blob-namn eller en behållare eller en tabell i stället för att hårdkoda ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="1f520-290">Sometimes you want to specify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="1f520-291">Du kan till exempel vill ange namnet på kön för `QueueTrigger` i en konfiguration av fil- eller miljö variabel.</span><span class="sxs-lookup"><span data-stu-id="1f520-291">For example, you might want to specify the queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="1f520-292">Du kan göra det genom att passera i en `NameResolver` objekt till den `JobHostConfiguration` typen.</span><span class="sxs-lookup"><span data-stu-id="1f520-292">You can do that by passing in a `NameResolver` object to the `JobHostConfiguration` type.</span></span> <span data-ttu-id="1f520-293">Du inkludera särskilda platshållare omges av procenttecken (%) i WebJobs SDK attributet konstruktorn parametrar och dina `NameResolver` koden anger de faktiska värdena som ska användas i stället för dessa platshållare.</span><span class="sxs-lookup"><span data-stu-id="1f520-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies the actual values to be used in place of those placeholders.</span></span>

<span data-ttu-id="1f520-294">Anta att du vill använda en kö med namnet logqueuetest i testmiljön och en namngiven logqueueprod i produktion.</span><span class="sxs-lookup"><span data-stu-id="1f520-294">For example, suppose you want to use a queue named logqueuetest in the test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="1f520-295">I stället för en hårdkodad kö du vill ange namnet på en post i den `appSettings` samling som skulle ha faktiska könamnet.</span><span class="sxs-lookup"><span data-stu-id="1f520-295">Instead of a hard-coded queue name, you want to specify the name of an entry in the `appSettings` collection that would have the actual queue name.</span></span> <span data-ttu-id="1f520-296">Om den `appSettings` nyckeln är logqueue, din funktion kan se ut som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-296">If the `appSettings` key is logqueue, your function could look like the following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="1f520-297">Din `NameResolver` klassen kan sedan hämta könamnet från `appSettings` som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1f520-297">Your `NameResolver` class could then get the queue name from `appSettings` as shown in the following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="1f520-298">Du skickar den `NameResolver` klassen i att den `JobHost` objekt som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-298">You pass the `NameResolver` class in to the `JobHost` object as shown in the following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="1f520-299">**Obs:** kön tabell och blobbnamnen är löst varje gång som en funktion kallas men blob-behållaren namnmatchning bara när programmet startas.</span><span class="sxs-lookup"><span data-stu-id="1f520-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when the application starts.</span></span> <span data-ttu-id="1f520-300">Du kan inte ändra namnet för blob-behållare medan jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="1f520-300">You can't change blob container name while the job is running.</span></span>

## <span data-ttu-id="1f520-301"><a id="manual"></a>Hur du utlöser en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="1f520-301"><a id="manual"></a>How to trigger a function manually</span></span>
<span data-ttu-id="1f520-302">Utlös en funktion manuellt genom att använda den `Call` eller `CallAsync` -metoden i den `JobHost` objekt och `NoAutomaticTrigger` attributet för funktionen, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-302">To trigger a function manually, use the `Call` or `CallAsync` method on the `JobHost` object and the `NoAutomaticTrigger` attribute on the function, as shown in the following example.</span></span>

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

## <span data-ttu-id="1f520-303"><a id="logs"></a>Hur du skriver loggar</span><span class="sxs-lookup"><span data-stu-id="1f520-303"><a id="logs"></a>How to write logs</span></span>
<span data-ttu-id="1f520-304">Instrumentpanelen visar loggar på två platser: sidan för Webbjobbet, och på sidan för ett särskilt Webbjobb-anrop.</span><span class="sxs-lookup"><span data-stu-id="1f520-304">The Dashboard shows logs in two places: the page for the WebJob, and the page for a particular WebJob invocation.</span></span>

![Loggar Webbjobb på sidan](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="1f520-307">Utdata från konsolen metoder som anropas i en funktion eller i den `Main()` metoden visas på sidan instrumentpanelen för Webbjobbet, inte på sidan för en viss metodanropet.</span><span class="sxs-lookup"><span data-stu-id="1f520-307">Output from Console methods that you call in a function or in the `Main()` method appears in the Dashboard page for the WebJob, not in the page for a particular method invocation.</span></span> <span data-ttu-id="1f520-308">Utdata från TextWriter-objekt som du får från en parameter i Metodsignaturen visas på sidan instrumentpanelen för ett metodanrop.</span><span class="sxs-lookup"><span data-stu-id="1f520-308">Output from the TextWriter object that you get from a parameter in your method signature appears in the Dashboard page for a method invocation.</span></span>

<span data-ttu-id="1f520-309">Konsolens utdata kan inte länkas till en viss metodanropet eftersom konsolen är enkeltrådad, men många jobbfunktioner kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1f520-309">Console output can't be linked to a particular method invocation because the Console is single-threaded, while many job functions may be running at the same time.</span></span> <span data-ttu-id="1f520-310">Det är därför SDK innehåller varje funktionsanrop med sin egen unika loggen skrivarobjekt.</span><span class="sxs-lookup"><span data-stu-id="1f520-310">That's why the  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="1f520-311">Att skriva [spårning programloggarna](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), använda `Console.Out` (skapar loggar som är märkta som INFO) och `Console.Error` (skapar loggar som är märkta som fel).</span><span class="sxs-lookup"><span data-stu-id="1f520-311">To write [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="1f520-312">Ett alternativ är att använda [spårningen eller TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), som innehåller utförlig, varning, och kritiska nivåer utöver Info och fel.</span><span class="sxs-lookup"><span data-stu-id="1f520-312">An alternative is to use [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition to Info and Error.</span></span> <span data-ttu-id="1f520-313">Spårningsloggar för programmet visas i web app loggfilerna Azure-tabeller eller Azure BLOB-objekt beroende på hur du konfigurerar din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="1f520-313">Application tracing logs appear in the web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="1f520-314">Som gäller för alla konsolens utdata, visas senaste 100 programloggarna även på sidan instrumentpanelen för Webbjobb, inte sidan för ett funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="1f520-314">As is true of all Console output, the most recent 100 application logs also appear in the Dashboard page for the WebJob, not the page for a function invocation.</span></span>

<span data-ttu-id="1f520-315">Konsolens utdata visas i instrumentpanelen bara om programmet körs i en Azure-Webbjobb inte om programmet körs lokalt eller i en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="1f520-315">Console output appears in the Dashboard only if the program is running in an Azure WebJob, not if the program is running locally or in some other environment.</span></span>

<span data-ttu-id="1f520-316">Inaktivera loggning för instrumentpanelen för scenarier med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="1f520-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="1f520-317">Som standard SDK skriver loggar till lagring och den här aktiviteten kan försämra prestanda vid bearbetning av många meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f520-317">By default, the SDK writes logs to storage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="1f520-318">Om du vill inaktivera loggning, ange anslutningssträngen instrumentpanelen null som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1f520-318">To disable logging, set the dashboard connection string to null as shown in the following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="1f520-319">I följande exempel visar flera olika sätt att skriva loggar:</span><span class="sxs-lookup"><span data-stu-id="1f520-319">The following example shows several ways to write logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="1f520-320">I WebJobs SDK instrumentpanelen för utdata från den `TextWriter` objekt ser ut när du gå till sidan för en viss fungera anrop och klicka på **växla utdata**:</span><span class="sxs-lookup"><span data-stu-id="1f520-320">In the WebJobs SDK Dashboard, the output from the `TextWriter` object shows up when you go to the page for a particular function invocation and click **Toggle Output**:</span></span>

![Klicka på länken för anrop av funktionen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="1f520-323">I instrumentpanelen för WebJobs SDK senaste 100 raderna i konsolen utdata visar dig när du gå till sidan för Webbjobb (inte för funktionsanrop) och klicka på **växla utdata**.</span><span class="sxs-lookup"><span data-stu-id="1f520-323">In the WebJobs SDK Dashboard, the most recent 100 lines of Console output show up when you go to the page for the WebJob (not for the function invocation) and click **Toggle Output**.</span></span>

![Klicka på Visa/Dölj utdata](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="1f520-325">I ett kontinuerligt Webbjobb programloggarna visas i/data/jobb/kontinuerlig/*{webjobname}*/job_log.txt i filsystemet web app.</span><span class="sxs-lookup"><span data-stu-id="1f520-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in the web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="1f520-326">I ett Azure blob-program loggar ser ut så här: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fel, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="1f520-326">In an Azure blob the application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="1f520-327">Och i en Azure-tabellen i `Console.Out` och `Console.Error` loggar ut så här:</span><span class="sxs-lookup"><span data-stu-id="1f520-327">And in an Azure table the `Console.Out` and `Console.Error` logs look like this:</span></span>

![Loggen för information i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Felloggen i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="1f520-330">Om du vill ansluta din egen loggaren finns [det här exemplet](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="1f520-330">If you want to plug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="1f520-331"><a id="errors"></a>Hantera fel och konfigurera tidsgränser</span><span class="sxs-lookup"><span data-stu-id="1f520-331"><a id="errors"></a>How to handle errors and configure timeouts</span></span>
<span data-ttu-id="1f520-332">WebJobs SDK innehåller också en [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribut som du kan använda för att en funktion som ska avbrytas om inte slutföra inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="1f520-332">The WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use to cause a function to be canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="1f520-333">Och om du vill aktivera en varning när för många fel som inträffar inom en angiven tidsperiod, kan du använda den `ErrorTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="1f520-333">And if you want to raise an alert when too many errors happen within a specified period of time, you can use the `ErrorTrigger` attribute.</span></span> <span data-ttu-id="1f520-334">Här är en [ErrorTrigger exempel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="1f520-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="1f520-335">Du kan också dynamiskt inaktiverar och aktiverar funktioner för att kontrollera om de kan utlösas, med hjälp av en konfigurationsväxel som kan vara en appinställning eller Miljövariabelns namn.</span><span class="sxs-lookup"><span data-stu-id="1f520-335">You can also dynamically disable and enable functions to control whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="1f520-336">Exempelkod finns i `Disable` attribut i [WebJobs SDK exempel databasen](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="1f520-336">For sample code, see the `Disable` attribute in [the WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="1f520-337"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f520-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="1f520-338">Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure köer.</span><span class="sxs-lookup"><span data-stu-id="1f520-338">This guide has provided code samples that show how to handle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="1f520-339">Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="1f520-339">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
