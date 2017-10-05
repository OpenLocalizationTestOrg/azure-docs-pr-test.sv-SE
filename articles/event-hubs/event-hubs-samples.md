---
title: Azure Event Hubs-exempel | Microsoft Docs
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
ms.openlocfilehash: ae9fbd97a1747d8f14c561f247a0973bb11fd039
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="dd4a6-103">Event Hubs-exempel</span><span class="sxs-lookup"><span data-stu-id="dd4a6-103">Event Hubs samples</span></span> 

<span data-ttu-id="dd4a6-104">Uppsättning Azure Event Hubs exempel visar viktiga funktioner i [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="dd4a6-104">The set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="dd4a6-105">Den här artikeln kategoriserar och beskriver tillgängliga, med länkar till varje exemplen.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-105">This article categorizes and describes the samples available, with links to each.</span></span>

<span data-ttu-id="dd4a6-106">När detta skrivs finns Händelsehubbar exempel i flera olika platser:</span><span class="sxs-lookup"><span data-stu-id="dd4a6-106">At the time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="dd4a6-107">Kodexempel för MSDN-utvecklare</span><span class="sxs-lookup"><span data-stu-id="dd4a6-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="dd4a6-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="dd4a6-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="dd4a6-109">Mer information om olika versioner av .NET Framework finns [ramverk och mål](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="dd4a6-109">For more information about different versions of the .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="dd4a6-110">Flera exempel kommer att läggas till med tiden, så kom tillbaka ofta efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="dd4a6-111">Standard för .NET</span><span class="sxs-lookup"><span data-stu-id="dd4a6-111">.NET Standard</span></span>

<span data-ttu-id="dd4a6-112">Följande exempel visar hur du skickar och tar emot händelser med hjälp av den [händelsehubbklient](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) för den [.NET standardbibliotek](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="dd4a6-112">The following samples demonstrate how to send and receive events using the [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for the [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="dd4a6-113">Skicka händelser</span><span class="sxs-lookup"><span data-stu-id="dd4a6-113">Send events</span></span> 

<span data-ttu-id="dd4a6-114">Den [börjar skicka](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) exempel visas hur du skriver ett .NET Core-konsolprogram som skickar händelser till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-114">The [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how to write a .NET Core console application that sends events to an event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="dd4a6-115">Ta emot händelser</span><span class="sxs-lookup"><span data-stu-id="dd4a6-115">Receive events</span></span> 

<span data-ttu-id="dd4a6-116">Den [börja ta emot med den värd för händelsebearbetning](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) prov är ett .NET Core-konsolprogram som tar emot meddelanden från en händelsehubb med hjälp av den värd för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-116">The [Get started receiving with the Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using the Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="dd4a6-117">.NET framework</span><span class="sxs-lookup"><span data-stu-id="dd4a6-117">.NET Framework</span></span>   

<span data-ttu-id="dd4a6-118">De här exemplen visar olika funktioner i Azure Event Hubs, riktad på [biblioteket för .NET Framework](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="dd4a6-118">These samples demonstrate various other features of Azure Event Hubs, targeting the [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="dd4a6-119">Meddela användare om händelser som tagits emot</span><span class="sxs-lookup"><span data-stu-id="dd4a6-119">Notify users of events received</span></span>

<span data-ttu-id="dd4a6-120">Den [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) exempel meddelar användare av data från sensorer och andra system.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-120">The [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="dd4a6-121">Kom igång med händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="dd4a6-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="dd4a6-122">Den [Event Hubs komma igång](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) exempel visas de grundläggande funktionerna i Händelsehubbar, till exempel hur du skapar en händelsehubb, skicka händelser till en händelsehubb och använda händelser med hjälp av den [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .</span><span class="sxs-lookup"><span data-stu-id="dd4a6-122">The [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates the basic capabilities of Event Hubs, such as how to create an event hub, send events to an event hub, and consume events using the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="dd4a6-123">Skala ut händelsebearbetning</span><span class="sxs-lookup"><span data-stu-id="dd4a6-123">Scale out event processing</span></span> 

<span data-ttu-id="dd4a6-124">Den [skala ut händelsebearbetning](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) exempel visar hur du använder den [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) att fördela belastningen av Händelsehubbar dataströmmen förbrukning.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-124">The [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how to use the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) to distribute the workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="dd4a6-125">Den visar hur du implementerar den **EventProcessor** och **EventProcessorFactory** objekt som ska hanteras händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-125">It shows how to implement the **EventProcessor** and **EventProcessorFactory** objects to manage the event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="dd4a6-126">Hämtar data från SQL till en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="dd4a6-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="dd4a6-127">Den [dra SQL-data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) exemplet visar hur du hämtar data från en SQLtabell och skicka den till en händelsehubb ska användas som indata i underordnade analytiska program.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-127">The [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how to pull data from a SQL table and push it to an event hub, to use as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="dd4a6-128">Hämta webbdata till en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="dd4a6-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="dd4a6-129">Den [importera data från webben](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) exemplet visar hur du hämtar data från offentliga feeds (till exempel avdelning för transportens trafik information feed) och skicka den till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="dd4a6-129">The [Import data from the web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how to pull data from public feeds (such as the Department of Transportation's traffic information feed) and push it to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd4a6-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd4a6-130">Next steps</span></span>

<span data-ttu-id="dd4a6-131">Läs mer om .NET Framework-versioner genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="dd4a6-131">Learn more about .NET Framework versions by visiting the following links:</span></span>

- [<span data-ttu-id="dd4a6-132">Ramverk och mål</span><span class="sxs-lookup"><span data-stu-id="dd4a6-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="dd4a6-133">.NET framework 4.6 och 4.5</span><span class="sxs-lookup"><span data-stu-id="dd4a6-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="dd4a6-134">Du kan lära dig mer om Händelsehubbar i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="dd4a6-134">You can learn more about Event Hubs in the following articles:</span></span>

- [<span data-ttu-id="dd4a6-135">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="dd4a6-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="dd4a6-136">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="dd4a6-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="dd4a6-137">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="dd4a6-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)