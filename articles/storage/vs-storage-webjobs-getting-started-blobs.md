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
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Kom igång med Azure Blob storage och Visual Studio anslutna tjänster (Webbjobb projekt)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Den här artikeln innehåller C# kod exempel som visar hur tootrigger en process när ett Azure BLOB-objekt skapas eller uppdateras. hello kodexempel använda hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x. När du lägger till ett storage-konto tooa Webbjobb projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan hello lämplig Azure Storage NuGet-paketet har installerats, hello lämpliga .NET-referenserna är tillagda toohello projektet och anslutningssträngar för hello storage-konto uppdateras i hello App.config-fil.

## <a name="how-tootrigger-a-function-when-a-blob-is-created-or-updated"></a>Hur tootrigger en funktion när en blob skapas eller uppdateras
Det här avsnittet visas hur toouse hello **BlobTrigger** attribut.

 **Obs:** hello WebJobs SDK genomsökningar loggen filer toowatch för nya eller ändrade BLOB. Den här processen är långsam natur; en funktion kan hämta aktiveras inte förrän flera minuter eller längre efter hello-blob har skapats.  Om ditt program måste omedelbart tooprocess blobbar, hello rekommenderade metoden är toocreate ett kömeddelande när du skapar hello blob och använder hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribut i stället för hello **BlobTrigger** -attributet på hello-funktion som behandlar hello-blob.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Enskild platshållare för blob-namn med filtillägg
hello följande kodexempel kopierar text blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

hello Attributkonstruktorn använder en strängparameter som anger hello behållarens namn och en platshållare för hello blob-namnet. I det här exemplet, om en blob med namnet *Blob1.txt* skapas i hello *inkommande* behållare, hello funktionen skapas en blob med namnet *Blob1.txt* i hello *utdata*  behållare.

Du kan ange ett mönster med hello blob platshållare, som visas i följande kodexempel hello:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Den här koden kopieras endast blob som har namn som börjar med ”ursprungliga-”. Till exempel *ursprungliga Blob1.txt* i hello *inkommande* behållare kopieras för*kopiera Blob1.txt* i hello *utdata* behållare.

Om du behöver toospecify en namnmönstret för blob-namn som innehåller klammerparenteser i hello namn dubbel hello klammerparenteser. Om du vill toofind blobbar i hello exempelvis *bilder* behållare som har namn så här:

        {20140101}-soundfile.mp3

Använd detta för dina mönster:

        images/{{20140101}}-{name}

I exemplet hello hello *namn* platshållare värdet *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Separat blob namn och filnamnstillägg för platshållare
hello följande exempel kodändringar hello filtillägg som kopieras blobbar som visas i hello *inkommande* behållare toohello *utdata* behållare. hello kod loggar hello-tillägget för hello *inkommande* blob och anger hello-tillägget för hello *utdata* blob för*.txt*.

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

## <a name="types-that-you-can-bind-tooblobs"></a>Typer som du kan binda tooblobs
Du kan använda hello **BlobTrigger** -attributet på hello följande typer:

