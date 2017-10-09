---
title: "aaaAzure funktioner C# skript för utvecklare | Microsoft Docs"
description: "Förstå hur toodevelop Azure Functions med C#."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: donnam
ms.openlocfilehash: 27a8f4eb77497a373ff4031539e2e930585e48e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-c-script-developer-reference"></a>Azure Functions C# skript för utvecklare
> [!div class="op_single_selector"]
> * [C#-skript](functions-reference-csharp.md)
> * [F # skript](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
>
>

hello C# skript-upplevelse för Azure Functions baseras på hello Azure WebJobs SDK. Data flödar till ditt C#-funktionen via metodargument. Argumentnamn anges i `function.json`, och det finns fördefinierade namn för att komma åt exempelvis hello funktionen loggning och annullering token.

Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).

Information om hur du använder C# klassen bibliotek, se [med hjälp av .NET-klassbibliotek med Azure Functions](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>Så här fungerar .csx
Hej `.csx` formatet kan toowrite mindre ”standardtext” och fokusera på att skriva bara en C#-funktion. Inkludera alla referenser till sammansättningar och namnområden hello början av hello-filen som vanligt. I stället för omsluter allt i ett namnområde och klass kan bara definiera en `Run` metod. Om du behöver tooinclude hello alla klasser för instansen toodefine oformaterad gamla CLR-objekt (POCO) objekt, kan du inkludera en klass i samma fil.   

## <a name="binding-tooarguments"></a>Bindningen tooarguments
hello olika bindningar är bundna tooa C#-funktionen via hello `name` egenskap i hello *function.json* konfiguration. Varje bindning har sin egen typer som stöds. en blob-utlösare stöder till exempel en sträng, en POCO eller en CloudBlockBlob. hello stöds typer dokumenteras i hello referens för varje bindning. En POCO-objekt måste ha en get- och set som definierats för varje egenskap.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>Med hjälp av metoden returvärdet för utdata-bindning

Du kan använda ett returvärde för metoden för en output-bindning med namnet hello `$return` i *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Skrivning av flera utdatavärden

toowrite flera värden tooan utdata bindning använder hello [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) eller [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typer. Dessa typer är lässkyddad samlingar som är skrivna toohello utdata bindning när hello-metoden har slutförts.

Det här exemplet skriver flera meddelanden i kö med `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Loggning
toolog tooyour direktuppspelningsloggar i C# och innehålla ett argument av typen `TraceWriter`. Vi rekommenderar att du namnger den `log`. Undvik att använda `Console.Write` i Azure Functions. 

`TraceWriter`har definierats i hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Hej loggningsnivån för `TraceWriter` kan konfigureras i [värden\.json].

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynkrona
toomake en funktion som är asynkron, använda hello `async` nyckelordet och returnera ett `Task` objekt.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a>Annullering Token
Vissa åtgärder kräver korrekt avslutning. Det är alltid bästa toowrite kod som kan hantera kraschar i fall där du vill att toohandle korrekt avslutning begäranden som du definierar en [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) har angett argumentet.  En `CancellationToken` tillhandahålls toosignal att en värd avstängning utlöses.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importera namnområden
Om du behöver tooimport namnområden kan du göra det som vanligt med hello `using` satsen.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

hello följande namnområden importeras automatiskt och därför är valfria:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Refererar till externa sammansättningar
Lägg till referenser för framework-sammansättningar med hjälp av hello `#r "AssemblyName"` direktiv.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

hello läggs följande sammansättningar automatiskt till av hello Azure Functions värdmiljön:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

hello följande sammansättningar kan refereras till av enkelt namn (till exempel `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Referera till anpassade sammansättningar

tooreference en anpassad sammansättning, kan du använda antingen en *delade* sammansättningen eller en *privata* sammansättning:
- Delade sammansättningar är gemensamma för alla funktioner i en funktionsapp. tooreference anpassade sammansättning, att överföra hello sammansättningen tooyour funktionsapp, som i en `bin` mapp i roten för hello funktionen app. 
- Privata sammansättningar ingår i kontexten för en viss funktion och stöd för separat inläsning av olika versioner. Privata sammansättningar som ska överföras i en `bin` mapp i hello funktionen directory. Referens med hello filnamn, till exempel `#r "MyAssembly.dll"`. 

Information om hur tooupload filer tooyour funktionen mappen finns hello följande avsnitt om hantering av paketet.

### <a name="watched-directories"></a>Bevakad kataloger

hello-katalog som innehåller hello funktionen skriptfilen bevakas automatiskt för ändringar tooassemblies. toowatch för sammansättningen ändringar i andra kataloger, lägga till dem toohello `watchDirectories` lista i [värden\.json].

## <a name="using-nuget-packages"></a>Med hjälp av NuGet-paket
toouse NuGet-paket i en C#-funktionen överför en *project.json* filen toohello funktionen mapp i hello funktionsapp-filsystemet. Här är ett exempel *project.json* -fil som lägger till en referens tooMicrosoft.ProjectOxford.Face version 1.1.0:

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

Endast hello .NET Framework 4.6 stöds, så se till att din *project.json* filen anger `net46` som visas här.

När du överför en *project.json* fil, hello runtime hämtar hello paket och lägger automatiskt till referenser toohello paketet sammansättningar. Du behöver inte tooadd `#r "AssemblyName"` direktiven. toouse hello typer som definieras i hello NuGet-paket, lägga till hello krävs `using` instruktioner tooyour *run.csx* fil 

I hello Functions-runtime NuGet återställningen fungerar genom att jämföra `project.json` och `project.lock.json`. Om hello datum och tid stämplar hello filer **inte** matchar, en NuGet-återställning körs och NuGet hämtningsbara filer uppdateras paket. Emellertid, om hello datum och tid stämplar hello filer **gör** matchar NuGet inte utför en återställning. Därför `project.lock.json` bör inte användas eftersom det orsakar NuGet tooskip paketet återställning. distribuera hello Lås tooavoid lägger du till hello `project.lock.json` toohello `.gitignore` fil.

toouse en anpassad NuGet-feed, ange hello mata in en *Nuget.Config* filen i hello Funktionsapp rot. Mer information finns i [konfigurera NuGet beteende](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Med hjälp av en project.json-fil
1. Öppna hello-funktionen i hello Azure-portalen. hello loggar fliken visar hello paketet installationen utdata.
2. tooupload en project.json-fil med någon av hello-metoder som beskrivs i hello [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate) i referensavsnittet om hello Azure Functions-utvecklare.
3. Efter hello *project.json* överföra filen, så se utdata som liknar hello som följande exempel i din funktion strömning loggen:

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
tooget en miljövariabel eller en app Inställningsvärdet använda `System.Environment.GetEnvironmentVariable`som visas i följande kodexempel hello:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Återanvända .csx kod
Du kan använda klasser och metoder som definierats i andra *.csx* filer i din *run.csx* fil. toodo som använder `#load` direktiven i din *run.csx* fil. I följande exempel hello, en rutin för loggning med namnet `MyLogger` delas i *myLogger.csx* och lästs in i *run.csx* med hello `#load` direktiv:

Exempel *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Exempel *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Med hjälp av en delad *.csx* är ett vanligt mönster om du vill toostrongly din typargument mellan funktioner med hjälp av en POCO-objektet. I följande exempel förenklad hello, en HTTP-utlösaren och kö utlösaren delar en POCO-objekt med namnet `Order` toostrongly typen hello ordning data:

Exempel *run.csx* för HTTP-utlösare:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting tooprocessing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Exempel *run.csx* för kön utlösare:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Exempel *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Du kan använda en relativ sökväg med hello `#load` direktiv:

* `#load "mylogger.csx"`läser in en fil i mappen för hello-funktionen.
* `#load "loadedfiles\mylogger.csx"`läser in en fil i en mapp i hello funktionen mapp.
* `#load "..\shared\mylogger.csx"`läser in en fil i en mapp på samma nivå som hello funktionen mapp, det vill säga hello direkt under *wwwroot*.

Hej `#load` direktiv fungerar bara med *.csx* (C#) skriptfiler inte med *.cs* filer.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Bindning under körning via tvingande bindningar

I C# och andra .NET-språk, kan du använda en [tvingande](https://en.wikipedia.org/wiki/Imperative_programming) bindning mönster som skillnad från toohello [ *deklarativa* ](https://en.wikipedia.org/wiki/Declarative_programming) bindningar i *function.json*. Tvingande bindning är användbart när bindande parametrar behöver toobe beräknade körning i stället för design samtidigt. Med det här mönstret du binda toosupported indata och utdata bindning på direkt i din funktionskoden.

Definiera en tvingande bindning på följande sätt:

- **Inte** innehåller en post i *function.json* för din önskade tvingande bindningar.
- Använd en indataparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) eller [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Använd hello följande C# mönster tooperform hello databindning.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

där `BindingTypeAttribute` är hello .NET attribut som definierar en bindning och `T` är hello inkommande eller utgående typ som stöds av denna bindning. `T`kan inte heller ett `out` parametertypen (exempelvis `out JObject`). Till exempel tabellen Mobile Apps spara bindningen stöder [sex utdata typer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), men du kan bara använda [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) eller [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) för `T`.

Följande exempelkod hello skapar en [lagringsblob utdatabindning](functions-bindings-storage-blob.md#using-a-blob-output-binding) med blob sökväg som har definierats vid körning, sedan skriver en sträng toohello blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definierar hello [lagringsblob](functions-bindings-storage-blob.md) indata och utdata bindning, och [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) är av bindningstypen stöds utdata.
Eftersom, hello kod hämtar hello app standardinställningen för hello anslutningssträngen för lagring konto (vilket är `AzureWebJobsStorage`). Du kan ange en anpassad app inställningen toouse genom att lägga till den [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) och passerar hello attributet matris i `BindAsync<T>()`. Exempel:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

hello visar följande tabell hello .NET attribut för varje bindning typ och hello paket som de har definierats.

> [!div class="mx-codeBreakAll"]
| Bindning | Attribut | Lägg till referens |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Händelsehubbar | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Storage-kö | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Lagringsblob | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Tabell för lagring | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

* [Metodtips för Azure Functions](functions-best-practices.md)
* [Azure Functions, info för utvecklare](functions-reference.md)
* [Azure Functions F # för utvecklare](functions-reference-fsharp.md)
* [Azure Functions NodeJS för utvecklare](functions-reference-node.md)
* [Azure Functions-utlösare och bindningar](functions-triggers-bindings.md)

[värden\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
