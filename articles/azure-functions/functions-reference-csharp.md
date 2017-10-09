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
# <a name="azure-functions-c-script-developer-reference"></a><span data-ttu-id="7a385-104">Azure Functions C# skript för utvecklare</span><span class="sxs-lookup"><span data-stu-id="7a385-104">Azure Functions C# script developer reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7a385-105">C#-skript</span><span class="sxs-lookup"><span data-stu-id="7a385-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="7a385-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="7a385-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="7a385-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="7a385-107">Node.js</span></span>](functions-reference-node.md)
>
>

<span data-ttu-id="7a385-108">hello C# skript-upplevelse för Azure Functions baseras på hello Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="7a385-108">hello C# script experience for Azure Functions is based on hello Azure WebJobs SDK.</span></span> <span data-ttu-id="7a385-109">Data flödar till ditt C#-funktionen via metodargument.</span><span class="sxs-lookup"><span data-stu-id="7a385-109">Data flows into your C# function via method arguments.</span></span> <span data-ttu-id="7a385-110">Argumentnamn anges i `function.json`, och det finns fördefinierade namn för att komma åt exempelvis hello funktionen loggning och annullering token.</span><span class="sxs-lookup"><span data-stu-id="7a385-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="7a385-111">Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="7a385-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

<span data-ttu-id="7a385-112">Information om hur du använder C# klassen bibliotek, se [med hjälp av .NET-klassbibliotek med Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="7a385-112">For information on using C# class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span>

## <a name="how-csx-works"></a><span data-ttu-id="7a385-113">Så här fungerar .csx</span><span class="sxs-lookup"><span data-stu-id="7a385-113">How .csx works</span></span>
<span data-ttu-id="7a385-114">Hej `.csx` formatet kan toowrite mindre ”standardtext” och fokusera på att skriva bara en C#-funktion.</span><span class="sxs-lookup"><span data-stu-id="7a385-114">hello `.csx` format allows you toowrite less "boilerplate" and focus on writing just a C# function.</span></span> <span data-ttu-id="7a385-115">Inkludera alla referenser till sammansättningar och namnområden hello början av hello-filen som vanligt.</span><span class="sxs-lookup"><span data-stu-id="7a385-115">Include any assembly references and namespaces at hello beginning of hello file as usual.</span></span> <span data-ttu-id="7a385-116">I stället för omsluter allt i ett namnområde och klass kan bara definiera en `Run` metod.</span><span class="sxs-lookup"><span data-stu-id="7a385-116">Instead of wrapping everything in a namespace and class, just define a `Run` method.</span></span> <span data-ttu-id="7a385-117">Om du behöver tooinclude hello alla klasser för instansen toodefine oformaterad gamla CLR-objekt (POCO) objekt, kan du inkludera en klass i samma fil.</span><span class="sxs-lookup"><span data-stu-id="7a385-117">If you need tooinclude any classes, for instance toodefine Plain Old CLR Object (POCO) objects, you can include a class inside hello same file.</span></span>   

## <a name="binding-tooarguments"></a><span data-ttu-id="7a385-118">Bindningen tooarguments</span><span class="sxs-lookup"><span data-stu-id="7a385-118">Binding tooarguments</span></span>
<span data-ttu-id="7a385-119">hello olika bindningar är bundna tooa C#-funktionen via hello `name` egenskap i hello *function.json* konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7a385-119">hello various bindings are bound tooa C# function via hello `name` property in hello *function.json* configuration.</span></span> <span data-ttu-id="7a385-120">Varje bindning har sin egen typer som stöds. en blob-utlösare stöder till exempel en sträng, en POCO eller en CloudBlockBlob.</span><span class="sxs-lookup"><span data-stu-id="7a385-120">Each binding has its own supported types; for instance, a blob trigger can support a string, a POCO, or a CloudBlockBlob.</span></span> <span data-ttu-id="7a385-121">hello stöds typer dokumenteras i hello referens för varje bindning.</span><span class="sxs-lookup"><span data-stu-id="7a385-121">hello supported types are documented in hello reference for each binding.</span></span> <span data-ttu-id="7a385-122">En POCO-objekt måste ha en get- och set som definierats för varje egenskap.</span><span class="sxs-lookup"><span data-stu-id="7a385-122">A POCO object must have a getter and setter defined for each property.</span></span>

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

