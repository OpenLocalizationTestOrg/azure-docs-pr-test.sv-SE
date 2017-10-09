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
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="ac22f-104">Azure Functions-utlösare och bindningar begrepp</span><span class="sxs-lookup"><span data-stu-id="ac22f-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="ac22f-105">Azure Functions kan du toowrite koden i svaret tooevents i Azure och andra tjänster via *utlösare* och *bindningar*.</span><span class="sxs-lookup"><span data-stu-id="ac22f-105">Azure Functions allows you toowrite code in response tooevents in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="ac22f-106">Den här artikeln innehåller en översikt av utlösare och bindningar för alla programmeringsspråk som stöds.</span><span class="sxs-lookup"><span data-stu-id="ac22f-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="ac22f-107">Funktioner som är vanliga tooall bindningar beskrivs.</span><span class="sxs-lookup"><span data-stu-id="ac22f-107">Features that are common tooall bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="ac22f-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="ac22f-108">Overview</span></span>

<span data-ttu-id="ac22f-109">Utlösare och bindningar är en deklarativ metod toodefine hur en funktion har anropats och vilka data den fungerar med.</span><span class="sxs-lookup"><span data-stu-id="ac22f-109">Triggers and bindings are a declarative way toodefine how a function is invoked and what data it works with.</span></span> <span data-ttu-id="ac22f-110">En *utlösaren* definierar hur en funktion har anropats.</span><span class="sxs-lookup"><span data-stu-id="ac22f-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="ac22f-111">En funktion måste ha exakt en utlösare.</span><span class="sxs-lookup"><span data-stu-id="ac22f-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="ac22f-112">Utlösare har associerade data, vilket är vanligtvis hello-nyttolast som utlöste hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-112">Triggers have associated data, which is usually hello payload that triggered hello function.</span></span> 

<span data-ttu-id="ac22f-113">Indata och utdata *bindningar* tillhandahåller en deklarativ metod tooconnect toodata från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="ac22f-113">Input and output *bindings* provide a declarative way tooconnect toodata from within your code.</span></span> <span data-ttu-id="ac22f-114">Liknande tootriggers du ange anslutningssträngar och andra egenskaper i konfigurationen av funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-114">Similar tootriggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="ac22f-115">Bindningar är valfria och en funktion kan ha flera indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="ac22f-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="ac22f-116">Du kan skriva kod som är generisk och har inte hårdkoda hello detaljer om hello-tjänster som samverkar med utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="ac22f-116">Using triggers and bindings, you can write code that is more generic and does not hardcode hello details of hello services with which it interacts.</span></span> <span data-ttu-id="ac22f-117">Data som kommer från tjänster som bara blir indatavärden för din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="ac22f-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="ac22f-118">toooutput tooanother datatjänst (till exempel skapa en ny rad i Azure Table Storage), Använd hello returvärdet för hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="ac22f-118">toooutput data tooanother service (such as creating a new row in Azure Table Storage), use hello return value of hello method.</span></span> <span data-ttu-id="ac22f-119">Eller, om du behöver toooutput flera värden kan använda en hjälpobjektet.</span><span class="sxs-lookup"><span data-stu-id="ac22f-119">Or, if you need toooutput multiple values, use a helper object.</span></span> <span data-ttu-id="ac22f-120">Utlösare och bindningar har en **namn** -egenskap som är en identifierare som du använder i din kod tooaccess hello bindning.</span><span class="sxs-lookup"><span data-stu-id="ac22f-120">Triggers and bindings have a **name** property, which is an identifier you use in your code tooaccess hello binding.</span></span>

<span data-ttu-id="ac22f-121">Du kan konfigurera utlösare och bindningar i hello **integrera** fliken i hello Azure Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-121">You can configure triggers and bindings in hello **Integrate** tab in hello Azure Functions portal.</span></span> <span data-ttu-id="ac22f-122">Under hello omfattar hello UI ändrar en fil med namnet *function.json* fil i hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-122">Under hello covers, hello UI modifies a file called *function.json* file in hello function directory.</span></span> <span data-ttu-id="ac22f-123">Du kan redigera den här filen genom att ändra toohello **redigeraren**.</span><span class="sxs-lookup"><span data-stu-id="ac22f-123">You can edit this file by changing toohello **Advanced editor**.</span></span>

