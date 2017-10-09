---
title: "aaaWork med utlösare och bindningar i Azure Functions | Microsoft Docs"
description: "Lär dig hur toouse utlösare och bindningar i Azure Functions tooconnect din kod körning tooonline händelser och molnbaserade tjänster."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: donnam
ms.openlocfilehash: eb2ebfca172fcc8c0f479adbcfec99e90fc33615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-utlösare och bindningar begrepp
Azure Functions kan du toowrite koden i svaret tooevents i Azure och andra tjänster via *utlösare* och *bindningar*. Den här artikeln innehåller en översikt av utlösare och bindningar för alla programmeringsspråk som stöds. Funktioner som är vanliga tooall bindningar beskrivs.

## <a name="overview"></a>Översikt

Utlösare och bindningar är en deklarativ metod toodefine hur en funktion har anropats och vilka data den fungerar med. En *utlösaren* definierar hur en funktion har anropats. En funktion måste ha exakt en utlösare. Utlösare har associerade data, vilket är vanligtvis hello-nyttolast som utlöste hello-funktionen. 

Indata och utdata *bindningar* tillhandahåller en deklarativ metod tooconnect toodata från inom din kod. Liknande tootriggers du ange anslutningssträngar och andra egenskaper i konfigurationen av funktionen. Bindningar är valfria och en funktion kan ha flera indata och utdata bindningar. 

Du kan skriva kod som är generisk och har inte hårdkoda hello detaljer om hello-tjänster som samverkar med utlösare och bindningar. Data som kommer från tjänster som bara blir indatavärden för din funktionskoden. toooutput tooanother datatjänst (till exempel skapa en ny rad i Azure Table Storage), Använd hello returvärdet för hello-metoden. Eller, om du behöver toooutput flera värden kan använda en hjälpobjektet. Utlösare och bindningar har en **namn** -egenskap som är en identifierare som du använder i din kod tooaccess hello bindning.

Du kan konfigurera utlösare och bindningar i hello **integrera** fliken i hello Azure Functions-portalen. Under hello omfattar hello UI ändrar en fil med namnet *function.json* fil i hello-funktionen. Du kan redigera den här filen genom att ändra toohello **redigeraren**.

hello följande tabell visar hello utlösare och bindningar som stöds med Azure Functions. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Exempel: kön utlösare och tabellen utdatabindning

Anta att du vill toowrite en ny rad tooAzure Table Storage när ett nytt meddelande visas i Azure Queue Storage. Det här scenariot kan implementeras med hjälp av en Azure-kö utdatabindning utlösare och en tabell. 

En kö utlösare kräver följande information i hello hello **integrera** fliken:

* hello innehåller hello app inställningen som hello konto lagringsanslutningssträng för hello kö
* hello könamnet
* Hej identifierare i din kod tooread hello innehållet i hello kömeddelande som `order`.

toowrite tooAzure Table Storage, använda en output-bindning med hello följande information:

* hello innehåller hello app inställningen som hello konto lagringsanslutningssträng för hello tabellen
* hello tabellnamn
* hello identifierare i din kod toocreate utdata objekt eller hello returvärde från hello-funktionen.

Bindningar använda app-inställningar för anslutning strängar tooenforce hello bästa rutin som *function.json* innehåller inte tjänsten hemligheter.

Använd sedan hello-identifierare som du angav toointegrate med Azure Storage i din kod.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello second parameter toocontext.done is used as hello value for hello new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Här är hello *function.json* som motsvarar toohello föregående kod. Observera att hello samma konfiguration kan användas, oavsett hello språket för hello funktionen implementering.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
tooview och redigera hello innehållet i *function.json* i hello Azure-portalen klickar du på hello **redigeraren** alternativet på hello **integrera** för din funktion.

Mer kodexempel och information om att integrera med Azure Storage finns [Azure Functions-utlösare och bindningar för Azure Storage](functions-bindings-storage.md).

### <a name="binding-direction"></a>Bindningen riktning

Alla utlösare och bindningar har en `direction` egenskapen:

- För utlösare är hello riktningen alltid`in`
- Inkommande och utgående bindningar använda `in` och`out`
- Vissa bindningar stöd för en särskild riktning `inout`. Om du använder `inout`, endast hello **redigeraren** är tillgänglig i hello **integrera** fliken.

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a>Med hjälp av hello funktionen returtyp tooreturn ett enda utflöde

