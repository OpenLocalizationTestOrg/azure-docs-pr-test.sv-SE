---
title: "Arbeta med utlösare och bindningar i Azure Functions | Microsoft Docs"
description: "Lär dig hur du använder utlösare och bindningar i Azure Functions för att ansluta din kodkörning online händelser och molnbaserade tjänster."
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
ms.openlocfilehash: cc41debb2523df77be4db05817a4c7ac55604439
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="f5f25-104">Azure Functions-utlösare och bindningar begrepp</span><span class="sxs-lookup"><span data-stu-id="f5f25-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="f5f25-105">Azure Functions kan du skriva kod som svar på händelser i Azure och andra tjänster via *utlösare* och *bindningar*.</span><span class="sxs-lookup"><span data-stu-id="f5f25-105">Azure Functions allows you to write code in response to events in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="f5f25-106">Den här artikeln innehåller en översikt av utlösare och bindningar för alla programmeringsspråk som stöds.</span><span class="sxs-lookup"><span data-stu-id="f5f25-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="f5f25-107">Här beskrivs funktioner som är gemensamma för alla bindningar.</span><span class="sxs-lookup"><span data-stu-id="f5f25-107">Features that are common to all bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="f5f25-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="f5f25-108">Overview</span></span>

<span data-ttu-id="f5f25-109">Utlösare och bindningar är en deklarativ metod för att definiera hur en funktion anropas och hur det fungerar med data.</span><span class="sxs-lookup"><span data-stu-id="f5f25-109">Triggers and bindings are a declarative way to define how a function is invoked and what data it works with.</span></span> <span data-ttu-id="f5f25-110">En *utlösaren* definierar hur en funktion har anropats.</span><span class="sxs-lookup"><span data-stu-id="f5f25-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="f5f25-111">En funktion måste ha exakt en utlösare.</span><span class="sxs-lookup"><span data-stu-id="f5f25-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="f5f25-112">Utlösare har associerade data, vilket är vanligtvis nyttolasten som utlöste funktionen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-112">Triggers have associated data, which is usually the payload that triggered the function.</span></span> 

<span data-ttu-id="f5f25-113">Indata och utdata *bindningar* tillhandahåller en deklarativ metod för att ansluta till data från i din kod.</span><span class="sxs-lookup"><span data-stu-id="f5f25-113">Input and output *bindings* provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="f5f25-114">Precis som utlösare kan du ange anslutningssträngar och andra egenskaper i konfigurationen av funktionen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-114">Similar to triggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="f5f25-115">Bindningar är valfria och en funktion kan ha flera indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="f5f25-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="f5f25-116">Med hjälp av utlösare och bindningar, du kan skriva kod som är mer generisk och har inte hårdkoda information om tjänsterna med den samverkar.</span><span class="sxs-lookup"><span data-stu-id="f5f25-116">Using triggers and bindings, you can write code that is more generic and does not hardcode the details of the services with which it interacts.</span></span> <span data-ttu-id="f5f25-117">Data som kommer från tjänster som bara blir indatavärden för din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="f5f25-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="f5f25-118">Använd returvärdet för metoden för att spara data till en annan tjänst (till exempel skapa en ny rad i Azure Table Storage).</span><span class="sxs-lookup"><span data-stu-id="f5f25-118">To output data to another service (such as creating a new row in Azure Table Storage), use the return value of the method.</span></span> <span data-ttu-id="f5f25-119">Eller Använd en hjälpobjektet om du behöver utdata flera värden.</span><span class="sxs-lookup"><span data-stu-id="f5f25-119">Or, if you need to output multiple values, use a helper object.</span></span> <span data-ttu-id="f5f25-120">Utlösare och bindningar har en **namn** -egenskap som är en identifierare som du använder i din kod till bindningen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-120">Triggers and bindings have a **name** property, which is an identifier you use in your code to access the binding.</span></span>

