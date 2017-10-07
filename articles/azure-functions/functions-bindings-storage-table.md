---
title: "aaaAzure tabellen för lagring av funktioner bindningar | Microsoft Docs"
description: "Förstå hur toouse Azure Storage bindningar i Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 90c2a73329139d4ab3504bc0e2c90370133158bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-storage-table-bindings"></a><span data-ttu-id="1c29b-104">Azure Functions lagring tabell bindningar</span><span class="sxs-lookup"><span data-stu-id="1c29b-104">Azure Functions Storage table bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="1c29b-105">Den här artikeln förklarar hur tooconfigure och koden Azure Storage tabell bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1c29b-105">This article explains how tooconfigure and code Azure Storage table bindings in Azure Functions.</span></span> <span data-ttu-id="1c29b-106">Azure Functions stöder indata och utdata bindningar för Azure Storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="1c29b-106">Azure Functions supports input and output bindings for Azure Storage tables.</span></span>

<span data-ttu-id="1c29b-107">hello lagring tabell bindningen stöder hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="1c29b-107">hello Storage table binding supports hello following scenarios:</span></span>

* <span data-ttu-id="1c29b-108">**Läs en enskild rad i en C# eller Node.js-funktion** – du kan ställa `partitionKey` och `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-108">**Read a single row in a C# or Node.js function** - Set `partitionKey` and `rowKey`.</span></span> <span data-ttu-id="1c29b-109">Hej `filter` och `take` egenskaper inte används i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="1c29b-109">hello `filter` and `take` properties are not used in this scenario.</span></span>
* <span data-ttu-id="1c29b-110">**Läsa flera rader i en C#-funktion** -hello Functions-runtime ger en `IQueryable<T>` objektet bundet toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="1c29b-110">**Read multiple rows in a C# function** - hello Functions runtime provides an `IQueryable<T>` object bound toohello table.</span></span> <span data-ttu-id="1c29b-111">Typen `T` måste vara härledd från `TableEntity` eller implementerar `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-111">Type `T` must derive from `TableEntity` or implement `ITableEntity`.</span></span> <span data-ttu-id="1c29b-112">Hej `partitionKey`, `rowKey`, `filter`, och `take` egenskaper inte används i det här scenariot; du kan använda hello `IQueryable` objekt toodo filtrering krävs.</span><span class="sxs-lookup"><span data-stu-id="1c29b-112">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario; you can use hello `IQueryable` object toodo any filtering required.</span></span> 
* <span data-ttu-id="1c29b-113">**Läsa flera rader i en nod funktion** – du kan ställa hello `filter` och `take` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c29b-113">**Read multiple rows in a Node function** - Set hello `filter` and `take` properties.</span></span> <span data-ttu-id="1c29b-114">Konfigurerar inte `partitionKey` eller `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-114">Don't set `partitionKey` or `rowKey`.</span></span>
* <span data-ttu-id="1c29b-115">**Skriv en eller flera rader i en C#-funktion** -hello Functions-runtime ger en `ICollector<T>` eller `IAsyncCollector<T>` bundna toohello tabellen där `T` anger hello schemat hello entiteter som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="1c29b-115">**Write one or more rows in a C# function** - hello Functions runtime provides an `ICollector<T>` or `IAsyncCollector<T>` bound toohello table, where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="1c29b-116">Normalt anger `T` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte.</span><span class="sxs-lookup"><span data-stu-id="1c29b-116">Typically, type `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="1c29b-117">Hej `partitionKey`, `rowKey`, `filter`, och `take` egenskaper inte används i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="1c29b-117">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a><span data-ttu-id="1c29b-118">Lagring tabell indatabindning</span><span class="sxs-lookup"><span data-stu-id="1c29b-118">Storage table input binding</span></span>
<span data-ttu-id="1c29b-119">hello Azure Storage tabell indatabindning kan toouse en tabell för lagring i din funktion.</span><span class="sxs-lookup"><span data-stu-id="1c29b-119">hello Azure Storage table input binding enables you toouse a storage table in your function.</span></span> 