hello föregående exempel visar hur toouse hello funktionen returvärdet tooprovide utdatabindning tooa, som uppnås med hjälp av hello särskilda name-parametern `$return`. (Detta stöds endast på språk som har ett returvärde, till exempel C#, JavaScript och F #.) Om en funktion har flera bindningar för utdata använder `$return` för endast en av hello utdata bindningar. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

hello exemplen nedan visar tillbaka hur typer som används med utdata bindningar i C#, JavaScript och F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```javascript
// JavaScript: return a value in hello second parameter toocontext.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>DataType-egenskapen för bindning

I .NET, använda hello typer toodefine hello datatyp för indata. Till exempel använda `string` toobind toohello texten i en kö-utlösare och en byte-matris tooread som binary.

För språk som skrivs dynamiskt, till exempel JavaScript, använda hello `dataType` egenskap i hello bindning definition. Till exempel tooread hello innehållet i en HTTP-begäran i binärformat, Använd hello typen `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andra alternativ för `dataType` är `stream` och `string`.

## <a name="resolving-app-settings"></a>Lösa app-inställningar
Som bästa praxis, hemligheter och anslutningssträngar ska hanteras med app-inställningar i stället för konfigurationsfiler. Detta begränsar åtkomst toothese hemligheter och gör den säker toostore *function.json* i en offentlig källkontroll.

Appinställningar är också användbara när du vill toochange configuration utifrån hello-miljö. Du kan exempelvis vilja toomonitor en annan kö eller blob storage-behållare i en testmiljö.

Appinställningar är löst när ett värde som omges av procenttecken, `%MyAppSetting%`. Observera att hello `connection` -egenskapen för utlösare och bindningar är ett specialfall och löser automatiskt värden som app-inställningar. 

hello följande exempel är en kö utlösare som använder en appinställning `%input-queue-name%` toodefine hello kön tootrigger på.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Utlösaren metadataegenskaper

Ange ytterligare metadatavärden i tillägg toohello datanyttolasten som tillhandahålls av en utlösare (till exempel kön hälsningsmeddelande som utlöste en funktion), många utlösare. Dessa värden kan användas som indataparametrar i C# och F # eller egenskaper för hello `context.bindings` objekt i JavaScript. 

Till exempel stöder en kö utlösare hello följande egenskaper:

* QueueTrigger - utlösa meddelandeinnehåll om en giltig sträng
* DequeueCount
* ExpirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

Information om metadataegenskaper för varje utlösare beskrivs i hello motsvarande referensavsnittet. Dokumentation är också tillgänglig i hello **integrera** fliken hello portalen i hello **dokumentationen** avsnittet nedan hello konfigurationsområde för bindningen.  

Till exempel eftersom blob-utlösare har vissa fördröjningar kan du använda en kö utlösaren toorun din funktion (se [Blob Storage utlösaren](functions-bindings-storage-blob.md#storage-blob-trigger). hälsningsmeddelande för kön innehåller hello blob filename tootrigger på. Med hjälp av hello `queueTrigger` metadataegenskapen, du kan ange det här beteendet i konfigurationen, i stället för din kod.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Metadataegenskaper från en utlösare kan också användas i en *bindande uttryck* för en annan bindning som beskrivs i hello efter avsnittet.

## <a name="binding-expressions-and-patterns"></a>Bindande uttryck och mönster

En av hello mest kraftfulla funktioner för utlösare och bindningar är *bindningsuttryck*. Du kan definiera mönster för uttryck som kan användas i andra bindningar eller din kod i din bindning. Utlösaren metadata kan också användas i bindande uttryck, som visas i exemplet hello i hello föregående avsnitt.

Anta att du vill tooresize bilder i viss blob storage-behållare, liknande toohello **storleksändring av** mallen i hello **nya funktionen** sidan. Gå för**nya funktionen** -> språk **C#** -> scenariot **exempel** -> **ImageResizer CSharp**. 

Här är hello *function.json* definition:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Observera att hello `filename` parameter används i både hello blob utlösardefinition samt hello blob-utdatabindning. Den här parametern kan också användas i funktionskod.

```csharp
// C# example of binding too{filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Slumpmässig GUID
Azure Functions erbjuder en bekvämlighet syntax för att skapa GUID i dina bindningar via hello `{rand-guid}` bindande uttryck. hello används följande exempel den här toogenerate ett unikt blob-namn: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Aktuell tid

Du kan använda hello Bindningsuttrycket `DateTime`, vilket löser för`DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a>Binda toocustom inkommande egenskaper i ett uttryck för bindning

Bindande uttryck kan även referera till egenskaper som har definierats i hello utlösaren nyttolast sig själv. Du kan exempelvis vilja toodynamically bind tooa blob storage-fil från ett filnamn som anges i en webhook.

Till exempel hello följande *function.json* använder en egenskap som kallas `BlobName` från hello utlösaren nyttolast:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

tooaccomplish detta i C# och F #, måste du definiera en POCO som definierar hello fält som ska avserialiseras i hello utlösaren nyttolast.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

JSON-deserialisering utförs automatiskt i JavaScript, och du kan använda hello egenskaper direkt.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Konfigurerar bindningsdata vid körning

I C# och andra .NET-språk, du kan använda ett absolut nödvändigt bindning mönster som skillnad från toohello deklarativ bindningar i *function.json*. Tvingande bindning är användbart när bindande parametrar behöver toobe beräknade körning i stället för design samtidigt. Det finns fler toolearn [bindning under körning via tvingande bindningar](functions-reference-csharp.md#imperative-bindings) i hello C#-utvecklare.

## <a name="next-steps"></a>Nästa steg
Mer information om en specifik bindning finns hello följande artiklar:

- [HTTP och webhooks](functions-bindings-http-webhook.md)
- [Timer](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Händelsehubb](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Cosmos DB](functions-bindings-documentdb.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Extern fil](functions-bindings-external-file.md)
