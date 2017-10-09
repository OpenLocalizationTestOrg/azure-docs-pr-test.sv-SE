---
title: aaaAzure funktioner Cosmos DB bindningar | Microsoft Docs
description: "Förstå hur toouse Azure Cosmos DB bindningar i Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/18/2016
ms.author: glenga
ms.openlocfilehash: 76b89e8296db1dd28dff9528903b1f6a28f55232
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-cosmos-db-bindings"></a>Azure Functions Cosmos DB bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och kod Azure DB som Cosmos-bindningar i Azure Functions. Azure Functions stöder indata och utdata bindningar för Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Mer information om Cosmos DB finns [introduktion tooCosmos DB](../documentdb/documentdb-introduction.md) och [skapa ett konsolprogram Cosmos DB](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>DocumentDB-API-indatabindning
Hej DocumentDB API-indatabindning hämtar ett Cosmos-DB-dokument och skickar den toohello med namnet Indataparametern för hello-funktionen. hello dokumentet ID kan fastställas utifrån hello utlösare som anropar hello-funktionen. 

Hej DocumentDB API-indatabindning har följande egenskaper i hello *function.json*:

- `name`: Identifierarnamn används i Funktionskoden för hello dokumentet
- `type`: måste anges för ”documentdb”
- `databaseName`: hello-databas som innehåller hello dokumentet
- `collectionName`: hello samling som innehåller hello dokumentet
- `id`: hello-Id för hello dokumentet tooretrieve. Den här egenskapen stöder bindningar parametrar. Se [binda toocustom inkommande egenskaper i ett uttryck för bindning](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) i hello artikeln [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).
- `sqlQuery`: En Cosmos-Databasens SQL-fråga som används för att hämta flera dokument. hello query stöder runtime-bindningar. Exempel: `SELECT * FROM c where c.departmentId = {departmentId}`
- `connection`: hello namnet på hello appinställning med anslutningssträngen Cosmos DB
- `direction`: måste anges för`"in"`.

Hej egenskaper `id` och `sqlQuery` kan inte båda anges. Om varken `id` eller `sqlQuery` anges hello hela samlingen hämtas.

## <a name="using-a-documentdb-api-input-binding"></a>Med hjälp av en DocumentDB-API-indatabindning

* I C# och F # funktioner, när hello funktionen avslutas, sparas ändringar som görs toohello Indatadokumentet via namngivna indataparametrar automatiskt. 
* I JavaScript-funktioner görs uppdateringar inte automatiskt vid utloggning av funktionen. Använd i stället `context.bindings.<documentName>In` och `context.bindings.<documentName>Out` toomake uppdateringar. Se hello [JavaScript exempel](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Inkommande prov för enskilt dokument
Anta att du har följande hello DocumentDB API indata-bindning i hello `bindings` matris med function.json:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Se hello språkspecifika exempel som använder indatabindning tooupdate hello dokumentets textvärde.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Inkommande exemplet i C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Inkommande exemplet i F # #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Det här exemplet kräver en `project.json` -fil som anger hello `FSharp.Interop.Dynamic` och `Dynamitey` NuGet beroenden:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

tooadd en `project.json` fil, se [F # paketet management](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Inkommande exemplet i JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Inkommande exemplet med flera dokument

Anta att du vill tooretrieve flera dokument som anges av en SQL-fråga med hjälp av en kö utlösaren toocustomize hello Frågeparametrar. 

I det här exemplet hello kö utlösaren innehåller en parameter `departmentId`. Ett kömeddelande för `{ "departmentId" : "Finance" }` returnerar alla poster för hello ekonomiavdelningen. Använd följande hello i *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Inkommande exemplet med flera dokument i C#

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Inkommande exemplet med flera dokument i JavaScript

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a id="docdboutput"></a>DocumentDB-API-utdatabindning
Hej DocumentDB API utdata bindning kan du skriva en ny databas för dokumentet tooan Azure Cosmos DB. Det har följande egenskaper i hello *function.json*:

- `name`: Identifierare som används i funktionskod för hello nytt dokument
- `type`: måste anges för`"documentdb"`
- `databaseName`: hello-databas som innehåller hello samlingen där hello nytt dokument kommer att skapas.
- `collectionName`: hello samlingen där hello nytt dokument kommer att skapas.
- `createIfNotExists`: Ett booleskt värde tooindicate om hello samlingen kommer att skapas om den inte finns. hello standardvärdet är *FALSKT*. Hej orsak till det här är nya samlingar skapas med reserverat dataflöde, vilket. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: hello namnet på hello appinställning med anslutningssträngen Cosmos DB
- `direction`: måste anges för`"out"`

## <a name="using-a-documentdb-api-output-binding"></a>Med hjälp av ett DocumentDB-API-utdatabindning
Det här avsnittet visar hur toouse DocumentDB-API utdata bindningen i din funktionskoden.

När du skriver toohello output-parameter i din funktion som standard skapas ett nytt dokument i databasen med ett GUID som genererats automatiskt som hello dokumentera ID. Du kan ange hello dokument-ID för utdata dokument genom att ange hello `id` JSON-egenskapen i hello utdataparameter. 

>[!Note]  
>När du anger hello-ID för ett befintligt dokument, hämtar den över hello nya utdata-dokumentet. 

toooutput flera dokument du kan också binda för`ICollector<T>` eller `IAsyncCollector<T>` där `T` är en av typerna hello stöds.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>DocumentDB-API-utdata bindning exemplet
Anta att du har följande hello DocumentDB API-utdatabindning i hello `bindings` matris med function.json:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

Och du har en inkommande kö-bindning för en kö som tar emot JSON i hello följande format:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Och du vill toocreate Cosmos DB dokument i följande format för varje post hello:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Se hello språkspecifika exempel som använder den här utdata bindning tooadd dokument tooyour databasen.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Utdata exemplet i C# #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Utdata exemplet i F # #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Det här exemplet kräver en `project.json` -fil som anger hello `FSharp.Interop.Dynamic` och `Dynamitey` NuGet beroenden:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

tooadd en `project.json` fil, se [F # paketet management](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Utdata exemplet i JavaScript

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```
