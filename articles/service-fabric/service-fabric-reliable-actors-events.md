---
title: "aaaEvents i aktören-baserad Azure mikrotjänster | Microsoft Docs"
description: "Introduktion tooevents för Service Fabric Reliable Actors."
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
ms.openlocfilehash: a51e41c35441a5fea508138968b36a35f0ba6699
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="actor-events"></a><span data-ttu-id="ed076-103">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="ed076-103">Actor events</span></span>
<span data-ttu-id="ed076-104">Aktören händelser ger ett sätt toosend bästa meddelanden från hello aktören toohello klienter.</span><span class="sxs-lookup"><span data-stu-id="ed076-104">Actor events provide a way toosend best-effort notifications from hello actor toohello clients.</span></span> <span data-ttu-id="ed076-105">Aktören händelser är utformade för aktören till klientkommunikation och ska inte användas för aktören – aktören kommunikation.</span><span class="sxs-lookup"><span data-stu-id="ed076-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="ed076-106">hello följande kod kodavsnitt visar hur toouse aktören händelser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="ed076-106">hello following code snippets show how toouse actor events in your application.</span></span>

<span data-ttu-id="ed076-107">Definiera ett gränssnitt som beskriver hello händelser som publicerats av hello aktören.</span><span class="sxs-lookup"><span data-stu-id="ed076-107">Define an interface that describes hello events published by hello actor.</span></span> <span data-ttu-id="ed076-108">Det här gränssnittet måste härledas från hello `IActorEvents` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ed076-108">This interface must be derived from hello `IActorEvents` interface.</span></span> <span data-ttu-id="ed076-109">hello argument av hello-metoder måste vara [data minimera serialiserbara](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="ed076-109">hello arguments of hello methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="ed076-110">hello-metoder måste returnera void, som händelse-meddelanden är ett sätt och bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="ed076-110">hello methods must return void, as event notifications are one way and best effort.</span></span>

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
<span data-ttu-id="ed076-111">Deklarera hello händelser som publicerats av hello aktören i hello aktören gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ed076-111">Declare hello events published by hello actor in hello actor interface.</span></span>

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
<span data-ttu-id="ed076-112">Implementera hello händelsehanterare på klientsidan hello.</span><span class="sxs-lookup"><span data-stu-id="ed076-112">On hello client side, implement hello event handler.</span></span>

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

<span data-ttu-id="ed076-113">Skapa en proxy toohello aktören som publicerar hello händelse och prenumerera tooits händelser på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="ed076-113">On hello client, create a proxy toohello actor that publishes hello event and subscribe tooits events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="ed076-114">I hello händelse av växling vid fel misslyckas hello aktören över tooa annan process eller nod.</span><span class="sxs-lookup"><span data-stu-id="ed076-114">In hello event of failovers, hello actor may fail over tooa different process or node.</span></span> <span data-ttu-id="ed076-115">hello aktören proxy hanterar hello aktiva prenumerationer och igen prenumererar automatiskt dem.</span><span class="sxs-lookup"><span data-stu-id="ed076-115">hello actor proxy manages hello active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="ed076-116">Du kan styra hello ny prenumeration intervall via hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="ed076-116">You can control hello re-subscription interval through hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="ed076-117">toounsubscribe, Använd hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="ed076-117">toounsubscribe, use hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="ed076-118">Aktören hello, bara publicera på hello händelser när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="ed076-118">On hello actor, simply publish hello events as they happen.</span></span> <span data-ttu-id="ed076-119">Om det finns prenumeranter toohello händelse, skickar hello aktörer runtime dem hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ed076-119">If there are subscribers toohello event, hello Actors runtime will send them hello notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="ed076-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed076-120">Next steps</span></span>
* [<span data-ttu-id="ed076-121">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="ed076-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="ed076-122">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="ed076-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="ed076-123">Aktören API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="ed076-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="ed076-124">C# exempelkod</span><span class="sxs-lookup"><span data-stu-id="ed076-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="ed076-125">C# .NET Core exempelkod</span><span class="sxs-lookup"><span data-stu-id="ed076-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="ed076-126">Java-kodexempel</span><span class="sxs-lookup"><span data-stu-id="ed076-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
