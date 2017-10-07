---
title: aaaPolymorphism i hello Reliable Actors framework | Microsoft Docs
description: "Skapa hierarkier av .NET-gränssnitt och typer i hello Reliable Actors framework tooreuse funktioner och API-definitioner."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ed9ec442595bd9a5e48c9af1f6aac003439b81f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="polymorphism-in-hello-reliable-actors-framework"></a>Polymorfism i hello Reliable Actors framework
hello Reliable Actors framework kan du toobuild aktörer med många av hello samma teknik som du vill använda i objektorienterad design. En av dessa tekniker är polymorfism som tillåter typer och gränssnitt tooinherit från flera generaliserad överordnade. Arv i hello Reliable Actors framework följer vanligtvis hello .NET modellen med några ytterligare begränsningar. Vid Java-/ Linux följer hello Java-modellen.

## <a name="interfaces"></a>Gränssnitt
hello Reliable Actors framework kräver toodefine minst ett gränssnitt toobe implementeras av aktören-typen. Det här gränssnittet är används toogenerate en proxyklass som kan användas av klienter toocommunicate med din aktörer. Gränssnitt kan ärvas från andra gränssnitt, förutsatt att varje gränssnitt som implementeras av en aktörstyp av och alla dess överordnade slutligen härledas från IActor(C#) eller Actor(Java). IActor(C#) och Actor(Java) är hello plattform definierats basgränssnitt för aktörer i hello ramverk .NET och Java. Därför hello klassiska polymorfism exempel med hjälp av former kan se ut ungefär så här:

![Gränssnittet hierarki för formen aktörer][shapes-interface-hierarchy]

## <a name="types"></a>Typer
Du kan också skapa en hierarki av aktören typer som härletts från hello aktören basklass som tillhandahålls av hello-plattformen. Hello gäller former, kanske du har en bas `Shape`(C#) eller `ShapeImpl`(Java) typ:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

Subtypes av `Shape`(C#) eller `ShapeImpl`(Java) kan åsidosätta metoder från hello bas.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Obs hello `ActorService` attribut i hello aktörstyp. Det här attributet anger hello tillförlitliga aktören framework att den automatiskt skapar en tjänst som värd för aktörer av den här typen. I vissa fall kan du vilja toocreate en bastyp som endast är avsedd för att dela funktioner med undertyper och aldrig kommer att använda tooinstantiate konkreta aktörer. I sådana fall bör du använda hello `abstract` nyckelordet tooindicate som du vill skapa en aktör baserat på den aktuella typen aldrig.

## <a name="next-steps"></a>Nästa steg
* Se [hur hello Reliable Actors framework utnyttjar hello Service Fabric-plattformen](service-fabric-reliable-actors-platform.md) tooprovide tillförlitlighet, skalbarhet och konsekvent tillstånd.
* Lär dig mer om hello [aktören livscykel](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
