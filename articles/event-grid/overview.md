---
title: "aaaAzure händelse rutnätet översikt"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a><span data-ttu-id="005e5-103">En introduktion tooAzure händelse rutnätet</span><span class="sxs-lookup"><span data-stu-id="005e5-103">An introduction tooAzure Event Grid</span></span>

<span data-ttu-id="005e5-104">Azure händelse rutnätet kan tooeasily bygga program med händelsebaserat arkitekturerna.</span><span class="sxs-lookup"><span data-stu-id="005e5-104">Azure Event Grid allows you tooeasily build applications with event-based architectures.</span></span> <span data-ttu-id="005e5-105">Du kan välja hello Azure-resurs du gillar toosubscribe till och ger hello händelsehanteraren eller WebHook endpoint toosend hello händelse.</span><span class="sxs-lookup"><span data-stu-id="005e5-105">You select hello Azure resource you would like toosubscribe to, and give hello event handler or WebHook endpoint toosend hello event to.</span></span> <span data-ttu-id="005e5-106">Händelsen rutnätet har inbyggt stöd för händelser som kommer från Azure-tjänster, som storage-blobbar och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="005e5-106">Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.</span></span> <span data-ttu-id="005e5-107">Händelsen rutnätet har också anpassade stöd för program och händelser från tredje part, med hjälp av anpassade avsnitt och anpassade webhooks.</span><span class="sxs-lookup"><span data-stu-id="005e5-107">Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.</span></span> 

<span data-ttu-id="005e5-108">Du kan använda filter tooroute specifika händelser toodifferent slutpunkter, multicast toomultiple slutpunkter och kontrollera att din händelser levereras på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="005e5-108">You can use filters tooroute specific events toodifferent endpoints, multicast toomultiple endpoints, and make sure your events are reliably delivered.</span></span> <span data-ttu-id="005e5-109">Händelsen rutnätet har också inbyggt stöd för anpassade och tredjeparts-händelser.</span><span class="sxs-lookup"><span data-stu-id="005e5-109">Event Grid also has built in support for custom and third-party events.</span></span>

<span data-ttu-id="005e5-110">Hello förhandsversionen händelse rutnätet stöder **westus2** och **westcentralus** platser.</span><span class="sxs-lookup"><span data-stu-id="005e5-110">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span> <span data-ttu-id="005e5-111">Kommer att läggas till andra regioner.</span><span class="sxs-lookup"><span data-stu-id="005e5-111">Other regions will be added.</span></span>

