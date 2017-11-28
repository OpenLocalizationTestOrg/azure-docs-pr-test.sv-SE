---
title: "aaaReliable aktörer timers och påminnelser | Microsoft Docs"
description: "Introduktion tootimers och påminnelser för Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c5116ec1923014e131130b9f4e86dd1e133bbf7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="actor-timers-and-reminders"></a><span data-ttu-id="1f51e-103">Aktören timers och påminnelser</span><span class="sxs-lookup"><span data-stu-id="1f51e-103">Actor timers and reminders</span></span>
<span data-ttu-id="1f51e-104">Aktörer kan schemalägga regelbunden arbete på själva genom att registrera timers eller påminnelser.</span><span class="sxs-lookup"><span data-stu-id="1f51e-104">Actors can schedule periodic work on themselves by registering either timers or reminders.</span></span> <span data-ttu-id="1f51e-105">Den här artikeln visar hur toouse timers och påminnelser och förklarar hello skillnaderna mellan dem.</span><span class="sxs-lookup"><span data-stu-id="1f51e-105">This article shows how toouse timers and reminders and explains hello differences between them.</span></span>

## <a name="actor-timers"></a><span data-ttu-id="1f51e-106">Aktören timers</span><span class="sxs-lookup"><span data-stu-id="1f51e-106">Actor timers</span></span>
<span data-ttu-id="1f51e-107">Aktören timers ger en enkel omslutning runt en .NET eller Java timer tooensure att hello återanrop metoder respekterar hello bygger samtidighet garanterar att hello aktörer runtime tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="1f51e-107">Actor timers provide a simple wrapper around a .NET or Java timer tooensure that hello callback methods respect hello turn-based concurrency guarantees that hello Actors runtime provides.</span></span>

