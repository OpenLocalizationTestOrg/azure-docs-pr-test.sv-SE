---
title: "aaaReentrancy i aktören-baserad Azure mikrotjänster | Microsoft Docs"
description: "Introduktion tooreentrancy för Service Fabric Reliable Actors"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 61c69bcf0f100e075d19ba155954c05789b71761
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-reentrancy"></a><span data-ttu-id="6257d-103">Tillförlitliga aktörer återinträde</span><span class="sxs-lookup"><span data-stu-id="6257d-103">Reliable Actors reentrancy</span></span>
<span data-ttu-id="6257d-104">hello Reliable Actors körning, tillåter som standard logiska anropet kontext-baserade återinträde.</span><span class="sxs-lookup"><span data-stu-id="6257d-104">hello Reliable Actors runtime, by default, allows logical call context-based reentrancy.</span></span> <span data-ttu-id="6257d-105">Detta möjliggör aktörer toobe reentrant om de finns i hello samma anropa kontexten kedjan.</span><span class="sxs-lookup"><span data-stu-id="6257d-105">This allows for actors toobe reentrant if they are in hello same call context chain.</span></span> <span data-ttu-id="6257d-106">Till exempel skickar aktören A ett meddelande tooActor B, som skickar ett meddelande tooActor C. Som en del av hello meddelandebehandling, om aktören C anropar aktören A, är hello-meddelande fleraktivt, så ska tillåtas.</span><span class="sxs-lookup"><span data-stu-id="6257d-106">For example, Actor A sends a message tooActor B, who sends a message tooActor C. As part of hello message processing, if Actor C calls Actor A, hello message is reentrant, so it will be allowed.</span></span> <span data-ttu-id="6257d-107">Andra meddelanden som ingår i ett annat sammanhang blockeras på aktören A tills bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6257d-107">Any other messages that are part of a different call context will be blocked on Actor A until it finishes processing.</span></span>

<span data-ttu-id="6257d-108">Det finns två alternativ för aktören återinträde som definierats i hello `ActorReentrancyMode` enum:</span><span class="sxs-lookup"><span data-stu-id="6257d-108">There are two options available for actor reentrancy defined in hello `ActorReentrancyMode` enum:</span></span>

* <span data-ttu-id="6257d-109">`LogicalCallContext`(standardinställningen)</span><span class="sxs-lookup"><span data-stu-id="6257d-109">`LogicalCallContext` (default behavior)</span></span>
* <span data-ttu-id="6257d-110">`Disallowed`-inaktiverar återinträde</span><span class="sxs-lookup"><span data-stu-id="6257d-110">`Disallowed` - disables reentrancy</span></span>

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
<span data-ttu-id="6257d-111">Återinträde kan konfigureras i en `ActorService`'s inställningar under registreringen.</span><span class="sxs-lookup"><span data-stu-id="6257d-111">Reentrancy can be configured in an `ActorService`'s settings during registration.</span></span> <span data-ttu-id="6257d-112">hello inställningen gäller tooall aktören instanser som skapats i hello aktören service.</span><span class="sxs-lookup"><span data-stu-id="6257d-112">hello setting applies tooall actor instances created in hello actor service.</span></span>

<span data-ttu-id="6257d-113">hello följande exempel visas en aktören-tjänst som anger hello återinträde läge för`ActorReentrancyMode.Disallowed`.</span><span class="sxs-lookup"><span data-stu-id="6257d-113">hello following example shows an actor service that sets hello reentrancy mode too`ActorReentrancyMode.Disallowed`.</span></span> <span data-ttu-id="6257d-114">I det här fallet, om en aktör skickar ett fleraktivt meddelandet tooanother aktören, ett undantag av typen `FabricException` genereras.</span><span class="sxs-lookup"><span data-stu-id="6257d-114">In this case, if an actor sends a reentrant message tooanother actor, an exception of type `FabricException` will be thrown.</span></span>

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a><span data-ttu-id="6257d-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6257d-115">Next steps</span></span>
* <span data-ttu-id="6257d-116">Mer information om återinträde i hello [aktören API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span><span class="sxs-lookup"><span data-stu-id="6257d-116">Learn more about reentrancy in hello [Actor API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span></span>