## <a name="using-method-return-value-for-output-binding"></a><span data-ttu-id="7a385-123">Med hjälp av metoden returvärdet för utdata-bindning</span><span class="sxs-lookup"><span data-stu-id="7a385-123">Using method return value for output binding</span></span>

<span data-ttu-id="7a385-124">Du kan använda ett returvärde för metoden för en output-bindning med namnet hello `$return` i *function.json*:</span><span class="sxs-lookup"><span data-stu-id="7a385-124">You can use a method return value for an output binding, by using hello name `$return` in *function.json*:</span></span>

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

## <a name="writing-multiple-output-values"></a><span data-ttu-id="7a385-125">Skrivning av flera utdatavärden</span><span class="sxs-lookup"><span data-stu-id="7a385-125">Writing multiple output values</span></span>

<span data-ttu-id="7a385-126">toowrite flera värden tooan utdata bindning använder hello [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) eller [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typer.</span><span class="sxs-lookup"><span data-stu-id="7a385-126">toowrite multiple values tooan output binding, use hello [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) types.</span></span> <span data-ttu-id="7a385-127">Dessa typer är lässkyddad samlingar som är skrivna toohello utdata bindning när hello-metoden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7a385-127">These types are write-only collections that are written toohello output binding when hello method completes.</span></span>

<span data-ttu-id="7a385-128">Det här exemplet skriver flera meddelanden i kö med `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="7a385-128">This example writes multiple queue messages using `ICollector`:</span></span>

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a><span data-ttu-id="7a385-129">Loggning</span><span class="sxs-lookup"><span data-stu-id="7a385-129">Logging</span></span>
<span data-ttu-id="7a385-130">toolog tooyour direktuppspelningsloggar i C# och innehålla ett argument av typen `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="7a385-130">toolog output tooyour streaming logs in C#, include an argument of type `TraceWriter`.</span></span> <span data-ttu-id="7a385-131">Vi rekommenderar att du namnger den `log`.</span><span class="sxs-lookup"><span data-stu-id="7a385-131">We recommend that you name it `log`.</span></span> <span data-ttu-id="7a385-132">Undvik att använda `Console.Write` i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="7a385-132">Avoid using `Console.Write` in Azure Functions.</span></span> 

<span data-ttu-id="7a385-133">`TraceWriter`har definierats i hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span><span class="sxs-lookup"><span data-stu-id="7a385-133">`TraceWriter` is defined in hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span></span> <span data-ttu-id="7a385-134">Hej loggningsnivån för `TraceWriter` kan konfigureras i [värden\.json].</span><span class="sxs-lookup"><span data-stu-id="7a385-134">hello log level for `TraceWriter` can be configured in [host\.json].</span></span>

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a><span data-ttu-id="7a385-135">Asynkrona</span><span class="sxs-lookup"><span data-stu-id="7a385-135">Async</span></span>
<span data-ttu-id="7a385-136">toomake en funktion som är asynkron, använda hello `async` nyckelordet och returnera ett `Task` objekt.</span><span class="sxs-lookup"><span data-stu-id="7a385-136">toomake a function asynchronous, use hello `async` keyword and return a `Task` object.</span></span>

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a><span data-ttu-id="7a385-137">Annullering Token</span><span class="sxs-lookup"><span data-stu-id="7a385-137">Cancellation Token</span></span>
<span data-ttu-id="7a385-138">Vissa åtgärder kräver korrekt avslutning.</span><span class="sxs-lookup"><span data-stu-id="7a385-138">Some operations require graceful shutdown.</span></span> <span data-ttu-id="7a385-139">Det är alltid bästa toowrite kod som kan hantera kraschar i fall där du vill att toohandle korrekt avslutning begäranden som du definierar en [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) har angett argumentet.</span><span class="sxs-lookup"><span data-stu-id="7a385-139">While it's always best toowrite code that can handle crashing,  in cases where you want toohandle graceful shutdown requests, you define a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) typed argument.</span></span>  <span data-ttu-id="7a385-140">En `CancellationToken` tillhandahålls toosignal att en värd avstängning utlöses.</span><span class="sxs-lookup"><span data-stu-id="7a385-140">A `CancellationToken` is provided toosignal that a host shutdown is triggered.</span></span>

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

## <a name="importing-namespaces"></a><span data-ttu-id="7a385-141">Importera namnområden</span><span class="sxs-lookup"><span data-stu-id="7a385-141">Importing namespaces</span></span>
<span data-ttu-id="7a385-142">Om du behöver tooimport namnområden kan du göra det som vanligt med hello `using` satsen.</span><span class="sxs-lookup"><span data-stu-id="7a385-142">If you need tooimport namespaces, you can do so as usual, with hello `using` clause.</span></span>

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="7a385-143">hello följande namnområden importeras automatiskt och därför är valfria:</span><span class="sxs-lookup"><span data-stu-id="7a385-143">hello following namespaces are automatically imported and are therefore optional:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a><span data-ttu-id="7a385-144">Refererar till externa sammansättningar</span><span class="sxs-lookup"><span data-stu-id="7a385-144">Referencing External Assemblies</span></span>
<span data-ttu-id="7a385-145">Lägg till referenser för framework-sammansättningar med hjälp av hello `#r "AssemblyName"` direktiv.</span><span class="sxs-lookup"><span data-stu-id="7a385-145">For framework assemblies, add references by using hello `#r "AssemblyName"` directive.</span></span>

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="7a385-146">hello läggs följande sammansättningar automatiskt till av hello Azure Functions värdmiljön:</span><span class="sxs-lookup"><span data-stu-id="7a385-146">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

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

<span data-ttu-id="7a385-147">hello följande sammansättningar kan refereras till av enkelt namn (till exempel `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="7a385-147">hello following assemblies may be referenced by simple-name (for example, `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a><span data-ttu-id="7a385-148">Referera till anpassade sammansättningar</span><span class="sxs-lookup"><span data-stu-id="7a385-148">Referencing custom assemblies</span></span>

<span data-ttu-id="7a385-149">tooreference en anpassad sammansättning, kan du använda antingen en *delade* sammansättningen eller en *privata* sammansättning:</span><span class="sxs-lookup"><span data-stu-id="7a385-149">tooreference a custom assembly, you can use either a *shared* assembly or a *private* assembly:</span></span>
- <span data-ttu-id="7a385-150">Delade sammansättningar är gemensamma för alla funktioner i en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="7a385-150">Shared assemblies are shared across all functions within a function app.</span></span> <span data-ttu-id="7a385-151">tooreference anpassade sammansättning, att överföra hello sammansättningen tooyour funktionsapp, som i en `bin` mapp i roten för hello funktionen app.</span><span class="sxs-lookup"><span data-stu-id="7a385-151">tooreference a custom assembly, upload hello assembly tooyour function app, such as in a `bin` folder in hello function app root.</span></span> 
- <span data-ttu-id="7a385-152">Privata sammansättningar ingår i kontexten för en viss funktion och stöd för separat inläsning av olika versioner.</span><span class="sxs-lookup"><span data-stu-id="7a385-152">Private assemblies are part of a given function's context, and support side-loading of different versions.</span></span> <span data-ttu-id="7a385-153">Privata sammansättningar som ska överföras i en `bin` mapp i hello funktionen directory.</span><span class="sxs-lookup"><span data-stu-id="7a385-153">Private assemblies should be uploaded in a `bin` folder in hello function directory.</span></span> <span data-ttu-id="7a385-154">Referens med hello filnamn, till exempel `#r "MyAssembly.dll"`.</span><span class="sxs-lookup"><span data-stu-id="7a385-154">Reference using hello file name, such as  `#r "MyAssembly.dll"`.</span></span> 

<span data-ttu-id="7a385-155">Information om hur tooupload filer tooyour funktionen mappen finns hello följande avsnitt om hantering av paketet.</span><span class="sxs-lookup"><span data-stu-id="7a385-155">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

### <a name="watched-directories"></a><span data-ttu-id="7a385-156">Bevakad kataloger</span><span class="sxs-lookup"><span data-stu-id="7a385-156">Watched directories</span></span>

<span data-ttu-id="7a385-157">hello-katalog som innehåller hello funktionen skriptfilen bevakas automatiskt för ändringar tooassemblies.</span><span class="sxs-lookup"><span data-stu-id="7a385-157">hello directory that contains hello function script file is automatically watched for changes tooassemblies.</span></span> <span data-ttu-id="7a385-158">toowatch för sammansättningen ändringar i andra kataloger, lägga till dem toohello `watchDirectories` lista i [värden\.json].</span><span class="sxs-lookup"><span data-stu-id="7a385-158">toowatch for assembly changes in other directories, add them toohello `watchDirectories` list in [host\.json].</span></span>

## <a name="using-nuget-packages"></a><span data-ttu-id="7a385-159">Med hjälp av NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="7a385-159">Using NuGet packages</span></span>
<span data-ttu-id="7a385-160">toouse NuGet-paket i en C#-funktionen överför en *project.json* filen toohello funktionen mapp i hello funktionsapp-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="7a385-160">toouse NuGet packages in a C# function, upload a *project.json* file toohello function's folder in hello function app's file system.</span></span> <span data-ttu-id="7a385-161">Här är ett exempel *project.json* -fil som lägger till en referens tooMicrosoft.ProjectOxford.Face version 1.1.0:</span><span class="sxs-lookup"><span data-stu-id="7a385-161">Here is an example *project.json* file that adds a reference tooMicrosoft.ProjectOxford.Face version 1.1.0:</span></span>

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

<span data-ttu-id="7a385-162">Endast hello .NET Framework 4.6 stöds, så se till att din *project.json* filen anger `net46` som visas här.</span><span class="sxs-lookup"><span data-stu-id="7a385-162">Only hello .NET Framework 4.6 is supported, so make sure that your *project.json* file specifies `net46` as shown here.</span></span>

<span data-ttu-id="7a385-163">När du överför en *project.json* fil, hello runtime hämtar hello paket och lägger automatiskt till referenser toohello paketet sammansättningar.</span><span class="sxs-lookup"><span data-stu-id="7a385-163">When you upload a *project.json* file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="7a385-164">Du behöver inte tooadd `#r "AssemblyName"` direktiven.</span><span class="sxs-lookup"><span data-stu-id="7a385-164">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="7a385-165">toouse hello typer som definieras i hello NuGet-paket, lägga till hello krävs `using` instruktioner tooyour *run.csx* fil</span><span class="sxs-lookup"><span data-stu-id="7a385-165">toouse hello types defined in hello NuGet packages, add hello required `using` statements tooyour *run.csx* file</span></span> 

<span data-ttu-id="7a385-166">I hello Functions-runtime NuGet återställningen fungerar genom att jämföra `project.json` och `project.lock.json`.</span><span class="sxs-lookup"><span data-stu-id="7a385-166">In hello Functions runtime, NuGet restore works by comparing `project.json` and `project.lock.json`.</span></span> <span data-ttu-id="7a385-167">Om hello datum och tid stämplar hello filer **inte** matchar, en NuGet-återställning körs och NuGet hämtningsbara filer uppdateras paket.</span><span class="sxs-lookup"><span data-stu-id="7a385-167">If hello date and time stamps of hello files **do not** match, a NuGet restore runs and NuGet downloads updated packages.</span></span> <span data-ttu-id="7a385-168">Emellertid, om hello datum och tid stämplar hello filer **gör** matchar NuGet inte utför en återställning.</span><span class="sxs-lookup"><span data-stu-id="7a385-168">However, if hello date and time stamps of hello files **do** match, NuGet does not perform a restore.</span></span> <span data-ttu-id="7a385-169">Därför `project.lock.json` bör inte användas eftersom det orsakar NuGet tooskip paketet återställning.</span><span class="sxs-lookup"><span data-stu-id="7a385-169">Therefore, `project.lock.json` should not be deployed as it causes NuGet tooskip package restore.</span></span> <span data-ttu-id="7a385-170">distribuera hello Lås tooavoid lägger du till hello `project.lock.json` toohello `.gitignore` fil.</span><span class="sxs-lookup"><span data-stu-id="7a385-170">tooavoid deploying hello lock file, add hello `project.lock.json` toohello `.gitignore` file.</span></span>

<span data-ttu-id="7a385-171">toouse en anpassad NuGet-feed, ange hello mata in en *Nuget.Config* filen i hello Funktionsapp rot.</span><span class="sxs-lookup"><span data-stu-id="7a385-171">toouse a custom NuGet feed, specify hello feed in a *Nuget.Config* file in hello Function App root.</span></span> <span data-ttu-id="7a385-172">Mer information finns i [konfigurera NuGet beteende](/nuget/consume-packages/configuring-nuget-behavior).</span><span class="sxs-lookup"><span data-stu-id="7a385-172">For more information, see [Configuring NuGet behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span>

### <a name="using-a-projectjson-file"></a><span data-ttu-id="7a385-173">Med hjälp av en project.json-fil</span><span class="sxs-lookup"><span data-stu-id="7a385-173">Using a project.json file</span></span>
1. <span data-ttu-id="7a385-174">Öppna hello-funktionen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a385-174">Open hello function in hello Azure portal.</span></span> <span data-ttu-id="7a385-175">hello loggar fliken visar hello paketet installationen utdata.</span><span class="sxs-lookup"><span data-stu-id="7a385-175">hello logs tab displays hello package installation output.</span></span>
2. <span data-ttu-id="7a385-176">tooupload en project.json-fil med någon av hello-metoder som beskrivs i hello [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate) i referensavsnittet om hello Azure Functions-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="7a385-176">tooupload a project.json file, use one of hello methods described in hello [How tooupdate function app files](functions-reference.md#fileupdate) in hello Azure Functions developer reference topic.</span></span>
3. <span data-ttu-id="7a385-177">Efter hello *project.json* överföra filen, så se utdata som liknar hello som följande exempel i din funktion strömning loggen:</span><span class="sxs-lookup"><span data-stu-id="7a385-177">After hello *project.json* file is uploaded, you see output like hello following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="7a385-178">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="7a385-178">Environment variables</span></span>
<span data-ttu-id="7a385-179">tooget en miljövariabel eller en app Inställningsvärdet använda `System.Environment.GetEnvironmentVariable`som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="7a385-179">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, as shown in hello following code example:</span></span>

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

## <a name="reusing-csx-code"></a><span data-ttu-id="7a385-180">Återanvända .csx kod</span><span class="sxs-lookup"><span data-stu-id="7a385-180">Reusing .csx code</span></span>
<span data-ttu-id="7a385-181">Du kan använda klasser och metoder som definierats i andra *.csx* filer i din *run.csx* fil.</span><span class="sxs-lookup"><span data-stu-id="7a385-181">You can use classes and methods defined in other *.csx* files in your *run.csx* file.</span></span> <span data-ttu-id="7a385-182">toodo som använder `#load` direktiven i din *run.csx* fil.</span><span class="sxs-lookup"><span data-stu-id="7a385-182">toodo that, use `#load` directives in your *run.csx* file.</span></span> <span data-ttu-id="7a385-183">I följande exempel hello, en rutin för loggning med namnet `MyLogger` delas i *myLogger.csx* och lästs in i *run.csx* med hello `#load` direktiv:</span><span class="sxs-lookup"><span data-stu-id="7a385-183">In hello following example, a logging routine named `MyLogger` is shared in *myLogger.csx* and loaded into *run.csx* using hello `#load` directive:</span></span>

<span data-ttu-id="7a385-184">Exempel *run.csx*:</span><span class="sxs-lookup"><span data-stu-id="7a385-184">Example *run.csx*:</span></span>

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

<span data-ttu-id="7a385-185">Exempel *mylogger.csx*:</span><span class="sxs-lookup"><span data-stu-id="7a385-185">Example *mylogger.csx*:</span></span>

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

<span data-ttu-id="7a385-186">Med hjälp av en delad *.csx* är ett vanligt mönster om du vill toostrongly din typargument mellan funktioner med hjälp av en POCO-objektet.</span><span class="sxs-lookup"><span data-stu-id="7a385-186">Using a shared *.csx* is a common pattern when you want toostrongly type your arguments between functions using a POCO object.</span></span> <span data-ttu-id="7a385-187">I följande exempel förenklad hello, en HTTP-utlösaren och kö utlösaren delar en POCO-objekt med namnet `Order` toostrongly typen hello ordning data:</span><span class="sxs-lookup"><span data-stu-id="7a385-187">In hello following simplified example, an HTTP trigger and queue trigger share a POCO object named `Order` toostrongly type hello order data:</span></span>

<span data-ttu-id="7a385-188">Exempel *run.csx* för HTTP-utlösare:</span><span class="sxs-lookup"><span data-stu-id="7a385-188">Example *run.csx* for HTTP trigger:</span></span>

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

<span data-ttu-id="7a385-189">Exempel *run.csx* för kön utlösare:</span><span class="sxs-lookup"><span data-stu-id="7a385-189">Example *run.csx* for queue trigger:</span></span>

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

<span data-ttu-id="7a385-190">Exempel *order.csx*:</span><span class="sxs-lookup"><span data-stu-id="7a385-190">Example *order.csx*:</span></span>

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

<span data-ttu-id="7a385-191">Du kan använda en relativ sökväg med hello `#load` direktiv:</span><span class="sxs-lookup"><span data-stu-id="7a385-191">You can use a relative path with hello `#load` directive:</span></span>

* <span data-ttu-id="7a385-192">`#load "mylogger.csx"`läser in en fil i mappen för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="7a385-192">`#load "mylogger.csx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="7a385-193">`#load "loadedfiles\mylogger.csx"`läser in en fil i en mapp i hello funktionen mapp.</span><span class="sxs-lookup"><span data-stu-id="7a385-193">`#load "loadedfiles\mylogger.csx"` loads a file located in a folder in hello function folder.</span></span>
* <span data-ttu-id="7a385-194">`#load "..\shared\mylogger.csx"`läser in en fil i en mapp på samma nivå som hello funktionen mapp, det vill säga hello direkt under *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="7a385-194">`#load "..\shared\mylogger.csx"` loads a file located in a folder at hello same level as hello function folder, that is, directly under *wwwroot*.</span></span>

<span data-ttu-id="7a385-195">Hej `#load` direktiv fungerar bara med *.csx* (C#) skriptfiler inte med *.cs* filer.</span><span class="sxs-lookup"><span data-stu-id="7a385-195">hello `#load` directive works only with *.csx* (C# script) files, not with *.cs* files.</span></span>

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a><span data-ttu-id="7a385-196">Bindning under körning via tvingande bindningar</span><span class="sxs-lookup"><span data-stu-id="7a385-196">Binding at runtime via imperative bindings</span></span>

<span data-ttu-id="7a385-197">I C# och andra .NET-språk, kan du använda en [tvingande](https://en.wikipedia.org/wiki/Imperative_programming) bindning mönster som skillnad från toohello [ *deklarativa* ](https://en.wikipedia.org/wiki/Declarative_programming) bindningar i *function.json*.</span><span class="sxs-lookup"><span data-stu-id="7a385-197">In C# and other .NET languages, you can use an [imperative](https://en.wikipedia.org/wiki/Imperative_programming) binding pattern, as opposed toohello [*declarative*](https://en.wikipedia.org/wiki/Declarative_programming) bindings in *function.json*.</span></span> <span data-ttu-id="7a385-198">Tvingande bindning är användbart när bindande parametrar behöver toobe beräknade körning i stället för design samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7a385-198">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="7a385-199">Med det här mönstret du binda toosupported indata och utdata bindning på direkt i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="7a385-199">With this pattern, you can bind toosupported input and output binding on-the-fly in your function code.</span></span>

<span data-ttu-id="7a385-200">Definiera en tvingande bindning på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7a385-200">Define an imperative binding as follows:</span></span>

- <span data-ttu-id="7a385-201">**Inte** innehåller en post i *function.json* för din önskade tvingande bindningar.</span><span class="sxs-lookup"><span data-stu-id="7a385-201">**Do not** include an entry in *function.json* for your desired imperative bindings.</span></span>
- <span data-ttu-id="7a385-202">Använd en indataparameter [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) eller [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span><span class="sxs-lookup"><span data-stu-id="7a385-202">Pass in an input parameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) or [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span></span>
- <span data-ttu-id="7a385-203">Använd hello följande C# mönster tooperform hello databindning.</span><span class="sxs-lookup"><span data-stu-id="7a385-203">Use hello following C# pattern tooperform hello data binding.</span></span>

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

<span data-ttu-id="7a385-204">där `BindingTypeAttribute` är hello .NET attribut som definierar en bindning och `T` är hello inkommande eller utgående typ som stöds av denna bindning.</span><span class="sxs-lookup"><span data-stu-id="7a385-204">where `BindingTypeAttribute` is hello .NET attribute that defines your binding and `T` is hello input or output type that's supported by that binding type.</span></span> <span data-ttu-id="7a385-205">`T`kan inte heller ett `out` parametertypen (exempelvis `out JObject`).</span><span class="sxs-lookup"><span data-stu-id="7a385-205">`T` also cannot be an `out` parameter type (such as `out JObject`).</span></span> <span data-ttu-id="7a385-206">Till exempel tabellen Mobile Apps spara bindningen stöder [sex utdata typer](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), men du kan bara använda [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) eller [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) för `T`.</span><span class="sxs-lookup"><span data-stu-id="7a385-206">For example, the Mobile Apps table output binding supports [six output types](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), but you can only use [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) for `T`.</span></span>

<span data-ttu-id="7a385-207">Följande exempelkod hello skapar en [lagringsblob utdatabindning](functions-bindings-storage-blob.md#using-a-blob-output-binding) med blob sökväg som har definierats vid körning, sedan skriver en sträng toohello blob.</span><span class="sxs-lookup"><span data-stu-id="7a385-207">hello following example code creates a [Storage blob output binding](functions-bindings-storage-blob.md#using-a-blob-output-binding) with blob path that's defined at run time, then writes a string toohello blob.</span></span>

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

<span data-ttu-id="7a385-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definierar hello [lagringsblob](functions-bindings-storage-blob.md) indata och utdata bindning, och [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) är av bindningstypen stöds utdata.</span><span class="sxs-lookup"><span data-stu-id="7a385-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) defines hello [Storage blob](functions-bindings-storage-blob.md) input or output binding, and [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is a supported output binding type.</span></span>
<span data-ttu-id="7a385-209">Eftersom, hello kod hämtar hello app standardinställningen för hello anslutningssträngen för lagring konto (vilket är `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="7a385-209">As is, hello code gets hello default app setting for hello Storage account connection string (which is `AzureWebJobsStorage`).</span></span> <span data-ttu-id="7a385-210">Du kan ange en anpassad app inställningen toouse genom att lägga till den [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) och passerar hello attributet matris i `BindAsync<T>()`.</span><span class="sxs-lookup"><span data-stu-id="7a385-210">You can specify a custom app setting toouse by adding the [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) and passing hello attribute array into `BindAsync<T>()`.</span></span> <span data-ttu-id="7a385-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7a385-211">For example,</span></span>

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

<span data-ttu-id="7a385-212">hello visar följande tabell hello .NET attribut för varje bindning typ och hello paket som de har definierats.</span><span class="sxs-lookup"><span data-stu-id="7a385-212">hello following table lists hello .NET attributes for each binding type and hello packages in which they are defined.</span></span>

> [!div class="mx-codeBreakAll"]
| <span data-ttu-id="7a385-213">Bindning</span><span class="sxs-lookup"><span data-stu-id="7a385-213">Binding</span></span> | <span data-ttu-id="7a385-214">Attribut</span><span class="sxs-lookup"><span data-stu-id="7a385-214">Attribute</span></span> | <span data-ttu-id="7a385-215">Lägg till referens</span><span class="sxs-lookup"><span data-stu-id="7a385-215">Add reference</span></span> |
|------|------|------|
| <span data-ttu-id="7a385-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7a385-216">Cosmos DB</span></span> | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| <span data-ttu-id="7a385-217">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="7a385-217">Event Hubs</span></span> | <span data-ttu-id="7a385-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7a385-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| <span data-ttu-id="7a385-219">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="7a385-219">Mobile Apps</span></span> | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| <span data-ttu-id="7a385-220">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="7a385-220">Notification Hubs</span></span> | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| <span data-ttu-id="7a385-221">Service Bus</span><span class="sxs-lookup"><span data-stu-id="7a385-221">Service Bus</span></span> | <span data-ttu-id="7a385-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7a385-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| <span data-ttu-id="7a385-223">Storage-kö</span><span class="sxs-lookup"><span data-stu-id="7a385-223">Storage queue</span></span> | <span data-ttu-id="7a385-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7a385-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7a385-225">Lagringsblob</span><span class="sxs-lookup"><span data-stu-id="7a385-225">Storage blob</span></span> | <span data-ttu-id="7a385-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7a385-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7a385-227">Tabell för lagring</span><span class="sxs-lookup"><span data-stu-id="7a385-227">Storage table</span></span> | <span data-ttu-id="7a385-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7a385-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7a385-229">Twilio</span><span class="sxs-lookup"><span data-stu-id="7a385-229">Twilio</span></span> | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a><span data-ttu-id="7a385-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a385-230">Next steps</span></span>
<span data-ttu-id="7a385-231">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="7a385-231">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="7a385-232">Metodtips för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="7a385-232">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="7a385-233">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="7a385-233">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="7a385-234">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="7a385-234">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="7a385-235">Azure Functions NodeJS för utvecklare</span><span class="sxs-lookup"><span data-stu-id="7a385-235">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="7a385-236">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="7a385-236">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

[värden\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