<span data-ttu-id="1f51e-108">Aktörer kan använda hello `RegisterTimer`(C#) eller `registerTimer`(Java) och `UnregisterTimer`(C#) eller `unregisterTimer`(Java)-metoder på deras base klassen tooregister och avregistrera sina timers.</span><span class="sxs-lookup"><span data-stu-id="1f51e-108">Actors can use hello `RegisterTimer`(C#) or `registerTimer`(Java)  and `UnregisterTimer`(C#) or `unregisterTimer`(Java) methods on their base class tooregister and unregister their timers.</span></span> <span data-ttu-id="1f51e-109">hello exemplet nedan visar hello använder timer-API: er.</span><span class="sxs-lookup"><span data-stu-id="1f51e-109">hello example below shows hello use of timer APIs.</span></span> <span data-ttu-id="1f51e-110">hello API: er är mycket lik toohello .NET timer eller Java-timern.</span><span class="sxs-lookup"><span data-stu-id="1f51e-110">hello APIs are very similar toohello .NET timer or Java timer.</span></span> <span data-ttu-id="1f51e-111">I det här exemplet när hello timer förfaller hello aktörer runtime ringer hello `MoveObject`(C#) eller `moveObject`(Java)-metoden.</span><span class="sxs-lookup"><span data-stu-id="1f51e-111">In this example, when hello timer is due, hello Actors runtime will call hello `MoveObject`(C#) or `moveObject`(Java) method.</span></span> <span data-ttu-id="1f51e-112">hello-metoden är garanterat toorespect hello bygger samtidighet.</span><span class="sxs-lookup"><span data-stu-id="1f51e-112">hello method is guaranteed toorespect hello turn-based concurrency.</span></span> <span data-ttu-id="1f51e-113">Detta innebär att inga andra aktören metoder eller timer/påminnelse återanrop pågår förrän den här återanrop körning.</span><span class="sxs-lookup"><span data-stu-id="1f51e-113">This means that no other actor methods or timer/reminder callbacks will be in progress until this callback completes execution.</span></span>

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter toopass toohello callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time toodelay before hello callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of hello callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter toopass toohello callback method
                            Duration.ofMillis(10),                            // Amount of time toodelay before hello callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of hello callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

<span data-ttu-id="1f51e-114">hello nästa period i hello timer startar när hello motringning är slutfört.</span><span class="sxs-lookup"><span data-stu-id="1f51e-114">hello next period of hello timer starts after hello callback completes execution.</span></span> <span data-ttu-id="1f51e-115">Detta innebär att timern hello stoppas medan hello återanrop körs och är igång när hello motringning är klar.</span><span class="sxs-lookup"><span data-stu-id="1f51e-115">This implies that hello timer is stopped while hello callback is executing and is started when hello callback finishes.</span></span>

<span data-ttu-id="1f51e-116">hello aktörer runtime sparar ändringar som gjorts toohello aktören Tillståndshanterare när hello motringning är klar.</span><span class="sxs-lookup"><span data-stu-id="1f51e-116">hello Actors runtime saves changes made toohello actor's State Manager when hello callback finishes.</span></span> <span data-ttu-id="1f51e-117">Om ett fel uppstår i sparar hello tillstånd, aktören objektet kommer att inaktiveras och en ny instans aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1f51e-117">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="1f51e-118">Alla stoppas när hello aktören inaktiveras som en del av skräpinsamling.</span><span class="sxs-lookup"><span data-stu-id="1f51e-118">All timers are stopped when hello actor is deactivated as part of garbage collection.</span></span> <span data-ttu-id="1f51e-119">Inga timer-återanrop anropas efter.</span><span class="sxs-lookup"><span data-stu-id="1f51e-119">No timer callbacks are invoked after that.</span></span> <span data-ttu-id="1f51e-120">Hello aktörer runtime behåller dessutom inte någon information om hello timers som kördes före inaktivering.</span><span class="sxs-lookup"><span data-stu-id="1f51e-120">Also, hello Actors runtime does not retain any information about hello timers that were running before deactivation.</span></span> <span data-ttu-id="1f51e-121">Är det upp toohello aktören tooregister alla timers som krävs när den aktiveras i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="1f51e-121">It is up toohello actor tooregister any timers that it needs when it is reactivated in hello future.</span></span> <span data-ttu-id="1f51e-122">Mer information finns i avsnittet hello på [aktören skräpinsamling](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="1f51e-122">For more information, see hello section on [actor garbage collection](service-fabric-reliable-actors-lifecycle.md).</span></span>

## <a name="actor-reminders"></a><span data-ttu-id="1f51e-123">Aktören påminnelser</span><span class="sxs-lookup"><span data-stu-id="1f51e-123">Actor reminders</span></span>
<span data-ttu-id="1f51e-124">Påminnelser är en mekanism tootrigger beständiga återanrop på en aktör vid angivna tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="1f51e-124">Reminders are a mechanism tootrigger persistent callbacks on an actor at specified times.</span></span> <span data-ttu-id="1f51e-125">Deras funktion är liknande tootimers.</span><span class="sxs-lookup"><span data-stu-id="1f51e-125">Their functionality is similar tootimers.</span></span> <span data-ttu-id="1f51e-126">Men till skillnad från timers, påminnelser utlöses i alla fall tills hello aktören Avregistrerar dem explicit eller hello aktören uttryckligen tas bort.</span><span class="sxs-lookup"><span data-stu-id="1f51e-126">But unlike timers, reminders are triggered under all circumstances until hello actor explicitly unregisters them or hello actor is explicitly deleted.</span></span> <span data-ttu-id="1f51e-127">Mer specifikt utlöses påminnelser över aktören avaktiveringar och växling vid fel eftersom hello aktörer runtime kvarstår information om hello aktören påminnelser.</span><span class="sxs-lookup"><span data-stu-id="1f51e-127">Specifically, reminders are triggered across actor deactivations and failovers because hello Actors runtime persists information about hello actor's reminders.</span></span>

<span data-ttu-id="1f51e-128">tooregister en påminnelse om en aktör anropar hello `RegisterReminderAsync` metoden som angetts i basklassen hello, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="1f51e-128">tooregister a reminder, an actor calls hello `RegisterReminderAsync` method provided on hello base class, as shown in hello following example:</span></span>

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //hello amount of time toodelay before firing hello reminder
            period);    //hello time interval between firing of reminders
}
```

<span data-ttu-id="1f51e-129">I det här exemplet `"Pay cell phone bill"` är hello påminnelse namn.</span><span class="sxs-lookup"><span data-stu-id="1f51e-129">In this example, `"Pay cell phone bill"` is hello reminder name.</span></span> <span data-ttu-id="1f51e-130">Detta är en sträng som hello aktören använder toouniquely identifiera en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="1f51e-130">This is a string that hello actor uses toouniquely identify a reminder.</span></span> <span data-ttu-id="1f51e-131">`BitConverter.GetBytes(amountInDollars)`(C#) är hello kontext som är associerad med hello påminnelse.</span><span class="sxs-lookup"><span data-stu-id="1f51e-131">`BitConverter.GetBytes(amountInDollars)`(C#) is hello context that is associated with hello reminder.</span></span> <span data-ttu-id="1f51e-132">Den skickas tillbaka toohello aktören som ett argument toohello påminnelse motringning, d.v.s. `IRemindable.ReceiveReminderAsync`(C#) eller `Remindable.receiveReminderAsync`(Java).</span><span class="sxs-lookup"><span data-stu-id="1f51e-132">It will be passed back toohello actor as an argument toohello reminder callback, i.e. `IRemindable.ReceiveReminderAsync`(C#) or `Remindable.receiveReminderAsync`(Java).</span></span>

<span data-ttu-id="1f51e-133">Aktörer som använder påminnelser måste implementera hello `IRemindable` gränssnitt som visas i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="1f51e-133">Actors that use reminders must implement hello `IRemindable` interface, as shown in hello example below.</span></span>

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

<span data-ttu-id="1f51e-134">När en påminnelse utlöses hello Reliable Actors runtime ska anropa hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-metoden på hello aktören.</span><span class="sxs-lookup"><span data-stu-id="1f51e-134">When a reminder is triggered, hello Reliable Actors runtime will invoke hello  `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method on hello Actor.</span></span> <span data-ttu-id="1f51e-135">En aktören kan registrera flera påminnelser och hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-metoden anropas när någon av dessa påminnelser utlöses.</span><span class="sxs-lookup"><span data-stu-id="1f51e-135">An actor can register multiple reminders, and hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method is invoked when any of those reminders is triggered.</span></span> <span data-ttu-id="1f51e-136">hello aktören kan använda hello påminnelse namn som används i toohello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java) metoden toofigure reda på vilka påminnelse utlöstes.</span><span class="sxs-lookup"><span data-stu-id="1f51e-136">hello actor can use hello reminder name that is passed in toohello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method toofigure out which reminder was triggered.</span></span>

