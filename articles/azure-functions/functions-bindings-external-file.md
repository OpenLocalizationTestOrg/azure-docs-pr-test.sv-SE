---
title: "aaaAzure funktioner extern fil Bindningar (förhandsversion) | Microsoft Docs"
description: Med den externa filen bindningar i Azure Functions
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 583d9c0b871dc68a79614749ba6ac6711fa820fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a><span data-ttu-id="ff391-103">Azure Functions extern fil Bindningar (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="ff391-103">Azure Functions External File bindings (Preview)</span></span>
<span data-ttu-id="ff391-104">Den här artikeln visar hur toomanipulate filer från olika SaaS providers (t.ex. OneDrive, Dropbox) i din funktion använder inbyggda bindningar.</span><span class="sxs-lookup"><span data-stu-id="ff391-104">This article shows how toomanipulate files from different SaaS providers (e.g. OneDrive, Dropbox) within your function utilizing built-in bindings.</span></span> <span data-ttu-id="ff391-105">Azure functions stöder utlösa indata och utdata bindningar för extern fil.</span><span class="sxs-lookup"><span data-stu-id="ff391-105">Azure functions supports trigger, input, and output bindings for external file.</span></span>

<span data-ttu-id="ff391-106">Den här bindningen och skapar API-anslutningar tooSaaS leverantörer eller använder befintliga API-anslutningar från appen funktionen resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ff391-106">This binding creates API connections tooSaaS providers, or uses existing API connections from your Function App's resource group.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a><span data-ttu-id="ff391-107">Stöds filen anslutningar</span><span class="sxs-lookup"><span data-stu-id="ff391-107">Supported File connections</span></span>

|<span data-ttu-id="ff391-108">koppling</span><span class="sxs-lookup"><span data-stu-id="ff391-108">Connector</span></span>|<span data-ttu-id="ff391-109">Utlösare</span><span class="sxs-lookup"><span data-stu-id="ff391-109">Trigger</span></span>|<span data-ttu-id="ff391-110">Indata</span><span class="sxs-lookup"><span data-stu-id="ff391-110">Input</span></span>|<span data-ttu-id="ff391-111">Resultat</span><span class="sxs-lookup"><span data-stu-id="ff391-111">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="ff391-112">Rutan</span><span class="sxs-lookup"><span data-stu-id="ff391-112">Box</span></span>](https://www.box.com)|<span data-ttu-id="ff391-113">x</span><span class="sxs-lookup"><span data-stu-id="ff391-113">x</span></span>|<span data-ttu-id="ff391-114">x</span><span class="sxs-lookup"><span data-stu-id="ff391-114">x</span></span>|<span data-ttu-id="ff391-115">x</span><span class="sxs-lookup"><span data-stu-id="ff391-115">x</span></span>
|[<span data-ttu-id="ff391-116">Dropbox</span><span class="sxs-lookup"><span data-stu-id="ff391-116">Dropbox</span></span>](https://www.dropbox.com)|<span data-ttu-id="ff391-117">x</span><span class="sxs-lookup"><span data-stu-id="ff391-117">x</span></span>|<span data-ttu-id="ff391-118">x</span><span class="sxs-lookup"><span data-stu-id="ff391-118">x</span></span>|<span data-ttu-id="ff391-119">x</span><span class="sxs-lookup"><span data-stu-id="ff391-119">x</span></span>
|[<span data-ttu-id="ff391-120">FTP</span><span class="sxs-lookup"><span data-stu-id="ff391-120">FTP</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|<span data-ttu-id="ff391-121">x</span><span class="sxs-lookup"><span data-stu-id="ff391-121">x</span></span>|<span data-ttu-id="ff391-122">x</span><span class="sxs-lookup"><span data-stu-id="ff391-122">x</span></span>|<span data-ttu-id="ff391-123">x</span><span class="sxs-lookup"><span data-stu-id="ff391-123">x</span></span>
|[<span data-ttu-id="ff391-124">OneDrive</span><span class="sxs-lookup"><span data-stu-id="ff391-124">OneDrive</span></span>](https://onedrive.live.com)|<span data-ttu-id="ff391-125">x</span><span class="sxs-lookup"><span data-stu-id="ff391-125">x</span></span>|<span data-ttu-id="ff391-126">x</span><span class="sxs-lookup"><span data-stu-id="ff391-126">x</span></span>|<span data-ttu-id="ff391-127">x</span><span class="sxs-lookup"><span data-stu-id="ff391-127">x</span></span>
|[<span data-ttu-id="ff391-128">OneDrive för företag</span><span class="sxs-lookup"><span data-stu-id="ff391-128">OneDrive for Business</span></span>](https://onedrive.live.com/about/business/)|<span data-ttu-id="ff391-129">x</span><span class="sxs-lookup"><span data-stu-id="ff391-129">x</span></span>|<span data-ttu-id="ff391-130">x</span><span class="sxs-lookup"><span data-stu-id="ff391-130">x</span></span>|<span data-ttu-id="ff391-131">x</span><span class="sxs-lookup"><span data-stu-id="ff391-131">x</span></span>
|[<span data-ttu-id="ff391-132">SFTP</span><span class="sxs-lookup"><span data-stu-id="ff391-132">SFTP</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|<span data-ttu-id="ff391-133">x</span><span class="sxs-lookup"><span data-stu-id="ff391-133">x</span></span>|<span data-ttu-id="ff391-134">x</span><span class="sxs-lookup"><span data-stu-id="ff391-134">x</span></span>|<span data-ttu-id="ff391-135">x</span><span class="sxs-lookup"><span data-stu-id="ff391-135">x</span></span>
|[<span data-ttu-id="ff391-136">Google-enhet</span><span class="sxs-lookup"><span data-stu-id="ff391-136">Google Drive</span></span>](https://www.google.com/drive/)||<span data-ttu-id="ff391-137">x</span><span class="sxs-lookup"><span data-stu-id="ff391-137">x</span></span>|<span data-ttu-id="ff391-138">x</span><span class="sxs-lookup"><span data-stu-id="ff391-138">x</span></span>|

> [!NOTE]
> <span data-ttu-id="ff391-139">Den externa filen anslutningar kan också användas i [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="ff391-139">External File connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

## <a name="external-file-trigger-binding"></a><span data-ttu-id="ff391-140">Extern fil utlöser bindning</span><span class="sxs-lookup"><span data-stu-id="ff391-140">External File trigger binding</span></span>

<span data-ttu-id="ff391-141">hello Azure extern fil utlösare kan du övervaka en fjärransluten mapp och köra Funktionskoden när ändringar identifieras.</span><span class="sxs-lookup"><span data-stu-id="ff391-141">hello Azure external file trigger lets you monitor a remote folder and run your function code when changes are detected.</span></span>

<span data-ttu-id="ff391-142">hello extern fil utlösaren använder hello följande JSON-objekt i hello `bindings` matris med function.json</span><span class="sxs-lookup"><span data-stu-id="ff391-142">hello external file trigger uses hello following JSON objects in hello `bindings` array of function.json</span></span>

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder toomonitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of hello following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a><span data-ttu-id="ff391-143">Namnet mönster</span><span class="sxs-lookup"><span data-stu-id="ff391-143">Name patterns</span></span>
<span data-ttu-id="ff391-144">Du kan ange ett filnamnsmönster i hello `path` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ff391-144">You can specify a file name pattern in hello `path` property.</span></span> <span data-ttu-id="ff391-145">hello mappen refererar till måste finnas i hello SaaS-providern.</span><span class="sxs-lookup"><span data-stu-id="ff391-145">hello folder referenced must exist in hello SaaS provider.</span></span>
<span data-ttu-id="ff391-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ff391-146">Examples:</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="ff391-147">Den här sökvägen hittade en fil med namnet *ursprungliga File1.txt* i hello *inkommande* mapp och hello värdet för hello `name` variabel i Funktionskoden skulle vara `File1.txt`.</span><span class="sxs-lookup"><span data-stu-id="ff391-147">This path would find a file named *original-File1.txt* in hello *input* folder, and hello value of hello `name` variable in function code would be `File1.txt`.</span></span>

<span data-ttu-id="ff391-148">Ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="ff391-148">Another example:</span></span>

```json
"path": "input/{filename}.{fileextension}",
```

<span data-ttu-id="ff391-149">Den här sökvägen kan också söka efter en fil med namnet *ursprungliga File1.txt*, och hello värdet för hello `filename` och `fileextension` variabler i Funktionskoden skulle vara *ursprungliga File1* och  *txt*.</span><span class="sxs-lookup"><span data-stu-id="ff391-149">This path would also find a file named *original-File1.txt*, and hello value of hello `filename` and `fileextension` variables in function code would be *original-File1* and *txt*.</span></span>

<span data-ttu-id="ff391-150">Du kan begränsa hello filtypen för filer med hjälp av ett fast värde för hello filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="ff391-150">You can restrict hello file type of files by using a fixed value for hello file extension.</span></span> <span data-ttu-id="ff391-151">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ff391-151">For example:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="ff391-152">I det här fallet bara *.png* filer i hello *exempel* mappen utlösare hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ff391-152">In this case, only *.png* files in hello *samples* folder trigger hello function.</span></span>

<span data-ttu-id="ff391-153">Klammerparenteser är specialtecken i namnet mönster.</span><span class="sxs-lookup"><span data-stu-id="ff391-153">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="ff391-154">toospecify filnamn som innehåller klammerparenteser i hello name, double hello klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="ff391-154">toospecify file names that have curly braces in hello name, double hello curly braces.</span></span>
<span data-ttu-id="ff391-155">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ff391-155">For example:</span></span>

```json
"path": "images/{{20140101}}-{name}",
```

<span data-ttu-id="ff391-156">Den här sökvägen hittade en fil med namnet *{20140101}-soundfile.mp3* i hello *bilder* mappen och hello `name` variabelvärdet i hello Funktionskoden skulle vara *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="ff391-156">This path would find a file named *{20140101}-soundfile.mp3* in hello *images* folder, and hello `name` variable value in hello function code would be *soundfile.mp3*.</span></span>

<a name="receipts"></a>

<!--- ### File receipts
hello Azure Functions runtime makes sure that no external file trigger function gets called more than once for hello same new or updated file.
It does so by maintaining *file receipts* toodetermine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in hello Azure storage account for your function app
(specified by hello `AzureWebJobsStorage` app setting). A file receipt has hello following information:

* hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* hello folder name
* hello file type ("BlockFile" or "PageFile")
* hello file name
* hello ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

tooforce reprocessing of a file, delete hello file receipt for that file from hello *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a><span data-ttu-id="ff391-157">Hantering av skadligt filer</span><span class="sxs-lookup"><span data-stu-id="ff391-157">Handling poison files</span></span>
<span data-ttu-id="ff391-158">När en extern fil Utlösarfunktion misslyckas, försöker Azure Functions funktionen in too5 gånger som standard (inklusive hello första försöket) för en viss fil.</span><span class="sxs-lookup"><span data-stu-id="ff391-158">When an external file trigger function fails, Azure Functions retries that function up too5 times by default (including hello first try) for a given file.</span></span>
<span data-ttu-id="ff391-159">Om alla 5 försök misslyckas funktioner lägger till en tooa lagring meddelandekö med namnet *webjobs-apihubtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="ff391-159">If all 5 tries fail, Functions adds a message tooa Storage queue named *webjobs-apihubtrigger-poison*.</span></span> <span data-ttu-id="ff391-160">hälsningsmeddelande i kö för skadligt filer är en JSON-objekt som innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ff391-160">hello queue message for poison files is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="ff391-161">FunctionId (hello format  *&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*)</span><span class="sxs-lookup"><span data-stu-id="ff391-161">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="ff391-162">Filtyp</span><span class="sxs-lookup"><span data-stu-id="ff391-162">FileType</span></span>
* <span data-ttu-id="ff391-163">Mappnamn</span><span class="sxs-lookup"><span data-stu-id="ff391-163">FolderName</span></span>
* <span data-ttu-id="ff391-164">Filnamn</span><span class="sxs-lookup"><span data-stu-id="ff391-164">FileName</span></span>
* <span data-ttu-id="ff391-165">ETag (en fil versions-ID, till exempel: ”0x8D1DC6E70A277EF”)</span><span class="sxs-lookup"><span data-stu-id="ff391-165">ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")</span></span>


<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="ff391-166">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="ff391-166">Trigger usage</span></span>
<span data-ttu-id="ff391-167">I C#-funktioner, binda toohello indatafilen data med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="ff391-167">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="ff391-168">Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i den [utlösa JSON](#trigger).</span><span class="sxs-lookup"><span data-stu-id="ff391-168">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [trigger JSON](#trigger).</span></span> <span data-ttu-id="ff391-169">I Node.js-funktion, du åtkomst till hello indatafilen data med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="ff391-169">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="ff391-170">hello-filen kan avserialiseras till någon av följande typer hello:</span><span class="sxs-lookup"><span data-stu-id="ff391-170">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="ff391-171">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserad fildata.</span><span class="sxs-lookup"><span data-stu-id="ff391-171">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="ff391-172">Om du deklarerar en anpassad Indatatyp (t.ex. `FooType`), Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.</span><span class="sxs-lookup"><span data-stu-id="ff391-172">If you declare a custom input type (e.g. `FooType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="ff391-173">String - användbart för data från text.</span><span class="sxs-lookup"><span data-stu-id="ff391-173">String - useful for text file data.</span></span>

<span data-ttu-id="ff391-174">Du kan också binda tooany av hello följande typer i C#-funktioner, och hello Functions-runtime försöker att deserialisera hello filen data med hjälp av den typen:</span><span class="sxs-lookup"><span data-stu-id="ff391-174">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a><span data-ttu-id="ff391-175">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="ff391-175">Trigger sample</span></span>
<span data-ttu-id="ff391-176">Anta att du har följande function.json hello definierar som en extern fil-utlösare:</span><span class="sxs-lookup"><span data-stu-id="ff391-176">Suppose you have hello following function.json, that defines an external file trigger:</span></span>

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

<span data-ttu-id="ff391-177">Se hello språkspecifika exempel loggar hello innehållet i varje fil som har lagts till toohello övervakad mapp.</span><span class="sxs-lookup"><span data-stu-id="ff391-177">See hello language-specific sample that logs hello contents of each file that is added toohello monitored folder.</span></span>

* [<span data-ttu-id="ff391-178">C#</span><span class="sxs-lookup"><span data-stu-id="ff391-178">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="ff391-179">Node.js</span><span class="sxs-lookup"><span data-stu-id="ff391-179">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a><span data-ttu-id="ff391-180">Användning av utlösare i C#</span><span class="sxs-lookup"><span data-stu-id="ff391-180">Trigger usage in C#</span></span> #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a><span data-ttu-id="ff391-181">Användning av utlösare i Node.js</span><span class="sxs-lookup"><span data-stu-id="ff391-181">Trigger usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a><span data-ttu-id="ff391-182">Den externa filen inkommande bindningen</span><span class="sxs-lookup"><span data-stu-id="ff391-182">External File input binding</span></span>
<span data-ttu-id="ff391-183">hello Azure extern fil indatabindning kan du toouse en fil från en extern mapp i din funktion.</span><span class="sxs-lookup"><span data-stu-id="ff391-183">hello Azure external file input binding enables you toouse a file from an external folder in your function.</span></span>

<span data-ttu-id="ff391-184">hello extern fil inkommande tooa använder funktionen hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="ff391-184">hello external file input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

<span data-ttu-id="ff391-185">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="ff391-185">Note hello following:</span></span>

* <span data-ttu-id="ff391-186">`path`måste innehålla hello mappnamn och hello filnamn.</span><span class="sxs-lookup"><span data-stu-id="ff391-186">`path` must contain hello folder name and hello file name.</span></span> <span data-ttu-id="ff391-187">Om du har till exempel en [kö utlösaren](functions-bindings-storage-queue.md) i din funktion kan du använda `"path": "samples-workitems/{queueTrigger}"` toopoint tooa filen i hello `samples-workitems` mapp med ett namn som matchar angivna i utlösaren hälsningsmeddelande hello-filnamnet.</span><span class="sxs-lookup"><span data-stu-id="ff391-187">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="ff391-188">Inkommande användning</span><span class="sxs-lookup"><span data-stu-id="ff391-188">Input usage</span></span>
<span data-ttu-id="ff391-189">I C#-funktioner, binda toohello indatafilen data med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="ff391-189">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="ff391-190">Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i den [inkommande bindningen](#input).</span><span class="sxs-lookup"><span data-stu-id="ff391-190">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [input binding](#input).</span></span> <span data-ttu-id="ff391-191">I Node.js-funktion, du åtkomst till hello indatafilen data med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="ff391-191">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="ff391-192">hello-filen kan avserialiseras till någon av följande typer hello:</span><span class="sxs-lookup"><span data-stu-id="ff391-192">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="ff391-193">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserad fildata.</span><span class="sxs-lookup"><span data-stu-id="ff391-193">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="ff391-194">Om du deklarerar en anpassad Indatatyp (t.ex. `InputType`), Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.</span><span class="sxs-lookup"><span data-stu-id="ff391-194">If you declare a custom input type (e.g. `InputType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="ff391-195">String - användbart för data från text.</span><span class="sxs-lookup"><span data-stu-id="ff391-195">String - useful for text file data.</span></span>

<span data-ttu-id="ff391-196">Du kan också binda tooany av hello följande typer i C#-funktioner, och hello Functions-runtime försöker att deserialisera hello filen data med hjälp av den typen:</span><span class="sxs-lookup"><span data-stu-id="ff391-196">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a><span data-ttu-id="ff391-197">Den externa filen utdatabindning</span><span class="sxs-lookup"><span data-stu-id="ff391-197">External File output binding</span></span>
<span data-ttu-id="ff391-198">hello Azure extern fil utdata bindning kan du toowrite tooan externa-mappen i din funktion.</span><span class="sxs-lookup"><span data-stu-id="ff391-198">hello Azure external file output binding enables you toowrite files tooan external folder in your function.</span></span>

<span data-ttu-id="ff391-199">hello extern fil utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="ff391-199">hello external file output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

<span data-ttu-id="ff391-200">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="ff391-200">Note hello following:</span></span>

* <span data-ttu-id="ff391-201">`path`måste innehålla hello mappnamn och hello filen namnet toowrite till.</span><span class="sxs-lookup"><span data-stu-id="ff391-201">`path` must contain hello folder name and hello file name toowrite to.</span></span> <span data-ttu-id="ff391-202">Om du har till exempel en [kö utlösaren](functions-bindings-storage-queue.md) i din funktion kan du använda `"path": "samples-workitems/{queueTrigger}"` toopoint tooa filen i hello `samples-workitems` mapp med ett namn som matchar angivna i utlösaren hälsningsmeddelande hello-filnamnet.</span><span class="sxs-lookup"><span data-stu-id="ff391-202">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="ff391-203">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="ff391-203">Output usage</span></span>
<span data-ttu-id="ff391-204">I C#-funktioner, binda toohello utdatafilen med hjälp av hello med namnet `out` parameter i en funktionssignaturen som `out <T> <name>`, där `T` är hello datatypen som du vill tooserialize hello data till och `paramName` är hello servernamnet du anges i den [utdatabindning](#output).</span><span class="sxs-lookup"><span data-stu-id="ff391-204">In C# functions, you bind toohello output file by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in the [output binding](#output).</span></span> <span data-ttu-id="ff391-205">I Node.js-funktion, du åtkomst till hello utdata filen med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="ff391-205">In Node.js functions, you access hello output file using `context.bindings.<name>`.</span></span>

<span data-ttu-id="ff391-206">Du kan skriva toohello utdatafilen med hjälp av hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="ff391-206">You can write toohello output file using any of hello following types:</span></span>

* <span data-ttu-id="ff391-207">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialisering.</span><span class="sxs-lookup"><span data-stu-id="ff391-207">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialization.</span></span>
  <span data-ttu-id="ff391-208">Om du deklarerar en anpassad utdatatypen (t.ex. `out OutputType paramName`), Azure Functions försöker tooserialize objektet till JSON.</span><span class="sxs-lookup"><span data-stu-id="ff391-208">If you declare a custom output type (e.g. `out OutputType paramName`), Azure Functions attempts tooserialize object into JSON.</span></span> <span data-ttu-id="ff391-209">Om hello output-parameter är null när hello funktionen avslutas skapas hello Functions-runtime en fil som ett null-objekt.</span><span class="sxs-lookup"><span data-stu-id="ff391-209">If hello output parameter is null when hello function exits, hello Functions runtime creates a file as a null object.</span></span>
* <span data-ttu-id="ff391-210">String - (`out string paramName`) användbart för data från text.</span><span class="sxs-lookup"><span data-stu-id="ff391-210">String - (`out string paramName`) useful for text file data.</span></span> <span data-ttu-id="ff391-211">hello Functions-runtime skapas en fil endast om strängparametern är icke-null när hello funktionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="ff391-211">hello Functions runtime creates a file only if the string parameter is non-null when hello function exits.</span></span>

<span data-ttu-id="ff391-212">Du kan också spara tooany av hello följande typer i C#-funktioner:</span><span class="sxs-lookup"><span data-stu-id="ff391-212">In C# functions you can also output tooany of hello following types:</span></span>

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a><span data-ttu-id="ff391-213">Indata och utdata-exempel</span><span class="sxs-lookup"><span data-stu-id="ff391-213">Input + Output sample</span></span>
<span data-ttu-id="ff391-214">Anta att du har följande function.json hello som definierar en [lagring kö utlösaren](functions-bindings-storage-queue.md)en extern fil indata och utdata för en extern fil:</span><span class="sxs-lookup"><span data-stu-id="ff391-214">Suppose you have hello following function.json, that defines a [Storage queue trigger](functions-bindings-storage-queue.md), an external file input, and an external file output:</span></span>

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="ff391-215">Se hello språkspecifika prov som kopierar hello indatafilen toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="ff391-215">See hello language-specific sample that copies hello input file toohello output file.</span></span>

* [<span data-ttu-id="ff391-216">C#</span><span class="sxs-lookup"><span data-stu-id="ff391-216">C#</span></span>](#incsharp)
* [<span data-ttu-id="ff391-217">Node.js</span><span class="sxs-lookup"><span data-stu-id="ff391-217">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="ff391-218">Användning i C#</span><span class="sxs-lookup"><span data-stu-id="ff391-218">Usage in C#</span></span> #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a><span data-ttu-id="ff391-219">Användning i Node.js</span><span class="sxs-lookup"><span data-stu-id="ff391-219">Usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="ff391-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff391-220">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
