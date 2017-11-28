---
title: "Översikt över Azure händelse rutnätet"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 59a834f32793e349d5639baf3c80dbcba274dfa8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="an-introduction-to-azure-event-grid"></a><span data-ttu-id="dec11-103">En introduktion till Azure händelse rutnätet</span><span class="sxs-lookup"><span data-stu-id="dec11-103">An introduction to Azure Event Grid</span></span>

<span data-ttu-id="dec11-104">Azure händelse rutnätet kan du enkelt kan skapa program med händelsebaserat arkitekturerna.</span><span class="sxs-lookup"><span data-stu-id="dec11-104">Azure Event Grid allows you to easily build applications with event-based architectures.</span></span> <span data-ttu-id="dec11-105">Du väljer Azure-resursen du vill prenumerera på och ge händelsehanteraren eller WebHook-slutpunkten för att skicka händelsen till.</span><span class="sxs-lookup"><span data-stu-id="dec11-105">You select the Azure resource you would like to subscribe to, and give the event handler or WebHook endpoint to send the event to.</span></span> <span data-ttu-id="dec11-106">Händelsen rutnätet har inbyggt stöd för händelser som kommer från Azure-tjänster, som storage-blobbar och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="dec11-106">Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.</span></span> <span data-ttu-id="dec11-107">Händelsen rutnätet har också anpassade stöd för program och händelser från tredje part, med hjälp av anpassade avsnitt och anpassade webhooks.</span><span class="sxs-lookup"><span data-stu-id="dec11-107">Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.</span></span> 

<span data-ttu-id="dec11-108">Du kan använda filter för att vidarebefordra specifika händelser till olika slutpunkter samtidigt till flera slutpunkter och kontrollera att din händelser levereras på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="dec11-108">You can use filters to route specific events to different endpoints, multicast to multiple endpoints, and make sure your events are reliably delivered.</span></span> <span data-ttu-id="dec11-109">Händelsen rutnätet har också inbyggt stöd för anpassade och tredjeparts-händelser.</span><span class="sxs-lookup"><span data-stu-id="dec11-109">Event Grid also has built in support for custom and third-party events.</span></span>

<span data-ttu-id="dec11-110">I förhandsversionen har Event Grid stöd för platserna **westus2** och **westcentralus**.</span><span class="sxs-lookup"><span data-stu-id="dec11-110">For the preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span> <span data-ttu-id="dec11-111">Kommer att läggas till andra regioner.</span><span class="sxs-lookup"><span data-stu-id="dec11-111">Other regions will be added.</span></span>

