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
# <a name="azure-functions-blob-storage-bindings"></a><span data-ttu-id="582cf-104">Azure Functions Blob storage-bindningar</span><span class="sxs-lookup"><span data-stu-id="582cf-104">Azure Functions Blob storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="582cf-105">Den här artikeln förklarar hur tooconfigure och arbeta med Azure Blob storage bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="582cf-105">This article explains how tooconfigure and work with Azure Blob storage bindings in Azure Functions.</span></span> <span data-ttu-id="582cf-106">Azure Functions stöder utlösaren indata och utdata bindningar för Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="582cf-106">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="582cf-107">Funktioner som är tillgängliga i alla bindningar finns [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="582cf-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> <span data-ttu-id="582cf-108">En [blob storage-konto](../storage/common/storage-create-storage-account.md#blob-storage-accounts) stöds inte.</span><span class="sxs-lookup"><span data-stu-id="582cf-108">A [blob only storage account](../storage/common/storage-create-storage-account.md#blob-storage-accounts) is not supported.</span></span> <span data-ttu-id="582cf-109">BLOB storage-utlösare och bindningar kräver ett allmänt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="582cf-109">Blob storage triggers and bindings require a general-purpose storage account.</span></span> 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a><span data-ttu-id="582cf-110">BLOB storage-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="582cf-110">Blob storage triggers and bindings</span></span>

<span data-ttu-id="582cf-111">Använder hello Azure Blob storage-utlösare, anropas Funktionskoden när en ny eller uppdaterad blob har identifierats.</span><span class="sxs-lookup"><span data-stu-id="582cf-111">Using hello Azure Blob storage trigger, your function code is called when a new or updated blob is detected.</span></span> <span data-ttu-id="582cf-112">hello blobbinnehållet tillhandahålls som inkommande toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="582cf-112">hello blob contents are provided as input toohello function.</span></span>

<span data-ttu-id="582cf-113">Definiera en utlösare för lagring av blob med hello **integrera** fliken i hello Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="582cf-113">Define a blob storage trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="582cf-114">hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:</span><span class="sxs-lookup"><span data-stu-id="582cf-114">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