* **sträng**
* **TextReader**
* **Dataströmmen**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andra typer som avserialiseras av [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Om du vill toowork direkt med hello Azure storage-konto, du kan också lägga till en **CloudStorageAccount** Metodsignaturen för parametern toohello.

## <a name="getting-text-blob-content-by-binding-toostring"></a>Hämtar blob textinnehåll av bindning toostring
Om texten blobbar förväntas **BlobTrigger** kan vara tillämpade tooa **sträng** parameter. hello följande kodexempel Binder en text blob tooa **sträng** parameter med namnet **logMessage**. hello funktionen använder de parametern toowrite hello innehållet i hello blob toohello WebJobs-SDK-instrumentpanelen.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Hämta serialiserad blob-innehåll med hjälp av ICloudBlobStreamBinder
hello följande kodexempel används en klass som implementerar **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attributet toobind en blob-toohello **WebImage** typen.

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

Hej **WebImage** bindning kod har angetts i en **WebImageBinder** klass som härleds från **ICloudBlobStreamBinder**.

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

## <a name="how-toohandle-poison-blobs"></a>Hur toohandle poison BLOB-objekt
När en **BlobTrigger** funktionen misslyckas hello SDK anropar den igen, om hello misslyckandet orsakades av ett tillfälligt fel. Om hello felet orsakas av hello innehållet i hello blob fungerar hello funktion varje gång den försöker tooprocess hello blob. Som standard anropar hello SDK en funktion in too5 gånger för en given blob. Om hello femte försök misslyckas hello SDK lägger till en meddelandekö tooa med namnet *webjobs-blobtrigger-poison*.

hello maximalt antal försök kan konfigureras. Hej samma [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) inställningen används för hantering av skadligt blob och hantering av skadligt kön meddelandet.

hälsningsmeddelande i kö för skadligt BLOB är en JSON-objekt som innehåller hello följande egenskaper:

* FunctionId (hello format *{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*, till exempel: WebJob1.Functions.CopyBlob)
* BlobType (”BlockBlob” eller ”PageBlob”)
* ContainerName
* BlobName
* ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)

I följande hello code exemplet hello **CopyBlob** funktionen har kod som gör att den toofail varje gång den anropas. När hello SDK anropar den för hello maximalt antal försök, skapa ett meddelande på hello skadligt blob kön och meddelandet bearbetas av hello **LogPoisonBlob** funktion.

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

hello SDK deserializes automatiskt hello JSON-meddelande. Här är hello **PoisonBlobMessage** klass:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Algoritmen för BLOB-avsökning
Hej WebJobs SDK söker igenom alla behållare som anges av **BlobTrigger** attribut vid programstart. I ett stort lagringskonto skanningen kan ta lite tid, så det kan vara en stund innan nya blobbar finns och **BlobTrigger** funktioner utförs.

toodetect nya eller ändrade blobar efter start av programmet hello SDK regelbundet läser från hello blob storage loggar. hello blob är buffrade och bara hämta fysiskt skriva var 10: e minut eller så, så det kan finnas betydande fördröjning när en blob skapas eller uppdateras innan hello motsvarande **BlobTrigger** funktionen körs.

Det finns ett undantag för blob som du skapar med hjälp av hello **Blob** attribut. När hello WebJobs SDK skapar en ny blob, passerar hello nya blob omedelbart tooany matchar **BlobTrigger** funktioner. Därför om du har en kedja av blob-in- och utdataenheter kan hello SDK bearbeta dem effektivt. Men om du vill låg latens och kör din blob bearbetningsfunktioner för blob som skapats eller uppdaterats på annat sätt, bör du använda **QueueTrigger** snarare än **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB kvitton
Hej WebJobs SDK säkerställer du att inga **BlobTrigger** funktionen anropas mer än en gång för hello samma nya eller uppdaterade blob. Detta åstadkoms genom att upprätthålla *blob kvitton* i ordning toodetermine om en viss blob-version har bearbetats.

BLOB kvitton lagras i en behållare med namnet *webjobs-azure-värdar* i hello Azure storage-konto som anges av hello AzureWebJobsStorage anslutningssträngen. En blob-inleverans har hello följande information:

* Hej funktion som anropades för hello blob (”*{webbjobbsnamnet}*. Funktioner. *{Funktionsnamn}*”, till exempel:” WebJob1.Functions.CopyBlob ”)
* hello-behållarnamn
* hello blob-datatyp (”BlockBlob” eller ”PageBlob”)
* Hej blobbnamnet
* hello ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)

Om du vill tooforce ombearbetning av en blobb måste du manuellt ta bort hello blob inleverans för blobben från hello *webjobs-azure-värdar* behållare.

## <a name="related-topics-covered-by-hello-queues-article"></a>Närliggande ämnen som omfattas av hello köer artikel
Information om hur toohandle blob bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tooblob bearbetning, se [hur toouse Azure kö lagring med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Närliggande ämnen som beskrivs i artikeln inkludera hello följande:

* Async-funktion
* Flera instanser
* Korrekt avslutning
* Använda WebJobs-SDK-attribut i hello brödtext
* Ange hello SDK-anslutningssträngar i koden.
* Ange värden för WebJobs SDK konstruktorparametrarna i koden
* Konfigurera **MaxDequeueCount** för hantering av skadligt blob.
* Utlös en funktion manuellt
* Skriva loggar

## <a name="next-steps"></a>Nästa steg
Den här artikeln har lämnat kodexempel som visar hur toohandle vanliga scenarier för att arbeta med Azure BLOB-objekt. Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).

