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
# <a name="azure-functions-external-file-bindings-preview"></a>Azure Functions extern fil Bindningar (förhandsgranskning)
Den här artikeln visar hur toomanipulate filer från olika SaaS providers (t.ex. OneDrive, Dropbox) i din funktion använder inbyggda bindningar. Azure functions stöder utlösa indata och utdata bindningar för extern fil.

Den här bindningen och skapar API-anslutningar tooSaaS leverantörer eller använder befintliga API-anslutningar från appen funktionen resursgrupp.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Stöds filen anslutningar

|koppling|Utlösare|Indata|Resultat|
|:-----|:---:|:---:|:---:|
|[Rutan](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive för företag](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google-enhet](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Den externa filen anslutningar kan också användas i [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Extern fil utlöser bindning

hello Azure extern fil utlösare kan du övervaka en fjärransluten mapp och köra Funktionskoden när ändringar identifieras.

hello extern fil utlösaren använder hello följande JSON-objekt i hello `bindings` matris med function.json

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

### <a name="name-patterns"></a>Namnet mönster
Du kan ange ett filnamnsmönster i hello `path` egenskapen. hello mappen refererar till måste finnas i hello SaaS-providern.
Exempel:

```json
"path": "input/original-{name}",
```

Den här sökvägen hittade en fil med namnet *ursprungliga File1.txt* i hello *inkommande* mapp och hello värdet för hello `name` variabel i Funktionskoden skulle vara `File1.txt`.

Ett annat exempel:

```json
"path": "input/{filename}.{fileextension}",
```

Den här sökvägen kan också söka efter en fil med namnet *ursprungliga File1.txt*, och hello värdet för hello `filename` och `fileextension` variabler i Funktionskoden skulle vara *ursprungliga File1* och  *txt*.

Du kan begränsa hello filtypen för filer med hjälp av ett fast värde för hello filnamnstillägg. Exempel:

```json
"path": "samples/{name}.png",
```

I det här fallet bara *.png* filer i hello *exempel* mappen utlösare hello-funktionen.

Klammerparenteser är specialtecken i namnet mönster. toospecify filnamn som innehåller klammerparenteser i hello name, double hello klammerparenteser.
Exempel:

```json
"path": "images/{{20140101}}-{name}",
```

Den här sökvägen hittade en fil med namnet *{20140101}-soundfile.mp3* i hello *bilder* mappen och hello `name` variabelvärdet i hello Funktionskoden skulle vara *soundfile.mp3*.

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

### <a name="handling-poison-files"></a>Hantering av skadligt filer
När en extern fil Utlösarfunktion misslyckas, försöker Azure Functions funktionen in too5 gånger som standard (inklusive hello första försöket) för en viss fil.
Om alla 5 försök misslyckas funktioner lägger till en tooa lagring meddelandekö med namnet *webjobs-apihubtrigger-poison*. hälsningsmeddelande i kö för skadligt filer är en JSON-objekt som innehåller hello följande egenskaper:

* FunctionId (hello format  *&lt;funktionen appnamn >*. Funktioner.  *&lt;funktionsnamn >*)
* Filtyp
* Mappnamn
* Filnamn
* ETag (en fil versions-ID, till exempel: ”0x8D1DC6E70A277EF”)


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Utlösaren användning
I C#-funktioner, binda toohello indatafilen data med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.
Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i den [utlösa JSON](#trigger). I Node.js-funktion, du åtkomst till hello indatafilen data med `context.bindings.<name>`.

hello-filen kan avserialiseras till någon av följande typer hello:

* Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserad fildata.
  Om du deklarerar en anpassad Indatatyp (t.ex. `FooType`), Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.
* String - användbart för data från text.

Du kan också binda tooany av hello följande typer i C#-funktioner, och hello Functions-runtime försöker att deserialisera hello filen data med hjälp av den typen:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har följande function.json hello definierar som en extern fil-utlösare:

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

Se hello språkspecifika exempel loggar hello innehållet i varje fil som har lagts till toohello övervakad mapp.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Användning av utlösare i C# #

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

### <a name="trigger-usage-in-nodejs"></a>Användning av utlösare i Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Den externa filen inkommande bindningen
hello Azure extern fil indatabindning kan du toouse en fil från en extern mapp i din funktion.

hello extern fil inkommande tooa använder funktionen hello följande JSON-objekt i hello `bindings` matris med function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Observera följande hello:

* `path`måste innehålla hello mappnamn och hello filnamn. Om du har till exempel en [kö utlösaren](functions-bindings-storage-queue.md) i din funktion kan du använda `"path": "samples-workitems/{queueTrigger}"` toopoint tooa filen i hello `samples-workitems` mapp med ett namn som matchar angivna i utlösaren hälsningsmeddelande hello-filnamnet.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Inkommande användning
I C#-funktioner, binda toohello indatafilen data med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.
Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i den [inkommande bindningen](#input). I Node.js-funktion, du åtkomst till hello indatafilen data med `context.bindings.<name>`.

hello-filen kan avserialiseras till någon av följande typer hello:

* Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserad fildata.
  Om du deklarerar en anpassad Indatatyp (t.ex. `InputType`), Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.
* String - användbart för data från text.

Du kan också binda tooany av hello följande typer i C#-funktioner, och hello Functions-runtime försöker att deserialisera hello filen data med hjälp av den typen:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Den externa filen utdatabindning
hello Azure extern fil utdata bindning kan du toowrite tooan externa-mappen i din funktion.

hello extern fil utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Observera följande hello:

* `path`måste innehålla hello mappnamn och hello filen namnet toowrite till. Om du har till exempel en [kö utlösaren](functions-bindings-storage-queue.md) i din funktion kan du använda `"path": "samples-workitems/{queueTrigger}"` toopoint tooa filen i hello `samples-workitems` mapp med ett namn som matchar angivna i utlösaren hälsningsmeddelande hello-filnamnet.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Användning av utdata
I C#-funktioner, binda toohello utdatafilen med hjälp av hello med namnet `out` parameter i en funktionssignaturen som `out <T> <name>`, där `T` är hello datatypen som du vill tooserialize hello data till och `paramName` är hello servernamnet du anges i den [utdatabindning](#output). I Node.js-funktion, du åtkomst till hello utdata filen med `context.bindings.<name>`.

Du kan skriva toohello utdatafilen med hjälp av hello följande typer:

* Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialisering.
  Om du deklarerar en anpassad utdatatypen (t.ex. `out OutputType paramName`), Azure Functions försöker tooserialize objektet till JSON. Om hello output-parameter är null när hello funktionen avslutas skapas hello Functions-runtime en fil som ett null-objekt.
* String - (`out string paramName`) användbart för data från text. hello Functions-runtime skapas en fil endast om strängparametern är icke-null när hello funktionen avslutas.

Du kan också spara tooany av hello följande typer i C#-funktioner:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Indata och utdata-exempel
Anta att du har följande function.json hello som definierar en [lagring kö utlösaren](functions-bindings-storage-queue.md)en extern fil indata och utdata för en extern fil:

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

Se hello språkspecifika prov som kopierar hello indatafilen toohello utdatafilen.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Användning i C# #

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

### <a name="usage-in-nodejs"></a>Användning i Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
