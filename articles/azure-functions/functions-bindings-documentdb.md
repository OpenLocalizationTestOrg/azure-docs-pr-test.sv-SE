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
# <a name="azure-functions-cosmos-db-bindings"></a><span data-ttu-id="9e55b-104">Azure Functions Cosmos DB bindningar</span><span class="sxs-lookup"><span data-stu-id="9e55b-104">Azure Functions Cosmos DB bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="9e55b-105">Den här artikeln förklarar hur tooconfigure och kod Azure DB som Cosmos-bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9e55b-105">This article explains how tooconfigure and code Azure Cosmos DB bindings in Azure Functions.</span></span> <span data-ttu-id="9e55b-106">Azure Functions stöder indata och utdata bindningar för Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9e55b-106">Azure Functions supports input and output bindings for Cosmos DB.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="9e55b-107">Mer information om Cosmos DB finns [introduktion tooCosmos DB](../documentdb/documentdb-introduction.md) och [skapa ett konsolprogram Cosmos DB](../documentdb/documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9e55b-107">For more information on Cosmos DB, see [Introduction tooCosmos DB](../documentdb/documentdb-introduction.md) and [Build a Cosmos DB console application](../documentdb/documentdb-get-started.md).</span></span>

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a><span data-ttu-id="9e55b-108">DocumentDB-API-indatabindning</span><span class="sxs-lookup"><span data-stu-id="9e55b-108">DocumentDB API input binding</span></span>
<span data-ttu-id="9e55b-109">Hej DocumentDB API-indatabindning hämtar ett Cosmos-DB-dokument och skickar den toohello med namnet Indataparametern för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="9e55b-109">hello DocumentDB API input binding retrieves a Cosmos DB document and passes it toohello named input parameter of hello function.</span></span> <span data-ttu-id="9e55b-110">hello dokumentet ID kan fastställas utifrån hello utlösare som anropar hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="9e55b-110">hello document ID can be determined based on hello trigger that invokes hello function.</span></span> 

<span data-ttu-id="9e55b-111">Hej DocumentDB API-indatabindning har följande egenskaper i hello *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9e55b-111">hello DocumentDB API input binding has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="9e55b-112">`name`: Identifierarnamn används i Funktionskoden för hello dokumentet</span><span class="sxs-lookup"><span data-stu-id="9e55b-112">`name` : Identifier name used in function code for hello document</span></span>
- <span data-ttu-id="9e55b-113">`type`: måste anges för ”documentdb”</span><span class="sxs-lookup"><span data-stu-id="9e55b-113">`type` : must be set too"documentdb"</span></span>
- <span data-ttu-id="9e55b-114">`databaseName`: hello-databas som innehåller hello dokumentet</span><span class="sxs-lookup"><span data-stu-id="9e55b-114">`databaseName` : hello database containing hello document</span></span>
- <span data-ttu-id="9e55b-115">`collectionName`: hello samling som innehåller hello dokumentet</span><span class="sxs-lookup"><span data-stu-id="9e55b-115">`collectionName` : hello collection containing hello document</span></span>
- <span data-ttu-id="9e55b-116">`id`: hello-Id för hello dokumentet tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9e55b-116">`id` : hello Id of hello document tooretrieve.</span></span> <span data-ttu-id="9e55b-117">Den här egenskapen stöder bindningar parametrar. Se [binda toocustom inkommande egenskaper i ett uttryck för bindning](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) i hello artikeln [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="9e55b-117">This property supports bindings parameters; see [Bind toocustom input properties in a binding expression](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) in hello article [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>
- <span data-ttu-id="9e55b-118">`sqlQuery`: En Cosmos-Databasens SQL-fråga som används för att hämta flera dokument.</span><span class="sxs-lookup"><span data-stu-id="9e55b-118">`sqlQuery` : A Cosmos DB SQL query used for retrieving multiple documents.</span></span> <span data-ttu-id="9e55b-119">hello query stöder runtime-bindningar.</span><span class="sxs-lookup"><span data-stu-id="9e55b-119">hello query supports runtime bindings.</span></span> <span data-ttu-id="9e55b-120">Exempel: `SELECT * FROM c where c.departmentId = {departmentId}`</span><span class="sxs-lookup"><span data-stu-id="9e55b-120">For example: `SELECT * FROM c where c.departmentId = {departmentId}`</span></span>
- <span data-ttu-id="9e55b-121">`connection`: hello namnet på hello appinställning med anslutningssträngen Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9e55b-121">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="9e55b-122">`direction`: måste anges för`"in"`.</span><span class="sxs-lookup"><span data-stu-id="9e55b-122">`direction`  : must be set too`"in"`.</span></span>

