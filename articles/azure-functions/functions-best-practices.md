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
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a>Optimera hello prestanda och tillförlitlighet i Azure Functions

Den här artikeln innehåller vägledning tooimprove hello prestanda och tillförlitlighet för funktionen appar. 


## <a name="avoid-long-running-functions"></a>Undvika långvariga funktioner

Stora, långvariga funktioner kan orsaka oväntade timeout problem. En funktion kan bli stora på grund av toomany Node.js-beroenden. Importera beroenden kan också medföra ökad belastning gånger som resultera i oväntade timeout. Beroenden laddas både uttryckligen och implicit. En enda modul som lästs in av din kod kan läsa in en egen ytterligare moduler.  

När det är möjligt, flytta stora funktionerna i mindre funktionen anger som fungerar tillsammans och returnera svar snabbt. Till exempel kan en webhook eller HTTP-Utlösarfunktion kräva ett bekräftelse-svar inom en viss tid; Det är vanligt att webhooks toorequire en omedelbara åtgärder. Du kan överföra hello http-utlösaren nyttolast i en kö toobe som bearbetas av en kö Utlösarfunktion. Den här metoden kan du toodefer hello verkligt arbete och returnera ett omedelbart svar.


## <a name="cross-function-communication"></a>Funktionen kommunikation mellan

När du integrerar flera funktioner, men det är oftast en bästa praxis toouse lagringsköer för mellan funktionen kommunikation.  hello främsta skälet är lagringsköer är billigare och mycket enklare tooprovision. 

Enskilda meddelanden i en kö för lagring är begränsade i storlek too64 KB. Om toopass större meddelanden mellan funktioner måste vara en Azure Service Bus-kö används toosupport meddelandestorleken in too256 KB.

Service Bus-ämnen är användbara om du behöver meddelande filtrering innan bearbetning.

Händelsehubbar är användbara toosupport hög volym kommunikation.


## <a name="write-functions-toobe-stateless"></a>Skriva funktioner toobe tillståndslös 

Funktioner som ska vara tillståndslösa och idempotent om möjligt. Koppla statusinformation som krävs till dina data. Till exempel en order bearbetas skulle förmodligen ha en associerad `state` medlem. En funktion kan bearbeta en ordning som baseras på det aktuella tillståndet eftersom själva hello funktionen förblir tillståndslösa. 

Idempotent funktioner är särskilt användbart med timer-utlösare. Till exempel om du har något som verkligen måste köras en gång om dagen, skriva den så den kan köras under hello dag med hello samma resultat. hello-funktionen kan avsluta när det finns inget arbete för en viss dag. Även om en tidigare körning misslyckades toocomplete ska hello kör nästa gång hämta där den avbröts.


## <a name="write-defensive-functions"></a>Skriva defensiva funktioner

Anta att din funktion kan uppstå ett undantag när som helst. Utforma dina funktioner med hello möjlighet toocontinue från en tidigare punkt misslyckas under hello nästa körning. Överväg ett scenario som kräver hello följande åtgärder:

1. Frågan för 10 000 rader i en databas.
2. Skapa ett kömeddelande för var och en av dessa rader tooprocess ytterligare hello rad nedåt.
 
Beroende på hur komplexa datorn är kanske: ingår nedströms tjänster fungerar felaktigt, nätverk avbrott eller kvot begränsar nått, etc. Alla dessa kan påverka funktionen när som helst. Du måste toodesign dina funktioner toobe förberedd för den.

Hur koden reagera om ett fel uppstår när du har infogat 5 000 av dessa objekt i en kö för bearbetning? Spåra objekt i en samling som du har slutfört. Annars kan kan du lägga till dem igen nästa gång. Detta kan ha en betydande del av ditt arbetsflöde. 

Om ett köobjekt redan har bearbetats, Tillåt funktionen-toobe en no-op.

Dra nytta av försvarsåtgärder som redan ges för komponenter som du använder i hello Azure Functions-plattformen. Se exempelvis **hantering av skadligt Kömeddelanden** i hello-dokumentationen för [Azure Storage-kö utlöser](functions-bindings-storage-queue.md#trigger).
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a>Inte blandning test- och kod i hello samma funktionsapp

Funktioner i en funktionsapp dela resurser. Till exempel delas minne. Om du använder en funktionsapp i produktion, Lägg inte till test-relaterade funktioner och resurser tooit. Det kan orsaka oväntade kostnader under kodkörning för produktion.

Var noga med att du laddar i dina appar för produktion-funktionen. Minne beräknas för varje funktion i hello app.

Om du har en delad sammansättning som refereras i flera .net-funktioner kan du placera den i en gemensam delad mapp. Referens hello sammansättning med en instruktion liknande toohello följande exempel: 

    #r "..\Shared\MyAssembly.dll". 

I annat fall är det enkelt tooaccidentally distribuera flera testversioner av hello samma binary som fungerar annorlunda mellan fungerar.

Använd inte utförlig loggning i kod. Den har en negativ inverkan på prestanda.



## <a name="use-async-code-but-avoid-blocking-calls"></a>Använda async-kod men Undvik blockerar anrop

Asynkron programmering är en rekommenderad metod. Dock alltid undvika refererar till hello `Result` egenskap eller anropa `Wait` metod i en `Task` instans. Den här metoden kan leda toothread resursuttömning.


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

Eftersom Azure Functions använder Azure App Service, bör du också vara medveten om Apptjänst riktlinjer.
* [Patterns and Practices HTTP prestandaoptimering](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