<span data-ttu-id="f5f25-121">Du kan konfigurera utlösare och bindningar i den **integrera** i Azure Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-121">You can configure triggers and bindings in the **Integrate** tab in the Azure Functions portal.</span></span> <span data-ttu-id="f5f25-122">Under försättsbladen, Användargränssnittet ändrar en fil med namnet *function.json* fil i katalogen funktion.</span><span class="sxs-lookup"><span data-stu-id="f5f25-122">Under the covers, the UI modifies a file called *function.json* file in the function directory.</span></span> <span data-ttu-id="f5f25-123">Du kan redigera den här filen genom att ändra till den **redigeraren**.</span><span class="sxs-lookup"><span data-stu-id="f5f25-123">You can edit this file by changing to the **Advanced editor**.</span></span>

<span data-ttu-id="f5f25-124">I följande tabell visas utlösare och bindningar som stöds med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f5f25-124">The following table shows the triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="f5f25-125">Exempel: kön utlösare och tabellen utdatabindning</span><span class="sxs-lookup"><span data-stu-id="f5f25-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="f5f25-126">Anta att du vill skriva en ny rad till Azure Table Storage när ett nytt meddelande visas i Azure Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="f5f25-126">Suppose you want to write a new row to Azure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="f5f25-127">Det här scenariot kan implementeras med hjälp av en Azure-kö utdatabindning utlösare och en tabell.</span><span class="sxs-lookup"><span data-stu-id="f5f25-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="f5f25-128">En kö utlösare kräver följande information i den **integrera** fliken:</span><span class="sxs-lookup"><span data-stu-id="f5f25-128">A queue trigger requires the following information in the **Integrate** tab:</span></span>

* <span data-ttu-id="f5f25-129">Namnet på appen inställningen som innehåller anslutningssträngen för lagring konto för kön</span><span class="sxs-lookup"><span data-stu-id="f5f25-129">The name of the app setting that contains the storage account connection string for the queue</span></span>
* <span data-ttu-id="f5f25-130">Könamnet</span><span class="sxs-lookup"><span data-stu-id="f5f25-130">The queue name</span></span>
* <span data-ttu-id="f5f25-131">Identifieraren i koden för att läsa innehållet i meddelandet kön som `order`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-131">The identifier in your code to read the contents of the queue message, such as `order`.</span></span>

<span data-ttu-id="f5f25-132">Använd en output-bindning för att skriva till Azure Table Storage med följande information:</span><span class="sxs-lookup"><span data-stu-id="f5f25-132">To write to Azure Table Storage, use an output binding with the following details:</span></span>

* <span data-ttu-id="f5f25-133">Namnet på appen inställningen som innehåller anslutningssträngen för lagring konto för tabellen</span><span class="sxs-lookup"><span data-stu-id="f5f25-133">The name of the app setting that contains the storage account connection string for the table</span></span>
* <span data-ttu-id="f5f25-134">Tabellens namn</span><span class="sxs-lookup"><span data-stu-id="f5f25-134">The table name</span></span>
* <span data-ttu-id="f5f25-135">Identifieraren i koden för att skapa utdata objekt eller returvärdet från funktionen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-135">The identifier in your code to create output items, or the return value from the function.</span></span>

<span data-ttu-id="f5f25-136">Bindningar Använd appinställningar för anslutningssträngar för att tillämpa bäst rutin som *function.json* innehåller inte tjänsten hemligheter.</span><span class="sxs-lookup"><span data-stu-id="f5f25-136">Bindings use app settings for connection strings to enforce the best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="f5f25-137">Använd sedan de identifierare som du angav för att integrera med Azure Storage i koden.</span><span class="sxs-lookup"><span data-stu-id="f5f25-137">Then, use the identifiers you provided to integrate with Azure Storage in your code.</span></span>

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
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
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
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

