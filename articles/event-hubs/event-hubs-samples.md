---
title: "aaaAzure Händelsehubbar exempel | Microsoft Docs"
description: Azure Event Hubs-exempel
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="23648-103">Event Hubs-exempel</span><span class="sxs-lookup"><span data-stu-id="23648-103">Event Hubs samples</span></span> 

<span data-ttu-id="23648-104">hello Azure Event Hubs urval visar viktiga funktioner i [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="23648-104">hello set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="23648-105">Den här artikeln kategoriserar och beskriver hello-exempel som är tillgängliga med länkar tooeach.</span><span class="sxs-lookup"><span data-stu-id="23648-105">This article categorizes and describes hello samples available, with links tooeach.</span></span>

<span data-ttu-id="23648-106">När hello detta skrivs finns Händelsehubbar exempel i flera olika platser:</span><span class="sxs-lookup"><span data-stu-id="23648-106">At hello time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="23648-107">Kodexempel för MSDN-utvecklare</span><span class="sxs-lookup"><span data-stu-id="23648-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="23648-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="23648-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="23648-109">Läs mer om olika versioner av hello .NET Framework [ramverk och mål](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="23648-109">For more information about different versions of hello .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="23648-110">Flera exempel kommer att läggas till med tiden, så kom tillbaka ofta efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="23648-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="23648-111">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="23648-111">.NET Standard</span></span>

<span data-ttu-id="23648-112">hello följande exempel visar hur toosend och ta emot händelser med hjälp av hello [händelsehubbklient](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) för hello [.NET standardbibliotek](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="23648-112">hello following samples demonstrate how toosend and receive events using hello [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for hello [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="23648-113">Skicka händelser</span><span class="sxs-lookup"><span data-stu-id="23648-113">Send events</span></span> 

<span data-ttu-id="23648-114">Hej [börjar skicka](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) exempel visas hur toowrite en .NET Core-konsolapp som skickar händelser tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="23648-114">hello [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how toowrite a .NET Core console application that sends events tooan event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="23648-115">Ta emot händelser</span><span class="sxs-lookup"><span data-stu-id="23648-115">Receive events</span></span> 

<span data-ttu-id="23648-116">Hej [börja ta emot med hello värd för händelsebearbetning](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) prov är ett .NET Core-konsolprogram som tar emot meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="23648-116">hello [Get started receiving with hello Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using hello Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="23648-117">.NET framework</span><span class="sxs-lookup"><span data-stu-id="23648-117">.NET Framework</span></span>   

<span data-ttu-id="23648-118">De här exemplen visar olika funktioner i Azure Event Hubs, riktad hello [biblioteket för .NET Framework](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="23648-118">These samples demonstrate various other features of Azure Event Hubs, targeting hello [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="23648-119">Meddela användare om händelser som tagits emot</span><span class="sxs-lookup"><span data-stu-id="23648-119">Notify users of events received</span></span>

<span data-ttu-id="23648-120">Hej [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) exempel meddelar användare av data från sensorer och andra system.</span><span class="sxs-lookup"><span data-stu-id="23648-120">hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="23648-121">Kom igång med händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="23648-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="23648-122">Hej [Event Hubs komma igång](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) exemplet visar hello grundläggande funktionerna i Händelsehubbar, till exempel hur toocreate en händelsehubb, skicka händelser tooan händelsehubb och använda händelser med hjälp av hello [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="23648-122">hello [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates hello basic capabilities of Event Hubs, such as how toocreate an event hub, send events tooan event hub, and consume events using hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="23648-123">Skala ut händelsebearbetning</span><span class="sxs-lookup"><span data-stu-id="23648-123">Scale out event processing</span></span> 

<span data-ttu-id="23648-124">Hej [skala ut händelsebearbetning](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) exemplet visar hur toouse hello [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello arbetsbelastning av Händelsehubbar dataströmmen förbrukning.</span><span class="sxs-lookup"><span data-stu-id="23648-124">hello [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="23648-125">Den visar hur tooimplement hello **EventProcessor** och **EventProcessorFactory** objekt toomanage hello händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="23648-125">It shows how tooimplement hello **EventProcessor** and **EventProcessorFactory** objects toomanage hello event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="23648-126">Hämtar data från SQL till en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="23648-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="23648-127">Hej [dra SQL-data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) exempel visas hur toopull data från en SQL tabell och push-installera den tooan händelsehubb, toouse indata i underordnade analytiska program.</span><span class="sxs-lookup"><span data-stu-id="23648-127">hello [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how toopull data from a SQL table and push it tooan event hub, toouse as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="23648-128">Hämta webbdata till en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="23648-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="23648-129">Hej [importera data från hello web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) exempel visas hur toopull data från offentliga feeds (till exempel hello avdelning av transports trafikinformation feed) och skicka tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="23648-129">hello [Import data from hello web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how toopull data from public feeds (such as hello Department of Transportation's traffic information feed) and push it tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23648-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23648-130">Next steps</span></span>

<span data-ttu-id="23648-131">Läs mer om .NET Framework-versioner genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="23648-131">Learn more about .NET Framework versions by visiting hello following links:</span></span>

- [<span data-ttu-id="23648-132">Ramverk och mål</span><span class="sxs-lookup"><span data-stu-id="23648-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="23648-133">.NET framework 4.6 och 4.5</span><span class="sxs-lookup"><span data-stu-id="23648-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="23648-134">Mer information om Händelsehubbar finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="23648-134">You can learn more about Event Hubs in hello following articles:</span></span>

- [<span data-ttu-id="23648-135">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="23648-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="23648-136">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="23648-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="23648-137">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="23648-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)