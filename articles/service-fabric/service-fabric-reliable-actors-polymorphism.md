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
# <a name="polymorphism-in-hello-reliable-actors-framework"></a><span data-ttu-id="52fca-103">Polymorfism i hello Reliable Actors framework</span><span class="sxs-lookup"><span data-stu-id="52fca-103">Polymorphism in hello Reliable Actors framework</span></span>
<span data-ttu-id="52fca-104">hello Reliable Actors framework kan du toobuild aktörer med många av hello samma teknik som du vill använda i objektorienterad design.</span><span class="sxs-lookup"><span data-stu-id="52fca-104">hello Reliable Actors framework allows you toobuild actors using many of hello same techniques that you would use in object-oriented design.</span></span> <span data-ttu-id="52fca-105">En av dessa tekniker är polymorfism som tillåter typer och gränssnitt tooinherit från flera generaliserad överordnade.</span><span class="sxs-lookup"><span data-stu-id="52fca-105">One of those techniques is polymorphism, which allows types and interfaces tooinherit from more generalized parents.</span></span> <span data-ttu-id="52fca-106">Arv i hello Reliable Actors framework följer vanligtvis hello .NET modellen med några ytterligare begränsningar.</span><span class="sxs-lookup"><span data-stu-id="52fca-106">Inheritance in hello Reliable Actors framework generally follows hello .NET model with a few additional constraints.</span></span> <span data-ttu-id="52fca-107">Vid Java-/ Linux följer hello Java-modellen.</span><span class="sxs-lookup"><span data-stu-id="52fca-107">In case of Java/Linux, it follows hello Java model.</span></span>

## <a name="interfaces"></a><span data-ttu-id="52fca-108">Gränssnitt</span><span class="sxs-lookup"><span data-stu-id="52fca-108">Interfaces</span></span>
<span data-ttu-id="52fca-109">hello Reliable Actors framework kräver toodefine minst ett gränssnitt toobe implementeras av aktören-typen.</span><span class="sxs-lookup"><span data-stu-id="52fca-109">hello Reliable Actors framework requires you toodefine at least one interface toobe implemented by your actor type.</span></span> <span data-ttu-id="52fca-110">Det här gränssnittet är används toogenerate en proxyklass som kan användas av klienter toocommunicate med din aktörer.</span><span class="sxs-lookup"><span data-stu-id="52fca-110">This interface is used toogenerate a proxy class that can be used by clients toocommunicate with your actors.</span></span> <span data-ttu-id="52fca-111">Gränssnitt kan ärvas från andra gränssnitt, förutsatt att varje gränssnitt som implementeras av en aktörstyp av och alla dess överordnade slutligen härledas från IActor(C#) eller Actor(Java).</span><span class="sxs-lookup"><span data-stu-id="52fca-111">Interfaces can inherit from other interfaces as long as every interface that is implemented by an actor type and all of its parents ultimately derive from IActor(C#) or Actor(Java) .</span></span> <span data-ttu-id="52fca-112">IActor(C#) och Actor(Java) är hello plattform definierats basgränssnitt för aktörer i hello ramverk .NET och Java.</span><span class="sxs-lookup"><span data-stu-id="52fca-112">IActor(C#) and Actor(Java) are hello platform-defined base interfaces for actors in hello frameworks .NET and Java respectively.</span></span> <span data-ttu-id="52fca-113">Därför hello klassiska polymorfism exempel med hjälp av former kan se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="52fca-113">Thus, hello classic polymorphism example using shapes might look something like this:</span></span>

![Gränssnittet hierarki för formen aktörer][shapes-interface-hierarchy]

## <a name="types"></a><span data-ttu-id="52fca-115">Typer</span><span class="sxs-lookup"><span data-stu-id="52fca-115">Types</span></span>
<span data-ttu-id="52fca-116">Du kan också skapa en hierarki av aktören typer som härletts från hello aktören basklass som tillhandahålls av hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="52fca-116">You can also create a hierarchy of actor types, which are derived from hello base Actor class that is provided by hello platform.</span></span> <span data-ttu-id="52fca-117">Hello gäller former, kanske du har en bas `Shape`(C#) eller `ShapeImpl`(Java) typ:</span><span class="sxs-lookup"><span data-stu-id="52fca-117">In hello case of shapes, you might have a base `Shape`(C#) or `ShapeImpl`(Java) type:</span></span>

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

<span data-ttu-id="52fca-118">Subtypes av `Shape`(C#) eller `ShapeImpl`(Java) kan åsidosätta metoder från hello bas.</span><span class="sxs-lookup"><span data-stu-id="52fca-118">Subtypes of `Shape`(C#) or `ShapeImpl`(Java) can override methods from hello base.</span></span>

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

<span data-ttu-id="52fca-119">Obs hello `ActorService` attribut i hello aktörstyp.</span><span class="sxs-lookup"><span data-stu-id="52fca-119">Note hello `ActorService` attribute on hello actor type.</span></span> <span data-ttu-id="52fca-120">Det här attributet anger hello tillförlitliga aktören framework att den automatiskt skapar en tjänst som värd för aktörer av den här typen.</span><span class="sxs-lookup"><span data-stu-id="52fca-120">This attribute tells hello Reliable Actor framework that it should automatically create a service for hosting actors of this type.</span></span> <span data-ttu-id="52fca-121">I vissa fall kan du vilja toocreate en bastyp som endast är avsedd för att dela funktioner med undertyper och aldrig kommer att använda tooinstantiate konkreta aktörer.</span><span class="sxs-lookup"><span data-stu-id="52fca-121">In some cases, you may wish toocreate a base type that is solely intended for sharing functionality with subtypes and will never be used tooinstantiate concrete actors.</span></span> <span data-ttu-id="52fca-122">I sådana fall bör du använda hello `abstract` nyckelordet tooindicate som du vill skapa en aktör baserat på den aktuella typen aldrig.</span><span class="sxs-lookup"><span data-stu-id="52fca-122">In those cases, you should use hello `abstract` keyword tooindicate that you will never create an actor based on that type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52fca-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52fca-123">Next steps</span></span>
* <span data-ttu-id="52fca-124">Se [hur hello Reliable Actors framework utnyttjar hello Service Fabric-plattformen](service-fabric-reliable-actors-platform.md) tooprovide tillförlitlighet, skalbarhet och konsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="52fca-124">See [how hello Reliable Actors framework leverages hello Service Fabric platform](service-fabric-reliable-actors-platform.md) tooprovide reliability, scalability, and consistent state.</span></span>
* <span data-ttu-id="52fca-125">Lär dig mer om hello [aktören livscykel](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="52fca-125">Learn about hello [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).</span></span>

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
