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
# <a name="azure-functions-storage-table-bindings"></a>Azure Functions lagring tabell bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och koden Azure Storage tabell bindningar i Azure Functions. Azure Functions stöder indata och utdata bindningar för Azure Storage-tabeller.

hello lagring tabell bindningen stöder hello följande scenarier:

* **Läs en enskild rad i en C# eller Node.js-funktion** – du kan ställa `partitionKey` och `rowKey`. Hej `filter` och `take` egenskaper inte används i det här scenariot.
* **Läsa flera rader i en C#-funktion** -hello Functions-runtime ger en `IQueryable<T>` objektet bundet toohello tabell. Typen `T` måste vara härledd från `TableEntity` eller implementerar `ITableEntity`. Hej `partitionKey`, `rowKey`, `filter`, och `take` egenskaper inte används i det här scenariot; du kan använda hello `IQueryable` objekt toodo filtrering krävs. 
* **Läsa flera rader i en nod funktion** – du kan ställa hello `filter` och `take` egenskaper. Konfigurerar inte `partitionKey` eller `rowKey`.
* **Skriv en eller flera rader i en C#-funktion** -hello Functions-runtime ger en `ICollector<T>` eller `IAsyncCollector<T>` bundna toohello tabellen där `T` anger hello schemat hello entiteter som du vill tooadd. Normalt anger `T` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte. Hej `partitionKey`, `rowKey`, `filter`, och `take` egenskaper inte används i det här scenariot.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Lagring tabell indatabindning
hello Azure Storage tabell indatabindning kan toouse en tabell för lagring i din funktion. 

hello lagringsfunktionen tabell inkommande tooa använder hello följande JSON-objekt i hello `bindings` matris med function.json:

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

Observera följande hello: 

* Använd `partitionKey` och `rowKey` tillsammans tooread en enda entitet. De här egenskaperna är valfria. 
* `connection`måste innehålla hello en app-inställning som innehåller en anslutningssträng för lagring. I hello Azure-portalen, hello standardredigeraren i hello **integrera** konfigurerar appinställningen för dig när du skapar en lagringsplats för kontot eller väljer en befintlig. Du kan också [konfigurera den här appen inställningen manuellt](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Inkommande användning
I C#-funktioner, du binda toohello inkommande tabell entitet (eller entiteter) med hjälp av en namngiven parameter i en funktionssignaturen som `<T> <name>`.
Där `T` är hello datatypen som du vill toodeserialize hello data till och `paramName` är hello namn du angav i hello [inkommande bindningen](#input). I Node.js-funktion, du åtkomst till hello inkommande tabell entitet (eller entiteter) med hjälp av `context.bindings.<name>`.

hello indata kan avserialiseras i Node.js- eller C#-funktioner. hello avserialiseras objekt har `RowKey` och `PartitionKey` egenskaper.

Du kan också binda tooany av hello följande typer i C#-funktioner, och hello funktioner runtime försöker för deserialisera hello tabelldata med hjälp av den typen:

* Typer som implementerar`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Indata-exempel
Anta att du har följande function.json som använder en kö utlösaren tooread en tabellrad hello. hello JSON anger `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Visar hello raden nyckeln kommer från hello kön meddelandet sträng.

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

Se hello språkspecifika prov som läser en enskild tabell entitet.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Inkommande exemplet i C# #
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

### <a name="input-sample-in-f"></a>Inkommande exemplet i F # #
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

### <a name="input-sample-in-nodejs"></a>Inkommande exemplet i Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Lagringstabellen utdatabindning
hello Azure Storage tabell utdata bindning kan du toowrite entiteter tooa lagringstabellen i din funktion. 

Hej lagring tabellutdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:

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

Observera följande hello: 

* Använd `partitionKey` och `rowKey` tillsammans toowrite en enda entitet. De här egenskaperna är valfria. Du kan också ange `PartitionKey` och `RowKey` när du skapar hello entitetsobjekt i funktionskoden.
* `connection`måste innehålla hello en app-inställning som innehåller en anslutningssträng för lagring. I hello Azure-portalen, hello standardredigeraren i hello **integrera** konfigurerar appinställningen för dig när du skapar en lagringsplats för kontot eller väljer en befintlig. Du kan också [konfigurera den här appen inställningen manuellt](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Användning av utdata
I C#-funktioner, binda toohello tabellutdata med hjälp av hello med namnet `out` parameter i en funktionssignaturen som `out <T> <name>`, där `T` är hello datatypen som du vill tooserialize hello data till och `paramName` är hello servernamnet du anges i hello [utdatabindning](#output). I Node.js-funktion, du åtkomst till hello tabell utdata med `context.bindings.<name>`.

Du kan serialisera objekt i Node.js- eller C#-funktioner. I C#-funktioner, kan du också binda toohello följande typer:

* Typer som implementerar`ITableEntity`
* `ICollector<T>`(toooutput flera enheter. Se [exempel](#outcsharp).)
* `IAsyncCollector<T>`(asynkrona versionen av `ICollector<T>`)
* `CloudTable`(med hello Azure Storage SDK: N. Se [exempel](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Exempel på utdata
hello följande *function.json* och *run.csx* exempel visar hur toowrite flera tabellentiteter.

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

Se hello språkspecifika prov som skapar flera tabellentiteter.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Utdata exemplet i C# #
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

### <a name="output-sample-in-f"></a>Utdata exemplet i F # #
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

### <a name="output-sample-in-nodejs"></a>Utdata exemplet i Node.js
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

## <a name="sample-read-multiple-table-entities-in-c"></a>Exempel: Läsa flera tabellentiteter i C#  #
hello följande *function.json* och C#-kodexempel läser entiteter för en partitionsnyckel som anges i kön hello-meddelande.

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

hello C#-kod lägger till en referens toohello Azure Storage SDK: N så att hello entitetstypen kan härledas från `TableEntity`.

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

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

