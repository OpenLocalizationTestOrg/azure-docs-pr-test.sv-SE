---
title: aaaHow toouse Azure blob storage med hello WebJobs SDK
description: "Lär dig hur toouse Azure blob storage med hello WebJobs-SDK. Starta en process när en ny blob som visas i en behållare och hantera 'skadligt blobbar'."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.assetid: bf32f919-f7bc-4aaa-916e-461c02f2e26c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: b34ea8cffee7c0475641886150dee521130a3132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="a6370-104">Hur toouse Azure blob storage med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="a6370-104">How toouse Azure blob storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="a6370-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="a6370-105">Overview</span></span>
<span data-ttu-id="a6370-106">Den här guiden innehåller C# kod exempel som visar hur tootrigger en process när ett Azure BLOB-objekt skapas eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a6370-106">This guide provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="a6370-107">hello kod exempel Använd [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="a6370-107">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="a6370-108">Kodexempel som visar hur toocreate blobbar finns [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="a6370-108">For code samples that show how toocreate blobs, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="a6370-109">hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller för[flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="a6370-109">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

## <span data-ttu-id="a6370-110"><a id="trigger"></a>Hur tootrigger en funktion när en blob skapas eller uppdateras</span><span class="sxs-lookup"><span data-stu-id="a6370-110"><a id="trigger"></a> How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="a6370-111">Det här avsnittet visas hur toouse hello `BlobTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="a6370-111">This section shows how toouse hello `BlobTrigger` attribute.</span></span> 

