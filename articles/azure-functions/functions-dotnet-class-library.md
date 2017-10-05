---
title: "Med hjälp av .NET-klassbibliotek med Azure Functions | Microsoft Docs"
description: "Lär dig hur du skapar .NET-klassbibliotek för användning med Azure Functions"
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
ms.openlocfilehash: 0613bb96d3afb85ff7e684246b128e4eef518d23
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a><span data-ttu-id="f0e00-104">Med hjälp av .NET-klassbibliotek med Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f0e00-104">Using .NET class libraries with Azure Functions</span></span>

<span data-ttu-id="f0e00-105">Azure Functions stöder publicerar en klassbiblioteket som implementeringen för en eller flera funktioner utöver skriptfiler.</span><span class="sxs-lookup"><span data-stu-id="f0e00-105">In addition to script files, Azure Functions supports publishing a class library as the implementation for one or more functions.</span></span> <span data-ttu-id="f0e00-106">Vi rekommenderar att du använder den [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="f0e00-106">We recommend that you use the [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0e00-107">Krav</span><span class="sxs-lookup"><span data-stu-id="f0e00-107">Prerequisites</span></span> 

<span data-ttu-id="f0e00-108">Den här artikeln har följande krav:</span><span class="sxs-lookup"><span data-stu-id="f0e00-108">This article has the following prerequisites:</span></span>

- <span data-ttu-id="f0e00-109">[Visual Studio 2017 15,3 Preview](https://www.visualstudio.com/vs/preview/).</span><span class="sxs-lookup"><span data-stu-id="f0e00-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span></span> <span data-ttu-id="f0e00-110">Installera arbetsbelastningarna **ASP.NET och web development** och **Azure-utveckling**.</span><span class="sxs-lookup"><span data-stu-id="f0e00-110">Install the workloads **ASP.NET and web development** and **Azure development**.</span></span>
- [<span data-ttu-id="f0e00-111">Azure funktionen Verktyg för Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f0e00-111">Azure Function Tools for Visual Studio 2017</span></span>](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a><span data-ttu-id="f0e00-112">Funktioner för klassbiblioteksprojektet</span><span class="sxs-lookup"><span data-stu-id="f0e00-112">Functions class library project</span></span>

<span data-ttu-id="f0e00-113">Skapa ett nytt Azure Functions-projekt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0e00-113">From Visual Studio, create a new Azure Functions project.</span></span> <span data-ttu-id="f0e00-114">Den nya projektmallen för skapar filerna *host.json* och *local.settings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0e00-114">The new project template creates the files *host.json* and *local.settings.json*.</span></span> <span data-ttu-id="f0e00-115">Du kan [Anpassa inställningar för Azure Functions-runtime i host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="f0e00-115">You can [customize Azure Functions runtime settings in host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span> 

<span data-ttu-id="f0e00-116">Filen *local.settings.json* lagrar app-inställningar, anslutningssträngar och inställningar för Azure Functions Core verktyg.</span><span class="sxs-lookup"><span data-stu-id="f0e00-116">The file *local.settings.json* stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="f0e00-117">Läs mer om strukturen i [koden och testa Azure functions lokalt](functions-run-local.md#local-settings).</span><span class="sxs-lookup"><span data-stu-id="f0e00-117">To learn more about its structure, see [Code and test Azure functions locally](functions-run-local.md#local-settings).</span></span>

### <a name="functionname-attribute"></a><span data-ttu-id="f0e00-118">FunctionName attribut</span><span class="sxs-lookup"><span data-stu-id="f0e00-118">FunctionName attribute</span></span>

<span data-ttu-id="f0e00-119">Attributet [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) markerar en metod som en startpunkt för funktionen.</span><span class="sxs-lookup"><span data-stu-id="f0e00-119">The attribute [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) marks a method as a function entry point.</span></span> <span data-ttu-id="f0e00-120">Den måste användas med en utlösare och 0 eller fler indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="f0e00-120">It must be used with exactly one trigger and 0 or more input and output bindings.</span></span>

### <a name="conversion-to-functionjson"></a><span data-ttu-id="f0e00-121">Konvertering till function.json</span><span class="sxs-lookup"><span data-stu-id="f0e00-121">Conversion to function.json</span></span>

<span data-ttu-id="f0e00-122">När du har skapat ett Azure Functions-projekt, ger en fil `function.json` i katalogen matchar namnet på funktionen definieras av `[FunctionName]`.</span><span class="sxs-lookup"><span data-stu-id="f0e00-122">When an Azure Functions project is built, it produces a file `function.json` in the directory matching the function name defined by `[FunctionName]`.</span></span> <span data-ttu-id="f0e00-123">Det anger utlösare och bindningar och pekar på sammansättningen projektfilen.</span><span class="sxs-lookup"><span data-stu-id="f0e00-123">It specifies triggers and bindings and points to the project assembly file.</span></span>

<span data-ttu-id="f0e00-124">Den här konverteringen utförs av NuGet-paketet [Microsoft\.NET\.Sdk\.funktioner](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span><span class="sxs-lookup"><span data-stu-id="f0e00-124">This conversion is performed by the NuGet package [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span></span> <span data-ttu-id="f0e00-125">Källan är tillgänglig i GitHub-repo [azure\-funktioner\-vs\-skapa\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span><span class="sxs-lookup"><span data-stu-id="f0e00-125">The source is available in the GitHub repo [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span></span>

## <a name="triggers-and-bindings"></a><span data-ttu-id="f0e00-126">Utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="f0e00-126">Triggers and bindings</span></span>

<span data-ttu-id="f0e00-127">I följande tabell visas utlösare och bindningar som är tillgängliga i en Azure Functions klassbiblioteksprojektet.</span><span class="sxs-lookup"><span data-stu-id="f0e00-127">The following table lists the triggers and bindings that are available in an Azure Functions class library project.</span></span> <span data-ttu-id="f0e00-128">Alla attribut som finns i namnområdet `Microsoft.Azure.WebJobs`.</span><span class="sxs-lookup"><span data-stu-id="f0e00-128">All attributes are in the namespace `Microsoft.Azure.WebJobs`.</span></span>

| <span data-ttu-id="f0e00-129">Bindning</span><span class="sxs-lookup"><span data-stu-id="f0e00-129">Binding</span></span> | <span data-ttu-id="f0e00-130">Attribut</span><span class="sxs-lookup"><span data-stu-id="f0e00-130">Attribute</span></span> | <span data-ttu-id="f0e00-131">NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="f0e00-131">NuGet package</span></span> |
|------   | ------    | ------        |
| [<span data-ttu-id="f0e00-132">BLOB storage-utlösare, indata, utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-132">Blob storage trigger, input, output</span></span>](#blob-storage) | <span data-ttu-id="f0e00-133">[BlobAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-133">[BlobAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="f0e00-134">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="f0e00-134">[Microsoft.Azure.WebJobs]</span></span> | <span data-ttu-id="f0e00-135">[Blobblagring]</span><span class="sxs-lookup"><span data-stu-id="f0e00-135">[Blob storage]</span></span> |
| [<span data-ttu-id="f0e00-136">Cosmos DB indata och utdata bindning</span><span class="sxs-lookup"><span data-stu-id="f0e00-136">Cosmos DB input and output binding</span></span>](#cosmos-db) | <span data-ttu-id="f0e00-137">[DocumentDBAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-137">[DocumentDBAttribute]</span></span> | <span data-ttu-id="f0e00-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span><span class="sxs-lookup"><span data-stu-id="f0e00-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span></span> | 
| [<span data-ttu-id="f0e00-139">Händelseutlösare NAV- och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-139">Event Hubs trigger and output</span></span>](#event-hub) | <span data-ttu-id="f0e00-140">[EventHubTriggerAttribute], [EventHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-140">[EventHubTriggerAttribute], [EventHubAttribute]</span></span> | <span data-ttu-id="f0e00-141">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="f0e00-141">[Microsoft.Azure.WebJobs.ServiceBus]</span></span> |
| [<span data-ttu-id="f0e00-142">Den externa filen inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-142">External file input and output</span></span>](#api-hub) | <span data-ttu-id="f0e00-143">[ApiHubFileAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-143">[ApiHubFileAttribute]</span></span> | <span data-ttu-id="f0e00-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span><span class="sxs-lookup"><span data-stu-id="f0e00-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span></span> |
| [<span data-ttu-id="f0e00-145">HTTP- och webhook-utlösare</span><span class="sxs-lookup"><span data-stu-id="f0e00-145">HTTP and webhook trigger</span></span>](#http) | <span data-ttu-id="f0e00-146">[HttpTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-146">[HttpTriggerAttribute]</span></span> | <span data-ttu-id="f0e00-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span><span class="sxs-lookup"><span data-stu-id="f0e00-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span></span> |
| [<span data-ttu-id="f0e00-148">Mobile Apps inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-148">Mobile Apps input and output</span></span>](#mobile-apps) | <span data-ttu-id="f0e00-149">[MobileTableAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-149">[MobileTableAttribute]</span></span> | <span data-ttu-id="f0e00-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span><span class="sxs-lookup"><span data-stu-id="f0e00-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span></span> | 
| [<span data-ttu-id="f0e00-151">Notification Hubs utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-151">Notification Hubs output</span></span>](#nh) | <span data-ttu-id="f0e00-152">[NotificationHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-152">[NotificationHubAttribute]</span></span> | <span data-ttu-id="f0e00-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span><span class="sxs-lookup"><span data-stu-id="f0e00-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span></span> | 
| [<span data-ttu-id="f0e00-154">Queue storage utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-154">Queue storage trigger and output</span></span>](#queue) | <span data-ttu-id="f0e00-155">[QueueAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-155">[QueueAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="f0e00-156">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="f0e00-156">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="f0e00-157">SendGrid-utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-157">SendGrid output</span></span>](#sendgrid) | <span data-ttu-id="f0e00-158">[SendGridAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-158">[SendGridAttribute]</span></span> | <span data-ttu-id="f0e00-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span><span class="sxs-lookup"><span data-stu-id="f0e00-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span></span> | 
| [<span data-ttu-id="f0e00-160">Service Bus-utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-160">Service Bus trigger and output</span></span>](#service-bus) | <span data-ttu-id="f0e00-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span></span> | <span data-ttu-id="f0e00-162">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="f0e00-162">[Microsoft.Azure.WebJobs.ServiceBus]</span></span>
| [<span data-ttu-id="f0e00-163">Table storage inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-163">Table storage input and output</span></span>](#table) | <span data-ttu-id="f0e00-164">[TableAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-164">[TableAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="f0e00-165">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="f0e00-165">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="f0e00-166">Timerutlösare</span><span class="sxs-lookup"><span data-stu-id="f0e00-166">Timer trigger</span></span>](#timer) | <span data-ttu-id="f0e00-167">[TimerTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-167">[TimerTriggerAttribute]</span></span> | <span data-ttu-id="f0e00-168">[Microsoft.Azure.WebJobs.Extensions]</span><span class="sxs-lookup"><span data-stu-id="f0e00-168">[Microsoft.Azure.WebJobs.Extensions]</span></span> | 
| [<span data-ttu-id="f0e00-169">Twilio-utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-169">Twilio output</span></span>](#twilio) | <span data-ttu-id="f0e00-170">[TwilioSmsAttribute]</span><span class="sxs-lookup"><span data-stu-id="f0e00-170">[TwilioSmsAttribute]</span></span> | <span data-ttu-id="f0e00-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span><span class="sxs-lookup"><span data-stu-id="f0e00-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span></span> | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a><span data-ttu-id="f0e00-172">BLOB storage utlösaren indata och utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="f0e00-172">Blob storage trigger, input, and output bindings</span></span>

<span data-ttu-id="f0e00-173">Azure Functions stöder utlösaren indata och utdata bindningar för Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="f0e00-173">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="f0e00-174">Mer information om bindande uttryck och metadata finns [Azure Functions Blob storage bindningar](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-174">For more information on binding expressions and metadata, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>

<span data-ttu-id="f0e00-175">En blob-utlösare har definierats med den `[BlobTrigger]` attribut.</span><span class="sxs-lookup"><span data-stu-id="f0e00-175">A blob trigger is defined with the `[BlobTrigger]` attribute.</span></span> <span data-ttu-id="f0e00-176">Du kan använda attributet `[StorageAccount]` definiera storage-konto som används av ett hela funktion eller en klass.</span><span class="sxs-lookup"><span data-stu-id="f0e00-176">You can use the attribute `[StorageAccount]` to define the storage account that is used by an entire function or class.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

<span data-ttu-id="f0e00-177">BLOB som indata och utdata har definierats med hjälp av den `[Blob]` attributet tillsammans med en `FileAccess` parameter som anger att läsa eller skriva.</span><span class="sxs-lookup"><span data-stu-id="f0e00-177">Blob input and output are defined using the `[Blob]` attribute, along with a `FileAccess` parameter indicating read or write.</span></span> <span data-ttu-id="f0e00-178">I följande exempel används en blob utlösare och blob-utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="f0e00-178">The following example uses a blob trigger and blob output binding.</span></span>

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

### <a name="cosmos-db-input-and-output-bindings"></a><span data-ttu-id="f0e00-179">Cosmos DB indata och utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="f0e00-179">Cosmos DB input and output bindings</span></span>

<span data-ttu-id="f0e00-180">Azure Functions stöder indata och utdata bindningar för Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f0e00-180">Azure Functions supports input and output bindings for Cosmos DB.</span></span> <span data-ttu-id="f0e00-181">Mer information om funktionerna i Cosmos-DB-bindning finns [Azure Functions Cosmos DB bindningar](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-181">To learn more about the features of the Cosmos DB binding, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>

<span data-ttu-id="f0e00-182">Om du vill binda till en Cosmos-DB-dokumentet använder attributet `[DocumentDB]` i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span><span class="sxs-lookup"><span data-stu-id="f0e00-182">To bind to a Cosmos DB document, use the attribute `[DocumentDB]` in the NuGet package [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span></span> <span data-ttu-id="f0e00-183">I följande exempel har en kö-utlösare och ett DocumentDB-API-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="f0e00-183">The following example has a queue trigger and a DocumentDB API output binding:</span></span>

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

### <a name="event-hubs-trigger-and-output"></a><span data-ttu-id="f0e00-184">Händelseutlösare NAV- och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-184">Event Hubs trigger and output</span></span>

<span data-ttu-id="f0e00-185">Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="f0e00-185">Azure Functions supports trigger and output bindings for Event Hubs.</span></span> <span data-ttu-id="f0e00-186">Mer information finns i [Azure Functions Event Hub bindningar](functions-bindings-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-186">For more information, see  [Azure Functions Event Hub bindings](functions-bindings-event-hubs.md).</span></span>

<span data-ttu-id="f0e00-187">Typerna `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` och `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` definieras i NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="f0e00-187">The types `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` and `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="f0e00-188">I följande exempel används en Event Hub-utlösare:</span><span class="sxs-lookup"><span data-stu-id="f0e00-188">The following example uses an Event Hub trigger:</span></span>

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="f0e00-189">I följande exempel har en Händelsehubb utdata, med hjälp av metoden returvärdet som utdata:</span><span class="sxs-lookup"><span data-stu-id="f0e00-189">The following example has an Event Hub output, using the method return value as the output:</span></span>

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

### <a name="external-file-input-and-output"></a><span data-ttu-id="f0e00-190">Den externa filen inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-190">External file input and output</span></span>

<span data-ttu-id="f0e00-191">Azure Functions stöder utlösaren indata och utdata bindningar för externa filer, till exempel Google Drive, Dropbox och OneDrive.</span><span class="sxs-lookup"><span data-stu-id="f0e00-191">Azure Functions supports trigger, input, and output bindings for external files, such as Google Drive, Dropbox, and OneDrive.</span></span> <span data-ttu-id="f0e00-192">Läs mer i [Azure Functions extern fil bindningar](functions-bindings-external-file.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-192">To learn more, see [Azure Functions External File bindings](functions-bindings-external-file.md).</span></span> <span data-ttu-id="f0e00-193">Attributen `[ExternalFileTrigger]` och `[ExternalFile]` definieras i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span><span class="sxs-lookup"><span data-stu-id="f0e00-193">The attributes `[ExternalFileTrigger]` and `[ExternalFile]` are defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span></span>

<span data-ttu-id="f0e00-194">C# exemplet nedan visar en extern fil indata och utdata bindning.</span><span class="sxs-lookup"><span data-stu-id="f0e00-194">The following C# example demonstrates an external file input and output binding.</span></span> <span data-ttu-id="f0e00-195">Koden kopieras filen till utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="f0e00-195">The code copies the input file to the output file.</span></span>

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

### <a name="http-and-webhooks"></a><span data-ttu-id="f0e00-196">HTTP och webhooks</span><span class="sxs-lookup"><span data-stu-id="f0e00-196">HTTP and webhooks</span></span>

<span data-ttu-id="f0e00-197">Använd den `HttpTrigger` attribut för att ange en HTTP-utlösare eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="f0e00-197">Use the `HttpTrigger` attribute to define an HTTP trigger or webhook.</span></span> <span data-ttu-id="f0e00-198">Det här attributet definieras i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.Http].</span><span class="sxs-lookup"><span data-stu-id="f0e00-198">This attribute is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.Http].</span></span> <span data-ttu-id="f0e00-199">Du kan anpassa åtkomstnivå, webhook-typ, väg och metoder.</span><span class="sxs-lookup"><span data-stu-id="f0e00-199">You can customize the authorization level, webhook type, route, and methods.</span></span> <span data-ttu-id="f0e00-200">I följande exempel definieras en HTTP-utlösare med anonym autentisering och _genericJson_ webhook-typen.</span><span class="sxs-lookup"><span data-stu-id="f0e00-200">The following example defines an HTTP trigger with anonymous authentication and _genericJson_ webhook type.</span></span>

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a><span data-ttu-id="f0e00-201">Mobile Apps inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-201">Mobile Apps input and output</span></span>

<span data-ttu-id="f0e00-202">Azure Functions stöder indata och utdata bindningar för Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="f0e00-202">Azure Functions supports input and output bindings for Mobile Apps.</span></span> <span data-ttu-id="f0e00-203">Läs mer i [Azure Functions Mobile Apps bindningar](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-203">To learn more, see [Azure Functions Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span> <span data-ttu-id="f0e00-204">Attributet `[MobileTable]` har definierats i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span><span class="sxs-lookup"><span data-stu-id="f0e00-204">The attribute `[MobileTable]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span></span>

<span data-ttu-id="f0e00-205">I följande exempel visas en Mobile Apps-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="f0e00-205">The following example demonstrates a Mobile Apps output binding:</span></span>

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a><span data-ttu-id="f0e00-206">Notification Hubs utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-206">Notification Hubs output</span></span>

<span data-ttu-id="f0e00-207">Azure Functions stöder en output-bindning för Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="f0e00-207">Azure Functions supports an output binding for Notification Hubs.</span></span> <span data-ttu-id="f0e00-208">Läs mer i [Azure Functions Notification Hub-utdatabindning](functions-bindings-notification-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-208">To learn more, see [Azure Functions Notification Hub output binding](functions-bindings-notification-hubs.md).</span></span> <span data-ttu-id="f0e00-209">Attributet `[NotificationHub]` har definierats i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span><span class="sxs-lookup"><span data-stu-id="f0e00-209">The attribute `[NotificationHub]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span></span>

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a><span data-ttu-id="f0e00-210">Queue storage utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-210">Queue storage trigger and output</span></span>

<span data-ttu-id="f0e00-211">Azure Functions stöder utlösa och utgående bindningar för Azure köer.</span><span class="sxs-lookup"><span data-stu-id="f0e00-211">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="f0e00-212">Mer information finns i [Azure Functions Queue Storage bindningar](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-212">For more information, see [Azure Functions Queue Storage bindings](functions-bindings-storage-queue.md).</span></span>

<span data-ttu-id="f0e00-213">I följande exempel visas hur du använder funktionen returtypen med en kö utdata bindning, med hjälp av `[Queue]` attribut.</span><span class="sxs-lookup"><span data-stu-id="f0e00-213">The following example shows how to use the function return type with a queue output binding, using the `[Queue]` attribute.</span></span> <span data-ttu-id="f0e00-214">Om du vill definiera en utlösare för kön, använder den `[QueueTrigger]` attribut.</span><span class="sxs-lookup"><span data-stu-id="f0e00-214">To define a queue trigger, use the `[QueueTrigger]` attribute.</span></span>

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

### <a name="sendgrid-output"></a><span data-ttu-id="f0e00-215">SendGrid-utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-215">SendGrid output</span></span>

<span data-ttu-id="f0e00-216">Azure Functions stöder en SendGrid utdata bindning för att skicka e-post via programmering.</span><span class="sxs-lookup"><span data-stu-id="f0e00-216">Azure Functions supports a SendGrid output binding for sending email programmatically.</span></span> <span data-ttu-id="f0e00-217">Läs mer i [Azure Functions SendGrid bindningar](functions-bindings-sendgrid.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-217">To learn more, see [Azure Functions SendGrid bindings](functions-bindings-sendgrid.md).</span></span>

<span data-ttu-id="f0e00-218">Attributet `[SendGrid]` har definierats i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span><span class="sxs-lookup"><span data-stu-id="f0e00-218">The attribute `[SendGrid]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span></span>

<span data-ttu-id="f0e00-219">Följande är ett exempel på med en utlösare för Service Bus-kö och en SendGrid utdata bindning med `SendGridMessage`:</span><span class="sxs-lookup"><span data-stu-id="f0e00-219">The following is an example of using a Service Bus queue trigger and a SendGrid output binding using `SendGridMessage`:</span></span>

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
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a><span data-ttu-id="f0e00-220">Service Bus-utlösare och utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-220">Service Bus trigger and output</span></span>

<span data-ttu-id="f0e00-221">Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="f0e00-221">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span> <span data-ttu-id="f0e00-222">Mer information om hur du konfigurerar bindningar finns [Azure Functions Service Bus-bindningar](functions-bindings-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-222">For more information on configuring bindings, see [Azure Functions Service Bus bindings](functions-bindings-service-bus.md).</span></span>

<span data-ttu-id="f0e00-223">Attributen `[ServiceBusTrigger]` och `[ServiceBus]` definieras i NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="f0e00-223">The attributes `[ServiceBusTrigger]` and `[ServiceBus]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="f0e00-224">Följande är ett exempel på en utlösare för Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="f0e00-224">The following is an example of a Service Bus queue trigger:</span></span>

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<span data-ttu-id="f0e00-225">Följande är ett exempel på en Service Bus-utdata bindning, med Returtypen för metoden som utdata:</span><span class="sxs-lookup"><span data-stu-id="f0e00-225">The following is an example of a Service Bus output binding, using the method return type as the output:</span></span>

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

### <a name="table-storage-input-and-output"></a><span data-ttu-id="f0e00-226">Table storage inkommande och utgående</span><span class="sxs-lookup"><span data-stu-id="f0e00-226">Table storage input and output</span></span>

<span data-ttu-id="f0e00-227">Azure Functions stöder indata och utdata bindningar för Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="f0e00-227">Azure Functions supports input and output bindings for Azure Table storage.</span></span> <span data-ttu-id="f0e00-228">Läs mer i [Azure Functions Table storage bindningar](functions-bindings-storage-table.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-228">To learn more, see [Azure Functions Table storage bindings](functions-bindings-storage-table.md).</span></span>

<span data-ttu-id="f0e00-229">I följande exempel är en klass med två funktioner som visar tabellen lagring utgående och inkommande bindningar.</span><span class="sxs-lookup"><span data-stu-id="f0e00-229">The following example is a class with two functions, demonstrating table storage output and input bindings.</span></span> 

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

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a><span data-ttu-id="f0e00-230">Timerutlösare</span><span class="sxs-lookup"><span data-stu-id="f0e00-230">Timer trigger</span></span>

<span data-ttu-id="f0e00-231">Azure Functions har en timer utlösaren bindning som låter dig köra din funktionskod baserat på ett definierat schema.</span><span class="sxs-lookup"><span data-stu-id="f0e00-231">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> <span data-ttu-id="f0e00-232">Läs mer om funktionerna i bindningen i [schemalägga kodkörning med Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-232">To learn more about the features of the binding, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>

<span data-ttu-id="f0e00-233">På planen förbrukning, kan du definiera scheman med en [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span><span class="sxs-lookup"><span data-stu-id="f0e00-233">On the Consumption plan, you can define schedules with a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span></span> <span data-ttu-id="f0e00-234">Om du använder en App Service-Plan kan använda du också en TimeSpan-sträng.</span><span class="sxs-lookup"><span data-stu-id="f0e00-234">If you're using an App Service Plan, you can also use a TimeSpan string.</span></span> 

<span data-ttu-id="f0e00-235">I följande exempel definieras en timer som utlösare som körs var femte minut:</span><span class="sxs-lookup"><span data-stu-id="f0e00-235">The following example defines a timer trigger that runs every 5 minutes:</span></span>

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a><span data-ttu-id="f0e00-236">Twilio-utdata</span><span class="sxs-lookup"><span data-stu-id="f0e00-236">Twilio output</span></span>

<span data-ttu-id="f0e00-237">Azure Functions stöder Twilio utdata bindningar om du vill aktivera dina funktioner att skicka SMS textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="f0e00-237">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages.</span></span> <span data-ttu-id="f0e00-238">Läs mer i [skicka SMS-meddelanden från Azure-funktioner med hjälp av Twilio-utdatabindning](functions-bindings-twilio.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-238">To learn more, see [Send SMS messages from Azure Functions using the Twilio output binding](functions-bindings-twilio.md).</span></span> 

<span data-ttu-id="f0e00-239">Attributet `[TwilioSms]` har definierats i paketet [Microsoft.Azure.WebJobs.Extensions.Twilio].</span><span class="sxs-lookup"><span data-stu-id="f0e00-239">The attribute `[TwilioSms]` is defined in the package [Microsoft.Azure.WebJobs.Extensions.Twilio].</span></span>

<span data-ttu-id="f0e00-240">I följande C#-exempel används en kö utlösare och en Twilio-utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="f0e00-240">The following C# example uses a queue trigger and a Twilio output binding:</span></span>

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a><span data-ttu-id="f0e00-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0e00-241">Next steps</span></span>

<span data-ttu-id="f0e00-242">Mer information om hur du använder Azure Functions i C#-skript finns [Azure Functions C\# skript för utvecklare](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-242">For more information on using Azure Functions in C# scripting, see [Azure Functions C\# script developer reference](functions-reference-csharp.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
<span data-ttu-id="f0e00-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span></span>
<span data-ttu-id="f0e00-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span></span>
<span data-ttu-id="f0e00-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="f0e00-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span></span>
<span data-ttu-id="f0e00-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span></span>
<span data-ttu-id="f0e00-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="f0e00-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span></span>
<span data-ttu-id="f0e00-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span></span>
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
<span data-ttu-id="f0e00-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span><span class="sxs-lookup"><span data-stu-id="f0e00-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span></span>
<span data-ttu-id="f0e00-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span></span>
<span data-ttu-id="f0e00-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f0e00-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span></span>


<!-- Links to source --> 
<span data-ttu-id="f0e00-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span></span>
<span data-ttu-id="f0e00-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span></span>
<span data-ttu-id="f0e00-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span></span>
<span data-ttu-id="f0e00-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span></span>
<span data-ttu-id="f0e00-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span><span class="sxs-lookup"><span data-stu-id="f0e00-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span></span>
<span data-ttu-id="f0e00-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span></span>
<span data-ttu-id="f0e00-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span></span>
<span data-ttu-id="f0e00-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span></span>
<span data-ttu-id="f0e00-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span></span>
<span data-ttu-id="f0e00-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span></span>
<span data-ttu-id="f0e00-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span></span>
<span data-ttu-id="f0e00-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span></span>
<span data-ttu-id="f0e00-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span></span>
<span data-ttu-id="f0e00-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span></span>
<span data-ttu-id="f0e00-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span></span>
<span data-ttu-id="f0e00-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="f0e00-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span></span>