<span data-ttu-id="1c29b-120">hello lagringsfunktionen tabell inkommande tooa använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="1c29b-120">hello Storage table input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity tooread - see below>",
    "rowKey": "<RowKey of table entity tooread - see below>",
    "take": "<Maximum number of entities tooread in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="1c29b-121">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="1c29b-121">Note hello following:</span></span> 

* <span data-ttu-id="1c29b-122">Använd `partitionKey` och `rowKey` tillsammans tooread en enda entitet.</span><span class="sxs-lookup"><span data-stu-id="1c29b-122">Use `partitionKey` and `rowKey` together tooread a single entity.</span></span> <span data-ttu-id="1c29b-123">De här egenskaperna är valfria.</span><span class="sxs-lookup"><span data-stu-id="1c29b-123">These properties are optional.</span></span> 
* <span data-ttu-id="1c29b-124">`connection`måste innehålla hello en app-inställning som innehåller en anslutningssträng för lagring.</span><span class="sxs-lookup"><span data-stu-id="1c29b-124">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="1c29b-125">I hello Azure-portalen, hello standardredigeraren i hello **integrera** konfigurerar appinställningen för dig när du skapar en lagringsplats för kontot eller väljer en befintlig.</span><span class="sxs-lookup"><span data-stu-id="1c29b-125">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="1c29b-126">Du kan också [konfigurera den här appen inställningen manuellt](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="1c29b-126">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>  

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="1c29b-127">Inkommande användning</span><span class="sxs-lookup"><span data-stu-id="1c29b-127">Input usage</span></span>
<span data-ttu-id="1c29b-128">I C#-funktioner, du binda toohello inkommande tabell entitet (eller entiteter) med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-128">In C# functions, you bind toohello input table entity (or entities) by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="1c29b-129">Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i hello [inkommande bindningen](#input).</span><span class="sxs-lookup"><span data-stu-id="1c29b-129">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in hello [input binding](#input).</span></span> <span data-ttu-id="1c29b-130">I Node.js-funktion, du åtkomst till hello inkommande tabell entitet (eller entiteter) med hjälp av `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-130">In Node.js functions, you access hello input table entity (or entities) using `context.bindings.<name>`.</span></span>

<span data-ttu-id="1c29b-131">hello indata kan avserialiseras i Node.js- eller C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="1c29b-131">hello input data can be deserialized in Node.js or C# functions.</span></span> <span data-ttu-id="1c29b-132">hello avserialiseras objekt har `RowKey` och `PartitionKey` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1c29b-132">hello deserialized objects have `RowKey` and `PartitionKey` properties.</span></span>

<span data-ttu-id="1c29b-133">Du kan också binda tooany av hello följande typer i C#-funktioner, och hello funktioner runtime försöker för deserialisera hello tabelldata med hjälp av den typen:</span><span class="sxs-lookup"><span data-stu-id="1c29b-133">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime will attempt too deserialize hello table data using that type:</span></span>

* <span data-ttu-id="1c29b-134">Typer som implementerar`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="1c29b-134">Any type that implements `ITableEntity`</span></span>
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="1c29b-135">Indata-exempel</span><span class="sxs-lookup"><span data-stu-id="1c29b-135">Input sample</span></span>
<span data-ttu-id="1c29b-136">Anta att du har följande function.json som använder en kö utlösaren tooread en tabellrad hello.</span><span class="sxs-lookup"><span data-stu-id="1c29b-136">Supposed you have hello following function.json, which uses a queue trigger tooread a single table row.</span></span> <span data-ttu-id="1c29b-137">hello JSON anger `PartitionKey`  
 `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-137">hello JSON specifies `PartitionKey` 
`RowKey`.</span></span> <span data-ttu-id="1c29b-138">`"rowKey": "{queueTrigger}"`Visar hello raden nyckeln kommer från hello kön meddelandet sträng.</span><span class="sxs-lookup"><span data-stu-id="1c29b-138">`"rowKey": "{queueTrigger}"` indicates that hello row key comes from hello queue message string.</span></span>

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="1c29b-139">Se hello språkspecifika prov som läser en enskild tabell entitet.</span><span class="sxs-lookup"><span data-stu-id="1c29b-139">See hello language-specific sample that reads a single table entity.</span></span>

* [<span data-ttu-id="1c29b-140">C#</span><span class="sxs-lookup"><span data-stu-id="1c29b-140">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="1c29b-141">F#</span><span class="sxs-lookup"><span data-stu-id="1c29b-141">F#</span></span>](#inputfsharp)
* [<span data-ttu-id="1c29b-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="1c29b-142">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="1c29b-143">Inkommande exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="1c29b-143">Input sample in C#</span></span> #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a><span data-ttu-id="1c29b-144">Inkommande exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="1c29b-144">Input sample in F#</span></span> #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="1c29b-145">Inkommande exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="1c29b-145">Input sample in Node.js</span></span>
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a><span data-ttu-id="1c29b-146">Lagringstabellen utdatabindning</span><span class="sxs-lookup"><span data-stu-id="1c29b-146">Storage table output binding</span></span>
<span data-ttu-id="1c29b-147">hello Azure Storage tabell utdata bindning kan du toowrite entiteter tooa lagringstabellen i din funktion.</span><span class="sxs-lookup"><span data-stu-id="1c29b-147">hello Azure Storage table output binding enables you toowrite entities tooa Storage table in your function.</span></span> 

<span data-ttu-id="1c29b-148">Hej lagring tabellutdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="1c29b-148">hello Storage table output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity toowrite - see below>",
    "rowKey": "<RowKey of table entity toowrite - see below>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="1c29b-149">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="1c29b-149">Note hello following:</span></span> 

* <span data-ttu-id="1c29b-150">Använd `partitionKey` och `rowKey` tillsammans toowrite en enda entitet.</span><span class="sxs-lookup"><span data-stu-id="1c29b-150">Use `partitionKey` and `rowKey` together toowrite a single entity.</span></span> <span data-ttu-id="1c29b-151">De här egenskaperna är valfria.</span><span class="sxs-lookup"><span data-stu-id="1c29b-151">These properties are optional.</span></span> <span data-ttu-id="1c29b-152">Du kan också ange `PartitionKey` och `RowKey` när du skapar hello entitetsobjekt i funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="1c29b-152">You can also specify `PartitionKey` and `RowKey` when you create hello entity objects in your function code.</span></span>
* <span data-ttu-id="1c29b-153">`connection`måste innehålla hello en app-inställning som innehåller en anslutningssträng för lagring.</span><span class="sxs-lookup"><span data-stu-id="1c29b-153">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="1c29b-154">I hello Azure-portalen, hello standardredigeraren i hello **integrera** konfigurerar appinställningen för dig när du skapar en lagringsplats för kontot eller väljer en befintlig.</span><span class="sxs-lookup"><span data-stu-id="1c29b-154">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="1c29b-155">Du kan också [konfigurera den här appen inställningen manuellt](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="1c29b-155">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span> 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="1c29b-156">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="1c29b-156">Output usage</span></span>
<span data-ttu-id="1c29b-157">I C#-funktioner, binda toohello tabellutdata med hjälp av hello med namnet `out` parameter i en funktionssignaturen som `out <T> <name>`, där `T` är hello datatypen som du vill tooserialize hello data till och `paramName` är hello servernamnet du anges i hello [utdatabindning](#output).</span><span class="sxs-lookup"><span data-stu-id="1c29b-157">In C# functions, you bind toohello table output by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in hello [output binding](#output).</span></span> <span data-ttu-id="1c29b-158">I Node.js-funktion, du åtkomst till hello tabell utdata med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-158">In Node.js functions, you access hello table output using `context.bindings.<name>`.</span></span>

<span data-ttu-id="1c29b-159">Du kan serialisera objekt i Node.js- eller C#-funktioner.</span><span class="sxs-lookup"><span data-stu-id="1c29b-159">You can serialize objects in Node.js or C# functions.</span></span> <span data-ttu-id="1c29b-160">I C#-funktioner, kan du också binda toohello följande typer:</span><span class="sxs-lookup"><span data-stu-id="1c29b-160">In C# functions, you can also bind toohello following types:</span></span>

* <span data-ttu-id="1c29b-161">Typer som implementerar`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="1c29b-161">Any type that implements `ITableEntity`</span></span>
* <span data-ttu-id="1c29b-162">`ICollector<T>`(toooutput flera enheter.</span><span class="sxs-lookup"><span data-stu-id="1c29b-162">`ICollector<T>` (toooutput multiple entities.</span></span> <span data-ttu-id="1c29b-163">Se [exempel](#outcsharp).)</span><span class="sxs-lookup"><span data-stu-id="1c29b-163">See [sample](#outcsharp).)</span></span>
* <span data-ttu-id="1c29b-164">`IAsyncCollector<T>`(asynkrona versionen av `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="1c29b-164">`IAsyncCollector<T>` (async version of `ICollector<T>`)</span></span>
* <span data-ttu-id="1c29b-165">`CloudTable`(med hello Azure Storage SDK: N.</span><span class="sxs-lookup"><span data-stu-id="1c29b-165">`CloudTable` (using hello Azure Storage SDK.</span></span> <span data-ttu-id="1c29b-166">Se [exempel](#readmulti).)</span><span class="sxs-lookup"><span data-stu-id="1c29b-166">See [sample](#readmulti).)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="1c29b-167">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="1c29b-167">Output sample</span></span>
<span data-ttu-id="1c29b-168">hello följande *function.json* och *run.csx* exempel visar hur toowrite flera tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="1c29b-168">hello following *function.json* and *run.csx* example shows how toowrite multiple table entities.</span></span>

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="1c29b-169">Se hello språkspecifika prov som skapar flera tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="1c29b-169">See hello language-specific sample that creates multiple table entities.</span></span>

* [<span data-ttu-id="1c29b-170">C#</span><span class="sxs-lookup"><span data-stu-id="1c29b-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="1c29b-171">F#</span><span class="sxs-lookup"><span data-stu-id="1c29b-171">F#</span></span>](#outfsharp)
* [<span data-ttu-id="1c29b-172">Node.js</span><span class="sxs-lookup"><span data-stu-id="1c29b-172">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="1c29b-173">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="1c29b-173">Output sample in C#</span></span> #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="1c29b-174">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="1c29b-174">Output sample in F#</span></span> #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 too10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="1c29b-175">Utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="1c29b-175">Output sample in Node.js</span></span>
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a><span data-ttu-id="1c29b-176">Exempel: Läsa flera tabellentiteter i C#</span><span class="sxs-lookup"><span data-stu-id="1c29b-176">Sample: Read multiple table entities in C#</span></span>  #
<span data-ttu-id="1c29b-177">hello följande *function.json* och C#-kodexempel läser entiteter för en partitionsnyckel som anges i kön hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1c29b-177">hello following *function.json* and C# code example reads entities for a partition key that is specified in hello queue message.</span></span>

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="1c29b-178">hello C#-kod lägger till en referens toohello Azure Storage SDK: N så att hello entitetstypen kan härledas från `TableEntity`.</span><span class="sxs-lookup"><span data-stu-id="1c29b-178">hello C# code adds a reference toohello Azure Storage SDK so that hello entity type can derive from `TableEntity`.</span></span>

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a><span data-ttu-id="1c29b-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c29b-179">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

