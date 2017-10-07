---
title: "aaaAzure funktioner F # för utvecklare | Microsoft Docs"
description: "Förstå hur toodevelop Azure Functions med F #."
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur, F #"
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: functions
ms.devlang: fsharp
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 1ac366ba6f73d191c582dcd9214b688ef719617a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-f-developer-reference"></a><span data-ttu-id="8ff5d-104">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="8ff5d-104">Azure Functions F# Developer Reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8ff5d-105">C#-skript</span><span class="sxs-lookup"><span data-stu-id="8ff5d-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="8ff5d-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="8ff5d-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="8ff5d-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="8ff5d-107">Node.js</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="8ff5d-108">F # för Azure Functions är en lösning för att enkelt köra små delar av kod eller ”funktioner”, i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-108">F# for Azure Functions is a solution for easily running small pieces of code, or "functions," in hello cloud.</span></span> <span data-ttu-id="8ff5d-109">Data flödar in din F #-funktion via funktionsargument.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-109">Data flows into your F# function via function arguments.</span></span> <span data-ttu-id="8ff5d-110">Argumentnamn anges i `function.json`, och det finns fördefinierade namn för att komma åt exempelvis hello funktionen loggning och annullering token.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="8ff5d-111">Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="8ff5d-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="how-fsx-works"></a><span data-ttu-id="8ff5d-112">Så här fungerar .fsx</span><span class="sxs-lookup"><span data-stu-id="8ff5d-112">How .fsx works</span></span>
<span data-ttu-id="8ff5d-113">En `.fsx` filen är ett F # skript.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-113">An `.fsx` file is an F# script.</span></span> <span data-ttu-id="8ff5d-114">Det kan betraktas som ett F #-projekt som ingår i en enda fil.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-114">It can be thought of as an F# project that's contained in a single file.</span></span> <span data-ttu-id="8ff5d-115">hello-filen innehåller både hello-koden för programmet (i det här fallet din Azure-funktion) och riktlinjer för att hantera beroenden.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-115">hello file contains both hello code for your program (in this case, your Azure Function) and directives for managing dependencies.</span></span>

<span data-ttu-id="8ff5d-116">När du använder en `.fsx` för en Azure-funktion ofta krävs sammansättningar ingår automatiskt för dig, så att toofocus på hello funktion i stället för ”standardtext” kod.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-116">When you use an `.fsx` for an Azure Function, commonly required assemblies are automatically included for you, allowing you toofocus on hello function rather than "boilerplate" code.</span></span>