<span data-ttu-id="f5f25-138">Här är den *function.json* som motsvarar föregående kod.</span><span class="sxs-lookup"><span data-stu-id="f5f25-138">Here is the *function.json* that corresponds to the preceding code.</span></span> <span data-ttu-id="f5f25-139">Observera att samma konfiguration kan användas, oavsett vilket språk i funktionen implementering.</span><span class="sxs-lookup"><span data-stu-id="f5f25-139">Note that the same configuration can be used, regardless of the language of the function implementation.</span></span>

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
<span data-ttu-id="f5f25-140">Visa och redigera innehållet i *function.json* i Azure-portalen klickar du på den **redigeraren** alternativet på den **integrera** för din funktion.</span><span class="sxs-lookup"><span data-stu-id="f5f25-140">To view and edit the contents of *function.json* in the Azure portal, click the **Advanced editor** option on the **Integrate** tab of your function.</span></span>

<span data-ttu-id="f5f25-141">Mer kodexempel och information om att integrera med Azure Storage finns [Azure Functions-utlösare och bindningar för Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f5f25-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="f5f25-142">Bindningen riktning</span><span class="sxs-lookup"><span data-stu-id="f5f25-142">Binding direction</span></span>

<span data-ttu-id="f5f25-143">Alla utlösare och bindningar har en `direction` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="f5f25-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="f5f25-144">För utlösare är riktningen alltid`in`</span><span class="sxs-lookup"><span data-stu-id="f5f25-144">For triggers, the direction is always `in`</span></span>
- <span data-ttu-id="f5f25-145">Inkommande och utgående bindningar använda `in` och`out`</span><span class="sxs-lookup"><span data-stu-id="f5f25-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="f5f25-146">Vissa bindningar stöd för en särskild riktning `inout`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="f5f25-147">Om du använder `inout`, endast den **redigeraren** är tillgängliga i den **integrera** fliken.</span><span class="sxs-lookup"><span data-stu-id="f5f25-147">If you use `inout`, only the **Advanced editor** is available in the **Integrate** tab.</span></span>

## <a name="using-the-function-return-type-to-return-a-single-output"></a><span data-ttu-id="f5f25-148">Använda Returtypen för funktionen för att returnera ett enda utflöde</span><span class="sxs-lookup"><span data-stu-id="f5f25-148">Using the function return type to return a single output</span></span>

<span data-ttu-id="f5f25-149">Föregående exempel visar hur du använder funktionen returvärdet ge utdata till en bindning som uppnås med hjälp av parametern särskilda namnet `$return`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-149">The preceding example shows how to use the function return value to provide output to a binding, which is achieved by using the special name parameter `$return`.</span></span> <span data-ttu-id="f5f25-150">(Detta stöds endast på språk som har ett returvärde, till exempel C#, JavaScript och F #.) Om en funktion har flera bindningar för utdata använder `$return` för endast en av bindningarna som utdata.</span><span class="sxs-lookup"><span data-stu-id="f5f25-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of the output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="f5f25-151">Exemplen nedan visar tillbaka hur typer som används med utdata bindningar i C#, JavaScript och F #.</span><span class="sxs-lookup"><span data-stu-id="f5f25-151">The examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

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
// JavaScript: return a value in the second parameter to context.done
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

## <a name="binding-datatype-property"></a><span data-ttu-id="f5f25-152">DataType-egenskapen för bindning</span><span class="sxs-lookup"><span data-stu-id="f5f25-152">Binding dataType property</span></span>

<span data-ttu-id="f5f25-153">Använda typer i .NET, definiera datatypen för indata.</span><span class="sxs-lookup"><span data-stu-id="f5f25-153">In .NET, use the types to define the data type for input data.</span></span> <span data-ttu-id="f5f25-154">Till exempel använda `string` att binda till texten i en kö-utlösare och en bytematris läsa som binary.</span><span class="sxs-lookup"><span data-stu-id="f5f25-154">For instance, use `string` to bind to the text of a queue trigger and a byte array to read as binary.</span></span>

<span data-ttu-id="f5f25-155">Språk som skrivs dynamiskt, till exempel JavaScript, använda den `dataType` egenskapen i Bindningsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="f5f25-155">For languages that are dynamically typed such as JavaScript, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="f5f25-156">Till exempel använda typen om du vill läsa innehållet i en HTTP-begäran i binärformat `binary`:</span><span class="sxs-lookup"><span data-stu-id="f5f25-156">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="f5f25-157">Andra alternativ för `dataType` är `stream` och `string`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="f5f25-158">Lösa app-inställningar</span><span class="sxs-lookup"><span data-stu-id="f5f25-158">Resolving app settings</span></span>
<span data-ttu-id="f5f25-159">Som bästa praxis, hemligheter och anslutningssträngar ska hanteras med app-inställningar i stället för konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="f5f25-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="f5f25-160">Detta begränsar åtkomst till dessa hemligheter och gör det säkert att lagra *function.json* i en offentlig källkontroll.</span><span class="sxs-lookup"><span data-stu-id="f5f25-160">This limits access to these secrets and makes it safe to store *function.json* in a public source control repository.</span></span>

<span data-ttu-id="f5f25-161">Appinställningar är också användbara när du vill ändra konfigurationen baserat på miljön.</span><span class="sxs-lookup"><span data-stu-id="f5f25-161">App settings are also useful whenever you want to change configuration based on the environment.</span></span> <span data-ttu-id="f5f25-162">I en testmiljö kan du vill övervaka en annan kö eller blob storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="f5f25-162">For example, in a test environment, you may want to monitor a different queue or blob storage container.</span></span>

<span data-ttu-id="f5f25-163">Appinställningar är löst när ett värde som omges av procenttecken, `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="f5f25-164">Observera att den `connection` egenskapen för utlösare och bindningar är ett specialfall och löser automatiskt värden som app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="f5f25-164">Note that the `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="f5f25-165">I följande exempel är en kö-utlösare som använder en appinställning `%input-queue-name%` att definiera kön att utlösa på.</span><span class="sxs-lookup"><span data-stu-id="f5f25-165">The following example is a queue trigger that uses an app setting `%input-queue-name%` to define the queue to trigger on.</span></span>

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

## <a name="trigger-metadata-properties"></a><span data-ttu-id="f5f25-166">Utlösaren metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="f5f25-166">Trigger metadata properties</span></span>

<span data-ttu-id="f5f25-167">Ange ytterligare metadatavärden förutom datanyttolasten som tillhandahålls av en utlösare (till exempel kön meddelandet som utlöste en funktion) många utlösare.</span><span class="sxs-lookup"><span data-stu-id="f5f25-167">In addition to the data payload provided by a trigger (such as the queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="f5f25-168">Dessa värden kan användas som indataparametrar i C# och F # eller egenskaper på den `context.bindings` objekt i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f5f25-168">These values can be used as input parameters in C# and F# or properties on the `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="f5f25-169">Till exempel stöder en kö utlösare följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f5f25-169">For example, a queue trigger supports the following properties:</span></span>

* <span data-ttu-id="f5f25-170">QueueTrigger - utlösa meddelandeinnehåll om en giltig sträng</span><span class="sxs-lookup"><span data-stu-id="f5f25-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="f5f25-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="f5f25-171">DequeueCount</span></span>
* <span data-ttu-id="f5f25-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="f5f25-172">ExpirationTime</span></span>
* <span data-ttu-id="f5f25-173">Id</span><span class="sxs-lookup"><span data-stu-id="f5f25-173">Id</span></span>
* <span data-ttu-id="f5f25-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="f5f25-174">InsertionTime</span></span>
* <span data-ttu-id="f5f25-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="f5f25-175">NextVisibleTime</span></span>
* <span data-ttu-id="f5f25-176">PopReceipt</span><span class="sxs-lookup"><span data-stu-id="f5f25-176">PopReceipt</span></span>

<span data-ttu-id="f5f25-177">Information om metadataegenskaper för varje utlösare beskrivs i motsvarande referensavsnittet.</span><span class="sxs-lookup"><span data-stu-id="f5f25-177">Details of metadata properties for each trigger are described in the corresponding reference topic.</span></span> <span data-ttu-id="f5f25-178">Dokumentation är också tillgänglig i den **integrera** för portalen, i den **dokumentationen** avsnittet nedan konfigurationsområde bindning.</span><span class="sxs-lookup"><span data-stu-id="f5f25-178">Documentation is also available in the **Integrate** tab of the portal, in the **Documentation** section below the binding configuration area.</span></span>  

<span data-ttu-id="f5f25-179">Eftersom blob-utlösare har vissa fördröjningar kan du använda en utlösare för kön för att köra funktionen (se [Blob Storage utlösaren](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="f5f25-179">For example, since blob triggers have some delays, you can use a queue trigger to run your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="f5f25-180">Kön meddelandet innehåller blobfilnamn som utlöser på.</span><span class="sxs-lookup"><span data-stu-id="f5f25-180">The queue message would contain the blob filename to trigger on.</span></span> <span data-ttu-id="f5f25-181">Med hjälp av den `queueTrigger` metadataegenskapen, du kan ange det här beteendet i konfigurationen, i stället för din kod.</span><span class="sxs-lookup"><span data-stu-id="f5f25-181">Using the `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

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

<span data-ttu-id="f5f25-182">Metadataegenskaper från en utlösare kan också användas i en *bindande uttryck* för en annan bindning som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f5f25-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in the following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="f5f25-183">Bindande uttryck och mönster</span><span class="sxs-lookup"><span data-stu-id="f5f25-183">Binding expressions and patterns</span></span>

<span data-ttu-id="f5f25-184">En av de viktigaste funktionerna i utlösare och bindningar är *bindningsuttryck*.</span><span class="sxs-lookup"><span data-stu-id="f5f25-184">One of the most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="f5f25-185">Du kan definiera mönster för uttryck som kan användas i andra bindningar eller din kod i din bindning.</span><span class="sxs-lookup"><span data-stu-id="f5f25-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="f5f25-186">Utlösaren metadata kan också användas i bindande uttryck, som visas i exemplet i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f5f25-186">Trigger metadata can also be used in binding expressions, as show in the sample in the preceding section.</span></span>

<span data-ttu-id="f5f25-187">Anta att du vill ändra storlek på bilder i viss blob storage-behållare, liknar den **storleksändring av** mallen i den **nya funktionen** sidan.</span><span class="sxs-lookup"><span data-stu-id="f5f25-187">For example, suppose you want to resize images in particular blob storage container, similar to the **Image Resizer** template in the **New Function** page.</span></span> <span data-ttu-id="f5f25-188">Gå till **nya funktionen** -> språk **C#** -> scenariot **exempel** -> **ImageResizer CSharp**.</span><span class="sxs-lookup"><span data-stu-id="f5f25-188">Go to **New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="f5f25-189">Här är den *function.json* definition:</span><span class="sxs-lookup"><span data-stu-id="f5f25-189">Here is the *function.json* definition:</span></span>

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

<span data-ttu-id="f5f25-190">Observera att den `filename` parameter används i både blob utlösardefinition samt blob-utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="f5f25-190">Notice that the `filename` parameter is used in both the blob trigger definition as well as the blob output binding.</span></span> <span data-ttu-id="f5f25-191">Den här parametern kan också användas i funktionskod.</span><span class="sxs-lookup"><span data-stu-id="f5f25-191">This parameter can also be used in function code.</span></span>

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a><span data-ttu-id="f5f25-192">Slumpmässig GUID</span><span class="sxs-lookup"><span data-stu-id="f5f25-192">Random GUIDs</span></span>
<span data-ttu-id="f5f25-193">Azure Functions erbjuder en bekvämlighet syntax för att skapa GUID i dina bindningar via den `{rand-guid}` bindande uttryck.</span><span class="sxs-lookup"><span data-stu-id="f5f25-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through the `{rand-guid}` binding expression.</span></span> <span data-ttu-id="f5f25-194">I följande exempel använder detta för att generera ett unikt blob-namn:</span><span class="sxs-lookup"><span data-stu-id="f5f25-194">The following example uses this to generate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="f5f25-195">Aktuell tid</span><span class="sxs-lookup"><span data-stu-id="f5f25-195">Current time</span></span>

<span data-ttu-id="f5f25-196">Du kan använda Bindningsuttrycket `DateTime`, vilket motsvarar `DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="f5f25-196">You can use the binding expression `DateTime`, which resolves to `DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a><span data-ttu-id="f5f25-197">Binda till anpassade inkommande egenskaper i ett uttryck för bindning</span><span class="sxs-lookup"><span data-stu-id="f5f25-197">Bind to custom input properties in a binding expression</span></span>

<span data-ttu-id="f5f25-198">Bindande uttryck kan även referera till egenskaper som är definierade i utlösaren nyttolasten sig själv.</span><span class="sxs-lookup"><span data-stu-id="f5f25-198">Binding expressions can also reference properties that are defined in the trigger payload itself.</span></span> <span data-ttu-id="f5f25-199">Du kanske vill binda dynamiskt till en blob storage-fil från ett filnamn som anges i en webhook.</span><span class="sxs-lookup"><span data-stu-id="f5f25-199">For example, you may want to dynamically bind to a blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="f5f25-200">Till exempel följande *function.json* använder en egenskap som kallas `BlobName` från nyttolasten utlösare:</span><span class="sxs-lookup"><span data-stu-id="f5f25-200">For example, the following *function.json* uses a property called `BlobName` from the trigger payload:</span></span>

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

<span data-ttu-id="f5f25-201">Om du vill åstadkomma detta i C# och F #, måste du definiera en POCO som definierar vilka fält som ska avserialiseras i nyttolasten för utlösare.</span><span class="sxs-lookup"><span data-stu-id="f5f25-201">To accomplish this in C# and F#, you must define a POCO that defines the fields that will be deserialized in the trigger payload.</span></span>

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

<span data-ttu-id="f5f25-202">JSON-deserialisering utförs automatiskt i JavaScript, och du kan använda egenskaperna direkt.</span><span class="sxs-lookup"><span data-stu-id="f5f25-202">In JavaScript, JSON deserialization is automatically performed and you can use the properties directly.</span></span>

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

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="f5f25-203">Konfigurerar bindningsdata vid körning</span><span class="sxs-lookup"><span data-stu-id="f5f25-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="f5f25-204">I C# och andra .NET-språk, kan du använda en tvingande bindning mönster, till skillnad från deklarativ bindningar i *function.json*.</span><span class="sxs-lookup"><span data-stu-id="f5f25-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed to the declarative bindings in *function.json*.</span></span> <span data-ttu-id="f5f25-205">Tvingande bindning är användbar när bindande parametrar måste beräknas vid körning i stället för design tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5f25-205">Imperative binding is useful when binding parameters need to be computed at runtime rather than design time.</span></span> <span data-ttu-id="f5f25-206">Läs mer i [bindning under körning via tvingande bindningar](functions-reference-csharp.md#imperative-bindings) i C#-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="f5f25-206">To learn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in the C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5f25-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5f25-207">Next steps</span></span>
<span data-ttu-id="f5f25-208">Mer information om en specifik bindning finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="f5f25-208">For more information on a specific binding, see the following articles:</span></span>

- [<span data-ttu-id="f5f25-209">HTTP och webhooks</span><span class="sxs-lookup"><span data-stu-id="f5f25-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="f5f25-210">Timer</span><span class="sxs-lookup"><span data-stu-id="f5f25-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="f5f25-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="f5f25-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="f5f25-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="f5f25-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="f5f25-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="f5f25-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="f5f25-214">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="f5f25-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="f5f25-215">Service Bus</span><span class="sxs-lookup"><span data-stu-id="f5f25-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="f5f25-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f5f25-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="f5f25-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="f5f25-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="f5f25-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="f5f25-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="f5f25-219">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="f5f25-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="f5f25-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="f5f25-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="f5f25-221">Extern fil</span><span class="sxs-lookup"><span data-stu-id="f5f25-221">External file</span></span>](functions-bindings-external-file.md)