<span data-ttu-id="ac22f-124">hello följande tabell visar hello utlösare och bindningar som stöds med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ac22f-124">hello following table shows hello triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="ac22f-125">Exempel: kön utlösare och tabellen utdatabindning</span><span class="sxs-lookup"><span data-stu-id="ac22f-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="ac22f-126">Anta att du vill toowrite en ny rad tooAzure Table Storage när ett nytt meddelande visas i Azure Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="ac22f-126">Suppose you want toowrite a new row tooAzure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="ac22f-127">Det här scenariot kan implementeras med hjälp av en Azure-kö utdatabindning utlösare och en tabell.</span><span class="sxs-lookup"><span data-stu-id="ac22f-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="ac22f-128">En kö utlösare kräver följande information i hello hello **integrera** fliken:</span><span class="sxs-lookup"><span data-stu-id="ac22f-128">A queue trigger requires hello following information in hello **Integrate** tab:</span></span>

* <span data-ttu-id="ac22f-129">hello innehåller hello app inställningen som hello konto lagringsanslutningssträng för hello kö</span><span class="sxs-lookup"><span data-stu-id="ac22f-129">hello name of hello app setting that contains hello storage account connection string for hello queue</span></span>
* <span data-ttu-id="ac22f-130">hello könamnet</span><span class="sxs-lookup"><span data-stu-id="ac22f-130">hello queue name</span></span>
* <span data-ttu-id="ac22f-131">Hej identifierare i din kod tooread hello innehållet i hello kömeddelande som `order`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-131">hello identifier in your code tooread hello contents of hello queue message, such as `order`.</span></span>

<span data-ttu-id="ac22f-132">toowrite tooAzure Table Storage, använda en output-bindning med hello följande information:</span><span class="sxs-lookup"><span data-stu-id="ac22f-132">toowrite tooAzure Table Storage, use an output binding with hello following details:</span></span>

* <span data-ttu-id="ac22f-133">hello innehåller hello app inställningen som hello konto lagringsanslutningssträng för hello tabellen</span><span class="sxs-lookup"><span data-stu-id="ac22f-133">hello name of hello app setting that contains hello storage account connection string for hello table</span></span>
* <span data-ttu-id="ac22f-134">hello tabellnamn</span><span class="sxs-lookup"><span data-stu-id="ac22f-134">hello table name</span></span>
* <span data-ttu-id="ac22f-135">hello identifierare i din kod toocreate utdata objekt eller hello returvärde från hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-135">hello identifier in your code toocreate output items, or hello return value from hello function.</span></span>

<span data-ttu-id="ac22f-136">Bindningar använda app-inställningar för anslutning strängar tooenforce hello bästa rutin som *function.json* innehåller inte tjänsten hemligheter.</span><span class="sxs-lookup"><span data-stu-id="ac22f-136">Bindings use app settings for connection strings tooenforce hello best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="ac22f-137">Använd sedan hello-identifierare som du angav toointegrate med Azure Storage i din kod.</span><span class="sxs-lookup"><span data-stu-id="ac22f-137">Then, use hello identifiers you provided toointegrate with Azure Storage in your code.</span></span>

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

<span data-ttu-id="ac22f-138">Här är hello *function.json* som motsvarar toohello föregående kod.</span><span class="sxs-lookup"><span data-stu-id="ac22f-138">Here is hello *function.json* that corresponds toohello preceding code.</span></span> <span data-ttu-id="ac22f-139">Observera att hello samma konfiguration kan användas, oavsett hello språket för hello funktionen implementering.</span><span class="sxs-lookup"><span data-stu-id="ac22f-139">Note that hello same configuration can be used, regardless of hello language of hello function implementation.</span></span>

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
<span data-ttu-id="ac22f-140">tooview och redigera hello innehållet i *function.json* i hello Azure-portalen klickar du på hello **redigeraren** alternativet på hello **integrera** för din funktion.</span><span class="sxs-lookup"><span data-stu-id="ac22f-140">tooview and edit hello contents of *function.json* in hello Azure portal, click hello **Advanced editor** option on hello **Integrate** tab of your function.</span></span>