<span data-ttu-id="dec11-112">Den här artikeln innehåller en översikt över Azure händelse rutnätet.</span><span class="sxs-lookup"><span data-stu-id="dec11-112">This article provides an overview of Azure Event Grid.</span></span> <span data-ttu-id="dec11-113">Om du vill komma igång med händelsen rutnätet finns [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="dec11-113">If you want to get started with Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>

![Händelsen rutnätet funktionella modellen](./media/overview/event-grid-functional-model.png)

<span data-ttu-id="dec11-115">Blob Storage är för närvarande inte tillgänglig som en utgivare.</span><span class="sxs-lookup"><span data-stu-id="dec11-115">Currently, Blob Storage is not publicly available as a publisher.</span></span>

## <a name="concepts"></a><span data-ttu-id="dec11-116">Koncept</span><span class="sxs-lookup"><span data-stu-id="dec11-116">Concepts</span></span>

<span data-ttu-id="dec11-117">Det finns fem koncept i Azure händelse rutnät som gör att du kan komma igång:</span><span class="sxs-lookup"><span data-stu-id="dec11-117">There are five concepts in Azure Event Grid that let you get going:</span></span>

* <span data-ttu-id="dec11-118">**Händelser** -vad hände.</span><span class="sxs-lookup"><span data-stu-id="dec11-118">**Events** - What happened.</span></span>
* <span data-ttu-id="dec11-119">**Källor/händelseutfärdare** – där händelsen ägde rum.</span><span class="sxs-lookup"><span data-stu-id="dec11-119">**Event sources/publishers** - Where the event took place.</span></span>
* <span data-ttu-id="dec11-120">**Avsnitt om** -slutpunkten som utgivare för att skicka händelser.</span><span class="sxs-lookup"><span data-stu-id="dec11-120">**Topics** - The endpoint where publishers send events.</span></span>
* <span data-ttu-id="dec11-121">**Händelseprenumerationer** -slutpunkt eller inbyggda mekanism för vägen händelser, ibland till flera hanterarna.</span><span class="sxs-lookup"><span data-stu-id="dec11-121">**Event subscriptions** - The endpoint or built-in mechanism to route events, sometimes to multiple handlers.</span></span> <span data-ttu-id="dec11-122">Prenumerationer används också av hanterare Intelligent filtrera inkommande händelser.</span><span class="sxs-lookup"><span data-stu-id="dec11-122">Subscriptions are also used by handlers to intelligently filter incoming events.</span></span>
* <span data-ttu-id="dec11-123">**Händelsehanterare** -app eller tjänst reagera på händelsen.</span><span class="sxs-lookup"><span data-stu-id="dec11-123">**Event handlers** - The app or service reacting to the event.</span></span>

<span data-ttu-id="dec11-124">Mer information om de här koncepten finns [koncept i Azure händelse rutnät](concepts.md).</span><span class="sxs-lookup"><span data-stu-id="dec11-124">For more information about these concepts, see [Concepts in Azure Event Grid](concepts.md).</span></span>

## <a name="capabilities"></a><span data-ttu-id="dec11-125">Funktioner</span><span class="sxs-lookup"><span data-stu-id="dec11-125">Capabilities</span></span>

<span data-ttu-id="dec11-126">Här följer några viktiga funktioner i Azure händelse rutnätet:</span><span class="sxs-lookup"><span data-stu-id="dec11-126">Here are some of the key features of Azure Event Grid:</span></span>

* <span data-ttu-id="dec11-127">**Enkelhet** -plats och klicka på att sträva efter händelser från Azure-resurs till händelsehanteraren eller slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="dec11-127">**Simplicity** - Point and click to aim events from your Azure resource to any event handler or endpoint.</span></span>
* <span data-ttu-id="dec11-128">**Avancerad filtrering** -Filter på händelsen typ eller händelse publiceringssökväg att säkerställa händelsehanterare får endast relevanta händelser.</span><span class="sxs-lookup"><span data-stu-id="dec11-128">**Advanced filtering** - Filter on event type or event publish path to ensure event handlers only receive relevant events.</span></span>
* <span data-ttu-id="dec11-129">**FAN-out** -prenumerera på flera slutpunkter på samma händelse ska skicka kopior av händelsen till så många platser efter behov.</span><span class="sxs-lookup"><span data-stu-id="dec11-129">**Fan-out** - Subscribe multiple endpoints to the same event to send copies of the event to as many places as needed.</span></span>
* <span data-ttu-id="dec11-130">**Tillförlitlighet** -använda 24-timmarsformat återförsök med exponentiell backoff för att säkerställa att händelser levereras.</span><span class="sxs-lookup"><span data-stu-id="dec11-130">**Reliability** - Utilize 24-hour retry with exponential backoff to ensure events are delivered.</span></span>
* <span data-ttu-id="dec11-131">**Betala per händelse** – betala endast för hur du använder händelsen rutnätet.</span><span class="sxs-lookup"><span data-stu-id="dec11-131">**Pay-per-event** - Pay only for the amount you use Event Grid.</span></span>
* <span data-ttu-id="dec11-132">**Hög genomströmning** -skapa omfattande arbetsbelastningar i händelse rutnät med stöd för miljontals händelser per sekund.</span><span class="sxs-lookup"><span data-stu-id="dec11-132">**High throughput** - Build high-volume workloads on Event Grid with support for millions of events per second.</span></span>
* <span data-ttu-id="dec11-133">**Inbyggda händelser** – komma igång snabbt och köra med resurs-definierade inbyggda händelser.</span><span class="sxs-lookup"><span data-stu-id="dec11-133">**Built-in Events** - Get up and running quickly with resource-defined built-in events.</span></span>
* <span data-ttu-id="dec11-134">**Anpassade händelser** -Använd händelse rutnätet flödet, filter och tillförlitligt leverera anpassade händelser i din app.</span><span class="sxs-lookup"><span data-stu-id="dec11-134">**Custom Events** - use Event Grid route, filter, and reliably deliver custom events in your app.</span></span>

## <a name="built-in-publisher-and-handler-integration"></a><span data-ttu-id="dec11-135">Inbyggd integrering av utgivare och hanterare</span><span class="sxs-lookup"><span data-stu-id="dec11-135">Built-in publisher and handler integration</span></span>

<span data-ttu-id="dec11-136">Azure erbjuder inbyggda händelsestöd med ett stort antal tjänster, inklusive både utgivare och hanterare.</span><span class="sxs-lookup"><span data-stu-id="dec11-136">Azure offers built-in event support using numerous services, including both publishers and handlers.</span></span>

### <a name="publishers"></a><span data-ttu-id="dec11-137">Utgivare</span><span class="sxs-lookup"><span data-stu-id="dec11-137">Publishers</span></span>

<span data-ttu-id="dec11-138">För närvarande har följande Azure-tjänster inbyggda utgivarens support för händelsen rutnät:</span><span class="sxs-lookup"><span data-stu-id="dec11-138">Currently, the following Azure services have built-in publisher support for event grid:</span></span>

* <span data-ttu-id="dec11-139">Resursgrupper (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="dec11-139">Resource Groups (management operations)</span></span>
* <span data-ttu-id="dec11-140">Azure-prenumerationer (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="dec11-140">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="dec11-141">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="dec11-141">Event Hubs</span></span>
* <span data-ttu-id="dec11-142">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="dec11-142">Custom Topics</span></span>

<span data-ttu-id="dec11-143">Andra Azure-tjänster kommer att adderas i år.</span><span class="sxs-lookup"><span data-stu-id="dec11-143">Other Azure services will be added this year.</span></span>

### <a name="handlers"></a><span data-ttu-id="dec11-144">Hanterare</span><span class="sxs-lookup"><span data-stu-id="dec11-144">Handlers</span></span>

<span data-ttu-id="dec11-145">Följande Azure-tjänster har för närvarande stöd för inbyggda hanterare för händelsen rutnätet:</span><span class="sxs-lookup"><span data-stu-id="dec11-145">Currently, the following Azure services have built-in handler support for Event Grid:</span></span> 

* <span data-ttu-id="dec11-146">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="dec11-146">Azure Functions</span></span>
* <span data-ttu-id="dec11-147">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="dec11-147">Logic Apps</span></span>
* <span data-ttu-id="dec11-148">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="dec11-148">Azure Automation</span></span>
* <span data-ttu-id="dec11-149">WebHooks</span><span class="sxs-lookup"><span data-stu-id="dec11-149">WebHooks</span></span>

<span data-ttu-id="dec11-150">Andra Azure-tjänster kommer att adderas i år.</span><span class="sxs-lookup"><span data-stu-id="dec11-150">Other Azure services will be added this year.</span></span>

## <a name="what-can-i-do-with-event-grid"></a><span data-ttu-id="dec11-151">Vad kan jag göra med händelsen rutnät?</span><span class="sxs-lookup"><span data-stu-id="dec11-151">What can I do with Event Grid?</span></span>

<span data-ttu-id="dec11-152">Azure händelse rutnätet innehåller flera funktioner som kan förbättrar frågeprestanda avsevärt serverlösa ops automation och integrering arbete:</span><span class="sxs-lookup"><span data-stu-id="dec11-152">Azure Event Grid provides several capabilities that vastly improve serverless, ops automation, and integration work:</span></span> 

### <a name="serverless-application-architectures"></a><span data-ttu-id="dec11-153">Arkitekturer för program utan server</span><span class="sxs-lookup"><span data-stu-id="dec11-153">Serverless application architectures</span></span>

![Program](./media/overview/serverless_web_app.png)

<span data-ttu-id="dec11-155">Event Grid kopplar samman datakällor och händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="dec11-155">Event Grid connects data sources and event handlers.</span></span> <span data-ttu-id="dec11-156">Använd exempelvis Event Grid för att direkt utlösa att en funktion utan server kör bildanalys varje gång ett nytt foto läggs till i en blobblagringsbehållare.</span><span class="sxs-lookup"><span data-stu-id="dec11-156">For example, use Event Grid to instantly trigger a serverless function to run image analysis each time a new photo is added to a blob storage container.</span></span> 

### <a name="ops-automation"></a><span data-ttu-id="dec11-157">OPS Automation</span><span class="sxs-lookup"><span data-stu-id="dec11-157">Ops Automation</span></span>

![Automatisering av åtgärder](./media/overview/Ops_automation.png)

<span data-ttu-id="dec11-159">Event Grid ger snabbare automatisering och enklare principtillämpning.</span><span class="sxs-lookup"><span data-stu-id="dec11-159">Event Grid allows you to speed automation and simplify policy enforcement.</span></span> <span data-ttu-id="dec11-160">Event Grid kan exempelvis meddela Azure Automation när en virtuell dator skapas eller en SQL-databas startas.</span><span class="sxs-lookup"><span data-stu-id="dec11-160">For example, Event Grid can notify Azure Automation when a virtual machine is created, or a SQL Database is spun up.</span></span> <span data-ttu-id="dec11-161">De här händelserna kan användas för att automatiskt kontrollera att tjänstkonfigurationer följer standard, placera metadata i åtgärdsverktyg, tagga virtuella datorer eller arkivera arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="dec11-161">These events can be used to automatically check that service configurations are compliant, put metadata into operations tools, tag virtual machines, or file work items.</span></span>

### <a name="application-integration"></a><span data-ttu-id="dec11-162">Integrering av program</span><span class="sxs-lookup"><span data-stu-id="dec11-162">Application integration</span></span>

![Integrering av program](./media/overview/app_integration.png)

<span data-ttu-id="dec11-164">Event Grid ansluter din app till andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="dec11-164">Event Grid connects your app with other services.</span></span> <span data-ttu-id="dec11-165">Till exempel skapa en anpassad avsnittet för att skicka data om din app till händelse rutnätet och dra nytta av dess tillförlitlig leverans, avancerad routning, och dirigera integrering med Azure.</span><span class="sxs-lookup"><span data-stu-id="dec11-165">For example, create a custom topic to send your app's event data to Event Grid, and take advantage of its reliable delivery, advanced routing, and direct integration with Azure.</span></span> <span data-ttu-id="dec11-166">Du kan även använda Event Grid med Logic Apps för att bearbeta data var som helst, utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="dec11-166">Alternatively, you can use Event Grid with Logic Apps to process data anywhere, without writing code.</span></span> 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a><span data-ttu-id="dec11-167">Hur skiljer händelse rutnätet från andra Azure integration services?</span><span class="sxs-lookup"><span data-stu-id="dec11-167">How is Event Grid different from other Azure integration services?</span></span>

<span data-ttu-id="dec11-168">Händelsen rutnätet är en eventing bakplan som möjliggör händelsedriven, reaktiv programmering.</span><span class="sxs-lookup"><span data-stu-id="dec11-168">Event Grid is an eventing backplane that enables event-driven, reactive programming.</span></span> <span data-ttu-id="dec11-169">Den är helt integrerat med Azure-tjänster och kan integreras med tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="dec11-169">It is deeply integrated with Azure services and can be integrated with third-party services.</span></span> <span data-ttu-id="dec11-170">Händelsemeddelandet innehåller den information du behöver ta hänsyn till ändringar i tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="dec11-170">The event message contains the information you need to react to changes in services and applications.</span></span> <span data-ttu-id="dec11-171">Händelsen rutnätet är inte en data-pipeline och leverera inte det faktiska objektet har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="dec11-171">Event Grid is not a data pipeline, and does not deliver the actual object that was updated.</span></span>

<span data-ttu-id="dec11-172">Service Bus passar bra för traditionella företagsprogram som kräver transaktioner, ordning, dubblettidentifiering och omedelbar konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="dec11-172">Service Bus is well suited for traditional enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency.</span></span> <span data-ttu-id="dec11-173">Händelsen rutnätet är utformad för hastighet, skala, bredd och billig reaktiv modellen.</span><span class="sxs-lookup"><span data-stu-id="dec11-173">Event Grid is designed for speed, scale, breadth, and low cost in a reactive model.</span></span> <span data-ttu-id="dec11-174">Det är bra att serverlösa arkitektur.</span><span class="sxs-lookup"><span data-stu-id="dec11-174">It is well suited to serverless architecture.</span></span>

<span data-ttu-id="dec11-175">Händelsen rutnätet kompletterar andra Azure-tjänster som Logic Apps och Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="dec11-175">Event Grid complements other Azure services like Logic Apps and Event Hubs.</span></span> <span data-ttu-id="dec11-176">Händelsen rutnätet utlöser logikappen om du vill starta arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="dec11-176">Event Grid triggers the logic app to begin its workflow.</span></span> <span data-ttu-id="dec11-177">Händelsehubbar fungerar med händelsen rutnätet genom att du kan reagera på händelser från Event Hubs avbilda och skapa pipelines för ingång och omvandling av data.</span><span class="sxs-lookup"><span data-stu-id="dec11-177">Event Hubs works with Event Grid by enabling you to react to events from Event Hubs Capture, and build data ingress and transformation pipelines.</span></span>

## <a name="how-much-does-event-grid-cost"></a><span data-ttu-id="dec11-178">Hur mycket kostar händelse rutnätet?</span><span class="sxs-lookup"><span data-stu-id="dec11-178">How much does Event Grid cost?</span></span>

<span data-ttu-id="dec11-179">Azure händelse rutnätet använder en modell med betalning per händelse prisnivå så att du betalar bara för att du använder.</span><span class="sxs-lookup"><span data-stu-id="dec11-179">Azure Event Grid uses a pay-per-event pricing model, so you only pay for what you use.</span></span>

<span data-ttu-id="dec11-180">Händelsen rutnätet kostnaden $0,60 per miljoner operations ($0,30 under förhandsgranskning) och åtgärden först 100 000 per månad är gratis.</span><span class="sxs-lookup"><span data-stu-id="dec11-180">Event Grid costs $0.60 per million operations ($0.30 during preview) and the first 100,000 operation per month are free.</span></span> <span data-ttu-id="dec11-181">Åtgärder definieras som händelsen ingång, avancerad matchar, leverans försök och management-anrop.</span><span class="sxs-lookup"><span data-stu-id="dec11-181">Operations are defined as event ingress, advanced match, delivery attempt, and management calls.</span></span>  <span data-ttu-id="dec11-182">Mer information finns på den [sida med priser](https://azure.microsoft.com/pricing/details/event-grid/).</span><span class="sxs-lookup"><span data-stu-id="dec11-182">More details can be found on the [pricing page](https://azure.microsoft.com/pricing/details/event-grid/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dec11-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dec11-183">Next steps</span></span>

* <span data-ttu-id="dec11-184">[Skapa och prenumerera på anpassade händelser](custom-event-quickstart.md) igång direkt och börja skicka dina egna anpassade händelser för valfri slutpunkt med hjälp av Azure händelse rutnätet Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="dec11-184">[Create and subscribe to custom events](custom-event-quickstart.md) Jump right in and start sending your own custom events to any endpoint using the Azure Event Grid quickstart.</span></span>
* <span data-ttu-id="dec11-185">[Med hjälp av Logic Apps som händelsehanterare](monitor-virtual-machine-changes-event-grid-logic-app.md) en självstudiekurs om hur du skapar en app med Logic Apps för att reagera på händelser pushas efter händelsen rutnät.</span><span class="sxs-lookup"><span data-stu-id="dec11-185">[Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) A tutorial on building an app using Logic Apps to react to events pushed by Event Grid.</span></span>
* [<span data-ttu-id="dec11-186">Händelsen rutnätet REST API-referens</span><span class="sxs-lookup"><span data-stu-id="dec11-186">Event Grid REST API reference</span></span>](/rest/api/eventgrid)  
  <span data-ttu-id="dec11-187">Ger mer teknisk information om rutnätet Azure händelse och en referens för att hantera prenumerationer på händelser, Routning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="dec11-187">Provides more technical information about the Azure Event Grid, and a reference for managing Event Subscriptions, routing, and filtering.</span></span>