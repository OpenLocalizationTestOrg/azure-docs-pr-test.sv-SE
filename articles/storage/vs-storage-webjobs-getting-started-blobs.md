---
title: "Kom igång med blob storage och Visual Studio anslutna tjänster (Webbjobb projekt) | Microsoft Docs"
description: "Hur du kommer igång med Blob storage i ett Webbjobb projekt efter anslutning till en Azure storage med hjälp av Visual Studio anslutna tjänster."
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
ms.openlocfilehash: 7fec890590302a99bbc96f46f24ae440c041580c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="985e9-103">Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (Webbjobb projekt)</span><span class="sxs-lookup"><span data-stu-id="985e9-103">Get started with Azure Blob storage and Visual Studio connected services (WebJob projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="985e9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="985e9-104">Overview</span></span>
<span data-ttu-id="985e9-105">Den här artikeln innehåller C#-kodexempel som visar hur du utlöser en process när ett Azure BLOB-objekt skapas eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="985e9-105">This article provides C# code samples that show how to trigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="985e9-106">Koden exempel användning av [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="985e9-106">The code samples use the [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span> <span data-ttu-id="985e9-107">När du lägger till ett lagringskonto till ett Webbjobb-projekt med hjälp av Visual Studio **Lägg till anslutna tjänster** dialogrutan lämplig Azure Storage NuGet-paketet har installerats, lämpliga .NET referenserna har lagts till i projektet, och anslutningssträngar för storage-konto har uppdaterats i filen App.config.</span><span class="sxs-lookup"><span data-stu-id="985e9-107">When you add a storage account to a WebJob project by using the Visual Studio **Add Connected Services** dialog, the appropriate Azure Storage NuGet package is installed, the appropriate .NET references are added to the project, and connection strings for the storage account are updated in the App.config file.</span></span>

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a><span data-ttu-id="985e9-108">Hur du utlöser en funktion när en blob skapas eller uppdateras</span><span class="sxs-lookup"><span data-stu-id="985e9-108">How to trigger a function when a blob is created or updated</span></span>
<span data-ttu-id="985e9-109">Det här avsnittet visar hur du använder den **BlobTrigger** attribut.</span><span class="sxs-lookup"><span data-stu-id="985e9-109">This section shows how to use the **BlobTrigger** attribute.</span></span>

 <span data-ttu-id="985e9-110">**Obs:** WebJobs SDK igenom loggfiler kan du titta på för nya eller ändrade BLOB.</span><span class="sxs-lookup"><span data-stu-id="985e9-110">**Note:** The WebJobs SDK scans log files to watch for new or changed blobs.</span></span> <span data-ttu-id="985e9-111">Den här processen är långsam natur; en funktion kan hämta aktiveras inte förrän flera minuter eller längre efter blob skapas.</span><span class="sxs-lookup"><span data-stu-id="985e9-111">This process is inherently slow; a function might not get triggered until several minutes or longer after the blob is created.</span></span>  <span data-ttu-id="985e9-112">Om ditt program måste bearbeta blobbar omedelbart, den rekommenderade metoden är att skapa ett kömeddelande när du skapar blob och använder den [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribut i stället för den **BlobTrigger** attributet för den funktion som behandlar blob.</span><span class="sxs-lookup"><span data-stu-id="985e9-112">If your application needs to process blobs immediately, the recommended method is to create a queue message when you create the blob, and use the [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of the **BlobTrigger** attribute on the function that processes the blob.</span></span>

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="985e9-113">Enskild platshållare för blob-namn med filtillägg</span><span class="sxs-lookup"><span data-stu-id="985e9-113">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="985e9-114">Följande kodexempel kopierar text blobbar som visas i den *inkommande* behållare för att den *utdata* behållare:</span><span class="sxs-lookup"><span data-stu-id="985e9-114">The following code sample copies text blobs that appear in the *input* container to the *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="985e9-115">Attributkonstruktorn använder en strängparameter som anger behållarens namn och en platshållare för blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="985e9-115">The attribute constructor takes a string parameter that specifies the container name and a placeholder for the blob name.</span></span> <span data-ttu-id="985e9-116">I det här exemplet, om en blob med namnet *Blob1.txt* skapas i den *inkommande* -behållaren i funktionen skapas en blob med namnet *Blob1.txt* i den *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="985e9-116">In this example, if a blob named *Blob1.txt* is created in the *input* container, the function creates a blob named *Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="985e9-117">Du kan ange ett mönster med platshållaren för blob-namnet som visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="985e9-117">You can specify a name pattern with the blob name placeholder, as shown in the following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="985e9-118">Den här koden kopieras endast blob som har namn som börjar med ”ursprungliga-”.</span><span class="sxs-lookup"><span data-stu-id="985e9-118">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="985e9-119">Till exempel *ursprungliga Blob1.txt* i den *inkommande* behållare kopieras till *kopiera Blob1.txt* i den *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="985e9-119">For example, *original-Blob1.txt* in the *input* container is copied to *copy-Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="985e9-120">Om du behöver ange ett på namnmönster för blob-namn som innehåller klammerparenteser i namnet dubbla klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="985e9-120">If you need to specify a name pattern for blob names that have curly braces in the name, double the curly braces.</span></span> <span data-ttu-id="985e9-121">Om du vill söka efter blobbar i till exempel den *bilder* behållare som har namn så här:</span><span class="sxs-lookup"><span data-stu-id="985e9-121">For example, if you want to find blobs in the *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="985e9-122">Använd detta för dina mönster:</span><span class="sxs-lookup"><span data-stu-id="985e9-122">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="985e9-123">I det här exemplet kan den *namn* platshållare värdet *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="985e9-123">In the example, the *name* placeholder value would be *soundfile.mp3*.</span></span>

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="985e9-124">Separat blob namn och filnamnstillägg för platshållare</span><span class="sxs-lookup"><span data-stu-id="985e9-124">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="985e9-125">Följande kodexempel ändras filnamnstillägget när den kopierar blobar som visas i den *inkommande* behållare för att den *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="985e9-125">The following code sample changes the file extension as it copies blobs that appear in the *input* container to the *output* container.</span></span> <span data-ttu-id="985e9-126">Koden loggar tillägget för den *inkommande* blob och anger filnamnstillägget för den *utdata* blob till *.txt*.</span><span class="sxs-lookup"><span data-stu-id="985e9-126">The code logs the extension of the *input* blob and sets the extension of the *output* blob to *.txt*.</span></span>

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

## <a name="types-that-you-can-bind-to-blobs"></a><span data-ttu-id="985e9-127">Typer som du kan binda till blobbar</span><span class="sxs-lookup"><span data-stu-id="985e9-127">Types that you can bind to blobs</span></span>
<span data-ttu-id="985e9-128">Du kan använda den **BlobTrigger** attribut för följande typer:</span><span class="sxs-lookup"><span data-stu-id="985e9-128">You can use the **BlobTrigger** attribute on the following types:</span></span>

* <span data-ttu-id="985e9-129">**sträng**</span><span class="sxs-lookup"><span data-stu-id="985e9-129">**string**</span></span>
* <span data-ttu-id="985e9-130">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="985e9-130">**TextReader**</span></span>
* <span data-ttu-id="985e9-131">**Dataströmmen**</span><span class="sxs-lookup"><span data-stu-id="985e9-131">**Stream**</span></span>
* <span data-ttu-id="985e9-132">**ICloudBlob**</span><span class="sxs-lookup"><span data-stu-id="985e9-132">**ICloudBlob**</span></span>
* <span data-ttu-id="985e9-133">**CloudBlockBlob**</span><span class="sxs-lookup"><span data-stu-id="985e9-133">**CloudBlockBlob**</span></span>
* <span data-ttu-id="985e9-134">**CloudPageBlob**</span><span class="sxs-lookup"><span data-stu-id="985e9-134">**CloudPageBlob**</span></span>
* <span data-ttu-id="985e9-135">Andra typer som avserialiseras av [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span><span class="sxs-lookup"><span data-stu-id="985e9-135">Other types deserialized by [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span></span>

<span data-ttu-id="985e9-136">Om du vill arbeta direkt med Azure storage-konto, du kan också lägga till en **CloudStorageAccount** parameter till Metodsignaturen.</span><span class="sxs-lookup"><span data-stu-id="985e9-136">If you want to work directly with the Azure storage account, you can also add a **CloudStorageAccount** parameter to the method signature.</span></span>

## <a name="getting-text-blob-content-by-binding-to-string"></a><span data-ttu-id="985e9-137">Hämtar blob textinnehåll av bindning till en sträng</span><span class="sxs-lookup"><span data-stu-id="985e9-137">Getting text blob content by binding to string</span></span>
<span data-ttu-id="985e9-138">Om texten blobbar förväntas **BlobTrigger** kan tillämpas på en **sträng** parameter.</span><span class="sxs-lookup"><span data-stu-id="985e9-138">If text blobs are expected, **BlobTrigger** can be applied to a **string** parameter.</span></span> <span data-ttu-id="985e9-139">Följande kodexempel Binder en text blobb till en **sträng** parameter med namnet **logMessage**.</span><span class="sxs-lookup"><span data-stu-id="985e9-139">The following code sample binds a text blob to a **string** parameter named **logMessage**.</span></span> <span data-ttu-id="985e9-140">Parametern använder funktionen för att skriva innehållet i blob till WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="985e9-140">The function uses that parameter to write the contents of the blob to the WebJobs SDK dashboard.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><span data-ttu-id="985e9-141">Hämta serialiserad blob-innehåll med hjälp av ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="985e9-141">Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="985e9-142">Följande kodexempel används en klass som implementerar **ICloudBlobStreamBinder** att aktivera den **BlobTrigger** attribut för att binda en blobb till den **WebImage** typen.</span><span class="sxs-lookup"><span data-stu-id="985e9-142">The following code sample uses a class that implements **ICloudBlobStreamBinder** to enable the **BlobTrigger** attribute to bind a blob to the **WebImage** type.</span></span>

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

<span data-ttu-id="985e9-143">Den **WebImage** bindning kod har angetts i en **WebImageBinder** klass som härleds från **ICloudBlobStreamBinder**.</span><span class="sxs-lookup"><span data-stu-id="985e9-143">The **WebImage** binding code is provided in a **WebImageBinder** class that derives from **ICloudBlobStreamBinder**.</span></span>

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

## <a name="how-to-handle-poison-blobs"></a><span data-ttu-id="985e9-144">Hur du hanterar skadligt blobbar</span><span class="sxs-lookup"><span data-stu-id="985e9-144">How to handle poison blobs</span></span>
<span data-ttu-id="985e9-145">När en **BlobTrigger** funktionen misslyckas SDK anropar den igen, om felet orsakades av ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="985e9-145">When a **BlobTrigger** function fails, the SDK calls it again, in case the failure was caused by a transient error.</span></span> <span data-ttu-id="985e9-146">Om felet orsakas av innehållet för blob, misslyckas åtgärden varje gång den försöker bearbeta blob.</span><span class="sxs-lookup"><span data-stu-id="985e9-146">If the failure is caused by the content of the blob, the function fails every time it tries to process the blob.</span></span> <span data-ttu-id="985e9-147">Som standard anropar SDK en funktion upp till 5 gånger för en given blob.</span><span class="sxs-lookup"><span data-stu-id="985e9-147">By default, the SDK calls a function up to 5 times for a given blob.</span></span> <span data-ttu-id="985e9-148">Om femte misslyckas, SDK lägger till ett meddelande till en kö med namnet *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="985e9-148">If the fifth try fails, the SDK adds a message to a queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="985e9-149">Det maximala antalet nya försök kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="985e9-149">The maximum number of retries is configurable.</span></span> <span data-ttu-id="985e9-150">Samma [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) inställningen används för hantering av skadligt blob och hantering av skadligt kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="985e9-150">The same [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span>

<span data-ttu-id="985e9-151">Kömeddelande för skadligt BLOB är en JSON-objekt som innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="985e9-151">The queue message for poison blobs is a JSON object that contains the following properties:</span></span>

* <span data-ttu-id="985e9-152">FunctionId (i formatet *{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*, till exempel: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="985e9-152">FunctionId (in the format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="985e9-153">BlobType (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="985e9-153">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="985e9-154">ContainerName</span><span class="sxs-lookup"><span data-stu-id="985e9-154">ContainerName</span></span>
* <span data-ttu-id="985e9-155">BlobName</span><span class="sxs-lookup"><span data-stu-id="985e9-155">BlobName</span></span>
* <span data-ttu-id="985e9-156">ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="985e9-156">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="985e9-157">I följande kodexempel i **CopyBlob** funktionen har kod som gör att den misslyckas varje gång den anropas.</span><span class="sxs-lookup"><span data-stu-id="985e9-157">In the following code sample, the **CopyBlob** function has code that causes it to fail every time it's called.</span></span> <span data-ttu-id="985e9-158">När SDK-anrop den för det maximala antalet nya försök kan skapa ett meddelande i kön skadligt blob och meddelandet bearbetas av den **LogPoisonBlob** funktion.</span><span class="sxs-lookup"><span data-stu-id="985e9-158">After the SDK calls it for the maximum number of retries, a message is created on the poison blob queue, and that message is processed by the **LogPoisonBlob** function.</span></span>

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

<span data-ttu-id="985e9-159">SDK deserializes automatiskt JSON-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="985e9-159">The SDK automatically deserializes the JSON message.</span></span> <span data-ttu-id="985e9-160">Här är den **PoisonBlobMessage** klass:</span><span class="sxs-lookup"><span data-stu-id="985e9-160">Here is the **PoisonBlobMessage** class:</span></span>

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a><span data-ttu-id="985e9-161">Algoritmen för BLOB-avsökning</span><span class="sxs-lookup"><span data-stu-id="985e9-161">Blob polling algorithm</span></span>
<span data-ttu-id="985e9-162">WebJobs SDK söker igenom alla behållare som anges av **BlobTrigger** attribut vid programstart.</span><span class="sxs-lookup"><span data-stu-id="985e9-162">The WebJobs SDK scans all containers specified by **BlobTrigger** attributes at application start.</span></span> <span data-ttu-id="985e9-163">I ett stort lagringskonto skanningen kan ta lite tid, så det kan vara en stund innan nya blobbar finns och **BlobTrigger** funktioner utförs.</span><span class="sxs-lookup"><span data-stu-id="985e9-163">In a large storage account this scan can take some time, so it might be a while before new blobs are found and **BlobTrigger** functions are executed.</span></span>

<span data-ttu-id="985e9-164">För att identifiera nya eller ändrade blobbar när programmet startas läser SDK regelbundet från blob storage-loggar.</span><span class="sxs-lookup"><span data-stu-id="985e9-164">To detect new or changed blobs after application start, the SDK periodically reads from the blob storage logs.</span></span> <span data-ttu-id="985e9-165">Blob-loggarna är buffrade och bara hämta fysiskt skriva var 10: e minut eller så, så det kan finnas betydande fördröjning när en blob skapas eller uppdateras innan den motsvarande **BlobTrigger** funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="985e9-165">The blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before the corresponding **BlobTrigger** function executes.</span></span>

<span data-ttu-id="985e9-166">Det finns ett undantag för blob som du skapar med hjälp av den **Blob** attribut.</span><span class="sxs-lookup"><span data-stu-id="985e9-166">There is an exception for blobs that you create by using the **Blob** attribute.</span></span> <span data-ttu-id="985e9-167">När WebJobs SDK skapar en ny blob, passerar de nya blob omedelbart för alla matchande **BlobTrigger** funktioner.</span><span class="sxs-lookup"><span data-stu-id="985e9-167">When the WebJobs SDK creates a new blob, it passes the new blob immediately to any matching **BlobTrigger** functions.</span></span> <span data-ttu-id="985e9-168">Därför om du har en kedja av blob-in- och utdataenheter kan SDK bearbeta dem effektivt.</span><span class="sxs-lookup"><span data-stu-id="985e9-168">Therefore if you have a chain of blob inputs and outputs, the SDK can process them efficiently.</span></span> <span data-ttu-id="985e9-169">Men om du vill låg latens och kör din blob bearbetningsfunktioner för blob som skapats eller uppdaterats på annat sätt, bör du använda **QueueTrigger** snarare än **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="985e9-169">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using **QueueTrigger** rather than **BlobTrigger**.</span></span>

### <a name="blob-receipts"></a><span data-ttu-id="985e9-170">BLOB kvitton</span><span class="sxs-lookup"><span data-stu-id="985e9-170">Blob receipts</span></span>
<span data-ttu-id="985e9-171">WebJobs SDK säkerställer du att inga **BlobTrigger** funktionen anropas flera gånger för samma nya eller uppdaterade blob.</span><span class="sxs-lookup"><span data-stu-id="985e9-171">The WebJobs SDK makes sure that no **BlobTrigger** function gets called more than once for the same new or updated blob.</span></span> <span data-ttu-id="985e9-172">Detta åstadkoms genom att upprätthålla *blob kvitton* för att avgöra om en viss blob-version har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="985e9-172">It does this by maintaining *blob receipts* in order to determine if a given blob version has been processed.</span></span>

<span data-ttu-id="985e9-173">BLOB kvitton lagras i en behållare med namnet *webjobs-azure-värdar* i Azure storage-konto som angetts i anslutningssträngen för AzureWebJobsStorage.</span><span class="sxs-lookup"><span data-stu-id="985e9-173">Blob receipts are stored in a container named *azure-webjobs-hosts* in the Azure storage account specified by the AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="985e9-174">En blob-inleverans har följande information:</span><span class="sxs-lookup"><span data-stu-id="985e9-174">A blob receipt has the following  information:</span></span>

* <span data-ttu-id="985e9-175">Funktionen anropades för blob (”*{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*”, till exempel:” WebJob1.Functions.CopyBlob ”)</span><span class="sxs-lookup"><span data-stu-id="985e9-175">The function that was called for the blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="985e9-176">Behållarens namn</span><span class="sxs-lookup"><span data-stu-id="985e9-176">The container name</span></span>
* <span data-ttu-id="985e9-177">Blob-datatyp (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="985e9-177">The blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="985e9-178">Blobbnamnet</span><span class="sxs-lookup"><span data-stu-id="985e9-178">The blob name</span></span>
* <span data-ttu-id="985e9-179">ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="985e9-179">The ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="985e9-180">Om du vill tvinga ombearbetning av en blob kan du manuellt ta bort blob-inleverans för blobben från den *webjobs-azure-värdar* behållare.</span><span class="sxs-lookup"><span data-stu-id="985e9-180">If you want to force reprocessing of a blob, you can manually delete the blob receipt for that blob from the *azure-webjobs-hosts* container.</span></span>

## <a name="related-topics-covered-by-the-queues-article"></a><span data-ttu-id="985e9-181">Närliggande ämnen som omfattas av köer artikel</span><span class="sxs-lookup"><span data-stu-id="985e9-181">Related topics covered by the queues article</span></span>
<span data-ttu-id="985e9-182">För information om hur du hanterar blob-bearbetning som utlöses av ett kömeddelande eller för WebJobs SDK scenarier som inte är specifika för blob-bearbetning, se [använda Azure queue storage med WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="985e9-182">For information about how to handle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific to blob processing, see [How to use Azure queue storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span>

<span data-ttu-id="985e9-183">Närliggande ämnen som beskrivs i artikeln inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="985e9-183">Related topics covered in that article include the following:</span></span>

* <span data-ttu-id="985e9-184">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="985e9-184">Async functions</span></span>
* <span data-ttu-id="985e9-185">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="985e9-185">Multiple instances</span></span>
* <span data-ttu-id="985e9-186">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="985e9-186">Graceful shutdown</span></span>
* <span data-ttu-id="985e9-187">Använda WebJobs-SDK-attribut i brödtexten för en funktion</span><span class="sxs-lookup"><span data-stu-id="985e9-187">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="985e9-188">Ange anslutningssträngar SDK i koden.</span><span class="sxs-lookup"><span data-stu-id="985e9-188">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="985e9-189">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="985e9-189">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="985e9-190">Konfigurera **MaxDequeueCount** för hantering av skadligt blob.</span><span class="sxs-lookup"><span data-stu-id="985e9-190">Configure **MaxDequeueCount** for poison blob handling.</span></span>
* <span data-ttu-id="985e9-191">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="985e9-191">Trigger a function manually</span></span>
* <span data-ttu-id="985e9-192">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="985e9-192">Write logs</span></span>

## <a name="next-steps"></a><span data-ttu-id="985e9-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="985e9-193">Next steps</span></span>
<span data-ttu-id="985e9-194">Den här artikeln har lämnat kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="985e9-194">This article has provided code samples that show how to handle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="985e9-195">Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="985e9-195">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