<span data-ttu-id="ac22f-141">Mer kodexempel och information om att integrera med Azure Storage finns [Azure Functions-utlösare och bindningar för Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="ac22f-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="ac22f-142">Bindningen riktning</span><span class="sxs-lookup"><span data-stu-id="ac22f-142">Binding direction</span></span>

<span data-ttu-id="ac22f-143">Alla utlösare och bindningar har en `direction` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="ac22f-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="ac22f-144">För utlösare är hello riktningen alltid`in`</span><span class="sxs-lookup"><span data-stu-id="ac22f-144">For triggers, hello direction is always `in`</span></span>
- <span data-ttu-id="ac22f-145">Inkommande och utgående bindningar använda `in` och`out`</span><span class="sxs-lookup"><span data-stu-id="ac22f-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="ac22f-146">Vissa bindningar stöd för en särskild riktning `inout`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="ac22f-147">Om du använder `inout`, endast hello **redigeraren** är tillgänglig i hello **integrera** fliken.</span><span class="sxs-lookup"><span data-stu-id="ac22f-147">If you use `inout`, only hello **Advanced editor** is available in hello **Integrate** tab.</span></span>

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a><span data-ttu-id="ac22f-148">Med hjälp av hello funktionen returtyp tooreturn ett enda utflöde</span><span class="sxs-lookup"><span data-stu-id="ac22f-148">Using hello function return type tooreturn a single output</span></span>

<span data-ttu-id="ac22f-149">hello föregående exempel visar hur toouse hello funktionen returvärdet tooprovide utdatabindning tooa, som uppnås med hjälp av hello särskilda name-parametern `$return`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-149">hello preceding example shows how toouse hello function return value tooprovide output tooa binding, which is achieved by using hello special name parameter `$return`.</span></span> <span data-ttu-id="ac22f-150">(Detta stöds endast på språk som har ett returvärde, till exempel C#, JavaScript och F #.) Om en funktion har flera bindningar för utdata använder `$return` för endast en av hello utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="ac22f-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of hello output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="ac22f-151">hello exemplen nedan visar tillbaka hur typer som används med utdata bindningar i C#, JavaScript och F #.</span><span class="sxs-lookup"><span data-stu-id="ac22f-151">hello examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

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

## <a name="binding-datatype-property"></a><span data-ttu-id="ac22f-152">DataType-egenskapen för bindning</span><span class="sxs-lookup"><span data-stu-id="ac22f-152">Binding dataType property</span></span>

<span data-ttu-id="ac22f-153">I .NET, använda hello typer toodefine hello datatyp för indata.</span><span class="sxs-lookup"><span data-stu-id="ac22f-153">In .NET, use hello types toodefine hello data type for input data.</span></span> <span data-ttu-id="ac22f-154">Till exempel använda `string` toobind toohello texten i en kö-utlösare och en byte-matris tooread som binary.</span><span class="sxs-lookup"><span data-stu-id="ac22f-154">For instance, use `string` toobind toohello text of a queue trigger and a byte array tooread as binary.</span></span>

<span data-ttu-id="ac22f-155">För språk som skrivs dynamiskt, till exempel JavaScript, använda hello `dataType` egenskap i hello bindning definition.</span><span class="sxs-lookup"><span data-stu-id="ac22f-155">For languages that are dynamically typed such as JavaScript, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="ac22f-156">Till exempel tooread hello innehållet i en HTTP-begäran i binärformat, Använd hello typen `binary`:</span><span class="sxs-lookup"><span data-stu-id="ac22f-156">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="ac22f-157">Andra alternativ för `dataType` är `stream` och `string`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="ac22f-158">Lösa app-inställningar</span><span class="sxs-lookup"><span data-stu-id="ac22f-158">Resolving app settings</span></span>
<span data-ttu-id="ac22f-159">Som bästa praxis, hemligheter och anslutningssträngar ska hanteras med app-inställningar i stället för konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="ac22f-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="ac22f-160">Detta begränsar åtkomst toothese hemligheter och gör den säker toostore *function.json* i en offentlig källkontroll.</span><span class="sxs-lookup"><span data-stu-id="ac22f-160">This limits access toothese secrets and makes it safe toostore *function.json* in a public source control repository.</span></span>

<span data-ttu-id="ac22f-161">Appinställningar är också användbara när du vill toochange configuration utifrån hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="ac22f-161">App settings are also useful whenever you want toochange configuration based on hello environment.</span></span> <span data-ttu-id="ac22f-162">Du kan exempelvis vilja toomonitor en annan kö eller blob storage-behållare i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ac22f-162">For example, in a test environment, you may want toomonitor a different queue or blob storage container.</span></span>

<span data-ttu-id="ac22f-163">Appinställningar är löst när ett värde som omges av procenttecken, `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="ac22f-164">Observera att hello `connection` -egenskapen för utlösare och bindningar är ett specialfall och löser automatiskt värden som app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ac22f-164">Note that hello `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="ac22f-165">hello följande exempel är en kö utlösare som använder en appinställning `%input-queue-name%` toodefine hello kön tootrigger på.</span><span class="sxs-lookup"><span data-stu-id="ac22f-165">hello following example is a queue trigger that uses an app setting `%input-queue-name%` toodefine hello queue tootrigger on.</span></span>

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

## <a name="trigger-metadata-properties"></a><span data-ttu-id="ac22f-166">Utlösaren metadataegenskaper</span><span class="sxs-lookup"><span data-stu-id="ac22f-166">Trigger metadata properties</span></span>

<span data-ttu-id="ac22f-167">Ange ytterligare metadatavärden i tillägg toohello datanyttolasten som tillhandahålls av en utlösare (till exempel kön hälsningsmeddelande som utlöste en funktion), många utlösare.</span><span class="sxs-lookup"><span data-stu-id="ac22f-167">In addition toohello data payload provided by a trigger (such as hello queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="ac22f-168">Dessa värden kan användas som indataparametrar i C# och F # eller egenskaper för hello `context.bindings` objekt i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ac22f-168">These values can be used as input parameters in C# and F# or properties on hello `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="ac22f-169">Till exempel stöder en kö utlösare hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ac22f-169">For example, a queue trigger supports hello following properties:</span></span>

* <span data-ttu-id="ac22f-170">QueueTrigger - utlösa meddelandeinnehåll om en giltig sträng</span><span class="sxs-lookup"><span data-stu-id="ac22f-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="ac22f-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="ac22f-171">DequeueCount</span></span>
* <span data-ttu-id="ac22f-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="ac22f-172">ExpirationTime</span></span>
* <span data-ttu-id="ac22f-173">Id</span><span class="sxs-lookup"><span data-stu-id="ac22f-173">Id</span></span>
* <span data-ttu-id="ac22f-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="ac22f-174">InsertionTime</span></span>
* <span data-ttu-id="ac22f-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="ac22f-175">NextVisibleTime</span></span>
* <span data-ttu-id="ac22f-176">PopReceipt</span><span class="sxs-lookup"><span data-stu-id="ac22f-176">PopReceipt</span></span>

<span data-ttu-id="ac22f-177">Information om metadataegenskaper för varje utlösare beskrivs i hello motsvarande referensavsnittet.</span><span class="sxs-lookup"><span data-stu-id="ac22f-177">Details of metadata properties for each trigger are described in hello corresponding reference topic.</span></span> <span data-ttu-id="ac22f-178">Dokumentation är också tillgänglig i hello **integrera** fliken hello portalen i hello **dokumentationen** avsnittet nedan hello konfigurationsområde för bindningen.</span><span class="sxs-lookup"><span data-stu-id="ac22f-178">Documentation is also available in hello **Integrate** tab of hello portal, in hello **Documentation** section below hello binding configuration area.</span></span>  

<span data-ttu-id="ac22f-179">Till exempel eftersom blob-utlösare har vissa fördröjningar kan du använda en kö utlösaren toorun din funktion (se [Blob Storage utlösaren](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="ac22f-179">For example, since blob triggers have some delays, you can use a queue trigger toorun your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="ac22f-180">hälsningsmeddelande för kön innehåller hello blob filename tootrigger på.</span><span class="sxs-lookup"><span data-stu-id="ac22f-180">hello queue message would contain hello blob filename tootrigger on.</span></span> <span data-ttu-id="ac22f-181">Med hjälp av hello `queueTrigger` metadataegenskapen, du kan ange det här beteendet i konfigurationen, i stället för din kod.</span><span class="sxs-lookup"><span data-stu-id="ac22f-181">Using hello `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

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

<span data-ttu-id="ac22f-182">Metadataegenskaper från en utlösare kan också användas i en *bindande uttryck* för en annan bindning som beskrivs i hello efter avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ac22f-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in hello following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="ac22f-183">Bindande uttryck och mönster</span><span class="sxs-lookup"><span data-stu-id="ac22f-183">Binding expressions and patterns</span></span>

<span data-ttu-id="ac22f-184">En av hello mest kraftfulla funktioner för utlösare och bindningar är *bindningsuttryck*.</span><span class="sxs-lookup"><span data-stu-id="ac22f-184">One of hello most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="ac22f-185">Du kan definiera mönster för uttryck som kan användas i andra bindningar eller din kod i din bindning.</span><span class="sxs-lookup"><span data-stu-id="ac22f-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="ac22f-186">Utlösaren metadata kan också användas i bindande uttryck, som visas i exemplet hello i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac22f-186">Trigger metadata can also be used in binding expressions, as show in hello sample in hello preceding section.</span></span>

<span data-ttu-id="ac22f-187">Anta att du vill tooresize bilder i viss blob storage-behållare, liknande toohello **storleksändring av** mallen i hello **nya funktionen** sidan.</span><span class="sxs-lookup"><span data-stu-id="ac22f-187">For example, suppose you want tooresize images in particular blob storage container, similar toohello **Image Resizer** template in hello **New Function** page.</span></span> <span data-ttu-id="ac22f-188">Gå för**nya funktionen** -> språk **C#** -> scenariot **exempel** -> **ImageResizer CSharp**.</span><span class="sxs-lookup"><span data-stu-id="ac22f-188">Go too**New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="ac22f-189">Här är hello *function.json* definition:</span><span class="sxs-lookup"><span data-stu-id="ac22f-189">Here is hello *function.json* definition:</span></span>

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

<span data-ttu-id="ac22f-190">Observera att hello `filename` parameter används i både hello blob utlösardefinition samt hello blob-utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="ac22f-190">Notice that hello `filename` parameter is used in both hello blob trigger definition as well as hello blob output binding.</span></span> <span data-ttu-id="ac22f-191">Den här parametern kan också användas i funktionskod.</span><span class="sxs-lookup"><span data-stu-id="ac22f-191">This parameter can also be used in function code.</span></span>

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


### <a name="random-guids"></a><span data-ttu-id="ac22f-192">Slumpmässig GUID</span><span class="sxs-lookup"><span data-stu-id="ac22f-192">Random GUIDs</span></span>
<span data-ttu-id="ac22f-193">Azure Functions erbjuder en bekvämlighet syntax för att skapa GUID i dina bindningar via hello `{rand-guid}` bindande uttryck.</span><span class="sxs-lookup"><span data-stu-id="ac22f-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through hello `{rand-guid}` binding expression.</span></span> <span data-ttu-id="ac22f-194">hello används följande exempel den här toogenerate ett unikt blob-namn:</span><span class="sxs-lookup"><span data-stu-id="ac22f-194">hello following example uses this toogenerate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="ac22f-195">Aktuell tid</span><span class="sxs-lookup"><span data-stu-id="ac22f-195">Current time</span></span>

<span data-ttu-id="ac22f-196">Du kan använda hello Bindningsuttrycket `DateTime`, vilket löser för`DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="ac22f-196">You can use hello binding expression `DateTime`, which resolves too`DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a><span data-ttu-id="ac22f-197">Binda toocustom inkommande egenskaper i ett uttryck för bindning</span><span class="sxs-lookup"><span data-stu-id="ac22f-197">Bind toocustom input properties in a binding expression</span></span>

<span data-ttu-id="ac22f-198">Bindande uttryck kan även referera till egenskaper som har definierats i hello utlösaren nyttolast sig själv.</span><span class="sxs-lookup"><span data-stu-id="ac22f-198">Binding expressions can also reference properties that are defined in hello trigger payload itself.</span></span> <span data-ttu-id="ac22f-199">Du kan exempelvis vilja toodynamically bind tooa blob storage-fil från ett filnamn som anges i en webhook.</span><span class="sxs-lookup"><span data-stu-id="ac22f-199">For example, you may want toodynamically bind tooa blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="ac22f-200">Till exempel hello följande *function.json* använder en egenskap som kallas `BlobName` från hello utlösaren nyttolast:</span><span class="sxs-lookup"><span data-stu-id="ac22f-200">For example, hello following *function.json* uses a property called `BlobName` from hello trigger payload:</span></span>

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

<span data-ttu-id="ac22f-201">tooaccomplish detta i C# och F #, måste du definiera en POCO som definierar hello fält som ska avserialiseras i hello utlösaren nyttolast.</span><span class="sxs-lookup"><span data-stu-id="ac22f-201">tooaccomplish this in C# and F#, you must define a POCO that defines hello fields that will be deserialized in hello trigger payload.</span></span>

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

<span data-ttu-id="ac22f-202">JSON-deserialisering utförs automatiskt i JavaScript, och du kan använda hello egenskaper direkt.</span><span class="sxs-lookup"><span data-stu-id="ac22f-202">In JavaScript, JSON deserialization is automatically performed and you can use hello properties directly.</span></span>

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

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="ac22f-203">Konfigurerar bindningsdata vid körning</span><span class="sxs-lookup"><span data-stu-id="ac22f-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="ac22f-204">I C# och andra .NET-språk, du kan använda ett absolut nödvändigt bindning mönster som skillnad från toohello deklarativ bindningar i *function.json*.</span><span class="sxs-lookup"><span data-stu-id="ac22f-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed toohello declarative bindings in *function.json*.</span></span> <span data-ttu-id="ac22f-205">Tvingande bindning är användbart när bindande parametrar behöver toobe beräknade körning i stället för design samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ac22f-205">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="ac22f-206">Det finns fler toolearn [bindning under körning via tvingande bindningar](functions-reference-csharp.md#imperative-bindings) i hello C#-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="ac22f-206">toolearn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in hello C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac22f-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac22f-207">Next steps</span></span>
<span data-ttu-id="ac22f-208">Mer information om en specifik bindning finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="ac22f-208">For more information on a specific binding, see hello following articles:</span></span>

- [<span data-ttu-id="ac22f-209">HTTP och webhooks</span><span class="sxs-lookup"><span data-stu-id="ac22f-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="ac22f-210">Timer</span><span class="sxs-lookup"><span data-stu-id="ac22f-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="ac22f-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="ac22f-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="ac22f-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="ac22f-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="ac22f-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="ac22f-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="ac22f-214">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="ac22f-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="ac22f-215">Service Bus</span><span class="sxs-lookup"><span data-stu-id="ac22f-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="ac22f-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ac22f-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="ac22f-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="ac22f-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="ac22f-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="ac22f-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="ac22f-219">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="ac22f-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="ac22f-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="ac22f-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="ac22f-221">Extern fil</span><span class="sxs-lookup"><span data-stu-id="ac22f-221">External file</span></span>](functions-bindings-external-file.md)