## <a name="binding-tooarguments"></a><span data-ttu-id="8ff5d-117">Bindningen tooarguments</span><span class="sxs-lookup"><span data-stu-id="8ff5d-117">Binding tooarguments</span></span>
<span data-ttu-id="8ff5d-118">Varje bindning stöder vissa argument som anges i hello [Azure Functions-utlösare och bindningar utvecklare](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="8ff5d-118">Each binding supports some set of arguments, as detailed in hello [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md).</span></span> <span data-ttu-id="8ff5d-119">En av hello argumentet bindningar har stöd för en blob-utlösare är till exempel en POCO som kan uttryckas i en F #-post.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-119">For example, one of hello argument bindings a blob trigger supports is a POCO, which can be expressed using an F# record.</span></span> <span data-ttu-id="8ff5d-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-120">For example:</span></span>

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

<span data-ttu-id="8ff5d-121">F # Azure funktionen tar ett eller flera argument.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-121">Your F# Azure Function will take one or more arguments.</span></span> <span data-ttu-id="8ff5d-122">När vi pratar om Azure Functions argument vi refererar för*inkommande* argument och *utdata* argument.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-122">When we talk about Azure Functions arguments, we refer too*input* arguments and *output* arguments.</span></span> <span data-ttu-id="8ff5d-123">En indataargument är exakt vad det låter som: indata tooyour F # Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-123">An input argument is exactly what it sounds like: input tooyour F# Azure Function.</span></span> <span data-ttu-id="8ff5d-124">En *utdata* argumentet är föränderliga data eller en `byref<>` argumentet som fungerar som ett sätt toopass data tillbaka *ut* för din funktion.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-124">An *output* argument is mutable data or a `byref<>` argument that serves as a way toopass data back *out* of your function.</span></span>

<span data-ttu-id="8ff5d-125">I hello-exemplet ovan, `blob` är ett indataargument och `output` finns ett argument som utdata.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-125">In hello example above, `blob` is an input argument, and `output` is an output argument.</span></span> <span data-ttu-id="8ff5d-126">Observera att vi använde `byref<>` för `output` (det finns inget behov av tooadd hello `[<Out>]` anteckningen).</span><span class="sxs-lookup"><span data-stu-id="8ff5d-126">Notice that we used `byref<>` for `output` (there's no need tooadd hello `[<Out>]` annotation).</span></span> <span data-ttu-id="8ff5d-127">Med hjälp av en `byref<>` kan din funktion toochange vilken post eller objekt hello argument refererar till.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-127">Using a `byref<>` type allows your function toochange which record or object hello argument refers to.</span></span>

<span data-ttu-id="8ff5d-128">När en post F # används som en Indatatyp hello poster definition måste markeras med `[<CLIMutable>]` ordning tooallow hello Azure Functions framework tooset hello fält korrekt innan skicka hello poster tooyour funktion.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-128">When an F# record is used as an input type, hello record definition must be marked with `[<CLIMutable>]` in order tooallow hello Azure Functions framework tooset hello fields appropriately before passing hello record tooyour function.</span></span> <span data-ttu-id="8ff5d-129">Under hello huven `[<CLIMutable>]` genererar set-metoder för hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-129">Under hello hood, `[<CLIMutable>]` generates setters for hello record properties.</span></span> <span data-ttu-id="8ff5d-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-130">For example:</span></span>

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

<span data-ttu-id="8ff5d-131">En F #-klass kan också användas för både i och ut argument.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-131">An F# class can also be used for both in and out arguments.</span></span> <span data-ttu-id="8ff5d-132">För en klass behöver vanligtvis egenskaper get och set.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-132">For a class, properties will usually need getters and setters.</span></span> <span data-ttu-id="8ff5d-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-133">For example:</span></span>

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a><span data-ttu-id="8ff5d-134">Loggning</span><span class="sxs-lookup"><span data-stu-id="8ff5d-134">Logging</span></span>
<span data-ttu-id="8ff5d-135">toolog utdata tooyour [direktuppspelningsloggar](../app-service-web/web-sites-streaming-logs-and-console.md) F #, din funktion bör ta ett argument av typen `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-135">toolog output tooyour [streaming logs](../app-service-web/web-sites-streaming-logs-and-console.md) in F#, your function should take an argument of type `TraceWriter`.</span></span> <span data-ttu-id="8ff5d-136">För konsekvens, rekommenderar vi det här argumentet heter `log`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-136">For consistency, we recommend this argument is named `log`.</span></span> <span data-ttu-id="8ff5d-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-137">For example:</span></span>

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a><span data-ttu-id="8ff5d-138">Asynkrona</span><span class="sxs-lookup"><span data-stu-id="8ff5d-138">Async</span></span>
<span data-ttu-id="8ff5d-139">Hej `async` arbetsflöde kan användas men hello resultatet måste tooreturn en `Task`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-139">hello `async` workflow can be used, but hello result needs tooreturn a `Task`.</span></span> <span data-ttu-id="8ff5d-140">Detta kan göras med `Async.StartAsTask`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-140">This can be done with `Async.StartAsTask`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a><span data-ttu-id="8ff5d-141">Annullering Token</span><span class="sxs-lookup"><span data-stu-id="8ff5d-141">Cancellation Token</span></span>
<span data-ttu-id="8ff5d-142">Om din funktion måste toohandle avstängning utan problem, kan du ge det ett [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentet.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-142">If your function needs toohandle shutdown gracefully, you can give it a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span></span> <span data-ttu-id="8ff5d-143">Detta kan kombineras med `async`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-143">This can be combined with `async`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a><span data-ttu-id="8ff5d-144">Importera namnområden</span><span class="sxs-lookup"><span data-stu-id="8ff5d-144">Importing namespaces</span></span>
<span data-ttu-id="8ff5d-145">Namnområden kan öppnas i hello vanligt:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-145">Namespaces can be opened in hello usual way:</span></span>

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="8ff5d-146">hello följande namnområden öppnas automatiskt:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-146">hello following namespaces are automatically opened:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* <span data-ttu-id="8ff5d-147">`Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-147">`Microsoft.Azure.WebJobs.Host`.</span></span>

## <a name="referencing-external-assemblies"></a><span data-ttu-id="8ff5d-148">Refererar till externa sammansättningar</span><span class="sxs-lookup"><span data-stu-id="8ff5d-148">Referencing External Assemblies</span></span>
<span data-ttu-id="8ff5d-149">På liknande sätt framework-sammansättningen referenser läggas till med hello `#r "AssemblyName"` direktiv.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-149">Similarly, framework assembly references be added with hello `#r "AssemblyName"` directive.</span></span>

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="8ff5d-150">hello läggs följande sammansättningar automatiskt till av hello Azure Functions värdmiljön:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-150">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

* <span data-ttu-id="8ff5d-151">`mscorlib`,</span><span class="sxs-lookup"><span data-stu-id="8ff5d-151">`mscorlib`,</span></span>
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* <span data-ttu-id="8ff5d-152">`System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-152">`System.Net.Http.Formatting`.</span></span>

<span data-ttu-id="8ff5d-153">Dessutom hello följande sammansättningar är särskilda fall och kan refereras av simplename (t.ex. `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="8ff5d-153">In addition, hello following assemblies are special cased and may be referenced by simplename (e.g. `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* <span data-ttu-id="8ff5d-154">`Microsoft.AspNEt.WebHooks.Common`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-154">`Microsoft.AspNEt.WebHooks.Common`.</span></span>

<span data-ttu-id="8ff5d-155">Om du behöver tooreference en privat sammansättning kan du överföra hello sammansättningsfilen i en `bin` mappen relativa tooyour funktion och referens den med hjälp av hello filnamn (t.ex.)  `#r "MyAssembly.dll"`).</span><span class="sxs-lookup"><span data-stu-id="8ff5d-155">If you need tooreference a private assembly, you can upload hello assembly file into a `bin` folder relative tooyour function and reference it by using hello file name (e.g.  `#r "MyAssembly.dll"`).</span></span> <span data-ttu-id="8ff5d-156">Information om hur tooupload filer tooyour funktionen mappen finns hello följande avsnitt om hantering av paketet.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-156">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

## <a name="editor-prelude"></a><span data-ttu-id="8ff5d-157">Redigeraren Prelude</span><span class="sxs-lookup"><span data-stu-id="8ff5d-157">Editor Prelude</span></span>
<span data-ttu-id="8ff5d-158">En redigerare som stöder F #-kompilatorn Services är inte medveten om hello namnområden och sammansättningar som automatiskt lägger till Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-158">An editor that supports F# Compiler Services will not be aware of hello namespaces and assemblies that Azure Functions automatically includes.</span></span> <span data-ttu-id="8ff5d-159">Därför kan det vara användbart tooinclude en prelude som hjälper hello editor hitta hello sammansättningar som du använder och tooexplicitly öppna namnområden.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-159">As such, it can be useful tooinclude a prelude that helps hello editor find hello assemblies you are using, and tooexplicitly open namespaces.</span></span> <span data-ttu-id="8ff5d-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-160">For example:</span></span>

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

<span data-ttu-id="8ff5d-161">När Azure Functions utförs koden, behandlar hello källa med `COMPILED` definierats så hello editor prelude kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-161">When Azure Functions executes your code, it processes hello source with `COMPILED` defined, so hello editor prelude will be ignored.</span></span>

<a name="package"></a>

## <a name="package-management"></a><span data-ttu-id="8ff5d-162">Hantering av paketet</span><span class="sxs-lookup"><span data-stu-id="8ff5d-162">Package management</span></span>
<span data-ttu-id="8ff5d-163">toouse NuGet-paket i en F # funktion, lägga till en `project.json` filen toohello hello funktionen mapp i hello funktionsapp-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-163">toouse NuGet packages in an F# function, add a `project.json` file toohello hello function's folder in hello function app's file system.</span></span> <span data-ttu-id="8ff5d-164">Här är ett exempel `project.json` -fil som lägger till en referens för NuGet-paketet för`Microsoft.ProjectOxford.Face` version 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-164">Here is an example `project.json` file that adds a NuGet package reference too`Microsoft.ProjectOxford.Face` version 1.1.0:</span></span>

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

<span data-ttu-id="8ff5d-165">Endast hello .NET Framework 4.6 stöds, så se till att din `project.json` filen anger `net46` som visas här.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-165">Only hello .NET Framework 4.6 is supported, so make sure that your `project.json` file specifies `net46` as shown here.</span></span>

<span data-ttu-id="8ff5d-166">När du överför en `project.json` fil, hello runtime hämtar hello paket och lägger automatiskt till referenser toohello paketet sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-166">When you upload a `project.json` file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="8ff5d-167">Du behöver inte tooadd `#r "AssemblyName"` direktiven.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-167">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="8ff5d-168">Lägg bara till hello krävs `open` instruktioner tooyour `.fsx` fil.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-168">Just add hello required `open` statements tooyour `.fsx` file.</span></span>

<span data-ttu-id="8ff5d-169">Vill du kanske tooput automatiskt refererar till sammansättningar i din editor prelude tooimprove din editor interaktion med F # kompilera tjänster.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-169">You may wish tooput automatically references assemblies in your editor prelude, tooimprove your editor's interaction with F# Compile Services.</span></span>

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a><span data-ttu-id="8ff5d-170">Hur tooadd en `project.json` filen tooyour Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="8ff5d-170">How tooadd a `project.json` file tooyour Azure Function</span></span>
1. <span data-ttu-id="8ff5d-171">Börja genom att kontrollera att funktionen appen körs det gör du genom att öppna din funktion i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-171">Begin by making sure your function app is running, which you can do by opening your function in hello Azure portal.</span></span> <span data-ttu-id="8ff5d-172">Detta ger även åtkomst toohello direktuppspelningsloggar där paketet installationen utdata visas.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-172">This also gives access toohello streaming logs where package installation output will be displayed.</span></span>
2. <span data-ttu-id="8ff5d-173">tooupload en `project.json` fil ska du använda en av metoderna i hello [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="8ff5d-173">tooupload a `project.json` file, use one of hello methods described in [how tooupdate function app files](functions-reference.md#fileupdate).</span></span> <span data-ttu-id="8ff5d-174">Om du använder [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md), kan du lägga till en `project.json` filen tooyour mellanlagring gren i ordning tooexperiment med den innan du lägger till den tooyour distribution grenen.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-174">If you are using [Continuous Deployment for Azure Functions](functions-continuous-deployment.md), you can add a `project.json` file tooyour staging branch in order tooexperiment with it before adding it tooyour deployment branch.</span></span>
3. <span data-ttu-id="8ff5d-175">Efter hello `project.json` fil har lagts till, visas utdata liknande toohello följande exempel i din funktion strömning loggen:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-175">After hello `project.json` file is added, you will see output similar toohello following example in your function's streaming log:</span></span>

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a><span data-ttu-id="8ff5d-176">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="8ff5d-176">Environment variables</span></span>
<span data-ttu-id="8ff5d-177">tooget en miljövariabel eller en app Inställningsvärdet använda `System.Environment.GetEnvironmentVariable`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-177">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, for example:</span></span>

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a><span data-ttu-id="8ff5d-178">Återanvända .fsx kod</span><span class="sxs-lookup"><span data-stu-id="8ff5d-178">Reusing .fsx code</span></span>
<span data-ttu-id="8ff5d-179">Du kan använda koden från andra `.fsx` filer med hjälp av en `#load` direktiv.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-179">You can use code from other `.fsx` files by using a `#load` directive.</span></span> <span data-ttu-id="8ff5d-180">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-180">For example:</span></span>

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

<span data-ttu-id="8ff5d-181">Sökvägar ger toohello `#load` direktiv är relativ toohello platsen för din `.fsx` fil.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-181">Paths provides toohello `#load` directive are relative toohello location of your `.fsx` file.</span></span>

* <span data-ttu-id="8ff5d-182">`#load "logger.fsx"`läser in en fil i mappen för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-182">`#load "logger.fsx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="8ff5d-183">`#load "package\logger.fsx"`läser in en fil i hello `package` i mappen med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-183">`#load "package\logger.fsx"` loads a file located in hello `package` folder in hello function folder.</span></span>
* <span data-ttu-id="8ff5d-184">`#load "..\shared\mylogger.fsx"`läser in en fil i hello `shared` mappen på samma nivå som hello funktionen mapp, det vill säga hello direkt under `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-184">`#load "..\shared\mylogger.fsx"` loads a file located in hello `shared` folder at hello same level as hello function folder, that is, directly under `wwwroot`.</span></span>

<span data-ttu-id="8ff5d-185">Hej `#load` direktiv fungerar bara med `.fsx` (F #) skriptfiler och inte med `.fs` filer.</span><span class="sxs-lookup"><span data-stu-id="8ff5d-185">hello `#load` directive only works with `.fsx` (F# script) files, and not with `.fs` files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ff5d-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ff5d-186">Next steps</span></span>
<span data-ttu-id="8ff5d-187">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="8ff5d-187">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="8ff5d-188">F # Guide</span><span class="sxs-lookup"><span data-stu-id="8ff5d-188">F# Guide</span></span>](/dotnet/articles/fsharp/index)
* [<span data-ttu-id="8ff5d-189">Metodtips för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="8ff5d-189">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="8ff5d-190">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="8ff5d-190">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="8ff5d-191">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="8ff5d-191">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="8ff5d-192">Azure Functions NodeJS för utvecklare</span><span class="sxs-lookup"><span data-stu-id="8ff5d-192">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="8ff5d-193">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="8ff5d-193">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* [<span data-ttu-id="8ff5d-194">Azure Functions testning</span><span class="sxs-lookup"><span data-stu-id="8ff5d-194">Azure Functions testing</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="8ff5d-195">Azure Functions skalning</span><span class="sxs-lookup"><span data-stu-id="8ff5d-195">Azure Functions scaling</span></span>](functions-scale.md)