<span data-ttu-id="582cf-115">BLOB-indata och utdata bindningar definieras med hjälp av `blob` som hello bindningstyp:</span><span class="sxs-lookup"><span data-stu-id="582cf-115">Blob input and output bindings are defined using `blob` as hello binding type:</span></span>

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* <span data-ttu-id="582cf-116">Hej `path` egenskapen stöder bindande uttryck och filterparametrarna.</span><span class="sxs-lookup"><span data-stu-id="582cf-116">hello `path` property supports binding expressions and filter parameters.</span></span> <span data-ttu-id="582cf-117">Se [namn mönster](#pattern).</span><span class="sxs-lookup"><span data-stu-id="582cf-117">See [Name patterns](#pattern).</span></span>
* <span data-ttu-id="582cf-118">Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring.</span><span class="sxs-lookup"><span data-stu-id="582cf-118">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="582cf-119">I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="582cf-119">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="582cf-120">När du använder en blob-utlösare på en plan för förbrukning, kan det finnas upp tooa 10 minuters fördröjning vid bearbetningen av nya blobbar när en funktionsapp är inaktiv.</span><span class="sxs-lookup"><span data-stu-id="582cf-120">When you're using a blob trigger on a Consumption plan, there can be up tooa 10-minute delay in processing new blobs after a function app has gone idle.</span></span> <span data-ttu-id="582cf-121">När hello funktionen appen körs bearbetas blobbar omedelbart.</span><span class="sxs-lookup"><span data-stu-id="582cf-121">After hello function app is running, blobs are processed immediately.</span></span> <span data-ttu-id="582cf-122">Det här första tooavoid fördröjning bör du överväga att något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="582cf-122">tooavoid this initial delay, consider one of hello following options:</span></span>
> - <span data-ttu-id="582cf-123">Använda en apptjänstplan med alltid på aktiverad.</span><span class="sxs-lookup"><span data-stu-id="582cf-123">Use an App Service plan with Always On enabled.</span></span>
> - <span data-ttu-id="582cf-124">Använd tootrigger hello av en annan mekanism-blob som bearbetning, till exempel ett kömeddelande som innehåller hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="582cf-124">Use another mechanism tootrigger hello blob processing, such as a queue message that contains hello blob name.</span></span> <span data-ttu-id="582cf-125">Ett exempel finns [kön utlösare med blob inkommande bindningen](#input-sample).</span><span class="sxs-lookup"><span data-stu-id="582cf-125">For an example, see [Queue trigger with blob input binding](#input-sample).</span></span>

<a name="pattern"></a>

### <a name="name-patterns"></a><span data-ttu-id="582cf-126">Namnet mönster</span><span class="sxs-lookup"><span data-stu-id="582cf-126">Name patterns</span></span>
<span data-ttu-id="582cf-127">Du kan ange ett mönster för blob-namnet i hello `path` -egenskap som kan vara ett filter eller bindande uttryck.</span><span class="sxs-lookup"><span data-stu-id="582cf-127">You can specify a blob name pattern in hello `path` property, which can be a filter or binding expression.</span></span> <span data-ttu-id="582cf-128">Se [bindande uttryck och mönster](functions-triggers-bindings.md#binding-expressions-and-patterns).</span><span class="sxs-lookup"><span data-stu-id="582cf-128">See [Binding expressions and patterns](functions-triggers-bindings.md#binding-expressions-and-patterns).</span></span>

<span data-ttu-id="582cf-129">Till exempel använda toofilter tooblobs som börjar med hello strängen ”ursprungliga” hello definitionen.</span><span class="sxs-lookup"><span data-stu-id="582cf-129">For example, toofilter tooblobs that start with hello string "original," use hello following definition.</span></span> <span data-ttu-id="582cf-130">Den här sökvägen hittar en blob med namnet *ursprungliga Blob1.txt* i hello *inkommande* behållare och hello värdet för hello `name` variabeln i Funktionskoden är `Blob1`.</span><span class="sxs-lookup"><span data-stu-id="582cf-130">This path finds a blob named *original-Blob1.txt* in hello *input* container, and hello value of hello `name` variable in function code is `Blob1`.</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="582cf-131">toobind toohello blob-filnamnet och filnamnstillägget separat, använda två mönster.</span><span class="sxs-lookup"><span data-stu-id="582cf-131">toobind toohello blob file name and extension separately, use two patterns.</span></span> <span data-ttu-id="582cf-132">Den här sökvägen hittar också en blob med namnet *ursprungliga Blob1.txt*, och hello värdet för hello `blobname` och `blobextension` variabler i Funktionskoden är *ursprungliga Blob1* och *txt*.</span><span class="sxs-lookup"><span data-stu-id="582cf-132">This path also finds a blob named *original-Blob1.txt*, and hello value of hello `blobname` and `blobextension` variables in function code are *original-Blob1* and *txt*.</span></span>

```json
"path": "input/{blobname}.{blobextension}",
```

<span data-ttu-id="582cf-133">Du kan begränsa hello filtyp blobbar med hjälp av ett fast värde för hello filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="582cf-133">You can restrict hello file type of blobs by using a fixed value for hello file extension.</span></span> <span data-ttu-id="582cf-134">Till exempel tootrigger endast på PNG-filer, Använd hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="582cf-134">For instance, tootrigger only on .png files, use hello following pattern:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="582cf-135">Klammerparenteser är specialtecken i namnet mönster.</span><span class="sxs-lookup"><span data-stu-id="582cf-135">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="582cf-136">toospecify blob-namn som innehåller klammerparenteser i hello namn du escape hello klammerparenteser med två klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="582cf-136">toospecify blob names that have curly braces in hello name, you can escape hello braces using two braces.</span></span> <span data-ttu-id="582cf-137">hello följande exempel söker efter en blob med namnet *{20140101}-soundfile.mp3* i hello *bilder* behållare och hello `name` variabelvärdet i hello Funktionskoden är  *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="582cf-137">hello following example finds a blob named *{20140101}-soundfile.mp3* in hello *images* container, and hello `name` variable value in hello function code is *soundfile.mp3*.</span></span> 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a><span data-ttu-id="582cf-138">Utlösaren metadata</span><span class="sxs-lookup"><span data-stu-id="582cf-138">Trigger metadata</span></span>

<span data-ttu-id="582cf-139">hello blob-utlösare innehåller flera metadataegenskaper för.</span><span class="sxs-lookup"><span data-stu-id="582cf-139">hello blob trigger provides several metadata properties.</span></span> <span data-ttu-id="582cf-140">De här egenskaperna kan användas som en del av bindningar uttryck i andra bindningar eller parametrar i din kod.</span><span class="sxs-lookup"><span data-stu-id="582cf-140">These properties can be used as part of bindings expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="582cf-141">Dessa värden ha hello samma semantik som [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="582cf-141">These values have hello same semantics as [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span></span>

- <span data-ttu-id="582cf-142">**BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="582cf-142">**BlobTrigger**.</span></span> <span data-ttu-id="582cf-143">Skriv `string`.</span><span class="sxs-lookup"><span data-stu-id="582cf-143">Type `string`.</span></span> <span data-ttu-id="582cf-144">hello utlösande blob-sökväg</span><span class="sxs-lookup"><span data-stu-id="582cf-144">hello triggering blob path</span></span>
- <span data-ttu-id="582cf-145">**URI**.</span><span class="sxs-lookup"><span data-stu-id="582cf-145">**Uri**.</span></span> <span data-ttu-id="582cf-146">Skriv `System.Uri`.</span><span class="sxs-lookup"><span data-stu-id="582cf-146">Type `System.Uri`.</span></span> <span data-ttu-id="582cf-147">hello blob-URI för hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="582cf-147">hello blob's URI for hello primary location.</span></span>
- <span data-ttu-id="582cf-148">**Egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="582cf-148">**Properties**.</span></span> <span data-ttu-id="582cf-149">Skriv `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span><span class="sxs-lookup"><span data-stu-id="582cf-149">Type `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span></span> <span data-ttu-id="582cf-150">Hej blob Systemegenskaper.</span><span class="sxs-lookup"><span data-stu-id="582cf-150">hello blob's system properties.</span></span>
- <span data-ttu-id="582cf-151">**Metadata**.</span><span class="sxs-lookup"><span data-stu-id="582cf-151">**Metadata**.</span></span> <span data-ttu-id="582cf-152">Skriv `IDictionary<string,string>`.</span><span class="sxs-lookup"><span data-stu-id="582cf-152">Type `IDictionary<string,string>`.</span></span> <span data-ttu-id="582cf-153">hello användardefinierade metadata för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="582cf-153">hello user-defined metadata for hello blob.</span></span>

<a name="receipts"></a>

### <a name="blob-receipts"></a><span data-ttu-id="582cf-154">BLOB kvitton</span><span class="sxs-lookup"><span data-stu-id="582cf-154">Blob receipts</span></span>
<span data-ttu-id="582cf-155">hello Azure Functions-runtime garanterar att inga utlösare blob-funktionen anropas mer än en gång för hello samma nya eller uppdaterade blob.</span><span class="sxs-lookup"><span data-stu-id="582cf-155">hello Azure Functions runtime ensures that no blob trigger function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="582cf-156">toodetermine om en viss blob-version har bearbetats, den underhåller *blob kvitton*.</span><span class="sxs-lookup"><span data-stu-id="582cf-156">toodetermine if a given blob version has been processed, it maintains *blob receipts*.</span></span>

<span data-ttu-id="582cf-157">Azure Functions lagrar blob inleveranser i en behållare med namnet *webjobs-azure-värdar* i hello Azure storage-konto för din funktionsapp (definieras av hello appinställningen `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="582cf-157">Azure Functions stores blob receipts in a container named *azure-webjobs-hosts* in hello Azure storage account for your function app (defined by hello app setting `AzureWebJobsStorage`).</span></span> <span data-ttu-id="582cf-158">En blob-inleverans har hello följande information:</span><span class="sxs-lookup"><span data-stu-id="582cf-158">A blob receipt has hello following information:</span></span>

* <span data-ttu-id="582cf-159">hello aktiveras funktionen (”*&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*”, till exempel:” MyFunctionApp.Functions.CopyBlob ”)</span><span class="sxs-lookup"><span data-stu-id="582cf-159">hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "MyFunctionApp.Functions.CopyBlob")</span></span>
* <span data-ttu-id="582cf-160">hello-behållarnamn</span><span class="sxs-lookup"><span data-stu-id="582cf-160">hello container name</span></span>
* <span data-ttu-id="582cf-161">hello blob-datatyp (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="582cf-161">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="582cf-162">Hej blobbnamnet</span><span class="sxs-lookup"><span data-stu-id="582cf-162">hello blob name</span></span>
* <span data-ttu-id="582cf-163">hello ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="582cf-163">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="582cf-164">tooforce ombearbetning av en blob, ta bort hello blob inleverans för blobben från hello *webjobs-azure-värdar* behållaren manuellt.</span><span class="sxs-lookup"><span data-stu-id="582cf-164">tooforce reprocessing of a blob, delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container manually.</span></span>

<a name="poison"></a>

### <a name="handling-poison-blobs"></a><span data-ttu-id="582cf-165">Hantering av skadligt blobbar</span><span class="sxs-lookup"><span data-stu-id="582cf-165">Handling poison blobs</span></span>
<span data-ttu-id="582cf-166">När en blob utlösaren misslyckas åtgärden för en given blob Azure Functions försök som fungerar totalt 5 gånger som standard.</span><span class="sxs-lookup"><span data-stu-id="582cf-166">When a blob trigger function fails for a given blob, Azure Functions retries that function a total of 5 times by default.</span></span> 

<span data-ttu-id="582cf-167">Om alla 5 försök misslyckas Azure Functions lägger till en tooa lagring meddelandekö med namnet *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="582cf-167">If all 5 tries fail, Azure Functions adds a message tooa Storage queue named *webjobs-blobtrigger-poison*.</span></span> <span data-ttu-id="582cf-168">hälsningsmeddelande i kö för skadligt BLOB är en JSON-objekt som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="582cf-168">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="582cf-169">FunctionId (hello format  *&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*)</span><span class="sxs-lookup"><span data-stu-id="582cf-169">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="582cf-170">BlobType (”BlockBlob” eller ”PageBlob”)</span><span class="sxs-lookup"><span data-stu-id="582cf-170">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="582cf-171">ContainerName</span><span class="sxs-lookup"><span data-stu-id="582cf-171">ContainerName</span></span>
* <span data-ttu-id="582cf-172">BlobName</span><span class="sxs-lookup"><span data-stu-id="582cf-172">BlobName</span></span>
* <span data-ttu-id="582cf-173">ETag (en blob versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="582cf-173">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

### <a name="blob-polling-for-large-containers"></a><span data-ttu-id="582cf-174">BLOB avsöker för stora behållare</span><span class="sxs-lookup"><span data-stu-id="582cf-174">Blob polling for large containers</span></span>
<span data-ttu-id="582cf-175">Om hello blob-behållare som övervakas innehåller fler än 10 000 blobbar, logga hello funktioner runtime söker igenom filer toowatch för nya eller ändrade BLOB.</span><span class="sxs-lookup"><span data-stu-id="582cf-175">If hello blob container being monitored contains more than 10,000 blobs, hello Functions runtime scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="582cf-176">Den här processen är inte i realtid.</span><span class="sxs-lookup"><span data-stu-id="582cf-176">This process is not real time.</span></span> <span data-ttu-id="582cf-177">En funktion kan hämta aktiveras inte förrän flera minuter eller längre efter hello-blob har skapats.</span><span class="sxs-lookup"><span data-stu-id="582cf-177">A function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="582cf-178">Dessutom [lagring loggfiler skapas på ”bästa prestanda”](/rest/api/storageservices/About-Storage-Analytics-Logging) basis.</span><span class="sxs-lookup"><span data-stu-id="582cf-178">In addition, [storage logs are created on a "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) basis.</span></span> <span data-ttu-id="582cf-179">Det är inte säkert att alla händelser fångas.</span><span class="sxs-lookup"><span data-stu-id="582cf-179">There is no guarantee that all events are captured.</span></span> <span data-ttu-id="582cf-180">Loggar under vissa förhållanden kan missas.</span><span class="sxs-lookup"><span data-stu-id="582cf-180">Under some conditions, logs may be missed.</span></span> <span data-ttu-id="582cf-181">Om du behöver snabbare och mer tillförlitlig blob-bearbetning kan du skapa en [kömeddelande](../storage/queues/storage-dotnet-how-to-use-queues.md) när du skapar hello-blob.</span><span class="sxs-lookup"><span data-stu-id="582cf-181">If you require faster or more reliable blob processing, consider creating a [queue message](../storage/queues/storage-dotnet-how-to-use-queues.md) when you create hello blob.</span></span> <span data-ttu-id="582cf-182">Använd sedan en [kö utlösaren](functions-bindings-storage-queue.md) i stället för en blob utlösaren tooprocess hello-blob.</span><span class="sxs-lookup"><span data-stu-id="582cf-182">Then, use a [queue trigger](functions-bindings-storage-queue.md) instead of a blob trigger tooprocess hello blob.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a><span data-ttu-id="582cf-183">Med hjälp av en blob-utlösare och indatabindning</span><span class="sxs-lookup"><span data-stu-id="582cf-183">Using a blob trigger and input binding</span></span>
<span data-ttu-id="582cf-184">Komma åt hello blob-data med en metodparameter i .NET-funktioner `Stream paramName`.</span><span class="sxs-lookup"><span data-stu-id="582cf-184">In .NET functions, access hello blob data using a method parameter such as `Stream paramName`.</span></span> <span data-ttu-id="582cf-185">Här, `paramName` är hello-värde som du angav i hello [konfiguration av utlösare](#trigger).</span><span class="sxs-lookup"><span data-stu-id="582cf-185">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="582cf-186">I Node.js-funktion, ange åtkomst hello blob data med hjälp av `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="582cf-186">In Node.js functions, access hello input blob data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="582cf-187">I .NET, kan du binda tooany hello typer i hello listan nedan.</span><span class="sxs-lookup"><span data-stu-id="582cf-187">In .NET, you can bind tooany of hello types in hello list below.</span></span> <span data-ttu-id="582cf-188">Om används som en indatabindning vissa av dessa typer kräver en `inout` bindning riktning i *function.json*.</span><span class="sxs-lookup"><span data-stu-id="582cf-188">If used as an input binding, some of these types require an `inout` binding direction in *function.json*.</span></span> <span data-ttu-id="582cf-189">Den här riktningen stöds inte av hello standardredigeraren, så du måste använda hello Avancerad redigerare.</span><span class="sxs-lookup"><span data-stu-id="582cf-189">This direction is not supported by hello standard editor, so you must use hello advanced editor.</span></span>

* `TextReader`
* `Stream`
* <span data-ttu-id="582cf-190">`ICloudBlob`(kräver ”inout” bindning riktning)</span><span class="sxs-lookup"><span data-stu-id="582cf-190">`ICloudBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="582cf-191">`CloudBlockBlob`(kräver ”inout” bindning riktning)</span><span class="sxs-lookup"><span data-stu-id="582cf-191">`CloudBlockBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="582cf-192">`CloudPageBlob`(kräver ”inout” bindning riktning)</span><span class="sxs-lookup"><span data-stu-id="582cf-192">`CloudPageBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="582cf-193">`CloudAppendBlob`(kräver ”inout” bindning riktning)</span><span class="sxs-lookup"><span data-stu-id="582cf-193">`CloudAppendBlob` (requires "inout" binding direction)</span></span>

<span data-ttu-id="582cf-194">Om texten blobbar förväntas, du kan också binda tooa .NET `string` typen.</span><span class="sxs-lookup"><span data-stu-id="582cf-194">If text blobs are expected, you can also bind tooa .NET `string` type.</span></span> <span data-ttu-id="582cf-195">Detta rekommenderas endast om hello blobbstorleken är liten, som hello hela blobbinnehållet läses in i minnet.</span><span class="sxs-lookup"><span data-stu-id="582cf-195">This is only recommended if hello blob size is small, as hello entire blob contents are loaded into memory.</span></span> <span data-ttu-id="582cf-196">Vanligtvis är det bättre toouse en `Stream` eller `CloudBlockBlob` typen.</span><span class="sxs-lookup"><span data-stu-id="582cf-196">Generally, it is preferable toouse a `Stream` or `CloudBlockBlob` type.</span></span>

## <a name="trigger-sample"></a><span data-ttu-id="582cf-197">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="582cf-197">Trigger sample</span></span>
<span data-ttu-id="582cf-198">Anta att du har hello följande function.json som definierar en blob storage-utlösare:</span><span class="sxs-lookup"><span data-stu-id="582cf-198">Suppose you have hello following function.json that defines a blob storage trigger:</span></span>

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

<span data-ttu-id="582cf-199">Se hello språkspecifika exempel loggar hello innehållet i varje blob som har lagts till toohello övervakade behållare.</span><span class="sxs-lookup"><span data-stu-id="582cf-199">See hello language-specific sample that logs hello contents of each blob that is added toohello monitored container.</span></span>

* [<span data-ttu-id="582cf-200">C#</span><span class="sxs-lookup"><span data-stu-id="582cf-200">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="582cf-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="582cf-201">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a><span data-ttu-id="582cf-202">BLOB-utlösare exemplen i C#</span><span class="sxs-lookup"><span data-stu-id="582cf-202">Blob trigger examples in C#</span></span> #

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

### <a name="trigger-example-in-nodejs"></a><span data-ttu-id="582cf-203">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="582cf-203">Trigger example in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a><span data-ttu-id="582cf-204">Med hjälp av en blob-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="582cf-204">Using a blob output binding</span></span>

<span data-ttu-id="582cf-205">I .NET-funktioner, bör du antingen genom att använda en `out string` parameter i funktionssignaturen eller Använd en hello typer i hello följande lista.</span><span class="sxs-lookup"><span data-stu-id="582cf-205">In .NET functions, you should either use a `out string` parameter in your function signature or use one of hello types in hello following list.</span></span> <span data-ttu-id="582cf-206">I Node.js-funktion, du åtkomst till hello utdata blob med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="582cf-206">In Node.js functions, you access hello output blob using `context.bindings.<name>`.</span></span>

<span data-ttu-id="582cf-207">Du kan spara tooany av hello följande typer i .NET-funktioner:</span><span class="sxs-lookup"><span data-stu-id="582cf-207">In .NET functions you can output tooany of hello following types:</span></span>

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a><span data-ttu-id="582cf-208">Kön utlösare med blob inkommande och utgående exempel</span><span class="sxs-lookup"><span data-stu-id="582cf-208">Queue trigger with blob input and output sample</span></span>
<span data-ttu-id="582cf-209">Anta att du har följande function.json hello som definierar en [Queue Storage utlösaren](functions-bindings-storage-queue.md)blob-lagring indata och utdata för ett blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="582cf-209">Suppose you have hello following function.json, that defines a [Queue Storage trigger](functions-bindings-storage-queue.md), a blob storage input, and a blob storage output.</span></span> <span data-ttu-id="582cf-210">Meddelande hello användning av hello `queueTrigger` metadata för egenskapen.</span><span class="sxs-lookup"><span data-stu-id="582cf-210">Notice hello use of hello `queueTrigger` metadata property.</span></span> <span data-ttu-id="582cf-211">i hello blob indata och utdata `path` egenskaper:</span><span class="sxs-lookup"><span data-stu-id="582cf-211">in hello blob input and output `path` properties:</span></span>

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

<span data-ttu-id="582cf-212">Se hello språkspecifika prov som kopierar hello inkommande blob toohello utdata blob.</span><span class="sxs-lookup"><span data-stu-id="582cf-212">See hello language-specific sample that copies hello input blob toohello output blob.</span></span>

* [<span data-ttu-id="582cf-213">C#</span><span class="sxs-lookup"><span data-stu-id="582cf-213">C#</span></span>](#incsharp)
* [<span data-ttu-id="582cf-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="582cf-214">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a><span data-ttu-id="582cf-215">Exempel för BLOB-bindning i C#</span><span class="sxs-lookup"><span data-stu-id="582cf-215">Blob binding example in C#</span></span> #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a><span data-ttu-id="582cf-216">Exempel för BLOB-bindning i Node.js</span><span class="sxs-lookup"><span data-stu-id="582cf-216">Blob binding example in Node.js</span></span>

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="582cf-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="582cf-217">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

