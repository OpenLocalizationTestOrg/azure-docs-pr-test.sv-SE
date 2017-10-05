---
title: "Vad är Azure Event Hubs och varför ska jag använda det? | Microsoft Docs"
description: "En översikt och introduktion till Azure Event Hubs – telemetriinmatning i molnskala från webbplatser, appar och enheter"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: 1a6bf0a0352e6d9e3a22586ac825558d12e1307a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-event-hubs"></a><span data-ttu-id="3991c-103">Vad är Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="3991c-103">What is Event Hubs?</span></span>

<span data-ttu-id="3991c-104">Azure Event Hubs är en mycket skalbar dataströmningsplattform och händelseinmatningstjänst som kan ta emot och bearbeta flera miljoner händelser per sekund.</span><span class="sxs-lookup"><span data-stu-id="3991c-104">Azure Event Hubs is a highly scalable data streaming platform and event ingestion service capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="3991c-105">Event Hubs kan bearbeta och lagra händelser, data eller telemetri som producerats av distribuerade program och enheter.</span><span class="sxs-lookup"><span data-stu-id="3991c-105">Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.</span></span> <span data-ttu-id="3991c-106">Data som skickas till en händelsehubb kan omvandlas och lagras med valfri provider för realtidsanalys eller batchbearbetnings-/lagringsadapter.</span><span class="sxs-lookup"><span data-stu-id="3991c-106">Data sent to an event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="3991c-107">Event Hubs har [funktioner för att publicera och prenumerera](https://msdn.microsoft.com/library/aa560414.aspx) med kort svarstid och i massiv skala och tjänar därmed som inkörsvägen för stordata.</span><span class="sxs-lookup"><span data-stu-id="3991c-107">With the ability to provide [publish-subscribe capabilities](https://msdn.microsoft.com/library/aa560414.aspx) with low latency and at massive scale, Event Hubs serves as the "on ramp" for Big Data.</span></span>

## <a name="why-use-event-hubs"></a><span data-ttu-id="3991c-108">Varför ska jag använda Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="3991c-108">Why use Event Hubs?</span></span>

<span data-ttu-id="3991c-109">Händelse- och telemetrihanteringsfunktionerna gör Event Hubs särskilt lämpligt för:</span><span class="sxs-lookup"><span data-stu-id="3991c-109">Event Hubs event and telemetry handling capabilities make it especially useful for:</span></span>

* <span data-ttu-id="3991c-110">Programinstrumentering</span><span class="sxs-lookup"><span data-stu-id="3991c-110">Application instrumentation</span></span>
* <span data-ttu-id="3991c-111">Bearbetning av användarupplevelser och arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="3991c-111">User experience or workflow processing</span></span>
* <span data-ttu-id="3991c-112">Scenarier i sakernas internet (IoT)</span><span class="sxs-lookup"><span data-stu-id="3991c-112">Internet of Things (IoT) scenarios</span></span>

<span data-ttu-id="3991c-113">Andra funktioner i Event Hubs är till exempel beteendespårning i mobilappar, trafikinformation från webbservergrupper, inspelning av spelhändelser i konsolspel eller telemetri som samlats in från industriella datorer, anslutna fordon eller andra enheter.</span><span class="sxs-lookup"><span data-stu-id="3991c-113">For example, Event Hubs enables behavior tracking in mobile apps, traffic information from web farms, in-game event capture in console games, or telemetry collected from industrial machines, connected vehicles, or other devices.</span></span>

## <a name="azure-event-hubs-overview"></a><span data-ttu-id="3991c-114">Översikt av händelsehubbar i Azure</span><span class="sxs-lookup"><span data-stu-id="3991c-114">Azure Event Hubs overview</span></span>

<span data-ttu-id="3991c-115">I lösningsarkitekturer fungerar händelsehubbar ofta som ”dörren” för en händelsepipeline, ofta kallad en *händelseinmatare*.</span><span class="sxs-lookup"><span data-stu-id="3991c-115">The common role that Event Hubs plays in solution architectures is the "front door" for an event pipeline, often called an *event ingestor*.</span></span> <span data-ttu-id="3991c-116">En händelseinmatare är en komponent eller tjänst som placeras mellan händelseutgivare och -konsumenter och frikopplar produktion av en händelseström från användningen av de händelserna.</span><span class="sxs-lookup"><span data-stu-id="3991c-116">An event ingestor is a component or service that sits between event publishers and event consumers to decouple the production of an event stream from the consumption of those events.</span></span> <span data-ttu-id="3991c-117">Arkitekturen visas på följande bild:</span><span class="sxs-lookup"><span data-stu-id="3991c-117">The following figure depicts this architecture:</span></span>

![Händelsehubbar](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

<span data-ttu-id="3991c-119">Event Hubs tillhandahåller funktioner för hantering av meddelandeströmmar men har egenskaper som skiljer sig från traditionell meddelandehantering för företag.</span><span class="sxs-lookup"><span data-stu-id="3991c-119">Event Hubs provides message stream handling capability but has characteristics that are different from traditional enterprise messaging.</span></span> <span data-ttu-id="3991c-120">Funktionerna i Event Hubs är byggda för scenarier med högt genomflöde och intensiv händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="3991c-120">Event Hubs capabilities are built around high throughput and event processing scenarios.</span></span> <span data-ttu-id="3991c-121">Event Hubs skiljer sig därmed från [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)-meddelanden och implementerar inte vissa av de funktioner som är tillgängliga för [Service Bus](/azure/service-bus-messaging/)-meddelandeentiteter, till exempel avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3991c-121">As such, Event Hubs is different from [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) messaging, and does not implement some of the capabilities that are available for [Service Bus messaging](/azure/service-bus-messaging/) entities, such as topics.</span></span>

## <a name="event-hubs-features"></a><span data-ttu-id="3991c-122">Event Hubs-funktioner</span><span class="sxs-lookup"><span data-stu-id="3991c-122">Event Hubs features</span></span>

<span data-ttu-id="3991c-123">Event Hubs innehåller följande viktiga element:</span><span class="sxs-lookup"><span data-stu-id="3991c-123">Event Hubs contains the following key elements:</span></span>

- <span data-ttu-id="3991c-124">[**Händelseskapare/utfärdare**](event-hubs-features.md#event-publishers): En entitet som skickar data till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="3991c-124">[**Event producers/publishers**](event-hubs-features.md#event-publishers): An entity that sends data to an event hub.</span></span> <span data-ttu-id="3991c-125">En händelse publiceras via AMQP 1.0 eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3991c-125">An event is published via AMQP 1.0 or HTTPS.</span></span>
- <span data-ttu-id="3991c-126">[**Avbildning**](event-hubs-features.md#capture): Gör att du kan avbilda strömmande data från Event Hubs och lagra dem på ett Azure Blob Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3991c-126">[**Capture**](event-hubs-features.md#capture): Enables you to capture Event Hubs streaming data and store it in an Azure Blob storage account.</span></span>
- <span data-ttu-id="3991c-127">[**Partitioner**](event-hubs-features.md#partitions): Gör att varje konsument endast kan läsa en viss delmängd, eller partition, i händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="3991c-127">[**Partitions**](event-hubs-features.md#partitions): Enables each consumer to only read a specific subset, or partition, of the event stream.</span></span>
- <span data-ttu-id="3991c-128">[**SAS-token**](event-hubs-features.md#sas-tokens): Identifierar och autentiserar händelseutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="3991c-128">[**SAS tokens**](event-hubs-features.md#sas-tokens): Identifies and authenticates the event publisher.</span></span>
- <span data-ttu-id="3991c-129">[**Händelsekonsument**](event-hubs-features.md#event-consumers): En enhet som läser händelsedata från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="3991c-129">[**Event consumers**](event-hubs-features.md#event-consumers): An entity that reads event data from an event hub.</span></span> <span data-ttu-id="3991c-130">Händelsekonsumenter ansluter via AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="3991c-130">Event consumers connect via AMQP 1.0.</span></span> 
- <span data-ttu-id="3991c-131">[**Konsumentgrupper**](event-hubs-features.md#consumer-groups): Ger varje flerkonsumerande program en separat vy över händelseströmmen, vilket gör att konsumenterna kan agera oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="3991c-131">[**Consumer groups**](event-hubs-features.md#consumer-groups): Provides each multiple consuming application with a separate view of the event stream, enabling those consumers to act independently.</span></span>
- <span data-ttu-id="3991c-132">[**Genomflödesenheter**](event-hubs-features.md#capacity): Förköpta kapacitetsenheter.</span><span class="sxs-lookup"><span data-stu-id="3991c-132">[**Throughput units**](event-hubs-features.md#capacity): Pre-purchased units of capacity.</span></span> <span data-ttu-id="3991c-133">En enskild partition har en maximal skala på en genomflödesenhet.</span><span class="sxs-lookup"><span data-stu-id="3991c-133">A single partition has a maximum scale of 1 throughput unit.</span></span>

<span data-ttu-id="3991c-134">Teknisk information om de här och andra Event Hubs-funktioner finns i [översikten över Event Hubs-funktioner](event-hubs-features.md).</span><span class="sxs-lookup"><span data-stu-id="3991c-134">For technical details about these and other Event Hubs features, see the [Event Hubs features overview](event-hubs-features.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3991c-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3991c-135">Next steps</span></span>

<span data-ttu-id="3991c-136">Utförlig prisinformation för Event Hubs finns i [Priser för Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="3991c-136">For detailed Event Hubs pricing information, see [Event Hubs Pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span>

<span data-ttu-id="3991c-137">Besök följande länkar för mer utförlig information om Event Hubs:</span><span class="sxs-lookup"><span data-stu-id="3991c-137">For more information about Event Hubs, visit the following links:</span></span>

* <span data-ttu-id="3991c-138">Kom igång med en [kurs om händelsehubbar](event-hubs-dotnet-standard-getstarted-send.md)</span><span class="sxs-lookup"><span data-stu-id="3991c-138">Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)</span></span>
* [<span data-ttu-id="3991c-139">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3991c-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)
* [<span data-ttu-id="3991c-140">Exempelprogram som använder Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3991c-140">Sample applications that use Event Hubs</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

