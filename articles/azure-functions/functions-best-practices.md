---
title: "aaaBest praxis för Azure Functions | Microsoft Docs"
description: "Läs metodtips och mönster för Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, mönster, bästa praxis, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/13/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd3d826b75267a740e355b008943a48f71ebd339
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="f0e2a-104">Optimera hello prestanda och tillförlitlighet i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f0e2a-104">Optimize hello performance and reliability of Azure Functions</span></span>

<span data-ttu-id="f0e2a-105">Den här artikeln innehåller vägledning tooimprove hello prestanda och tillförlitlighet för funktionen appar.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-105">This article provides guidance tooimprove hello performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="f0e2a-106">Undvika långvariga funktioner</span><span class="sxs-lookup"><span data-stu-id="f0e2a-106">Avoid long running functions</span></span>

<span data-ttu-id="f0e2a-107">Stora, långvariga funktioner kan orsaka oväntade timeout problem.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="f0e2a-108">En funktion kan bli stora på grund av toomany Node.js-beroenden.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-108">A function can become large due toomany Node.js dependencies.</span></span> <span data-ttu-id="f0e2a-109">Importera beroenden kan också medföra ökad belastning gånger som resultera i oväntade timeout.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="f0e2a-110">Beroenden laddas både uttryckligen och implicit.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="f0e2a-111">En enda modul som lästs in av din kod kan läsa in en egen ytterligare moduler.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="f0e2a-112">När det är möjligt, flytta stora funktionerna i mindre funktionen anger som fungerar tillsammans och returnera svar snabbt.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="f0e2a-113">Till exempel kan en webhook eller HTTP-Utlösarfunktion kräva ett bekräftelse-svar inom en viss tid; Det är vanligt att webhooks toorequire en omedelbara åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks toorequire an immediate response.</span></span> <span data-ttu-id="f0e2a-114">Du kan överföra hello http-utlösaren nyttolast i en kö toobe som bearbetas av en kö Utlösarfunktion.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-114">You can pass hello HTTP trigger payload into a queue toobe processed by a queue trigger function.</span></span> <span data-ttu-id="f0e2a-115">Den här metoden kan du toodefer hello verkligt arbete och returnera ett omedelbart svar.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-115">This approach allows you toodefer hello actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="f0e2a-116">Funktionen kommunikation mellan</span><span class="sxs-lookup"><span data-stu-id="f0e2a-116">Cross function communication</span></span>

<span data-ttu-id="f0e2a-117">När du integrerar flera funktioner, men det är oftast en bästa praxis toouse lagringsköer för mellan funktionen kommunikation.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-117">When integrating multiple functions, it is generally a best practice toouse storage queues for cross function communication.</span></span>  <span data-ttu-id="f0e2a-118">hello främsta skälet är lagringsköer är billigare och mycket enklare tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-118">hello main reason is storage queues are cheaper and much easier tooprovision.</span></span> 

<span data-ttu-id="f0e2a-119">Enskilda meddelanden i en kö för lagring är begränsade i storlek too64 KB.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-119">Individual messages in a storage queue are limited in size too64 KB.</span></span> <span data-ttu-id="f0e2a-120">Om toopass större meddelanden mellan funktioner måste vara en Azure Service Bus-kö används toosupport meddelandestorleken in too256 KB.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-120">If you need toopass larger messages between functions, an Azure Service Bus queue could be used toosupport message sizes up too256 KB.</span></span>

<span data-ttu-id="f0e2a-121">Service Bus-ämnen är användbara om du behöver meddelande filtrering innan bearbetning.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="f0e2a-122">Händelsehubbar är användbara toosupport hög volym kommunikation.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-122">Event hubs are useful toosupport high volume communications.</span></span>


## <a name="write-functions-toobe-stateless"></a><span data-ttu-id="f0e2a-123">Skriva funktioner toobe tillståndslös</span><span class="sxs-lookup"><span data-stu-id="f0e2a-123">Write functions toobe stateless</span></span> 

<span data-ttu-id="f0e2a-124">Funktioner som ska vara tillståndslösa och idempotent om möjligt.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="f0e2a-125">Koppla statusinformation som krävs till dina data.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-125">Associate any required state information with your data.</span></span> <span data-ttu-id="f0e2a-126">Till exempel en order bearbetas skulle förmodligen ha en associerad `state` medlem.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="f0e2a-127">En funktion kan bearbeta en ordning som baseras på det aktuella tillståndet eftersom själva hello funktionen förblir tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-127">A function could process an order based on that state while hello function itself remains stateless.</span></span> 

<span data-ttu-id="f0e2a-128">Idempotent funktioner är särskilt användbart med timer-utlösare.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="f0e2a-129">Till exempel om du har något som verkligen måste köras en gång om dagen, skriva den så den kan köras under hello dag med hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during hello day with hello same results.</span></span> <span data-ttu-id="f0e2a-130">hello-funktionen kan avsluta när det finns inget arbete för en viss dag.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-130">hello function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="f0e2a-131">Även om en tidigare körning misslyckades toocomplete ska hello kör nästa gång hämta där den avbröts.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-131">Also if a previous run failed toocomplete, hello next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="f0e2a-132">Skriva defensiva funktioner</span><span class="sxs-lookup"><span data-stu-id="f0e2a-132">Write defensive functions</span></span>

<span data-ttu-id="f0e2a-133">Anta att din funktion kan uppstå ett undantag när som helst.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="f0e2a-134">Utforma dina funktioner med hello möjlighet toocontinue från en tidigare punkt misslyckas under hello nästa körning.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-134">Design your functions with hello ability toocontinue from a previous fail point during hello next execution.</span></span> <span data-ttu-id="f0e2a-135">Överväg ett scenario som kräver hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="f0e2a-135">Consider a scenario that requires hello following actions:</span></span>

1. <span data-ttu-id="f0e2a-136">Frågan för 10 000 rader i en databas.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="f0e2a-137">Skapa ett kömeddelande för var och en av dessa rader tooprocess ytterligare hello rad nedåt.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-137">Create a queue message for each of those rows tooprocess further down hello line.</span></span>
 
<span data-ttu-id="f0e2a-138">Beroende på hur komplexa datorn är kanske: ingår nedströms tjänster fungerar felaktigt, nätverk avbrott eller kvot begränsar nått, etc. Alla dessa kan påverka funktionen när som helst.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="f0e2a-139">Du måste toodesign dina funktioner toobe förberedd för den.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-139">You need toodesign your functions toobe prepared for it.</span></span>

<span data-ttu-id="f0e2a-140">Hur koden reagera om ett fel uppstår när du har infogat 5 000 av dessa objekt i en kö för bearbetning?</span><span class="sxs-lookup"><span data-stu-id="f0e2a-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="f0e2a-141">Spåra objekt i en samling som du har slutfört.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="f0e2a-142">Annars kan kan du lägga till dem igen nästa gång.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="f0e2a-143">Detta kan ha en betydande del av ditt arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="f0e2a-144">Om ett köobjekt redan har bearbetats, Tillåt funktionen-toobe en no-op.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-144">If a queue item was already processed, allow your function toobe a no-op.</span></span>

<span data-ttu-id="f0e2a-145">Dra nytta av försvarsåtgärder som redan ges för komponenter som du använder i hello Azure Functions-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-145">Take advantage of defensive measures already provided for components you use in hello Azure Functions platform.</span></span> <span data-ttu-id="f0e2a-146">Se exempelvis **hantering av skadligt Kömeddelanden** i hello-dokumentationen för [Azure Storage-kö utlöser](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="f0e2a-146">For example, see **Handling poison queue messages** in hello documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a><span data-ttu-id="f0e2a-147">Inte blandning test- och kod i hello samma funktionsapp</span><span class="sxs-lookup"><span data-stu-id="f0e2a-147">Don't mix test and production code in hello same function app</span></span>

<span data-ttu-id="f0e2a-148">Funktioner i en funktionsapp dela resurser.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-148">Functions within a function app share resources.</span></span> <span data-ttu-id="f0e2a-149">Till exempel delas minne.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-149">For example, memory is shared.</span></span> <span data-ttu-id="f0e2a-150">Om du använder en funktionsapp i produktion, Lägg inte till test-relaterade funktioner och resurser tooit.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-150">If you're using a function app in production, don't add test-related functions and resources tooit.</span></span> <span data-ttu-id="f0e2a-151">Det kan orsaka oväntade kostnader under kodkörning för produktion.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="f0e2a-152">Var noga med att du laddar i dina appar för produktion-funktionen.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="f0e2a-153">Minne beräknas för varje funktion i hello app.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-153">Memory is averaged across each function in hello app.</span></span>

<span data-ttu-id="f0e2a-154">Om du har en delad sammansättning som refereras i flera .net-funktioner kan du placera den i en gemensam delad mapp.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="f0e2a-155">Referens hello sammansättning med en instruktion liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="f0e2a-155">Reference hello assembly with a statement similar toohello following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="f0e2a-156">I annat fall är det enkelt tooaccidentally distribuera flera testversioner av hello samma binary som fungerar annorlunda mellan fungerar.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-156">Otherwise, it is easy tooaccidentally deploy multiple test versions of hello same binary that behave differently between functions.</span></span>

<span data-ttu-id="f0e2a-157">Använd inte utförlig loggning i kod.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="f0e2a-158">Den har en negativ inverkan på prestanda.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="f0e2a-159">Använda async-kod men Undvik blockerar anrop</span><span class="sxs-lookup"><span data-stu-id="f0e2a-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="f0e2a-160">Asynkron programmering är en rekommenderad metod.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="f0e2a-161">Dock alltid undvika refererar till hello `Result` egenskap eller anropa `Wait` metod i en `Task` instans.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-161">However, always avoid referencing hello `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="f0e2a-162">Den här metoden kan leda toothread resursuttömning.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-162">This approach can lead toothread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="f0e2a-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0e2a-163">Next steps</span></span>
<span data-ttu-id="f0e2a-164">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f0e2a-164">For more information, see hello following resources:</span></span>

<span data-ttu-id="f0e2a-165">Eftersom Azure Functions använder Azure App Service, bör du också vara medveten om Apptjänst riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="f0e2a-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="f0e2a-166">Patterns and Practices HTTP prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="f0e2a-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

