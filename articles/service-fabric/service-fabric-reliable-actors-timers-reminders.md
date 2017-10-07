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
# <a name="actor-timers-and-reminders"></a>Aktören timers och påminnelser
Aktörer kan schemalägga regelbunden arbete på själva genom att registrera timers eller påminnelser. Den här artikeln visar hur toouse timers och påminnelser och förklarar hello skillnaderna mellan dem.

## <a name="actor-timers"></a>Aktören timers
Aktören timers ger en enkel omslutning runt en .NET eller Java timer tooensure att hello återanrop metoder respekterar hello bygger samtidighet garanterar att hello aktörer runtime tillhandahåller.

Aktörer kan använda hello `RegisterTimer`(C#) eller `registerTimer`(Java) och `UnregisterTimer`(C#) eller `unregisterTimer`(Java)-metoder på deras base klassen tooregister och avregistrera sina timers. hello exemplet nedan visar hello använder timer-API: er. hello API: er är mycket lik toohello .NET timer eller Java-timern. I det här exemplet när hello timer förfaller hello aktörer runtime ringer hello `MoveObject`(C#) eller `moveObject`(Java)-metoden. hello-metoden är garanterat toorespect hello bygger samtidighet. Detta innebär att inga andra aktören metoder eller timer/påminnelse återanrop pågår förrän den här återanrop körning.

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

hello nästa period i hello timer startar när hello motringning är slutfört. Detta innebär att timern hello stoppas medan hello återanrop körs och är igång när hello motringning är klar.

hello aktörer runtime sparar ändringar som gjorts toohello aktören Tillståndshanterare när hello motringning är klar. Om ett fel uppstår i sparar hello tillstånd, aktören objektet kommer att inaktiveras och en ny instans aktiveras.

Alla stoppas när hello aktören inaktiveras som en del av skräpinsamling. Inga timer-återanrop anropas efter. Hello aktörer runtime behåller dessutom inte någon information om hello timers som kördes före inaktivering. Är det upp toohello aktören tooregister alla timers som krävs när den aktiveras i hello framtida. Mer information finns i avsnittet hello på [aktören skräpinsamling](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Aktören påminnelser
Påminnelser är en mekanism tootrigger beständiga återanrop på en aktör vid angivna tidpunkter. Deras funktion är liknande tootimers. Men till skillnad från timers, påminnelser utlöses i alla fall tills hello aktören Avregistrerar dem explicit eller hello aktören uttryckligen tas bort. Mer specifikt utlöses påminnelser över aktören avaktiveringar och växling vid fel eftersom hello aktörer runtime kvarstår information om hello aktören påminnelser.

tooregister en påminnelse om en aktör anropar hello `RegisterReminderAsync` metoden som angetts i basklassen hello, som visas i följande exempel hello:

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

I det här exemplet `"Pay cell phone bill"` är hello påminnelse namn. Detta är en sträng som hello aktören använder toouniquely identifiera en påminnelse. `BitConverter.GetBytes(amountInDollars)`(C#) är hello kontext som är associerad med hello påminnelse. Den skickas tillbaka toohello aktören som ett argument toohello påminnelse motringning, d.v.s. `IRemindable.ReceiveReminderAsync`(C#) eller `Remindable.receiveReminderAsync`(Java).

Aktörer som använder påminnelser måste implementera hello `IRemindable` gränssnitt som visas i hello exemplet nedan.

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

När en påminnelse utlöses hello Reliable Actors runtime ska anropa hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-metoden på hello aktören. En aktören kan registrera flera påminnelser och hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-metoden anropas när någon av dessa påminnelser utlöses. hello aktören kan använda hello påminnelse namn som används i toohello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java) metoden toofigure reda på vilka påminnelse utlöstes.

hello aktörer runtime sparar hello aktören tillstånd när hello `ReceiveReminderAsync`(C#) eller `receiveReminderAsync`(Java)-anropet har slutförts. Om ett fel uppstår i sparar hello tillstånd, aktören objektet kommer att inaktiveras och en ny instans aktiveras.

toounregister en påminnelse om en aktör anropar hello `UnregisterReminderAsync`(C#) eller `unregisterReminderAsync`(Java)-metoden, som visas i hello exemplen nedan.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Enligt ovan hello `UnregisterReminderAsync`(C#) eller `unregisterReminderAsync`(Java) metoden godkänner ett `IActorReminder`(C#) eller `ActorReminder`(Java) gränssnitt. Hej aktören basklass stöder en `GetReminder`(C#) eller `getReminder`(Java)-metod som kan använda tooretrieve hello `IActorReminder`(C#) eller `ActorReminder`(Java)-gränssnittet genom att passera i hello påminnelse namn. Det är praktiskt eftersom hello aktören inte behöver toopersist hello `IActorReminder`(C#) eller `ActorReminder`(Java) gränssnitt som returnerades från hello `RegisterReminder`(C#) eller `registerReminder`(Java)-anrop.

## <a name="next-steps"></a>Nästa steg
Mer information om tillförlitlig aktören händelser och återinträde:
* [Aktören händelser](service-fabric-reliable-actors-events.md)
* [Aktören återinträde](service-fabric-reliable-actors-reentrancy.md)
