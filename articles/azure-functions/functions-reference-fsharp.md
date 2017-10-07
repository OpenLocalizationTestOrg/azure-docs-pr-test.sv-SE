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
# <a name="azure-functions-f-developer-reference"></a>Azure Functions F # för utvecklare
> [!div class="op_single_selector"]
> * [C#-skript](functions-reference-csharp.md)
> * [F # skript](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
> 
> 

F # för Azure Functions är en lösning för att enkelt köra små delar av kod eller ”funktioner”, i hello molnet. Data flödar in din F #-funktion via funktionsargument. Argumentnamn anges i `function.json`, och det finns fördefinierade namn för att komma åt exempelvis hello funktionen loggning och annullering token.

Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).

## <a name="how-fsx-works"></a>Så här fungerar .fsx
En `.fsx` filen är ett F # skript. Det kan betraktas som ett F #-projekt som ingår i en enda fil. hello-filen innehåller både hello-koden för programmet (i det här fallet din Azure-funktion) och riktlinjer för att hantera beroenden.

När du använder en `.fsx` för en Azure-funktion ofta krävs sammansättningar ingår automatiskt för dig, så att toofocus på hello funktion i stället för ”standardtext” kod.

## <a name="binding-tooarguments"></a>Bindningen tooarguments
Varje bindning stöder vissa argument som anges i hello [Azure Functions-utlösare och bindningar utvecklare](functions-triggers-bindings.md). En av hello argumentet bindningar har stöd för en blob-utlösare är till exempel en POCO som kan uttryckas i en F #-post. Exempel:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F # Azure funktionen tar ett eller flera argument. När vi pratar om Azure Functions argument vi refererar för*inkommande* argument och *utdata* argument. En indataargument är exakt vad det låter som: indata tooyour F # Azure-funktion. En *utdata* argumentet är föränderliga data eller en `byref<>` argumentet som fungerar som ett sätt toopass data tillbaka *ut* för din funktion.

I hello-exemplet ovan, `blob` är ett indataargument och `output` finns ett argument som utdata. Observera att vi använde `byref<>` för `output` (det finns inget behov av tooadd hello `[<Out>]` anteckningen). Med hjälp av en `byref<>` kan din funktion toochange vilken post eller objekt hello argument refererar till.

När en post F # används som en Indatatyp hello poster definition måste markeras med `[<CLIMutable>]` ordning tooallow hello Azure Functions framework tooset hello fält korrekt innan skicka hello poster tooyour funktion. Under hello huven `[<CLIMutable>]` genererar set-metoder för hello egenskaper. Exempel:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

En F #-klass kan också användas för både i och ut argument. För en klass behöver vanligtvis egenskaper get och set. Exempel:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Loggning
toolog utdata tooyour [direktuppspelningsloggar](../app-service-web/web-sites-streaming-logs-and-console.md) F #, din funktion bör ta ett argument av typen `TraceWriter`. För konsekvens, rekommenderar vi det här argumentet heter `log`. Exempel:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynkrona
Hej `async` arbetsflöde kan användas men hello resultatet måste tooreturn en `Task`. Detta kan göras med `Async.StartAsTask`, till exempel:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Annullering Token
Om din funktion måste toohandle avstängning utan problem, kan du ge det ett [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentet. Detta kan kombineras med `async`, till exempel:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importera namnområden
Namnområden kan öppnas i hello vanligt:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

hello följande namnområden öppnas automatiskt:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Refererar till externa sammansättningar
På liknande sätt framework-sammansättningen referenser läggas till med hello `#r "AssemblyName"` direktiv.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

hello läggs följande sammansättningar automatiskt till av hello Azure Functions värdmiljön:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Dessutom hello följande sammansättningar är särskilda fall och kan refereras av simplename (t.ex. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Om du behöver tooreference en privat sammansättning kan du överföra hello sammansättningsfilen i en `bin` mappen relativa tooyour funktion och referens den med hjälp av hello filnamn (t.ex.)  `#r "MyAssembly.dll"`). Information om hur tooupload filer tooyour funktionen mappen finns hello följande avsnitt om hantering av paketet.

## <a name="editor-prelude"></a>Redigeraren Prelude
En redigerare som stöder F #-kompilatorn Services är inte medveten om hello namnområden och sammansättningar som automatiskt lägger till Azure Functions. Därför kan det vara användbart tooinclude en prelude som hjälper hello editor hitta hello sammansättningar som du använder och tooexplicitly öppna namnområden. Exempel:

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

När Azure Functions utförs koden, behandlar hello källa med `COMPILED` definierats så hello editor prelude kommer att ignoreras.

<a name="package"></a>

## <a name="package-management"></a>Hantering av paketet
toouse NuGet-paket i en F # funktion, lägga till en `project.json` filen toohello hello funktionen mapp i hello funktionsapp-filsystemet. Här är ett exempel `project.json` -fil som lägger till en referens för NuGet-paketet för`Microsoft.ProjectOxford.Face` version 1.1.0:

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

Endast hello .NET Framework 4.6 stöds, så se till att din `project.json` filen anger `net46` som visas här.

När du överför en `project.json` fil, hello runtime hämtar hello paket och lägger automatiskt till referenser toohello paketet sammansättningar. Du behöver inte tooadd `#r "AssemblyName"` direktiven. Lägg bara till hello krävs `open` instruktioner tooyour `.fsx` fil.

Vill du kanske tooput automatiskt refererar till sammansättningar i din editor prelude tooimprove din editor interaktion med F # kompilera tjänster.

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a>Hur tooadd en `project.json` filen tooyour Azure-funktion
1. Börja genom att kontrollera att funktionen appen körs det gör du genom att öppna din funktion i hello Azure-portalen. Detta ger även åtkomst toohello direktuppspelningsloggar där paketet installationen utdata visas.
2. tooupload en `project.json` fil ska du använda en av metoderna i hello [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate). Om du använder [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md), kan du lägga till en `project.json` filen tooyour mellanlagring gren i ordning tooexperiment med den innan du lägger till den tooyour distribution grenen.
3. Efter hello `project.json` fil har lagts till, visas utdata liknande toohello följande exempel i din funktion strömning loggen:

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

## <a name="environment-variables"></a>Miljövariabler
tooget en miljövariabel eller en app Inställningsvärdet använda `System.Environment.GetEnvironmentVariable`, till exempel:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Återanvända .fsx kod
Du kan använda koden från andra `.fsx` filer med hjälp av en `#load` direktiv. Exempel:

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

Sökvägar ger toohello `#load` direktiv är relativ toohello platsen för din `.fsx` fil.

* `#load "logger.fsx"`läser in en fil i mappen för hello-funktionen.
* `#load "package\logger.fsx"`läser in en fil i hello `package` i mappen med hello-funktionen.
* `#load "..\shared\mylogger.fsx"`läser in en fil i hello `shared` mappen på samma nivå som hello funktionen mapp, det vill säga hello direkt under `wwwroot`.

Hej `#load` direktiv fungerar bara med `.fsx` (F #) skriptfiler och inte med `.fs` filer.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

* [F # Guide](/dotnet/articles/fsharp/index)
* [Metodtips för Azure Functions](functions-best-practices.md)
* [Azure Functions, info för utvecklare](functions-reference.md)
* [Azure Functions C# för utvecklare](functions-reference-csharp.md)
* [Azure Functions NodeJS för utvecklare](functions-reference-node.md)
* [Azure Functions-utlösare och bindningar](functions-triggers-bindings.md)
* [Azure Functions testning](functions-test-a-function.md)
* [Azure Functions skalning](functions-scale.md)