<span data-ttu-id="9e55b-123">Hej egenskaper `id` och `sqlQuery` kan inte båda anges.</span><span class="sxs-lookup"><span data-stu-id="9e55b-123">hello properties `id` and `sqlQuery` cannot both be specified.</span></span> <span data-ttu-id="9e55b-124">Om varken `id` eller `sqlQuery` anges hello hela samlingen hämtas.</span><span class="sxs-lookup"><span data-stu-id="9e55b-124">If neither `id` nor `sqlQuery` is set, hello entire collection is retrieved.</span></span>

## <a name="using-a-documentdb-api-input-binding"></a><span data-ttu-id="9e55b-125">Med hjälp av en DocumentDB-API-indatabindning</span><span class="sxs-lookup"><span data-stu-id="9e55b-125">Using a DocumentDB API input binding</span></span>

* <span data-ttu-id="9e55b-126">I C# och F # funktioner, när hello funktionen avslutas, sparas ändringar som görs toohello Indatadokumentet via namngivna indataparametrar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9e55b-126">In C# and F# functions, when hello function exits successfully, any changes made toohello input document via named input parameters are automatically persisted.</span></span> 
* <span data-ttu-id="9e55b-127">I JavaScript-funktioner görs uppdateringar inte automatiskt vid utloggning av funktionen.</span><span class="sxs-lookup"><span data-stu-id="9e55b-127">In JavaScript functions, updates are not made automatically upon function exit.</span></span> <span data-ttu-id="9e55b-128">Använd i stället `context.bindings.<documentName>In` och `context.bindings.<documentName>Out` toomake uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="9e55b-128">Instead, use `context.bindings.<documentName>In` and `context.bindings.<documentName>Out` toomake updates.</span></span> <span data-ttu-id="9e55b-129">Se hello [JavaScript exempel](#injavascript).</span><span class="sxs-lookup"><span data-stu-id="9e55b-129">See hello [JavaScript sample](#injavascript).</span></span>

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a><span data-ttu-id="9e55b-130">Inkommande prov för enskilt dokument</span><span class="sxs-lookup"><span data-stu-id="9e55b-130">Input sample for single document</span></span>
<span data-ttu-id="9e55b-131">Anta att du har följande hello DocumentDB API indata-bindning i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9e55b-131">Suppose you have hello following DocumentDB API input binding in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="9e55b-132">Se hello språkspecifika exempel som använder indatabindning tooupdate hello dokumentets textvärde.</span><span class="sxs-lookup"><span data-stu-id="9e55b-132">See hello language-specific sample that uses this input binding tooupdate hello document's text value.</span></span>

* [<span data-ttu-id="9e55b-133">C#</span><span class="sxs-lookup"><span data-stu-id="9e55b-133">C#</span></span>](#incsharp)
* [<span data-ttu-id="9e55b-134">F#</span><span class="sxs-lookup"><span data-stu-id="9e55b-134">F#</span></span>](#infsharp)
* [<span data-ttu-id="9e55b-135">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e55b-135">JavaScript</span></span>](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a><span data-ttu-id="9e55b-136">Inkommande exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="9e55b-136">Input sample in C#</span></span> #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a><span data-ttu-id="9e55b-137">Inkommande exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="9e55b-137">Input sample in F#</span></span> #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

<span data-ttu-id="9e55b-138">Det här exemplet kräver en `project.json` -fil som anger hello `FSharp.Interop.Dynamic` och `Dynamitey` NuGet beroenden:</span><span class="sxs-lookup"><span data-stu-id="9e55b-138">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

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

<span data-ttu-id="9e55b-139">tooadd en `project.json` fil, se [F # paketet management](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="9e55b-139">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a><span data-ttu-id="9e55b-140">Inkommande exemplet i JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e55b-140">Input sample in JavaScript</span></span>

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a><span data-ttu-id="9e55b-141">Inkommande exemplet med flera dokument</span><span class="sxs-lookup"><span data-stu-id="9e55b-141">Input sample with multiple documents</span></span>

<span data-ttu-id="9e55b-142">Anta att du vill tooretrieve flera dokument som anges av en SQL-fråga med hjälp av en kö utlösaren toocustomize hello Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="9e55b-142">Suppose that you wish tooretrieve multiple documents specified by a SQL query, using a queue trigger toocustomize hello query parameters.</span></span> 

<span data-ttu-id="9e55b-143">I det här exemplet hello kö utlösaren innehåller en parameter `departmentId`. Ett kömeddelande för `{ "departmentId" : "Finance" }` returnerar alla poster för hello ekonomiavdelningen.</span><span class="sxs-lookup"><span data-stu-id="9e55b-143">In this example, hello queue trigger provides a parameter `departmentId`.A queue message of `{ "departmentId" : "Finance" }` would return all records for hello finance department.</span></span> <span data-ttu-id="9e55b-144">Använd följande hello i *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9e55b-144">Use hello following in *function.json*:</span></span>

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

### <a name="input-sample-with-multiple-documents-in-c"></a><span data-ttu-id="9e55b-145">Inkommande exemplet med flera dokument i C#</span><span class="sxs-lookup"><span data-stu-id="9e55b-145">Input sample with multiple documents in C#</span></span>

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

### <a name="input-sample-with-multiple-documents-in-javascript"></a><span data-ttu-id="9e55b-146">Inkommande exemplet med flera dokument i JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e55b-146">Input sample with multiple documents in JavaScript</span></span>

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

## <span data-ttu-id="9e55b-147"><a id="docdboutput"></a>DocumentDB-API-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="9e55b-147"><a id="docdboutput"></a>DocumentDB API output binding</span></span>
<span data-ttu-id="9e55b-148">Hej DocumentDB API utdata bindning kan du skriva en ny databas för dokumentet tooan Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9e55b-148">hello DocumentDB API output binding lets you write a new document tooan Azure Cosmos DB database.</span></span> <span data-ttu-id="9e55b-149">Det har följande egenskaper i hello *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9e55b-149">It has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="9e55b-150">`name`: Identifierare som används i funktionskod för hello nytt dokument</span><span class="sxs-lookup"><span data-stu-id="9e55b-150">`name` : Identifier used in function code for hello new document</span></span>
- <span data-ttu-id="9e55b-151">`type`: måste anges för`"documentdb"`</span><span class="sxs-lookup"><span data-stu-id="9e55b-151">`type` : must be set too`"documentdb"`</span></span>
- <span data-ttu-id="9e55b-152">`databaseName`: hello-databas som innehåller hello samlingen där hello nytt dokument kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="9e55b-152">`databaseName` : hello database containing hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="9e55b-153">`collectionName`: hello samlingen där hello nytt dokument kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="9e55b-153">`collectionName` : hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="9e55b-154">`createIfNotExists`: Ett booleskt värde tooindicate om hello samlingen kommer att skapas om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="9e55b-154">`createIfNotExists` : A boolean value tooindicate whether hello collection will be created if it does not exist.</span></span> <span data-ttu-id="9e55b-155">hello standardvärdet är *FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="9e55b-155">hello default is *false*.</span></span> <span data-ttu-id="9e55b-156">Hej orsak till det här är nya samlingar skapas med reserverat dataflöde, vilket.</span><span class="sxs-lookup"><span data-stu-id="9e55b-156">hello reason for this is new collections are created with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="9e55b-157">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="9e55b-157">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/).</span></span>
- <span data-ttu-id="9e55b-158">`connection`: hello namnet på hello appinställning med anslutningssträngen Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9e55b-158">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="9e55b-159">`direction`: måste anges för`"out"`</span><span class="sxs-lookup"><span data-stu-id="9e55b-159">`direction` : must be set too`"out"`</span></span>

## <a name="using-a-documentdb-api-output-binding"></a><span data-ttu-id="9e55b-160">Med hjälp av ett DocumentDB-API-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="9e55b-160">Using a DocumentDB API output binding</span></span>
<span data-ttu-id="9e55b-161">Det här avsnittet visar hur toouse DocumentDB-API utdata bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="9e55b-161">This section shows you how toouse your DocumentDB API output binding in your function code.</span></span>

<span data-ttu-id="9e55b-162">När du skriver toohello output-parameter i din funktion som standard skapas ett nytt dokument i databasen med ett GUID som genererats automatiskt som hello dokumentera ID.</span><span class="sxs-lookup"><span data-stu-id="9e55b-162">When you write toohello output parameter in your function, by default a new document is generated in your database, with an automatically generated GUID as hello document ID.</span></span> <span data-ttu-id="9e55b-163">Du kan ange hello dokument-ID för utdata dokument genom att ange hello `id` JSON-egenskapen i hello utdataparameter.</span><span class="sxs-lookup"><span data-stu-id="9e55b-163">You can specify hello document ID of output document by specifying hello `id` JSON property in hello output parameter.</span></span> 

>[!Note]  
><span data-ttu-id="9e55b-164">När du anger hello-ID för ett befintligt dokument, hämtar den över hello nya utdata-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9e55b-164">When you specify hello ID of an existing document, it gets overwritten by hello new output document.</span></span> 

<span data-ttu-id="9e55b-165">toooutput flera dokument du kan också binda för`ICollector<T>` eller `IAsyncCollector<T>` där `T` är en av typerna hello stöds.</span><span class="sxs-lookup"><span data-stu-id="9e55b-165">toooutput multiple documents, you can also bind too`ICollector<T>` or `IAsyncCollector<T>` where `T` is one of hello supported types.</span></span>

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a><span data-ttu-id="9e55b-166">DocumentDB-API-utdata bindning exemplet</span><span class="sxs-lookup"><span data-stu-id="9e55b-166">DocumentDB API output binding sample</span></span>
<span data-ttu-id="9e55b-167">Anta att du har följande hello DocumentDB API-utdatabindning i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9e55b-167">Suppose you have hello following DocumentDB API output binding in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="9e55b-168">Och du har en inkommande kö-bindning för en kö som tar emot JSON i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="9e55b-168">And you have a queue input binding for a queue that receives JSON in hello following format:</span></span>

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="9e55b-169">Och du vill toocreate Cosmos DB dokument i följande format för varje post hello:</span><span class="sxs-lookup"><span data-stu-id="9e55b-169">And you want toocreate Cosmos DB documents in hello following format for each record:</span></span>

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="9e55b-170">Se hello språkspecifika exempel som använder den här utdata bindning tooadd dokument tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="9e55b-170">See hello language-specific sample that uses this output binding tooadd documents tooyour database.</span></span>

* [<span data-ttu-id="9e55b-171">C#</span><span class="sxs-lookup"><span data-stu-id="9e55b-171">C#</span></span>](#outcsharp)
* [<span data-ttu-id="9e55b-172">F#</span><span class="sxs-lookup"><span data-stu-id="9e55b-172">F#</span></span>](#outfsharp)
* [<span data-ttu-id="9e55b-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e55b-173">JavaScript</span></span>](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="9e55b-174">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="9e55b-174">Output sample in C#</span></span> #

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

### <a name="output-sample-in-f"></a><span data-ttu-id="9e55b-175">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="9e55b-175">Output sample in F#</span></span> #

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

<span data-ttu-id="9e55b-176">Det här exemplet kräver en `project.json` -fil som anger hello `FSharp.Interop.Dynamic` och `Dynamitey` NuGet beroenden:</span><span class="sxs-lookup"><span data-stu-id="9e55b-176">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

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

<span data-ttu-id="9e55b-177">tooadd en `project.json` fil, se [F # paketet management](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="9e55b-177">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a><span data-ttu-id="9e55b-178">Utdata exemplet i JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e55b-178">Output sample in JavaScript</span></span>

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
