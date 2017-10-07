---
title: aaaAzure funktioner Blob Storage bindningar | Microsoft Docs
description: "Förstå hur toouse Azure Storage utlösare och bindningar i Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: cef44bd2154d0b97cca9220b6c5024a5b620c80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Azure Functions Blob storage-bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och arbeta med Azure Blob storage bindningar i Azure Functions. Azure Functions stöder utlösaren indata och utdata bindningar för Azure Blob storage. Funktioner som är tillgängliga i alla bindningar finns [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> En [blob storage-konto](../storage/common/storage-create-storage-account.md#blob-storage-accounts) stöds inte. BLOB storage-utlösare och bindningar kräver ett allmänt lagringskonto. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>BLOB storage-utlösare och bindningar

Använder hello Azure Blob storage-utlösare, anropas Funktionskoden när en ny eller uppdaterad blob har identifierats. hello blobbinnehållet tillhandahålls som inkommande toohello funktion.

Definiera en utlösare för lagring av blob med hello **integrera** fliken i hello Functions-portalen. hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

BLOB-indata och utdata bindningar definieras med hjälp av `blob` som hello bindningstyp:

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* Hej `path` egenskapen stöder bindande uttryck och filterparametrarna. Se [namn mönster](#pattern).
* Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring. I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.

> [!NOTE]
> När du använder en blob-utlösare på en plan för förbrukning, kan det finnas upp tooa 10 minuters fördröjning vid bearbetningen av nya blobbar när en funktionsapp är inaktiv. När hello funktionen appen körs bearbetas blobbar omedelbart. Det här första tooavoid fördröjning bör du överväga att något av följande alternativ för hello:
> - Använda en apptjänstplan med alltid på aktiverad.
> - Använd tootrigger hello av en annan mekanism-blob som bearbetning, till exempel ett kömeddelande som innehåller hello blob-namnet. Ett exempel finns [kön utlösare med blob inkommande bindningen](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Namnet mönster
Du kan ange ett mönster för blob-namnet i hello `path` -egenskap som kan vara ett filter eller bindande uttryck. Se [bindande uttryck och mönster](functions-triggers-bindings.md#binding-expressions-and-patterns).

Till exempel använda toofilter tooblobs som börjar med hello strängen ”ursprungliga” hello definitionen. Den här sökvägen hittar en blob med namnet *ursprungliga Blob1.txt* i hello *inkommande* behållare och hello värdet för hello `name` variabeln i Funktionskoden är `Blob1`.

```json
"path": "input/original-{name}",
```

toobind toohello blob-filnamnet och filnamnstillägget separat, använda två mönster. Den här sökvägen hittar också en blob med namnet *ursprungliga Blob1.txt*, och hello värdet för hello `blobname` och `blobextension` variabler i Funktionskoden är *ursprungliga Blob1* och *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Du kan begränsa hello filtyp blobbar med hjälp av ett fast värde för hello filnamnstillägg. Till exempel tootrigger endast på PNG-filer, Använd hello följande mönster:

```json
"path": "samples/{name}.png",
```

Klammerparenteser är specialtecken i namnet mönster. toospecify blob-namn som innehåller klammerparenteser i hello namn du escape hello klammerparenteser med två klammerparenteser. hello följande exempel söker efter en blob med namnet *{20140101}-soundfile.mp3* i hello *bilder* behållare och hello `name` variabelvärdet i hello Funktionskoden är  *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Utlösaren metadata

hello blob-utlösare innehåller flera metadataegenskaper för. De här egenskaperna kan användas som en del av bindningar uttryck i andra bindningar eller parametrar i din kod. Dessa värden ha hello samma semantik som [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Skriv `string`. hello utlösande blob-sökväg
- **URI**. Skriv `System.Uri`. hello blob-URI för hello primär plats.
- **Egenskaper för**. Skriv `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. Hej blob Systemegenskaper.
- **Metadata**. Skriv `IDictionary<string,string>`. hello användardefinierade metadata för hello-blob.

<a name="receipts"></a>

### <a name="blob-receipts"></a>BLOB kvitton
hello Azure Functions-runtime garanterar att inga utlösare blob-funktionen anropas mer än en gång för hello samma nya eller uppdaterade blob. toodetermine om en viss blob-version har bearbetats, den underhåller *blob kvitton*.

Azure Functions lagrar blob inleveranser i en behållare med namnet *webjobs-azure-värdar* i hello Azure storage-konto för din funktionsapp (definieras av hello appinställningen `AzureWebJobsStorage`). En blob-inleverans har hello följande information:

* hello aktiveras funktionen (”*&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*”, till exempel:” MyFunctionApp.Functions.CopyBlob ”)
* hello-behållarnamn
* hello blob-datatyp (”BlockBlob” eller ”PageBlob”)
* Hej blobbnamnet
* hello ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)

tooforce ombearbetning av en blob, ta bort hello blob inleverans för blobben från hello *webjobs-azure-värdar* behållaren manuellt.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Hantering av skadligt blobbar
När en blob utlösaren misslyckas åtgärden för en given blob Azure Functions försök som fungerar totalt 5 gånger som standard. 

Om alla 5 försök misslyckas Azure Functions lägger till en tooa lagring meddelandekö med namnet *webjobs-blobtrigger-poison*. hälsningsmeddelande i kö för skadligt BLOB är en JSON-objekt som innehåller hello följande egenskaper:

* FunctionId (hello format  *&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*)
* BlobType (”BlockBlob” eller ”PageBlob”)
* ContainerName
* BlobName
* ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)

### <a name="blob-polling-for-large-containers"></a>BLOB avsöker för stora behållare
Om hello blob-behållare som övervakas innehåller fler än 10 000 blobbar, logga hello funktioner runtime söker igenom filer toowatch för nya eller ändrade BLOB. Den här processen är inte i realtid. En funktion kan hämta aktiveras inte förrän flera minuter eller längre efter hello-blob har skapats. Dessutom [lagring loggfiler skapas på ”bästa prestanda”](/rest/api/storageservices/About-Storage-Analytics-Logging) basis. Det är inte säkert att alla händelser fångas. Loggar under vissa förhållanden kan missas. Om du behöver snabbare och mer tillförlitlig blob-bearbetning kan du skapa en [kömeddelande](../storage/queues/storage-dotnet-how-to-use-queues.md) när du skapar hello-blob. Använd sedan en [kö utlösaren](functions-bindings-storage-queue.md) i stället för en blob utlösaren tooprocess hello-blob.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Med hjälp av en blob-utlösare och indatabindning
Komma åt hello blob-data med en metodparameter i .NET-funktioner `Stream paramName`. Här, `paramName` är hello-värde som du angav i hello [konfiguration av utlösare](#trigger). I Node.js-funktion, ange åtkomst hello blob data med hjälp av `context.bindings.<name>`.

I .NET, kan du binda tooany hello typer i hello listan nedan. Om används som en indatabindning vissa av dessa typer kräver en `inout` bindning riktning i *function.json*. Den här riktningen stöds inte av hello standardredigeraren, så du måste använda hello Avancerad redigerare.

* `TextReader`
* `Stream`
* `ICloudBlob`(kräver ”inout” bindning riktning)
* `CloudBlockBlob`(kräver ”inout” bindning riktning)
* `CloudPageBlob`(kräver ”inout” bindning riktning)
* `CloudAppendBlob`(kräver ”inout” bindning riktning)

Om texten blobbar förväntas, du kan också binda tooa .NET `string` typen. Detta rekommenderas endast om hello blobbstorleken är liten, som hello hela blobbinnehållet läses in i minnet. Vanligtvis är det bättre toouse en `Stream` eller `CloudBlockBlob` typen.

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har hello följande function.json som definierar en blob storage-utlösare:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Se hello språkspecifika exempel loggar hello innehållet i varje blob som har lagts till toohello övervakade behållare.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>BLOB-utlösare exemplen i C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding tooa CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Utlösaren exemplet i Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Med hjälp av en blob-utdatabindning

I .NET-funktioner, bör du antingen genom att använda en `out string` parameter i funktionssignaturen eller Använd en hello typer i hello följande lista. I Node.js-funktion, du åtkomst till hello utdata blob med `context.bindings.<name>`.

Du kan spara tooany av hello följande typer i .NET-funktioner:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Kön utlösare med blob inkommande och utgående exempel
Anta att du har följande function.json hello som definierar en [Queue Storage utlösaren](functions-bindings-storage-queue.md)blob-lagring indata och utdata för ett blob-lagring. Meddelande hello användning av hello `queueTrigger` metadata för egenskapen. i hello blob indata och utdata `path` egenskaper:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Se hello språkspecifika prov som kopierar hello inkommande blob toohello utdata blob.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Exempel för BLOB-bindning i C# #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Exempel för BLOB-bindning i Node.js

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

