---
title: "aaaGetting igång med queue storage- och Visual Studio anslutna tjänster (Webbjobb projekt) | Microsoft Docs"
description: "Hur tooget igång med Azure Queue storage i ett Webbjobb projekt när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 720e96fda734c31e1b1d68d4f95aa9531a20a3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="d10c3-103">Komma igång med Azure Queue storage och Visual Studio anslutna tjänster (Webbjobb projekt)</span><span class="sxs-lookup"><span data-stu-id="d10c3-103">Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="d10c3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d10c3-104">Overview</span></span>
<span data-ttu-id="d10c3-105">Den här artikeln beskriver hur få igång med Azure Queue storage i Visual Studio Azure Webjobs-projekt när du har skapat eller refererar till ett Azure storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d10c3-105">This article describes how get started using Azure Queue storage in a Visual Studio Azure WebJob project after you have created or referenced an Azure storage account by using hello Visual Studio  **Add Connected Services** dialog box.</span></span> <span data-ttu-id="d10c3-106">När du lägger till ett storage-konto tooa Webbjobb projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan hello lämplig Azure Storage NuGet-paket installeras, hello lämpliga .NET-referenserna är tillagda toohello projektet och anslutningssträngar för hello storage-konto uppdateras i hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="d10c3-106">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet packages are installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>  

<span data-ttu-id="d10c3-107">Den här artikeln innehåller C#-kodexempel som visar hur toouse hello Azure WebJobs SDK version 1.x med hello Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d10c3-107">This article provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure Queue storage service.</span></span>