> [!NOTE]
> <span data-ttu-id="a6370-112">Hej WebJobs SDK genomsökningar loggen filer toowatch för nya eller ändrade BLOB.</span><span class="sxs-lookup"><span data-stu-id="a6370-112">hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="a6370-113">Den här processen är inte realtid; en funktion kan hämta aktiveras inte förrän flera minuter eller längre efter hello-blob har skapats.</span><span class="sxs-lookup"><span data-stu-id="a6370-113">This process is not real-time; a function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="a6370-114">Dessutom [lagring loggfiler skapas på ”bästa för”](https://msdn.microsoft.com/library/azure/hh343262.aspx) grund; det är inte säkert att alla händelser kommer att sparas.</span><span class="sxs-lookup"><span data-stu-id="a6370-114">In addition, [storage logs are created on a "best efforts"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; there is no guarantee that all events will be captured.</span></span> <span data-ttu-id="a6370-115">Under vissa förhållanden kan du missat loggar.</span><span class="sxs-lookup"><span data-stu-id="a6370-115">Under some conditions, logs might be missed.</span></span> <span data-ttu-id="a6370-116">Om hello hastighet och tillförlitlighet begränsningar av blob-utlösare inte är godkänd för programmet hello rekommenderas metoden är toocreate ett kömeddelande när du skapar hello blob och använder hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribut i stället för Hej `BlobTrigger` attributet på hello-funktion som behandlar hello-blob.</span><span class="sxs-lookup"><span data-stu-id="a6370-116">If hello speed and reliability limitations of blob triggers are not acceptable for your application, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello `BlobTrigger` attribute on hello function that processes hello blob.</span></span>
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="a6370-117">Enskild platshållare för blob-namn med filtillägg</span><span class="sxs-lookup"><span data-stu-id="a6370-117">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="a6370-118">hello följande kodexempel kopierar text blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare:</span><span class="sxs-lookup"><span data-stu-id="a6370-118">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="a6370-119">hello Attributkonstruktorn använder en strängparameter som anger hello behållarens namn och en platshållare för hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="a6370-119">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="a6370-120">I det här exemplet, om en blob med namnet *Blob1.txt* skapas i hello *inkommande* behållare, hello funktionen skapas en blob med namnet *Blob1.txt* i hello *utdata*  behållare.</span><span class="sxs-lookup"><span data-stu-id="a6370-120">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span> 

<span data-ttu-id="a6370-121">Du kan ange ett mönster med hello blob platshållare, som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="a6370-121">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="a6370-122">Den här koden kopieras endast blob som har namn som börjar med ”ursprungliga-”.</span><span class="sxs-lookup"><span data-stu-id="a6370-122">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="a6370-123">Till exempel *ursprungliga Blob1.txt* i hello *inkommande* behållare kopieras för*kopiera Blob1.txt* i hello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="a6370-123">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="a6370-124">Om du behöver toospecify en namnmönstret för blob-namn som innehåller klammerparenteser i hello namn dubbel hello klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="a6370-124">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="a6370-125">Om du vill toofind blobbar i hello exempelvis *bilder* behållare som har namn så här:</span><span class="sxs-lookup"><span data-stu-id="a6370-125">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="a6370-126">Använd detta för dina mönster:</span><span class="sxs-lookup"><span data-stu-id="a6370-126">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="a6370-127">I exemplet hello hello *namn* platshållare värdet *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="a6370-127">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span> 

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="a6370-128">Separat blob namn och filnamnstillägg för platshållare</span><span class="sxs-lookup"><span data-stu-id="a6370-128">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="a6370-129">hello följande exempel kodändringar hello filtillägg som kopieras blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="a6370-129">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="a6370-130">hello kod loggar hello-tillägget för hello *inkommande* blob och anger hello-tillägget för hello *utdata* blob för*.txt*.</span><span class="sxs-lookup"><span data-stu-id="a6370-130">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <span data-ttu-id="a6370-131"><a id="types"></a>Typer som du kan binda tooblobs</span><span class="sxs-lookup"><span data-stu-id="a6370-131"><a id="types"></a> Types that you can bind tooblobs</span></span>
<span data-ttu-id="a6370-132">Du kan använda hello `BlobTrigger` attributet på hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="a6370-132">You can use hello `BlobTrigger` attribute on hello following types:</span></span>

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* <span data-ttu-id="a6370-133">Andra typer som avserialiseras av [ICloudBlobStreamBinder](#icbsb)</span><span class="sxs-lookup"><span data-stu-id="a6370-133">Other types deserialized by [ICloudBlobStreamBinder](#icbsb)</span></span> 

<span data-ttu-id="a6370-134">Om du vill toowork direkt med hello Azure storage-konto, du kan också lägga till en `CloudStorageAccount` Metodsignaturen för parametern toohello.</span><span class="sxs-lookup"><span data-stu-id="a6370-134">If you want toowork directly with hello Azure storage account, you can also add a `CloudStorageAccount` parameter toohello method signature.</span></span>

<span data-ttu-id="a6370-135">Exempel finns hello [blob-bindning koden i hello azure webjobs sdk-databasen på GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="a6370-135">For examples, see hello [blob binding code in hello azure-webjobs-sdk repository on GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span></span>

## <span data-ttu-id="a6370-136"><a id="string"></a>Hämtar blob textinnehåll av bindning toostring</span><span class="sxs-lookup"><span data-stu-id="a6370-136"><a id="string"></a> Getting text blob content by binding toostring</span></span>
<span data-ttu-id="a6370-137">Om texten blobbar förväntas `BlobTrigger` kan vara tillämpade tooa `string` parameter.</span><span class="sxs-lookup"><span data-stu-id="a6370-137">If text blobs are expected, `BlobTrigger` can be applied tooa `string` parameter.</span></span> <span data-ttu-id="a6370-138">hello följande kodexempel Binder en text blob tooa `string` parameter med namnet `logMessage`.</span><span class="sxs-lookup"><span data-stu-id="a6370-138">hello following code sample binds a text blob tooa `string` parameter named `logMessage`.</span></span> <span data-ttu-id="a6370-139">hello funktionen använder de parametern toowrite hello innehållet i hello blob toohello WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a6370-139">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span> 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <span data-ttu-id="a6370-140"><a id="icbsb"></a>Hämta serialiserad blob-innehåll med hjälp av ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="a6370-140"><a id="icbsb"></a> Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="a6370-141">hello följande kodexempel används en klass som implementerar `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attributet toobind en blob-toohello `WebImage` typen.</span><span class="sxs-lookup"><span data-stu-id="a6370-141">hello following code sample uses a class that implements `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attribute toobind a blob toohello `WebImage` type.</span></span>

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

<span data-ttu-id="a6370-142">Hej `WebImage` bindning kod har angetts i en `WebImageBinder` klass som härleds från `ICloudBlobStreamBinder`.</span><span class="sxs-lookup"><span data-stu-id="a6370-142">hello `WebImage` binding code is provided in a `WebImageBinder` class that derives from `ICloudBlobStreamBinder`.</span></span>

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-hello-blob-path-for-hello-triggering-blob"></a><span data-ttu-id="a6370-143">Hämta hello blob sökväg för hello utlösa blob</span><span class="sxs-lookup"><span data-stu-id="a6370-143">Getting hello blob path for hello triggering blob</span></span>
<span data-ttu-id="a6370-144">tooget hello behållarens namn och blobbnamnet på hello blob som har utlösts hello funktionen innehåller en `blobTrigger` strängparameter i hello funktionssignatur.</span><span class="sxs-lookup"><span data-stu-id="a6370-144">tooget hello container name and blob name of hello blob that has triggered hello function, include a `blobTrigger` string parameter in hello function signature.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <span data-ttu-id="a6370-145"><a id="poison"></a>Hur toohandle poison BLOB-objekt</span><span class="sxs-lookup"><span data-stu-id="a6370-145"><a id="poison"></a> How toohandle poison blobs</span></span>
<span data-ttu-id="a6370-146">När en `BlobTrigger` funktionen misslyckas hello SDK anropar den igen, om hello misslyckandet orsakades av ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="a6370-146">When a `BlobTrigger` function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="a6370-147">Om hello felet orsakas av hello innehållet i hello blob fungerar hello funktion varje gång den försöker tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="a6370-147">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="a6370-148">Som standard anropar hello SDK en funktion in too5 gånger för en given blob.</span><span class="sxs-lookup"><span data-stu-id="a6370-148">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="a6370-149">Om hello femte försök misslyckas hello SDK lägger till en meddelandekö tooa med namnet *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="a6370-149">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="a6370-150">hello maximalt antal försök kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="a6370-150">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="a6370-151">Hej samma [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) inställningen används för hantering av skadligt blob och hantering av skadligt kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a6370-151">hello same [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span> 

<span data-ttu-id="a6370-152">hälsningsmeddelande i kö för skadligt BLOB är en JSON-objekt som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a6370-152">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="a6370-153">FunctionId (hello format *{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*, till exempel: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="a6370-153">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="a6370-154">BlobType (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="a6370-154">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="a6370-155">ContainerName</span><span class="sxs-lookup"><span data-stu-id="a6370-155">ContainerName</span></span>
* <span data-ttu-id="a6370-156">BlobName</span><span class="sxs-lookup"><span data-stu-id="a6370-156">BlobName</span></span>
* <span data-ttu-id="a6370-157">ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="a6370-157">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="a6370-158">I följande hello code exemplet hello `CopyBlob` funktionen har kod som gör att den toofail varje gång den anropas.</span><span class="sxs-lookup"><span data-stu-id="a6370-158">In hello following code sample, hello `CopyBlob` function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="a6370-159">När hello SDK anropar den för hello maximalt antal försök, skapa ett meddelande på hello skadligt blob kön och meddelandet bearbetas av hello `LogPoisonBlob` funktion.</span><span class="sxs-lookup"><span data-stu-id="a6370-159">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello `LogPoisonBlob` function.</span></span> 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

<span data-ttu-id="a6370-160">hello SDK deserializes automatiskt hello JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a6370-160">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="a6370-161">Här är hello `PoisonBlobMessage` klass:</span><span class="sxs-lookup"><span data-stu-id="a6370-161">Here is hello `PoisonBlobMessage` class:</span></span> 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <span data-ttu-id="a6370-162"><a id="polling"></a>Algoritmen för BLOB-avsökning</span><span class="sxs-lookup"><span data-stu-id="a6370-162"><a id="polling"></a> Blob polling algorithm</span></span>
<span data-ttu-id="a6370-163">Hej WebJobs SDK söker igenom alla behållare som anges av `BlobTrigger` attribut vid programstart.</span><span class="sxs-lookup"><span data-stu-id="a6370-163">hello WebJobs SDK scans all containers specified by `BlobTrigger` attributes at application start.</span></span> <span data-ttu-id="a6370-164">I ett stort lagringskonto skanningen kan ta lite tid, så det kan vara en stund innan nya blobbar finns och `BlobTrigger` funktioner utförs.</span><span class="sxs-lookup"><span data-stu-id="a6370-164">In a large storage account this scan can take some time, so it might be a while before new blobs are found and `BlobTrigger` functions are executed.</span></span>

<span data-ttu-id="a6370-165">toodetect nya eller ändrade blobar efter start av programmet hello SDK regelbundet läser från hello blob storage loggar.</span><span class="sxs-lookup"><span data-stu-id="a6370-165">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="a6370-166">hello blob är buffrade och bara hämta fysiskt skriva var 10: e minut eller så, så det kan finnas betydande fördröjning när en blob skapas eller uppdateras innan hello motsvarande `BlobTrigger` funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="a6370-166">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding `BlobTrigger` function executes.</span></span> 

<span data-ttu-id="a6370-167">Det finns ett undantag för blob som du skapar med hjälp av hello `Blob` attribut.</span><span class="sxs-lookup"><span data-stu-id="a6370-167">There is an exception for blobs that you create by using hello `Blob` attribute.</span></span> <span data-ttu-id="a6370-168">När hello WebJobs SDK skapar en ny blob, passerar hello nya blob omedelbart tooany matchar `BlobTrigger` funktioner.</span><span class="sxs-lookup"><span data-stu-id="a6370-168">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching `BlobTrigger` functions.</span></span> <span data-ttu-id="a6370-169">Därför om du har en kedja av blob-in- och utdataenheter kan hello SDK bearbeta dem effektivt.</span><span class="sxs-lookup"><span data-stu-id="a6370-169">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="a6370-170">Men om du vill låg latens och kör din blob bearbetningsfunktioner för blob som skapats eller uppdaterats på annat sätt, bör du använda `QueueTrigger` snarare än `BlobTrigger`.</span><span class="sxs-lookup"><span data-stu-id="a6370-170">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using `QueueTrigger` rather than `BlobTrigger`.</span></span>

### <span data-ttu-id="a6370-171"><a id="receipts"></a>BLOB kvitton</span><span class="sxs-lookup"><span data-stu-id="a6370-171"><a id="receipts"></a> Blob receipts</span></span>
<span data-ttu-id="a6370-172">Hej WebJobs SDK säkerställer du att inga `BlobTrigger` funktionen anropas mer än en gång för hello samma nya eller uppdaterade blob.</span><span class="sxs-lookup"><span data-stu-id="a6370-172">hello WebJobs SDK makes sure that no `BlobTrigger` function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="a6370-173">Detta åstadkoms genom att upprätthålla *blob kvitton* i ordning toodetermine om en viss blob-version har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="a6370-173">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="a6370-174">BLOB kvitton lagras i en behållare med namnet *webjobs-azure-värdar* i hello Azure storage-konto som anges av hello AzureWebJobsStorage anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="a6370-174">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="a6370-175">En blob-inleverans har hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a6370-175">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="a6370-176">Hej funktion som anropades för hello blob (”*{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*”, till exempel:” WebJob1.Functions.CopyBlob ”)</span><span class="sxs-lookup"><span data-stu-id="a6370-176">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="a6370-177">hello-behållarnamn</span><span class="sxs-lookup"><span data-stu-id="a6370-177">hello container name</span></span>
* <span data-ttu-id="a6370-178">hello blob-datatyp (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="a6370-178">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="a6370-179">Hej blobbnamnet</span><span class="sxs-lookup"><span data-stu-id="a6370-179">hello blob name</span></span>
* <span data-ttu-id="a6370-180">hello ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="a6370-180">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="a6370-181">Om du vill tooforce ombearbetning av en blobb måste du manuellt ta bort hello blob inleverans för blobben från hello *webjobs-azure-värdar* behållare.</span><span class="sxs-lookup"><span data-stu-id="a6370-181">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <span data-ttu-id="a6370-182"><a id="queues"></a>Närliggande ämnen som omfattas av hello köer artikel</span><span class="sxs-lookup"><span data-stu-id="a6370-182"><a id="queues"></a>Related topics covered by hello queues article</span></span>
<span data-ttu-id="a6370-183">Information om hur toohandle blob bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tooblob bearbetning, se [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="a6370-183">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="a6370-184">Närliggande ämnen som beskrivs i artikeln inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="a6370-184">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="a6370-185">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="a6370-185">Async functions</span></span>
* <span data-ttu-id="a6370-186">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="a6370-186">Multiple instances</span></span>
* <span data-ttu-id="a6370-187">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="a6370-187">Graceful shutdown</span></span>
* <span data-ttu-id="a6370-188">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="a6370-188">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="a6370-189">Ange hello SDK-anslutningssträngar i koden.</span><span class="sxs-lookup"><span data-stu-id="a6370-189">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="a6370-190">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="a6370-190">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="a6370-191">Konfigurera `MaxDequeueCount` för hantering av skadligt blob.</span><span class="sxs-lookup"><span data-stu-id="a6370-191">Configure `MaxDequeueCount` for poison blob handling.</span></span>
* <span data-ttu-id="a6370-192">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="a6370-192">Trigger a function manually</span></span>
* <span data-ttu-id="a6370-193">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="a6370-193">Write logs</span></span>

## <span data-ttu-id="a6370-194"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6370-194"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="a6370-195">Den här guiden tillhandahåller kodexempel som visar hur toohandle vanliga scenarier för att arbeta med Azure BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="a6370-195">This guide has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="a6370-196">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="a6370-196">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

