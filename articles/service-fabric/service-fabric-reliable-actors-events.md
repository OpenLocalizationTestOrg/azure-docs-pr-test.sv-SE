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
# <a name="actor-events"></a>Aktören händelser
Aktören händelser ger ett sätt toosend bästa meddelanden från hello aktören toohello klienter. Aktören händelser är utformade för aktören till klientkommunikation och ska inte användas för aktören – aktören kommunikation.

hello följande kod kodavsnitt visar hur toouse aktören händelser i ditt program.

Definiera ett gränssnitt som beskriver hello händelser som publicerats av hello aktören. Det här gränssnittet måste härledas från hello `IActorEvents` gränssnitt. hello argument av hello-metoder måste vara [data minimera serialiserbara](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). hello-metoder måste returnera void, som händelse-meddelanden är ett sätt och bästa prestanda.

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
Deklarera hello händelser som publicerats av hello aktören i hello aktören gränssnitt.

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
Implementera hello händelsehanterare på klientsidan hello.

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

Skapa en proxy toohello aktören som publicerar hello händelse och prenumerera tooits händelser på hello-klienten.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

I hello händelse av växling vid fel misslyckas hello aktören över tooa annan process eller nod. hello aktören proxy hanterar hello aktiva prenumerationer och igen prenumererar automatiskt dem. Du kan styra hello ny prenumeration intervall via hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. toounsubscribe, Använd hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Aktören hello, bara publicera på hello händelser när de inträffar. Om det finns prenumeranter toohello händelse, skickar hello aktörer runtime dem hello-meddelande.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Nästa steg
* [Aktören återinträde](service-fabric-reliable-actors-reentrancy.md)
* [Aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md)
* [Aktören API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# exempelkod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core exempelkod](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java-kodexempel](http://github.com/Azure-Samples/service-fabric-java-getting-started)