<span data-ttu-id="d10c3-108">Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d10c3-108">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="d10c3-109">Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d10c3-109">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span> <span data-ttu-id="d10c3-110">Se [Kom igång med Azure Queue Storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d10c3-110">See [Get started with Azure Queue Storage using .NET](storage-dotnet-how-to-use-queues.md) for more information.</span></span> <span data-ttu-id="d10c3-111">Läs mer om ASP.NET [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="d10c3-111">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a><span data-ttu-id="d10c3-112">Hur tootrigger en funktion när ett kömeddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="d10c3-112">How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="d10c3-113">toowrite anropar en funktion som hello WebJobs SDK när ett kömeddelande tas emot använder hello **QueueTrigger** attribut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-113">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello **QueueTrigger** attribute.</span></span> <span data-ttu-id="d10c3-114">hello Attributkonstruktorn använder en strängparameter som anger hello kön toopoll hello namn.</span><span class="sxs-lookup"><span data-stu-id="d10c3-114">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="d10c3-115">toosee hur tooset hello könamnet dynamiskt, kolla [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-115">toosee how tooset hello queue name dynamically, check out [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="d10c3-116">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="d10c3-116">String queue messages</span></span>
<span data-ttu-id="d10c3-117">I följande exempel hello, hello kön innehåller ett strängmeddelande, så **QueueTrigger** är tillämpade tooa strängparameter med namnet **logMessage** som innehåller hello innehållet hello kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-117">In hello following example, hello queue contains a string message, so **QueueTrigger** is applied tooa string parameter named **logMessage** which contains hello content of hello queue message.</span></span> <span data-ttu-id="d10c3-118">Hej funktionen [skriver en logg meddelandet toohello instrumentpanelen](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="d10c3-118">hello function [writes a log message toohello Dashboard](#how-to-write-logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="d10c3-119">Förutom **sträng**, hello kan vara en bytematris en **CloudQueueMessage** objekt eller en POCO som du definierar.</span><span class="sxs-lookup"><span data-stu-id="d10c3-119">Besides **string**, hello parameter may be a byte array, a **CloudQueueMessage** object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="d10c3-120">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="d10c3-120">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="d10c3-121">I följande exempel hello, hello kön meddelandet innehåller JSON för en **BlobInformation** objekt som innehåller en **BlobName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d10c3-121">In hello following example, hello queue message contains JSON for a **BlobInformation** object which includes a **BlobName** property.</span></span> <span data-ttu-id="d10c3-122">hello SDK deserializes automatiskt hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-122">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="d10c3-123">hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d10c3-123">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="d10c3-124">Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="d10c3-124">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="d10c3-125">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="d10c3-125">Async functions</span></span>
<span data-ttu-id="d10c3-126">hello följande async-funktionen [skriver en logg toohello instrumentpanelen](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="d10c3-126">hello following async function [writes a log toohello Dashboard](#how-to-write-logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="d10c3-127">Asynkrona funktioner kan ta en [annullering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)som visas i följande exempel som kopierar en blobb hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-127">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="d10c3-128">(En förklaring av hello **queueTrigger** platshållare finns hello [Blobbar](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="d10c3-128">(For an explanation of hello **queueTrigger** placeholder, see hello [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a><span data-ttu-id="d10c3-129">Typer hello QueueTrigger attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="d10c3-129">Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="d10c3-130">Du kan använda **QueueTrigger** med hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="d10c3-130">You can use **QueueTrigger** with hello following types:</span></span>

* <span data-ttu-id="d10c3-131">**sträng**</span><span class="sxs-lookup"><span data-stu-id="d10c3-131">**string**</span></span>
* <span data-ttu-id="d10c3-132">En POCO-typen som serialiseras som JSON</span><span class="sxs-lookup"><span data-stu-id="d10c3-132">A POCO type serialized as JSON</span></span>
* <span data-ttu-id="d10c3-133">**byte]**</span><span class="sxs-lookup"><span data-stu-id="d10c3-133">**byte[]**</span></span>
* <span data-ttu-id="d10c3-134">**CloudQueueMessage**</span><span class="sxs-lookup"><span data-stu-id="d10c3-134">**CloudQueueMessage**</span></span>

## <a name="polling-algorithm"></a><span data-ttu-id="d10c3-135">Avsökningen algoritm</span><span class="sxs-lookup"><span data-stu-id="d10c3-135">Polling algorithm</span></span>
<span data-ttu-id="d10c3-136">hello SDK implementerar en exponentiell tillbaka av algoritmen tooreduce hello effekten av inaktiv-kön avsökning på transaktion lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="d10c3-136">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="d10c3-137">När ett meddelande hittas hello SDK väntar två sekunder och söker efter en annan message; När det finns inget meddelande väntar på fyra sekunder innan du försöker igen.</span><span class="sxs-lookup"><span data-stu-id="d10c3-137">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="d10c3-138">När efterföljande misslyckade försök tooget ett kömeddelande hello väntetiden fortsätter tooincrease tills den når maximal väntetid för hello, vilka standardvärden tooone minut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-138">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="d10c3-139">[hello väntetiden konfigureras](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-139">[hello maximum wait time is configurable](#how-to-set-configuration-options).</span></span>

## <a name="multiple-instances"></a><span data-ttu-id="d10c3-140">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="d10c3-140">Multiple instances</span></span>
<span data-ttu-id="d10c3-141">Om ditt webbprogram som körs på flera instanser, kontinuerliga Webbjobb som körs på varje dator och varje dator ska vänta för utlösare och försök toorun funktioner.</span><span class="sxs-lookup"><span data-stu-id="d10c3-141">If your web app runs on multiple instances, a continuous WebJobs runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="d10c3-142">I vissa fall kan detta leda toosome funktioner bearbetning hello samma data två gånger så funktioner ska vara idempotent (skrivs så som anropar dem flera gånger med samma indata inte ger hello duplicera resultat).</span><span class="sxs-lookup"><span data-stu-id="d10c3-142">In some scenarios this can lead toosome functions processing hello same data twice, so functions should be idempotent (written so that calling them repeatedly with hello same input data doesn't produce duplicate results).</span></span>  

## <a name="parallel-execution"></a><span data-ttu-id="d10c3-143">Parallell körning</span><span class="sxs-lookup"><span data-stu-id="d10c3-143">Parallel execution</span></span>
<span data-ttu-id="d10c3-144">Om du har flera funktioner som lyssnar på olika köer anropar hello SDK dem parallellt när meddelanden tas emot samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-144">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="d10c3-145">hello sak samma gäller när flera meddelanden tas emot för en enskild kö.</span><span class="sxs-lookup"><span data-stu-id="d10c3-145">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="d10c3-146">Som standard hello SDK hämtar en batch med 16 Kömeddelanden i taget och utför hello-funktion som behandlar dem parallellt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-146">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="d10c3-147">[hello storlek är konfigurerbar](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-147">[hello batch size is configurable](#how-to-set-configuration-options).</span></span> <span data-ttu-id="d10c3-148">När hello nummer bearbetas hämtar ned toohalf av hello batchstorlek, hämtar en annan batch hello SDK och påbörjar bearbetningen av dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d10c3-148">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="d10c3-149">Hello maximalt antal samtidiga meddelanden som bearbetas per funktion är därför en och en halv gånger hello batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="d10c3-149">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="d10c3-150">Den här begränsningen gäller separat tooeach funktion som har en **QueueTrigger** attribut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-150">This limit applies separately tooeach function that has a **QueueTrigger** attribute.</span></span> <span data-ttu-id="d10c3-151">Om du inte vill parallell körning för meddelanden som tas emot i en kö kan du ange hello batch storlek too1.</span><span class="sxs-lookup"><span data-stu-id="d10c3-151">If you don't want parallel execution for messages received on one queue, set hello batch size too1.</span></span>

## <a name="get-queue-or-queue-message-metadata"></a><span data-ttu-id="d10c3-152">Hämta kön eller kön meddelandet metadata</span><span class="sxs-lookup"><span data-stu-id="d10c3-152">Get queue or queue message metadata</span></span>
<span data-ttu-id="d10c3-153">Du kan få följande meddelandeegenskaper genom att lägga till parametrar toohello Metodsignaturen hello:</span><span class="sxs-lookup"><span data-stu-id="d10c3-153">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="d10c3-154">**DateTimeOffset** expirationTime</span><span class="sxs-lookup"><span data-stu-id="d10c3-154">**DateTimeOffset** expirationTime</span></span>
* <span data-ttu-id="d10c3-155">**DateTimeOffset** insertionTime</span><span class="sxs-lookup"><span data-stu-id="d10c3-155">**DateTimeOffset** insertionTime</span></span>
* <span data-ttu-id="d10c3-156">**DateTimeOffset** nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="d10c3-156">**DateTimeOffset** nextVisibleTime</span></span>
* <span data-ttu-id="d10c3-157">**strängen** queueTrigger (innehåller meddelandetext)</span><span class="sxs-lookup"><span data-stu-id="d10c3-157">**string** queueTrigger (contains message text)</span></span>
* <span data-ttu-id="d10c3-158">**strängen** id</span><span class="sxs-lookup"><span data-stu-id="d10c3-158">**string** id</span></span>
* <span data-ttu-id="d10c3-159">**strängen** popReceipt</span><span class="sxs-lookup"><span data-stu-id="d10c3-159">**string** popReceipt</span></span>
* <span data-ttu-id="d10c3-160">**int** dequeueCount</span><span class="sxs-lookup"><span data-stu-id="d10c3-160">**int** dequeueCount</span></span>

<span data-ttu-id="d10c3-161">Om du vill toowork direkt med hello Azure storage-API, du kan också lägga till en **CloudStorageAccount** parameter.</span><span class="sxs-lookup"><span data-stu-id="d10c3-161">If you want toowork directly with hello Azure storage API, you can also add a **CloudStorageAccount** parameter.</span></span>

<span data-ttu-id="d10c3-162">hello skriver följande exempel alla denna metadata tooan INFO programloggen.</span><span class="sxs-lookup"><span data-stu-id="d10c3-162">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="d10c3-163">I exemplet hello innehåll både logMessage och queueTrigger hello av hälsningsmeddelande för kön.</span><span class="sxs-lookup"><span data-stu-id="d10c3-163">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

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

<span data-ttu-id="d10c3-164">Här är en exempellogg som skrivits av hello exempelkod:</span><span class="sxs-lookup"><span data-stu-id="d10c3-164">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a><span data-ttu-id="d10c3-165">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="d10c3-165">Graceful shutdown</span></span>
<span data-ttu-id="d10c3-166">En funktion som körs i ett kontinuerligt Webbjobb kan acceptera en **CancellationToken** parameter som aktiverar hello operativsystemet toonotify hello fungerar när hello Webbjobb kommer toobe avslutades.</span><span class="sxs-lookup"><span data-stu-id="d10c3-166">A function that runs in a continuous WebJob can accept a **CancellationToken** parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="d10c3-167">Du kan använda det här meddelandet toomake att hello funktionen inte avslutas oväntat på ett sätt som lämnar data i ett inkonsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d10c3-167">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="d10c3-168">följande exempel visar hur hello toocheck för nära förestående Webbjobb avslutning i en funktion.</span><span class="sxs-lookup"><span data-stu-id="d10c3-168">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="d10c3-169">**Obs:** hello instrumentpanelen kanske inte korrekt visar hello status och utdata för funktioner som har stängts av.</span><span class="sxs-lookup"><span data-stu-id="d10c3-169">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="d10c3-170">Mer information finns i [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="d10c3-170">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a><span data-ttu-id="d10c3-171">Hur toocreate en kö meddelande vid bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="d10c3-171">How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="d10c3-172">toowrite en funktion som skapar ett nytt meddelande i kön, Använd hello **kön** attribut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-172">toowrite a function that creates a new queue message, use hello **Queue** attribute.</span></span> <span data-ttu-id="d10c3-173">Som **QueueTrigger**kan du skicka in hello könamnet som en sträng eller [ange hello könamnet dynamiskt](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-173">Like **QueueTrigger**, you pass in hello queue name as a string or you can [set hello queue name dynamically](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="d10c3-174">Sträng Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="d10c3-174">String queue messages</span></span>
<span data-ttu-id="d10c3-175">hello följande kodexempel icke asynkron skapar ett nytt meddelande i kön i hello kö med namnet ”outputqueue” med hello samma innehåll som kön hello-meddelande togs emot i hello kö med namnet ”inputqueue”.</span><span class="sxs-lookup"><span data-stu-id="d10c3-175">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="d10c3-176">(För asynkrona funktioner använder **IAsyncCollector<T>**  enligt senare i det här avsnittet.)</span><span class="sxs-lookup"><span data-stu-id="d10c3-176">(For async functions use **IAsyncCollector<T>** as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="d10c3-177">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="d10c3-177">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="d10c3-178">toocreate ett kömeddelande som innehåller en POCO i stället för en sträng, pass hello POCO skriver som en output-parameter toohello **kön** Attributkonstruktorn.</span><span class="sxs-lookup"><span data-stu-id="d10c3-178">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello **Queue** attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="d10c3-179">hello SDK Serialiserar automatiskt hello objektet tooJSON.</span><span class="sxs-lookup"><span data-stu-id="d10c3-179">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="d10c3-180">Ett kömeddelande skapas alltid, även om hello-objektet är null.</span><span class="sxs-lookup"><span data-stu-id="d10c3-180">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="d10c3-181">Skapa flera meddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="d10c3-181">Create multiple messages or in async functions</span></span>
<span data-ttu-id="d10c3-182">toocreate flera meddelanden gör hello parametertypen för hello utgående kö **ICollector<T>**  eller **IAsyncCollector<T>**som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-182">toocreate multiple messages, make hello parameter type for hello output queue **ICollector<T>** or **IAsyncCollector<T>**, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="d10c3-183">Varje meddelande i kön skapas omedelbart när hello **Lägg till** -metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="d10c3-183">Each queue message is created immediately when hello **Add** method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="d10c3-184">Typer hello kön attributet fungerar med</span><span class="sxs-lookup"><span data-stu-id="d10c3-184">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="d10c3-185">Du kan använda hello **kön** attributet på hello följande parametertyper:</span><span class="sxs-lookup"><span data-stu-id="d10c3-185">You can use hello **Queue** attribute on hello following parameter types:</span></span>

* <span data-ttu-id="d10c3-186">**i strängen** (skapar kömeddelande om parametervärdet är icke-null när hello funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="d10c3-186">**out string** (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="d10c3-187">**ut byte []** (fungerar som **sträng**)</span><span class="sxs-lookup"><span data-stu-id="d10c3-187">**out byte[]** (works like **string**)</span></span>
* <span data-ttu-id="d10c3-188">**ut CloudQueueMessage** (fungerar som **sträng**)</span><span class="sxs-lookup"><span data-stu-id="d10c3-188">**out CloudQueueMessage** (works like **string**)</span></span>
* <span data-ttu-id="d10c3-189">**ut POCO** (en serialiserbar typ. skapar ett meddelande med ett null-objekt om hello parameter är null när hello funktionen avslutas)</span><span class="sxs-lookup"><span data-stu-id="d10c3-189">**out POCO** (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* <span data-ttu-id="d10c3-190">**ICollector**</span><span class="sxs-lookup"><span data-stu-id="d10c3-190">**ICollector**</span></span>
* <span data-ttu-id="d10c3-191">**IAsyncCollector**</span><span class="sxs-lookup"><span data-stu-id="d10c3-191">**IAsyncCollector**</span></span>
* <span data-ttu-id="d10c3-192">**CloudQueue** (för att skapa meddelanden manuellt med hjälp av hello Azure Storage-API direkt)</span><span class="sxs-lookup"><span data-stu-id="d10c3-192">**CloudQueue** (for creating messages manually using hello Azure Storage API directly)</span></span>

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a><span data-ttu-id="d10c3-193">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="d10c3-193">Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="d10c3-194">Om du behöver toodo vissa fungerar i din funktion innan du använder ett WebJobs-SDK-attribut som **kön**, **Blob**, eller **tabell**, du kan använda hello **IBinder** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-194">If you need toodo some work in your function before using a WebJobs SDK attribute such as **Queue**, **Blob**, or **Table**, you can use hello **IBinder** interface.</span></span>

<span data-ttu-id="d10c3-195">hello följande exempel tar ett inkommande kö-meddelande och skapar ett nytt meddelande med hello samma innehåll i en utgående kö.</span><span class="sxs-lookup"><span data-stu-id="d10c3-195">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="d10c3-196">hello utgående kö namn anges av koden i hello brödtexten i hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="d10c3-196">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="d10c3-197">Hej **IBinder** gränssnitt kan även användas med hello **tabell** och **Blob** attribut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-197">hello **IBinder** interface can also be used with hello **Table** and **Blob** attributes.</span></span>

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a><span data-ttu-id="d10c3-198">Hur tooread och skriva BLOB-objekt och tabeller under bearbetning av ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="d10c3-198">How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="d10c3-199">Hej **Blob** och **tabell** attribut aktivera tooread och skriva BLOB och tabeller.</span><span class="sxs-lookup"><span data-stu-id="d10c3-199">hello **Blob** and **Table** attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="d10c3-200">hello prover i det här avsnittet gäller tooblobs.</span><span class="sxs-lookup"><span data-stu-id="d10c3-200">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="d10c3-201">Kodexempel som visar hur tootrigger bearbetar när blobbar har skapats eller uppdaterats, se [hur toouse Azure blob storage med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), och kodexempel som läsa och skriva tabeller, se [hur toouse Azure-tabellen lagring med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="d10c3-201">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="d10c3-202">Sträng Kömeddelanden utlösa blob-åtgärder</span><span class="sxs-lookup"><span data-stu-id="d10c3-202">String queue messages triggering blob operations</span></span>
<span data-ttu-id="d10c3-203">För ett meddelande i kön som innehåller en sträng, **queueTrigger** är en platshållare som du kan använda i hello **Blob** attributets **blobPath** parametrar som innehåller hello innehållet i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d10c3-203">For a queue message that contains a string, **queueTrigger** is a placeholder you can use in hello **Blob** attribute's **blobPath** parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="d10c3-204">hello följande exempel används **dataströmmen** tooread och skriva BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-204">hello following example uses **Stream** objects tooread and write blobs.</span></span> <span data-ttu-id="d10c3-205">hälsningsmeddelande för kön är en blob som finns i hello textblobs behållare hello namn.</span><span class="sxs-lookup"><span data-stu-id="d10c3-205">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="d10c3-206">En kopia av hello blob med ”-nya” tillagda toohello namnet skapas i hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="d10c3-206">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="d10c3-207">Hej **Blob** attributet konstruktorn tar en **blobPath** parameter som anger hello-behållaren och blobbnamnet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-207">hello **Blob** attribute constructor takes a **blobPath** parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="d10c3-208">Mer information om den här platshållaren finns [hur toouse Azure blob storage med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="d10c3-208">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span></span>

<span data-ttu-id="d10c3-209">När attributet hello decorates en **dataströmmen** objekt, en annan konstruktorparametern anger hello **FileAccess** läge som Läs-, Skriv- eller läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="d10c3-209">When hello attribute decorates a **Stream** object, another constructor parameter specifies hello **FileAccess** mode as read, write, or read/write.</span></span>

<span data-ttu-id="d10c3-210">hello följande exempel används en **CloudBlockBlob** objekt toodelete en blob.</span><span class="sxs-lookup"><span data-stu-id="d10c3-210">hello following example uses a **CloudBlockBlob** object toodelete a blob.</span></span> <span data-ttu-id="d10c3-211">Hej kömeddelande är hello namn hello-blob.</span><span class="sxs-lookup"><span data-stu-id="d10c3-211">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="d10c3-212">POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="d10c3-212">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="d10c3-213">Du kan använda platshållare att egenskaperna för hello objekt i hello-namnet för en POCO som lagras som JSON i kön hälsningsmeddelande **kön** attributets **blobPath** parameter.</span><span class="sxs-lookup"><span data-stu-id="d10c3-213">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello **Queue** attribute's **blobPath** parameter.</span></span> <span data-ttu-id="d10c3-214">Du kan också använda kön metadata egenskapsnamn som platshållare.</span><span class="sxs-lookup"><span data-stu-id="d10c3-214">You can also use queue metadata property names as placeholders.</span></span> <span data-ttu-id="d10c3-215">Se [hämta kön eller kön meddelandet metadata](#get-queue-or-queue-message-metadata).</span><span class="sxs-lookup"><span data-stu-id="d10c3-215">See [Get queue or queue message metadata](#get-queue-or-queue-message-metadata).</span></span>

<span data-ttu-id="d10c3-216">hello kopieras följande exempel en blob tooa nya blob med ett annat tillägg.</span><span class="sxs-lookup"><span data-stu-id="d10c3-216">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="d10c3-217">hälsningsmeddelande för kön är en **BlobInformation** -objekt som innehåller **BlobName** och **BlobNameWithoutExtension** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d10c3-217">hello queue message is a **BlobInformation** object that includes **BlobName** and **BlobNameWithoutExtension** properties.</span></span> <span data-ttu-id="d10c3-218">hello egenskapsnamn används som platshållare i hello blob sökväg för hello **Blob** attribut.</span><span class="sxs-lookup"><span data-stu-id="d10c3-218">hello property names are used as placeholders in hello blob path for hello **Blob** attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="d10c3-219">hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d10c3-219">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="d10c3-220">Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.</span><span class="sxs-lookup"><span data-stu-id="d10c3-220">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="d10c3-221">Om du behöver toodo vissa fungerar i din funktion innan du binder en tooan blob-objektet, du kan använda hello-attributet i hello brödtext hello funktionen enligt [använder WebJobs-SDK-attribut i hello brödtext](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span><span class="sxs-lookup"><span data-stu-id="d10c3-221">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, as shown in [Use WebJobs SDK attributes in hello body of a function](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span></span>

### <a name="types-you-can-use-hello-blob-attribute-with"></a><span data-ttu-id="d10c3-222">Du kan använda hello Blob-attribut med</span><span class="sxs-lookup"><span data-stu-id="d10c3-222">Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="d10c3-223">Hej **Blob** attributet kan användas med hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="d10c3-223">hello **Blob** attribute can be used with hello following types:</span></span>

* <span data-ttu-id="d10c3-224">**Dataströmmen** (läsa eller skriva som anges av parametern hello FileAccess konstruktor)</span><span class="sxs-lookup"><span data-stu-id="d10c3-224">**Stream** (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* <span data-ttu-id="d10c3-225">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="d10c3-225">**TextReader**</span></span>
* <span data-ttu-id="d10c3-226">**TextWriter**</span><span class="sxs-lookup"><span data-stu-id="d10c3-226">**TextWriter**</span></span>
* <span data-ttu-id="d10c3-227">**strängen** (läsa)</span><span class="sxs-lookup"><span data-stu-id="d10c3-227">**string** (read)</span></span>
* <span data-ttu-id="d10c3-228">**i strängen** (skriva; skapar en blob endast om hello strängparameter är icke-null om hello funktionen returnerar)</span><span class="sxs-lookup"><span data-stu-id="d10c3-228">**out string** (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="d10c3-229">POCO (läsa)</span><span class="sxs-lookup"><span data-stu-id="d10c3-229">POCO (read)</span></span>
* <span data-ttu-id="d10c3-230">ut POCO (skriva; alltid skapar en blob, skapar som null-objekt om POCO-parametern är null när hello funktionen returnerar)</span><span class="sxs-lookup"><span data-stu-id="d10c3-230">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="d10c3-231">**CloudBlobStream** (Skriv)</span><span class="sxs-lookup"><span data-stu-id="d10c3-231">**CloudBlobStream** (write)</span></span>
* <span data-ttu-id="d10c3-232">**ICloudBlob** (läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="d10c3-232">**ICloudBlob** (read or write)</span></span>
* <span data-ttu-id="d10c3-233">**CloudBlockBlob** (läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="d10c3-233">**CloudBlockBlob** (read or write)</span></span>
* <span data-ttu-id="d10c3-234">**CloudPageBlob** (läsa eller skriva)</span><span class="sxs-lookup"><span data-stu-id="d10c3-234">**CloudPageBlob** (read or write)</span></span>

## <a name="how-toohandle-poison-messages"></a><span data-ttu-id="d10c3-235">Hur toohandle förgiftade meddelanden</span><span class="sxs-lookup"><span data-stu-id="d10c3-235">How toohandle poison messages</span></span>
<span data-ttu-id="d10c3-236">Meddelanden vars innehåll orsakar en funktionen toofail kallas *förgiftade meddelanden*.</span><span class="sxs-lookup"><span data-stu-id="d10c3-236">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="d10c3-237">När hello funktionen misslyckas hello kön meddelandet tas inte bort och slutligen hämtas igen, vilket ledde hello cykel toobe upprepas.</span><span class="sxs-lookup"><span data-stu-id="d10c3-237">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="d10c3-238">hello SDK kan avbryta hello cykel efter ett begränsat antal upprepningar eller så kan du göra det manuellt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-238">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="d10c3-239">Hantering av automatisk skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="d10c3-239">Automatic poison message handling</span></span>
<span data-ttu-id="d10c3-240">hello SDK ska anropa en funktion in too5 gånger tooprocess ett kömeddelande.</span><span class="sxs-lookup"><span data-stu-id="d10c3-240">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="d10c3-241">Om hello femte försök misslyckas är hello-meddelande har flyttats tooa skadligt kö.</span><span class="sxs-lookup"><span data-stu-id="d10c3-241">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="d10c3-242">Du kan se hur tooconfigure hello maximalt antal försök i [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-242">You can see how tooconfigure hello maximum number of retries in [How tooset configuration options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="d10c3-243">hello skadligt kön heter *{originalqueuename}*-skadligt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-243">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="d10c3-244">Du kan skriva en funktion tooprocess meddelanden från hello skadligt kö med loggning av dem eller skicka ett meddelande om att manuella åtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="d10c3-244">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="d10c3-245">I följande exempel hello hello **CopyBlob** funktionen kommer att misslyckas när ett kömeddelande innehåller hello namnet på en blob som inte finns.</span><span class="sxs-lookup"><span data-stu-id="d10c3-245">In hello following example hello **CopyBlob** function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="d10c3-246">När det händer kan flyttas hello-meddelande från hello copyblobqueue kön toohello copyblobqueue poison kö.</span><span class="sxs-lookup"><span data-stu-id="d10c3-246">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="d10c3-247">Hej **ProcessPoisonMessage** och sedan loggar hello skadligt meddelande.</span><span class="sxs-lookup"><span data-stu-id="d10c3-247">hello **ProcessPoisonMessage** then logs hello poison message.</span></span>

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

<span data-ttu-id="d10c3-248">hello följande bild visar konsolens utdata från de här funktionerna när ett skadligt meddelande bearbetas.</span><span class="sxs-lookup"><span data-stu-id="d10c3-248">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Konsolens utdata för hantering av skadligt meddelande](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="d10c3-250">Hantering av manuell skadligt meddelande</span><span class="sxs-lookup"><span data-stu-id="d10c3-250">Manual poison message handling</span></span>
<span data-ttu-id="d10c3-251">Du kan hämta hello många gånger meddelandet har plockats för bearbetning genom att lägga till en **int** parameter med namnet **dequeueCount** tooyour funktion.</span><span class="sxs-lookup"><span data-stu-id="d10c3-251">You can get hello number of times a message has been picked up for processing by adding an **int** parameter named **dequeueCount** tooyour function.</span></span> <span data-ttu-id="d10c3-252">Därefter kan du kontrollera hello status Created antalet i Funktionskoden och utföra egna skadligt meddelandehantering när hello antalet överskrider ett tröskelvärde som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-252">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

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

## <a name="how-tooset-configuration-options"></a><span data-ttu-id="d10c3-253">Hur tooset konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="d10c3-253">How tooset configuration options</span></span>
<span data-ttu-id="d10c3-254">Du kan använda hello **JobHostConfiguration** typen tooset hello följande konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="d10c3-254">You can use hello **JobHostConfiguration** type tooset hello following configuration options:</span></span>

* <span data-ttu-id="d10c3-255">Ange hello SDK-anslutningssträngar i koden.</span><span class="sxs-lookup"><span data-stu-id="d10c3-255">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="d10c3-256">Konfigurera **QueueTrigger** inställningar, till exempel maximalt antal har status Created.</span><span class="sxs-lookup"><span data-stu-id="d10c3-256">Configure **QueueTrigger** settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="d10c3-257">Hämta könamn från konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d10c3-257">Get queue names from configuration.</span></span>

### <a name="set-sdk-connection-strings-in-code"></a><span data-ttu-id="d10c3-258">Ange SDK anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="d10c3-258">Set SDK connection strings in code</span></span>
<span data-ttu-id="d10c3-259">Hello SDK-anslutningssträngar kan i koden du toouse egna namn på anslutning sträng i configuration-filer eller miljövariabler som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-259">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

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

### <a name="configure-queuetrigger--settings"></a><span data-ttu-id="d10c3-260">Konfigurera inställningar för QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="d10c3-260">Configure QueueTrigger  settings</span></span>
<span data-ttu-id="d10c3-261">Du kan konfigurera följande inställningar som gäller toohello kön meddelandebehandling hello:</span><span class="sxs-lookup"><span data-stu-id="d10c3-261">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="d10c3-262">Maximalt antal meddelanden i kö som tas upp samtidigt toobe utförs parallellt hello (standard är 16).</span><span class="sxs-lookup"><span data-stu-id="d10c3-262">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="d10c3-263">Hej maximalt antal försök innan ett kömeddelande skickas tooa skadligt kön (standardvärdet är 5).</span><span class="sxs-lookup"><span data-stu-id="d10c3-263">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="d10c3-264">hello väntetiden innan avsökning igen när en kö är tomt (standard är 1 minut).</span><span class="sxs-lookup"><span data-stu-id="d10c3-264">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="d10c3-265">följande exempel visar hur hello tooconfigure dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="d10c3-265">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a><span data-ttu-id="d10c3-266">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="d10c3-266">Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="d10c3-267">Vill ibland du toospecify en kö, ett blob-namn eller en behållare eller en tabell i stället för att hårdkoda ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-267">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="d10c3-268">Du kan exempelvis toospecify hello könamnet för **QueueTrigger** i en konfiguration av fil- eller miljö variabel.</span><span class="sxs-lookup"><span data-stu-id="d10c3-268">For example, you might want toospecify hello queue name for **QueueTrigger** in a configuration file or environment variable.</span></span>

<span data-ttu-id="d10c3-269">Du kan göra det genom att passera i en **NameResolver** objekt toohello **JobHostConfiguration** typen.</span><span class="sxs-lookup"><span data-stu-id="d10c3-269">You can do that by passing in a **NameResolver** object toohello **JobHostConfiguration** type.</span></span> <span data-ttu-id="d10c3-270">Du inkludera särskilda platshållare omges av procenttecken (%) i WebJobs SDK attributet konstruktorn parametrar och **NameResolver** koden anger hello faktiska värden toobe används i stället för dessa platshållare.</span><span class="sxs-lookup"><span data-stu-id="d10c3-270">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your **NameResolver** code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="d10c3-271">Till exempel anta att du vill toouse en kö med namnet logqueuetest i hello testmiljö och en namngiven logqueueprod i produktion.</span><span class="sxs-lookup"><span data-stu-id="d10c3-271">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="d10c3-272">I stället för en hårdkodad könamnet önskade toospecify hello namnet på en post i hello **appSettings** samling som skulle ha hello faktiska könamnet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-272">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello **appSettings** collection that would have hello actual queue name.</span></span> <span data-ttu-id="d10c3-273">Om hello **appSettings** nyckeln är logqueue, din funktion kan se ut så hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d10c3-273">If hello **appSettings** key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="d10c3-274">Din **NameResolver** klassen kan sedan hämta hello könamnet från **appSettings** som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="d10c3-274">Your **NameResolver** class could then get hello queue name from **appSettings** as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="d10c3-275">Du skickar hello **NameResolver** klassen i toohello **JobHost** objekt som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-275">You pass hello **NameResolver** class in toohello **JobHost** object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="d10c3-276">**Obs:** kön tabell och blobbnamnen är löst varje gång som en funktion kallas, men blob-behållaren namnmatchning bara när hello programmet startas.</span><span class="sxs-lookup"><span data-stu-id="d10c3-276">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="d10c3-277">Du kan inte ändra namnet för blob-behållare medan hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="d10c3-277">You can't change blob container name while hello job is running.</span></span>

## <a name="how-tootrigger-a-function-manually"></a><span data-ttu-id="d10c3-278">Hur tootrigger en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="d10c3-278">How tootrigger a function manually</span></span>
<span data-ttu-id="d10c3-279">tootrigger en funktion manuellt, använda hello **anropa** eller **CallAsync** metod på hello **JobHost** objektet och hello **NoAutomaticTrigger** attribut i hello funktion, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d10c3-279">tootrigger a function manually, use hello **Call** or **CallAsync** method on hello **JobHost** object and hello **NoAutomaticTrigger** attribute on hello function, as shown in hello following example.</span></span>

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

## <a name="how-toowrite-logs"></a><span data-ttu-id="d10c3-280">Hur toowrite loggar</span><span class="sxs-lookup"><span data-stu-id="d10c3-280">How toowrite logs</span></span>
<span data-ttu-id="d10c3-281">hello instrumentpanelen visar loggar på två platser: hello sidan för hello Webbjobb och hello-sidan för ett särskilt Webbjobb-anrop.</span><span class="sxs-lookup"><span data-stu-id="d10c3-281">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Loggar Webbjobb på sidan](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Loggar i funktionen anrops-sida](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="d10c3-284">Utdata från konsolen metoderna i en funktion eller hello **Main()** metoden visas i hello instrumentpanelssidan för hello Webbjobb, inte i hello-sida för en viss metodanropet.</span><span class="sxs-lookup"><span data-stu-id="d10c3-284">Output from Console methods that you call in a function or in hello **Main()** method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="d10c3-285">Utdata från hello TextWriter-objekt som du får från en parameter i Metodsignaturen visas i hello instrumentpanelssidan för ett metodanrop.</span><span class="sxs-lookup"><span data-stu-id="d10c3-285">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="d10c3-286">Konsolens utdata kan inte länkade tooa viss metodanropet eftersom hello konsolen är enkeltrådad, medan många jobbfunktioner hello kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-286">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="d10c3-287">Det är därför hello SDK innehåller varje funktionsanrop med sin egen unika loggen skrivarobjekt.</span><span class="sxs-lookup"><span data-stu-id="d10c3-287">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="d10c3-288">toowrite [spårning programloggarna](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), använda **Console.Out** (skapar loggar som är märkta som INFO) och **Console.Error** (skapar loggar som är märkta som fel).</span><span class="sxs-lookup"><span data-stu-id="d10c3-288">toowrite [application tracing logs](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use **Console.Out** (creates logs marked as INFO) and **Console.Error** (creates logs marked as ERROR).</span></span> <span data-ttu-id="d10c3-289">Ett alternativ är toouse [spårningen eller TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), vilket möjliggör utförlig, varning, och kritiska nivåer i tillägget tooInfo och fel.</span><span class="sxs-lookup"><span data-stu-id="d10c3-289">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="d10c3-290">Spårningsloggar för programmet visas i hello web app-loggfiler, Azure-tabeller eller Azure BLOB-objekt beroende på hur du konfigurerar din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="d10c3-290">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="d10c3-291">Som gäller för alla konsolens utdata visas också hello senaste 100 programloggarna i hello instrumentpanelssidan för hello Webbjobb, inte hello sidan för ett funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="d10c3-291">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="d10c3-292">Konsolens utdata visas i hello instrumentpanelen endast om hello programmet körs i en Azure-Webbjobb inte om hello program körs lokalt eller i en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="d10c3-292">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="d10c3-293">Du kan inaktivera loggning genom att ange hello instrumentpanelen anslutning sträng toonull.</span><span class="sxs-lookup"><span data-stu-id="d10c3-293">You can disable logging by setting hello Dashboard connection string toonull.</span></span> <span data-ttu-id="d10c3-294">Mer information finns i [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="d10c3-294">For more information, see [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="d10c3-295">hello följande exempel visar flera olika sätt toowrite loggar:</span><span class="sxs-lookup"><span data-stu-id="d10c3-295">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="d10c3-296">I hello WebJobs SDK-instrumentpanelen, hello utdata från hello **TextWriter** objekt visar upp går toohello sida för en viss fungera anrop och välj **växla utdata**:</span><span class="sxs-lookup"><span data-stu-id="d10c3-296">In hello WebJobs SDK Dashboard, hello output from hello **TextWriter** object shows up when you go toohello page for a particular function invocation and select **Toggle Output**:</span></span>

![Anrops-länk](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Loggar i funktionen anrops-sida](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="d10c3-299">I hello WebJobs SDK-instrumentpanelen, hello senaste 100 rader i konsolen utdata visar dig när du gå toohello sidan för hello Webbjobb (inte för hello funktionsanrop) och väljer **växla utdata**.</span><span class="sxs-lookup"><span data-stu-id="d10c3-299">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and select **Toggle Output**.</span></span>

![Växla utdata](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

<span data-ttu-id="d10c3-301">I ett kontinuerligt Webbjobb programloggarna visas i/data/jobb/kontinuerlig/*{webjobname}*/job_log.txt i filsystemet för hello web app.</span><span class="sxs-lookup"><span data-stu-id="d10c3-301">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="d10c3-302">I ett Azure blob hello program loggar ut så här: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fel, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="d10c3-302">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="d10c3-303">Och i en Azure-tabellen hello **Console.Out** och **Console.Error** loggar ut så här:</span><span class="sxs-lookup"><span data-stu-id="d10c3-303">And in an Azure table hello **Console.Out** and **Console.Error** logs look like this:</span></span>

![Loggen för information i tabellen](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Felloggen i tabellen](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a><span data-ttu-id="d10c3-306">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d10c3-306">Next steps</span></span>
<span data-ttu-id="d10c3-307">Den här artikeln har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure köer.</span><span class="sxs-lookup"><span data-stu-id="d10c3-307">This article has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="d10c3-308">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="d10c3-308">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

