---
title: aaaStateful Reliable Services diagnostik | Microsoft Docs
description: "Diagnostisk funktionalitet för tillståndskänsliga Reliable Services"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 6200800b858957c06039d9af062633b12a446318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostisk funktionalitet för tillståndskänsliga Reliable Services
hello Stateful Reliable Services StatefulServiceBase klassen avger [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) händelser som kan använda toodebug Hej tjänsten, som ger insikter om hur hello runtime drift och underlätta felsökningen.

## <a name="eventsource-events"></a>EventSource händelser
hello EventSource namnet hello Stateful Reliable Services StatefulServiceBase klass är ”Microsoft-ServiceFabric-Services”. Händelser från den här händelsekällan visas i den [diagnostikhändelser](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) fönstret när hello tjänsten används [felsökas i Visual Studio](service-fabric-debugging-your-application.md).

Exempel på Verktyg och tekniker som hjälper till att samla in och/eller visa EventSource händelser är [förhandsgranskning](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure-diagnostik](../cloud-services/cloud-services-dotnet-diagnostics.md), och [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Händelser
| händelsenamnet | Händelse-ID | Nivå | Händelsebeskrivning |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Information |Orsakat när tjänsten RunAsync aktiviteten har startats |
| StatefulRunAsyncCancellation |2 |Information |Orsakat när tjänsten RunAsync aktiviteten avbryts |
| StatefulRunAsyncCompletion |3 |Information |Orsakat när tjänsten RunAsync slutförs |
| StatefulRunAsyncSlowCancellation |4 |Varning |Orsakat när tjänsten RunAsync aktiviteten tar för lång toocomplete annullering |
| StatefulRunAsyncFailure |5 |Fel |Orsakat när tjänsten RunAsync aktivitet genererar ett undantag |

## <a name="interpret-events"></a>Tolka händelser
StatefulRunAsyncInvocation och StatefulRunAsyncCompletion StatefulRunAsyncCancellation händelser är användbara toohello service writer toounderstand hello livscykeln för en tjänst--samt hello tiden för när en tjänst är igång, avbröts eller slutförts . Detta kan vara användbar vid felsökning av problem med tjänsten eller förstå hello service lifecycle.

Tjänsten skrivare ska betala uppmärksam tooStatefulRunAsyncSlowCancellation och StatefulRunAsyncFailure händelser eftersom de visar på problem med hello-tjänsten.

StatefulRunAsyncFailure genereras när hello service RunAsync() aktiviteten utlöser ett undantag. Normalt anger ett undantag uppstod ett fel eller ett programfel i hello-tjänsten. Dessutom gör hello undantag hello service toofail så att den är flyttade tooa annan nod. Detta kan vara en kostsam åtgärd och kan fördröja inkommande begäranden när hello service flyttas. Tjänsten skrivare ska kontrollera hello orsaken till hello undantag och minimera om möjligt den.

StatefulRunAsyncSlowCancellation genereras när en begäran om att avbryta för hello RunAsync uppgift tar längre tid än fyra sekunder. När en tjänst tar för lång toocomplete annullering, påverkar hello möjligheten för hello service toobe snabbt startas om på en annan nod. Detta kan påverka hello övergripande tillgänglighet av hello-tjänsten.

## <a name="next-steps"></a>Nästa steg
* [EventSource providrar på förhandsgranskning](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