<span data-ttu-id="005e5-112">Den här artikeln innehåller en översikt över Azure händelse rutnätet.</span><span class="sxs-lookup"><span data-stu-id="005e5-112">This article provides an overview of Azure Event Grid.</span></span> <span data-ttu-id="005e5-113">Om du vill tooget igång med händelsen rutnätet finns [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="005e5-113">If you want tooget started with Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>

![Händelsen rutnätet funktionella modellen](./media/overview/event-grid-functional-model.png)

<span data-ttu-id="005e5-115">Blob Storage är för närvarande inte tillgänglig som en utgivare.</span><span class="sxs-lookup"><span data-stu-id="005e5-115">Currently, Blob Storage is not publicly available as a publisher.</span></span>

## <a name="concepts"></a><span data-ttu-id="005e5-116">Koncept</span><span class="sxs-lookup"><span data-stu-id="005e5-116">Concepts</span></span>

<span data-ttu-id="005e5-117">Det finns fem koncept i Azure händelse rutnät som gör att du kan komma igång:</span><span class="sxs-lookup"><span data-stu-id="005e5-117">There are five concepts in Azure Event Grid that let you get going:</span></span>

* <span data-ttu-id="005e5-118">**Händelser** -vad hände.</span><span class="sxs-lookup"><span data-stu-id="005e5-118">**Events** - What happened.</span></span>
* <span data-ttu-id="005e5-119">**Källor/händelseutfärdare** - när hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="005e5-119">**Event sources/publishers** - Where hello event took place.</span></span>
* <span data-ttu-id="005e5-120">**Avsnitt om** -hello slutpunkt där utgivare skicka händelser.</span><span class="sxs-lookup"><span data-stu-id="005e5-120">**Topics** - hello endpoint where publishers send events.</span></span>
* <span data-ttu-id="005e5-121">**Händelseprenumerationer** -hello slutpunkt eller inbyggda mekanism tooroute händelser, ibland toomultiple hanterare.</span><span class="sxs-lookup"><span data-stu-id="005e5-121">**Event subscriptions** - hello endpoint or built-in mechanism tooroute events, sometimes toomultiple handlers.</span></span> <span data-ttu-id="005e5-122">Prenumerationer används också av hanterare toointelligently filtrera inkommande händelser.</span><span class="sxs-lookup"><span data-stu-id="005e5-122">Subscriptions are also used by handlers toointelligently filter incoming events.</span></span>
* <span data-ttu-id="005e5-123">**Händelsehanterare** – hello app eller tjänst korsreagerande toohello händelse.</span><span class="sxs-lookup"><span data-stu-id="005e5-123">**Event handlers** - hello app or service reacting toohello event.</span></span>

<span data-ttu-id="005e5-124">Mer information om de här koncepten finns [koncept i Azure händelse rutnät](concepts.md).</span><span class="sxs-lookup"><span data-stu-id="005e5-124">For more information about these concepts, see [Concepts in Azure Event Grid](concepts.md).</span></span>

## <a name="capabilities"></a><span data-ttu-id="005e5-125">Funktioner</span><span class="sxs-lookup"><span data-stu-id="005e5-125">Capabilities</span></span>

<span data-ttu-id="005e5-126">Här följer några av hello viktiga funktioner i Azure händelse rutnätet:</span><span class="sxs-lookup"><span data-stu-id="005e5-126">Here are some of hello key features of Azure Event Grid:</span></span>

* <span data-ttu-id="005e5-127">**Enkelhet** -punkt och tooaim händelsen från Azure-resurs tooany händelsehanteraren eller slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="005e5-127">**Simplicity** - Point and click tooaim events from your Azure resource tooany event handler or endpoint.</span></span>
* <span data-ttu-id="005e5-128">**Avancerad filtrering** -Filter på händelsen typ eller händelse publicera sökvägen tooensure händelsehanterare får bara relevanta händelser.</span><span class="sxs-lookup"><span data-stu-id="005e5-128">**Advanced filtering** - Filter on event type or event publish path tooensure event handlers only receive relevant events.</span></span>
* <span data-ttu-id="005e5-129">**FAN-out** -prenumerera på flera slutpunkter toohello samma händelse toosend kopior av hello händelse tooas många platser efter behov.</span><span class="sxs-lookup"><span data-stu-id="005e5-129">**Fan-out** - Subscribe multiple endpoints toohello same event toosend copies of hello event tooas many places as needed.</span></span>
* <span data-ttu-id="005e5-130">**Tillförlitlighet** -använda 24-timmarsformat återförsök med exponentiell backoff tooensure händelser levereras.</span><span class="sxs-lookup"><span data-stu-id="005e5-130">**Reliability** - Utilize 24-hour retry with exponential backoff tooensure events are delivered.</span></span>
* <span data-ttu-id="005e5-131">**Betala per händelse** – betala endast för hello mycket du använder händelsen rutnätet.</span><span class="sxs-lookup"><span data-stu-id="005e5-131">**Pay-per-event** - Pay only for hello amount you use Event Grid.</span></span>
* <span data-ttu-id="005e5-132">**Hög genomströmning** -skapa omfattande arbetsbelastningar i händelse rutnät med stöd för miljontals händelser per sekund.</span><span class="sxs-lookup"><span data-stu-id="005e5-132">**High throughput** - Build high-volume workloads on Event Grid with support for millions of events per second.</span></span>
* <span data-ttu-id="005e5-133">**Inbyggda händelser** – komma igång snabbt och köra med resurs-definierade inbyggda händelser.</span><span class="sxs-lookup"><span data-stu-id="005e5-133">**Built-in Events** - Get up and running quickly with resource-defined built-in events.</span></span>
* <span data-ttu-id="005e5-134">**Anpassade händelser** -Använd händelse rutnätet flödet, filter och tillförlitligt leverera anpassade händelser i din app.</span><span class="sxs-lookup"><span data-stu-id="005e5-134">**Custom Events** - use Event Grid route, filter, and reliably deliver custom events in your app.</span></span>

## <a name="built-in-publisher-and-handler-integration"></a><span data-ttu-id="005e5-135">Inbyggd integrering av utgivare och hanterare</span><span class="sxs-lookup"><span data-stu-id="005e5-135">Built-in publisher and handler integration</span></span>

<span data-ttu-id="005e5-136">Azure erbjuder inbyggda händelsestöd med ett stort antal tjänster, inklusive både utgivare och hanterare.</span><span class="sxs-lookup"><span data-stu-id="005e5-136">Azure offers built-in event support using numerous services, including both publishers and handlers.</span></span>

### <a name="publishers"></a><span data-ttu-id="005e5-137">Utgivare</span><span class="sxs-lookup"><span data-stu-id="005e5-137">Publishers</span></span>

<span data-ttu-id="005e5-138">För närvarande har hello följande Azure-tjänster inbyggda utgivarens support för händelsen rutnät:</span><span class="sxs-lookup"><span data-stu-id="005e5-138">Currently, hello following Azure services have built-in publisher support for event grid:</span></span>

* <span data-ttu-id="005e5-139">Resursgrupper (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="005e5-139">Resource Groups (management operations)</span></span>
* <span data-ttu-id="005e5-140">Azure-prenumerationer (hanteringsåtgärder)</span><span class="sxs-lookup"><span data-stu-id="005e5-140">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="005e5-141">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="005e5-141">Event Hubs</span></span>
* <span data-ttu-id="005e5-142">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="005e5-142">Custom Topics</span></span>

<span data-ttu-id="005e5-143">Andra Azure-tjänster kommer att adderas i år.</span><span class="sxs-lookup"><span data-stu-id="005e5-143">Other Azure services will be added this year.</span></span>

### <a name="handlers"></a><span data-ttu-id="005e5-144">Hanterare</span><span class="sxs-lookup"><span data-stu-id="005e5-144">Handlers</span></span>

<span data-ttu-id="005e5-145">För närvarande har hello följande Azure-tjänster stöd för inbyggda hanterare för händelsen rutnätet:</span><span class="sxs-lookup"><span data-stu-id="005e5-145">Currently, hello following Azure services have built-in handler support for Event Grid:</span></span> 

* <span data-ttu-id="005e5-146">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="005e5-146">Azure Functions</span></span>
* <span data-ttu-id="005e5-147">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="005e5-147">Logic Apps</span></span>
* <span data-ttu-id="005e5-148">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="005e5-148">Azure Automation</span></span>
* <span data-ttu-id="005e5-149">WebHooks</span><span class="sxs-lookup"><span data-stu-id="005e5-149">WebHooks</span></span>

<span data-ttu-id="005e5-150">Andra Azure-tjänster kommer att adderas i år.</span><span class="sxs-lookup"><span data-stu-id="005e5-150">Other Azure services will be added this year.</span></span>

## <a name="what-can-i-do-with-event-grid"></a><span data-ttu-id="005e5-151">Vad kan jag göra med händelsen rutnät?</span><span class="sxs-lookup"><span data-stu-id="005e5-151">What can I do with Event Grid?</span></span>

<span data-ttu-id="005e5-152">Azure händelse rutnätet innehåller flera funktioner som kan förbättrar frågeprestanda avsevärt serverlösa ops automation och integrering arbete:</span><span class="sxs-lookup"><span data-stu-id="005e5-152">Azure Event Grid provides several capabilities that vastly improve serverless, ops automation, and integration work:</span></span> 

### <a name="serverless-application-architectures"></a><span data-ttu-id="005e5-153">Arkitekturer för program utan server</span><span class="sxs-lookup"><span data-stu-id="005e5-153">Serverless application architectures</span></span>

![Program](./media/overview/serverless_web_app.png)

<span data-ttu-id="005e5-155">Event Grid kopplar samman datakällor och händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="005e5-155">Event Grid connects data sources and event handlers.</span></span> <span data-ttu-id="005e5-156">Till exempel använda händelsen rutnätet tooinstantly utlösaren serverlösa funktionen toorun avbildningen analys varje gång en ny är läggs tooa blob storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="005e5-156">For example, use Event Grid tooinstantly trigger a serverless function toorun image analysis each time a new photo is added tooa blob storage container.</span></span> 

### <a name="ops-automation"></a><span data-ttu-id="005e5-157">OPS Automation</span><span class="sxs-lookup"><span data-stu-id="005e5-157">Ops Automation</span></span>

![Automatisering av åtgärder](./media/overview/Ops_automation.png)

<span data-ttu-id="005e5-159">Händelsen rutnätet kan du toospeed automation och förenkla tvingande principer.</span><span class="sxs-lookup"><span data-stu-id="005e5-159">Event Grid allows you toospeed automation and simplify policy enforcement.</span></span> <span data-ttu-id="005e5-160">Event Grid kan exempelvis meddela Azure Automation när en virtuell dator skapas eller en SQL-databas startas.</span><span class="sxs-lookup"><span data-stu-id="005e5-160">For example, Event Grid can notify Azure Automation when a virtual machine is created, or a SQL Database is spun up.</span></span> <span data-ttu-id="005e5-161">Dessa händelser kan användas för tooautomatically Kontrollera tjänstkonfiguration är kompatibel, placera metadata i operations-verktyg, taggen virtuella datorer eller filen arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="005e5-161">These events can be used tooautomatically check that service configurations are compliant, put metadata into operations tools, tag virtual machines, or file work items.</span></span>

### <a name="application-integration"></a><span data-ttu-id="005e5-162">Integrering av program</span><span class="sxs-lookup"><span data-stu-id="005e5-162">Application integration</span></span>

![Integrering av program](./media/overview/app_integration.png)

<span data-ttu-id="005e5-164">Event Grid ansluter din app till andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="005e5-164">Event Grid connects your app with other services.</span></span> <span data-ttu-id="005e5-165">Exempelvis skapa en anpassad avsnittet toosend appens händelse data tooEvent rutnätet och dra nytta av dess tillförlitlig leverans, avancerad routning, och direktintegrering med Azure.</span><span class="sxs-lookup"><span data-stu-id="005e5-165">For example, create a custom topic toosend your app's event data tooEvent Grid, and take advantage of its reliable delivery, advanced routing, and direct integration with Azure.</span></span> <span data-ttu-id="005e5-166">Du kan också använda händelsen rutnät med Logic Apps tooprocess data var som helst, utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="005e5-166">Alternatively, you can use Event Grid with Logic Apps tooprocess data anywhere, without writing code.</span></span> 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a><span data-ttu-id="005e5-167">Hur skiljer händelse rutnätet från andra Azure integration services?</span><span class="sxs-lookup"><span data-stu-id="005e5-167">How is Event Grid different from other Azure integration services?</span></span>

<span data-ttu-id="005e5-168">Händelsen rutnätet är en eventing bakplan som möjliggör händelsedriven, reaktiv programmering.</span><span class="sxs-lookup"><span data-stu-id="005e5-168">Event Grid is an eventing backplane that enables event-driven, reactive programming.</span></span> <span data-ttu-id="005e5-169">Den är helt integrerat med Azure-tjänster och kan integreras med tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="005e5-169">It is deeply integrated with Azure services and can be integrated with third-party services.</span></span> <span data-ttu-id="005e5-170">hello Händelsemeddelandet innehåller hello information du behöver tooreact toochanges i tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="005e5-170">hello event message contains hello information you need tooreact toochanges in services and applications.</span></span> <span data-ttu-id="005e5-171">Händelsen rutnätet är inte en data-pipeline och leverera inte hello faktiska objekt som har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="005e5-171">Event Grid is not a data pipeline, and does not deliver hello actual object that was updated.</span></span>

<span data-ttu-id="005e5-172">Service Bus passar bra för traditionella företagsprogram som kräver transaktioner, ordning, dubblettidentifiering och omedelbar konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="005e5-172">Service Bus is well suited for traditional enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency.</span></span> <span data-ttu-id="005e5-173">Händelsen rutnätet är utformad för hastighet, skala, bredd och billig reaktiv modellen.</span><span class="sxs-lookup"><span data-stu-id="005e5-173">Event Grid is designed for speed, scale, breadth, and low cost in a reactive model.</span></span> <span data-ttu-id="005e5-174">Det är bra tooserverless arkitektur.</span><span class="sxs-lookup"><span data-stu-id="005e5-174">It is well suited tooserverless architecture.</span></span>

<span data-ttu-id="005e5-175">Händelsen rutnätet kompletterar andra Azure-tjänster som Logic Apps och Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="005e5-175">Event Grid complements other Azure services like Logic Apps and Event Hubs.</span></span> <span data-ttu-id="005e5-176">Händelseutlösare rutnätet hello logik app toobegin dess arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="005e5-176">Event Grid triggers hello logic app toobegin its workflow.</span></span> <span data-ttu-id="005e5-177">Händelsehubbar fungerar med händelsen rutnätet genom att aktivera du tooreact tooevents från Event Hubs avbilda och bygga data ingång och omvandling pipelines.</span><span class="sxs-lookup"><span data-stu-id="005e5-177">Event Hubs works with Event Grid by enabling you tooreact tooevents from Event Hubs Capture, and build data ingress and transformation pipelines.</span></span>

## <a name="how-much-does-event-grid-cost"></a><span data-ttu-id="005e5-178">Hur mycket kostar händelse rutnätet?</span><span class="sxs-lookup"><span data-stu-id="005e5-178">How much does Event Grid cost?</span></span>

<span data-ttu-id="005e5-179">Azure händelse rutnätet använder en modell med betalning per händelse prisnivå så att du betalar bara för att du använder.</span><span class="sxs-lookup"><span data-stu-id="005e5-179">Azure Event Grid uses a pay-per-event pricing model, so you only pay for what you use.</span></span>

<span data-ttu-id="005e5-180">Händelsen rutnätet kostnaden $0,60 per miljoner operations ($0,30 under förhandsgranskning) och hello först 100 000 åtgärden per månad är gratis.</span><span class="sxs-lookup"><span data-stu-id="005e5-180">Event Grid costs $0.60 per million operations ($0.30 during preview) and hello first 100,000 operation per month are free.</span></span> <span data-ttu-id="005e5-181">Åtgärder definieras som händelsen ingång, avancerad matchar, leverans försök och management-anrop.</span><span class="sxs-lookup"><span data-stu-id="005e5-181">Operations are defined as event ingress, advanced match, delivery attempt, and management calls.</span></span>  <span data-ttu-id="005e5-182">Mer information finns på hello [sida med priser](https://azure.microsoft.com/pricing/details/event-grid/).</span><span class="sxs-lookup"><span data-stu-id="005e5-182">More details can be found on hello [pricing page](https://azure.microsoft.com/pricing/details/event-grid/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="005e5-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="005e5-183">Next steps</span></span>

* <span data-ttu-id="005e5-184">[Skapa och prenumerera toocustom händelser](custom-event-quickstart.md) igång direkt och börja skicka dina egna anpassade händelser tooany slutpunkt med hello Azure händelse rutnätet Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="005e5-184">[Create and subscribe toocustom events](custom-event-quickstart.md) Jump right in and start sending your own custom events tooany endpoint using hello Azure Event Grid quickstart.</span></span>
* <span data-ttu-id="005e5-185">[Med hjälp av Logic Apps som händelsehanterare](monitor-virtual-machine-changes-event-grid-logic-app.md) en självstudiekurs om hur du skapar en app med Logic Apps tooreact tooevents pushas efter händelsen rutnät.</span><span class="sxs-lookup"><span data-stu-id="005e5-185">[Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) A tutorial on building an app using Logic Apps tooreact tooevents pushed by Event Grid.</span></span>
* [<span data-ttu-id="005e5-186">Händelsen rutnätet REST API-referens</span><span class="sxs-lookup"><span data-stu-id="005e5-186">Event Grid REST API reference</span></span>](/rest/api/eventgrid)  
  <span data-ttu-id="005e5-187">Ger mer teknisk information om hello Azure händelse rutnätet och en referens för att hantera prenumerationer på händelser, Routning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="005e5-187">Provides more technical information about hello Azure Event Grid, and a reference for managing Event Subscriptions, routing, and filtering.</span></span>
