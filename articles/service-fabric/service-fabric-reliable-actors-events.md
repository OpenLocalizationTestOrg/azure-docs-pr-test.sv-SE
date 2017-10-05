---
title: "Händelser i aktören-baserad Azure mikrotjänster | Microsoft Docs"
description: "Introduktion till händelser för Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: d936670c548ff709fc2e935d3f28d94e4bde8a04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="actor-events"></a><span data-ttu-id="9fe73-103">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="9fe73-103">Actor events</span></span>
<span data-ttu-id="9fe73-104">Aktören händelser är ett sätt att skicka meddelanden för bästa prestanda från aktören till klienterna.</span><span class="sxs-lookup"><span data-stu-id="9fe73-104">Actor events provide a way to send best-effort notifications from the actor to the clients.</span></span> <span data-ttu-id="9fe73-105">Aktören händelser är utformade för aktören till klientkommunikation och ska inte användas för aktören – aktören kommunikation.</span><span class="sxs-lookup"><span data-stu-id="9fe73-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="9fe73-106">Följande kodavsnitt visar hur du använder aktören händelser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9fe73-106">The following code snippets show how to use actor events in your application.</span></span>

<span data-ttu-id="9fe73-107">Definiera ett gränssnitt som beskrivs de händelser som publicerats av den.</span><span class="sxs-lookup"><span data-stu-id="9fe73-107">Define an interface that describes the events published by the actor.</span></span> <span data-ttu-id="9fe73-108">Det här gränssnittet måste härledas från den `IActorEvents` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9fe73-108">This interface must be derived from the `IActorEvents` interface.</span></span> <span data-ttu-id="9fe73-109">Argument av metoder måste vara [data minimera serialiserbara](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="9fe73-109">The arguments of the methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="9fe73-110">Metoderna måste returnera void, som händelse-meddelanden är ett sätt och bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="9fe73-110">The methods must return void, as event notifications are one way and best effort.</span></span>

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
<span data-ttu-id="9fe73-111">Deklarera händelser som publicerats av aktören i gränssnittet aktören.</span><span class="sxs-lookup"><span data-stu-id="9fe73-111">Declare the events published by the actor in the actor interface.</span></span>

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
<span data-ttu-id="9fe73-112">Implementera händelsehanteraren på klienten.</span><span class="sxs-lookup"><span data-stu-id="9fe73-112">On the client side, implement the event handler.</span></span>

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

<span data-ttu-id="9fe73-113">Skapa en proxy för aktören som publicerar händelsen och prenumerera på händelser på klienten.</span><span class="sxs-lookup"><span data-stu-id="9fe73-113">On the client, create a proxy to the actor that publishes the event and subscribe to its events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="9fe73-114">Vid växling vid fel, kan aktören växlas över till en annan process eller en nod.</span><span class="sxs-lookup"><span data-stu-id="9fe73-114">In the event of failovers, the actor may fail over to a different process or node.</span></span> <span data-ttu-id="9fe73-115">Aktören proxy hanterar de aktiva prenumerationerna och igen prenumererar automatiskt dem.</span><span class="sxs-lookup"><span data-stu-id="9fe73-115">The actor proxy manages the active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="9fe73-116">Du kan styra intervallet för ny prenumerationen via den `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="9fe73-116">You can control the re-subscription interval through the `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="9fe73-117">Om du vill avbryta prenumerationen, använder den `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="9fe73-117">To unsubscribe, use the `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="9fe73-118">Publicera händelser på aktören, när de görs.</span><span class="sxs-lookup"><span data-stu-id="9fe73-118">On the actor, simply publish the events as they happen.</span></span> <span data-ttu-id="9fe73-119">Om det finns prenumeranter att händelsen, skickar aktörer runtime dem meddelandet.</span><span class="sxs-lookup"><span data-stu-id="9fe73-119">If there are subscribers to the event, the Actors runtime will send them the notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="9fe73-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fe73-120">Next steps</span></span>
* [<span data-ttu-id="9fe73-121">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="9fe73-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="9fe73-122">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="9fe73-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="9fe73-123">Aktören API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="9fe73-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="9fe73-124">C# exempelkod</span><span class="sxs-lookup"><span data-stu-id="9fe73-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="9fe73-125">C# .NET Core exempelkod</span><span class="sxs-lookup"><span data-stu-id="9fe73-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="9fe73-126">Java-kodexempel</span><span class="sxs-lookup"><span data-stu-id="9fe73-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