<span data-ttu-id="1f51e-137">hello aktörer runtime sparar hello aktören tillstånd när hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1f51e-137">hello Actors runtime saves hello actor's state when hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) call finishes.</span></span> <span data-ttu-id="1f51e-138">Om ett fel uppstår i sparar hello tillstånd, aktören objektet kommer att inaktiveras och en ny instans aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1f51e-138">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="1f51e-139">toounregister en påminnelse om en aktör anropar hello `UnregisterReminderAsync`(C#) eller `unregisterReminderAsync`(Java)-metoden, som visas i hello exemplen nedan.</span><span class="sxs-lookup"><span data-stu-id="1f51e-139">toounregister a reminder, an actor calls hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method, as shown in hello examples below.</span></span>

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

<span data-ttu-id="1f51e-140">Enligt ovan hello `UnregisterReminderAsync`(C#) eller `unregisterReminderAsync`(Java) metoden godkänner ett `IActorReminder`(C#) eller `ActorReminder`(Java) gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1f51e-140">As shown above, hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method accepts an `IActorReminder`(C#) or `ActorReminder`(Java) interface.</span></span> <span data-ttu-id="1f51e-141">Hej aktören basklass stöder en `GetReminder`(C#) eller `getReminder`(Java)-metod som kan använda tooretrieve hello `IActorReminder`(C#) eller `ActorReminder`(Java)-gränssnittet genom att passera i hello påminnelse namn.</span><span class="sxs-lookup"><span data-stu-id="1f51e-141">hello actor base class supports a `GetReminder`(C#) or `getReminder`(Java) method that can be used tooretrieve hello `IActorReminder`(C#) or `ActorReminder`(Java) interface by passing in hello reminder name.</span></span> <span data-ttu-id="1f51e-142">Det är praktiskt eftersom hello aktören inte behöver toopersist hello `IActorReminder`(C#) eller `ActorReminder`(Java) gränssnitt som returnerades från hello `RegisterReminder`(C#) eller `registerReminder`(Java)-anrop.</span><span class="sxs-lookup"><span data-stu-id="1f51e-142">This is convenient because hello actor does not need toopersist hello `IActorReminder`(C#) or `ActorReminder`(Java) interface that was returned from hello `RegisterReminder`(C#) or `registerReminder`(Java) method call.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f51e-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f51e-143">Next Steps</span></span>
<span data-ttu-id="1f51e-144">Mer information om tillförlitlig aktören händelser och återinträde:</span><span class="sxs-lookup"><span data-stu-id="1f51e-144">Learn about Reliable Actor events and reentrancy:</span></span>
* [<span data-ttu-id="1f51e-145">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="1f51e-145">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="1f51e-146">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="1f51e-146">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
