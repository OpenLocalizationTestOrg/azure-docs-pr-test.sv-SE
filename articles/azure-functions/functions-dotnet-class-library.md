---
title: aaaUsing .NET klassen bibliotek med Azure Functions | Microsoft Docs
description: "Lär dig hur tooauthor .NET-klassbibliotek för hjälp med Azure Functions"
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/09/2017
ms.author: donnam
ms.openlocfilehash: 4e0fd954b554006ba1d8ecc47403a9fb1c67c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a><span data-ttu-id="3fb12-104">Med hjälp av .NET-klassbibliotek med Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3fb12-104">Using .NET class libraries with Azure Functions</span></span>

<span data-ttu-id="3fb12-105">Dessutom stöder tooscript filer, Azure Functions publicerar en klassbiblioteket som hello implementering för en eller flera funktioner.</span><span class="sxs-lookup"><span data-stu-id="3fb12-105">In addition tooscript files, Azure Functions supports publishing a class library as hello implementation for one or more functions.</span></span> <span data-ttu-id="3fb12-106">Vi rekommenderar att du använder hello [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="3fb12-106">We recommend that you use hello [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fb12-107">Krav</span><span class="sxs-lookup"><span data-stu-id="3fb12-107">Prerequisites</span></span> 

<span data-ttu-id="3fb12-108">Den här artikeln har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="3fb12-108">This article has hello following prerequisites:</span></span>

- <span data-ttu-id="3fb12-109">[Visual Studio 2017 15,3 Preview](https://www.visualstudio.com/vs/preview/).</span><span class="sxs-lookup"><span data-stu-id="3fb12-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span></span> <span data-ttu-id="3fb12-110">Installera hello arbetsbelastningar **ASP.NET och web development** och **Azure-utveckling**.</span><span class="sxs-lookup"><span data-stu-id="3fb12-110">Install hello workloads **ASP.NET and web development** and **Azure development**.</span></span>
- [<span data-ttu-id="3fb12-111">Azure funktionen Verktyg för Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3fb12-111">Azure Function Tools for Visual Studio 2017</span></span>](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a><span data-ttu-id="3fb12-112">Funktioner för klassbiblioteksprojektet</span><span class="sxs-lookup"><span data-stu-id="3fb12-112">Functions class library project</span></span>

<span data-ttu-id="3fb12-113">Skapa ett nytt Azure Functions-projekt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3fb12-113">From Visual Studio, create a new Azure Functions project.</span></span> <span data-ttu-id="3fb12-114">hello ny projektmall skapar hello filer *host.json* och *local.settings.json*.</span><span class="sxs-lookup"><span data-stu-id="3fb12-114">hello new project template creates hello files *host.json* and *local.settings.json*.</span></span> <span data-ttu-id="3fb12-115">Du kan [Anpassa inställningar för Azure Functions-runtime i host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="3fb12-115">You can [customize Azure Functions runtime settings in host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span> 

<span data-ttu-id="3fb12-116">hello filen *local.settings.json* lagrar app-inställningar, anslutningssträngar och inställningar för Azure Functions Core verktyg.</span><span class="sxs-lookup"><span data-stu-id="3fb12-116">hello file *local.settings.json* stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="3fb12-117">toolearn mer om dess struktur finns [koden och testa Azure functions lokalt](functions-run-local.md#local-settings).</span><span class="sxs-lookup"><span data-stu-id="3fb12-117">toolearn more about its structure, see [Code and test Azure functions locally](functions-run-local.md#local-settings).</span></span>

### <a name="functionname-attribute"></a><span data-ttu-id="3fb12-118">FunctionName attribut</span><span class="sxs-lookup"><span data-stu-id="3fb12-118">FunctionName attribute</span></span>

<span data-ttu-id="3fb12-119">hello attributet [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) markerar en metod som en startpunkt för funktionen.</span><span class="sxs-lookup"><span data-stu-id="3fb12-119">hello attribute [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) marks a method as a function entry point.</span></span> <span data-ttu-id="3fb12-120">Den måste användas med en utlösare och 0 eller fler indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="3fb12-120">It must be used with exactly one trigger and 0 or more input and output bindings.</span></span>

### <a name="conversion-toofunctionjson"></a><span data-ttu-id="3fb12-121">Konvertering toofunction.json</span><span class="sxs-lookup"><span data-stu-id="3fb12-121">Conversion toofunction.json</span></span>

<span data-ttu-id="3fb12-122">När du har skapat ett Azure Functions-projekt, ger en fil `function.json` i hello directory matchar hello funktionsnamn definieras av `[FunctionName]`.</span><span class="sxs-lookup"><span data-stu-id="3fb12-122">When an Azure Functions project is built, it produces a file `function.json` in hello directory matching hello function name defined by `[FunctionName]`.</span></span> <span data-ttu-id="3fb12-123">Det anger utlösare och bindningar och punkter toohello projektfilen sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="3fb12-123">It specifies triggers and bindings and points toohello project assembly file.</span></span>

<span data-ttu-id="3fb12-124">Den här konverteringen utförs av hello NuGet-paketet [Microsoft\.NET\.Sdk\.funktioner](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span><span class="sxs-lookup"><span data-stu-id="3fb12-124">This conversion is performed by hello NuGet package [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span></span> <span data-ttu-id="3fb12-125">hello källan är tillgänglig i GitHub-repo-hello [azure\-funktioner\-vs\-skapa\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span><span class="sxs-lookup"><span data-stu-id="3fb12-125">hello source is available in hello GitHub repo [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span></span>

## <a name="triggers-and-bindings"></a><span data-ttu-id="3fb12-126">Utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="3fb12-126">Triggers and bindings</span></span>

<span data-ttu-id="3fb12-127">hello följande tabell visas hello utlösare och bindningar som är tillgängliga i en Azure Functions klassbiblioteksprojektet.</span><span class="sxs-lookup"><span data-stu-id="3fb12-127">hello following table lists hello triggers and bindings that are available in an Azure Functions class library project.</span></span> <span data-ttu-id="3fb12-128">Alla attribut är i hello namnområdet `Microsoft.Azure.WebJobs`.</span><span class="sxs-lookup"><span data-stu-id="3fb12-128">All attributes are in hello namespace `Microsoft.Azure.WebJobs`.</span></span>

| <span data-ttu-id="3fb12-129">Bindning</span><span class="sxs-lookup"><span data-stu-id="3fb12-129">Binding</span></span> | <span data-ttu-id="3fb12-130">Attribut</span><span class="sxs-lookup"><span data-stu-id="3fb12-130">Attribute</span></span> | <span data-ttu-id="3fb12-131">NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="3fb12-131">NuGet package</span></span> |
|------   | ------    | ------        |
| [<span data-ttu-id="3fb12-132">BLOB storage-utlösare, indata, utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-132">Blob storage trigger, input, output</span></span>](#blob-storage) | <span data-ttu-id="3fb12-133">[BlobAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-133">[BlobAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="3fb12-134">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="3fb12-134">[Microsoft.Azure.WebJobs]</span></span> | <span data-ttu-id="3fb12-135">[Blobblagring]</span><span class="sxs-lookup"><span data-stu-id="3fb12-135">[Blob storage]</span></span> |
| [<span data-ttu-id="3fb12-136">Cosmos DB indata och utdata bindning</span><span class="sxs-lookup"><span data-stu-id="3fb12-136">Cosmos DB input and output binding</span></span>](#cosmos-db) | <span data-ttu-id="3fb12-137">[DocumentDBAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-137">[DocumentDBAttribute]</span></span> | <span data-ttu-id="3fb12-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span><span class="sxs-lookup"><span data-stu-id="3fb12-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span></span> | 
| [<span data-ttu-id="3fb12-139">Händelseutlösare NAV- och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-139">Event Hubs trigger and output</span></span>](#event-hub) | <span data-ttu-id="3fb12-140">[EventHubTriggerAttribute], [EventHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-140">[EventHubTriggerAttribute], [EventHubAttribute]</span></span> | <span data-ttu-id="3fb12-141">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="3fb12-141">[Microsoft.Azure.WebJobs.ServiceBus]</span></span> |
| [<span data-ttu-id="3fb12-142">Den externa filen inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-142">External file input and output</span></span>](#api-hub) | <span data-ttu-id="3fb12-143">[ApiHubFileAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-143">[ApiHubFileAttribute]</span></span> | <span data-ttu-id="3fb12-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span><span class="sxs-lookup"><span data-stu-id="3fb12-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span></span> |
| [<span data-ttu-id="3fb12-145">HTTP- och webhook-utlösare</span><span class="sxs-lookup"><span data-stu-id="3fb12-145">HTTP and webhook trigger</span></span>](#http) | <span data-ttu-id="3fb12-146">[HttpTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-146">[HttpTriggerAttribute]</span></span> | <span data-ttu-id="3fb12-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span><span class="sxs-lookup"><span data-stu-id="3fb12-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span></span> |
| [<span data-ttu-id="3fb12-148">Mobile Apps inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-148">Mobile Apps input and output</span></span>](#mobile-apps) | <span data-ttu-id="3fb12-149">[MobileTableAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-149">[MobileTableAttribute]</span></span> | <span data-ttu-id="3fb12-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span><span class="sxs-lookup"><span data-stu-id="3fb12-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span></span> | 
| [<span data-ttu-id="3fb12-151">Notification Hubs utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-151">Notification Hubs output</span></span>](#nh) | <span data-ttu-id="3fb12-152">[NotificationHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-152">[NotificationHubAttribute]</span></span> | <span data-ttu-id="3fb12-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span><span class="sxs-lookup"><span data-stu-id="3fb12-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span></span> | 
| [<span data-ttu-id="3fb12-154">Queue storage utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-154">Queue storage trigger and output</span></span>](#queue) | <span data-ttu-id="3fb12-155">[QueueAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-155">[QueueAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="3fb12-156">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="3fb12-156">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="3fb12-157">SendGrid-utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-157">SendGrid output</span></span>](#sendgrid) | <span data-ttu-id="3fb12-158">[SendGridAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-158">[SendGridAttribute]</span></span> | <span data-ttu-id="3fb12-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span><span class="sxs-lookup"><span data-stu-id="3fb12-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span></span> | 
| [<span data-ttu-id="3fb12-160">Service Bus-utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-160">Service Bus trigger and output</span></span>](#service-bus) | <span data-ttu-id="3fb12-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span></span> | <span data-ttu-id="3fb12-162">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="3fb12-162">[Microsoft.Azure.WebJobs.ServiceBus]</span></span>
| [<span data-ttu-id="3fb12-163">Table storage inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-163">Table storage input and output</span></span>](#table) | <span data-ttu-id="3fb12-164">[TableAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-164">[TableAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="3fb12-165">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="3fb12-165">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="3fb12-166">Timerutlösare</span><span class="sxs-lookup"><span data-stu-id="3fb12-166">Timer trigger</span></span>](#timer) | <span data-ttu-id="3fb12-167">[TimerTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-167">[TimerTriggerAttribute]</span></span> | <span data-ttu-id="3fb12-168">[Microsoft.Azure.WebJobs.Extensions]</span><span class="sxs-lookup"><span data-stu-id="3fb12-168">[Microsoft.Azure.WebJobs.Extensions]</span></span> | 
| [<span data-ttu-id="3fb12-169">Twilio-utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-169">Twilio output</span></span>](#twilio) | <span data-ttu-id="3fb12-170">[TwilioSmsAttribute]</span><span class="sxs-lookup"><span data-stu-id="3fb12-170">[TwilioSmsAttribute]</span></span> | <span data-ttu-id="3fb12-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span><span class="sxs-lookup"><span data-stu-id="3fb12-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span></span> | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a><span data-ttu-id="3fb12-172">BLOB storage utlösaren indata och utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="3fb12-172">Blob storage trigger, input, and output bindings</span></span>

<span data-ttu-id="3fb12-173">Azure Functions stöder utlösaren indata och utdata bindningar för Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3fb12-173">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="3fb12-174">Mer information om bindande uttryck och metadata finns [Azure Functions Blob storage bindningar](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-174">For more information on binding expressions and metadata, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>

<span data-ttu-id="3fb12-175">En blob-utlösare har definierats med hello `[BlobTrigger]` attribut.</span><span class="sxs-lookup"><span data-stu-id="3fb12-175">A blob trigger is defined with hello `[BlobTrigger]` attribute.</span></span> <span data-ttu-id="3fb12-176">Du kan använda hello attributet `[StorageAccount]` toodefine hello storage-konto som används av ett hela funktion eller en klass.</span><span class="sxs-lookup"><span data-stu-id="3fb12-176">You can use hello attribute `[StorageAccount]` toodefine hello storage account that is used by an entire function or class.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

<span data-ttu-id="3fb12-177">BLOB som indata och utdata har definierats med hello `[Blob]` attributet tillsammans med en `FileAccess` parameter som anger att läsa eller skriva.</span><span class="sxs-lookup"><span data-stu-id="3fb12-177">Blob input and output are defined using hello `[Blob]` attribute, along with a `FileAccess` parameter indicating read or write.</span></span> <span data-ttu-id="3fb12-178">Hej följande exempel används en blob-utlösare och blob utdata bindning.</span><span class="sxs-lookup"><span data-stu-id="3fb12-178">hello following example uses a blob trigger and blob output binding.</span></span>

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-input-and-output-bindings"></a><span data-ttu-id="3fb12-179">Cosmos DB indata och utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="3fb12-179">Cosmos DB input and output bindings</span></span>

<span data-ttu-id="3fb12-180">Azure Functions stöder indata och utdata bindningar för Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3fb12-180">Azure Functions supports input and output bindings for Cosmos DB.</span></span> <span data-ttu-id="3fb12-181">toolearn mer om hello funktioner för hello Cosmos DB bindning finns [Azure Functions Cosmos DB bindningar](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-181">toolearn more about hello features of hello Cosmos DB binding, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>

<span data-ttu-id="3fb12-182">toobind tooa Cosmos-DB-dokumentet använder hello attributet `[DocumentDB]` i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span><span class="sxs-lookup"><span data-stu-id="3fb12-182">toobind tooa Cosmos DB document, use hello attribute `[DocumentDB]` in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span></span> <span data-ttu-id="3fb12-183">hello följande exempel har en kö-utlösare och ett DocumentDB-API-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="3fb12-183">hello following example has a queue trigger and a DocumentDB API output binding:</span></span>

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "DocDBConnection")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a><span data-ttu-id="3fb12-184">Händelseutlösare NAV- och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-184">Event Hubs trigger and output</span></span>

<span data-ttu-id="3fb12-185">Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="3fb12-185">Azure Functions supports trigger and output bindings for Event Hubs.</span></span> <span data-ttu-id="3fb12-186">Mer information finns i [Azure Functions Event Hub bindningar](functions-bindings-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-186">For more information, see  [Azure Functions Event Hub bindings](functions-bindings-event-hubs.md).</span></span>

<span data-ttu-id="3fb12-187">Hej typer `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` och `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="3fb12-187">hello types `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` and `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="3fb12-188">hello används följande exempel en Event Hub-utlösare:</span><span class="sxs-lookup"><span data-stu-id="3fb12-188">hello following example uses an Event Hub trigger:</span></span>

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="3fb12-189">hello har följande exempel en Händelsehubb utdata, med hjälp av hello metoden returnerade värdet som hello utdata:</span><span class="sxs-lookup"><span data-stu-id="3fb12-189">hello following example has an Event Hub output, using hello method return value as hello output:</span></span>

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a><span data-ttu-id="3fb12-190">Den externa filen inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-190">External file input and output</span></span>

<span data-ttu-id="3fb12-191">Azure Functions stöder utlösaren indata och utdata bindningar för externa filer, till exempel Google Drive, Dropbox och OneDrive.</span><span class="sxs-lookup"><span data-stu-id="3fb12-191">Azure Functions supports trigger, input, and output bindings for external files, such as Google Drive, Dropbox, and OneDrive.</span></span> <span data-ttu-id="3fb12-192">Det finns fler toolearn [Azure Functions extern fil bindningar](functions-bindings-external-file.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-192">toolearn more, see [Azure Functions External File bindings](functions-bindings-external-file.md).</span></span> <span data-ttu-id="3fb12-193">Hej attribut `[ExternalFileTrigger]` och `[ExternalFile]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span><span class="sxs-lookup"><span data-stu-id="3fb12-193">hello attributes `[ExternalFileTrigger]` and `[ExternalFile]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span></span>

<span data-ttu-id="3fb12-194">hello följande C#-exempel visar en extern fil indata och utdata bindning.</span><span class="sxs-lookup"><span data-stu-id="3fb12-194">hello following C# example demonstrates an external file input and output binding.</span></span> <span data-ttu-id="3fb12-195">hello kod kopior hello indatafilen toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="3fb12-195">hello code copies hello input file toohello output file.</span></span>

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a><span data-ttu-id="3fb12-196">HTTP och webhooks</span><span class="sxs-lookup"><span data-stu-id="3fb12-196">HTTP and webhooks</span></span>

<span data-ttu-id="3fb12-197">Använd hello `HttpTrigger` attributet toodefine en HTTP-utlösare eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="3fb12-197">Use hello `HttpTrigger` attribute toodefine an HTTP trigger or webhook.</span></span> <span data-ttu-id="3fb12-198">Det här attributet definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.Http].</span><span class="sxs-lookup"><span data-stu-id="3fb12-198">This attribute is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.Http].</span></span> <span data-ttu-id="3fb12-199">Du kan anpassa hello åtkomstnivå, webhook-typ, väg och metoder.</span><span class="sxs-lookup"><span data-stu-id="3fb12-199">You can customize hello authorization level, webhook type, route, and methods.</span></span> <span data-ttu-id="3fb12-200">hello följande exempel definierar en HTTP-utlösare med anonym autentisering och _genericJson_ webhook-typen.</span><span class="sxs-lookup"><span data-stu-id="3fb12-200">hello following example defines an HTTP trigger with anonymous authentication and _genericJson_ webhook type.</span></span>

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a><span data-ttu-id="3fb12-201">Mobile Apps inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-201">Mobile Apps input and output</span></span>

<span data-ttu-id="3fb12-202">Azure Functions stöder indata och utdata bindningar för Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="3fb12-202">Azure Functions supports input and output bindings for Mobile Apps.</span></span> <span data-ttu-id="3fb12-203">Det finns fler toolearn [Azure Functions Mobile Apps bindningar](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-203">toolearn more, see [Azure Functions Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span> <span data-ttu-id="3fb12-204">hello attributet `[MobileTable]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span><span class="sxs-lookup"><span data-stu-id="3fb12-204">hello attribute `[MobileTable]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span></span>

<span data-ttu-id="3fb12-205">hello exemplet nedan visar en Mobile Apps-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="3fb12-205">hello following example demonstrates a Mobile Apps output binding:</span></span>

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a><span data-ttu-id="3fb12-206">Notification Hubs utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-206">Notification Hubs output</span></span>

<span data-ttu-id="3fb12-207">Azure Functions stöder en output-bindning för Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="3fb12-207">Azure Functions supports an output binding for Notification Hubs.</span></span> <span data-ttu-id="3fb12-208">Det finns fler toolearn [Azure Functions Notification Hub-utdatabindning](functions-bindings-notification-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-208">toolearn more, see [Azure Functions Notification Hub output binding](functions-bindings-notification-hubs.md).</span></span> <span data-ttu-id="3fb12-209">hello attributet `[NotificationHub]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span><span class="sxs-lookup"><span data-stu-id="3fb12-209">hello attribute `[NotificationHub]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span></span>

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a><span data-ttu-id="3fb12-210">Queue storage utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-210">Queue storage trigger and output</span></span>

<span data-ttu-id="3fb12-211">Azure Functions stöder utlösa och utgående bindningar för Azure köer.</span><span class="sxs-lookup"><span data-stu-id="3fb12-211">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="3fb12-212">Mer information finns i [Azure Functions Queue Storage bindningar](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-212">For more information, see [Azure Functions Queue Storage bindings](functions-bindings-storage-queue.md).</span></span>

<span data-ttu-id="3fb12-213">hello följande exempel visas hur toouse hello funktionen returtyp med en kö utdata bindning, med hello `[Queue]` attribut.</span><span class="sxs-lookup"><span data-stu-id="3fb12-213">hello following example shows how toouse hello function return type with a queue output binding, using hello `[Queue]` attribute.</span></span> <span data-ttu-id="3fb12-214">toodefine en kö-utlösare, Använd hello `[QueueTrigger]` attribut.</span><span class="sxs-lookup"><span data-stu-id="3fb12-214">toodefine a queue trigger, use hello `[QueueTrigger]` attribute.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }

    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```

<a name="sendgrid"></a>

### <a name="sendgrid-output"></a><span data-ttu-id="3fb12-215">SendGrid-utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-215">SendGrid output</span></span>

<span data-ttu-id="3fb12-216">Azure Functions stöder en SendGrid utdata bindning för att skicka e-post via programmering.</span><span class="sxs-lookup"><span data-stu-id="3fb12-216">Azure Functions supports a SendGrid output binding for sending email programmatically.</span></span> <span data-ttu-id="3fb12-217">Det finns fler toolearn [Azure Functions SendGrid bindningar](functions-bindings-sendgrid.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-217">toolearn more, see [Azure Functions SendGrid bindings](functions-bindings-sendgrid.md).</span></span>

<span data-ttu-id="3fb12-218">hello attributet `[SendGrid]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span><span class="sxs-lookup"><span data-stu-id="3fb12-218">hello attribute `[SendGrid]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span></span>

<span data-ttu-id="3fb12-219">hello följande är ett exempel på med en utlösare för Service Bus-kö och en SendGrid utdata bindning med `SendGridMessage`:</span><span class="sxs-lookup"><span data-stu-id="3fb12-219">hello following is an example of using a Service Bus queue trigger and a SendGrid output binding using `SendGridMessage`:</span></span>

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string too{ get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a><span data-ttu-id="3fb12-220">Service Bus-utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-220">Service Bus trigger and output</span></span>

<span data-ttu-id="3fb12-221">Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="3fb12-221">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span> <span data-ttu-id="3fb12-222">Mer information om hur du konfigurerar bindningar finns [Azure Functions Service Bus-bindningar](functions-bindings-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-222">For more information on configuring bindings, see [Azure Functions Service Bus bindings](functions-bindings-service-bus.md).</span></span>

<span data-ttu-id="3fb12-223">Hej attribut `[ServiceBusTrigger]` och `[ServiceBus]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="3fb12-223">hello attributes `[ServiceBusTrigger]` and `[ServiceBus]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="3fb12-224">hello följande är ett exempel på en utlösare för Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="3fb12-224">hello following is an example of a Service Bus queue trigger:</span></span>

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<span data-ttu-id="3fb12-225">hello följande är ett exempel på en Service Bus-utdata bindning, med hello-metodens returtyp som hello utdata:</span><span class="sxs-lookup"><span data-stu-id="3fb12-225">hello following is an example of a Service Bus output binding, using hello method return type as hello output:</span></span>

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a><span data-ttu-id="3fb12-226">Table storage inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="3fb12-226">Table storage input and output</span></span>

<span data-ttu-id="3fb12-227">Azure Functions stöder indata och utdata bindningar för Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="3fb12-227">Azure Functions supports input and output bindings for Azure Table storage.</span></span> <span data-ttu-id="3fb12-228">Det finns fler toolearn [Azure Functions Table storage bindningar](functions-bindings-storage-table.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-228">toolearn more, see [Azure Functions Table storage bindings](functions-bindings-storage-table.md).</span></span>

<span data-ttu-id="3fb12-229">hello är följande exempel en klass med två funktioner som visar tabellen lagring utgående och inkommande bindningar.</span><span class="sxs-lookup"><span data-stu-id="3fb12-229">hello following example is a class with two functions, demonstrating table storage output and input bindings.</span></span> 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use hello metadata parameter "queueTrigger" toobind hello queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a><span data-ttu-id="3fb12-230">Timerutlösare</span><span class="sxs-lookup"><span data-stu-id="3fb12-230">Timer trigger</span></span>

<span data-ttu-id="3fb12-231">Azure Functions har en timer utlösaren bindning som låter dig köra din funktionskod baserat på ett definierat schema.</span><span class="sxs-lookup"><span data-stu-id="3fb12-231">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> <span data-ttu-id="3fb12-232">toolearn mer om hello funktioner för hello bindning finns [schemalägga kodkörning med Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-232">toolearn more about hello features of hello binding, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>

<span data-ttu-id="3fb12-233">Hello förbrukning planen kan du definiera scheman med en [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span><span class="sxs-lookup"><span data-stu-id="3fb12-233">On hello Consumption plan, you can define schedules with a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span></span> <span data-ttu-id="3fb12-234">Om du använder en App Service-Plan kan använda du också en TimeSpan-sträng.</span><span class="sxs-lookup"><span data-stu-id="3fb12-234">If you're using an App Service Plan, you can also use a TimeSpan string.</span></span> 

<span data-ttu-id="3fb12-235">hello följande exempel definierar en timer som utlösare som körs var femte minut:</span><span class="sxs-lookup"><span data-stu-id="3fb12-235">hello following example defines a timer trigger that runs every 5 minutes:</span></span>

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a><span data-ttu-id="3fb12-236">Twilio-utdata</span><span class="sxs-lookup"><span data-stu-id="3fb12-236">Twilio output</span></span>

<span data-ttu-id="3fb12-237">Azure Functions stöder Twilio utdata bindningar tooenable funktioner för toosend SMS textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="3fb12-237">Azure Functions supports Twilio output bindings tooenable your functions toosend SMS text messages.</span></span> <span data-ttu-id="3fb12-238">Det finns fler toolearn [skicka SMS-meddelanden från Azure-funktioner med hjälp av hello Twilio-utdatabindning](functions-bindings-twilio.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-238">toolearn more, see [Send SMS messages from Azure Functions using hello Twilio output binding](functions-bindings-twilio.md).</span></span> 

<span data-ttu-id="3fb12-239">hello attributet `[TwilioSms]` har definierats i hello paketet [Microsoft.Azure.WebJobs.Extensions.Twilio].</span><span class="sxs-lookup"><span data-stu-id="3fb12-239">hello attribute `[TwilioSms]` is defined in hello package [Microsoft.Azure.WebJobs.Extensions.Twilio].</span></span>

<span data-ttu-id="3fb12-240">hello följande C# exempel används en kö utlösare och en Twilio-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="3fb12-240">hello following C# example uses a queue trigger and a Twilio output binding:</span></span>

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        too= order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a><span data-ttu-id="3fb12-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3fb12-241">Next steps</span></span>

<span data-ttu-id="3fb12-242">Mer information om hur du använder Azure Functions i C#-skript finns [Azure Functions C\# skript för utvecklare](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3fb12-242">For more information on using Azure Functions in C# scripting, see [Azure Functions C\# script developer reference](functions-reference-csharp.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links toosource --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs
