---
title: "Metodtips för Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: 645a5dd16e72619e7c2470ab8f03098f0fa6c7f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="d9427-104">Optimera prestanda och tillförlitlighet i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="d9427-104">Optimize the performance and reliability of Azure Functions</span></span>

<span data-ttu-id="d9427-105">Den här artikeln innehåller anvisningar för att förbättra prestanda och tillförlitlighet för funktionen appar.</span><span class="sxs-lookup"><span data-stu-id="d9427-105">This article provides guidance to improve the performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="d9427-106">Undvika långvariga funktioner</span><span class="sxs-lookup"><span data-stu-id="d9427-106">Avoid long running functions</span></span>

<span data-ttu-id="d9427-107">Stora, långvariga funktioner kan orsaka oväntade timeout problem.</span><span class="sxs-lookup"><span data-stu-id="d9427-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="d9427-108">En funktion kan bli stora på grund av många Node.js-beroenden.</span><span class="sxs-lookup"><span data-stu-id="d9427-108">A function can become large due to many Node.js dependencies.</span></span> <span data-ttu-id="d9427-109">Importera beroenden kan också medföra ökad belastning gånger som resultera i oväntade timeout.</span><span class="sxs-lookup"><span data-stu-id="d9427-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="d9427-110">Beroenden laddas både uttryckligen och implicit.</span><span class="sxs-lookup"><span data-stu-id="d9427-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="d9427-111">En enda modul som lästs in av din kod kan läsa in en egen ytterligare moduler.</span><span class="sxs-lookup"><span data-stu-id="d9427-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="d9427-112">När det är möjligt, flytta stora funktionerna i mindre funktionen anger som fungerar tillsammans och returnera svar snabbt.</span><span class="sxs-lookup"><span data-stu-id="d9427-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="d9427-113">Till exempel kan en webhook eller HTTP-Utlösarfunktion kräva ett bekräftelse-svar inom en viss tid; Det är vanligt att webhooks kräver ett omedelbart svar.</span><span class="sxs-lookup"><span data-stu-id="d9427-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks to require an immediate response.</span></span> <span data-ttu-id="d9427-114">Du kan skicka HTTP-utlösaren nyttolast i en kö för att kunna bearbetas av en kö Utlösarfunktion.</span><span class="sxs-lookup"><span data-stu-id="d9427-114">You can pass the HTTP trigger payload into a queue to be processed by a queue trigger function.</span></span> <span data-ttu-id="d9427-115">Den här metoden kan du skjuta upp det faktiska arbetet och returnera ett omedelbart svar.</span><span class="sxs-lookup"><span data-stu-id="d9427-115">This approach allows you to defer the actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="d9427-116">Funktionen kommunikation mellan</span><span class="sxs-lookup"><span data-stu-id="d9427-116">Cross function communication</span></span>

<span data-ttu-id="d9427-117">När du integrerar flera funktioner, men det är vanligtvis en bra idé att använda lagringsköer för mellan funktionen kommunikation.</span><span class="sxs-lookup"><span data-stu-id="d9427-117">When integrating multiple functions, it is generally a best practice to use storage queues for cross function communication.</span></span>  <span data-ttu-id="d9427-118">Det främsta skälet är lagringsköer är billigare och enklare att etablera.</span><span class="sxs-lookup"><span data-stu-id="d9427-118">The main reason is storage queues are cheaper and much easier to provision.</span></span> 

<span data-ttu-id="d9427-119">Enskilda meddelanden i en kö för lagring är begränsade i storlek till 64 KB.</span><span class="sxs-lookup"><span data-stu-id="d9427-119">Individual messages in a storage queue are limited in size to 64 KB.</span></span> <span data-ttu-id="d9427-120">Om du behöver skicka större meddelanden mellan funktioner storlek en Azure Service Bus-kö kan användas för att stödja meddelande upp till 256 KB.</span><span class="sxs-lookup"><span data-stu-id="d9427-120">If you need to pass larger messages between functions, an Azure Service Bus queue could be used to support message sizes up to 256 KB.</span></span>

<span data-ttu-id="d9427-121">Service Bus-ämnen är användbara om du behöver meddelande filtrering innan bearbetning.</span><span class="sxs-lookup"><span data-stu-id="d9427-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="d9427-122">Händelsehubbar är användbara för att stödja kommunikation med hög volym.</span><span class="sxs-lookup"><span data-stu-id="d9427-122">Event hubs are useful to support high volume communications.</span></span>


## <a name="write-functions-to-be-stateless"></a><span data-ttu-id="d9427-123">Skriva funktioner ska tillståndslös</span><span class="sxs-lookup"><span data-stu-id="d9427-123">Write functions to be stateless</span></span> 

<span data-ttu-id="d9427-124">Funktioner som ska vara tillståndslösa och idempotent om möjligt.</span><span class="sxs-lookup"><span data-stu-id="d9427-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="d9427-125">Koppla statusinformation som krävs till dina data.</span><span class="sxs-lookup"><span data-stu-id="d9427-125">Associate any required state information with your data.</span></span> <span data-ttu-id="d9427-126">Till exempel en order bearbetas skulle förmodligen ha en associerad `state` medlem.</span><span class="sxs-lookup"><span data-stu-id="d9427-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="d9427-127">En funktion kan bearbeta en ordning som baseras på det aktuella tillståndet eftersom själva funktionen förblir tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="d9427-127">A function could process an order based on that state while the function itself remains stateless.</span></span> 

<span data-ttu-id="d9427-128">Idempotent funktioner är särskilt användbart med timer-utlösare.</span><span class="sxs-lookup"><span data-stu-id="d9427-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="d9427-129">Till exempel om du har något som verkligen måste köras en gång om dagen, skriva den så den kan köras när under dagen med samma resultat.</span><span class="sxs-lookup"><span data-stu-id="d9427-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during the day with the same results.</span></span> <span data-ttu-id="d9427-130">Funktionen kan avsluta när det finns inget arbete för en viss dag.</span><span class="sxs-lookup"><span data-stu-id="d9427-130">The function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="d9427-131">Även om en tidigare körning kunde inte slutföra bör nästa körning hämta där den avbröts.</span><span class="sxs-lookup"><span data-stu-id="d9427-131">Also if a previous run failed to complete, the next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="d9427-132">Skriva defensiva funktioner</span><span class="sxs-lookup"><span data-stu-id="d9427-132">Write defensive functions</span></span>

<span data-ttu-id="d9427-133">Anta att din funktion kan uppstå ett undantag när som helst.</span><span class="sxs-lookup"><span data-stu-id="d9427-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="d9427-134">Utforma dina funktioner med möjligheten att fortsätta från en tidigare punkt misslyckas vid nästa körning.</span><span class="sxs-lookup"><span data-stu-id="d9427-134">Design your functions with the ability to continue from a previous fail point during the next execution.</span></span> <span data-ttu-id="d9427-135">Överväg ett scenario som kräver följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="d9427-135">Consider a scenario that requires the following actions:</span></span>

1. <span data-ttu-id="d9427-136">Frågan för 10 000 rader i en databas.</span><span class="sxs-lookup"><span data-stu-id="d9427-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="d9427-137">Skapa ett kömeddelande för var och en av de rader som ska bearbetas ytterligare ned.</span><span class="sxs-lookup"><span data-stu-id="d9427-137">Create a queue message for each of those rows to process further down the line.</span></span>
 
<span data-ttu-id="d9427-138">Beroende på hur komplexa datorn är kanske: ingår nedströms tjänster fungerar felaktigt, nätverk avbrott eller kvot begränsar nått, etc. Alla dessa kan påverka funktionen när som helst.</span><span class="sxs-lookup"><span data-stu-id="d9427-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="d9427-139">Måste du utformar dina funktioner att vara förberedd för den.</span><span class="sxs-lookup"><span data-stu-id="d9427-139">You need to design your functions to be prepared for it.</span></span>

<span data-ttu-id="d9427-140">Hur koden reagera om ett fel uppstår när du har infogat 5 000 av dessa objekt i en kö för bearbetning?</span><span class="sxs-lookup"><span data-stu-id="d9427-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="d9427-141">Spåra objekt i en samling som du har slutfört.</span><span class="sxs-lookup"><span data-stu-id="d9427-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="d9427-142">Annars kan kan du lägga till dem igen nästa gång.</span><span class="sxs-lookup"><span data-stu-id="d9427-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="d9427-143">Detta kan ha en betydande del av ditt arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d9427-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="d9427-144">Tillåt att funktionen ska vara en no-op om ett köobjekt redan hade bearbetats.</span><span class="sxs-lookup"><span data-stu-id="d9427-144">If a queue item was already processed, allow your function to be a no-op.</span></span>

<span data-ttu-id="d9427-145">Dra nytta av försvarsåtgärder som redan ges för komponenter som du använder i Azure Functions-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d9427-145">Take advantage of defensive measures already provided for components you use in the Azure Functions platform.</span></span> <span data-ttu-id="d9427-146">Se exempelvis **hantering av skadligt Kömeddelanden** i dokumentationen för [Azure Storage-kö utlöser](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="d9427-146">For example, see **Handling poison queue messages** in the documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a><span data-ttu-id="d9427-147">Blanda inte test- och koden i appen med samma funktion</span><span class="sxs-lookup"><span data-stu-id="d9427-147">Don't mix test and production code in the same function app</span></span>

<span data-ttu-id="d9427-148">Funktioner i en funktionsapp dela resurser.</span><span class="sxs-lookup"><span data-stu-id="d9427-148">Functions within a function app share resources.</span></span> <span data-ttu-id="d9427-149">Till exempel delas minne.</span><span class="sxs-lookup"><span data-stu-id="d9427-149">For example, memory is shared.</span></span> <span data-ttu-id="d9427-150">Om du använder en funktionsapp i produktion, Lägg inte till test-relaterade funktioner och resurser till den.</span><span class="sxs-lookup"><span data-stu-id="d9427-150">If you're using a function app in production, don't add test-related functions and resources to it.</span></span> <span data-ttu-id="d9427-151">Det kan orsaka oväntade kostnader under kodkörning för produktion.</span><span class="sxs-lookup"><span data-stu-id="d9427-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="d9427-152">Var noga med att du laddar i dina appar för produktion-funktionen.</span><span class="sxs-lookup"><span data-stu-id="d9427-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="d9427-153">Minne beräknas för varje funktion i appen.</span><span class="sxs-lookup"><span data-stu-id="d9427-153">Memory is averaged across each function in the app.</span></span>

<span data-ttu-id="d9427-154">Om du har en delad sammansättning som refereras i flera .net-funktioner kan du placera den i en gemensam delad mapp.</span><span class="sxs-lookup"><span data-stu-id="d9427-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="d9427-155">Referera till en sammansättning med en instruktion som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d9427-155">Reference the assembly with a statement similar to the following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="d9427-156">Annars är det enkelt att av misstag distribuerar flera testversioner av samma binära som uppför sig annorlunda mellan funktioner.</span><span class="sxs-lookup"><span data-stu-id="d9427-156">Otherwise, it is easy to accidentally deploy multiple test versions of the same binary that behave differently between functions.</span></span>

<span data-ttu-id="d9427-157">Använd inte utförlig loggning i kod.</span><span class="sxs-lookup"><span data-stu-id="d9427-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="d9427-158">Den har en negativ inverkan på prestanda.</span><span class="sxs-lookup"><span data-stu-id="d9427-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="d9427-159">Använda async-kod men Undvik blockerar anrop</span><span class="sxs-lookup"><span data-stu-id="d9427-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="d9427-160">Asynkron programmering är en rekommenderad metod.</span><span class="sxs-lookup"><span data-stu-id="d9427-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="d9427-161">Dock alltid undvika refererar till den `Result` egenskap eller anropa `Wait` metod i en `Task` instans.</span><span class="sxs-lookup"><span data-stu-id="d9427-161">However, always avoid referencing the `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="d9427-162">Den här metoden kan leda till uttömning tråd.</span><span class="sxs-lookup"><span data-stu-id="d9427-162">This approach can lead to thread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="d9427-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9427-163">Next steps</span></span>
<span data-ttu-id="d9427-164">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="d9427-164">For more information, see the following resources:</span></span>

<span data-ttu-id="d9427-165">Eftersom Azure Functions använder Azure App Service, bör du också vara medveten om Apptjänst riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="d9427-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="d9427-166">Patterns and Practices HTTP prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="d9427-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

