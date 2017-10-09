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
# <a name="reliable-actors-reentrancy"></a>Tillförlitliga aktörer återinträde
hello Reliable Actors körning, tillåter som standard logiska anropet kontext-baserade återinträde. Detta möjliggör aktörer toobe reentrant om de finns i hello samma anropa kontexten kedjan. Till exempel skickar aktören A ett meddelande tooActor B, som skickar ett meddelande tooActor C. Som en del av hello meddelandebehandling, om aktören C anropar aktören A, är hello-meddelande fleraktivt, så ska tillåtas. Andra meddelanden som ingår i ett annat sammanhang blockeras på aktören A tills bearbetningen har slutförts.

Det finns två alternativ för aktören återinträde som definierats i hello `ActorReentrancyMode` enum:

* `LogicalCallContext`(standardinställningen)
* `Disallowed`-inaktiverar återinträde

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
Återinträde kan konfigureras i en `ActorService`'s inställningar under registreringen. hello inställningen gäller tooall aktören instanser som skapats i hello aktören service.

hello följande exempel visas en aktören-tjänst som anger hello återinträde läge för`ActorReentrancyMode.Disallowed`. I det här fallet, om en aktör skickar ett fleraktivt meddelandet tooanother aktören, ett undantag av typen `FabricException` genereras.

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


## <a name="next-steps"></a>Nästa steg
* Mer information om återinträde i hello [aktören API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
