---
title: "aaaGet igång med blob storage och Visual Studio anslutna tjänster (Webbjobb projekt) | Microsoft Docs"
description: "Hur tooget igång med Blob storage i ett Webbjobb projekt efter anslutning tooan Azure storage med hjälp av Visual Studio anslutna tjänster."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: cb2cecacbb1a59f093d5453a91fae33e0f279d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="dddd2-103">Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (Webbjobb projekt)</span><span class="sxs-lookup"><span data-stu-id="dddd2-103">Get started with Azure Blob storage and Visual Studio connected services (WebJob projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="dddd2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dddd2-104">Overview</span></span>
<span data-ttu-id="dddd2-105">Den här artikeln innehåller C# kod exempel som visar hur tootrigger en process när ett Azure BLOB-objekt skapas eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="dddd2-105">This article provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="dddd2-106">hello kodexempel använda hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="dddd2-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span> <span data-ttu-id="dddd2-107">När du lägger till ett storage-konto tooa Webbjobb projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan hello lämplig Azure Storage NuGet-paketet har installerats, hello lämpliga .NET-referenserna är tillagda toohello projektet och anslutningssträngar för hello storage-konto uppdateras i hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="dddd2-107">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet package is installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>

## <a name="how-tootrigger-a-function-when-a-blob-is-created-or-updated"></a><span data-ttu-id="dddd2-108">Hur tootrigger en funktion när en blob skapas eller uppdateras</span><span class="sxs-lookup"><span data-stu-id="dddd2-108">How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="dddd2-109">Det här avsnittet visas hur toouse hello **BlobTrigger** attribut.</span><span class="sxs-lookup"><span data-stu-id="dddd2-109">This section shows how toouse hello **BlobTrigger** attribute.</span></span>

 <span data-ttu-id="dddd2-110">**Obs:** hello WebJobs SDK genomsökningar loggen filer toowatch för nya eller ändrade BLOB.</span><span class="sxs-lookup"><span data-stu-id="dddd2-110">**Note:** hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="dddd2-111">Den här processen är långsam natur; en funktion kan hämta aktiveras inte förrän flera minuter eller längre efter hello-blob har skapats.</span><span class="sxs-lookup"><span data-stu-id="dddd2-111">This process is inherently slow; a function might not get triggered until several minutes or longer after hello blob is created.</span></span>  <span data-ttu-id="dddd2-112">Om ditt program måste omedelbart tooprocess blobbar, hello rekommenderade metoden är toocreate ett kömeddelande när du skapar hello blob och använder hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribut i stället för hello **BlobTrigger** -attributet på hello-funktion som behandlar hello-blob.</span><span class="sxs-lookup"><span data-stu-id="dddd2-112">If your application needs tooprocess blobs immediately, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello **BlobTrigger** attribute on hello function that processes hello blob.</span></span>

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="dddd2-113">Enskild platshållare för blob-namn med filtillägg</span><span class="sxs-lookup"><span data-stu-id="dddd2-113">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="dddd2-114">hello följande kodexempel kopierar text blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare:</span><span class="sxs-lookup"><span data-stu-id="dddd2-114">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="dddd2-115">hello Attributkonstruktorn använder en strängparameter som anger hello behållarens namn och en platshållare för hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="dddd2-115">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="dddd2-116">I det här exemplet, om en blob med namnet *Blob1.txt* skapas i hello *inkommande* behållare, hello funktionen skapas en blob med namnet *Blob1.txt* i hello *utdata*  behållare.</span><span class="sxs-lookup"><span data-stu-id="dddd2-116">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="dddd2-117">Du kan ange ett mönster med hello blob platshållare, som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="dddd2-117">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="dddd2-118">Den här koden kopieras endast blob som har namn som börjar med ”ursprungliga-”.</span><span class="sxs-lookup"><span data-stu-id="dddd2-118">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="dddd2-119">Till exempel *ursprungliga Blob1.txt* i hello *inkommande* behållare kopieras för*kopiera Blob1.txt* i hello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="dddd2-119">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="dddd2-120">Om du behöver toospecify en namnmönstret för blob-namn som innehåller klammerparenteser i hello namn dubbel hello klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="dddd2-120">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="dddd2-121">Om du vill toofind blobbar i hello exempelvis *bilder* behållare som har namn så här:</span><span class="sxs-lookup"><span data-stu-id="dddd2-121">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="dddd2-122">Använd detta för dina mönster:</span><span class="sxs-lookup"><span data-stu-id="dddd2-122">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="dddd2-123">I exemplet hello hello *namn* platshållare värdet *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="dddd2-123">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span>

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="dddd2-124">Separat blob namn och filnamnstillägg för platshållare</span><span class="sxs-lookup"><span data-stu-id="dddd2-124">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="dddd2-125">hello följande exempel kodändringar hello filtillägg som kopieras blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="dddd2-125">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="dddd2-126">hello kod loggar hello-tillägget för hello *inkommande* blob och anger hello-tillägget för hello *utdata* blob för*.txt*.</span><span class="sxs-lookup"><span data-stu-id="dddd2-126">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

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

## <a name="types-that-you-can-bind-tooblobs"></a><span data-ttu-id="dddd2-127">Typer som du kan binda tooblobs</span><span class="sxs-lookup"><span data-stu-id="dddd2-127">Types that you can bind tooblobs</span></span>
<span data-ttu-id="dddd2-128">Du kan använda hello **BlobTrigger** -attributet på hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="dddd2-128">You can use hello **BlobTrigger** attribute on hello following types:</span></span>

* <span data-ttu-id="dddd2-129">**sträng**</span><span class="sxs-lookup"><span data-stu-id="dddd2-129">**string**</span></span>
* <span data-ttu-id="dddd2-130">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="dddd2-130">**TextReader**</span></span>
* <span data-ttu-id="dddd2-131">**Dataströmmen**</span><span class="sxs-lookup"><span data-stu-id="dddd2-131">**Stream**</span></span>
* <span data-ttu-id="dddd2-132">**ICloudBlob**</span><span class="sxs-lookup"><span data-stu-id="dddd2-132">**ICloudBlob**</span></span>
* <span data-ttu-id="dddd2-133">**CloudBlockBlob**</span><span class="sxs-lookup"><span data-stu-id="dddd2-133">**CloudBlockBlob**</span></span>
* <span data-ttu-id="dddd2-134">**CloudPageBlob**</span><span class="sxs-lookup"><span data-stu-id="dddd2-134">**CloudPageBlob**</span></span>
* <span data-ttu-id="dddd2-135">Andra typer som avserialiseras av [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span><span class="sxs-lookup"><span data-stu-id="dddd2-135">Other types deserialized by [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span></span>

<span data-ttu-id="dddd2-136">Om du vill toowork direkt med hello Azure storage-konto, du kan också lägga till en **CloudStorageAccount** Metodsignaturen för parametern toohello.</span><span class="sxs-lookup"><span data-stu-id="dddd2-136">If you want toowork directly with hello Azure storage account, you can also add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="getting-text-blob-content-by-binding-toostring"></a><span data-ttu-id="dddd2-137">Hämtar blob textinnehåll av bindning toostring</span><span class="sxs-lookup"><span data-stu-id="dddd2-137">Getting text blob content by binding toostring</span></span>
<span data-ttu-id="dddd2-138">Om texten blobbar förväntas **BlobTrigger** kan vara tillämpade tooa **sträng** parameter.</span><span class="sxs-lookup"><span data-stu-id="dddd2-138">If text blobs are expected, **BlobTrigger** can be applied tooa **string** parameter.</span></span> <span data-ttu-id="dddd2-139">hello följande kodexempel Binder en text blob tooa **sträng** parameter med namnet **logMessage**.</span><span class="sxs-lookup"><span data-stu-id="dddd2-139">hello following code sample binds a text blob tooa **string** parameter named **logMessage**.</span></span> <span data-ttu-id="dddd2-140">hello funktionen använder de parametern toowrite hello innehållet i hello blob toohello WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="dddd2-140">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><span data-ttu-id="dddd2-141">Hämta serialiserad blob-innehåll med hjälp av ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="dddd2-141">Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="dddd2-142">hello följande kodexempel används en klass som implementerar **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attributet toobind en blob-toohello **WebImage** typen.</span><span class="sxs-lookup"><span data-stu-id="dddd2-142">hello following code sample uses a class that implements **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attribute toobind a blob toohello **WebImage** type.</span></span>

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

<span data-ttu-id="dddd2-143">Hej **WebImage** bindning kod har angetts i en **WebImageBinder** klass som härleds från **ICloudBlobStreamBinder**.</span><span class="sxs-lookup"><span data-stu-id="dddd2-143">hello **WebImage** binding code is provided in a **WebImageBinder** class that derives from **ICloudBlobStreamBinder**.</span></span>

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

## <a name="how-toohandle-poison-blobs"></a><span data-ttu-id="dddd2-144">Hur toohandle poison BLOB-objekt</span><span class="sxs-lookup"><span data-stu-id="dddd2-144">How toohandle poison blobs</span></span>
<span data-ttu-id="dddd2-145">När en **BlobTrigger** funktionen misslyckas hello SDK anropar den igen, om hello misslyckandet orsakades av ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="dddd2-145">When a **BlobTrigger** function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="dddd2-146">Om hello felet orsakas av hello innehållet i hello blob fungerar hello funktion varje gång den försöker tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="dddd2-146">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="dddd2-147">Som standard anropar hello SDK en funktion in too5 gånger för en given blob.</span><span class="sxs-lookup"><span data-stu-id="dddd2-147">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="dddd2-148">Om hello femte försök misslyckas hello SDK lägger till en meddelandekö tooa med namnet *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="dddd2-148">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="dddd2-149">hello maximalt antal försök kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="dddd2-149">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="dddd2-150">Hej samma [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) inställningen används för hantering av skadligt blob och hantering av skadligt kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="dddd2-150">hello same [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span>

<span data-ttu-id="dddd2-151">hälsningsmeddelande i kö för skadligt BLOB är en JSON-objekt som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="dddd2-151">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="dddd2-152">FunctionId (hello format *{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*, till exempel: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="dddd2-152">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="dddd2-153">BlobType (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="dddd2-153">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="dddd2-154">ContainerName</span><span class="sxs-lookup"><span data-stu-id="dddd2-154">ContainerName</span></span>
* <span data-ttu-id="dddd2-155">BlobName</span><span class="sxs-lookup"><span data-stu-id="dddd2-155">BlobName</span></span>
* <span data-ttu-id="dddd2-156">ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="dddd2-156">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="dddd2-157">I följande hello code exemplet hello **CopyBlob** funktionen har kod som gör att den toofail varje gång den anropas.</span><span class="sxs-lookup"><span data-stu-id="dddd2-157">In hello following code sample, hello **CopyBlob** function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="dddd2-158">När hello SDK anropar den för hello maximalt antal försök, skapa ett meddelande på hello skadligt blob kön och meddelandet bearbetas av hello **LogPoisonBlob** funktion.</span><span class="sxs-lookup"><span data-stu-id="dddd2-158">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello **LogPoisonBlob** function.</span></span>

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

<span data-ttu-id="dddd2-159">hello SDK deserializes automatiskt hello JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="dddd2-159">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="dddd2-160">Här är hello **PoisonBlobMessage** klass:</span><span class="sxs-lookup"><span data-stu-id="dddd2-160">Here is hello **PoisonBlobMessage** class:</span></span>

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a><span data-ttu-id="dddd2-161">Algoritmen för BLOB-avsökning</span><span class="sxs-lookup"><span data-stu-id="dddd2-161">Blob polling algorithm</span></span>
<span data-ttu-id="dddd2-162">Hej WebJobs SDK söker igenom alla behållare som anges av **BlobTrigger** attribut vid programstart.</span><span class="sxs-lookup"><span data-stu-id="dddd2-162">hello WebJobs SDK scans all containers specified by **BlobTrigger** attributes at application start.</span></span> <span data-ttu-id="dddd2-163">I ett stort lagringskonto skanningen kan ta lite tid, så det kan vara en stund innan nya blobbar finns och **BlobTrigger** funktioner utförs.</span><span class="sxs-lookup"><span data-stu-id="dddd2-163">In a large storage account this scan can take some time, so it might be a while before new blobs are found and **BlobTrigger** functions are executed.</span></span>

<span data-ttu-id="dddd2-164">toodetect nya eller ändrade blobar efter start av programmet hello SDK regelbundet läser från hello blob storage loggar.</span><span class="sxs-lookup"><span data-stu-id="dddd2-164">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="dddd2-165">hello blob är buffrade och bara hämta fysiskt skriva var 10: e minut eller så, så det kan finnas betydande fördröjning när en blob skapas eller uppdateras innan hello motsvarande **BlobTrigger** funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="dddd2-165">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding **BlobTrigger** function executes.</span></span>

<span data-ttu-id="dddd2-166">Det finns ett undantag för blob som du skapar med hjälp av hello **Blob** attribut.</span><span class="sxs-lookup"><span data-stu-id="dddd2-166">There is an exception for blobs that you create by using hello **Blob** attribute.</span></span> <span data-ttu-id="dddd2-167">När hello WebJobs SDK skapar en ny blob, passerar hello nya blob omedelbart tooany matchar **BlobTrigger** funktioner.</span><span class="sxs-lookup"><span data-stu-id="dddd2-167">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching **BlobTrigger** functions.</span></span> <span data-ttu-id="dddd2-168">Därför om du har en kedja av blob-in- och utdataenheter kan hello SDK bearbeta dem effektivt.</span><span class="sxs-lookup"><span data-stu-id="dddd2-168">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="dddd2-169">Men om du vill låg latens och kör din blob bearbetningsfunktioner för blob som skapats eller uppdaterats på annat sätt, bör du använda **QueueTrigger** snarare än **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="dddd2-169">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using **QueueTrigger** rather than **BlobTrigger**.</span></span>

### <a name="blob-receipts"></a><span data-ttu-id="dddd2-170">BLOB kvitton</span><span class="sxs-lookup"><span data-stu-id="dddd2-170">Blob receipts</span></span>
<span data-ttu-id="dddd2-171">Hej WebJobs SDK säkerställer du att inga **BlobTrigger** funktionen anropas mer än en gång för hello samma nya eller uppdaterade blob.</span><span class="sxs-lookup"><span data-stu-id="dddd2-171">hello WebJobs SDK makes sure that no **BlobTrigger** function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="dddd2-172">Detta åstadkoms genom att upprätthålla *blob kvitton* i ordning toodetermine om en viss blob-version har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="dddd2-172">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="dddd2-173">BLOB kvitton lagras i en behållare med namnet *webjobs-azure-värdar* i hello Azure storage-konto som anges av hello AzureWebJobsStorage anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="dddd2-173">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="dddd2-174">En blob-inleverans har hello följande information:</span><span class="sxs-lookup"><span data-stu-id="dddd2-174">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="dddd2-175">Hej funktion som anropades för hello blob (”*{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*”, till exempel:” WebJob1.Functions.CopyBlob ”)</span><span class="sxs-lookup"><span data-stu-id="dddd2-175">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="dddd2-176">hello-behållarnamn</span><span class="sxs-lookup"><span data-stu-id="dddd2-176">hello container name</span></span>
* <span data-ttu-id="dddd2-177">hello blob-datatyp (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="dddd2-177">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="dddd2-178">Hej blobbnamnet</span><span class="sxs-lookup"><span data-stu-id="dddd2-178">hello blob name</span></span>
* <span data-ttu-id="dddd2-179">hello ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="dddd2-179">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="dddd2-180">Om du vill tooforce ombearbetning av en blobb måste du manuellt ta bort hello blob inleverans för blobben från hello *webjobs-azure-värdar* behållare.</span><span class="sxs-lookup"><span data-stu-id="dddd2-180">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <a name="related-topics-covered-by-hello-queues-article"></a><span data-ttu-id="dddd2-181">Närliggande ämnen som omfattas av hello köer artikel</span><span class="sxs-lookup"><span data-stu-id="dddd2-181">Related topics covered by hello queues article</span></span>
<span data-ttu-id="dddd2-182">Information om hur toohandle blob bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tooblob bearbetning, se [hur toouse Azure kö lagring med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="dddd2-182">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span>

<span data-ttu-id="dddd2-183">Närliggande ämnen som beskrivs i artikeln inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="dddd2-183">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="dddd2-184">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="dddd2-184">Async functions</span></span>
* <span data-ttu-id="dddd2-185">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="dddd2-185">Multiple instances</span></span>
* <span data-ttu-id="dddd2-186">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="dddd2-186">Graceful shutdown</span></span>
* <span data-ttu-id="dddd2-187">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="dddd2-187">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="dddd2-188">Ange hello SDK-anslutningssträngar i koden.</span><span class="sxs-lookup"><span data-stu-id="dddd2-188">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="dddd2-189">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="dddd2-189">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="dddd2-190">Konfigurera **MaxDequeueCount** för hantering av skadligt blob.</span><span class="sxs-lookup"><span data-stu-id="dddd2-190">Configure **MaxDequeueCount** for poison blob handling.</span></span>
* <span data-ttu-id="dddd2-191">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="dddd2-191">Trigger a function manually</span></span>
* <span data-ttu-id="dddd2-192">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="dddd2-192">Write logs</span></span>

## <a name="next-steps"></a><span data-ttu-id="dddd2-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dddd2-193">Next steps</span></span>
<span data-ttu-id="dddd2-194">Den här artikeln har lämnat kodexempel som visar hur toohandle vanliga scenarier för att arbeta med Azure BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="dddd2-194">This article has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="dddd2-195">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="dddd2-195">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

